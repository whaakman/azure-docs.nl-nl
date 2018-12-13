---
title: De ouderdom van vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat leeftijd vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 88d2633a107f36c7c0eab8803a3b6ea10e067506
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166537"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>De ouderdom van vooraf gedefinieerde entiteit voor een LUIS-app
De vooraf gedefinieerde leeftijd entiteit bevat de leeftijdwaarde zowel numeriek en in termen van dagen, weken, maanden en jaren. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met leeftijd aan de toepassing intents. Leeftijd entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Typen leeftijd
Leeftijd wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) GitHub-opslagplaats

## <a name="resolution-for-prebuilt-age-entity"></a>Oplossing voor vooraf gedefinieerde leeftijd entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.age** entiteit.

```json
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8236133
    }
  ],
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [valuta](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), en [dimensie](luis-reference-prebuilt-dimension.md) entiteiten. 