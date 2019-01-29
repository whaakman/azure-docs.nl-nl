---
title: Met behulp van trefwoordenrangschikking om weer te geven antwoorden - Bing entiteiten zoeken
titlesuffix: Azure Cognitive Services
description: Ziet u hoe u volgorde om weer te geven van de antwoorden die de Bing Entity Search API retourneert.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: aahi
ms.openlocfilehash: f0cae32acf2db62a5d3c060ea944b1131252beda
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195916"
---
# <a name="using-ranking-to-display-results"></a>Met behulp van trefwoordenrangschikking om resultaten weer te geven  

Elke entiteit zoeken antwoord bevat een [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) beantwoorden, net als bij het item in een reactie van de Bing webzoekopdrachten, die Hiermee geeft u op hoe moet u de lijst met zoekresultaten weergeven. Het antwoord rangorde groepen resultaten in de pool, hoofdlijnen, en de inhoud van de zijbalk. Het resultaat van de pool is het resultaat van de belangrijkste of opvallende en moet eerst worden weergegeven. Als de resterende resulteert in een traditionele hoofdlijnen en de indeling van de zijbalk niet wordt weergegeven, moet u de hoofdlijnen inhoud hoger zichtbaarheid dan de zijbalk-inhoud opgeven. 
  
Binnen elke groep de [Items](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) matrix identificeert de volgorde waarin de inhoud moet worden weergegeven in. Elk item biedt twee manieren om te identificeren van het resultaat binnen een antwoord.  
  
-   `answerType` en `resultIndex` : de `answerType` veld identificeert het antwoord (entiteit of plaats) en `resultIndex` geeft een resultaat in het antwoord (bijvoorbeeld een entiteit). De index is op basis van nul.  
  
-   `value` : De `value` veld bevat een ID die overeenkomt met de ID van een antwoord of een resultaat in het antwoord. Het antwoord of de resultaten bevatten de ID, maar niet beide.  
  
Met behulp van de ID, moet u overeen met de volgorde-ID met de ID van een antwoord of een van de resultaten. Als een object van het antwoord bevat een `id` veld, van het antwoord resultaten tegelijk weergeven. Bijvoorbeeld, als de `Entities` object bevat de `id` veld, alle artikelen van de entiteiten tegelijk weergeven. Als de `Entities` object omvat niet de `id` veld, wordt elke entiteit bevat een `id` veld en de reactie rangschikking van de entiteiten combineert met de resultaten plaatsen.  
  
Met behulp van de `answerType` en `resultIndex` is een proces in twee stappen. U gebruikt eerst `answerType` voor het identificeren van het antwoord met de resultaten om weer te geven. Vervolgens u gebruikt `resultIndex` index in de resultaten van het antwoord voor het resultaat om weer te geven. (De `answerType` waarde is de naam van het veld in de [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) object.) Als u verwacht om samen de resultaten van het antwoord weer te geven, het antwoord rangorde item niet bevatten de `resultIndex` veld.

## <a name="ranking-response-example"></a>Voorbeeld van de reactie rangorde

Hieronder ziet u een voorbeeld [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
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

Op basis van deze rangschikking antwoord, zou de zijbalk de resultaten van de twee entiteiten met betrekking tot Jimi Hendrix weergegeven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bing Entity Search-zelfstudie](tutorial-bing-entities-search-single-page-app.md)
