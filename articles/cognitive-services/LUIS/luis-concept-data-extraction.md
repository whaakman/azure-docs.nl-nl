---
title: Inzicht in gegevens extractie concepten in LUIS - Azure | Microsoft Docs
description: Meer informatie over wat voor soort gegevens kan worden geëxtraheerd uit Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: ad9eaa966aafc4a9fc526d44d57c68cdaee3a5e0
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345975"
---
# <a name="data-extraction"></a>Ophalen van gegevens
LUIS biedt u de mogelijkheid informatie ophalen van een gebruiker natuurlijke taal utterances. De informatie wordt geëxtraheerd zodanig dat deze kan worden gebruikt door een programma, toepassing of chatbot actie te ondernemen.

In de volgende secties meer informatie over welke gegevens worden geretourneerd van intents en entiteiten met voorbeelden van JSON. De moeilijkst gegevens uitpakken zijn de gegevens machine geleerd, omdat het is niet een overeenkomst exact tekst. Ophalen van gegevens van de machine geleerd [entiteiten](luis-concept-entity-types.md) moet deel uitmaken van de [cyclus authoring](luis-concept-app-iteration.md) totdat u er zeker van te zijn ontvangen van de gegevens die u verwacht. 

## <a name="data-location-and-key-usage"></a>Dataverbruik van locatie en de sleutel
LUIS biedt de gegevens van de gepubliceerde [eindpunt](luis-glossary.md#endpoint). De **HTTPS-aanvraag** (POST of GET) bevat de utterance, evenals een aantal optionele configuraties zoals tijdelijke of productieomgevingen. 

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

De `appID` beschikbaar is op de **instellingen** pagina van uw app LUIS, evenals een deel van de URL (nadat `/apps/`) wanneer u die app LUIS bewerkt. De `subscription-key` is de endpoint-sleutel gebruikt voor query's in uw app. U kunt uw gratis authoring/starter sleutel gebruiken terwijl u LUIS leert, is het belangrijk dat de abonnementssleutel wijzigen in een sleutel die ondersteuning biedt voor uw [verwacht LUIS gebruik](luis-boundaries.md#key-limits). De `timezoneOffset` eenheid minuten is.

De **HTTPS antwoord** bevat alle de bedoeling en entiteit informatie LUIS kunt bepalen op basis van het huidige, gepubliceerde model uit het eindpunt van de test- of productieomgeving. Het eindpunt URL is gevonden op de [LUIS] [ LUIS] website **publiceren** pagina. 

## <a name="data-from-intents"></a>Gegevens van intents
De primaire gegevens, is het hoogste score berekenen **opzet naam**. Met behulp van de `MyStore` [Quick Start](luis-quickstart-intents-only.md), het antwoord van het eindpunt is:

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

|Gegevensobject|Gegevenstype|Gegevenslocatie|Waarde|
|--|--|--|--|
|Doel|Reeks|topScoringIntent.intent|'GetStoreInfo'|

Als uw chatbot of LUIS aanroepen app maakt een beslissing op basis van meer dan één opzet score, alle de intents scores geretourneerd door de parameter querystring `verbose=true`. Het antwoord van het eindpunt is:

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

De intents zijn gerangschikt van hoogste naar laagste score.

|Gegevensobject|Gegevenstype|Gegevenslocatie|Waarde|Score|
|--|--|--|--|:--|
|Doel|Reeks|.intent intents [0]|'GetStoreInfo'|0.984749258|
|Doel|Reeks|.intent intents [1]|'None'|0.0168218873|

Als u vooraf gedefinieerde domeinen toevoegt, geeft de naam van de opzet het domein, zoals `Utilties` of `Communication` evenals de bedoeling:

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
    
|Domein|Gegevensobject|Gegevenstype|Gegevenslocatie|Waarde|
|--|--|--|--|--|
|Nutsbedrijven|Doel|Reeks|.intent intents [0]|"<b>Hulpprogramma's</b>. ShowNext'|
|Communicatie|Doel|Reeks|.intent intents [1]|<b>Communicatie</b>. Opnieuw'|
||Doel|Reeks|.intent intents [2]|'None'|


## <a name="data-from-entities"></a>Gegevens van entiteiten
De meeste chatbots en toepassingen moeten meer dan de naam van de opzet. Deze aanvullende, optionele gegevens afkomstig van entiteiten in de utterance gedetecteerd. Elk type entiteit retourneert andere informatie over de overeenkomst. 

Een enkel woord of zinsdeel in een utterance kan overeenkomen met meer dan één entiteit. In dat geval wordt elke overeenkomende entiteit geretourneerd met de score. 

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

## <a name="tokenized-entity-returned"></a>Tokens entiteit geretourneerd
Verschillende [culturen](luis-supported-languages.md#tokenization) entiteit retourneren met de `entity` waarde [tokenized](luis-glossary.md#token). StartIndex en endIndex geretourneerd door LUIS in de entiteitsobject niet aan zijn toegewezen aan de nieuwe, tokens-waarde maar in plaats daarvan de oorspronkelijke query zodat u de onbewerkte entiteit programmatisch ophalen. 

Bijvoorbeeld, in het Duits, het woord `das Bauernbrot` is tokenized in `das bauern brot`. De tokens waarde `das bauern brot`, wordt geretourneerd en de oorspronkelijke waarde via een programma kan worden bepaald uit het startIndex en endIndex van de oorspronkelijke query, zodat u `das Bauernbrot`.

## <a name="simple-entity-data"></a>Eenvoudige entiteitsgegevens

Een [eenvoudige entiteit](luis-concept-entity-types.md) is de waarde voor een machine geleerd. Het kan een woord of woordgroep zijn. 

`Bob Jones wants 3 meatball pho`

In de vorige utterance `Bob Jones` wordt aangeduid als een eenvoudige `Customer` entiteit.

De gegevens die worden geretourneerd van het eindpunt bevat de naam van de entiteit, de gedetecteerde tekst van de utterance, de locatie van de gedetecteerde tekst en de score:

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

|Gegevensobject|De naam van de entiteit|Waarde|
|--|--|--|
|Enkele entiteit|"Klant"|'bob jones'|

## <a name="hierarchical-entity-data"></a>Hiërarchische entiteitsgegevens

[Hiërarchische](luis-concept-entity-types.md) entiteiten worden geleerd van machine en een woord of woordgroep kunt opnemen. Onderliggende elementen worden geïdentificeerd door context. Als u een bovenliggende / onderliggende relatie met exacte tekst overeenkomst zoekt, gebruikt u een [lijst](#list-entity-data) entiteit. 

`book 2 tickets to paris`

In de vorige utterance `paris` heet een `Location::ToLocation` onderliggend element van de `Location` hiërarchische entiteit. 

De gegevens die worden geretourneerd van het eindpunt bevat de naam van de entiteit en naam van het onderliggende, de gedetecteerde tekst van de utterance, de locatie van de gedetecteerde tekst en de score: 

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

|Gegevensobject|Bovenliggend item|Onderliggende|Waarde|
|--|--|--|--|--|
|Hiërarchische entiteit|Locatie|ToLocation|'Parijs'|

## <a name="composite-entity-data"></a>Samengestelde entiteitsgegevens
[Samengestelde](luis-concept-entity-types.md) entiteiten worden geleerd van machine en een woord of woordgroep kunt opnemen. Neem bijvoorbeeld een samengestelde entiteit van het vooraf gedefinieerde `number` en `Location::ToLocation` met de volgende utterance:

`book 2 tickets to paris`

U ziet dat `2`, het getal en `paris`, de ToLocation woorden daartussen die geen deel uitmaken van een van de entiteiten hebben. De groene onderstreping, die wordt gebruikt in een gelabelde utterance in de [LUIS] [ LUIS] website, geeft een samengestelde entiteit.

![Samengestelde entiteit](./media/luis-concept-data-extraction/composite-entity.png)

Samengestelde entiteiten worden geretourneerd in een `compositeEntities` matrix en alle entiteiten in de samenstelling wordt ook geretourneerd in de `entities` matrix:

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

|Gegevensobject|De naam van de entiteit|Waarde|
|--|--|--|
|Vooraf gedefinieerde entiteit - nummer|'builtin.number'|'2'|
|Hiërarchische entiteit - locatie|'Location::ToLocation'|'Parijs'|

## <a name="list-entity-data"></a>Lijst entiteitsgegevens

Een [lijst](luis-concept-entity-types.md) entiteit is niet machine is vastgelegd. Het is een overeenkomst exact tekst. Een lijst met vertegenwoordigt items in de lijst samen met synoniemen voor die items. LUIS markeert een overeenkomst aan een item in een lijst als een entiteit in het antwoord. Een synoniem kan zich in meer dan één lijst. 

Stel dat de app heeft een lijst met de naam `Cities`, zodat voor variaties van de namen van steden inclusief stad van luchthaven (Sea tac), luchthaven code (SEA), postcode (98101) en het netnummer telefoon (206). 

|Lijstitem|Synoniemen item|
|---|---|
|Seattle|SEA tac, sea, 98101, 206, + 1 |
|Parijs|cdg, roissy schiedenisitems, 75001, 1, +33|

`book 2 tickets to paris`

In de vorige utterance, het woord `paris` is toegewezen aan het item Parijs als onderdeel van de `Cities` lijst entiteit. De entiteit lijst komt overeen met zowel de genormaliseerde naam van het item als de synoniemen item. 

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

Een ander voorbeeld-utterance, met een synoniem voor Parijs:

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

## <a name="prebuilt-entity-data"></a>Vooraf gedefinieerde entiteitsgegevens
[Vooraf gedefinieerde](luis-concept-entity-types.md) entiteiten zijn gedetecteerd op basis van een overeen met reguliere expressie met behulp van de open-source [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text) project. Vooraf gedefinieerde entiteiten in de matrix entiteiten worden geretourneerd en gebruikt u de naam van het type voorafgegaan door `builtin::`. De volgende tekst is een voorbeeld utterance met de geretourneerde vooraf gemaakte entiteiten:

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

## <a name="regular-expression-entity-data"></a>Reguliere expressie entiteitsgegevens
[Reguliere expressie](luis-concept-entity-types.md) entiteiten zijn gedetecteerd op basis van een overeen met reguliere expressie met een expressie die u opgeeft tijdens het maken van de entiteit. Wanneer u `kb[0-9]{6}` als de definitie van de entiteit reguliere expressie is het volgende JSON-antwoord een utterance voorbeeld met de entiteiten geretourneerde reguliere expressie voor de query `When was kb123456 published?`:

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

## <a name="extracting-names"></a>Het uitpakken van namen
Het is moeilijk om namen ophalen uit een utterance omdat het een naam kan vrijwel elke combinatie van letters en woorden zijn. Afhankelijk van welk type de naam die u uitpakken wilt, hebt u verschillende mogelijkheden. Dit zijn geen regels maar meer richtlijnen. 

### <a name="names-of-people"></a>Namen van personen
Naam van gebruikers kan een lichte indeling, afhankelijk van de taal en cultuur hebben. Gebruik een hiërarchische entiteit met de voor- en achternamen als onderliggende of een enkele entiteit met de rol van de voornaam en achternaam. Zorg ervoor dat bevatten voorbeelden die de naam van de eerste en laatste in verschillende onderdelen van de utterance, in utterances van verschillende lengtes en utterances in alle intents gebruiken, met inbegrip van de geen opzet. [Bekijk](label-suggested-utterances.md) eindpunt utterances regelmatig te labelen namen die zijn niet correct voorspeld. 

### <a name="names-of-places"></a>Namen van plaatsen
Locatienamen zijn ingesteld en bekend zoals steden, regio's, statussen, provincies en landen. Als uw app gebruikmaakt van een bekende reeks locaties, kunt u een lijst-entiteit. Als u zoeken naar dat alle namen plaatsen wilt, maak een enkele entiteit en bieden een aantal voorbeelden. Voeg een woordgroepenlijst met namen te versterken welke place names eruit in uw app. [Bekijk](label-suggested-utterances.md) eindpunt utterances regelmatig te labelen namen die zijn niet correct voorspeld. 

### <a name="new-and-emerging-names"></a>Nieuwe en opkomende namen
Sommige apps moeten kunnen nieuwe en opkomende namen zoals producten of bedrijven vinden. Dit is het zeer moeilijk type ophalen van gegevens. Beginnen met een enkele entiteit en een woordgroepenlijst toevoegen. [Bekijk](label-suggested-utterances.md) eindpunt utterances regelmatig te labelen namen die zijn niet correct voorspeld. 

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

## <a name="patternany-entity-data"></a>Pattern.any entiteitsgegevens
Pattern.any entiteiten zijn variabele lengte entiteiten die zijn gebruikt in de sjabloon utterances van een [patroon](luis-concept-patterns.md). 

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
Als gevoel analyse is geconfigureerd, wordt in het antwoord van de json LUIS gevoel analyse bevat. Meer informatie over gevoel analyse in de [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) documentatie.

### <a name="sentiment-data"></a>Gevoel gegevens
Gevoel gegevens is een score tussen 1 en 0 positieve waarmee wordt aangegeven (dichter bij 1) of een negatieve (dichter op 0) gevoel van de gegevens.

Als cultuur is `en-us`, het antwoord is:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Het antwoord is voor alle andere culturen:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Sleutel woordgroep extractie entiteitsgegevens
De belangrijkste woordgroep extractie entiteit retourneert sleutel zinnen in de utterance, geleverd door [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

<!-- TBD: verify JSON-->
```JSON
"keyPhrases": [
    "places",
    "beautiful views",
    "favorite trail"
]
```

## <a name="data-matching-multiple-entities"></a>Gegevens die overeenkomen met meerdere entiteiten
LUIS retourneert alle entiteiten in de utterance gedetecteerd. Als gevolg hiervan moet uw chatbot mogelijk beslissing op basis van de resultaten. Een utterance kan vele entiteiten in een utterance hebben:

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

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over het toevoegen van entiteiten aan uw app LUIS.

[LUIS]:luis-reference-regions.md