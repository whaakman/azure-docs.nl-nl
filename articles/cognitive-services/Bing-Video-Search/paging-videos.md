---
title: Het bladeren door de beschikbare video's | Microsoft Docs
description: Laat zien hoe pagina via alle video's die Bing kan retourneren.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 910A485F-BCF3-42B9-958D-DD48BDEDA965
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 00476825eb3fc1008c3f2172b591d8b7a2f35884
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344424"
---
# <a name="paging-videos"></a>Paginering video 's

Wanneer u de Video zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant voor de query zijn. Als u het geschatte aantal beschikbare resultaten, toegang krijgen tot het antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) veld.  
  
Het volgende voorbeeld wordt de `totalEstimatedMatches` veld dat u een Video-antwoord bevat.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
Pagina via de video's die beschikbaar is, gebruikt u de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) queryparameters.  
  
De `count` parameter geeft u het aantal resultaten te retourneren in het antwoord. Het maximum aantal resultaten dat u in het antwoord aanvragen mogelijk is 105. De standaardwaarde is 35. Het werkelijke aantal geleverde mogelijk minder dan aangevraagd.

De `offset` parameter geeft u het aantal resultaten om over te slaan. De `offset` is gebaseerd op nul zijn en moet kleiner zijn dan (`totalEstimatedMatches` - `count`).  
  
Als u 20 video's per pagina weergegeven wilt, stelt u `count` 20 en `offset` op 0 om op te halen van de eerste pagina van de resultaten. Voor elke volgende pagina opgehoogd `offset` door 20 (bijvoorbeeld 20, 40).  

Hieronder ziet u een voorbeeld van 20 video's die beginnen bij verschuiving 40 aanvragen.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als de standaard `count` waarde werkt voor uw implementatie, moet u alleen om op te geven de `offset` queryparameter.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Normaal gesproken als u pagina 35 video's op een tijdstip, stelt u de `offset` parameterwaarde 0 op de eerste aanvraag opvragen en verhoog vervolgens `offset` door 35 bij elke volgende aanvraag. Sommige van de resultaten in het volgende antwoord zijn echter duplicaten zijn van het vorige antwoord. Bijvoorbeeld kunnen de eerste twee video's in het antwoord niet hetzelfde als de laatste twee video's van het vorige antwoord.

Om te voorkomen dubbele resultaten, gebruiken de [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) veld van de `Videos` object.

Bijvoorbeeld, als u pagina 30 video's op een tijdstip wilt, stelt u `count` tot en met 30 en `offset` in 0 op de eerste aanvraag. In de volgende aanvraag, stelt u de `offset` query-parameter voor de `nextOffset` waarde.


> [!NOTE]
> Paginering geldt alleen voor video's zoeken (video's / search) en niet voor video insights (video's / details) of trends video's (video's / trends).