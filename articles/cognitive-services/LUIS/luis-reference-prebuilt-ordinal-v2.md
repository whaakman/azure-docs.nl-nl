---
title: Rang telwoord v2 vooraf gebouwde entiteit-LUIS
titleSuffix: Azure Cognitive Services
description: Dit artikel bevat vooraf opgemaakte nummer v2-entiteits gegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.openlocfilehash: 972f75fd1c977e79a2fa70c44bb3069e2c69a2c5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563415"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Rang telwoord v2 vooraf gebouwde entiteit voor een LUIS-app
Ordinal v2 `next`-nummer breidt [rang telwoord](luis-reference-prebuilt-ordinal.md) uit om relatieve verwijzingen te `last`bieden, `previous`zoals,, en. Deze worden niet geëxtraheerd met behulp van de vooraf gebouwde ordinale entiteit.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Oplossing voor vooraf ontwikkelde rang telwoord v2-entiteit

### <a name="api-version-2x"></a>API-versie 2. x

In het volgende voor beeld ziet u de oplossing van de **ingebouwde entiteit. ordinalV2** .

```json
{
    "query": "what is the second to last choice in the list",
    "topScoringIntent": {
        "intent": "None",
        "score": 0.823669851
    },
    "intents": [
        {
            "intent": "None",
            "score": 0.823669851
        }
    ],
    "entities": [
        {
            "entity": "the second to last",
            "type": "builtin.ordinalV2.relative",
            "startIndex": 8,
            "endIndex": 25,
            "resolution": {
                "offset": "-1",
                "relativeTo": "end"
            }
        }
    ]
}
```

### <a name="preview-api-version-3x"></a>Preview-API versie 3. x

De volgende JSON is met de `verbose` para meter ingesteld `false`op:

```json
{
    "query": "what is the second to last choice in the list",
    "prediction": {
        "normalizedQuery": "what is the second to last choice in the list",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "ordinalV2": [
                {
                    "offset": -1,
                    "relativeTo": "end"
                }
            ]
        }
    }
}
```

De volgende JSON is met de `verbose` para meter ingesteld `true`op:

```json
{
    "query": "what is the second to last choice in the list",
    "prediction": {
        "normalizedQuery": "what is the second to last choice in the list",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "ordinalV2": [
                {
                    "offset": -1,
                    "relativeTo": "end"
                }
            ],
            "$instance": {
                "ordinalV2": [
                    {
                        "type": "builtin.ordinalV2.relative",
                        "text": "the second to last",
                        "startIndex": 8,
                        "length": 18,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [percentage](luis-reference-prebuilt-percentage.md), het [telefoon nummer](luis-reference-prebuilt-phonenumber.md)en de [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten. 
