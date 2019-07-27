---
title: Aantal vooraf samengestelde entiteit-LUIS
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat een aantal vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 88f36fb6d73e2ec88940e7eb53d982824e194074
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560191"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Aantal vooraf samengestelde entiteiten voor een LUIS-app
Er zijn veel manieren waarin numerieke waarden te kwantificeren, express en beschrijven stukjes informatie worden gebruikt. Dit artikel behandelt alleen enkele van de mogelijke voorbeelden. LUIS interpreteert de variaties faciliteren voor de gebruiker uitingen en consistente numerieke waarden als resultaat. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen voorbeeld uitingen met getal tot de toepassing intents. 

## <a name="types-of-number"></a>Typen van getal
Nummer wordt beheerd vanuit de map [recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) github

## <a name="examples-of-number-resolution"></a>Voorbeelden van het aantal resolutie

| Utterance        | Entiteit   | Oplossing |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS omvat de herkende waarde van een **`builtin.number`** entiteit in de `resolution` veld van het JSON-antwoord wordt geretourneerd.

## <a name="resolution-for-prebuilt-number"></a>Oplossing voor vooraf gedefinieerde getal


### <a name="api-version-2x"></a>API-versie 2. x

Het volgende voorbeeld ziet een JSON-antwoord van LUIS, die de resolutie van de waarde van 24 uur per dag, voor de utterance "tientallen" bevat.

```json
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "subtype": "integer",
        "value": "24"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Preview-API versie 3. x

De volgende JSON is met de `verbose` para meter ingesteld `false`op:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ]
        }
    }
}
```

De volgende JSON is met de `verbose` para meter ingesteld `true`op:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "two dozen",
                        "startIndex": 6,
                        "length": 9,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [valuta](luis-reference-prebuilt-currency.md), [rangtelwoord](luis-reference-prebuilt-ordinal.md), en [percentage](luis-reference-prebuilt-percentage.md). 
