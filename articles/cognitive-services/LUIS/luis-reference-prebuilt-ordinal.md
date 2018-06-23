---
title: Vooraf gedefinieerde LUIS entiteiten verwijzingsnummer - Azure | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat informatie rangtelwoord vooraf gedefinieerde entiteit in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 2ff9b083e6cabe455baea3ed777dd6cc00b6fbfe
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321965"
---
# <a name="ordinal-entity"></a>Rangtelwoord entiteit
Rangtelwoord is een numerieke representatie van een object in een set: `first`, `second`, `third`. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances met rangtelwoord voor de toepassing intents toevoegen. Rangtelwoord entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Typen rangtelwoord
Rangtelwoord wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) Github-opslagplaats

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Oplossing voor vooraf gedefinieerde rangtelwoord entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.ordinal** entiteit.

```JSON
{
  "query": "Order the second option",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.9993253
  },
  "intents": [
    {
      "intent": "OrderFood",
      "score": 0.9993253
    },
    {
      "intent": "None",
      "score": 0.05046708
    }
  ],
  "entities": [
    {
      "entity": "second",
      "type": "builtin.ordinal",
      "startIndex": 10,
      "endIndex": 15,
      "resolution": {
        "value": "2"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [percentage](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md), en [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten. 