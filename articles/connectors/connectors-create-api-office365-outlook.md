---
title: Verbinding maken met Office 365 Outlook - Azure Logic Apps | Microsoft Docs
description: E-mail, contactpersonen en agenda's met REST API's van Office 365 en Azure Logic Apps beheren
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 10/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 0355f121a09e1ba89f98a8af5037eb1371db2242
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215635"
---
# <a name="get-started-with-the-office-365-outlook-connector"></a>Aan de slag met de Office 365 Outlook-connector
De Office 365 Outlook-connector wordt interactie met Outlook in Office 365. Met deze connector kunt maken, bewerken, bijwerken, contactpersonen en agenda-items, en ook ophalen, verzenden en e-mail beantwoorden.

Met Office 365 Outlook, u:

* Bouw uw werkstroom met behulp van de e-mail en agenda-functies in Office 365. 
* Met triggers kunt u uw werkstroom starten wanneer er een nieuwe e-mail wanneer een agenda-item wordt bijgewerkt, en meer.
* Acties voor een e-mailbericht verzenden, het maken van een nieuwe agendagebeurtenis en meer gebruiken. Bijvoorbeeld, wanneer er een nieuw object in Salesforce (een trigger), een e-mail verzenden naar uw Office 365 Outlook (een actie). 

Dit artikel ziet u hoe u de Office 365 Outlook-connector in een logische app, en bevat ook de triggers en acties.

> [!NOTE]
> Deze versie van het artikel is van toepassing op Logic Apps-algemene beschikbaarheid (GA).
> 
> 

Zie voor meer informatie over Logic Apps, [wat zijn logische apps](../logic-apps/logic-apps-overview.md) en [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-office-365"></a>Verbinding maken met Office 365
Voordat uw logische app toegang heeft tot elke service, maakt u eerst een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, als u wilt verbinding maken met Office 365 Outlook, moet u eerst een Office 365 *verbinding*. Een verbinding wilt maken, voert u de referenties die u normaal gesproken gebruiken voor toegang tot de service die u wilt verbinden. Met Office 365 Outlook, voert u de referenties op voor uw Office 365-account om de verbinding te maken.

## <a name="create-the-connection"></a>De verbinding maken
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Een trigger te gebruiken
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom gedefinieerd in een logische app te starten. Triggers controleren' ' de service op een interval en de frequentie die u wilt. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Typ 'office 365' een lijst van de triggers te krijgen in de logische app:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Selecteer **Office 365 Outlook - wanneer een aanstaande agendagebeurtenis binnenkort begint**. Als er al een verbinding bestaat, moet u vervolgens een agenda selecteren uit de vervolgkeuzelijst.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Als u wordt gevraagd of u zich aanmeldt, voert u het teken in de details van de verbinding te maken. [Maak de verbinding](connectors-create-api-office365-outlook.md#create-the-connection) vermeldt de stappen in dit onderwerp. 
   
   > [!NOTE]
   > In dit voorbeeld wordt de logische app uitgevoerd wanneer een agenda-gebeurtenis wordt bijgewerkt. U kunt de resultaten van deze trigger, een andere actie toevoegen die u een SMS-bericht stuurt. Voeg bijvoorbeeld de Twilio *bericht verzenden* actie die teksten u bij het starten van de agendagebeurtenis in 15 minuten. 
   > 
   > 
3. Selecteer de **bewerken** knop en stel de **frequentie** en **Interval** waarden. Bijvoorbeeld, als u wilt dat de trigger op te vragen om de 15 minuten, stelt de **frequentie** naar **minuut**, en stel de **Interval** naar **15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en kan automatisch worden ingeschakeld.

## <a name="use-an-action"></a>Gebruik een actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Selecteer het plusteken. Ziet u verschillende mogelijkheden: **een actie toevoegen**, **een voorwaarde toevoegen**, of een van de **meer** opties.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Kies **een actie toevoegen**.
3. Typ in het tekstvak, "office 365' als u een lijst met alle beschikbare acties.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. Kies in ons voorbeeld **Office 365 Outlook - contactpersoon maken**. Als er al een verbinding bestaat, kiest u de **map-ID**, **voornaam**, en andere eigenschappen:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Als u de verbindingsgegevens wordt gevraagd, voert u de details van de verbinding te maken. [Maak de verbinding](connectors-create-api-office365-outlook.md#create-the-connection) in dit onderwerp beschrijft deze eigenschappen. 
   
   > [!NOTE]
   > In dit voorbeeld maken we een nieuwe contactpersoon in Office 365 Outlook. Uitvoer van een andere trigger kunt u de contactpersoon maken. Voeg bijvoorbeeld SalesForce *wanneer een object wordt gemaakt* trigger. Voeg op de Office 365 Outlook *-contactpersoon maken* actie die de SalesForce-velden gebruikt voor het maken van de nieuwe contactpersoon in Office 365. 
   > 
   > 
5. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en kan automatisch worden ingeschakeld.

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/office365connector/). 

## <a name="next-steps"></a>Volgende stappen
[Maak een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Verken de andere beschikbare connectors in Logic Apps op onze [lijst van API's](apis-list.md).

