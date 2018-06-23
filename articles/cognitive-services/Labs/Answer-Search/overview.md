---
title: Antwoord zoeken overzicht - cognitieve Services van Microsoft Project | Microsoft Docs
description: Inleiding tot het Project antwoord zoeken.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: d87cf1390970d2c815b94bcaee7e07c19bc03cce
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345319"
---
# <a name="what-is-project-answer-search"></a>Wat is Project antwoord zoeken?
Project antwoord Zoek-API maakt gebruik van het eindpunt van Bing v7 antwoorden op interrogative query's. Een vraag, zoals 'Wat is de omtrek van de aarde?' retourneert een antwoord met feitelijke gegevens.  Een query voor een persoon, plaats of ding retourneert informatie over de entiteit geïdentificeerd door de query. Deze scenario's kunnen nuttig zijn in toepassingen zoals eigen bots, messaging-apps, lezers, enzovoort.  

Query's retourneren antwoorden die afhankelijk van de query-scenario zijn: webpagina's worden altijd geretourneerd, terwijl [feiten](fact-queries.md) en/of [entiteiten](entity-queries.md) als relevant worden geretourneerd.

## <a name="endpoint"></a>Eindpunt
Als u antwoorden op een vraag of de informatie over een persoon, plaats of ding, een aanvraag te verzenden naar het antwoord Search API-eindpunt. Gebruik de kopteksten en de URL-parameters voor verschillende specificaties.  Omvatten *Ocp-Apim-Subscription-Key* -koptekst met een geldig token.  De markt-parameter is vereist. Alleen `en-us` markt wordt momenteel ondersteund.

De volgende query haalt u antwoorden op de vraag: 'Wat is de omtrek van de aarde?'

GET:
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=what+is+circumference+of+the=earth?&mkt=en-us

````

De URL-parameter `q=` is vereist voor het object van de zoekactie opgeven.

## <a name="response-object"></a>Response-object

Het antwoord bevat de HTTP-headers, webpagina's, feiten en/of entiteiten.

````
BingAPIs-TraceId: AB2E75C998614ADB8EBF5110DF648298
X-MSEdge-ClientID: 1E48FC4F7B8768C80B14F7997A106906
BingAPIs-SessionId: 0504DDD6DAE84861A4842306F8DA7A58
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: AB2E75C998614ADB8EBF5110DF648298 Ref B: CO1EDGE0322 Ref C: 2018-04-19T19:57:13Z

