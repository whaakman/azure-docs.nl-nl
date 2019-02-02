---
title: Het bladeren door de beschikbare video's - Bing video's zoeken
titlesuffix: Azure Cognitive Services
description: Leer hoe u pagina via alle video's die wordt geretourneerd door de Bing video's zoeken-API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 5fd11088c84327325040a05de9616f19c3455b01
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564898"
---
# <a name="paging-through-video-search-results"></a>Door video zoekresultaten bladeren

De Bing video's zoeken-API retourneert een subset van alle zoekresultaten die deze voor uw query gevonden. U kunt door te bladeren door deze resultaten met opeenvolgende aanroepen naar de API, ophalen en ze weergeven in uw toepassing.

> [!NOTE]
> Wisselgeheugengebruik geldt alleen voor video's zoeken (/ video's / zoeken) en niet voor inzichten in video's (/ video's / details) of trending video's (/ video's / populair).

## <a name="total-estimated-matches"></a>Totale geschatte komt overeen met

Als u het geschatte aantal gevonden resultaten, gebruikt de [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) veld in het JSON-antwoord.   
  
```json  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
## <a name="paging-through-videos"></a>Door video's bladeren

Pagina met beschikbare video's, gebruikt u de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) queryparameters bij het verzenden van uw aanvraag.  
  

|Parameter  |Description  |
|---------|---------|
|`count`     | Hiermee geeft u het aantal resultaten dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten die u in het antwoord aanvragen kunt is 100. De standaardwaarde is 10. Het werkelijke getal geleverd mogelijk lager dan aangevraagd.        |
|`offset`     | Hiermee geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is gebaseerd op nul en moet kleiner dan (`totalEstimatedMatches` - `count`).          |

Bijvoorbeeld, als u weergeven van 20 artikelen per pagina wilt, stelt u `count` tot en met 20 en `offset` op 0 om de eerste pagina van de resultaten. Voor elke volgende pagina opgehoogd `offset` door 20 (bijvoorbeeld 20, 40).  
  
Het volgende voorbeeld vraagt om 20 video's, beginnend bij de offset 40.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als u de standaardwaarde voor de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count), u hoeft alleen te geven de `offset` queryparameter, zoals in het volgende voorbeeld.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als u via 35 video's op een tijdstip, stelt u de `offset` queryparameter op 0 op de eerste aanvraag, en vervolgens verhogen `offset` van 35 op elke volgende aanvraag. Bepaalde resultaten in het volgende antwoord bevatten echter dubbele videoresultaten uit het vorige antwoord. Bijvoorbeeld, de eerste twee video's in een reactie mogelijk hetzelfde als de laatste twee video's van het vorige antwoord.

Om te voorkomen dubbele resultaten, gebruikt u de [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) veld van de `Videos` object.

Bijvoorbeeld, als u op pagina 30 video's tegelijk wilt, kunt u instellen `count` tot en met 30 en `offset` in 0 op de eerste aanvraag. In de volgende aanvraag, stelt u de `offset` queryparameter naar de `nextOffset` waarde.

> [!NOTE]
> De `TotalEstimatedAnswers` veld is een schatting van het totale aantal zoekresultaten kunt u voor de huidige query ophalen.  Als u instelt `count` en `offset` parameters, de `TotalEstimatedAnswers` getal kan worden gewijzigd. 
  
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
[Inzichten in video's ophalen](video-insights.md)
