---
title: Vooraf gedefinieerde LUIS entiteiten e-referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: Dit artikel e-mailbericht bevat vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 517e92202bdd5bc8d970306b9e24999fa62a8e43
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321867"
---
# <a name="email-entity"></a>E-entiteit
Uitpakken van het e-mailbericht bevat het volledige e-mailadres van een utterance. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances met e-mailbericht naar de toepassing intents toevoegen. E-entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="resolution-for-prebuilt-email"></a>Oplossing voor vooraf gemaakte e-mail
Het volgende voorbeeld ziet u de resolutie van de **builtin.email** entiteit.

```JSON
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