---
title: De OneDrive-connector in uw logische Apps toevoegen | Microsoft Docs
description: Overzicht van de connector OneDrive met parameters van de REST-API
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: e988ae22a3d6e47591c37ad0b4c9f781d523bcc8
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-onedrive-connector"></a>Aan de slag met de OneDrive-connector
Verbinding maken met OneDrive voor het beheren van de bestanden, waaronder het uploaden, ophalen en verwijderen van bestanden en meer. 

Met OneDrive, u: 

* Het bouwen van uw werkstroom door het opslaan van bestanden in OneDrive of bijwerken van bestaande bestanden in OneDrive. 
* Met triggers kunt u uw werkstroom te starten wanneer een bestand wordt gemaakt of binnen uw OneDrive bijgewerkt.
* Acties gebruiken om te maken van een bestand, verwijderen van een bestand en meer. Bijvoorbeeld wanneer een nieuwe Office 365-e-mail is ontvangen met een bijlage (een trigger), maakt u een nieuw bestand in OneDrive (een actie).

In dit onderwerp leest u hoe de OneDrive-connector gebruiken in een logische app en vermeldt tevens de triggers en acties.

Zie voor meer informatie over Logic Apps, [wat zijn logic apps](../logic-apps/logic-apps-overview.md) en [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>Verbinding maken met OneDrive
Om uw logische app toegang alle services tot, maakt u eerst een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, als u wilt verbinding maken met OneDrive, moet u eerst een OneDrive *verbinding*. Voer de referenties die u gebruikt om toegang tot de service die u verbinding maken wilt met een verbinding wilt maken. Voer dus met OneDrive, de referenties naar uw OneDrive-account om de verbinding te maken.

### <a name="create-the-connection"></a>De verbinding maken
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Een trigger te gebruiken
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. Triggers pollen' ' de service op het interval en de gewenste frequentie. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Typ in de logische app, 'onedrive' om een lijst van de triggers:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Selecteer **wanneer een bestand wordt gewijzigd**. Als er al een verbinding bestaat, selecteert u de objectkiezer tonen knop om een map te selecteren.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Als u wordt gevraagd aan te melden, voert u het teken in details om de verbinding te maken. [De verbinding](connectors-create-api-onedrive.md#create-the-connection) vermeldt de stappen in dit onderwerp. 
   
   > [!NOTE]
   > In dit voorbeeld wordt de logische app wordt uitgevoerd als een bestand in de map die u kiest, wordt bijgewerkt. U kunt de resultaten van deze trigger toevoegen nog een actie die u een e-mailbericht verzendt. Bijvoorbeeld, voeg de Office 365 Outlook *e-mailbericht verzenden* actie die u e-mailberichten wanneer een bestand wordt bijgewerkt. 

3. Selecteer de **bewerken** knop en stel de **frequentie** en **Interval** waarden. Bijvoorbeeld, als u wilt dat de trigger voor het pollen van elke 15 minuten, wordt ingesteld de **frequentie** naar **minuut**, en stel de **Interval** naar **15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en automatisch kan worden ingeschakeld.

## <a name="use-an-action"></a>Gebruik een actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Selecteer het plusteken. Ziet u verschillende mogelijkheden: **een actie toevoegen**, **een voorwaarde toevoegen**, of een van de **meer** opties.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Kies **een actie toevoegen**.
3. Typ in het tekstvak 'onedrive' om een lijst met alle beschikbare acties.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. Kies in ons voorbeeld **OneDrive - bestand maken**. Als er al een verbinding bestaat, selecteert u de **mappad** invoeren om het bestand de **bestandsnaam**, en kies de **bestandsinhoud** gewenste:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Als u wordt gevraagd om de verbindingsinformatie, voert u de details om de verbinding te maken. [De verbinding](connectors-create-api-onedrive.md#create-the-connection) in dit onderwerp beschrijft deze eigenschappen. 
   
   > [!NOTE]
   > In dit voorbeeld maken we een nieuw bestand in een map OneDrive. Uitvoer van een andere trigger kunt u het bestand OneDrive te maken. Bijvoorbeeld, voeg de Office 365 Outlook *wanneer een nieuw e-mailadres binnenkomt* trigger. Voeg vervolgens de OneDrive *Create file* actie die u de bijlagen en de Content-Type gebruikt velden binnen een ForEach maken van het nieuwe bestand in OneDrive. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en automatisch kan worden ingeschakeld.


## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).