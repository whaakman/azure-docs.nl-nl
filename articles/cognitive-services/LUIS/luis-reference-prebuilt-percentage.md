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
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: b55aca79047a224a9e1a474afdabf43e2701872d
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57341739"
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
