---
title: Zoek op Internet voor trends video's | Microsoft Docs
description: Laat zien hoe de Bing Video zoeken-API gebruiken om te zoeken op het web voor trends video's.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 897A28A3-0980-484E-814F-FFE1D5C885E6
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8db7fcf77042631260b4b165bd3d44053827f3ce
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344419"
---
# <a name="get-trending-videos"></a>Trends video's ophalen  

Als u vandaag trends video's, de volgende GET-aanvraag te verzenden:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

De volgende markten ondersteuning trends video's.  
 
-   en-AU (Engels, Australië)  
-   NL-CA (Engels, Canada)  
-   en-GB (Engels, Engeland)  
-   NL-ID (Engels, Indonesië)  
-   NL-IE (Engels, Ierland)  
-   NL-IN (Engels, India)  
-   NL-NZ (Engels, Nieuw-Zeeland)  
-   NL-PH (Engels, Filipijnen)  
-   NL-SG (Engels, Singapore)  
-   en-US (Engels, Verenigde Staten)  
-   NL WW (Engels, wereldwijd statistische-code)  
-   NL-ZA (Engels, Zuid-Afrika)  
-   zh-CN (Chinees, China)

  
Het volgende voorbeeld ziet een antwoord met trends video's.  

```  
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```  
Het antwoord bevat een lijst met video's op categorie en subcategorie. Als de lijst met categorieën een categorie muziek video's bevat en een van de subcategorieën Top is, kan u een categorie boven muziek video's maken in de gebruikerservaring. U kunt vervolgens de `thumbnailUrl`, `displayText`, en `webSearchUrl` velden voor het maken van een klikbaar tegel onder elke categorie (bijvoorbeeld bovenste muziek video's). Wanneer de gebruiker op de tegel, bent ze naar Bing van video browser geleid waar de video wordt afgespeeld.

Het antwoord bevat ook banner video's, die de meest populaire trends video's. De banner video's kunnen afkomstig zijn van een of meer van de categorieën.  
  
