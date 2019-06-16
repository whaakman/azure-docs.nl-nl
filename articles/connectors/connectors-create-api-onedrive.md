---
title: Verbinding maken met OneDrive - Azure Logic Apps | Microsoft Docs
description: Uploaden en beheren van bestanden met de REST-API's van OneDrive en Azure Logic Apps
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
ms.openlocfilehash: 840a010f8606387a250552d884621a96d0031f90
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106224"
---
# <a name="get-started-with-the-onedrive-connector"></a>Aan de slag met de OneDrive-connector
Verbinding maken met OneDrive voor het beheren van uw bestanden, met inbegrip van het uploaden, ophalen, verwijderen van bestanden en meer. 

Met OneDrive, u: 

* Uw werkstroom kunt maken met het opslaan van bestanden in OneDrive of bijwerken van bestaande bestanden in OneDrive. 
* Met triggers kunt u uw werkstroom starten wanneer een bestand wordt gemaakt of in OneDrive bijgewerkt.
* Acties voor het maken van een bestand, het verwijderen van een bestand, en meer gebruiken. Bijvoorbeeld, wanneer een nieuwe Office 365-e-mailbericht wordt ontvangen met een bijlage (een trigger), maak een nieuw bestand in OneDrive (een actie).

Dit artikel ziet u hoe u kunt de OneDrive-connector gebruiken in een logische app, en bevat ook de triggers en acties.

Zie voor meer informatie over Logic Apps, [wat zijn logische apps](../logic-apps/logic-apps-overview.md) en [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>Verbinding maken met OneDrive
Voordat uw logische app toegang heeft tot elke service, maakt u eerst een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, als u wilt verbinding maken met OneDrive, moet u eerst een OneDrive *verbinding*. Een verbinding wilt maken, voert u de referenties die u normaal gesproken gebruiken voor toegang tot de service die u wilt verbinden. Dus met OneDrive, voert u de referenties in uw OneDrive-account om de verbinding te maken.

### <a name="create-the-connection"></a>De verbinding maken
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Een trigger te gebruiken
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom gedefinieerd in een logische app te starten. Triggers controleren' ' de service op een interval en de frequentie die u wilt. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Typ in de logische app, "onedrive" voor een overzicht van de triggers:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Selecteer **wanneer een bestand wordt gewijzigd**. Als er al een verbinding bestaat, selecteert u de knop kiezer weergeven om een map te selecteren.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Als u wordt gevraagd of u zich aanmeldt, voert u het teken in de details van de verbinding te maken. [Maak de verbinding](connectors-create-api-onedrive.md#create-the-connection) vermeldt de stappen in dit artikel. 
   
   > [!NOTE]
   > In dit voorbeeld wordt de logische app wordt uitgevoerd wanneer een bestand in de map die u kiest is bijgewerkt. De resultaten van deze trigger, toe te voegen een andere actie die een e-mailbericht. Voeg bijvoorbeeld het Office 365 Outlook *een e-mailbericht verzenden* actie die u een e-mail stuurt wanneer een bestand wordt bijgewerkt. 

3. Selecteer de **bewerken** knop en stel de **frequentie** en **Interval** waarden. Bijvoorbeeld, als u wilt dat de trigger op te vragen om de 15 minuten, stelt de **frequentie** naar **minuut**, en stel de **Interval** naar **15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en kan automatisch worden ingeschakeld.

## <a name="use-an-action"></a>Gebruik een actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Selecteer het plusteken. Ziet u verschillende mogelijkheden: **Een actie toevoegen**, **een voorwaarde toevoegen**, of een van de **meer** opties.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Kies **een actie toevoegen**.
3. Typ in het tekstvak, "onedrive" een lijst van de beschikbare acties te krijgen.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. Kies in ons voorbeeld **OneDrive - bestand maken**. Als er al een verbinding bestaat, selecteert u de **mappad** invoeren om het bestand, de **bestandsnaam**, en kies de **bestandsinhoud** u wilt:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Als u de verbindingsgegevens wordt gevraagd, voert u de details van de verbinding te maken. [Maak de verbinding](connectors-create-api-onedrive.md#create-the-connection) in dit artikel beschrijft deze eigenschappen. 
   
   > [!NOTE]
   > In dit voorbeeld maken we een nieuw bestand in een OneDrive-map. U kunt uitvoer van een andere trigger gebruiken om de OneDrive-bestand te maken. Voeg bijvoorbeeld het Office 365 Outlook *wanneer een nieuwe e-mail binnenkomt* trigger. Voeg op de OneDrive *bestand maken* actie die gebruikmaakt van de bijlagen en de Content-Type velden binnen een ForEach te maken van het nieuwe bestand in OneDrive. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en kan automatisch worden ingeschakeld.


## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [lijst van API's](apis-list.md).