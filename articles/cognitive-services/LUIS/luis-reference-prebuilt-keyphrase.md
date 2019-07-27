---
title: Vooraf samengestelde LUIS van de woordgroepen-entiteit
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat keyphrase vooraf gedefinieerde entiteitgegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: d7ecb579305cb17525e220bc16be430c77d25080
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563421"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>vooraf samengestelde woordgroepen instantie voor een LUIS-app
keyPhrase haalt u een verscheidenheid aan sleuteltermen uit een utterance. U hoeft niet te voorbeeld utterances met keyPhrase naar de toepassing toevoegen. keyPhrase entiteit wordt ondersteund in [veel culturen](luis-language-support.md#languages-supported) als onderdeel van de [tekstanalyse](../text-analytics/overview.md) functies. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Oplossing voor vooraf gedefinieerde keyPhrase entiteit

### <a name="api-version-2x"></a>API-versie 2. x

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
