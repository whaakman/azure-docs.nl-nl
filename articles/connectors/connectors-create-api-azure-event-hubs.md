---
title: Controleprogramma voor gebeurtenissen met Azure Event Hubs instellen voor Azure Logic Apps | Microsoft Docs
description: Gebeurtenissen ontvangen en verzenden van gebeurtenissen voor Azure Logic Apps met Azure Event Hubs-gegevensstromen bewaken
services: logic-apps
keywords: gegevensstroom, controleprogramma voor gebeurtenissen van event hubs
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: a7f31c2c17d326d58ede0bb00cdc0f701069ea14
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Bewaken, ontvangen en verzenden van gebeurtenissen met de Event Hubs-connector

Als u wilt een controleprogramma voor gebeurtenissen zo instellen dat uw logische app kunt detecteren van gebeurtenissen, gebeurtenissen ontvangen en verzenden van gebeurtenissen, verbinding maken met een [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) van uw logische app. Meer informatie over [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Vereisten

* Er moeten een [Event Hubs-naamruimte en Event Hub](../event-hubs/event-hubs-create.md) in Azure. Meer informatie over [het maken van een Event Hubs-naamruimte en Event Hub](../event-hubs/event-hubs-create.md). 

* Gebruik [elke connector](https://docs.microsoft.com/azure/connectors/apis-list) in uw logische app, hebt u eerst een logische app maken. Meer informatie over [het maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-the-connection-string"></a>Controleer machtigingen voor Event Hubs-naamruimte en de verbindingsreeks vinden

Voor uw app logica voor toegang tot alle services die u hebt gemaakt een [ *verbinding* ](./connectors-overview.md) tussen uw logische app en de service, als u dat nog niet gedaan hebt. Deze verbinding toestaat uw logische app te krijgen tot gegevens.
Voor uw logische app toegang tot uw Event Hub, hebt u hebben **beheren** machtigingen en de verbindingsreeks voor de naamruimte van uw Event Hubs.

Controleer uw machtigingen en de verbindingsreeks ophalen, als volgt te werk.

1.  Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). 

2.  Ga naar de Event Hubs *naamruimte*, niet de specifieke Event Hub. Op de blade naamruimte onder **instellingen**, kies **gedeeld toegangsbeleid**. Onder **Claims**, Controleer of u hebt **beheren** machtigingen voor die naamruimte.

    ![Machtigingen voor uw Event Hub-naamruimte beheren](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  Als u wilt kopiëren van de verbindingsreeks voor de naamruimte van Event Hubs, kies **RootManageSharedAccessKey**. Naast de primaire sleutel verbindingsreeks, kies de knop kopiëren.

    ![Kopieer de verbindingsreeks voor Event Hubs-naamruimte](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Om te controleren of de verbindingsreeks gekoppeld aan de naamruimte van uw Event Hubs of met een specifieke Event Hub is, Controleer de verbindingsreeks voor de `EntityPath` parameter. Als u deze parameter vinden, de verbindingsreeks is voor een specifieke Event Hub 'entiteit' en is niet de juiste tekenreeks voor gebruik met uw logische app.

4.  Wanneer u wordt gevraagd om referenties na een trigger voor Event Hubs of actie toe te voegen aan uw logische app, kunt u kunt nu voor verbinding met uw naamruimte Event Hubs. Geef een naam op voor de verbinding, voert u de verbindingsreeks die u hebt gekopieerd en kies **maken**.

    ![Geef de verbindingsreeks voor Event Hubs-naamruimte](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Nadat u uw verbinding maakt, de naam van de verbinding moet worden weergegeven in de Event Hubs trigger of actie. 
    Vervolgens kunt u doorgaan met de overige stappen in uw logische app.

    ![Event Hubs naamruimte verbinding is gemaakt](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Werkstroom starten wanneer uw Event Hub nieuwe gebeurtenissen ontvangt

Een [ *trigger* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een gebeurtenis die een werkstroom in uw logische app start. U start een werkstroom wanneer nieuwe gebeurtenissen worden verzonden naar uw Event Hub, volg deze stappen voor het toevoegen van de trigger die deze gebeurtenis wordt gedetecteerd.

1.  In de [Azure-portal](https://portal.azure.com "Azure-portal"), gaat u naar uw bestaande logische app of een lege logische app maken.

2.  Voer in het zoekvak voor Logic App Designer `event hubs` voor uw filter. Selecteer deze trigger: **wanneer gebeurtenissen zijn beschikbaar in de Event Hub**

    ![Selecteer de trigger voor wanneer de nieuwe gebeurtenissen voor het ontvangen van uw Event Hub](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Als u nog een verbinding met de naamruimte van uw Event Hubs hebt, wordt u gevraagd om deze verbinding nu te maken. Geef een naam op voor uw verbinding en geef de verbindingsreeks voor de naamruimte van uw Event Hubs. 
    Indien nodig, meer [het zoeken van de verbindingsreeks](#permissions-connection-string).

    ![Geef de verbindingsreeks voor Event Hubs-naamruimte](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Nadat u de verbinding van de instellingen voor de **wanneer een gebeurtenis in beschikbaar zijn in een Event Hub** trigger worden weergegeven.

    ![De instellingen van de trigger voor wanneer de nieuwe gebeurtenissen voor het ontvangen van uw Event Hub](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Typ of Selecteer de naam voor de Event Hub die u wilt bewaken. Selecteer de frequentie en het interval voor hoe vaak u wilt controleren van de Event Hub.

    > [!TIP]
    > Schakel eventueel een consumergroep voor het lezen van gebeurtenissen, kies **geavanceerde opties weergeven**. 

    ![Geef een Event Hub of klantengroep](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    U hebt nu een trigger ingesteld geen werkstroom starten voor uw logische app. 
    Uw logische app controleert de opgegeven Event Hub op basis van de door u ingesteld schema. 
    Als uw app vindt de nieuwe gebeurtenissen in de Event Hub, de trigger andere acties uitgevoerd of in uw logische app getriggerd.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Gebeurtenissen verzenden naar uw Event Hub van uw logische app

Een [*actie*](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een taak die wordt uitgevoerd tijdens de werkstroom voor de logische app. Nadat u een trigger hebt toegevoegd aan uw logische app, kunt u een actie toevoegen om bewerkingen uit te voeren op gegevens die met deze trigger worden gegenereerd. Volg deze stappen om een gebeurtenis verzenden naar uw Event Hub van uw logische app.

1.  In Logic App Designer onder uw logische app worden geactiveerd, kies **nieuwe stap** > **een actie toevoegen**.

    ![Kies 'Nieuwe stap' vervolgens 'Een actie toevoegen'](./media/connectors-create-api-azure-event-hubs/add-action.png)

    U kunt nu vinden en selecteer een actie om uit te voeren. 
    Hoewel u geen actie, bijvoorbeeld, willen we de Event Hubs-actie voor het verzenden van gebeurtenissen.

2.  Voer in het zoekvak `event hubs` voor uw filter.
Deze actie selecteert: **verzendgebeurtenis**

    ![Selecteer 'Event Hubs - verzendgebeurtenis' actie](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Voer de vereiste gegevens voor de gebeurtenis, zoals de naam voor de Event Hub waar u de gebeurtenis te verzenden. Voer een optionele details over de gebeurtenis, zoals inhoud voor die gebeurtenis.

    > [!TIP]
    > Als eventueel opgeven waar de gebeurtenis de Event Hub-partitie, kiest u **geavanceerde opties weergeven**. 

    ![Event Hub-naam en optionele Gebeurtenisdetails invoeren](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Sla uw wijzigingen op.

    ![Uw logische app opslaan](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    U hebt nu een actie ingesteld voor het verzenden van gebeurtenissen vanuit uw logische app. 

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/eventhubs/). 

## <a name="get-help"></a>Help opvragen

Ga naar het [forum voor Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Logic Apps en connectors kunt u stemmen op ideeën of ideeën indienen op de [site voor gebruikersfeedback van Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

*  [Andere connectors voor Azure Logic apps zoeken](./apis-list.md)