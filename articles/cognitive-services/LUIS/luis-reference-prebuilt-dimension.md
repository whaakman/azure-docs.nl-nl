---
title: Dimensie vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS) dimensie.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2018
ms.author: diberry
ms.openlocfilehash: ceed37a8482051bb0fae14c2fe1fe8a366c6a6f3
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57215333"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Dimensie vooraf gedefinieerde entiteit voor een LUIS-app
De vooraf gedefinieerde dimensie entiteit detecteert de verschillende typen dimensies, ongeacht de cultuur van LUIS-app. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met dimensies aan de toepassing intents. Dimensie entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Typen van de dimensie

Dimensie wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) GitHub-opslagplaats


## <a name="resolution-for-dimension-entity"></a>Oplossing voor de dimensie entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.dimension** entiteit.

```json
{
  "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.762141049
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.762141049
    }
  ],
  "entities": [
    {
      "entity": "10 1/2 miles",
      "type": "builtin.dimension",
      "startIndex": 19,
      "endIndex": 30,
      "resolution": {
        "unit": "Mile",
        "value": "10.5"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [e](luis-reference-prebuilt-email.md), [getal](luis-reference-prebuilt-number.md), en [rangtelwoord](luis-reference-prebuilt-ordinal.md) entiteiten. 
