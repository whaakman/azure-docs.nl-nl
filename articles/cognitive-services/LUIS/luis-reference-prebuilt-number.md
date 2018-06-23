---
title: Vooraf gedefinieerde LUIS entiteiten nummer verwijzing - Azure | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat een aantal vooraf gedefinieerde entiteitsgegevens in Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: aa0b389a0694a3b742259fd42bed08055fbbadbe
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321860"
---
# <a name="number-entity"></a>Aantal entiteit
Er zijn veel manieren waarin numerieke waarden die worden gebruikt om te kwantificeren, express en beschrijven stukjes informatie. Dit artikel behandelt alleen enkele mogelijke voorbeelden. LUIS interpreteert de verschillen in gebruiker utterances en consistente numerieke waarden als resultaat. Omdat deze entiteit wordt al getraind, hoeft u geen voorbeeld utterances getal tot de toepassing die met toevoegen. 

## <a name="types-of-number"></a>Typen getal
Nummer wordt beheerd vanaf de [kenmerken tekst](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) Github-opslagplaats

## <a name="examples-of-number-resolution"></a>Voorbeelden van nummer resolutie

| utterance        | Entiteit   | Oplossing |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS omvat de herkende waarde van een **`builtin.number`** entiteit in de `resolution` veld van het JSON-antwoord geretourneerd.

## <a name="resolution-for-prebuilt-number"></a>Oplossing voor vooraf gedefinieerde getal
Het volgende voorbeeld ziet een JSON-reactie van LUIS, die de resolutie van de waarde van 24, voor de utterance "twintig" bevat.

```JSON
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [valuta](luis-reference-prebuilt-currency.md), [rangtelwoord](luis-reference-prebuilt-ordinal.md), en [percentage](luis-reference-prebuilt-percentage.md). 