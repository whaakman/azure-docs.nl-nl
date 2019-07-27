---
title: Leeftijd preconstrueerde entiteit-LUIS
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat leeftijd vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 77290790342c1159eca049108e1c9fadd5d4bbf3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560300"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>Leeftijd preconstrueerde entiteit voor een LUIS-app
De vooraf gedefinieerde leeftijd entiteit bevat de leeftijdwaarde zowel numeriek en in termen van dagen, weken, maanden en jaren. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met leeftijd aan de toepassing intents. Leeftijd entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Typen leeftijd
De leeftijd wordt beheerd vanuit de map [recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) github

## <a name="resolution-for-prebuilt-age-entity"></a>Oplossing voor vooraf gedefinieerde leeftijd entiteit

### <a name="api-version-2x"></a>API-versie 2. x

Het volgende voorbeeld ziet u de resolutie van de **builtin.age** entiteit.

```json
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Preview-API versie 3. x

De volgende JSON is met de `verbose` para meter ingesteld `false`op:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ]
        }
    }
}
```

De volgende JSON is met de `verbose` para meter ingesteld `true`op:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ],
            "$instance": {
                "age": [
                    {
                        "type": "builtin.age",
                        "text": "90 day old",
                        "startIndex": 2,
                        "length": 10,
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

Meer informatie over de [valuta](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), en [dimensie](luis-reference-prebuilt-dimension.md) entiteiten. 
