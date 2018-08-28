---
title: Maak verbinding met Dropbox - Azure Logic Apps | Microsoft Docs
description: Uploaden en beheren van bestanden met Dropbox REST-API's en Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/15/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 256a0b34d5050e17abe5bb98ca0c13ab0b61787e
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094435"
---
# <a name="get-started-with-the-dropbox-connector"></a>Aan de slag met de Dropbox-connector
Verbinding maken met Dropbox om uw bestanden te beheren. U kunt verschillende acties uitvoeren zoals uploaden, bijwerken, ophalen en verwijderen van bestanden in Dropbox.

Gebruik [connectors](apis-list.md), moet u eerst een logische app maken. U kunt aan de slag door [het maken van een logische app nu](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-dropbox"></a>Maak verbinding met Dropbox
Voordat uw logische app krijgt een service tot toegang, moet u eerst maken een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, als u wilt verbinding maken met Dropbox, moet u eerst een Dropbox *verbinding*. Een verbinding wilt maken, moet u voor de referenties die u normaal gesproken gebruiken voor toegang tot de service die u wilt verbinden. Dus in de Dropbox-voorbeeld, zou u moeten de referenties naar uw Dropbox-account om te kunnen maken van de verbinding met Dropbox. 

### <a name="create-a-connection-to-dropbox"></a>Maak een verbinding met Dropbox
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Gebruik een Dropbox-trigger
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom gedefinieerd in een logische app te starten. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).

In dit voorbeeld gebruiken we de **wanneer een bestand wordt gemaakt** trigger. Wanneer deze trigger plaatsvindt, noemen we de **bestandsinhoud ophalen via pad** Dropbox-actie. 

1. Voer *dropbox* in het zoekvak op de ontwerper van logische Apps, schakelt u vervolgens de **Dropbox - wanneer een bestand wordt gemaakt** trigger.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Selecteer de map waarin u wilt maken van het bestand bijhouden. Selecteren... (te herkennen in een rood kader) en blader naar de map die u selecteren wilt voor de invoer van de trigger.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Gebruik een Dropbox-actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Nu dat de trigger is toegevoegd, volgt u deze stappen voor het toevoegen van een actie die de inhoud van het nieuwe bestand krijgt.

1. Selecteer **+ nieuwe stap** om toe te voegen van de actie die u uitvoeren wilt wanneer een nieuw bestand wordt gemaakt.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Selecteer **een actie toevoegen**. Deze wordt geopend de zoekvak waarin u voor elke actie u zoeken kunt wilt uitvoeren.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Voer *dropbox* om te zoeken naar acties die verband houden met Dropbox.  
4. Selecteer **Dropbox - bestandsinhoud ophalen via het pad** als de actie te ondernemen wanneer een nieuw bestand wordt gemaakt in de geselecteerde Dropbox-map. Hiermee opent u de actie in het Configuratiescherm. U wordt gevraagd om uw logische app voor toegang tot uw Dropbox-account als u niet hebt gedaan, eerder te autoriseren.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Selecteren... (dat zich bevindt in aan de rechterkant van de **bestandspad** besturingselement) en blader naar het bestandspad dat u wilt gebruiken. Of gebruik de **bestandspad** token om de snelheid van uw logische app te maken.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Sla uw werk en maak een nieuw bestand in Dropbox uw werkstroom activeren.  

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/dropbox/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [lijst van API's](apis-list.md).
