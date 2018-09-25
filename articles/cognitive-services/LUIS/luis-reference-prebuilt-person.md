---
title: PersonName vooraf gedefinieerde entiteit - LUIS Reference
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat personName vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 0bc9d59fa44125429bfcb4e6ca9e68d93b81c04d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950570"
---
# <a name="personname-entity"></a>PersonName entiteit
De vooraf gedefinieerde personName entiteit detecteert de namen van personen. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen voorbeeld uitingen met personName naar de toepassing intents. personName entiteit wordt ondersteund in het Engels en Chinees [culturen](luis-reference-prebuilt-entities.md).

## <a name="resolution-for-personname-entity"></a>Oplossing voor personName entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.personName** entiteit.

```JSON
{
  "query": "Is Jill Jones in Cairo?",
  "topScoringIntent": {
    "intent": "WhereIsEmployee",
    "score": 0.762141049
  }
  "entities": [
    {
      "entity": "Jill Jones",
      "type": "builtin.personName",
      "startIndex": 3,
      "endIndex": 12
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [e](luis-reference-prebuilt-email.md), [getal](luis-reference-prebuilt-number.md), en [rangtelwoord](luis-reference-prebuilt-ordinal.md) entiteiten. 