---
title: Url-verwijzing LUIS vooraf gemaakte entiteiten - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel bevat een url vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 4eacf564a295a568a3e2c8d2f44ad0af3fbbe258
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321958"
---
# <a name="url-entity"></a>URL-entiteit
URL-entiteit extraheert URL's met domeinnamen of IP-adressen. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances met URL's naar de toepassing toevoegen. URL-entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="types-of-urls"></a>URL 's
De URL wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) Github-opslagplaats

## <a name="resolution-for-prebuilt-url-entity"></a>Oplossing voor vooraf gedefinieerde URL entiteit
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