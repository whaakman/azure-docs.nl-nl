---
title: Pagina met resultaten voor zoeken in Bing nieuws
titlesuffix: Azure Cognitive Services
description: Leer hoe u door de nieuwsartikelen die de Bing nieuws zoeken-API retourneert.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: 1d344f388b03acb3a81fcfde0e214eb7d82dc9b9
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885069"
---
# <a name="how-to-page-through-news-search-results"></a>Pagina met zoekresultaten voor nieuws

Wanneer u de nieuws zoeken-API aanroept, retourneert Bing een lijst met resultaten die relevant voor uw query zijn. Als u de geschatte totale aantal beschikbare resultaten, toegang krijgen tot de antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) veld.  
  
Het volgende voorbeeld wordt de `totalEstimatedMatches` veld dat een nieuwsantwoord bevat.  

```json
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Als u wilt de beschikbare artikelen doorlopen, gebruikt u de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) queryparameters.  
 

|Parameter  |Description  |
|---------|---------|
|`count`     | Hiermee geeft u het aantal resultaten dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten die u in het antwoord aanvragen kunt is 100. De standaardwaarde is 10. Het werkelijke getal geleverd mogelijk lager dan aangevraagd.        |
|`offset`     | Hiermee geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is gebaseerd op nul en moet kleiner dan (`totalEstimatedMatches` - `count`).          |

Bijvoorbeeld, als u weergeven van 20 artikelen per pagina wilt, stelt u `count` tot en met 20 en `offset` op 0 om de eerste pagina van de resultaten. Voor elke volgende pagina opgehoogd `offset` door 20 (bijvoorbeeld 20, 40).  
  
Het volgende voorbeeld vraagt om 20 nieuwsartikelen beginnen bij verschuiving 40.  

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

> [!NOTE]
> De `TotalEstimatedAnswers` veld is een schatting van het totale aantal zoekresultaten kunt u voor de huidige query ophalen.  Als u instelt `count` en `offset` parameters, de `TotalEstimatedAnswers` getal kan worden gewijzigd. 
