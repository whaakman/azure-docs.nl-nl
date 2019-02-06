---
title: Met behulp van trefwoordenrangschikking om weer te geven antwoorden - Bing entiteiten zoeken
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de volgorde om weer te geven van de antwoorden die de Bing Entity Search API retourneert.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 56215bfa17343576b6bebec3a5dc5076ac56073c
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754129"
---
# <a name="using-ranking-to-display-entity-search-results"></a>Met behulp van trefwoordenrangschikking om weer te geven van de zoekresultaten van entiteiten  

Elke entiteit zoeken antwoord bevat een [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) antwoord waarmee wordt aangegeven hoe u resultaten geretourneerd door de Bing Entity Search API moet weergeven. Het antwoord rangorde groepen resultaten in de pool, hoofdlijnen, en de inhoud van de zijbalk. Het resultaat van de pool is het resultaat van de belangrijkste of opvallende en moet eerst worden weergegeven. Als de resterende resulteert in een traditionele hoofdlijnen en de indeling van de zijbalk niet wordt weergegeven, moet u de hoofdlijnen inhoud hoger zichtbaarheid dan de zijbalk-inhoud opgeven. 
  
Binnen elke groep de [Items](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) matrix identificeert de volgorde waarin de inhoud moet worden weergegeven in. Elk item biedt twee manieren om te identificeren van het resultaat binnen een antwoord.  
 

|Veld | Description  |
|---------|---------|
|`answerType` en `resultIndex` | `answerType` het antwoord (entiteit of plaats) identificeert en `resultIndex` geeft een resultaat binnen dat antwoord (bijvoorbeeld een entiteit). De index begint bij 0.|
|`value`    | `value` Een ID die overeenkomt met de ID van een antwoord of een resultaat in het antwoord bevat. Het antwoord of de resultaten bevatten de ID, maar niet beide. |
  
Met behulp van de `answerType` en `resultIndex` is een proces in twee stappen. Gebruik eerst `answerType` voor het identificeren van het antwoord met de resultaten om weer te geven. Gebruik vervolgens `resultIndex` index in de resultaten van het antwoord voor het resultaat om weer te geven. (De `answerType` waarde is de naam van het veld in de [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) object.) Als u verwacht om samen de resultaten van het antwoord weer te geven, het antwoord rangorde item niet bevatten de `resultIndex` veld.

Met behulp van de ID, moet u overeen met de volgorde-ID met de ID van een antwoord of een van de resultaten. Als een object van het antwoord bevat een `id` veld, van het antwoord resultaten tegelijk weergeven. Bijvoorbeeld, als de `Entities` object bevat de `id` veld, alle artikelen van de entiteiten tegelijk weergeven. Als de `Entities` object omvat niet de `id` veld, wordt elke entiteit bevat een `id` veld en de reactie rangschikking van de entiteiten combineert met de resultaten plaatsen.  
  
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
> [Een web-app met één pagina maken](tutorial-bing-entities-search-single-page-app.md)
