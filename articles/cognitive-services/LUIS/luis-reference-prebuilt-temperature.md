---
title: Vooraf gedefinieerde temperatuur-entiteit
titleSuffix: Azure
description: In dit artikel bevat temperatuur vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 348030d888383c497d80259b279056d8ff892bfe
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165517"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Temperatuur vooraf gedefinieerde entiteit voor een LUIS-app
Temperatuur extraheert verschillende typen temperatuur. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met temperatuur tot de toepassing. Temperatuur entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Typen temperatuur
Temperatuur wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) GitHub-opslagplaats

## <a name="resolution-for-prebuilt-temperature-entity"></a>Oplossing voor vooraf gedefinieerde temperatuur entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.temperature** entiteit.

```json
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [percentage](luis-reference-prebuilt-percentage.md), [getal](luis-reference-prebuilt-number.md), en [leeftijd](luis-reference-prebuilt-age.md) entiteiten. 