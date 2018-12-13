---
title: Telefoon aantal vooraf gemaakte entiteiten
titleSuffix: Azure
description: In dit artikel bevat vooraf gedefinieerde entiteit telefoonnummergegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 3a538886b0137e1069af1fa680374b749a9b3f92
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165403"
---
# <a name="phonenumber-prebuilt-entity-for-a-luis-app"></a>Telefoonnummer vooraf gedefinieerde entiteit voor een LUIS-app
De `phonenumber` entiteit extraheert tal van telefoonnummers, inclusief landcode. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld uitingen toevoegen aan de toepassing. De `phonenumber` entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="types-of-phonenumber"></a>Typen van telefoonnummer
Telefoonnummer wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) GitHub-opslagplaats

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Oplossing voor vooraf gedefinieerde phonenumber entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.phonenumber** entiteit.

```json
{
  "query": "my mobile is 00 44 161 1234567",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [percentage](luis-reference-prebuilt-percentage.md), [getal](luis-reference-prebuilt-number.md), en [temperatuur](luis-reference-prebuilt-temperature.md) entiteiten. 