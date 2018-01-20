---
title: De Outlook van Office 365-connector in uw logische Apps toevoegen | Microsoft Docs
description: 'Logic apps maken met Office 365-connector voor het inschakelen van interactie met Office 365. Bijvoorbeeld: maken, bewerken en bijwerken van contactpersonen en agenda-items.'
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 51b8e3de639b5cce954547adb77ff13b79ad6747
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-office-365-outlook-connector"></a>Aan de slag met de Outlook van Office 365-connector
De Outlook van Office 365-connector wordt interactie met Outlook in Office 365. Met deze connector kunt maken, bewerken, en contactpersonen en agenda-items, bijwerken en ook krijgen, verzenden en e-mailbericht beantwoordt.

Met Office 365 Outlook u:

* Bouw uw werkstroom met behulp van de e-mail en agenda-functies in Office 365. 
* Met triggers kunt u uw werkstroom starten als er een nieuw e-mailadres, wanneer een agenda-item wordt bijgewerkt en meer.
* Acties gebruiken om te e-mailbericht verzenden, maakt u een nieuwe gebeurtenis en meer. Bijvoorbeeld, wanneer er een nieuw object in Salesforce (een trigger), een e-mail sturen naar uw Office 365 Outlook (een actie). 

In dit onderwerp leest u het gebruik van de Outlook van Office 365-connector in een logische app, en vermeldt tevens de triggers en acties.

> [!NOTE]
> Deze versie van het artikel is van toepassing op Logic Apps algemene beschikbaarheid (GA).
> 
> 

Zie voor meer informatie over Logic Apps, [wat zijn logic apps](../logic-apps/logic-apps-overview.md) en [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-office-365"></a>Verbinding maken met Office 365
Om uw logische app toegang alle services tot, maakt u eerst een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, als u wilt verbinding maken met Office 365 Outlook, moet u eerst een Office 365 *verbinding*. Voer de referenties die u gebruikt om toegang tot de service die u verbinding maken wilt met een verbinding wilt maken. Met Outlook van Office 365, voert u de referenties op voor uw Office 365-account om de verbinding te maken.

## <a name="create-the-connection"></a>De verbinding maken
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Een trigger te gebruiken
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. Triggers pollen' ' de service op het interval en de gewenste frequentie. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Typ in de logische app, 'office 365' om een lijst van de triggers:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Selecteer **Office 365 Outlook - bij het snel starten van een aanstaande gebeurtenis**. Als er al een verbinding bestaat, selecteert u een kalender in de lijst vervolgkeuzelijst.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Als u wordt gevraagd aan te melden, voert u het teken in details om de verbinding te maken. [De verbinding](connectors-create-api-office365-outlook.md#create-the-connection) vermeldt de stappen in dit onderwerp. 
   
   > [!NOTE]
   > In dit voorbeeld wordt de logische app uitgevoerd wanneer een gebeurtenis wordt bijgewerkt. U kunt de resultaten van deze trigger toevoegen nog een actie die u een SMS-bericht verzonden. Bijvoorbeeld, voeg de Twilio *bericht verzenden* actie die tekst wanneer de gebeurtenis wordt gestart binnen 15 minuten. 
   > 
   > 
3. Selecteer de **bewerken** knop en stel de **frequentie** en **Interval** waarden. Bijvoorbeeld, als u wilt dat de trigger voor het pollen van elke 15 minuten, wordt ingesteld de **frequentie** naar **minuut**, en stel de **Interval** naar **15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en automatisch kan worden ingeschakeld.

## <a name="use-an-action"></a>Gebruik een actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Selecteer het plusteken. Ziet u verschillende mogelijkheden: **een actie toevoegen**, **een voorwaarde toevoegen**, of een van de **meer** opties.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Kies **een actie toevoegen**.
3. Typ 'office 365' om een lijst met alle beschikbare acties in het tekstvak.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. Kies in ons voorbeeld **Outlook van Office 365 - maken contact op met**. Als er al een verbinding bestaat, kiest u de **map-ID**, **voornaam**, en andere eigenschappen:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Als u wordt gevraagd om de verbindingsinformatie, voert u de details om de verbinding te maken. [De verbinding](connectors-create-api-office365-outlook.md#create-the-connection) in dit onderwerp beschrijft deze eigenschappen. 
   
   > [!NOTE]
   > In dit voorbeeld maken we een nieuwe contactpersoon in Outlook van Office 365. De uitvoer van een andere trigger kunt u de contactpersoon maken. Bijvoorbeeld, voeg de SalesForce *wanneer een object wordt gemaakt* trigger. Voeg vervolgens de Office 365 Outlook *maken contact op met* actie die de SalesForce-velden gebruikt voor het maken van de nieuwe nieuwe contactpersoon in Office 365. 
   > 
   > 
5. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en automatisch kan worden ingeschakeld.

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/office365connector/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).

