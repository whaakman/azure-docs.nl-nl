---
title: Filteren van de web-antwoorden die Bing retourneert | Microsoft Docs
description: Laat zien hoe responseFilter gebruiken voor het filteren van de antwoorden die de Bing Web zoeken-API retourneert.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/12/2017
ms.author: scottwhi
ms.openlocfilehash: a5ee6241630ee24a05c2c4b932453bd7946a7508
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344913"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>De antwoorden die de search-antwoord bevat filteren  

Als u query's voor het web, retourneert Bing alle inhoud die deze denkt dat relevant is voor de zoekopdracht. Als de query 'varen + dinghies' is, kan het antwoord bijvoorbeeld de volgende antwoorden bevatten:

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

Als u geïnteresseerd in specifieke typen inhoud, zoals afbeeldingen en video's Nieuws bent, kunt u alleen de antwoorden vragen met behulp van de [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter) queryparameter. Als de relevante inhoud Bing gevonden voor de opgegeven antwoorden, wordt het door de Bing retourneert. Het antwoordfilter is een door komma's gescheiden lijst met antwoorden. Hieronder vindt u het gebruik van `responseFilter` aanvraag afbeeldingen, video's en nieuws van Zeilsloepen. Wanneer u de queryreeks coderen, wordt de komma's wijzigen in %2, C.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Hieronder ziet u het antwoord op de vorige query. Zoals u ziet gevonden Bing relevante resultaten van video en nieuws, zodat het antwoord niet opnemen.

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

Hoewel Bing heeft geen video en nieuws resultaten in het vorige antwoord geretourneerd, betekent niet dat inhoud video en nieuws niet bestaat. Dit betekent dat de pagina niet opnemen. Echter, als u [pagina](./paging-webpages.md) via meer resultaten de volgende pagina's waarschijnlijk omvat ze. Ook als u de [Video-API van zoekservice](../bing-video-search/search-the-web.md) en [nieuws Search API](../bing-news-search/search-the-web.md) eindpunten rechtstreeks, het antwoord zou waarschijnlijk resultaten bevatten. 

U staat op het gebruik maken van `responseFilter` resultaten van één API ophalen. Als u wilt de inhoud van één Bing-API, rechtstreeks die API aanroepen. Bijvoorbeeld: voor het ontvangen van alleen afbeeldingen, een aanvraag verzenden naar het eindpunt installatiekopie Search API `https://api.cognitive.microsoft.com/bing/v7.0/images/search` of een van de andere [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#endpoints) eindpunten. Het aanroepen van één API is niet alleen belangrijk voor prestaties, maar omdat de inhoud-specifieke API's rijkere resultaten bieden. Bijvoorbeeld, kunt u filters die zijn niet beschikbaar voor de Web-API voor zoeken om de resultaten te filteren.  
  
Als u de zoekresultaten van een bepaald domein, omvatten de `site:` queryoperator in de queryreeks.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE] 
> Afhankelijk van de query, als u de `site:` queryoperator, bestaat de kans dat het antwoord inhoud voor volwassenen ongeacht bevatten mag de [veilig zoeken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#safesearch) instelling. U moet gebruiken `site:` alleen als u zich bewust bent van de inhoud op de site en de mogelijkheid van inhoud voor volwassenen ondersteuning biedt voor uw scenario. 
  
## <a name="limiting-the-number-of-answers-in-the-response"></a>Het aantal antwoorden in het antwoord beperken

Bing bevat antwoorden in het antwoord op basis van de positie. Bijvoorbeeld, als u een query *varen + dinghies*, Bing retourneert `webpages`, `images`, `videos`, en `relatedSearches`.

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

Het maximum van antwoorden die Bing retourneert aan de antwoorden van de twee bovenste (webpagina's en afbeeldingen), de [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) queryparameter naar 2. 

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

Als u de `responseFilter` queryparameter op de vorige query en deze webpagina's en nieuws, het antwoord bevat alleen webpagina's omdat nieuws niet is gerangschikt.

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

## <a name="promoting-answers-that-are-not-ranked"></a>De antwoorden die niet worden gerangschikt promoveren

Als de bovenste gerangschikt antwoorden die Bing voor een query retourneert webpagina's, afbeeldingen, video's en relatedSearches zijn, zou de antwoorden opnemen in het antwoord. Als u instelt [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) naar twee (2), Bing, retourneert de bovenste twee gerangschikte antwoorden: webpagina's en afbeeldingen. Als u Bing afbeeldingen en video's opnemen in het antwoord wilt, geef de [promoveren](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) queryparameter en wordt ingesteld op afbeeldingen en video's. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Hier volgt het antwoord op de bovenstaande aanvraag. Bing retourneert de bovenste twee antwoorden, webpagina's en afbeeldingen en video's bevordert in het antwoord.

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

Als u instelt `promote` nieuws, het antwoord bevat geen het nieuwsantwoord omdat deze niet een gerangschikte antwoord&mdash;kunt u alleen antwoorden gerangschikt promoveren.

De antwoorden die u wilt promoveren niet meegeteld in het `answerCount` limiet. Bijvoorbeeld, als de gerangschikte antwoorden zijn nieuws, afbeeldingen en video's en u `answerCount` op 1 en `promote` naar nieuws, het antwoord bevat nieuws en afbeeldingen. Of als de gerangschikte antwoorden video's, installatiekopieën en nieuws zijn, het antwoord bevat video's en nieuws.

U kunt `promote` alleen als u opgeeft de `answerCount` queryparameter.
