---
title: Woordgroepen lijsten-spraak service
titleSuffix: Azure Cognitive Services
description: Informatie over het leveren van de spraak Services met een woordgroepen lijst `PhraseListGrammar` met behulp van het object om herkennings resultaten voor spraak naar tekst te verbeteren.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: rhurey
ms.openlocfilehash: 0e552d502184d1b537263c2c1f6b2a8562cdf791
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562781"
---
# <a name="phrase-lists-for-speech-to-text"></a>Woordgroepen lijsten voor spraak naar tekst

Door de spraak Services met een woordgroepen lijst te bieden, kunt u de nauw keurigheid van spraak herkenning verbeteren. Woordgroepen lijsten worden gebruikt voor het identificeren van bekende woord groepen in audio gegevens, zoals de naam van een persoon of een specifieke locatie.

Als u bijvoorbeeld een opdracht ' verplaatsen naar ' en een mogelijke bestemming van ' naar onder ' hebt, kunt u een vermelding van ' verplaatsen naar binnen ' toevoegen. Als u een woord groep toevoegt, wordt de kans groter dat wanneer de audio wordt herkend dat ' naar binnen verplaatsen ' wordt herkend.

Enkele woorden of volledige woord groepen kunnen worden toegevoegd aan een woordgroepen lijst. Tijdens de herkenning wordt een vermelding in een woordgroepen lijst gebruikt als de audio een exacte overeenkomst bevat. Als de woordgroepen lijst in het vorige voor beeld is opgenomen, wordt ' verplaatsen naar de andere ' weer geven en de vastgelegde woord groep ' langzaam verplaatsen naar de andere '.

>[!Note]
> Op dit moment ondersteunt een woordgroepen lijst alleen het Engels voor spraak naar tekst.

## <a name="how-to-use-phrase-lists"></a>Woordgroepen lijsten gebruiken

In de onderstaande voor beelden ziet u hoe u een woordgroepen lijst `PhraseListGrammar` maakt met behulp van het object.

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
> Het maximum aantal woordgroepen lijsten dat door de spraak service wordt gebruikt om te voldoen aan spraak is 1024 zinsdelen.

U kunt ook de zinsdelen wissen die zijn gekoppeld `PhraseListGrammar` aan de door het aanroepen van Clear ().

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
> Wijzigingen in een `PhraseListGrammar` object worden van invloed op de volgende herkenning of na het opnieuw verbinden met de spraak Services.

## <a name="next-steps"></a>Volgende stappen

* [Naslag documentatie voor Speech SDK](speech-sdk.md)
