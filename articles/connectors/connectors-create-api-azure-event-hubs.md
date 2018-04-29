---
title: Bewaking met Azure Event Hubs voor Azure Logic Apps instellen | Microsoft Docs
description: Gebeurtenissen ontvangen en verzenden van gebeurtenissen met uw logische apps met behulp van Azure Event Hubs-gegevensstromen bewaken
services: logic-apps
keywords: gegevensstroom, controleprogramma voor gebeurtenissen van event hubs
author: ecfan
manager: anneta
editor: ''
documentationcenter: ''
tags: connectors
ms.assetid: ''
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 8de56cd64f38791fb27d9bcce1e16641fb162c2f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Bewaken, ontvangen en verzenden van gebeurtenissen met de Event Hubs-connector

Als u wilt een controleprogramma voor gebeurtenissen zo instellen dat uw logische app kunt detecteren van gebeurtenissen, gebeurtenissen ontvangen en verzenden van gebeurtenissen, verbinding maken met een [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) van uw logische app. Meer informatie over [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) en [prijzen werking voor Logic Apps connectors](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Vereisten

Voordat u de connector Event Hubs gebruiken kunt, moet u deze items hebben:

* Een [Azure Event Hubs-naamruimte en Event Hub](../event-hubs/event-hubs-create.md)
* Een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-event-hubs"></a>Verbinding maken met Azure Event Hubs

Om uw logische app toegang alle services tot, u moet maken een [ *verbinding* ](./connectors-overview.md) tussen uw logische app en de service, als u dat nog niet gedaan hebt. Deze verbinding toestaat uw logische app te krijgen tot gegevens. Controleer uw machtigingen voor uw logische app toegang tot uw Event Hub en de verbindingsreeks voor de Event Hubs-naamruimte ophalen.

1.  Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). 

