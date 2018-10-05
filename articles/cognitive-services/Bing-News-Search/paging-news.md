---
title: Het bladeren door de beschikbare nieuwsartikelen - zoeken in Bing nieuws
titlesuffix: Azure Cognitive Services
description: Laat zien hoe u een pagina via al het nieuwsartikelen die nieuwe zoeken in Bing kunnen retourneren.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: fff1da15df2e690cd0b37bb82654a4d30159325a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803389"
---
# <a name="paging-news"></a>Wisselbestand-nieuws

Wanneer u de nieuws zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die mogelijk relevant zijn voor de query. Als u de geschatte totale aantal beschikbare resultaten, toegang krijgen tot de antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) veld.  
  
Het volgende voorbeeld wordt de `totalEstimatedMatches` veld dat een nieuwsantwoord bevat.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Als u wilt de beschikbare artikelen doorlopen, gebruikt u de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) queryparameters.  
  
De `count` parameter geeft u het aantal resultaten dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten die u in het antwoord aanvragen kunt is 100. De standaardwaarde is 10. Het werkelijke getal geleverd mogelijk lager dan aangevraagd.

De `offset` parameter geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is gebaseerd op nul en moet kleiner dan (`totalEstimatedMatches` - `count`).  

Als u weergeven van 20 artikelen per pagina wilt, stelt u `count` tot en met 20 en `offset` op 0 om de eerste pagina van de resultaten. Voor elke volgende pagina opgehoogd `offset` door 20 (bijvoorbeeld 20, 40).  
  
Hieronder ziet u een voorbeeld waarin 20 nieuwsartikelen beginnen bij verschuiving 40 aanvragen.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Als de standaard `count` waarde werkt voor uw implementatie, geeft u alleen de `offset` parameter opvragen, zoals weergegeven in het volgende voorbeeld:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> Wisselgeheugengebruik geldt alleen voor nieuws zoeken (/ nieuws/search), en niet voor actuele onderwerpen op (/ nieuws/trendingtopics) of nieuwscategorieën (/ nieuws).