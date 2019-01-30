---
title: LUIS vooraf gemaakte entiteiten e-referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel e-mailbericht bevat vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 080c4d08348265ea891dd02ff3aa2fe8ba2ad2f1
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221484"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>E-vooraf gedefinieerde entiteit voor een LUIS-app
Extractie van e-mailbericht bevat het volledige e-mailadres van een utterance. Omdat deze entiteit wordt al getraind, hoeft u niet om toe te voegen van de voorbeeld-uitingen met e-mailbericht naar de toepassing intents. E-entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="resolution-for-prebuilt-email"></a>Oplossing voor vooraf gemaakte e-mailadres
Het volgende voorbeeld ziet u de resolutie van de **builtin.email** entiteit.

```json
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [getal](luis-reference-prebuilt-number.md), [rangtelwoord](luis-reference-prebuilt-ordinal.md), en [percentage](luis-reference-prebuilt-percentage.md). 
