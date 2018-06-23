---
title: Zoek op Internet voor installatiekopieën van trends | Microsoft Docs
description: Laat zien hoe de Bing installatiekopieën zoeken-API gebruiken om te zoeken op het web voor installatiekopieën van trends.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: b12524cd4c1896501820209b3a45746b8f38b210
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344438"
---
# <a name="get-trending-images"></a>Installatiekopieën van trends ophalen  

Als u vandaag trends afbeeldingen, verzenden de volgende GET-aanvraag:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

De afbeeldingen trends API ondersteunt momenteel alleen de volgende markten:  

- en-US (Engels, Verenigde Staten)  
- NL-CA (Engels, Canada)  
- en-AU (Engels, Australië)  
- zh-CN (Chinees, China)

Het antwoord bevat een [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#trendingimages) object met een lijst met afbeeldingen per categorie. Gebruik de categorie `title` voor het groeperen van de afbeeldingen in uw gebruikerservaring. De categorieën kunnen dagelijks worden gewijzigd.  
  
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
  
Elke tegel bevat een afbeelding en opties voor het ophalen van afbeeldingen. Als u de gerelateerde afbeeldingen, kunt u de query `text` aan te roepen de [installatiekopie Search API](./search-the-web.md) en weergeven van de gerelateerde afbeeldingen zelf. Of u kunt de URL in `webSearchUrl` moet worden uitgevoerd door de gebruiker van Bing installatiekopieën pagina met zoekresultaten, die de installatiekopieën van het gerelateerde bevat. 

Als u de installatiekopie Search API om op te halen van de installatiekopieën van het gerelateerde aanroepen, stelt u de [id](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#id) queryparameter naar de ID in de `id` veld. De ID opgeven, zorgt u ervoor dat het antwoord bevat de installatiekopie (dit is de eerste afbeelding in het antwoord) en de bijbehorende verwante afbeeldingen. Stel ook de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#q) queryparameter op de tekst in de `query` van het object `text` veld.

Het volgende voorbeeld laat zien hoe de afbeeldings-ID gebruiken voor afbeeldingen van Dhr. Smith in het voorgaande trends installatiekopieën API-antwoord.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
