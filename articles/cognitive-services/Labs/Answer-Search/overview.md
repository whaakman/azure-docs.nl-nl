---
title: Wat is Project Answer Search?
titlesuffix: Azure Cognitive Services
description: Hier vindt u algemene informatie over Project Answer Search.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: overview
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: cb02c9067e4d672b0aace4caf13e4c8f0d718afb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220192"
---
# <a name="what-is-project-answer-search"></a>Wat is Project Answer Search?
De Project Answer Search-API maakt gebruik van het eindpunt van Bing versie 7 om antwoorden te zoeken voor query's in de vorm van vragen. Een vraag zoals 'What is the circumference of the earth?' (Wat is de omtrek van de aarde?) levert een antwoord op met feitelijke gegevens.  Een query naar een persoon, plaats of ding retourneert informatie over de entiteit die is geïdentificeerd door de query. Deze scenario's kunnen handig zijn in toepassingen zoals gespreks-bots, berichten-apps, schermlezers, enzovoort.  

Query's retourneren antwoorden die afhankelijk zijn van het queryscenario: webpagina's worden altijd geretourneerd, terwijl [feiten](fact-queries.md) en/of [entiteiten](entity-queries.md) alleen worden geretourneerd als ze relevant zijn.

## <a name="endpoint"></a>Eindpunt
Om antwoord te krijgen op een vraag of informatie over een persoon, plaats of ding, verstuurt u een aanvraag naar het eindpunt van de Antwoord zoeken-API. Gebruik de headers en URL-parameters voor verschillende specificaties.  Voeg de header *Ocp-Apim-Subscription-Key* toe met een geldig token.  De parameter 'market' is vereist. Op dit moment wordt alleen de markt `en-us` ondersteund.

De volgende query haalt antwoorden op voor de vraag: 'What is the circumference of the earth?' (Wat is de omtrek van de aarde?)

GET:
```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=what+is+circumference+of+the=earth?&mkt=en-us

```

De URL-parameter `q=` is vereist om het object voor de zoekopdracht op te geven.

## <a name="response-object"></a>Responsobject

Het antwoord of de respons bevat HTTP-headers, webpagina's, feiten en/of entiteiten.

```
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

```

## <a name="terms-of-use"></a>Gebruiksvoorwaarden
Project Answer Search en Project Video Trends zijn onderhevig aan de [gebruiks- en weergavevereisten van Bing Zoeken](use-display-requirements.md).

U, of een derde partij die namens u handelt, mag geen gegevens van de URL Preview-API gebruiken, behouden, opslaan, in een cache plaatsen, delen of distribueren voor test-, ontwikkelings-, trainings- of distributiedoeleinden, noch voor het beschikbaar maken van een niet-Microsoft-service of -functie. 

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="data-attribution"></a>Toeschrijving van gegevens  

Antwoorden van Project Answer Search bevatten informatie die het eigendom is van derde partijen. Het is uw verantwoordelijkheid om ervoor te zorgen dat u op de juiste manier gebruikmaakt van de gegevens, bijvoorbeeld door te voldoen aan een Creative Commons-licentie die is afgegeven om uw gebruikerservaring mogelijk te maken.  
  
Als een antwoord of resultaat het veld `contractualRules`, `attributions` of `provider` bevat, moet u de gegevens toeschrijven. Als het antwoord geen van deze velden bevat, is dit niet vereist. Als het antwoord het veld `contractualRules` en het veld `attributions` en/of de velden `provider` bevat, moet u de contractuele regels volgen om de gegevens toe te schrijven.  
  
In het volgende voorbeeld ziet u een entiteit met de contractuele regel MediaAttribution en een Image-element met een veld `provider`. De regel MediaAttribution identificeert de afbeelding als het doel van de regel, zodat u het veld `provider` van de afbeelding kunt negeren en in plaats daarvan de regel MediaAttribution moet volgen om de gegevens toe te schrijven.  
  
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
  
Als een contractuele regel het veld `targetPropertyName` bevat, geldt de regel alleen voor het betreffende veld. Anders is de regel van toepassing op het bovenliggende object dat het veld `contractualRules` bevat.  
  
  
In het volgende voorbeeld bevat de regel `LinkAttribution` het veld `targetPropertyName`, wat betekent dat de regel geldt voor het veld `description`. Voor regels die betrekking hebben op specifieke velden moet u direct na de betreffende gegevens een regel invoegen met een hyperlink naar de website van de provider. Als u bijvoorbeeld de beschrijving wilt toeschrijven, voegt u direct na de beschrijvende tekst een regel in met een hyperlink naar de gegevens op de website van de provider, in dit voorbeeld een koppeling naar en.wikipedia.org.  
  
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

### <a name="license-attribution"></a>Toeschrijving van licentie  

Als de lijst met contractuele regels een regel [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) bevat, moet u de mededeling opnemen op de regel direct na de inhoud waarop de licentie van toepassing is. De regel `LicenseAttribution` maakt gebruik van het veld `targetPropertyName` om de eigenschap te identificeren waarop de licentie van toepassing is.  
  
Hieronder ziet u een voorbeeld met een regel `LicenseAttribution`.  
  
![Toeschrijving van licentie](./media/licenseattribution.png)  
  
De licentiemededeling die u opneemt, moet een hyperlink bevatten naar de website met informatie over de licentie. Meestal maakt u een hyperlink van de naam van de licentie. Als de mededeling bijvoorbeeld **Tekst valt onder CC-BY-SA-licentie** is en CC-BY-SA de naam is van de licentie, maakt u van CC-BY-SA een hyperlink.  
  
### <a name="link-and-text-attribution"></a>Toeschrijving van koppeling en tekst  

De regels [LinkAttribution](reference.md#linkattribution) en [TextAttribution](reference.md#textattribution) worden meestal gebruikt voor identificatie van de provider van de gegevens. Het veld `targetPropertyName` geeft het veld aan waarop de regel van toepassing is.  
  
Als u de providers wilt toeschrijven, neemt u direct na de inhoud waarom het gaat een regel op met de toeschrijving (bijvoorbeeld na het target-veld). De regel moet een label hebben dat duidelijk aangeeft dat de providers de bron zijn van de gegevens. bijvoorbeeld 'Data from: en.wikipedia.org'. Voor `LinkAttribution`-regels moet u een hyperlink toevoegen naar de website van de provider.  
  
Hieronder ziet u een voorbeeld met de regels `LinkAttribution` en `TextAttribution`.  
  
![Toeschrijving van koppelingstekst](./media/linktextattribution.png)  

### <a name="media-attribution"></a>Toeschrijving van media  

Als de entiteit een afbeelding bevat en u deze wilt weergeven, moet u een klikbare koppeling naar de website van de provider opgeven. Als de entiteit een regel [MediaAttribution](reference.md#mediaattribution) bevat, gebruikt u de URL van de regel om de klikbare koppeling te maken. Gebruik anders de URL die is opgenomen in het veld `provider` van de afbeelding om de klikbare koppeling te maken.  
  
Hieronder ziet u een voorbeeld met het veld `provider` van een afbeelding en contractuele regels. Omdat het voorbeeld een contractuele regel bevat, kunt u het veld `provider` van de afbeelding negeren en de regel `MediaAttribution` toepassen.  
  
![Toeschrijving van media](./media/mediaattribution.png)  

## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](c-sharp-quickstart.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor Node](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
