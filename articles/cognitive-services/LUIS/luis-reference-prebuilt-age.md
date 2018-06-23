---
title: Vooraf gedefinieerde LUIS entiteiten na verloop van tijd referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat leeftijd vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 59732469cf0d1e55643f3977958ec34a887130d3
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321986"
---
# <a name="age-entity"></a>Leeftijd entiteit
De vooraf gedefinieerde leeftijd entiteit bevat de leeftijdswaarde zowel numeriek en in termen van dagen, weken, maanden en jaren. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances met de toepassing die een leeftijd jonger toevoegen. Leeftijd entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Typen leeftijd
Leeftijd wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) Github-opslagplaats

## <a name="resolution-for-prebuilt-age-entity"></a>Oplossing voor vooraf gedefinieerde leeftijd entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.age** entiteit.

```JSON
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