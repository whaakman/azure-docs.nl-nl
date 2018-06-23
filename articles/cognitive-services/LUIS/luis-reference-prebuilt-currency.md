---
title: Vooraf gedefinieerde LUIS entiteiten valuta referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel valuta bevat vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3e20642f6734b0247d23db1a63317eb8b4a96b5e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321846"
---
# <a name="currency-entity"></a>Valuta entiteit
De vooraf gedefinieerde valuta entiteit detecteert valuta in veel coupures en landen, ongeacht de cultuur van LUIS app. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances met valuta naar de toepassing intents toevoegen. Valuta entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Soorten valuta
Valuta wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) Github-opslagplaats

## <a name="resolution-for-currency-entity"></a>Oplossing voor valuta entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.currency** entiteit.

```JSON
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