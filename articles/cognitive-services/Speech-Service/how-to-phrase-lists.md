---
title: Woordgroep lijsten - spraakservices
titlesuffix: Azure Cognitive Services
description: Meer informatie over het opgeven van de Speech Services met een lijst met woorden met de `PhraseListGrammar` object voor het verbeteren van de resultaten van spraak-naar-tekst.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 5/02/2019
ms.author: rhurey
ms.openlocfilehash: a3be5d28ebe394771a2d8b492f1f6a9c8a82fb9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66515312"
---
# <a name="phrase-lists-for-speech-to-text"></a>Woordgroep lijsten voor spraak-naar-tekst

Door op te geven de Speech Services met een woordgroep-lijst, kunt u de nauwkeurigheid van de spraakherkenning verbeteren. Een lijst met woorden worden gebruikt voor identificatie van bekende zinnen in audiogegevens, zoals de naam van een persoon of een specifieke locatie.

Een voorbeeld: als u een opdracht 'Naar' en een mogelijke doel van 'Ward"die kan worden uitgesproken, kunt u een vermelding van 'Verplaatsen naar Ward' toevoegen. Een wachtwoordzin toe te voegen, wordt de kans groter dat wanneer de audio wordt herkend dat "Verplaatsen naar Ward" in plaats van "Verplaatsen naar" wordt herkend.

Enkele woorden of volledige zinnen kunnen worden toegevoegd aan een woordgroep-lijst. Tijdens de opname, een vermelding in een woordgroepenlijst met gebruikt als een exacte overeenkomst is opgenomen in de audio. Bouwen op het vorige voorbeeld, als de lijst met woorden bevat 'Verplaatsen naar Ward' en de woordgroep vastgelegd is "Verplaatsen naar langzaam" en vervolgens het herkenningsresultaat worden 'Verplaatsen naar Ward langzaam'.

## <a name="how-to-use-phrase-lists"></a>Het gebruik van woorden bevat

De onderstaande voorbeelden laten zien over het bouwen van een woordgroepenlijst met met de `PhraseListGrammar` object.

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

>[!Note]
> Het maximum aantal woordgroep lijsten die de spraak-Service wordt gebruikt zodat deze overeenkomen met spraak is 1024 zinnen.

U kunt ook schakelt u de items die zijn gekoppeld aan de `PhraseListGrammar` door aanroepen clear().

```C++
phraselist->Clear();
```

```cs
phraseList.Clear();
```

```Python
phrase_list_grammar.clear()
```

```JavaScript
phraseListGrammar.clear();
```

```Java
phraseListGrammar.clear();
```

> [!NOTE]
> Wijzigingen in een `PhraseListGrammar` nemen van invloed op de volgende opname of na een opnieuw verbinding maken met de Speech Services-object.

## <a name="next-steps"></a>Volgende stappen

* [Spraak SDK-referentiedocumentatie](speech-sdk.md)
