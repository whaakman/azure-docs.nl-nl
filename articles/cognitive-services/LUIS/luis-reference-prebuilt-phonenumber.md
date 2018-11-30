---
title: LUIS vooraf gemaakte entiteiten telefoon de referentie naar - Azure | Microsoft Docs
titleSuffix: Azure
description: In dit artikel bevat vooraf gedefinieerde entiteit telefoonnummergegevens in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 9a8fcbaf946f936d7a6d6d883a0416fce9d0c158
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441707"
---
# <a name="phonenumber-entity"></a>Telefoonnummerentiteit
De `phonenumber` entiteit extraheert tal van telefoonnummers, inclusief landcode. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld uitingen toevoegen aan de toepassing. De `phonenumber` entiteit wordt ondersteund in `en-us` alleen de cultuur. 

## <a name="types-of-phonenumber"></a>Typen van telefoonnummer
Telefoonnummer wordt beheerd via de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) Github-opslagplaats

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Oplossing voor vooraf gedefinieerde phonenumber entiteit
Het volgende voorbeeld ziet u de resolutie van de **builtin.phonenumber** entiteit.

```JSON
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