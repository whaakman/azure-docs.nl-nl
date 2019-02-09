---
title: Azure-activiteitenlogboeken verzamelen in Log Analytics voor meerdere abonnementen | Microsoft Docs
description: Event Hubs en Logic Apps gebruiken om gegevens uit het Azure-activiteitenlogboek te verzamelen en te verzenden naar een Azure Log Analytics-werkruimte in een andere tenant.
services: log-analytics, logic-apps, event-hubs
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: magoedte
ms.openlocfilehash: 4d336f657baef99d97568ba722e64a1f4440367f
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984576"
---
# <a name="collect-azure-activity-logs-into-log-analytics-across-subscriptions-in-different-azure-active-directory-tenants"></a>Azure-activiteitenlogboeken verzamelen in Log Analytics voor meerdere abonnementen in verschillende Azure Active Directory-tenants

In dit artikel wordt een stapsgewijze methode besproken voor het verzamelen van Azure-activiteitenlogboeken in een Log Analytics-werkruimte met behulp van de connector van de Azure Log Analytics-gegevensverzamelaar voor Logic Apps. Gebruik het proces in dit artikel als u nodig hebt om Logboeken te verzenden naar een werkruimte in een andere Azure Active Directory-tenant. Als u bijvoorbeeld een aanbieder van beheerde services bent, wilt u mogelijk activiteitenlogboeken verzamelen uit een abonnement van een klant en opslaan in een Log Analytics-werkruimte in uw eigen abonnement.

Als de Log Analytics-werkruimte zich in hetzelfde Azure-abonnement, of in een ander abonnement maar in dezelfde Azure Active Directory bevindt, gebruikt u de stappen in de [Oplossing voor Azure-activiteitenlogboeken](collect-activity-logs.md) voor het verzamelen van Azure-activiteitenlogboeken.

## <a name="overview"></a>Overzicht

De strategie die in dit scenario wordt gebruikt, is het laten verzenden van gebeurtenissen door Azure Activity Log naar een [Event Hub](../../event-hubs/event-hubs-about.md), waar een [logische app](../../logic-apps/logic-apps-overview.md) deze naar uw Log Analytics-werkruimte verzendt. 

![afbeelding van gegevensstroom van een activiteitenlogboek naar Log Analytics](media/collect-activity-logs-subscriptions/data-flow-overview.png)

Voordelen van deze methode zijn:
- Lage latentie omdat het Azure-activiteitenlogboek wordt gestreamd naar Event Hub.  De logische app wordt vervolgens geactiveerd en deze verzendt de gegevens naar Log Analytics. 
- Er is minimale code vereist en er hoeft geen serverinfrastructuur te worden geïmplementeerd.

In dit artikel doorloopt u de volgende stappen:
1. Een Event Hub maken. 
2. Activiteitenlogboeken exporteren naar een Event Hub met het exportprofiel van het Azure-activiteitenlogboek.
3. Een logische app maken om te lezen uit de Event Hub en gebeurtenissen te verzenden met Log Analytics.

## <a name="requirements"></a>Vereisten
Hieronder volgen de vereisten voor de Azure-resources die in dit scenario worden gebruikt.

- De Event Hub-naamruimte hoeft zich niet in hetzelfde abonnement te bevinden als het abonnement dat logboeken verzendt. De gebruiker die de instelling configureert, moet de juiste toegangsrechten voor beide abonnementen hebben. Als u meerdere abonnementen in dezelfde Azure Active Directory hebt, kunt u de activiteitenlogboeken voor alle abonnementen verzenden naar een enkele event hub.
- De logische app kan zich in een ander abonnement bevinden dan de event hub en hoeft niet in hetzelfde Azure Active Directory te zijn. De logische app leest uit de Event Hub met behulp van de gedeelde toegangssleutel van de Event Hub.
- De Log Analytics-werkruimte kan zich in een ander abonnement en Azure Active Directory bevinden dan de logische app, maar voor het gemak raden we aan dat deze zich in hetzelfde abonnement bevinden. De logische app verzendt gegevens naar Log Analytics met behulp van de Log Analytics-werkruimte-id en -sleutel.



## <a name="step-1---create-an-event-hub"></a>Stap 1: een Event Hub maken

