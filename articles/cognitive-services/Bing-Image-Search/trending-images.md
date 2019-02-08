---
title: Trending afbeeldingen met de Bing afbeeldingen zoeken-API ophalen
titleSuffix: Azure Cognitive Services
description: Zoeken naar trending afbeeldingen van vandaag van het web met de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: ea65fc9a6719b034a5f9d904ab2e970d61a2a7a2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880831"
---
# <a name="get-trending-images-from-the-web"></a>Ophalen van afbeeldingen-trending van het web

Voor trending afbeeldingen van vandaag, de volgende GET-aanvraag te verzenden:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

De Trending afbeeldingen-API ondersteunt momenteel alleen de volgende markten:  

- en-US (Engels, Verenigde Staten)  
- NL-CA (Engels, Canada)  
- en-AU (Engels, Australië)  
- zh-CN (Chinees, China)

Het antwoord bevat een [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#trendingimages) object met een lijst met installatiekopieën op categorie. Gebruik van de categorie `title` voor het groeperen van de afbeeldingen in uw gebruikerservaring. De categorieën kunnen dagelijks worden gewijzigd.  

```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  

Elke tegel bevat een afbeelding en opties voor het ophalen van gerelateerde afbeeldingen. Als u de gerelateerde afbeeldingen, kunt u de query `text` om aan te roepen de [afbeeldingen zoeken-API](./search-the-web.md) en de gerelateerde afbeeldingen zelf weer te geven. Of u kunt de URL in `webSearchUrl` te nemen van de gebruiker van de Bing afbeeldingen pagina met zoekresultaten, waarin de gerelateerde afbeeldingen.

Als u de afbeeldingen zoeken-API om op te halen van de gerelateerde afbeeldingen aanroepen, stelt u de [id](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#id) queryparameter met de ID in de `id` veld. De ID op te geven, zorgt u ervoor dat het antwoord bevat de installatiekopie (dit is de eerste afbeelding in het antwoord) en de bijbehorende gerelateerde afbeeldingen. Stel ook de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) queryparameter op de tekst in de `query` van het object `text` veld.

Het volgende voorbeeld ziet hoe u de afbeeldings-ID gebruikt om op te halen van gerelateerde afbeeldingen van Mr. Smith in de voorgaande Trending afbeeldingen-API-reactie.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
