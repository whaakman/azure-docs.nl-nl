---
title: Url-verwijzing LUIS vooraf gemaakte entiteiten - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel bevat een url vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 62872a3fd3e6e830e25fc4cdba01d1f32b80db91
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446517"
---
# <a name="url-entity"></a>URL-entiteit
URL-entiteit extraheert URL's met domeinnamen of IP-adressen. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met URL's naar de toepassing. URL-entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="types-of-urls"></a>Typen van URL 's
URL wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) Github-opslagplaats

## <a name="resolution-for-prebuilt-url-entity"></a>Oplossing voor vooraf gedefinieerde URL-entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.url** entiteit.

```JSON
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [rangtelwoord](luis-reference-prebuilt-ordinal.md), [getal](luis-reference-prebuilt-number.md), en [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten.