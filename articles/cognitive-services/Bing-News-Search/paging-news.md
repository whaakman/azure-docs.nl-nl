---
title: Het bladeren door de beschikbare nieuwsartikelen | Microsoft Docs
description: Laat zien hoe u de pagina door alle nieuwsartikelen Bing kunt terugkeren.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: EA388F72-FA43-493B-967C-9560B3243C62
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 2c90d468536f0864d7deac073667e29e9a54692f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344435"
---
# <a name="paging-news"></a>Paginering nieuws

Wanneer u de nieuws zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant voor de query zijn mogelijk. Als u het geschatte aantal beschikbare resultaten, toegang krijgen tot het antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) veld.  
  
Het volgende voorbeeld wordt de `totalEstimatedMatches` veld dat een nieuwsantwoord omvat.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Pagina via de beschikbare artikelen, gebruiken de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) queryparameters.  
  
De `count` parameter geeft u het aantal resultaten te retourneren in het antwoord. Het maximum aantal resultaten dat u in het antwoord aanvragen mogelijk is 100. De standaardwaarde is 10. Het werkelijke aantal geleverde mogelijk minder dan aangevraagd.

De `offset` parameter geeft u het aantal resultaten om over te slaan. De `offset` is gebaseerd op nul zijn en moet kleiner zijn dan (`totalEstimatedMatches` - `count`).  

Als u 20 artikelen per pagina weergegeven wilt, stelt u `count` 20 en `offset` op 0 om op te halen van de eerste pagina van de resultaten. Voor elke volgende pagina opgehoogd `offset` door 20 (bijvoorbeeld 20, 40).  
  
Hieronder ziet u een voorbeeld van 20 nieuwsartikelen beginnen bij verschuiving 40 aanvragen.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Als de standaard `count` waarde werkt voor uw implementatie, geeft u alleen de `offset` queryparameter zoals weergegeven in het volgende voorbeeld:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> Paginering geldt alleen voor nieuws zoeken (nieuws/search), en niet voor trends onderwerpen (nieuws/trendingtopics) of nieuwscategorieën (/ nieuws).