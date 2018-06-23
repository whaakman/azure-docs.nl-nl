---
title: Installatiekopieën van Snel starten-API van zoekservice | Microsoft Docs
description: Laat zien hoe u aan de slag met de Bing-API voor het zoeken van afbeeldingen.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: BE2B2F8C-20B5-4E0B-AEC8-EAD505BA4C85
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 9e211cf5acd17ab80948d0b7161bdd2a9220c4a6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344487"
---
# <a name="your-first-images-search-query"></a>Uw eerste afbeeldingen zoeken query

Voordat u uw eerste aanroep aanbrengen kunt, moet u een abonnement cognitieve zoekservices Bing code. Als u een sleutel, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api).

Als u de installatiekopie zoekresultaten, zou u een GET-aanvraag verzenden naar het volgende eindpunt:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search
```
  
De aanvraag moet de HTTPS-protocol gebruiken.

We raden aan dat alle aanvragen zijn afkomstig uit een server. Het distribueren van de sleutel als onderdeel van een clienttoepassing biedt meer mogelijkheden om een schadelijke van derden om deze te openen. Het aanroepen van een server biedt ook een centraal punt voor de upgrade voor toekomstige versies van de API.

De aanvraag moet opgeven de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) queryparameter waarin de gebruiker zoekterm. Hoewel dit optioneel is, geef de aanvraag ook de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#mkt) queryparameter waarmee de markt waar u de resultaten uit te komen. Query voor een lijst met optionele parameters zoals `freshness` en `size`, Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query-parameters). Alle parameterwaarden moet URL zijn gecodeerd.  
  
De aanvraag moet opgeven de [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#subscriptionkey) header. Hoewel dit optioneel, wordt u aangeraden ook de volgende headers opgeven:  
  
-   [Gebruikersagent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#clientid)  
-   [X-Search-client-IP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#clientip)  
-   [X-Search-locatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#location)  

De client-IP-adres en de locatie-headers zijn belangrijk voor het retourneren van locatie op de hoogte inhoud.  

Zie voor een lijst van alle aanvraag- en reactieheaders [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#headers).

## <a name="the-request"></a>De aanvraag

Hieronder ziet u een zoekaanvraag met alle voorgestelde queryparameters en -koppen. Als dit is de eerste keer aanroepen van een van de Bing-API's, niet opgenomen in de koptekst van de client-ID. Alleen de client-ID bevatten als u eerder een Bing-API hebt genoemd en Bing een client-ID voor de gebruiker en apparaat combinatie geretourneerd. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Hieronder ziet u het antwoord op de vorige aanvraag.

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "totalEstimatedMatches" : 838,
    "value" : [
        {
            "name" : "File:Rich Passage Minto Sailing Dinghy.jpg - Wikipedia",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
            "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
            "datePublished" : "2011-10-29T11:26:00",
            "contentUrl" : "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
            "hostPageUrl" : "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
            "contentSize" : "79239 B",
            "encodingFormat" : "jpeg",
            "hostPageDisplayUrl" : "en.wikipedia.org\/wiki\/File:Rich_Passage...",
            "width" : 526,
            "height" : 688,
            "thumbnail" : {
                "width" : 229,
                "height" : 300
            },
            "imageInsightsToken" : "ccid_GNarK7ma*mid_CCF85447ADA6...",
            "insightsSourcesSummary" : {
                "shoppingSourcesCount" : 0,
                "recipeSourcesCount" : 0
            },
            "imageId" : "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
            "accentColor" : "376094"
        },
        . . .
    ],
    "queryExpansions" : [
        {
            "text" : "Small Sailing Dinghies",
            "displayText" : "Small",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4...",
            "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail" : {
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=..."
            }
        },
        . . .
    ],
    "nextOffset" : 10,
    "pivotSuggestions" : [
        {
            "pivot" : "sailing",
            "suggestions" : []
        },
        {
            "pivot" : "dinghies",
            "suggestions" : [
                {
                    "text" : "Sailing Tender",
                    "displayText" : "Tender",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=..."
                    }
                },
                . . .
            ]
        }
    ],
    "displayShoppingSourcesBadges" : false,
    "displayRecipeSourcesBadges" : true
}
```

## <a name="next-steps"></a>Volgende stappen

De API uitproberen. Ga naar [installatiekopie zoeken-API voor testdoeleinden Console](https://dev.cognitive.microsoft.com/docs/services/8336afba49a84475ba401758c0dbf749/operations/571fab09dbe2d933e891028f). 

Zie voor meer informatie over het gebruiken van de objecten antwoord [zoeken op het Web](./search-the-web.md).

Zie voor meer informatie over het verkrijgen van inzicht in een afbeelding zoals webpagina's die de installatiekopie of personen die zijn herkend opnemen in de afbeelding [installatiekopie Insights](./image-insights.md).  
  
Zie voor meer informatie over de installatiekopieën die zijn trends op sociale media [trends installatiekopieën](./trending-images.md).  
