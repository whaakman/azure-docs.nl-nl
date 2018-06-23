---
title: Met behulp van rangschikking om antwoorden te geven | Microsoft Docs
description: Laat zien hoe rangschikking gebruiken om de antwoorden die de Search-API van Bing entiteit retourneert weer te geven.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/12/2017
ms.author: v-jerkin
ms.openlocfilehash: ff5b004aaa863dbdfc460a774a5dfd658ce52537
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344423"
---
# <a name="using-ranking-to-display-results"></a>Met behulp van rangschikking om resultaten weer te geven  

Elke entiteit search-antwoord bevat een [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-entities-v7-reference#rankingresponse) beantwoorden, vergelijkbaar met de structuur in een antwoord Bing webpagina's zoeken, die Hiermee geeft u op hoe de zoekresultaten moet worden weergegeven. Het antwoord ranking groepen resultaten in de pool hoofdlijnen, en de inhoud van de zijbalk. Het resultaat van de pool is het resultaat van de belangrijkste of opvallende en eerst moet worden weergegeven. Als de resterende resulteert in een traditionele hoofdlijnen en zijbalk indeling niet wordt weergegeven, moet u de hoofdlijnen inhoud hoger zichtbaarheid dan inhoud voor de zijbalk opgeven. 
  
In elke groep de [Items](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) matrix geeft de volgorde die de inhoud moet worden weergegeven in. Elk item biedt twee methoden voor het identificeren van het resultaat binnen een antwoord.  
  
-   `answerType` en `resultIndex` : de `answerType` veld identificeert het antwoord (entiteit of plaats) en `resultIndex` identificeert een resultaat in het antwoord (bijvoorbeeld een entiteit). De index is een op nul gebaseerd.  
  
-   `value` : De `value` veld bevat een ID die overeenkomt met de ID van een antwoord of een resultaat in het antwoord. Het antwoord of de resultaten bevatten de ID, maar niet beide.  
  
Met behulp van de ID, moet u overeen met de ID van de positie met de ID van een antwoord of een van de resultaten. Als een object van het antwoord bevat een `id` veld, de resultaten van het antwoord tegelijk weergeven. Bijvoorbeeld, als de `Entities` -object bevat de `id` veld, alle entiteiten artikelen tegelijk weergeven. Als de `Entities` object bevat niet de `id` veld, wordt elke entiteit bevat een `id` veld en het antwoord ranking combineert de entiteiten met de resultaten plaatsen.  
  
Met behulp van de `answerType` en `resultIndex` is een proces in twee stappen. Gebruik eerst `answerType` voor het identificeren van het antwoord van de resultaten om weer te geven. U gebruikt `resultIndex` voor index in de resultaten van het antwoord voor het resultaat om weer te geven. (De `answerType` waarde is de naam van het veld in de [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) object.) Als u verwacht om samen de resultaten van het antwoord weer te geven, het antwoord ranking item niet bevat de `resultIndex` veld.

## <a name="ranking-response-example"></a>Ranking antwoord-voorbeeld

Hieronder vindt u een voorbeeld [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

Op basis van dit antwoord ranking, zou de zijbalk de resultaten van de twee entiteit betrekking hebben op Jimi Hendrix weergegeven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bing entiteit Search-zelfstudie](tutorial-bing-entities-search-single-page-app.md)
