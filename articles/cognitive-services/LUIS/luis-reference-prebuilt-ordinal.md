---
title: LUIS vooraf gemaakte entiteiten rangtelwoord referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel bevat rangtelwoord vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: e80b9dc63accec43532116b81e6eb0b9bd52ee22
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335250"
---
# <a name="ordinal-entity"></a>Rangtelwoordentiteit
Rangtelwoord is een numerieke representatie van een object in een set: `first`, `second`, `third`. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met rangtelwoord voor de toepassing intents. Rangtelwoord entiteit wordt ondersteund in [veel culturen](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Typen volgnummer
Het rangtelwoord voor wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) Github-opslagplaats

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