<!-- Follow the steps in [how to create an Event Hubs namespace and Event Hub](../../event-hubs/event-hubs-create.md) to create your event hub. -->

1. Selecteer in Azure Portal **Een resource maken** > **Internet of Things** > **Event Hubs**.

   ![Nieuwe event hub Marketplace](media/collect-activity-logs-subscriptions/marketplace-new-event-hub.png)

3. Voer onder **Naamruimte maken** een nieuwe naamruimte in of selecteer een bestaande. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.

   ![afbeelding van dialoogvenster event hub maken](media/collect-activity-logs-subscriptions/create-event-hub1.png)

4. Kies de prijscategorie (Basic of Standard), een Azure-abonnement, resourcegroep en locatie voor de nieuwe resource.  Klik op **Maken** om de naamruimte te maken. U moet een paar minuten wachten voordat het systeem de resources volledig heeft ingericht.
6. Klik op de naamruimte die u zojuist hebt gemaakt in de lijst.
7. Selecteer **Beleid voor gedeelde toegang** en klik op **RootManageSharedAccessKey**.

   ![afbeelding van gedeeld toegangsbeleid van event hub](media/collect-activity-logs-subscriptions/create-event-hub7.png)
   
8. Klik op de knop voor kopiëren om de verbindingsreeks **RootManageSharedAccessKey** naar het Klembord te kopiëren. 

   ![afbeelding van gedeelde toegangssleutel van event hub](media/collect-activity-logs-subscriptions/create-event-hub8.png)

9. Bewaar op een tijdelijke locatie, zoals Kladblok, een kopie van de naam van de Event Hub en de primaire of secundaire Event Hub-verbindingsreeks. Deze waarden zijn vereist voor de logische app.  Voor de verbindingsreeks van de Event Hub kunt u de verbindingsreeks **RootManageSharedAccessKey** gebruiken of een afzonderlijke maken.  De verbindingsreeks die u gebruikt, moet beginnen met `Endpoint=sb://` en voor een beleid zijn dat het toegangsbeleid **Beheren** bevat.


## <a name="step-2---export-activity-logs-to-event-hub"></a>Stap 2: activiteitenlogboeken streamen naar Event Hubs

Om streaming van het activiteitenlogboek in te schakelen, kiest u een Event Hub-naamruimte en een gedeeld toegangsbeleid voor die naamruimte. Er wordt een Event Hub gemaakt in die naamruimte als de eerste nieuwe activiteitenlogboekgebeurtenis plaatsvindt. 

U kunt een event hub-naamruimte gebruiken die zich niet in hetzelfde abonnement bevindt als het abonnement dat logboeken verzendt, maar de abonnementen moeten zich wel in dezelfde Azure Active Directory bevinden. De gebruiker die de instelling configureert moet de juiste RBAC voor beide abonnementen hebben. 

1. Selecteer in Azure Portal **Controle** > **Activiteitenlogboek**.
3. Klik op de knop **Exporteren** bovenaan de pagina.

   ![afbeelding van controle van azure in de navigatie](media/collect-activity-logs-subscriptions/activity-log-blade.png)

4. Selecteer het **Abonnement** waaruit u wilt exporteren uit en klik vervolgens op **Alles selecteren** in de vervolgkeuzelijst **Regio's** om gebeurtenissen voor resources in alle regio's te selecteren. Klik op het selectievakje **Exporteren naar een Event Hub**.
7. Klik op **Service Bus-naamruimte** en selecteer vervolgens het **Abonnement** met de event hub, de **event hub-naamruimte** en een **event hub-beleidsnaam**.

    ![afbeelding van exporteren naar de event hub-pagina](media/collect-activity-logs-subscriptions/export-activity-log2.png)

11. Klik op **OK** en vervolgens op **Opslaan** om deze instellingen op te slaan. De instellingen worden onmiddellijk toegepast op uw abonnement.

<!-- Follow the steps in [stream the Azure Activity Log to Event Hubs](../../azure-monitor/platform/activity-logs-stream-event-hubs.md) to configure a log profile that writes activity logs to an event hub. -->

## <a name="step-3---create-logic-app"></a>Stap 3: een logische app maken

Zodra de activiteitenlogboeken naar de event hub schrijven, maakt u een logische app voor het verzamelen van de logboeken van de event hub en schrijft u deze naar Log Analytics.

