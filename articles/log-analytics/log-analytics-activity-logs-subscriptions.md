---
title: Azure activiteitenlogboeken in logboekanalyse verzamelen over abonnementen | Microsoft Docs
description: Event Hubs en Logic Apps gebruiken voor het verzamelen van gegevens van de Azure Activity Log en verzenden naar een Azure-logboekanalyse-werkruimte in een andere tenant.
services: log-analytics, logic-apps, event-hubs
documentationcenter: 
author: richrundmsft
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: richrund; bwren
ms.openlocfilehash: ded0b4cdcbac747d52435023a24b5719f3c58758
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="collect-azure-activity-logs-into-log-analytics-across-subscriptions"></a>Azure activiteitenlogboeken in logboekanalyse verzamelen over abonnementen

In dit artikel wordt stapsgewijs via een methode voor het verzamelen van Logboeken voor Azure-activiteit in een werkruimte voor logboekanalyse met de Azure Log Analytics-gegevensverzamelaar-connector voor Logic Apps. Het proces in dit artikel gebruiken als u nodig hebt om Logboeken te verzenden naar een werkruimte in een andere Azure Active Directory. Bijvoorbeeld, als u een beheerde serviceprovider, kan u activiteitenlogboeken verzamelen van een klant abonnement en op te slaan in een werkruimte voor logboekanalyse in uw eigen abonnement.

Als de werkruimte voor logboekanalyse in de dezelfde Azure-abonnement, of in een ander abonnement, maar in dezelfde Azure Active Directory is, gebruikt u de stappen in de [Azure activiteit logboek oplossing](../log-analytics/log-analytics-activity.md) voor het verzamelen van activiteitenlogboeken van de Azure-.

## <a name="overview"></a>Overzicht

De strategie die in dit scenario gebruikt is om Azure Activity Log verzenden van gebeurtenissen naar een [Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) waar een [logische App](../logic-apps/logic-apps-overview.md) zendt deze naar de werkruimte voor logboekanalyse. 

![afbeelding van gegevensstroom van de activiteit logboek-log analytics](media/log-analytics-activity-logs-subscriptions/data-flow-overview.png)

Voordelen van deze methode zijn:
- Lage latentie sinds de Azure Activity Log gestreamd in de Event Hub.  De logische App wordt vervolgens geactiveerd en de gegevens verzendt met logboekanalyse. 
- Minimale code is vereist en er is geen serverinfrastructuur te implementeren.

In dit artikel begeleidt u door het:
1. Maak een Event Hub. 
2. Activiteitenlogboeken exporteren naar een Event Hub met Azure Activity Log exportprofiel.
3. Maak een logische App om te lezen van de Event Hub en verzenden van gebeurtenissen met logboekanalyse.

## <a name="requirements"></a>Vereisten
Hier volgen de vereisten voor de Azure-resources in dit scenario gebruikt.

- De Event Hub-naamruimte heeft geen zich in hetzelfde abonnement als het abonnement dat zich aanmeldt. De gebruiker die u configureert u de instelling moet de juiste toegangsmachtigingen voor beide abonnementen hebben. Als u meerdere abonnementen in dezelfde Azure Active directory hebt, kunt u de activiteitenlogboeken voor alle abonnementen verzenden naar een enkele event hub.
- De logische App kunnen zich in een ander abonnement van de event hub en hoeft niet te worden in dezelfde Azure Active Directory. De logische App leest van de Event Hub met behulp van de Event Hub en gedeelde toegangssleutel.
- De werkruimte voor logboekanalyse kan zich in een ander abonnement en Azure Active Directory van de logische App, maar voor eenvoud raden we aan dat ze zich in hetzelfde abonnement. De logische App verzendt met logboekanalyse met logboekanalyse werkruimte-ID en sleutel.



## <a name="step-1---create-an-event-hub"></a>Stap 1: een Event Hub maken

<!-- Follow the steps in [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) to create your event hub. -->

1. Selecteer in de Azure-portal **nieuw**> **Internet der dingen** > **Event Hubs**.

   ![Marketplace nieuwe event hub](media/log-analytics-activity-logs-subscriptions/marketplace-new-event-hub.png)

3. Onder **naamruimte maken**, ofwel voert u een nieuwe naamruimte of een bestaande set selecteren. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.

   ![afbeelding van een event hub dialoogvenster maken](media/log-analytics-activity-logs-subscriptions/create-event-hub1.png)

