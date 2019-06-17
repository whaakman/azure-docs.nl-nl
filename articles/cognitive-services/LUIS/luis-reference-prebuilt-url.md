---
title: URL van vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat een url vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: d59020eb45f7dcced5ea8da04b3b908def34fb70
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65072232"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>URL van vooraf gedefinieerde entiteit voor een LUIS-app
URL-entiteit extraheert URL's met domeinnamen of IP-adressen. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met URL's naar de toepassing. URL-entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="types-of-urls"></a>Typen van URL 's
URL wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) GitHub-opslagplaats

## <a name="resolution-for-prebuilt-url-entity"></a>Oplossing voor vooraf gedefinieerde URL-entiteit

### <a name="api-version-2x"></a>API-versie 2.x

Het volgende voorbeeld ziet u de resolutie van de **builtin.url** entiteit.

```json
{
  "query": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "https://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Preview-API-versie 3.x

De volgende JSON is met de `verbose` parameter ingesteld op `false`:

```json
{
    "query": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
    "prediction": {
        "normalizedQuery": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.421936184
            }
        },
        "entities": {
            "url": [
                "https://www.luis.ai"
            ]
        }
    }
}
```

De volgende JSON is met de `verbose` parameter ingesteld op `true`:

```json
{
    "query": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
    "prediction": {
        "normalizedQuery": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.421936184
            }
        },
        "entities": {
            "url": [
                "https://www.luis.ai"
            ],
            "$instance": {
                "url": [
                    {
                        "type": "builtin.url",
                        "text": "https://www.luis.ai",
                        "startIndex": 0,
                        "length": 19,
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

Meer informatie over de [rangtelwoord](luis-reference-prebuilt-ordinal.md), [getal](luis-reference-prebuilt-number.md), en [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten.
