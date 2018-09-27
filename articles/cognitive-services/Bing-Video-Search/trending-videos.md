---
title: Het web zoeken naar trending video's - Bing video's zoeken
titlesuffix: Azure Cognitive Services
description: Laat zien hoe de Bing video's zoeken-API gebruiken om te zoeken naar het web voor trending video's.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8a6ccc9ea8cf9468d7638360c9db8131bc6dc5be
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222039"
---
# <a name="get-trending-videos"></a>Trending video's ophalen  

Om op te halen van vandaag trending video's, de volgende GET-aanvraag te verzenden:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

De volgende markten ondersteuning voor trending video's.  
 
-   en-AU (Engels, Australië)  
-   NL-CA (Engels, Canada)  
-   en-GB (Engels, Groot-Brittannië)  
-   NL-ID (Engels, Indonesië)  
-   NL-Internet Explorer (Engels, Ierland)  
-   NL-IN-(Engels, India)  
-   NL-NZ (Engels, Nieuw-Zeeland)  
-   NL-PH (Engels, Filipijnen)  
-   en-Lidmaatschappen (Engels, Singapore)  
-   en-US (Engels, Verenigde Staten)  
-   NL-WW (Engels, wereldwijd statistische-code)  
-   NL-ZA-(Engels, Zuid-Afrika)  
-   zh-CN (Chinees, China)

  
Het volgende voorbeeld ziet een antwoord met trending video's.  

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
Het antwoord bevat een lijst met video's op categorie en subcategorie. Als de lijst met categorieën die een categorie muziek video's en een van de subcategorieën boven is, kan u bijvoorbeeld een categorie boven muziek-video's maken in uw gebruikerservaring. U kunt vervolgens de `thumbnailUrl`, `displayText`, en `webSearchUrl` velden op de tegel met een geklikt in elke categorie (bijvoorbeeld bovenste muziek video's) maken. Wanneer de gebruiker op de tegel, er ze wordt van de Bing video-browser waar de video wordt afgespeeld.

Het antwoord bevat ook een banner video's, die het meest populaire trending video's. De banner video's kunnen afkomstig zijn van een of meer van de categorieën.  
  
