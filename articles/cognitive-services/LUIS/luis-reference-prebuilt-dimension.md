---
title: Dimensie vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS) dimensie.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: fcd90d42f297cec86b32f9a806df4a148e6a93af
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214531"
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
