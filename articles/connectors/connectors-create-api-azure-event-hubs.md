---
title: Verbinding maken met Azure Eventhubs - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: a91daf08a56470e4d1e112e37b51150c2c5f00ef
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230315"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>Bewaken, ontvangen en verzenden van gebeurtenissen met Azure Event Hubs en Azure Logic Apps 

Dit artikel wordt beschreven hoe u kunt controleren en beheren van gebeurtenissen die worden verzonden naar [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) uit in een logische app met de Azure Event Hubs-connector. Op die manier kunt u logische apps die automatiseren van taken en werkstromen voor het controleren, verzenden en ontvangen van gebeurtenissen van uw Event Hub maken. 

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/eventhubs/" target="blank">documentatie voor Azure Event Hubs-connector</a>.

## <a name="prerequisites"></a>Vereisten

* Een [Azure Event Hubs-naamruimte en Event Hub](../event-hubs/event-hubs-create.md)

* De logische app waar u toegang tot uw Event Hub. Als uw logische app met een Azure Event Hubs-trigger wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>Controleer de machtigingen en -verbindingsreeks ophalen

Controleer uw machtigingen voor uw logische app voor toegang tot uw Event Hub, en de verbindingsreeks voor uw Event Hubs-naamruimte ophalen.

1. Meld u aan bij <a href="https://portal.azure.com" target="_blank">Azure Portal</a>. 

