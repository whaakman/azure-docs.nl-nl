---
title: Vooraf gedefinieerde Keyphrase entiteit
titleSuffix: Azure
description: In dit artikel bevat keyphrase vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: a2bb486de0999ad61cf2fdfdfb57dd91d376f597
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206594"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>keyPhrase vooraf gedefinieerde entiteit voor een LUIS-app
keyPhrase haalt u een verscheidenheid aan sleuteltermen uit een utterance. U hoeft niet te voorbeeld utterances met keyPhrase naar de toepassing toevoegen. keyPhrase entiteit wordt ondersteund in [veel culturen](luis-language-support.md#languages-supported) als onderdeel van de [tekstanalyse](../text-analytics/overview.md) functies. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Oplossing voor vooraf gedefinieerde keyPhrase entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.keyPhrase** entiteit.

```json
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
