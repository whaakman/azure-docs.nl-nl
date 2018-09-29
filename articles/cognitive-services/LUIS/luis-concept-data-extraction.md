---
title: Gegevens uitpakken concepten in LUIS - Language Understanding
titleSuffix: Azure Cognitive Services
description: Informatie over wat voor soort gegevens kan worden geëxtraheerd uit Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 39d36ee0c46d3e6954c3264f37f3f575130186b9
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434480"
---
# <a name="data-extraction"></a>Ophalen van gegevens
LUIS biedt u de mogelijkheid informatie ophalen van natuurlijke taal-uitingen van een gebruiker. De informatie wordt opgehaald op een manier dat deze kan worden gebruikt door een programma, toepassing of bot chatten om actie te ondernemen. In de volgende secties meer informatie over welke gegevens worden geretourneerd door intenties en entiteiten met voorbeelden van JSON.

De moeilijkst gegevens om op te halen zijn de gegevens hebt geleerd van een machine omdat deze niet een overeenkomst exact overeenkomende tekst. Ophalen van gegevens van de machine geleerd [entiteiten](luis-concept-entity-types.md) moet deel uitmaken van de [ontwerpen cyclus](luis-concept-app-iteration.md) totdat u er zeker van te zijn ontvangen van de gegevens die u verwacht.

