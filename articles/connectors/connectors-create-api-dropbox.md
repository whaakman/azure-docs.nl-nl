---
title: Dropbox-connector in Azure Logic Apps | Microsoft Docs
description: Logic apps maken met Azure App service. Verbinding maken met Dropbox voor het beheren van uw bestanden. U kunt uitvoeren van verschillende acties zoals het uploaden, bijwerken, ophalen en verwijderen van bestanden in Dropbox.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 0d09580c60fd620811b539147439d0922839fe7e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-dropbox-connector"></a>Aan de slag met de Dropbox-connector
Verbinding maken met Dropbox voor het beheren van uw bestanden. U kunt uitvoeren van verschillende acties zoals het uploaden, bijwerken, ophalen en verwijderen van bestanden in Dropbox.

Gebruik [elke connector](apis-list.md), moet u eerst een logische app maken. U kunt aan de slag door [maken van een logische app nu](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-dropbox"></a>Verbinding maken met Dropbox
Om uw logische app toegang alle services tot, moet u eerst maken een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, om verbinding met Dropbox, moet u eerst een Dropbox *verbinding*. Een verbinding wilt maken, moet u de referenties die u gebruikt om toegang tot de service die u verbinding wilt maken. Dus in het voorbeeld Dropbox moet u de referenties voor uw Dropbox-account om te kunnen maken van de verbinding met Dropbox. [Meer informatie over verbindingen]()

### <a name="create-a-connection-to-dropbox"></a>Maak een verbinding met Dropbox
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Gebruik een trigger Dropbox
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

In dit voorbeeld gebruiken we de **wanneer een bestand wordt gemaakt** trigger. Wanneer deze trigger optreedt, noemen we de **ophalen met behulp van pad bestandsinhoud** Dropbox-actie. 

1. Voer *dropbox* in het zoekvak op de ontwerpfunctie Logic Apps, selecteer de **Dropbox - wanneer een bestand wordt gemaakt** trigger.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Selecteer de map waarin u wilt maken van het bestand bijhouden. Selecteren... (geïdentificeerd in het vak rood) en blader naar de map die u selecteren wilt voor de trigger de invoer.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Gebruik een Dropbox-actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Nu dat de trigger is toegevoegd, volg deze stappen uit om een actie die de inhoud van het nieuwe bestand krijgen.

1. Selecteer **+ een nieuwe stap** om toe te voegen van de actie die u uitvoeren wilt wanneer een nieuw bestand wordt gemaakt.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Selecteer **een actie toevoegen**. Deze wordt geopend het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Voer *dropbox* om te zoeken naar acties met betrekking tot Dropbox.  
4. Selecteer **Dropbox - bestandsinhoud ophalen met behulp van pad** als de actie moet worden uitgevoerd wanneer een nieuw bestand is gemaakt in de geselecteerde Dropbox-map. Hiermee opent u de actie in het Configuratiescherm. U wordt gevraagd uw logische app toegang tot uw Dropbox-account als u nog niet gedaan eerder autoriseren.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Selecteren... (te vinden op de rechterkant van de **bestandspad** besturingselement) en blader naar het bestandspad dat u wilt gebruiken. Of gebruik de **bestandspad** token gebruikt voor het maken van uw logische app te versnellen.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Sla uw werk op en maak een nieuw bestand in Dropbox voor het activeren van uw werkstroom.  

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/dropbox/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).