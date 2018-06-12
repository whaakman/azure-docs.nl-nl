---
title: Verbinding maken met Azure Event Hubs - Azure Logic Apps | Microsoft Docs
description: Beheren en controleren van gebeurtenissen met Azure Event Hubs en Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 86fc1c3542bea1be840041bb73df15631c066c7e
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294969"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>Bewaken, ontvangen en verzenden van gebeurtenissen met Azure Event Hubs en Azure Logic Apps 

Dit artikel laat zien hoe u kunt bewaken en beheren van gebeurtenissen die worden verzonden naar [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) uit in een logische app met Azure Event Hubs-connector. Op die manier kunt u logische apps voor het automatiseren van taken en werkstromen voor controleren, verzenden en ontvangen van gebeurtenissen van uw Event Hub maken. 

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Voor connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/eventhubs/" target="blank">Azure Event Hubs connector verwijzing</a>.

## <a name="prerequisites"></a>Vereisten

* Een [Azure Event Hubs-naamruimte en Event Hub](../event-hubs/event-hubs-create.md)

* De logische app waar u toegang tot uw Event Hub. U kunt uw logische app starten met een trigger Azure Event Hubs, moet u een [lege logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>Controleer machtigingen en de verbindingsreeks ophalen

Controleer uw machtigingen voor uw logische app toegang tot uw Event Hub en de verbindingsreeks voor de Event Hubs-naamruimte ophalen.

1. Meld u aan bij <a href="https://portal.azure.com" target="_blank">Azure Portal</a>. 

2. Ga naar de Event Hubs *naamruimte*, niet in een specifieke Event Hub. Op de pagina naamruimte onder **instellingen**, kies **gedeeld toegangsbeleid**. Onder **Claims**, Controleer of u hebt **beheren** machtigingen voor die naamruimte.

   ![Machtigingen voor uw Event Hub-naamruimte beheren](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Als u later handmatig invoeren de verbindingsgegevens wilt, moet u de verbindingsreeks ophalen voor de naamruimte van uw Event Hubs. 

   1. Onder **beleid**, kies **RootManageSharedAccessKey**. 

   2. Verbindingsreeks voor de primaire sleutel vinden. Kies de knop kopiëren en opslaan van de verbindingsreeks voor later gebruik.

      ![Kopieer de verbindingsreeks voor Event Hubs-naamruimte](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Om te controleren of de verbindingsreeks gekoppeld aan de naamruimte van uw Event Hubs of met een specifieke event hub is, zorg ervoor dat de verbindingsreeks beschikt niet over de `EntityPath` parameter. Als u deze parameter vinden, de verbindingsreeks voor een specifieke Event Hub 'entiteit' en niet de juiste tekenreeks voor gebruik met uw logische app.

4. Nu gaan met [toevoegen van een trigger Event Hubs](#add-trigger) of [een Event Hubs-actie toevoegen](#add-action).

<a name="add-trigger"></a>

## <a name="add-an-event-hubs-trigger"></a>Een Event Hubs-trigger toevoegen

In Azure Logic Apps elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde is voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine maakt een logische app-exemplaar en de werkstroom van uw app wordt gestart.

In dit voorbeeld ziet hoe u een logische app werkstroom kunt starten wanneer nieuwe gebeurtenissen naar uw Event Hub worden verzonden. 

1. In de Azure-portal of Visual Studio maakt een lege logische app, Logic Apps Designer te openen. In dit voorbeeld wordt de Azure-portal.

2. Voer in het zoekvak 'event hubs' als filter. Selecteer de gewenste trigger in de lijst triggers. 

   In dit voorbeeld wordt deze trigger: **Event Hubs - wanneer gebeurtenissen beschikbaar in de Event Hub zijn**

   ![Trigger selecteren](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

3. Als u wordt gevraagd om de details voor verbinding [maken van de verbinding van Event Hubs nu](#create-connection). Of als uw verbinding al aanwezig is, geeft u de benodigde gegevens voor de trigger.

   1. Van de **Event Hub-naam** , selecteert u de Event Hub die u wilt bewaken.

      ![Geef een Event Hub of klantengroep](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

   2. Selecteer de interval en frequentie voor hoe vaak de trigger om te controleren van de Event Hub.
 
   3. U enkele van de geavanceerde activeringsopties eventueel selecteert **geavanceerde opties weergeven**.
   
      ![Geavanceerde opties voor de trigger](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

      | Eigenschap | Details | 
      |----------|---------| 
      | Inhoudstype  | Selecteer het inhoudstype van de gebeurtenis. De standaardwaarde is ' application/octet-stream'. |
      | Inhoudsschema | Geef het schema met content in JSON voor de gebeurtenissen die in de Event Hub worden gelezen. |
      | De naam van de consumer | Voer de Event Hub [consumer groepsnaam](../event-hubs/event-hubs-features.md#consumer-groups) voor het lezen van gebeurtenissen. Als niet wordt opgegeven, wordt de consumergroep standaard gebruikt. |
      | Minimale partitiesleutel | Voer de minimale [partitie](../event-hubs/event-hubs-features.md#partitions) ID te lezen. Standaard zijn alle partities worden gelezen. |
      | Maximale partitiesleutel | De maximale invoeren [partitie](../event-hubs/event-hubs-features.md#partitions) ID te lezen. Standaard zijn alle partities worden gelezen. |
      | Maximum aantal gebeurtenissen tellen | Voer een waarde voor het maximum aantal gebeurtenissen. De trigger retourneert tussen één en het aantal gebeurtenissen dat is opgegeven door deze eigenschap. |
      |||

4. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**.

5. Nu doorgaan met een of meer acties toe te voegen aan uw logische app voor de taken die u wilt uitvoeren met de trigger-resultaten.

> [!NOTE]
> De Event Hub-triggers *lange polling* triggers, wat betekent dat wanneer een trigger wordt gestart, de trigger worden alle gebeurtenissen verwerkt en wordt er gewacht totdat 30 seconden meer gebeurtenissen worden weergegeven in de Event Hub.
> Als er geen gebeurtenissen worden ontvangen in 30 seconden, wordt de trigger uitvoeren overgeslagen. Anders, blijft de trigger lezen van gebeurtenissen tot uw Event Hub leeg is.
> De volgende trigger poll gebeurt op basis van het terugkeerpatroon die u in de trigger-eigenschappen opgeeft.

<a name="add-action"></a>

## <a name="add-an-event-hubs-action"></a>Een Event Hubs-actie toevoegen

In Azure Logic Apps een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in de werkstroom die na een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met een Event Hubs-trigger die op nieuwe gebeurtenissen in uw Event Hub controleert. 

1. Open uw logische app in Logic Apps Designer in de Azure-portal of Visual Studio. In dit voorbeeld wordt de Azure-portal.

2. Kies onder de trigger of actie **nieuwe stap** > **een actie toevoegen**.

   Als u een actie tussen bestaande stappen toevoegen, wilt u de muisaanwijzer over de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

3. Voer in het zoekvak 'event hubs' als filter.
Selecteer de actie die u wilt in de lijst van acties. 

   Bijvoorbeeld, selecteert u deze actie: **Event Hubs - verzendgebeurtenis**

   ![Selecteer 'Event Hubs - verzendgebeurtenis'](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

4. Als u wordt gevraagd om de details voor verbinding [maken van de verbinding van Event Hubs nu](#create-connection). Of als uw verbinding al aanwezig is, geeft u de benodigde gegevens voor de actie.

   | Eigenschap | Vereist | Beschrijving | 
   |----------|----------|-------------|
   | Event Hub-naam | Ja | Selecteer de Event Hub waar u de gebeurtenis te verzenden | 
   | Inhoud van de gebeurtenis | Nee | De inhoud voor de gebeurtenis die u wilt verzenden | 
   | Eigenschappen | Nee | De app-eigenschappen en -waarden om te verzenden | 
   |||| 

   Bijvoorbeeld: 

   ![Selecteer de naam van de Event Hub en leveren van inhoud van de gebeurtenis](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

5. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Verbinding maken met uw Event Hub

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Wanneer u wordt gevraagd om de verbindingsgegevens, bieden deze details:

   | Eigenschap | Vereist | Waarde | Beschrijving | 
   |----------|----------|-------|-------------|
   | Verbindingsnaam | Ja | <*naam van de verbinding*> | De naam voor uw verbinding maken |
   | Event Hubs-naamruimte | Ja | <*Event-hubs-naamruimte*> | Selecteer de Event Hubs-naamruimte die u wilt gebruiken. | 
   |||||  

   Bijvoorbeeld:

   ![Event Hub-verbinding maken](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Voer handmatig de verbindingsreeks de optie kiezen **Voer handmatig de verbindingsinformatie**. 
   Meer informatie over [het zoeken van de verbindingsreeks](#permissions-connection-string).

2. Selecteer het Event Hubs-beleid moet worden gebruikt, als niet is geselecteerd. Kies **Maken**.

   ![Event Hub-verbinding, deel 2 maken](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Nadat u uw verbinding maakt, doorgaan met [toevoegen Event Hubs activeren](#add-trigger) of [toevoegen Event Hubs actie](#add-action).

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische details, zoals triggers, acties en limieten, zoals is beschreven door de connector Swagger-bestand, de [van connector-verwijzingspagina](/connectors/eventhubs/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)