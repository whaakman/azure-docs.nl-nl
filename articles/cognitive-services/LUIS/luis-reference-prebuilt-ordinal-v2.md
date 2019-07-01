---
title: Rangtelwoord V2 vooraf gemaakte entiteiten
titleSuffix: Language Understanding - Azure Cognitive Services
description: In dit artikel bevat rangtelwoord V2 vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.openlocfilehash: 862b962f5642e01d7ed8250f49d51a6132447083
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486138"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Rangtelwoord V2 vooraf gedefinieerde entiteit voor een LUIS-app
Rangtelwoord V2-nummer wordt uitgebreid [rangtelwoord](luis-reference-prebuilt-ordinal.md) voor relatieve verwijzingen zoals `next`, `last`, en `previous`. Deze gegevens worden niet opgehaald met behulp van de rangtelwoord vooraf gedefinieerde entiteit.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Oplossing voor vooraf gedefinieerde rangtelwoord V2-entiteit

### <a name="api-version-2x"></a>API-versie 2.x

Het volgende voorbeeld ziet u de resolutie van de **builtin.ordinalV2** entiteit.

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

### <a name="preview-api-version-3x"></a>Preview-API-versie 3.x

De volgende JSON is met de `verbose` parameter ingesteld op `false`:

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

De volgende JSON is met de `verbose` parameter ingesteld op `true`:

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

Meer informatie over de [percentage](luis-reference-prebuilt-percentage.md), [telefoonnummer](luis-reference-prebuilt-phonenumber.md), en [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten. 
