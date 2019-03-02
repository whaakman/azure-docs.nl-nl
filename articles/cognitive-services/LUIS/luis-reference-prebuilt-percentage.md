---
title: Percentage vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat percentage vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2018
ms.author: diberry
ms.openlocfilehash: 7f2808913178eb1d9a3de78b9f4948ee8031e4f4
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57214415"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>Percentage vooraf gedefinieerde entiteit voor een LUIS-app
Percentage getallen kunnen worden weergegeven als delen, `3 1/2`, of als percentage, `2%`. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met percentage van de toepassing intents. Percentage entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Typen percentage
Percentage wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) GitHub-opslagplaats

## <a name="resolution-for-prebuilt-percentage-entity"></a>Oplossing voor vooraf gedefinieerde percentage entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.percentage** entiteit.

```json
{
  "query": "set a trigger when my stock goes up 2%",
  "topScoringIntent": {
    "intent": "SetTrigger",
    "score": 0.971157849
  },
  "intents": [
    {
      "intent": "SetTrigger",
      "score": 0.971157849
    }
  ],
  "entities": [
    {
      "entity": "2%",
      "type": "builtin.percentage",
      "startIndex": 36,
      "endIndex": 37,
      "resolution": {
        "value": "2%"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [rangtelwoord](luis-reference-prebuilt-ordinal.md), [getal](luis-reference-prebuilt-number.md), en [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten. 