4. Kies de prijscategorie (basis of standaard), een Azure-abonnement, resourcegroep en locatie voor de nieuwe resource.  Klik op **Maken** om de naamruimte te maken. U moet wacht een paar minuten voordat het systeem om de resources volledig inrichten.
6. Klik op de naamruimte die u zojuist hebt gemaakt in de lijst.
7. Selecteer **gedeeld toegangsbeleid**, en klik vervolgens op **RootManageSharedAccessKey**.

   ![afbeelding van de event hub die wordt gedeeld toegangsbeleid](media/log-analytics-activity-logs-subscriptions/create-event-hub7.png)
   
8. Klik op de knop voor kopiëren om de verbindingsreeks **RootManageSharedAccessKey** naar het Klembord te kopiëren. 

   ![afbeelding van de event hub en gedeelde toegangssleutel](media/log-analytics-activity-logs-subscriptions/create-event-hub8.png)

9. In een tijdelijke locatie, zoals Kladblok, moet u een kopie bewaren de naam van de Event Hub en ofwel de primaire of secundaire Event Hub verbindingsreeks. De logische App vereist deze waarden.  Voor de verbindingsreeks van de Event Hub, kunt u de **RootManageSharedAccessKey** connection string of een afzonderlijke maken.  De verbindingsreeks die u moet beginnen met `Endpoint=sb://` en worden voor een beleid met de **beheren** toegangsbeleid.


## <a name="step-2---export-activity-logs-to-event-hub"></a>Stap 2: de activiteitenlogboeken uitvoer naar Event Hub

Om streaming van het activiteitenlogboek, kiest u een Event Hub-Namespace en een gedeeld toegangsbeleid voor die naamruimte. Een Event Hub wordt gemaakt in die naamruimte als de eerste nieuwe activiteitenlogboek gebeurtenis plaatsvindt. 

U kunt een event hub-naamruimte die zich niet in hetzelfde abonnement als het abonnement dat zich aanmeldt, maar de abonnementen moeten in dezelfde Azure Active Directory. De gebruiker die de instelling configureert, moet de juiste RBAC voor toegang tot beide abonnementen hebben. 

1. Selecteer in de Azure-portal **Monitor** > **activiteitenlogboek**.
3. Klik op de **exporteren** knop aan de bovenkant van de pagina.

   ![afbeelding van azure monitor in de navigatie](media/log-analytics-activity-logs-subscriptions/activity-log-blade.png)

4. Selecteer de **abonnement** exporteren uit en klik vervolgens op **Alles selecteren** in de **regio's** vervolgkeuzelijst gebeurtenissen voor resources in alle regio's selecteren. Klik op de **exporteren naar een event hub** selectievakje.
7. Klik op **Service bus-naamruimte**, en selecteer vervolgens de **abonnement** met de event hub, de **event hub naamruimte**, en een **event hub beleidsnaam**.

    ![afbeelding van uitvoer naar de pagina met event hub](media/log-analytics-activity-logs-subscriptions/export-activity-log2.png)

11. Klik op **OK** en vervolgens **opslaan** deze instellingen op te slaan. De instellingen zijn onmiddellijk worden toegepast op uw abonnement.

<!-- Follow the steps in [stream the Azure Activity Log to Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) to configure a log profile that writes activity logs to an event hub. -->

## <a name="step-3---create-logic-app"></a>Stap 3: logische App maken

Als de activiteitenlogboeken naar de event hub schrijft, maakt u een logische App voor het verzamelen van de logboeken van de event hub en geschreven met logboekanalyse.

