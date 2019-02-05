---
title: 'Quickstart: Zoeken naar entiteiten met Project Answer Search'
titlesuffix: Azure Cognitive Services
description: Zoeken naar entiteiten met behulp van Project Answer Search
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/16/2018
ms.author: rosh
ms.openlocfilehash: 2ec11412b5b0e713742029f05c91a6ecbe78c344
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210689"
---
# <a name="quickstart-query-for-entities"></a>Quickstart: Zoeken naar entiteiten

Als de query informatie opvraagt over een persoon, plaats of ding, kan de respons een `entities`-antwoord bevatten.  Query's retourneren altijd webpagina's, waarbij [feiten](fact-queries.md) en/of [entiteiten](entity-queries.md) afhankelijk van de query zijn.

Entiteiten bieden ondersteuning voor drie queryscenario's: 
-   DominantEntity: Er is slechts één entiteit die overeenkomt met de query en intentie van de gebruiker. De query Space Needle is een voorbeeld van een DominantEntity-scenario. 
-   Oplossen van ambiguïteit: Er is meer dan één entiteit die overeenkomt met de query en intentie van de gebruiker en het is aan de gebruiker om de juiste entiteit te selecteren. De query Game of Thrones is een voorbeeld van een scenario voor het oplossen van ambiguïteit omdat hiermee het tv-programma en de serie boeken wordt geretourneerd. 
-   List: Er zijn meerdere entiteiten die overeenkomen met de query en de intentie van de gebruiker. De query 'List of endangered species' is bijvoorbeeld een lijstscenario dat waarden in tabelvorm retourneert opgemaakt voor weergave in rijen en cellen. 
 
U bepaalt het queryscenario via het veld `queryScenario` van het object `entities`. Het type entiteit bepaalt welke gegevens de entiteit bevat. Hoewel entiteiten altijd dezelfde algemene gegevens bevatten, bevatten sommige entiteiten, zoals toeristische bezienswaardigheden of boeken, extra eigenschappen. Entiteiten met extra eigenschappen bevatten het veld `_type`, met daarin een hint die wordt gebruikt door de serializer. De volgende entiteiten hebben aanvullende eigenschappen: 
-   Book 
-   MusicRecording 
-   Person 
-   Attraction 
 
Om het type entiteit te bepalen dat de respons bevat, gebruikt u het veld `entityTypeHints` zoals wordt weergegeven in de query voor Bill Gates.
```
        },
        "description": "Bill Gates is an American business man and philanthropist, co-founder of Microsoft",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Person"
          ]
        },
        "bingId": "6d7d66a7-2cb8-0ae9-637c-f81fd749dc9a"
      }
```
Hieronder volgt een query voor Space Needle:
```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=space+needle&mkt=en-us
```
De respons bevat het antwoord `entities`. Let ook op de velden `entityScenario` en `entityTypeHints`. 
```
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
            "url": "http://en.wikipedia.org/wiki/Space_Needle"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Space_Needle"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dSpace+Needle\u0026filters\u003dsid:%22f8dd5b08-206d-2554-6e4a-893f51f4de7e%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Space Needle",
        "image": {
          "name": "Space Needle",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA15d336cf119b9b5c7e0ab37e271421d3\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Space_Needle"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 152,
          "sourceHeight": 300
        },
        "description": "The Space Needle is an observation tower in Seattle, Washington, a landmark of the Pacific Northwest, and an icon of Seattle. It was built in the Seattle Center for the 1962 World\u0027s Fair, which drew over 2.3 million visitors, when nearly 20,000 people a day used its elevators.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Attraction"
          ]
        },
        "bingId": "f8dd5b08-206d-2554-6e4a-893f51f4de7e"
      }
    ]
  },
```

Een query kan een lijst retourneren als deze relevant is.

**Query:** Met de volgende query vindt u een lijst met bedreigde diersoorten:

```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=list+of+endangered+species

```

**Antwoord:** Het antwoord bevat een lijst die is opgemaakt om te worden weergegeven als tabelwaarden:
```
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "seeMoreUrl": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "value": [
      {
        "subjectName": "Species Directory",
        "richCaption": {
          "_type": "StructuredValue/TabularData",
          "header": [
            "Common name",
            "Scientific name",
            "Conservation status ↓"
          ],
          "rows": [
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Amur Leopard",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/amur-leopard"
                },
                {
                  "text": "Panthera pardus orientalis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Black Rhino",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/black-rhino"
                },
                {
                  "text": "Diceros bicornis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Bornean Orangutan",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/bornean-orangutan"
                },
                {
                  "text": "Pongo pygmaeus"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Cross River Gorilla",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/cross-river-gorilla"
                },
                {
                  "text": "Gorilla gorilla diehli"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            }
          ],
          "seeMoreUrl": {
            "text": "46 more rows",
            "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
          }
        }
      }
    ]
  },

```


## <a name="next-steps"></a>Volgende stappen
- [Quickstart voor C#](c-sharp-quickstart.md)
- [Quickstart voor Java](java-quickstart.md)
- [Quickstart voor Node](node-quickstart.md)
- [Quickstart voor Python](python-quickstart.md)