2.  Ga naar de Event Hubs *naamruimte*, niet in een specifieke Event Hub. Op de pagina naamruimte onder **instellingen**, kies **gedeeld toegangsbeleid**. Onder **Claims**, Controleer of u hebt **beheren** machtigingen voor die naamruimte.

    ![Machtigingen voor uw Event Hub-naamruimte beheren](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Als u later handmatig invoeren de verbindingsgegevens wilt, moet u de verbindingsreeks ophalen voor de naamruimte van uw Event Hubs. Kies **RootManageSharedAccessKey**. Naast de primaire sleutel verbindingsreeks, kies de knop kopiëren. De verbindingstekenreeks opslaan voor later gebruik.

    ![Kopieer de verbindingsreeks voor Event Hubs-naamruimte](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Om te controleren of de verbindingsreeks gekoppeld aan de naamruimte van uw Event Hubs of met een specifieke event hub is, Controleer de verbindingsreeks voor de `EntityPath` parameter. Als u deze parameter vinden, de verbindingsreeks is voor een specifieke Event Hub 'entiteit' en is niet de juiste tekenreeks voor gebruik met uw logische app.

## <a name="trigger-workflow-when-your-event-hub-gets-new-events"></a>Werkstroom activeert wanneer uw Event Hub nieuwe gebeurtenissen opgehaald

Een [ *trigger* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een gebeurtenis die een werkstroom in uw logische app start. U start een werkstroom wanneer nieuwe gebeurtenissen worden verzonden naar uw Event Hub, volg deze stappen voor het toevoegen van de trigger die deze gebeurtenis wordt gedetecteerd.

1. In de [Azure-portal](https://portal.azure.com "Azure-portal"), gaat u naar uw bestaande logische app of een lege logische app maken.

2. Voer in Logic Apps Designer 'event hubs' in het zoekvak als filter. Selecteer deze trigger: **wanneer gebeurtenissen zijn beschikbaar in de Event Hub**

   ![Selecteer de trigger voor wanneer de nieuwe gebeurtenissen voor het ontvangen van uw Event Hub](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

   1. Als u nog een verbinding met de naamruimte van uw Event Hubs hebt, wordt u gevraagd om deze verbinding nu te maken. Geef een naam op voor uw verbinding en selecteer de Event Hubs-naamruimte die u wilt gebruiken.

      ![Event Hub-verbinding maken](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

      Of voer handmatig de verbindingsreeks de optie kiezen **Voer handmatig de verbindingsinformatie**. 
      Meer informatie over [het zoeken van de verbindingsreeks](#permissions-connection-string).

   2. Nu selecteren de Event Hubs-beleid wilt gebruiken en kiest u **maken**.

      ![Event Hub-verbinding, deel 2 maken](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Selecteer de Event Hub om te controleren en het interval en frequentie voor het controleren van de Event Hub ingesteld.

    ![Geef een Event Hub of klantengroep](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)
    
    > [!NOTE]
    > De Event Hub-triggers *lange polling* triggers, wat betekent dat wanneer een trigger wordt gestart, de trigger worden alle gebeurtenissen verwerkt en wordt er gewacht totdat 30 seconden voor meer gebeurtenissen worden weergegeven in de Event Hub.
    > Als er geen gebeurtenissen worden ontvangen in 30 seconden, wordt de trigger uitvoeren overgeslagen. Anders, blijft de trigger lezen van gebeurtenissen tot Event Hub leeg is.
    > De volgende trigger poll is gebaseerd op het terugkeerpatroon die is opgegeven in de trigger-eigenschappen.


4. U enkele van de geavanceerde activeringsopties eventueel selecteert **geavanceerde opties weergeven**.

    ![Geavanceerde opties voor de trigger](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

    | Eigenschap | Details |
    | --- | --- |
    | Inhoudstype  |Selecteer inhoudstype de gebeurtenissen in de vervolgkeuzelijst. Application/octet-stream is standaard geselecteerd. |
    | Inhoudsschema |Geef het schema met content in JSON voor de gebeurtenissen die in de Event Hub worden gelezen. |
    | De naam van de consumer |Voer de Event Hub [consumer groepsnaam](../event-hubs/event-hubs-features.md#consumer-groups) de gebeurtenissen moeten worden gelezen. Wanneer de groepsnaam van de consumer niet is opgegeven, wordt standaard consumergroep gebruikt. |
    | Minimale partitiesleutel |Voer de minimale [partitie](../event-hubs/event-hubs-features.md#partitions) ID te lezen. Standaard zijn alle partities worden gelezen. |
    | Maximale partitiesleutel |De maximale invoeren [partitie](../event-hubs/event-hubs-features.md#partitions) ID te lezen. Standaard zijn alle partities worden gelezen. |
    | Maximum aantal gebeurtenissen tellen |Voer een waarde voor het maximum aantal gebeurtenissen. De trigger retourneert tussen één en het aantal gebeurtenissen dat is opgegeven door deze eigenschap. |
    |||

5. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Nu wanneer uw logische app gecontroleerd van de geselecteerde Event Hub, en een nieuwe gebeurtenis, kan de trigger de acties in uw logische app voor de gebeurtenis gevonden.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Gebeurtenissen verzenden naar uw Event Hub van uw logische app

Een [*actie*](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een taak die wordt uitgevoerd tijdens de werkstroom voor de logische app. Nadat u een trigger hebt toegevoegd aan uw logische app, kunt u een actie toevoegen om bewerkingen uit te voeren op gegevens die met deze trigger worden gegenereerd. Volg deze stappen om een gebeurtenis verzenden naar uw Event Hub van uw logische app.

1. In Logic Apps Designer onder de trigger kiezen **nieuwe stap** > **een actie toevoegen**.

2. Voer in het zoekvak 'event hubs' als filter.
Deze actie selecteert: **Event Hubs - verzendgebeurtenis**

   ![Selecteer 'Event Hubs - verzendgebeurtenis'](./media/connectors-create-api-azure-event-hubs/select-event-hubs-send-event-action.png)

3. Selecteer de Event Hub voor het verzenden van de gebeurtenis. Voer vervolgens de inhoud van de gebeurtenis en andere details.

   ![Selecteer de naam van de Event Hub en leveren van inhoud van de gebeurtenis](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

4. Sla uw logische app op.

U hebt nu een actie die u gebeurtenissen van uw logische app verzendt ingesteld. 

## <a name="connector-specific-details"></a>Connector-specifieke details

Bekijk voor meer informatie over triggers en acties die zijn gedefinieerd door de Swagger-bestand en beperkingen, de [connector details](/connectors/eventhubs/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [andere connectors voor Azure Logic apps](../connectors/apis-list.md)