De logische App omvat het volgende:
- Een [Event Hub connector](https://docs.microsoft.com/connectors/eventhubs/) trigger te lezen van de Event Hub.
- Een [parseren JSON actie](../logic-apps/logic-apps-content-type.md) uitpakken van de JSON-gebeurtenissen.
- Een [opstellen actie](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) de JSON converteren naar een object.
- Een [logboekanalyse verzenden gegevensconnector](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) waarnaar de gegevens met logboekanalyse.

   ![afbeelding van de event hub trigger toevoegen in logic apps](media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="logic-app-requirements"></a>Vereisten voor Logic Apps
Voordat u uw logische App maakt, moet dat u de volgende informatie uit de vorige stappen hebt:
- Event Hub-naam
- Event Hub verbindingsreeks (de primaire of secundaire) voor de Event Hub-naamruimte.
- Log Analytics-werkruimte-ID
- Log Analytics gedeelde sleutel

Als u de event Hub en de verbindingsreeks, volg de stappen in [controleren Event Hubs naamruimte machtigingen en zoek de verbindingsreeks](../connectors/connectors-create-api-azure-event-hubs.md#check-event-hubs-namespace-permissions-and-find-the-connection-string).


### <a name="create-a-new-blank-logic-app"></a>Een nieuwe lege logische App maken

1. Kies in de Azure-portal **maken van een resource** > **Enterprise Integration** > **logische App**.

    ![De nieuwe logische app Marketplace](media/log-analytics-activity-logs-subscriptions/marketplace-new-logic-app.png)

2. Geef de instellingen in de onderstaande tabel.

    ![Logische app maken](media/log-analytics-activity-logs-subscriptions/create-logic-app.png)

   |Instelling | Beschrijving  |
   |:---|:---|
   | Naam           | Unieke naam voor de logische app. |
   | Abonnement   | Selecteer het Azure-abonnement dat de logische app bevat. |
   | Resourcegroep | Selecteer een bestaande Azure-resourcegroep of maak een nieuwe voor de logische app. |
   | Locatie       | Selecteer de regio van het datacenter voor het implementeren van uw logische app. |
   | Log Analytics  | Selecteer als u wilt dat de status van elke uitvoering van uw logische app aanmelden met logboekanalyse.  |

    
3. Selecteer **Maken**. Wanneer **implementatie is voltooid** melding weergegeven, klikt u op **gaat u naar de resource** uw logische App openen.

4. Kies onder **Sjablonen** de optie **Lege logische app**. 

De ontwerpfunctie van Logic Apps ziet u nu beschikbare connectors en hun triggers, die u gebruikt voor het starten van uw logische app-werkstroom.

<!-- Learn [how to create a logic app](../logic-apps/quickstart-create-first-logic-app-workflow.md). -->

### <a name="add-event-hub-trigger"></a>Event Hub-trigger toevoegen

1. Typ in het zoekvak voor Logic App Designer *event hubs* voor uw filter. Selecteer de trigger **Event Hubs - wanneer gebeurtenissen beschikbaar in de Event Hub zijn**.

   ![afbeelding van de event hub trigger toevoegen in logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-trigger.png)

2. Wanneer u wordt gevraagd om referenties, verbinding met uw naamruimte Event Hubs. Voer een naam voor uw verbinding en vervolgens de verbindingsreeks die u hebt gekopieerd.  Selecteer **Maken**.

   ![afbeelding van de event hubverbinding toe te voegen in logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-connection.png)

3. Nadat u de verbinding hebt gemaakt, kunt u de instellingen voor de trigger bewerken. Begin met het selecteren **insights operationele logboeken** van de **Event Hub-naam** vervolgkeuzelijst.

   ![Wanneer gebeurtenissen dialoogvenster beschikbaar zijn](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-read-events.png)

5. Vouw **geavanceerde opties weergeven** en wijzig **inhoudstype** naar *application/json*

   ![Dialoogvenster Event hub-configuratie](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-configuration.png)

### <a name="add-parse-json-action"></a>Parseren van JSON-actie toevoegen

De uitvoer van de Event Hub bevat een JSON-nettolading met een matrix van records. De [parseren JSON](../logic-apps/logic-apps-content-type.md) actie wordt gebruikt om op te halen, alleen de matrix van de records voor het verzenden van logboekanalyse.

1. Klik op **nieuwe stap** > **een actie toevoegen**
2. Typ in het zoekvak *parseren json* voor uw filter. Selecteer de actie **gegevensbewerkingen - JSON parseren**.

   ![Parse json actie toe te voegen in logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-add-parse-json-action.png)

3. Klik in de **inhoud** veld en selecteer vervolgens *hoofdtekst*.

4. Kopieer en plak het volgende schema in de **Schema** veld.  Dit schema komt overeen met de uitvoer van de Event Hub-actie.  

   ![Dialoogvenster json parseren](media/log-analytics-activity-logs-subscriptions/logic-apps-parse-json-configuration.png)

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
> U kunt een voorbeeld-nettolading ophalen door te klikken op **uitvoeren** en kijken naar de **onbewerkte uitvoer** van de Event Hub.  U kunt deze uitvoer met **gebruik voorbeeld nettolading voor het genereren van schema** in de **parseren JSON** activiteit voor het genereren van het schema.

### <a name="add-compose-action"></a>Opstellen actie toevoegen
De [opstellen](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) actie wordt de JSON-uitvoer en maakt een object dat kan worden gebruikt door de actie logboekanalyse.

1. Klik op **nieuwe stap** > **een actie toevoegen**
2. Type *opstellen* voor uw filter en selecteer vervolgens de actie **gegevensbewerkingen - opstellen**.

    ![Opstellen actie toevoegen](media/log-analytics-activity-logs-subscriptions/logic-apps-add-compose-action.png)

3. Klik op de **invoer** veld en selecteert u **hoofdtekst** onder de **parseren JSON** activiteit.


### <a name="add-log-analytics-send-data-action"></a>Log Analytics-verzenden gegevens actie toevoegen
De [Azure Log Analytics-gegevensverzamelaar](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) actie duurt het object van de actie opstellen en verzendt het naar logboekanalyse.

1. Klik op **nieuwe stap** > **een actie toevoegen**
2. Type *Meld analytics* voor uw filter en selecteer vervolgens de actie **Azure Log Analytics gegevensverzamelaar - gegevens verzenden**.

   ![Toe te voegen logboekanalyse verzenden Gegevensactie in logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-connector.png)

3. Voer een naam voor uw verbinding en plak in het **werkruimte-ID** en **Werkruimtesleutel** voor uw werkruimte voor logboekanalyse.  Klik op **Create**.

   ![Log analytics verbinding toe te voegen in logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-log-analytics-add-connection.png)

4. Nadat u de verbinding hebt gemaakt, kunt u de instellingen in de onderstaande tabel bewerken. 

    ![Configureren van de actie gegevens verzenden](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-configuration.png)

   |Instelling        | Waarde           | Beschrijving  |
   |---------------|---------------------------|--------------|
   |JSON-aanvraagtekst  | **Uitvoer** van de **opstellen** actie | Haalt de records van de hoofdtekst van de actie opstellen. |
   | Aangepaste logboeknaam | AzureActivity | Naam van de aangepaste logboektabel in logboekanalyse voor het opslaan van de geïmporteerde gegevens maken. |
   | Time-gegenereerd-veld | tijd | Selecteer niet als u het veld JSON voor **tijd** -typt u de tijd word. Als u de JSON-veld selecteert de ontwerpfunctie plaatst de **gegevens verzenden** actie naar een *voor elk* lus, die niet wenst. |




10. Klik op **opslaan** de wijzigingen die u hebt aangebracht aan uw logische App wilt opslaan.

## <a name="step-4---test-and-troubleshoot-the-logic-app"></a>Stap 4 - Test en de logische App oplossen
Met de werkstroom is voltooid, kunt u testen in de ontwerpfunctie om te controleren of deze zonder fouten werkt.

Klik in de ontwerpfunctie voor Logic App **uitvoeren** voor het testen van de logische App. Elke stap in de logische App toont een statuspictogram met een wit vinkje in een groene cirkel die slagen aangeeft.

   ![Test logische app](media/log-analytics-activity-logs-subscriptions/test-logic-app.png)

Als u gedetailleerde informatie voor elke stap, klik op de naam van de stap uit te vouwen. Klik op **onbewerkte invoer weergeven** en **onbewerkte uitvoer weergeven** voor meer informatie over de gegevens ontvangen en verzonden bij elke stap.

## <a name="step-5---view-azure-activity-log-in-log-analytics"></a>Stap 5 - weergave Azure Activity Log in Log Analytics
De laatste stap is om te controleren van de werkruimte voor logboekanalyse om ervoor te zorgen dat gegevens worden verzameld zoals verwacht.

1. Selecteer in de Azure-portal **logboekanalyse**.
2. Uw werkruimte selecteren en vervolgens de **logboek zoeken** tegel.
3. Typ in de zoekbalk query `AzureActivity_CL` en klik op de zoekknop. Als u uw aangepaste logboek niet naam *AzureActivity*, typ de naam die u hebt gekozen en toevoeg- `_CL`.

>[!NOTE]
> De eerste keer die een nieuw aangepast logboek wordt verzonden met logboekanalyse dit kan een uur duren voor het aangepaste logboek doorzoekbaar.

>[!NOTE]
> De activiteitenlogboeken van de worden geschreven naar een aangepaste tabel en niet weergeven in de [activiteitenlogboek oplossing](./log-analytics-activity.md).


![Test logische app](media/log-analytics-activity-logs-subscriptions/log-analytics-results.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een logische app Azure activiteitenlogboeken lezen van een Event Hub en ze verzenden naar Log Analytics voor analyse gemaakt. Bekijk de zelfstudie voor het visualiseren gegevens voor meer informatie over de gegevens in Log Analytics, zoals het maken van dashboards, te visualiseren.

> [!div class="nextstepaction"]
> [Logboek zoeken gegevens zelfstudie visualiseren](./log-analytics-tutorial-dashboards.md)
