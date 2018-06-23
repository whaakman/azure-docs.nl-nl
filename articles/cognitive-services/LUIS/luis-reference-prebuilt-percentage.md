---
title: Vooraf gedefinieerde LUIS entiteiten percentage referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel bevat percentage vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 4d502e93f8b6bf3af2da05a499c359faeabb51da
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321909"
---
# <a name="percentage-entity"></a>Percentage entiteit
Percentage getallen kunnen worden weergegeven als breuken, `3 1/2`, of als percentage, `2%`. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances percentage voor de toepassing die met toevoegen. Percentage entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Typen percentage
Percentage wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) Github-opslagplaats

## <a name="resolution-for-prebuilt-percentage-entity"></a>Oplossing voor vooraf gedefinieerde percentage entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.percentage** entiteit.

```JSON
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