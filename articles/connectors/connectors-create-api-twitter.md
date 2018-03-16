---
title: Informatie over het gebruik van de Twitter-connector in logic apps | Microsoft Docs
description: Overzicht van Twitter-connector met de parameters van de REST-API
services: 
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: eb953ee7701d407b9b75a0699f53b9b64828a0e5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-twitter-connector"></a>Aan de slag met de Twitter-connector
Met de Twitter-connector kunt u het volgende doen:

* Tweets post en get-tweets
* Toegang tot tijdlijnen, vrienden en pc-gebruikers
* Voer één van de andere bewerkingen en triggers die zijn beschreven in dit artikel

Gebruik [elke connector](apis-list.md), moet u eerst een logische app maken. U kunt aan de slag door [maken van een logische app nu](../logic-apps/quickstart-create-first-logic-app-workflow.md).  

## <a name="connect-to-twitter"></a>Verbinding maken met Twitter
Om uw logische app toegang alle services tot, moet u eerst maken een *verbinding* naar de service. Een [verbinding](connectors-overview.md) biedt connectiviteit tussen een logische app en een andere service.  

### <a name="create-a-connection-to-twitter"></a>Maak een verbinding met Twitter
> [!INCLUDE [Steps to create a connection to Twitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a>Gebruik een trigger Twitter
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).

In dit voorbeeld gebruikt u de **wanneer een nieuwe tweet wordt gepost** trigger om te zoeken naar #Seattle. En als #Seattle wordt gevonden, het bijwerken van een bestand in Dropbox met de tekst van de tweet. In een enterprise-voorbeeld, kan u de naam van uw bedrijf zoeken en een SQL-database bijwerken met de tekst van de tweet.

1. Voer *twitter* in het zoekvak op de ontwerpfunctie van logic apps selecteert u vervolgens de **Twitter - wanneer een nieuwe tweet wordt teruggepost** trigger   
   ![Twitter triggerafbeelding 1](./media/connectors-create-api-twitter/trigger-1.png)  
2. Voer *#Seattle* in de **zoektekst** besturingselement  
   ![Afbeelding van de trigger Twitter 2](./media/connectors-create-api-twitter/trigger-2.png) 

Op dit moment is uw logische app geconfigureerd met een trigger die een uitvoering van de andere triggers en acties in de werkstroom wordt gestart. 

> [!NOTE]
> Voor een logische app wilt gebruiken, moet er ten minste één trigger en één actie bevatten. Gebruik de stappen in de volgende sectie voor een actie toevoegen.

## <a name="add-a-condition"></a>Een voorwaarde toevoegen
We bent alleen geïnteresseerd in tweets van gebruikers met meer dan 50 gebruikers. Dus is een voorwaarde waarmee wordt bevestigd het aantal pc-gebruikers dat eerst toegevoegd aan de logische app.  

1. Selecteer **+ een nieuwe stap** om toe te voegen van de actie die u uitvoeren wilt wanneer #Seattle in een nieuwe tweet wordt gevonden  
   ![Afbeelding 1 Twitter](../../includes/media/connectors-create-api-twitter/action-1.png)  
2. Selecteer de **een voorwaarde toevoegen** koppeling.  
   ![Twitter voorwaarde afbeelding 1](../../includes/media/connectors-create-api-twitter/condition-1.png)   
   Hiermee opent u de **voorwaarde** besturingselement waarin kunt u voorwaarden zoals controleren *gelijk is aan*, *is minder dan*, *is groter dan*, *bevat*, enzovoort.  
   ![Afbeelding van de voorwaarde Twitter 2](../../includes/media/connectors-create-api-twitter/condition-2.png)   
3. Selecteer de **kiest u een waarde** besturingselement. In dit besturingselement, kunt u een of meer van de eigenschappen van de vorige acties of triggers selecteren. De voorwaarde van de waarde van deze eigenschap wordt geëvalueerd als waar of ONWAAR.
   ![Twitter voorwaarde afbeelding 3](../../includes/media/connectors-create-api-twitter/condition-3.png)   
4. Selecteer de **...**  uitbreiden van de lijst met eigenschappen, zodat u kunt zien dat alle eigenschappen die beschikbaar zijn.        
   ![Twitter voorwaarde afbeelding 4](../../includes/media/connectors-create-api-twitter/condition-4.png)   
5. Selecteer de **pc-gebruikers aantal** eigenschap.    
   ![Twitter voorwaarde afbeelding 5](../../includes/media/connectors-create-api-twitter/condition-5.png)   
6. Let op de pc-gebruikers count-eigenschap is nu in het besturingselement waarde.    
   ![Twitter voorwaarde afbeelding 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. Selecteer **is groter dan** uit de lijst operators.    
   ![Twitter voorwaarde afbeelding 7](../../includes/media/connectors-create-api-twitter/condition-7.png)   
8. 50 invoeren als de operand voor de *is groter dan* operator.  
   De voorwaarde is nu toegevoegd. Sla uw werk met de **opslaan** koppeling in het menu.    
   ![Twitter voorwaarde afbeelding 8](../../includes/media/connectors-create-api-twitter/condition-8.png)   

## <a name="use-a-twitter-action"></a>Gebruik een Twitter-actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Nu dat er een trigger is, een actie die een nieuwe tweet met de inhoud van de tweets gevonden door de trigger toevoegen. Voor deze procedure worden alleen tweets van gebruikers met meer dan 50 pc-gebruikers geplaatst.  

In de volgende stap de Twitter actie die een tweet gebruikmakend van de eigenschappen van elke tweet die wordt gepost door een gebruiker met meer dan 50 pc-gebruikers toevoegen.  

1. Selecteer **een actie toevoegen**. Deze stap wordt de zoekbesturing waar u naar andere acties en triggers zoeken kunt geopend.  
   ![Twitter voorwaarde afbeelding 9](../../includes/media/connectors-create-api-twitter/condition-9.png)   
2. Voer *twitter* in het zoekvak selecteert de **Twitter - een tweet boeken** in te grijpen. Hiermee opent u deze stap de **een tweet boeken** beheren waarin u alle gegevens invoeren voor de tweet wordt geplaatst.      
   ![Actie afbeelding 1-5 Twitter](../../includes/media/connectors-create-api-twitter/action-1-5.png)   
3. Selecteer de **Tweet tekst** besturingselement. Alle uitvoerwaarden van de vorige acties en triggers in de logische app worden nu weergegeven. U kunt een van deze uitvoer selecteren en ze als onderdeel van de tekst tweet van de nieuwe tweet gebruiken.     
   ![Afbeelding 2 Twitter](../../includes/media/connectors-create-api-twitter/action-2.png)   
4. Selecteer **gebruikersnaam**   
5. Onmiddellijk na de gebruikersnaam, voer *zegt:* in het tekstvak tweet.
6. Selecteer *Tweet tekst*.       
   ![Afbeelding 3 Twitter](../../includes/media/connectors-create-api-twitter/action-3.png)   
7. De workflow activeert, sla uw werk en een tweet met de hashtag #Seattle wordt verzonden.


## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/twitterconnector/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)