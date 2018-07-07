---
title: Inzicht in de voorspelling score geretourneerd door LUIS - Azure | Microsoft Docs
description: Informatie over wat de voorspelling score betekent LUIS
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 88d5eb22186248024a356610addab0d43f68a961
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887127"
---
# <a name="prediction-score"></a>Voorspellingsscore
Een voorspelling score geeft aan dat de mate van vertrouwen die LUIS voor voorspellingsresultaten heeft. 

De score van een voorspelling is meestal tussen nul (0) en één (1). Een voorbeeld van een zeer vertrouwen LUIS-score is 0,99. Een voorbeeld van een score van lage vertrouwen is 0,01. 

|Score-waarde|Betrouwbaarheid|
|--|--|
|1|goede overeenkomst|
|0,99|hoge betrouwbaarheid|
|0.01|lage vertrouwen|
|0|goede niet overeen met|

Wanneer een utterance in een laag-betrouwbaarheidsscore resulteert, LUIS markeert die in de [LUIS](luis-reference-regions.md) website **kunt u lezen wat** pagina, met de geïdentificeerde **met het label bedoelingen** die worden beschreven met rood. 

![Score discrepantie](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Scoring-bovenaan doel
Elke utterance voorspelling retourneert een scoring-bovenaan doel. Dit is een numerieke vergelijking van scores voorspelling. De eerste twee scores kunnen een klein verschil tussen deze twee hebben. LUIS wordt niet aangegeven deze nabijheid dan scores retourneren.  

Als u zich zorgen over de nabijheid van bovenste scores, moet u de score voor alle intents retourneren. U kunt ofwel uitingen toevoegen aan de twee intenties die wijzen op de verschillen met het woord keuze en schema's of u kunt beschikken over de toepassing LUIS-aanroepen, zoals een chatbot, programmatische keuzes over het afhandelen van de twee bovenste intents. 

## <a name="return-prediction-score-for-all-intents"></a>Voorspelling score voor alle intents retourneren
Resultaat van een test- of -eindpunt kan alle intents bevatten. Deze configuratie is ingesteld op de [eindpunt](https://aka.ms/v1-endpoint-api-docs) met de `verbose=true` query uitvoeren op de naam/waarde-paar tekenreeks. 

## <a name="review-intents-with-similar-scores"></a>Intents met vergelijkbare scores bekijken
Controleren van de score voor alle intents is een goede manier om te controleren dat niet alleen de juiste intentie wordt geïdentificeerd, maar die de komende aangegeven kunt u lezen wat de score zijn aanzienlijk lager is consistent voor uitingen. 

Als meerdere intents sluiten voorspelling scores op basis van de context van een utterance kan LUIS schakelen tussen de intents. U kunt dit verhelpen blijven uitingen toevoegen aan elk doel met een breder scala van contextuele verschillen.   

## <a name="e-exponent-notation"></a>De notatie E (exponent)

Voorspelling scores kunt exponent notatie *weergegeven* boven de 0-1 bereik, zoals `9.910309E-07`. Deze score geeft aan dat er een zeer **kleine** getal.

|E-notatie score |Werkelijke score|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>Verschillen met voorspellingen
Wanneer u hetzelfde model in een andere app trainen en de scores niet hetzelfde zijn, is dit omdat er een element van de onzekerheid in de training. Ten tweede betekent een overlapping van een utterance aan meer dan één kunt u lezen wat dat het belangrijkste doel voor de dezelfde utterance kunt wijzigen op basis van de training.

Als uw chatbot een specifieke LUIS score om aan te geven vertrouwen in opzet vereist, moet u in plaats daarvan de score verschil tussen de bovenste twee intents. Dit biedt flexibiliteit voor variaties in Trainingen. 

## <a name="next-steps"></a>Volgende stappen

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over entiteiten toevoegen aan uw LUIS-app.