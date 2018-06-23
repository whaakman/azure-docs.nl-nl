---
title: Gebruik positie voor het weergeven van de web-antwoorden | Microsoft Docs
description: Laat zien hoe rangschikking gebruiken om de antwoorden die de Bing Web zoeken-API retourneert weer te geven.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 750146f3bb28b94594a71733b68f092880360c5a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345470"
---
# <a name="using-ranking-to-display-results"></a>Met behulp van rangschikking om resultaten weer te geven  

Elke search-antwoord bevat een [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse) antwoord, waarmee wordt aangegeven hoe de zoekresultaten moet worden weergegeven. Het antwoord ranking groepen resultaten door mainline inhoud en zijbalk inhoud voor een traditionele pagina met zoekresultaten. Als de resultaten in een traditionele hoofdlijnen en zijbalk indeling niet wordt weergegeven, moet u de hoofdlijnen inhoud hoger zichtbaarheid dan inhoud voor de zijbalk opgeven.  
  
In elke groep (mainline of zijbalk), wordt de [Items](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) matrix geeft de volgorde die de inhoud moet worden weergegeven in. Elk item biedt de volgende twee methoden voor het identificeren van het resultaat binnen een antwoord.  
  
-   `answerType` en `resultIndex` : de `answerType` veld identificeert het antwoord (bijvoorbeeld een webpagina of nieuws) en `resultIndex` identificeert een resultaat in het antwoord (bijvoorbeeld een nieuwsbericht). De index is een op nul gebaseerd.  
  
-   `value` : De `value` veld bevat een ID die overeenkomt met de ID van een antwoord of een resultaat in het antwoord. Het antwoord of de resultaten bevatten de ID, maar niet beide.  
  
Met behulp van de ID is eenvoudiger om te gebruiken omdat u alleen moet overeenkomen met de ID ranking met de ID van een antwoord of een van de resultaten. Als een object van het antwoord bevat een `id` veld, de resultaten van het antwoord tegelijk weergeven. Bijvoorbeeld, als de `News` -object bevat de `id` veld, alle nieuwsartikelen tegelijk weergeven. Als de `News` object bevat niet de `id` veld, wordt elk nieuwsartikel bevat een `id` veld en het antwoord ranking nieuwsartikelen combineert met de resultaten van andere antwoorden.  
  
Met behulp van de `answerType` en `resultIndex` is iets gecompliceerder. U gebruikt `answerType` voor het identificeren van het antwoord van de resultaten om weer te geven. Vervolgens gebruikt u `resultIndex` naar de index door de resultaten van het antwoord voor het resultaat om weer te geven. (De `answerType` waarde is de naam van het veld in de [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) object.) Als u verwacht om samen de resultaten van het antwoord weer te geven, het antwoord ranking item niet bevat de `resultIndex` veld.  

## <a name="ranking-response-example"></a>Ranking antwoord-voorbeeld

Hieronder vindt u een voorbeeld [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse). Omdat de Web-antwoord geen bevat een `id` veld, zou u alle webpagina's afzonderlijk op basis van de volgorde worden weergegeven (elke webpagina bevat een `id` veld). En omdat de afbeeldingen, video's en verwante zoekopdrachten antwoorden omvatten de `id` veld, zou u de resultaten van elk van de antwoorden samen op basis van de volgorde worden weergegeven.
  
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
    },  
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
  
Op basis van dit antwoord ranking, de hoofdlijnen zou de volgende zoekresultaten weergeven:  
  
-   Het eerste resultaat van de webpagina 
-   Alle installatiekopieën  
-   De resultaten van de tweede en derde webpagina  
-   De video 's  
-   De resultaten 4, 5 en 6 webpagina  
  
En de zijbalk de volgende zoekresultaten wilt weergeven:  
  
-   De verwante zoekopdrachten  
  

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het promoveren van geen positie resultaten [promoveren antwoorden die niet worden gerangschikt](./filter-answers.md#promoting-answers-that-are-not-ranked).

Zie voor meer informatie over het beperken van het aantal gerangschikte antwoorden in het antwoord [beperken het nummer van antwoorden in het antwoord](./filter-answers.md#limiting-the-number-of-answers-in-the-response).

Zie voor een C#-voorbeeld die gebruikmaakt van rangschikking om resultaten weer te geven, [C# ranking zelfstudie](./csharp-ranking-tutorial.md).