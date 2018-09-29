---
title: LUIS vooraf gemaakte entiteiten keyphrase referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel bevat keyphrase vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/09/2018
ms.author: diberry
ms.openlocfilehash: 4133b7c7c3fabbe92a3208c567d7b4c6c2c27283
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434395"
---
# <a name="keyphrase-entity"></a>keyPhrase-entiteit
keyPhrase haalt u een verscheidenheid aan sleuteltermen uit een utterance. U hoeft niet te voorbeeld utterances met keyPhrase naar de toepassing toevoegen. keyPhrase entiteit wordt ondersteund in [veel culturen](luis-language-support.md#languages-supported) als onderdeel van de [tekstanalyse](../text-analytics/overview.md) functies. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Oplossing voor vooraf gedefinieerde keyPhrase entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.keyPhrase** entiteit.

```JSON
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [percentage](luis-reference-prebuilt-percentage.md), [getal](luis-reference-prebuilt-number.md), en [leeftijd](luis-reference-prebuilt-age.md) entiteiten.
