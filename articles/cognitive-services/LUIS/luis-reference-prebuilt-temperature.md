---
title: Vooraf gedefinieerde LUIS entiteiten temperatuur referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat temperatuur vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3cc16e7ec87775407c4261655d8f680cc0903e81
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321839"
---
# <a name="temperature-entity"></a>Temperatuur entiteit
Temperatuur extraheert verschillende typen temperatuur. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances met temperatuur aan de toepassing toevoegen. Temperatuur entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Typen temperatuur
Temperatuur wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) Github-opslagplaats

## <a name="resolution-for-prebuilt-temperature-entity"></a>Oplossing voor vooraf gedefinieerde temperatuur entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.temperature** entiteit.

```JSON
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