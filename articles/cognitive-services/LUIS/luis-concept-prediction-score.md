---
title: Voorspellings scores-LUIS
titleSuffix: Azure Cognitive Services
description: Een Voorspellings score geeft de mate van betrouw baarheid van de LUIS API-service voor Voorspellings resultaten op basis van een gebruikers utterance.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 34ec5588a510574f4ea9f01bd23c6f6487e288da
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638352"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Voorspellings scores geven de nauw keurigheid van de voor spelling voor intentie en entiteiten aan

Een Voorspellings score geeft aan dat de mate van betrouw baarheid LUIS heeft voor Voorspellings resultaten op basis van een gebruikers utterance.

De score van een voorspelling is tussen de nul (0) en één (1). Een voorbeeld van een zeer vertrouwen LUIS-score is 0,99. Een voorbeeld van een score van lage vertrouwen is 0,01. 

|Score-waarde|Betrouwbaarheid|
|--|--|
|1|goede overeenkomst|
|0,99|hoge betrouwbaarheid|
|0.01|lage vertrouwen|
|0|goede niet overeen met|

Wanneer een utterance in een laag-betrouwbaarheidsscore resulteert, LUIS markeert die in de [LUIS](luis-reference-regions.md) website **kunt u lezen wat** pagina, met de geïdentificeerde **met het label bedoelingen** die worden beschreven met rood.

![Score discrepantie](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Scoring-bovenaan doel

Elke utterance voorspelling retourneert een scoring-bovenaan doel. Deze voor spelling is een numerieke vergelijking van de Voorspellings scores. De bovenste twee scores kunnen een zeer klein verschil hebben. LUIS geeft deze nabijheid niet aan, anders dan het retour neren van de bovenste Score.  

## <a name="return-prediction-score-for-all-intents"></a>Voorspelling score voor alle intents retourneren

Resultaat van een test- of -eindpunt kan alle intents bevatten. Deze configuratie is ingesteld op de [eindpunt](https://aka.ms/v1-endpoint-api-docs) met de `verbose=true` query uitvoeren op de naam/waarde-paar tekenreeks.

## <a name="review-intents-with-similar-scores"></a>Intents met vergelijkbare scores bekijken

Controleren van de score voor alle intents is een goede manier om te controleren dat niet alleen de juiste intentie wordt geïdentificeerd, maar die de komende aangegeven kunt u lezen wat de score zijn aanzienlijk lager is consistent voor uitingen.

Als meerdere intents sluiten voorspelling scores op basis van de context van een utterance kan LUIS schakelen tussen de intents. Om deze situatie op te lossen, gaat u door met het toevoegen van uitingen aan elke intentie met een breder scala aan contextuele verschillen of kunt u de client toepassing, zoals een chat-bot, programmatische keuzes maken over het afhandelen van de twee belangrijkste intenties.

De twee intenten, die te nauw keurig zijn, kunnen worden tegengehouden als gevolg van niet-deterministische training. De hoogste score kan de tweede boven en de tweede belangrijkste score kan worden de eerste hoogste score. Om deze situatie te voor komen, voegt u bijvoorbeeld uitingen toe aan elk van de twee bovenste doel stellingen voor die utterance met de keuze en context van het woord dat de 2 intentie onderscheidt. De twee intenties moeten over hetzelfde aantal voorbeeld uitingen hebben. Vuistregel voor scheiding om te voorkomen dat tekenomkering vanwege training is een verschil 15% in scores.

U kunt de niet-deterministische training uitschakelen door te [trainen op alle gegevens](luis-how-to-train.md#train-with-all-data).

## <a name="differences-with-predictions-between-different-training-sessions"></a>Verschillen met voor spellingen tussen verschillende trainings sessies

Wanneer u hetzelfde model in een andere app traint en de scores zijn niet hetzelfde, is dit verschil omdat er niet-deterministische training (een element van wille keurigheid) is. Ten tweede betekent een overlapping van een utterance aan meer dan één kunt u lezen wat dat het belangrijkste doel voor de dezelfde utterance kunt wijzigen op basis van de training.

Als uw chat-bot een specifieke LUIS Score vereist om het vertrouwen in een intentie aan te geven, moet u het Score verschil tussen de twee beste doel stellingen gebruiken. Deze situatie biedt mogelijkheden voor variaties in training.

## <a name="e-exponent-notation"></a>De notatie E (exponent)

Voorspelling scores kunt exponent notatie *weergegeven* boven de 0-1 bereik, zoals `9.910309E-07`. Deze score geeft aan dat er een zeer **kleine** getal.

|E-notatie score |Werkelijke score|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="punctuation"></a>Leestekens

Meer [informatie](luis-concept-utterance.md#punctuation-marks) over het gebruik of negeren van interpunctie. 

## <a name="next-steps"></a>Volgende stappen

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.