JSON Response:

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "what is the circumference of earth"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q\u003dwhat+is+the+circumference+of+earth",
    "totalEstimatedMatches": 217000,
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.0",
        "name": "Circumference of the Earth - Universe Today",
        "url": "https://www.universetoday.com/26461/circumference-of-the-earth/",
        "isFamilyFriendly": true,
        "displayUrl": "https://www.universetoday.com/26461/circumference-of-the-earth",
        "snippet": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "deepLinks": [
          {
            "name": "About Earth",
            "url": "https://www.universetoday.com/14382/10-interesting-facts-about-planet-earth/"
          }
        ],
        "dateLastCrawled": "2018-04-12T14:13:00.0000000Z",
        "language": "en"
      },
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.1",
        "name": "Earth - Wikipedia",
        "url": "https://en.wikipedia.org/wiki/Earth",
        "about": [
          {
            "name": "Earth"
          },
          {
            "name": "Earth"
          }
        ],
        "isFamilyFriendly": true,
        "displayUrl": "https://en.wikipedia.org/wiki/Earth",
        "snippet": "Circumference: 40 075.017 km equatorial (24 901.461 mi) ... Earth is the third planet from the Sun and the only object in the Universe known to harbor life.",
        "deepLinks": [
          {
            "name": "Moon",
            "url": "https://en.wikipedia.org/wiki/Moon"
          },
          {
            "name": "Planet",
            "url": "https://en.wikipedia.org/wiki/Planet"
          },
          {
            "name": "Quasi-Satellites",
            "url": "https://en.wikipedia.org/wiki/Quasi-satellite"
          },
          {
            "name": "World Population",
            "url": "https://en.wikipedia.org/wiki/World_population"
          },
   . . .

    ]
  },
  "entities": {
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#Entities.0",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
              "name": "CC-BY-SA",
              "url": "http://creativecommons.org/licenses/by-sa/3.0/"
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "http://en.wikipedia.org/wiki/Earth"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Earth"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dEarth\u0026filters\u003dsid:%226ddb3372-4801-5567-321e-e8a53bd774a4%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Earth",
        "image": {
          "name": "Earth",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA3ab623665ab412f386c162bd29f0683a\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Earth"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/commons/9/97/The_Earth_seen_from_Apollo_17.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 799,
          "sourceHeight": 800
        },
        "description": "Earth is the third planet from the Sun and the only object in the Universe known to harbor life. According to radiometric dating and other sources of evidence, Earth formed over 4.5 billion years ago. Earth\u0027s gravity interacts with other objects in space, especially the Sun and the Moon, Earth\u0027s only natural satellite. Earth revolves around the Sun in 365.26 days, a period known as an Earth year. During this time, Earth rotates about its axis about 366.26 times.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Generic"
          ]
        },
        "bingId": "6ddb3372-4801-5567-321e-e8a53bd774a4"
      }
    ]
  },
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.universetoday.com/26461/circumference-of-the-earth/",
        "url": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.universetoday.com/26461/circumference-of-the-earth/",
        "seeMoreUrl": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "value": [
      {
        "description": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "subjectName": ""
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "Facts",
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Facts"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.0"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.1"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 2,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.2"
          }
        },
        
        . . . 
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        }
      ]
    }
  }
}

````

## <a name="terms-of-use"></a>Gebruiksrechtovereenkomst
Project antwoord zoeken en Project Video Trends zijn onderworpen aan de [Bing zoeken gebruiken en de vereisten van de weergave](use-display-requirements.md).

U of een derde partij namens u mogelijk niet gebruiken, behouden, opslaan, cache, delen, of distribueren van alle gegevens van de API van de Preview-URL voor de toepassing testen, ontwikkelen, training, distribueren of het beschikbaar maken van een niet-Microsoft-service of functie. 

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="data-attribution"></a>Toekenning van gegevens  

Project antwoord zoeken antwoorden bevatten informatie die eigendom zijn van derden. U bent zelf verantwoordelijk om te controleren of uw gebruik nodig, bijvoorbeeld door het voldoen aan eventuele creative commons-licentie die uw gebruikerservaring kan vertrouwen op.  
  
Als een antwoord of het resultaat bevat de `contractualRules`, `attributions`, of `provider` velden, moet u de gegevens van het kenmerk. Als het antwoord niet onder een van deze velden, is geen toekenning vereist. Als het antwoord bevat de `contractualRules` veld en de `attributions` en/of `provider` velden, moet u de contractueel regels gebruiken voor het kenmerk van de gegevens.  
  
Het volgende voorbeeld ziet u een entiteit die bestaat uit een MediaAttribution contractueel regel en een afbeelding met een `provider` veld. De regel MediaAttribution identificeert de afbeelding als het doel van de regel, zodat u de installatiekopie zou negeren `provider` veld en gebruiken in plaats daarvan de regel MediaAttribution voor toekenning.  
  
```  
        "value" : [{
            "contractualRules" : [
                . . .
                {
                    "_type" : "ContractualRules\/MediaAttribution",
                    "targetPropertyName" : "image",
                    "mustBeCloseToContent" : true,
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }
            ],
            . . .
            "image" : {
                "name" : "Space Needle",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A46378861201...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }],
                "hostPageUrl" : "http:\/\/www.citydictionary.com\/Uploaded...",
                "width" : 110,
                "height" : 110
            },
            . . .
        }]
```  
  