De logische app omvat het volgende:
- Een trigger voor een [Event Hub connector](https://docs.microsoft.com/connectors/eventhubs/) om te lezen vanuit de Event Hub.
- Een [actie JSON parseren](../../logic-apps/logic-apps-content-type.md) om de JSON-gebeurtenissen te extraheren.
- Een [actie Opstellen](../../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) om de JSON te converteren naar een object.
- Een [Log Analytics-connector voor het verzenden van gegevens](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) om de gegevens naar Log Analytics te verzenden.

   ![afbeelding van het toevoegen van een event hub-trigger in logische apps](media/collect-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="logic-app-requirements"></a>Vereisten voor logische apps
Voordat u een logische app maakt, moet u over de volgende informatie uit de vorige stappen beschikken:
- Event Hub-naam
- Event Hub-verbindingsreeks (de primaire of secundaire) voor de Event Hub-naamruimte.
- Log Analytics-werkruimte-id
- Gedeelde sleutel van Log Analytics

Voor het ophalen van de naam en verbindingsreeks van de event hub volgt u de stappen in [Naamruimte machtigingen van Event Hubs controleren en de verbindingsreeks zoeken](../../connectors/connectors-create-api-azure-event-hubs.md#permissions-connection-string).


### <a name="create-a-new-blank-logic-app"></a>Een nieuwe lege, logische app maken

1. Kies in Azure Portal **Een resource maken** > **Bedrijfsintegratie** > **Logische app**.

    ![Nieuwe logische Marketplace-app](media/collect-activity-logs-subscriptions/marketplace-new-logic-app.png)

2. Voer de instellingen in de onderstaande tabel in.

    ![Logische app maken](media/collect-activity-logs-subscriptions/create-logic-app.png)

   |Instelling | Description  |
   |:---|:---|
   | Name           | Unieke naam voor de logische app. |
   | Abonnement   | Selecteer het Azure-abonnement dat de logische app gaat bevatten. |
   | Resourcegroep | Selecteer een bestaande Azure-resourcegroep of maak een nieuwe voor de logische app. |
   | Locatie       | Selecteer de regio van het datacenter voor het implementeren van uw logische app. |
   | Log Analytics  | Selecteer deze optie als u wilt dat de status van elke uitvoering van uw logische app in Log Analytics wordt vastgelegd.  |

    
3. Selecteer **Maken**. Wanneer de melding **Implementatie is voltooid** wordt weergegeven, klikt u op **Naar de resource gaan** om uw logische app te openen.

4. Kies onder **Sjablonen** de optie **Lege logische app**. 

In de ontwerper van logische apps worden beschikbare connectors en de bijbehorende triggers getoond. U gebruikt deze voor het starten van de werkstroom voor uw logische app.

<!-- Learn [how to create a logic app](../../logic-apps/quickstart-create-first-logic-app-workflow.md). -->

### <a name="add-event-hub-trigger"></a>Event Hub-trigger toevoegen

1. Typ in het zoekvak voor de ontwerper van logische apps *event hubs* voor uw filter. Selecteer de trigger **Event Hubs - wanneer gebeurtenissen beschikbaar zijn in Event Hub**.

   ![afbeelding van het toevoegen van een event hub-trigger in logische apps](media/collect-activity-logs-subscriptions/logic-apps-event-hub-add-trigger.png)

2. Wanneer u wordt gevraagd om referenties, maakt u verbinding met uw Event Hubs-naamruimte. Voer een naam in voor uw verbinding en voer vervolgens de verbindingsreeks in die u hebt gekopieerd.  Selecteer **Maken**.

   ![afbeelding van het toevoegen van een event hub-verbinding in logische apps](media/collect-activity-logs-subscriptions/logic-apps-event-hub-add-connection.png)

3. Nadat u de verbinding hebt gemaakt, kunt u de instellingen voor de trigger bewerken. Selecteer eerst **insights-operational-logs** in de vervolgkeuzelijst **Event Hub-naam**.

   ![Het dialoogvenster Wanneer gebeurtenissen beschikbaar zijn](media/collect-activity-logs-subscriptions/logic-apps-event-hub-read-events.png)

5. Vouw **Geavanceerde opties weergeven** uit en wijzig **Inhoudstype** in *application/json*

   ![Dialoogvenster Event hub-configuratie](media/collect-activity-logs-subscriptions/logic-apps-event-hub-configuration.png)

### <a name="add-parse-json-action"></a>De actie JSON parseren toevoegen

De uitvoer van de Event Hub bevat een JSON-payload met een matrix van records. De actie [JSON parseren](../../logic-apps/logic-apps-content-type.md) wordt gebruikt om de matrix van records te extraheren voor verzending naar Log Analytics.

1. Klik op **Nieuwe stap** > **Een actie toevoegen**
2. Typ in het zoekvak *JSON parseren* voor uw filter. Selecteer de actie **Gegevensbewerkingen - JSON parseren**.

   ![De actie JSON parseren toevoegen in logische apps](media/collect-activity-logs-subscriptions/logic-apps-add-parse-json-action.png)

3. Klik in het veld **Inhoud** en selecteer vervolgens *Hoofdtekst*.

4. Kopieer en plak het volgende schema in het veld **Schema**.  Dit schema komt overeen met de uitvoer van de Event Hub-actie.  

   ![Dialoogvenster JSON parseren](media/collect-activity-logs-subscriptions/logic-apps-parse-json-configuration.png)

``` json-schema
{
    "properties": {
        "body": {
            "properties": {
                "ContentData": {
                    "type": "string"
                },
                "Properties": {
                    "properties": {
                        "ProfileName": {
                            "type": "string"
                        },
                        "x-opt-enqueued-time": {
                            "type": "string"
                        },
                        "x-opt-offset": {
                            "type": "string"
                        },
                        "x-opt-sequence-number": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                },
                "SystemProperties": {
                    "properties": {
                        "EnqueuedTimeUtc": {
                            "type": "string"
                        },
                        "Offset": {
                            "type": "string"
                        },
                        "PartitionKey": {},
                        "SequenceNumber": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                }
            },
            "type": "object"
        },
        "headers": {
            "properties": {
                "Cache-Control": {
                    "type": "string"
                },
                "Content-Length": {
                    "type": "string"
                },
                "Content-Type": {
                    "type": "string"
                },
                "Date": {
                    "type": "string"
                },
                "Expires": {
                    "type": "string"
                },
                "Location": {
                    "type": "string"
                },
                "Pragma": {
                    "type": "string"
                },
                "Retry-After": {
                    "type": "string"
                },
                "Timing-Allow-Origin": {
                    "type": "string"
                },
                "Transfer-Encoding": {
                    "type": "string"
                },
                "Vary": {
                    "type": "string"
                },
                "X-AspNet-Version": {
                    "type": "string"
                },
                "X-Powered-By": {
                    "type": "string"
                },
                "x-ms-request-id": {
                    "type": "string"
                }
            },
            "type": "object"
        }
    },
    "type": "object"
}
```

>[!TIP]
> U kunt een voorbeeld van een payload krijgen door te klikken op **Uitvoeren** en te kijken naar de **onbewerkte uitvoer** van de Event Hub.  U kunt deze uitvoer vervolgens gebruiken met **Voorbeeld van payload gebruiken om een schema te genereren** in de activiteit **JSON parseren** om het schema te genereren.

### <a name="add-compose-action"></a>De actie Opstellen toevoegen
De actie [Opstellen](../../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) neemt de JSON-uitvoer en maakt een object dat kan worden gebruikt door de actie Log Analytics.

1. Klik op **Nieuwe stap** > **Een actie toevoegen**
2. Typ *opstellen* voor uw filter en selecteer vervolgens de actie **Gegevensbewerkingen - Opstellen**.

    ![De actie Opstellen toevoegen](media/collect-activity-logs-subscriptions/logic-apps-add-compose-action.png)

3. Klik op het veld **Invoer** veld en selecteer **Hoofdtekst** onder de activiteit **JSON parseren**.


### <a name="add-log-analytics-send-data-action"></a>De actie Gegevens verzenden van Log Analytics toevoegen
De actie [Azure Log Analytics-gegevensverzamelaar](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) neemt het object van de actie Opstellen en verzendt het naar Log Analytics.

1. Klik op **Nieuwe stap** > **Een actie toevoegen**
2. Typ *log analytics* voor uw filter en selecteer vervolgens de actie **Azure Log Analytics-gegevensverzamelaar - Gegevens verzenden**.

   ![De actie Gegevens verzenden van Log Analytics toevoegen aan logische apps](media/collect-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-connector.png)

3. Voer een naam in voor uw verbinding en plak deze in de **Werkruimte-id** en **Werkruimtesleutel** voor uw Log Analytics-werkruimte.  Klik op **Create**.

   ![De actie Verbinding van Log Analytics toevoegen aan logische apps](media/collect-activity-logs-subscriptions/logic-apps-log-analytics-add-connection.png)

4. Nadat u de verbinding hebt gemaakt, kunt u de instellingen in de onderstaande tabel bewerken. 

    ![De actie Gegevens verzenden configureren](media/collect-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-configuration.png)

   |Instelling        | Waarde           | Description  |
   |---------------|---------------------------|--------------|
   |JSON-aanvraagtekst  | **Uitvoer** van de actie **Opstellen** | Haalt de records van de hoofdtekst van de actie Opstellen op. |
   | Aangepaste logboeknaam | AzureActivity | Naam van de aangepaste logboektabel die moet worden gemaakt in Log Analytics voor het opslaan van de geïmporteerde gegevens. |
   | Het veld Gegenereerde tijd | time | Selecteer het JSON-veld niet voor **time** - typ alleen het woord 'time'. Als u het JSON-veld selecteert, plaatst de ontwerpfunctie de actie **Gegevens verzenden** in een *Voor elk*-lus, en dat is niet iets dat u wilt. |




10. Klik op **Opslaan** om de wijzigingen die u hebt aangebracht in uw logische app op te slaan.

## <a name="step-4---test-and-troubleshoot-the-logic-app"></a>Stap 4: de logische app testen en problemen met de logische app oplossen
Nu de werkstroom is voltooid, kunt u de logische app testen in de ontwerper om te controleren of deze zonder fouten werkt.

Selecteer in de ontwerper van logische apps **Uitvoeren** om de logische app te testen. Bij elke stap in de logische app wordt een statuspictogram met een wit vinkje in een groene cirkel weergegeven, wat aangeeft dat de stap is geslaagd.

   ![Logische app testen](media/collect-activity-logs-subscriptions/test-logic-app.png)

Klik op de naam van de stap om de stap uit te vouwen en gedetailleerde informatie voor elke stap weer te geven. Klik op **Onbewerkte invoer weergeven** en **Onbewerkte uitvoer weergeven** voor meer informatie over de gegevens die bij elke stap zijn ontvangen en verzonden.

## <a name="step-5---view-azure-activity-log-in-log-analytics"></a>Stap 5: Azure-activiteitenlogboek weergeven in Log Analytics
De laatste stap bestaat uit het controleren van de Log Analytics-werkruimte om ervoor te zorgen dat gegevens worden verzameld zoals verwacht.

1. Klik in Azure Portal in de linkerbovenhoek op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Selecteer de werkruimte in de lijst met Log Analytics-werkruimten.
3.  Klik op de tegel **Zoeken in logboeken**. In het deelvenster Zoeken in logboeken typt u `AzureActivity_CL` in het queryveld en drukt u op Enter of klikt u op de zoekknop rechts van het queryveld. Als u uw aangepaste logboek niet *AzureActivity* hebt genoemd, typt u de naam die u hebt gekozen en voegt u `_CL` toe.

>[!NOTE]
> De eerste keer dat een nieuw aangepast logboek naar Log Analytics wordt verzonden, kan het een uur duren voordat het aangepaste logboek doorzoekbaar is.

>[!NOTE]
> De activiteitenlogboeken worden geschreven naar een aangepaste tabel en worden niet weergegeven in de [oplossing voor activiteitenlogboeken](./collect-activity-logs.md).


![Logische app testen](media/collect-activity-logs-subscriptions/log-analytics-results.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een logische app gemaakt om Azure-activiteitenlogboeken te lezen uit een Event Hub en te verzenden naar Log Analytics voor analyse. Bekijk voor meer informatie over het visualiseren van gegevens in Log Analytics, inclusief het maken van dashboards, de zelfstudie over het visualiseren gegevens.

> [!div class="nextstepaction"]
> [Zelfstudie over het visualiseren van gegevens uit Zoeken in logboeken](./../../azure-monitor/learn/tutorial-logs-dashboards.md)
