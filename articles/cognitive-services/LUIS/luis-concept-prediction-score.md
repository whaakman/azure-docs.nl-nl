---
title: Inzicht in de voorspelling score geretourneerd door LUIS - Azure | Microsoft Docs
description: Meer informatie over wat de score voorspelling betekent LUIS
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 1620fe80b032245c6ca19279c3981dcff4b9820f
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345968"
---
# <a name="prediction-score"></a>Voorspelling score
Een score voorspelling geeft aan dat de mate van betrouwbaarheid die LUIS voor de voorspelling resultaten heeft. 

De score van een voorspelling is meestal tussen nul (0) en één (1). Een voorbeeld van een maximaal vertrouwen LUIS score is 0,99. Een voorbeeld van een score lage vertrouwen is 0,01. 

|Score-waarde|Betrouwbaarheid|
|--|--|
|1|definitieve overeenkomst|
|0,99|hoge betrouwbaarheid|
|0.01|lage vertrouwen|
|0|definitieve niet overeen met|

Wanneer een utterance in een score laag vertrouwen resulteert, LUIS markeert die in de [LUIS] [ LUIS] website **bedoeling** pagina met de geïdentificeerde **gelabeld bedoelingen**  met rode beschreven. 

![Score discrepantie](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Score berekenen voor Top doel
Elke voorspelling utterance retourneert een score berekenen voor top doel. Dit is een numerieke vergelijking van scores voorspelling. De eerste twee scores kunnen een klein verschil tussen deze twee hebben. LUIS wordt niet aangegeven deze nabijheid dan scores retourneren.  

Als u zich zorgen over nabijheid van bovenste scores, moet u de score voor alle intents retourneren. U kunt ofwel utterances toevoegen aan de twee intents die wijzen op de verschillen met woord keuze en regeling of u kunt beschikken over de toepassing LUIS aanroepen, zoals een chatbot, programmatische keuzes over het afhandelen van de twee bovenste intents maken. 

## <a name="return-prediction-score-for-all-intents"></a>Score voorspelling voor alle intents retourneren
Een test- en eindpunt resultaat kan bestaan uit alle intents. Deze configuratie is ingesteld op de [eindpunt](https://aka.ms/v1-endpoint-api-docs) met de `verbose=true` query uitvoeren op de naam/waarde-paar tekenreeks. 

## <a name="review-intents-with-similar-scores"></a>Bekijk intents met vergelijkbare scores
Controleren van de score voor alle intents is een uitstekende manier om te controleren dat niet alleen de juiste bedoeling wordt geïdentificeerd, maar dat de volgende geïdentificeerd score van de bedoeling is aanzienlijk minder consistent voor utterances. 

Als meerdere intents sluiten voorspelling scores op basis van de context van een utterance hebt mogelijk LUIS schakelen tussen de intents. U kunt dit verhelpen blijven utterances toevoegen aan elk doel met een breder scala van contextuele verschillen.   

## <a name="e-exponent-notation"></a>Notatie voor E (exponent)

Voorspelling scores kunt exponent notatie *verschijnen* boven het 0-1 bereik, zoals `9.910309E-07`. Deze score geeft aan dat er een zeer **kleine** getal.

|E-notatie score |Werkelijke score|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>Verschillen met voorspellingen
Wanneer u hetzelfde model in een andere app trainen en de scores niet hetzelfde zijn, is dit omdat er een element van aanvraaggrootte in de training. Ten tweede betekent een overlapping van een utterance naar meer dan één doel dat het belangrijkste doel voor de dezelfde utterance kunt wijzigen op basis van training.

Als uw chatbot een specifieke LUIS score om aan te geven van vertrouwen in de opzet vereist, moet u in plaats daarvan het score verschil tussen de eerste twee intents. Dit biedt flexibiliteit voor variaties in training. 

## <a name="next-steps"></a>Volgende stappen

Zie [entiteiten toevoegen](luis-how-to-add-entities.md) voor meer informatie over het toevoegen van entiteiten aan uw app LUIS.

[LUIS]:luis-reference-regions.md