---
title: Bladeren door Bing News Search resultaten
titleSuffix: Azure Cognitive Services
description: Lees hoe u door de nieuws artikelen bladeert die de Bing Nieuws zoeken-API retourneert.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: e5f8dce1a03e44758eea737ad2da419fa67c36a2
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423745"
---
# <a name="how-to-page-through-news-search-results"></a>Pagina met zoek resultaten voor nieuws

Wanneer u de Nieuws zoeken-API aanroept, retourneert Bing een lijst met resultaten die relevant zijn voor uw query. Als u het geschatte totale aantal beschik bare resultaten wilt ophalen, opent u het veld [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news-totalmatches) van het antwoord object.  
  
In het volgende voor beeld `totalEstimatedMatches` wordt het veld weer gegeven dat een nieuws antwoord bevat.  

```json
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Als u door de beschik bare artikelen wilt bladeren, gebruikt u de para meters voor [aantal](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#count) en [Offset](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#offset) query.  
 

|Parameter  |Description  |
|---------|---------|
|`count`     | Hiermee geeft u het aantal resultaten op dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten dat u in het antwoord kunt aanvragen is 100. De standaard waarde is 10. Het daad werkelijke aantal dat kan worden geleverd, kan kleiner zijn dan het aangevraagde nummer.        |
|`offset`     | Hiermee geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is op nul gebaseerd en moet kleiner zijn dan (`totalEstimatedMatches` - `count`).          |

Als u bijvoorbeeld 20 artikelen per pagina wilt weer geven, stelt `count` u in op 20 en `offset` 0 om de eerste pagina met resultaten op te halen. Voor elke volgende pagina verhoogt `offset` u de 20 (bijvoorbeeld 20, 40).  
  
In het volgende voor beeld wordt aan 20 nieuws artikelen gevraagd, te beginnen bij offset 40.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Als de standaard `count` waarde voor uw implementatie werkt, geeft u alleen `offset` de query parameter op, zoals weer gegeven in het volgende voor beeld:  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> Paging is alleen van toepassing op zoek opdrachten (/News/Search) en niet op/News/trendingtopics (trending topics) of News Categories (/News).

> [!NOTE]
> Het `TotalEstimatedAnswers` veld is een schatting van het totale aantal Zoek resultaten dat u voor de huidige query kunt ophalen.  Wanneer u en `count` `offset` para meters instelt `TotalEstimatedAnswers` , kan het aantal worden gewijzigd. 
