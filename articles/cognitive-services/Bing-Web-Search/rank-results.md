---
title: 'Het gebruik van classificaties om weer te geven van zoekresultaten: Bing webzoekopdrachten-API'
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van de volgorde om weer te geven resultaten van de Bing webzoekopdrachten-API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/17/2019
ms.author: scottwhi
ms.openlocfilehash: 9a49c4af474d7f0618bf0cff1a093e5cbb62fe2d
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295603"
---
# <a name="how-to-use-ranking-to-display-bing-web-search-api-results"></a>Het gebruik van trefwoordenrangschikking om weer te geven resultaten van de Bing webzoekopdrachten-API  

Elke zoekopdracht-antwoord bevat een [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) antwoord, waarmee wordt aangegeven hoe u de lijst met zoekresultaten moet weergeven. Het antwoord rangorde groepen resultaten door mainline inhoud en zijbalk inhoud voor een traditionele pagina met zoekresultaten. Als de resultaten in een traditionele hoofdlijnen en de indeling van de zijbalk niet wordt weergegeven, moet u de hoofdlijnen inhoud hoger zichtbaarheid dan de zijbalk-inhoud opgeven.  

Binnen elke groep (mainline of zijbalk), wordt de [Items](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) matrix identificeert de volgorde waarin de inhoud moet worden weergegeven in. Elk item biedt de volgende twee manieren om te identificeren van het resultaat binnen een antwoord.  

-   `answerType` en `resultIndex` : de `answerType` veld identificeert het antwoord (bijvoorbeeld een webpagina of nieuws) en `resultIndex` geeft een resultaat in het antwoord (bijvoorbeeld een nieuwsbericht). De index is op basis van nul.  

-   `value` : De `value` veld bevat een ID die overeenkomt met de ID van een antwoord of een resultaat in het antwoord. Het antwoord of de resultaten bevatten de ID, maar niet beide.  

Met behulp van de ID is eenvoudiger om te gebruiken omdat u alleen moet overeenkomen met de volgorde-ID met de ID van een antwoord of een van de resultaten. Als een object van het antwoord bevat een `id` veld, van het antwoord resultaten tegelijk weergeven. Bijvoorbeeld, als de `News` object bevat de `id` veld, alle nieuwsartikelen tegelijk weergeven. Als de `News` object omvat niet de `id` veld, wordt elk nieuwsartikel bevat een `id` veld en de reactie rangschikking van de nieuwsartikelen combineert met de resultaten van andere antwoorden.  

Met behulp van de `answerType` en `resultIndex` is iets gecompliceerder. U gebruikt `answerType` voor het identificeren van het antwoord met de resultaten om weer te geven. Vervolgens gebruikt u `resultIndex` index via de resultaten van het antwoord voor het resultaat om weer te geven. (De `answerType` waarde is de naam van het veld in de [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) object.) Als u verwacht om samen de resultaten van het antwoord weer te geven, het antwoord rangorde item niet bevatten de `resultIndex` veld.  

## <a name="ranking-response-example"></a>Voorbeeld van de reactie rangorde

Hieronder ziet u een voorbeeld [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse). Omdat de Web-antwoord bevat geen een `id` veld, zou u alle webpagina's afzonderlijk op basis van de volgorde weergeven (elke webpagina bevat een `id` veld). En omdat de afbeeldingen, video's en verwante zoekopdrachten antwoorden omvatten de `id` veld, zou u de resultaten van elk van de antwoorden samen op basis van de volgorde weergeven.

```json
{  
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
        "totalEstimatedMatches" : 835000,
        "value" : [
            {
                "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
                "name" : "Motor Sports - Live at the race track ...",
                "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                "displayUrl" : "www.contoso.com\/usa\/eventsandracing\/motorsport",
                "snippet" : "Here you will find detailed information about racing...",
                "deepLinks" : [{
                    "name" : "Customer Racing",
                    "url" : "http:\/\/www.bing.com\/cr?IG=96C4CF214A0A43...",
                    "snippet" : "Customer racing news; General news..."
            },
            . . .  
        ]  
    }],  
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "2016 Supercar Wallpapers",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP...",
                "datePublished" : "2017-03-25T11:14:00",
                "contentUrl" : "http:\/\/www.contoso.com\/wall...",
                "hostPageUrl" : "http:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "contentSize" : "373283 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "http:\/\/www.contoso.com\/lmp-...",
                "width" : 1920,
                "height" : 1080,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "insightsSourcesSummary" : {
                    "shoppingSourcesCount" : 0,
                    "recipeSourcesCount" : 0
                }
            },
            . . .  
        ]  
    },  
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches",
        "value" : [
            {
                "text" : "vintage racing teams",
                "displayText" : "vintage racing teams",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2..."
            },
            . . .  
        ]  
    },  
    "videos" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/videos...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214A...",
        "isFamilyFriendly" : true,
        "value" : [
            {
                "name" : "Why We Race",
                "description" : "A new era begins in motorsports this weekend...",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF2...",
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.Vo1...",
                "datePublished" : "2014-01-25T16:31:48",
                "publisher" : [
                    {
                        "name" : "Fabrikam"
                    }
                ],
                "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=oL...",
                "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=96C4CF214...",
                "encodingFormat" : "mp4",
                "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=oLAZgD...",
                "width" : 480,
                "height" : 360,
                "duration" : "PT2M42S",
                "motionThumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OM...",
                "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www.you...<\/iframe>",
                "allowHttpsEmbed" : true,
                "viewCount" : 47325,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 168
                },
                "allowMobileEmbed" : true,
                "isSuperfresh" : false
            },
            . . .  
        ]  
    },  
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 2,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.2"
                }
            },
            {
                "answerType" : "Videos",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Videos"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 3,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.3"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 4,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.4"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 5,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.5"
                }
            }]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}  
```  

Op basis van deze rangschikking antwoord, de hoofdlijnen zou de volgende zoekresultaten weergeven:  

-   Het eerste webpagina-resultaat
-   Alle installatiekopieën  
-   De resultaten van de tweede en derde webpagina  
-   Alle video 's  
-   De resultaten 4, 5 en 6 webpagina  

En de zijbalk zou de volgende lijst met zoekresultaten weergeven:  

-   Alle verwante zoekopdrachten  


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het promoveren van geen positie resultaten [bevordering van de antwoorden die niet worden gerangschikt](./filter-answers.md#promoting-answers-that-are-not-ranked).

Zie voor meer informatie over het beperken van het aantal gerangschikte antwoorden in het antwoord [beperkt het aantal antwoorden in het antwoord](./filter-answers.md#limiting-the-number-of-answers-in-the-response).

Zie voor een voorbeeld met C# die gebruikmaakt van trefwoordenrangschikking om resultaten weer te geven, [C# rangorde zelfstudie](./csharp-ranking-tutorial.md).
