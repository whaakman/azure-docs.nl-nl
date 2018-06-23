---
title: Video-API van zoekservice snel aan de slag | Microsoft Docs
description: Laat zien hoe u aan de slag met de Bing-API voor het zoeken van Video.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7E59692A-83A8-4F4C-B122-1F0EDC8E5C86
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 0bd0f067d64cac3ebac342ebadcfcc010a47af7b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345462"
---
# <a name="your-first-video-search-query"></a>Uw eerste video query

Voordat u uw eerste aanroep aanbrengen kunt, moet u een abonnement cognitieve zoekservices Bing code. Als u een sleutel, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/?api=bing-video-search-api).

Als u Video zoekresultaten, zou u een GET-aanvraag verzenden naar het volgende eindpunt:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```
   
De aanvraag moet de HTTPS-protocol gebruiken.

We raden aan dat alle aanvragen zijn afkomstig uit een server. Het distribueren van de sleutel als onderdeel van een clienttoepassing biedt meer mogelijkheden om een schadelijke van derden om deze te openen. Het aanroepen van een server biedt ook een centraal punt voor de upgrade voor toekomstige versies van de API.

  
De aanvraag moet opgeven de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query) queryparameter waarin de gebruiker zoekterm. Hoewel dit optioneel is, geef de aanvraag ook de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#mkt) queryparameter waarmee de markt waar u de resultaten uit te komen. Query voor een lijst met optionele parameters zoals `pricing`, Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters). Alle parameterwaarden moet URL zijn gecodeerd.  
  
De aanvraag moet opgeven de [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#subscriptionkey) header. Hoewel dit optioneel, wordt u aangeraden ook de volgende headers opgeven:  
  
-   [Gebruikersagent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientid)  
-   [X-Search-client-IP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#clientip)  
-   [X-Search-locatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#location)  

De client-IP-adres en de locatie-headers zijn belangrijk voor het retourneren van locatie op de hoogte inhoud.  

Zie voor een lijst van alle aanvraag- en reactieheaders [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#headers).


## <a name="the-request"></a>De aanvraag

Hieronder ziet u een zoekaanvraag met alle voorgestelde queryparameters en -koppen. Als dit de eerste keer aanroepen van een van de Bing-API's, bevatten geen koptekst van de client-ID. Alleen de client-ID bevatten als u eerder een Bing-API hebt genoemd en Bing een client-ID voor de gebruiker en apparaat combinatie geretourneerd. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Hieronder ziet u het antwoord op de vorige aanvraag. Het voorbeeld ziet ook de Bing-specifieke antwoordheaders.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D5694...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYWCvh...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjHZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56944...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjBZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y6...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E11E3CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        . . .
    ],
    "nextOffset" : 0,
    "pivotSuggestions" : [
        {
            "pivot" : "sailing",
            "suggestions" : []
        },
        {
            "pivot" : "dinghies",
            "suggestions" : [
                {
                    "text" : "Sailing Cruising",
                    "displayText" : "Cruising",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF754...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Sailing..."
                    }
                },
                . . .
            ]
        }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

De API uitproberen. Ga naar [Video zoeken-API voor testdoeleinden Console](https://dev.cognitive.microsoft.com/docs/services/56b43f3ccf5ff8098cef3809/operations/58113fe5e31dac0a1ce6b0a8). 

Zie voor meer informatie over het gebruiken van de objecten antwoord [zoeken op het Web voor video's](./search-the-web.md).

Zie voor meer informatie over het verkrijgen van inzicht in een video zoals verwante zoekopdrachten [Video Insights](./video-insights.md).  
  
Zie voor meer informatie over de video's die zijn trends op sociale media [trends video's](./trending-videos.md).  