Als een contractueel regel bevat de `targetPropertyName` veld, de regel geldt alleen voor het betreffende veld. Anders wordt de regel geldt voor het bovenliggende object waarin de `contractualRules` veld.  
  
  
In het volgende voorbeeld wordt de `LinkAttribution` regel bevat de `targetPropertyName` veld, zodat de regel geldt voor de `description` veld. Voor regels die betrekking hebben op specifieke velden, moet u een regel onmiddellijk na de gerichte gegevens met een hyperlink naar de website van de provider opnemen. Als u wilt de beschrijving van het kenmerk, bijvoorbeeld een regel onmiddellijk na de beschrijvende tekst met een hyperlink naar de gegevens op de website van de provider, in dit geval een koppeling maken naar en.wikipedia.org.  
  
```  
"entities" : {  
    "value" : [{  
            . . .  
            "description" : "Peyton Williams Manning is a former American....",  
            . . .  
            "contractualRules" : [{  
                    "_type" : "ContractualRules\/LinkAttribution",  
                    "targetPropertyName" : "description",  
                    "mustBeCloseToContent" : true,  
                    "text" : "en.wikipedia.org",  
                    "url" : "http:\/\/www.bing.com\/cr?IG=B8AD73..."  
                 },  
            . . .  
  
```  

### <a name="license-attribution"></a>Licentie toekenning  

Als de lijst met regels voor contractueel bevat een [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) regel, moet u de kennisgeving weergeven op de regel onmiddellijk na de inhoud die de licentie is van toepassing op. De `LicenseAttribution` regel maakt gebruik van de `targetPropertyName` veld voor het identificeren van de eigenschap die de licentie is van toepassing op.  
  
Hieronder vindt u een voorbeeld waarin een `LicenseAttribution` regel.  
  
![Licentie toekenning](./media/licenseattribution.png)  
  
De aankondiging van de licentie die u wilt weergeven, moet een hyperlink naar de website die informatie over de licentie bevat bevatten. Normaal gesproken u de naam van de licentie een hyperlink. Bijvoorbeeld, als het bericht is **tekst onder licentie CC door SA** en CC door SA is de naam van de licentie, brengt u CC door SA een hyperlink.  
  
### <a name="link-and-text-attribution"></a>Koppeling en afschrijving van tekst  

De [LinkAttribution](reference.md#linkattribution) en [TextAttribution](reference.md#textattribution) regels worden doorgaans gebruikt voor het identificeren van de provider van de gegevens. De `targetPropertyName` veld identificeert het veld dat de regel van toepassing op.  
  
Als u wilt de providers kenmerk, bevatten een regel onmiddellijk na de inhoud die van toepassing zijn de afschrijvingen (bijvoorbeeld het betreffende veld). De regel moet duidelijk label hebben om aan te geven dat de providers de bron van de gegevens zijn. Bijvoorbeeld, "gegevens van: en.wikipedia.org '. Voor `LinkAttribution` regels, moet u een hyperlink naar de website van de provider.  
  
Hieronder vindt u een voorbeeld waarin `LinkAttribution` en `TextAttribution` regels.  
  
![Koppeling tekst toekenning](./media/linktextattribution.png)  

### <a name="media-attribution"></a>Toekenning Media  

Als de entiteit een afbeelding bevat en u weer te geven, moet u een koppeling doorklikken naar de website van de provider opgeven. Als de entiteit bevat een [MediaAttribution](reference.md#mediaattribution) regel, gebruikt u de URL van de regel om de koppeling doorklikken te maken. Anders gebruikt u de URL die is opgenomen in de installatiekopie `provider` veld om de koppeling doorklikken te maken.  
  
Hieronder vindt u een voorbeeld van een afbeelding `provider` veld en contractueel regels. Omdat in het voorbeeld de contractueel regel omvat, negeert u de installatiekopie `provider` veld en toe te passen de `MediaAttribution` regel.  
  
![Toekenning Media](./media/mediaattribution.png)  

## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](c-sharp-quickstart.md)
- [Java-Quick Start](java-quickstart.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