## <a name="data-location-and-key-usage"></a>Locatie en de sleutel gegevensgebruik
LUIS, biedt de gegevens van de gepubliceerde [eindpunt](luis-glossary.md#endpoint). De **HTTPS-aanvraag** (POST of GET) bevat de utterance, evenals van sommige optionele configuraties, zoals fasering of productie-omgevingen.

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

De `appID` is beschikbaar op de **instellingen** pagina van uw LUIS-app als onderdeel van de URL (nadat `/apps/`) wanneer u die LUIS-app wilt bewerken. De `subscription-key` is de eindpuntsleutel die wordt gebruikt voor query's in uw app. U kunt uw gratis ontwerpen/starter-sleutel gebruiken tijdens de zelfstudie LUIS, is het belangrijk om te wijzigen van de eindpuntsleutel in een sleutel die ondersteuning biedt voor uw [verwacht gebruik van LUIS](luis-boundaries.md#key-limits). De `timezoneOffset` eenheid minuten is.

De **HTTPS-antwoord** bevat alle de intentie en entiteit informatie LUIS kunt bepalen op basis van de huidige gepubliceerde model van een eindpunt van de fasering of productie. Het eindpunt van de URL die is gevonden op de [LUIS](luis-reference-regions.md) website in de **beheren** sectie, op de **sleutels en eindpunten** pagina.

## <a name="data-from-intents"></a>Gegevens van intenties
De primaire gegevens is de hoogste score **intentie naam**. Met behulp van de `MyStore` [snelstartgids](luis-quickstart-intents-only.md), het eindpunt-antwoord is:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

|Data-Object|Gegevenstype|Gegevenslocatie|Waarde|
|--|--|--|--|
|Intentie|Reeks|topScoringIntent.intent|"GetStoreInfo"|

Als uw chatbot of app van LUIS-aanroepen maakt een beslissing op basis van meer dan één intentie score, de intenties scores terug door in te stellen de queryreeks-parameter `verbose=true`. De eindpunt-antwoord is:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

De intenties zijn gerangschikt op de hoogste naar laagste score.

|Data-Object|Gegevenstype|Gegevenslocatie|Waarde|Score|
|--|--|--|--|:--|
|Intentie|Reeks|intents [0] .intent|"GetStoreInfo"|0.984749258|
|Intentie|Reeks|.intent intents [1]|'Geen'|0.0168218873|

Als u vooraf gemaakte domeinen toevoegt, geeft de naam van de intentie het domein, zoals `Utilties` of `Communication` en het doel:

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

|Domain|Data-Object|Gegevenstype|Gegevenslocatie|Waarde|
|--|--|--|--|--|
|Nutsbedrijven|Intentie|Reeks|intents [0] .intent|"<b>Hulpprogramma's voor</b>. ShowNext"|
|Communicatie|Intentie|Reeks|.intent intents [1]|<b>Communicatie</b>. Opnieuw"|
||Intentie|Reeks|.intent intents [2]|'Geen'|


## <a name="data-from-entities"></a>Gegevens van entiteiten
De meeste chatbots en toepassingen moeten meer dan de naam van de intentie. Deze aanvullende, optionele gegevens afkomstig van entiteiten in de utterance gedetecteerd. Elk type entiteit retourneert verschillende gegevens over de overeenkomst.

Een bepaald woord of zinsdeel in een utterance kan overeenkomen met meer dan één entiteit. In dat geval wordt elke overeenkomende entiteit geretourneerd met de score.

Alle entiteiten worden geretourneerd in de **entiteiten** matrix van het antwoord van het eindpunt:

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

## <a name="tokenized-entity-returned"></a>tokens entiteit geretourneerd
Verschillende [culturen](luis-language-support.md#tokenization) retourneren van de entiteit met de `entity` waarde [tokenized](luis-glossary.md#token). De startIndex en endIndex die zijn geretourneerd door LUIS in de entiteitsobject toewijzen niet aan de nieuwe, tokens waarde, maar in plaats daarvan naar de oorspronkelijke query zodat u de onbewerkte entiteit via een programma ophalen. 

Bijvoorbeeld, in het Duits, het woord `das Bauernbrot` is tokenized in `das bauern brot`. De waarde van tokens, `das bauern brot`, wordt geretourneerd en de oorspronkelijke waarde kan via een programma kan worden bepaald uit het startIndex en endIndex van de oorspronkelijke query, zodat u `das Bauernbrot`.

## <a name="simple-entity-data"></a>Eenvoudige entiteitsgegevens

Een [eenvoudige entiteit](luis-concept-entity-types.md) is een waarde hebt geleerd van een machine. Een woord of woordgroep kan het zijn.

`Bob Jones wants 3 meatball pho`

In de vorige utterance `Bob Jones` wordt aangeduid als een eenvoudige `Customer` entiteit.

De gegevens die worden geretourneerd van het eindpunt bevat de naam van de entiteit, de gedetecteerde tekst van de utterance, de locatie van de gedetecteerde tekst en de score is:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Data-object|De naam van de entiteit|Waarde|
|--|--|--|
|Eenvoudige entiteit|"Klant"|"bob jones"|

## <a name="hierarchical-entity-data"></a>Hiërarchische entiteitsgegevens

[Hiërarchische](luis-concept-entity-types.md) entiteiten zijn machine geleerd en een woord of woordgroep kan bevatten. Onderliggende items worden aangeduid met context. Als u naar een bovenliggende / onderliggende relatie met exact overeenkomende tekst overeenkomst zoekt, gebruikt u een [lijst](#list-entity-data) entiteit.

`book 2 tickets to paris`

In de vorige utterance `paris` heet een `Location::ToLocation` onderliggende lid van de `Location` hiërarchische entiteit.

De gegevens die worden geretourneerd van het eindpunt bevat de naam van de entiteit en naam van de onderliggende, de gedetecteerde tekst van de utterance, de locatie van de gedetecteerde tekst en de score is:

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Location::ToLocation",
    "startIndex": 18,
    "endIndex": 22,
    "score": 0.6866132
  }
]
```

|Data-object|Bovenliggend item|Onderliggende|Waarde|
|--|--|--|--|--|
|Hiërarchische entiteit|Locatie|ToLocation|"Parijs"|

## <a name="composite-entity-data"></a>Samengestelde entiteitsgegevens
[Samengestelde](luis-concept-entity-types.md) entiteiten zijn machine geleerd en een woord of woordgroep kan bevatten. Neem bijvoorbeeld een samengestelde entiteit van het vooraf gedefinieerde `number` en `Location::ToLocation` met de volgende utterance:

`book 2 tickets to paris`

U ziet dat `2`, het aantal en `paris`, de ToLocation woorden tussen deze die geen deel uitmaken van een van de entiteiten hebben. De groene lijn, gebruikt in een gelabelde utterance in de [LUIS](luis-reference-regions.md) -website geeft aan dat een samengestelde entiteit.

![Samengestelde entiteit](./media/luis-concept-data-extraction/composite-entity.png)

Samengestelde entiteiten worden geretourneerd in een `compositeEntities` matrix en alle entiteiten in de samengestelde worden ook weergegeven in de `entities` matrix:

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 18,
      "endIndex": 22,
      "score": 0.956998169
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 5,
      "endIndex": 5,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "2 tickets to paris",
      "type": "Order",
      "startIndex": 5,
      "endIndex": 22,
      "score": 0.7714499
    }
  ],
  "compositeEntities": [
    {
      "parentType": "Order",
      "value": "2 tickets to paris",
      "children": [
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Location::ToLocation",
          "value": "paris"
        }
      ]
    }
  ]
```    

|Data-object|De naam van de entiteit|Waarde|
|--|--|--|
|Vooraf gedefinieerde entiteit - nummer|"builtin.number"|"2"|
|Hiërarchische entiteit - locatie|"Location::ToLocation"|"Parijs"|

## <a name="list-entity-data"></a>Lijst met entiteitsgegevens

Een [lijst](luis-concept-entity-types.md) entiteit is niet machine-hebt geleerd. Het is een overeenkomst exact overeenkomende tekst. Een lijst met vertegenwoordigt items in de lijst samen met synoniemen voor die items. LUIS markeert een overeenkomst aan een item in een lijst als een entiteit in het antwoord. Een synoniem kan zich in meer dan één lijst.

Stel dat de app heeft een lijst met de naam `Cities`, zodat voor variaties in plaats van luchthaven (Sea-tac), luchthaven code (SEA), postcode (98101) en het netnummer telefoon (206) zoals plaatsnamen.

|Lijstitem|Item synoniemen|
|---|---|
|Seattle|zee tac, zee 98101, 206, + 1 |
|Parijs|cdg, roissy, schiedenisitems, 75001, 1, +33|

`book 2 tickets to paris`

In de vorige utterance, het woord `paris` is toegewezen aan het item Parijs als onderdeel van de `Cities` lijst van de entiteit. De entiteit lijst komt overeen met zowel genormaliseerde naam van het item als het item synoniemen.

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 22,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

Een ander voorbeeld-utterance, met behulp van een synoniem voor Parijs:

`book 2 tickets to roissy`

```JSON
"entities": [
  {
    "entity": "roissy",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 23,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

## <a name="prebuilt-entity-data"></a>Vooraf gemaakte entiteiten
[Vooraf gedefinieerde](luis-concept-entity-types.md) entiteiten worden gedetecteerd op basis van een komt overeen met de reguliere expressie met behulp van de open-source [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. Vooraf gemaakte entiteiten in de matrix entiteiten worden geretourneerd en gebruikt u de naam van het type voorafgegaan door `builtin::`. De volgende tekst is een voorbeeld-utterance met de geretourneerde vooraf gemaakte entiteiten:

`Dec 5th send to +1 360-555-1212`

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
```

## <a name="regular-expression-entity-data"></a>Entiteitsgegevens reguliere expressie
[Reguliere expressie](luis-concept-entity-types.md) entiteiten worden gedetecteerd op basis van een komt overeen met de reguliere expressie met behulp van een expressie die u opgeeft wanneer u de entiteit is gemaakt. Bij het gebruik van `kb[0-9]{6}` als de definitie van de reguliere expressie entiteit is het volgende JSON-antwoord een utterance voorbeeld met de entiteiten geretourneerde reguliere expressie voor de query `When was kb123456 published?`:

```JSON
{
  "query": "when was kb123456 published?",
  "topScoringIntent": {
    "intent": "FindKBArticle",
    "score": 0.933641255
  },
  "intents": [
    {
      "intent": "FindKBArticle",
      "score": 0.933641255
    },
    {
      "intent": "None",
      "score": 0.04397359
    }
  ],
  "entities": [
    {
      "entity": "kb123456",
      "type": "KB number",
      "startIndex": 9,
      "endIndex": 16
    }
  ]
}
```

## <a name="extracting-names"></a>Uitpakken van namen
Namen ophalen uit een utterance is moeilijk, omdat een naam mag bestaan uit vrijwel elke combinatie van letters en woorden. Afhankelijk van welk type de naam die u uitpakken wilt, hebt u verschillende mogelijkheden. Dit zijn geen regels, maar meer richtlijnen.

### <a name="names-of-people"></a>Namen van personen
De naam van mensen kan een lichte indeling, afhankelijk van de taal en cultuur hebben. Gebruik een hiërarchische entiteit met voor- en achternamen als onderliggende items of een enkele entiteit met de rol van de voornaam en achternaam. Zorg ervoor dat u voorbeelden gegeven die gebruikmaken van de naam van de eerste en laatste in de verschillende onderdelen van de utterance, in uitingen van verschillende lengtes en uitingen over alle intents, met inbegrip van de geen intentie. [Beoordeling](luis-how-to-review-endoint-utt.md) eindpunt uitingen regelmatig naar elke labelnamen zijn niet correct voorspeld.

### <a name="names-of-places"></a>Namen van plaatsen
Locatienamen zijn ingesteld en bekend zijn, zoals steden, regio's, Staten, provincies en landen. Als uw app gebruikmaakt van een bekende set locaties, kunt u een lijst met entiteit. Als u vinden dat alle namen plaatsen wilt, een eenvoudige entiteit maken en bieden een aantal voorbeelden. Een woordgroepenlijst van het geocoderen van plaatsnamen te versterken welke plaats namen eruit in uw app toevoegen. [Beoordeling](luis-how-to-review-endoint-utt.md) eindpunt uitingen regelmatig naar elke labelnamen zijn niet correct voorspeld.

### <a name="new-and-emerging-names"></a>Nieuwe en opkomende namen
Sommige apps moeten kunnen om nieuwe en opkomende namen, zoals producten of bedrijven te vinden. Dit is het moeilijkste type ophalen van gegevens. Beginnen met een enkele entiteit en een woordgroepenlijst met toevoegen. [Beoordeling](luis-how-to-review-endoint-utt.md) eindpunt uitingen regelmatig naar elke labelnamen zijn niet correct voorspeld.

## <a name="pattern-roles-data"></a>Patroon rollen gegevens
Rollen zijn contextuele verschillen van entiteiten.

```JSON
{
  "query": "move bob jones from seattle to redmond",
  "topScoringIntent": {
    "intent": "MoveAssetsOrPeople",
    "score": 0.9999998
  },
  "intents": [
    {
      "intent": "MoveAssetsOrPeople",
      "score": 0.9999998
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 6.12244548E-07
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 6.12244548E-07
    },
    {
      "intent": "FindForm",
      "score": 1.1E-09
    }
  ],
  "entities": [
    {
      "entity": "bob jones",
      "type": "Employee",
      "startIndex": 5,
      "endIndex": 13,
      "score": 0.922820568,
      "role": ""
    },
    {
      "entity": "seattle",
      "type": "Location",
      "startIndex": 20,
      "endIndex": 26,
      "score": 0.948008537,
      "role": "Origin"
    },
    {
      "entity": "redmond",
      "type": "Location",
      "startIndex": 31,
      "endIndex": 37,
      "score": 0.7047979,
      "role": "Destination"
    }
  ]
}
```

## <a name="patternany-entity-data"></a>Entiteitsgegevens Pattern.any
Pattern.any entiteiten zijn variabele lengte-entiteiten die worden gebruikt in de sjabloon-uitingen van een [patroon](luis-concept-patterns.md).

```JSON
{
  "query": "where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.999999464
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.999999464
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 4.883697E-06
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 9.278342E-07
    },
    {
      "intent": "MoveAssetsOrPeople",
      "score": 9.278342E-07
    }
  ],
  "entities": [
    {
      "entity": "understand your responsibilities as a member of the community",
      "type": "FormName",
      "startIndex": 18,
      "endIndex": 78,
      "role": ""
    }
  ]
}
```


## <a name="sentiment-analysis"></a>Sentimentanalyse
Als u sentimentanalyse is geconfigureerd, bevat de json-antwoord van LUIS sentimentanalyse. Meer informatie over sentimentanalyse in de [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) documentatie.

### <a name="sentiment-data"></a>Sentimentsgegevens
Sentimentsgegevens is een score tussen 1 en 0 waarmee wordt aangegeven welke positieve (dichter bij 1) of een negatieve (dichter bij 0) gevoel van de gegevens.

Wanneer de cultuur is `en-us`, het antwoord is:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Voor alle andere culturen en is het antwoord:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Sleuteluitdrukkingen extraheren entiteitsgegevens
De entiteit van sleuteluitdrukkingen extraheren retourneert belangrijke zinnen in de utterance, geleverd door [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

```JSON
{
  "query": "Is there a map of places with beautiful views on a favorite trail?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.764368951
  },
  "intents": [
    ...
  ],
  "entities": [
    {
      "entity": "beautiful views",
      "type": "builtin.keyPhrase",
      "startIndex": 30,
      "endIndex": 44
    },
    {
      "entity": "map of places",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 23
    },
    {
      "entity": "favorite trail",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 64
    }
  ]
}
```

## <a name="data-matching-multiple-entities"></a>Gegevens die overeenkomen met meerdere entiteiten
LUIS retourneert alle entiteiten in de utterance gedetecteerd. Als gevolg hiervan moet de chatbot mogelijk beslissing nemen op basis van de resultaten. Een utterance kan veel entiteiten in een utterance hebben:

`book me 2 adult business tickets to paris tomorrow on air france`

Het eindpunt LUIS kan dezelfde gegevens in verschillende entiteiten detecteren:

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.
