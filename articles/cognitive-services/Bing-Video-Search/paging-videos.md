---
title: Het bladeren door de beschikbare video's - Bing video's zoeken
titlesuffix: Azure Cognitive Services
description: Laat zien hoe u een pagina via alle video's die Bing kan retourneren.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 26e706b054653b8f0ad1ea14d0a9c2ca97cd227f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181789"
---
# <a name="paging-videos"></a>Wisselbestand-video 's

Wanneer u de Bing Video's zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant zijn voor de query. Als u de geschatte totale aantal beschikbare resultaten, toegang krijgen tot de antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) veld.  
  
Het volgende voorbeeld wordt de `totalEstimatedMatches` veld dat u een Video-antwoord bevat.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
Pagina met beschikbare video's, gebruikt u de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) queryparameters.  
  
De `count` parameter geeft u het aantal resultaten dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten die u in het antwoord aanvragen kunt is 105. De standaardwaarde is 35. Het werkelijke getal geleverd mogelijk lager dan aangevraagd.

De `offset` parameter geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is gebaseerd op nul en moet kleiner dan (`totalEstimatedMatches` - `count`).  
  
Als u weergeven van 20-video's per pagina wilt, stelt u `count` tot en met 20 en `offset` op 0 om de eerste pagina van de resultaten. Voor elke volgende pagina opgehoogd `offset` door 20 (bijvoorbeeld 20, 40).  

Hieronder ziet u een voorbeeld waarin 20 video's die beginnen bij verschuiving 40 aanvragen.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als de standaard `count` waarde werkt voor uw implementatie, u hoeft alleen te geven de `offset` queryparameter.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Normaal gesproken als u 35 video's op een tijdstip pagina, stelt u de `offset` queryparameter op 0 op de eerste aanvraag, en vervolgens verhogen `offset` van 35 op elke volgende aanvraag. Maar mogelijk enkele van de resultaten in het volgende antwoord duplicaten zijn van het vorige antwoord. Bijvoorbeeld, kunnen de eerste twee video's in het antwoord hetzelfde zijn als de laatste twee video's van het vorige antwoord.

Om te voorkomen dubbele resultaten, gebruikt u de [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) veld van de `Videos` object.

Bijvoorbeeld, als u op de pagina 30 video's tegelijk wilt, stelt u `count` tot en met 30 en `offset` in 0 op de eerste aanvraag. In de volgende aanvraag, stelt u de `offset` queryparameter naar de `nextOffset` waarde.


> [!NOTE]
> Wisselgeheugengebruik geldt alleen voor video's zoeken (/ video's / zoeken) en niet voor inzichten in video's (/ video's / details) of trending video's (/ video's / populair).

> [!NOTE]
> De `TotalEstimatedAnswers` veld is een schatting van het totale aantal zoekresultaten kunt u voor de huidige query ophalen.  Als u instelt `count` en `offset` parameters, de `TotalEstimatedAnswers` getal kan worden gewijzigd. 
