---
title: Entiteits type van reguliere expressie-LUIS
titleSuffix: Azure Cognitive Services
description: Een reguliere expressie is het meest geschikt voor onbewerkte utterance-tekst. Deze aanvraag wordt genegeerd en worden genegeerd culturele variant.  Reguliere expressie die overeenkomt met is na spellingcontrole wijzigingen op het niveau van het teken, niet het niveau van de token toegepast.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 82cce359f2161800c53ccce7cdb0342bba759d43
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559946"
---
# <a name="regular-expression-entity"></a>Een entiteit in de vorm van een reguliere expressie 

Een reguliere expressie-entiteit extraheert een entiteit op basis van een reguliere expressie patroon dat u opgeeft.

Een reguliere expressie is het meest geschikt voor onbewerkte utterance-tekst. Deze aanvraag wordt genegeerd en worden genegeerd culturele variant.  Reguliere expressie die overeenkomt met is na spellingcontrole wijzigingen op het niveau van het teken, niet het niveau van de token toegepast. Als de reguliere expressie te complex is, zoals het gebruik van een groot aantal haken, kunt u de expressie niet toevoegen aan het model. Maakt gebruik van een deel, maar niet alle [.net regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) -bibliotheek. 

**De entiteit is goed geschikt wanneer:**

* De gegevens zijn consistent opgemaakt met elke variatie die ook consistent is.
* De reguliere expressie heeft niet meer dan 2 geneste niveaus nodig. 

![Een entiteit in de vorm van een reguliere expressie](./media/luis-concept-entities/regex-entity.png)

## <a name="usage-considerations"></a>Gebruiks overwegingen

Reguliere expressies kunnen overeenkomen met meer dan verwacht. Een voor beeld hiervan is een numerieke woord overeenkomst zoals `one` en `two`. Een voor beeld is de volgende regex die overeenkomt met `one` het nummer en andere getallen:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
``` 

Deze regex-expressie komt ook overeen met een wille keurig woord dat met `phone`deze getallen eindigt, zoals. Als u problemen wilt oplossen, moet u ervoor zorgen dat de regex-overeenkomsten rekening houden met woord grenzen. De regex voor het gebruik van woord grenzen voor dit voor beeld wordt gebruikt in de volgende regex:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

### <a name="example-json"></a>Voorbeeld van JSON

Als de `kb[0-9]{6}`definitie van de reguliere expressie-entiteit wordt gebruikt, is de volgende JSON-reactie een voor beeld van een utterance met de geretourneerde reguliere expressie-entiteiten voor de query: `When was kb123456 published?`

```JSON
{
  "query": "when was kb123456 published?",
  "topScoringIntent": {
    "intent": "FindKBArticle",
    "score": 0.933641255
  },
  "intents": [
    {
      "intent": "FindKBArticle",
      "score": 0.933641255
    },
    {
      "intent": "None",
      "score": 0.04397359
    }
  ],
  "entities": [
    {
      "entity": "kb123456",
      "type": "KB number",
      "startIndex": 9,
      "endIndex": 16
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

In deze [zelf studie](luis-quickstart-intents-regex-entity.md)maakt u een app om consistent opgemaakte gegevens te extra heren uit een utterance met behulp van de **reguliere expressie** -entiteit.