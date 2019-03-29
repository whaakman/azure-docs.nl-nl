---
title: Vooraf gedefinieerde valuta-eenheid
titleSuffix: Azure
description: In dit artikel bevat valuta vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 9efaaa6bdd0f2b51efca398464dbf08de56d831d
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579271"
---
# <a name="currency-prebuilt-entity-for-a-luis-app"></a>Valuta vooraf gedefinieerde entiteit voor een LUIS-app
De vooraf gedefinieerde valuta-eenheid detecteert valuta in veel nominale waarden en landen, ongeacht de cultuur van LUIS-app. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen voorbeeld uitingen met valuta voor de toepassing intents. Valuta entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Typen valuta
Valuta wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) GitHub-opslagplaats

## <a name="resolution-for-currency-entity"></a>Oplossing voor de valuta-eenheid
Het volgende voorbeeld ziet u de resolutie van de **builtin.currency** entiteit.

```json
{
  "query": "search for items under $10.99",
  "topScoringIntent": {
    "intent": "SearchForItems",
    "score": 0.926173568
  },
  "intents": [
    {
      "intent": "SearchForItems",
      "score": 0.926173568
    },
    {
      "intent": "None",
      "score": 0.07376878
    }
  ],
  "entities": [
    {
      "entity": "$10.99",
      "type": "builtin.currency",
      "startIndex": 23,
      "endIndex": 28,
      "resolution": {
        "unit": "Dollar",
        "value": "10.99"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [datetimeV2](luis-reference-prebuilt-datetimev2.md), [dimensie](luis-reference-prebuilt-dimension.md), en [e](luis-reference-prebuilt-email.md) entiteiten. 
