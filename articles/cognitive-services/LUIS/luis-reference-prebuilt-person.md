---
title: PersonName vooraf gemaakte entiteiten
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat personName vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: f5948dc32fe0d4655a3b5fc1520629535a323c9e
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882800"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>PersonName vooraf gedefinieerde entiteit voor een LUIS-app
De vooraf gedefinieerde personName entiteit detecteert de namen van personen. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen voorbeeld uitingen met personName naar de toepassing intents. personName entiteit wordt ondersteund in het Engels en Chinees [culturen](luis-reference-prebuilt-entities.md).

## <a name="resolution-for-personname-entity"></a>Oplossing voor personName entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.personName** entiteit.

```json
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