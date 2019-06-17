---
title: LUIS vooraf gemaakte entiteiten e-referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel e-mailbericht bevat vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 2b11446c84ede0e8ecfce23eda1026919777fc66
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65072085"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>E-vooraf gedefinieerde entiteit voor een LUIS-app
Extractie van e-mailbericht bevat het volledige e-mailadres van een utterance. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met e-mailbericht naar de toepassing intents. E-entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="resolution-for-prebuilt-email"></a>Oplossing voor vooraf gemaakte e-mailadres

### <a name="api-version-2x"></a>API-versie 2.x

Het volgende voorbeeld ziet u de resolutie van de **builtin.email** entiteit.

```json
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55,
      "resolution": {
        "value": "patti.owens@microsoft.com"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Preview-API-versie 3.x

De volgende JSON is met de `verbose` parameter ingesteld op `false`:

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ]
        }
    }
}
```


De volgende JSON is met de `verbose` parameter ingesteld op `true`:

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ],
            "$instance": {
                "email": [
                    {
                        "type": "builtin.email",
                        "text": "patti.owens@microsoft.com",
                        "startIndex": 31,
                        "length": 25,
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

Meer informatie over de [getal](luis-reference-prebuilt-number.md), [rangtelwoord](luis-reference-prebuilt-ordinal.md), en [percentage](luis-reference-prebuilt-percentage.md). 
