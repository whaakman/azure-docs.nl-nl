---
title: De web-antwoorden die Bing retourneert filteren | Microsoft Docs
description: Laat zien hoe responseFilter gebruiken voor het filteren van de antwoorden die de Bing webzoekopdrachten-API retourneert.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/12/2017
ms.author: scottwhi
ms.openlocfilehash: 64095089e4c0841aa1f77165969221836c747738
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888570"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>De antwoorden die antwoord van de zoekactie bevat filteren  

Wanneer u het web zoeken, retourneert Bing alle inhoud die door deze als relevant zijn voor de zoekopdracht. Als de zoekopdracht is 'varen + dinghies', kan het antwoord, bijvoorbeeld de volgende antwoorden bevatten:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43C...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    },
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA5CA6464E5D...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [...]
        }
    }
}    
```

Als u geïnteresseerd in specifieke typen inhoud zoals afbeeldingen, video's en nieuws bent, kunt u de antwoorden op vragen met behulp van de [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter) queryparameter. Als Bing relevante inhoud voor de opgegeven antwoorden vindt, wordt het door Bing retourneert. Het antwoordfilter is een door komma's gescheiden lijst van antwoorden. Het volgende laat zien hoe u `responseFilter` op verzoek-afbeeldingen, video's en nieuws van Zeilsloepen. Wanneer u de queryreeks coderen, wordt de komma's wijzigen in %2, C.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Hieronder ziet u het antwoord op de vorige query. Zoals u ziet vinden Bing relevante resultaten voor video's en nieuws, zodat het antwoord niet opnemen.

```json
{
    "_type" : "SearchResponse",
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3AD78B183C56456C...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            }]
        }
    }
}
```

Als u uitsluiten van specifieke typen inhoud, zoals afbeeldingen, van het antwoord wilt, kunt u ze kunt uitsluiten met het afbreekstreepje (min)-voorvoegsel op in de responseFilter-waarde. Afzonderlijke uitgesloten typen met een door komma's: 

```
&responseFilter=-images,-videos
```

Hoewel Bing heeft geen video's en nieuws resultaten geretourneerd in het vorige antwoord, betekent dit niet dat inhoud voor video's en nieuws niet bestaat. Het gewoon betekent dat de pagina niet opnemen. Echter, als u [pagina](./paging-webpages.md) door meer resultaten, de volgende pagina's wilt waarschijnlijk opnemen. Ook als u de [video's zoeken-API](../bing-video-search/search-the-web.md) en [nieuws zoeken-API](../bing-news-search/search-the-web.md) eindpunten direct, het antwoord zou waarschijnlijk resultaten bevatten. 

Wordt afgeraden via `responseFilter` resultaten ophalen uit een enkele API. Als u wilt dat inhoud van een enkele API voor Bing, rechtstreeks die API aanroepen. Bijvoorbeeld, als u wilt ontvangen alleen afbeeldingen, een aanvraag verzenden naar het eindpunt van de afbeeldingen zoeken-API `https://api.cognitive.microsoft.com/bing/v7.0/images/search` of een van de andere [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#endpoints) eindpunten. Het aanroepen van één API is niet alleen belangrijk voor prestaties, maar omdat de inhoud-specifieke API's uitgebreidere resultaten bieden. Bijvoorbeeld, kunt u filters die zijn niet beschikbaar voor de webzoekopdrachten-API om de resultaten te filteren.  
  
Als u zoekresultaten vanuit een specifiek domein, zijn de `site:` query-operator in de querytekenreeks.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE] 
> Afhankelijk van de query, als u de `site:` query-operator, bestaat de kans dat het antwoord inhoud voor volwassen ongeacht bevatten de [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#safesearch) instelling. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en uw scenario de mogelijkheid van inhoud voor volwassenen ondersteunt. 
  
## <a name="limiting-the-number-of-answers-in-the-response"></a>Het aantal antwoorden in de respons beperken

Bing bevat antwoorden in de reactie op basis van positie. Bijvoorbeeld, als u een query *varen + dinghies*, Bing retourneert `webpages`, `images`, `videos`, en `relatedSearches`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "relatedSearches" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Het aantal antwoorden te beperken die Bing retourneert op de twee bovenste antwoorden (webpagina's en afbeeldingen), de [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) queryparameter naar 2. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Het antwoord bevat alleen `webPages` en `images`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "rankingResponse" : {...}
}
```

Als u de `responseFilter` queryparameter op de vorige query en deze webpagina's en nieuws, het antwoord bevat alleen webpagina's omdat nieuws wordt niet beoordeeld.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "rankingResponse" : {...}
}
```

## <a name="promoting-answers-that-are-not-ranked"></a>Bevordering van de antwoorden die niet worden gerangschikt

Als de antwoorden die Bing voor een query retourneert hebt beoordeeld boven webpagina's, afbeeldingen, video's en relatedSearches, kan het antwoord zou deze antwoorden bevatten. Als u instelt [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) aan twee (2), Bing, retourneert de eerste twee gerangschikte antwoorden: webpagina's en afbeeldingen. Als u wilt dat Bing afbeeldingen en video's opnemen in het antwoord, geeft u de [bevorderen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) queryparameter en stel deze in op de afbeeldingen en video's. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Hier volgt het antwoord op de bovenstaande aanvraag. Bing retourneert de eerste twee antwoorden, webpagina's en afbeeldingen en video's in het antwoord te verhogen.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailiing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Als u instelt `promote` nieuws, het antwoord bevat geen het nieuwsantwoord omdat het is niet een gerangschikte antwoord&mdash;kunt u alleen gerangschikt antwoorden promoveren.

De antwoorden die u wilt promoveren niet meegeteld in het `answerCount` limiet. Als de gerangschikte antwoorden zijn nieuws, afbeeldingen en video's en u stelt bijvoorbeeld `answerCount` op 1 en `promote` nieuws, het antwoord bevat nieuws en afbeeldingen. Of, als de gerangschikte antwoorden video's, afbeeldingen en nieuws zijn, het antwoord bevat video's en nieuws.

U kunt `promote` alleen als u opgeeft de `answerCount` queryparameter.