2. Ga naar uw Event Hubs *naamruimte*, niet in een specifieke Event Hub. Op de pagina naamruimte onder **instellingen**, kiest u **beleid voor gedeelde toegang**. Onder **Claims**, Controleer of u hebt **beheren** machtigingen voor die naamruimte.

   ![Machtigingen beheren voor uw Event Hub-naamruimte](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Als u de verbindingsgegevens van uw later handmatig invoeren wilt, moet u de verbindingsreeks ophalen voor uw Event Hubs-naamruimte. 

   1. Onder **beleid**, kiest u **RootManageSharedAccessKey**. 

   2. De verbindingsreeks van uw primaire sleutel vinden. Kies de knop kopiëren en opslaan van de verbindingsreeks voor later gebruik.

      ![Event Hubs-naamruimte-verbindingsreeks kopiëren](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Om te controleren of de verbindingsreeks gekoppeld aan uw Event Hubs-naamruimte of met een specifieke event hub is, zorg ervoor dat de verbindingsreeks beschikt niet over de `EntityPath`  parameter. Als u deze parameter kunt vinden, wordt de verbindingsreeks is voor een specifieke Event Hub 'entiteit' en is niet de juiste tekenreeks die moet worden gebruikt met uw logische app.

4. Nu doorgaan met [een Event Hubs-trigger toevoegen](#add-trigger) of [een Event Hubs-actie toevoegen](#add-action).

<a name="add-trigger"></a>

## <a name="add-an-event-hubs-trigger"></a>Een Event Hubs-trigger toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

In dit voorbeeld laat zien hoe u een werkstroom voor logische Apps kunt starten wanneer nieuwe gebeurtenissen worden verzonden naar uw Event Hub. 

1. In de Azure portal of Visual Studio, maak een lege logische app, die wordt geopend de ontwerper van logische Apps. Dit voorbeeld wordt de Azure-portal.

2. Typ 'eventhubs' als filter in het zoekvak. Selecteer de gewenste trigger in de lijst met triggers. 

   In dit voorbeeld maakt gebruik van deze trigger: 
   **Event Hubs - wanneer er gebeurtenissen zijn beschikbaar in de Event Hub**

   ![Trigger selecteren](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

3. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de verbinding van Event Hubs nu](#create-connection). Of, als de verbinding al bestaat, geeft u de benodigde gegevens voor de trigger.

   1. Uit de **Event Hub-naam** , selecteert u de Event Hub die u wilt bewaken.

      ![Event Hub of consumergroep opgeven](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

   2. Selecteer het interval en frequentie voor hoe vaak u wilt dat de trigger om te controleren of de Event Hub.
 
   3. (Optioneel) Schakel enkele van de geavanceerde trigger-opties, kies **geavanceerde opties weergeven**.
   
      ![Geavanceerde opties activeren](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

      | Eigenschap | Details | 
      |----------|---------| 
      | Inhoudstype  | Selecteer het inhoudstype van de gebeurtenis. De standaardwaarde is ' application/octet-stream'. |
      | Inhoudsschema | Geef het schema content in JSON voor de gebeurtenissen die worden gelezen uit de Event Hub. |
      | Naam van consumentengroep | Voer de Event Hub [consument groepsnaam](../event-hubs/event-hubs-features.md#consumer-groups) voor het lezen van gebeurtenissen. Indien niet opgegeven, wordt de standaard-consumergroep wordt gebruikt. |
      | Minimale partitiesleutel | Voer de minimale [partitie](../event-hubs/event-hubs-features.md#partitions) ID te lezen. Standaard worden alle partities worden gelezen. |
      | Maximale partitiesleutel | Voer de maximaal [partitie](../event-hubs/event-hubs-features.md#partitions) ID te lezen. Standaard worden alle partities worden gelezen. |
      | Maximum aantal gebeurtenissen tellen | Voer een waarde voor het maximum aantal gebeurtenissen. De trigger retourneert tussen één en het aantal gebeurtenissen dat is opgegeven door deze eigenschap. |
      |||

4. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

5. Nu doorgaan met een of meer acties toe te voegen aan uw logische app voor de taken die u wilt uitvoeren met de resultaten van de trigger.

> [!NOTE]
> Alle Event Hub-triggers zijn *long-polling* triggers, wat betekent dat wanneer een trigger wordt geactiveerd, de trigger worden alle gebeurtenissen verwerkt en wacht dan tot 30 seconden voor meer gebeurtenissen worden weergegeven in uw Event Hub.
> Als er geen gebeurtenissen worden ontvangen binnen 30 seconden, wordt de trigger uitvoeren wordt overgeslagen. Anders wordt de trigger blijft lezen van gebeurtenissen totdat uw Event Hub leeg is.
> De volgende trigger poll gebeurt op basis van het terugkeerpatroon die u in de eigenschappen van de trigger opgeeft.

<a name="add-action"></a>

## <a name="add-an-event-hubs-action"></a>Een Event Hubs-actie toevoegen

In Azure Logic Apps, een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in uw werkstroom die volgt op een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met een Event Hubs-trigger waarmee wordt gecontroleerd op nieuwe gebeurtenissen in uw Event Hub. 

1. Open uw logische app in de ontwerper van logische Apps in de Azure portal of Visual Studio. Dit voorbeeld wordt de Azure-portal.

2. Kies onder de trigger of actie **nieuwe stap** > **een actie toevoegen**.

   Als u wilt toevoegen een actie tussen bestaande stappen, Beweeg de muis boven de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

3. Typ 'eventhubs' als filter in het zoekvak.
Selecteer de actie die u wilt in de lijst met acties. 

   In dit voorbeeld selecteert u deze actie: **Event Hubs - verzendgebeurtenis**

   ![Selecteer 'Eventhubs - verzendgebeurtenis'](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

4. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de verbinding van Event Hubs nu](#create-connection). Of, als de verbinding al bestaat, geeft u de benodigde gegevens voor de actie.

   | Eigenschap | Vereist | Beschrijving | 
   |----------|----------|-------------|
   | Event Hub-naam | Ja | Selecteer de Event Hub waar u om de gebeurtenis te verzenden | 
   | Inhoud van de gebeurtenis | Nee | De inhoud voor de gebeurtenis die u wilt verzenden | 
   | Eigenschappen | Nee | De eigenschappen van de app en de waarden voor het verzenden van | 
   |||| 

   Bijvoorbeeld: 

   ![Selecteer de naam Event Hub en gebeurtenis inhoud leveren](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

5. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Verbinding maken met uw Event Hub

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Wanneer u wordt gevraagd om de verbindingsgegevens, geef de volgende gegevens:

   | Eigenschap | Vereist | Waarde | Beschrijving | 
   |----------|----------|-------|-------------|
   | Verbindingsnaam | Ja | <*naam van de verbinding*> | De naam voor uw verbinding maken |
   | Event Hubs-naamruimte | Ja | <*Event-hubs-naamruimte*> | Selecteer de Event Hubs-naamruimte die u wilt gebruiken. | 
   |||||  

   Bijvoorbeeld:

   ![Event Hub-verbinding maken](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Voer handmatig de connection string, de optie kiezen **handmatig verbindingsgegevens invoeren**. 
   Informatie over [over het vinden van de verbindingsreeks](#permissions-connection-string).

2. Selecteer het Event Hubs-beleid moet worden gebruikt, als u nog niet is geselecteerd. Kies **Maken**.

   ![Event Hub-verbinding, deel 2 maken](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Nadat u uw verbinding maakt, doorgaan met [toevoegen Event Hubs activeren](#add-trigger) of [toevoegen Event Hubs-actie](#add-action).

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische details, zoals triggers en acties limieten, zoals is beschreven in de Swagger-bestand van de connector, de [van de connector-verwijzingspagina](/connectors/eventhubs/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)