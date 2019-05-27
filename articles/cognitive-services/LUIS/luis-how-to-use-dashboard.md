---
title: Dashboard - Language Understanding
titleSuffix: Azure Cognitive Services
description: Intents herstellen met de analytics-dashboard, een gevisualiseerde rapportagetool in te voeren.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/22/2019
ms.author: diberry
ms.openlocfilehash: 055d113a2bc77f8de1b4b881718007c869470532
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236950"
---
# <a name="how-to-use-the-dashboard-to-improve-your-app"></a>Het gebruik van het Dashboard voor het verbeteren van uw app

Opsporen en oplossen van problemen met uw ervaren app-intents wanneer u de voorbeeld-uitingen gebruikt. Het dashboard toont de algemene app-gegevens, met belangrijke functies van intents die moeten worden hersteld. 

Bekijk de analyse een iteratief proces is, herhaald als u wijzigen en verbeteren van het model van uw Dashboard.

Deze pagina geen relevante analyse voor apps die geen alle uitingen voorbeeld in de intenties, ook wel _alleen-patroon_ apps. 

## <a name="what-issues-can-be-fixed-from-dashboard"></a>Welke problemen kan worden opgelost via dashboard?

De drie problemen die worden behandeld in het dashboard zijn:

|Probleem|Grafiekkleur|Uitleg|
|--|--|--|
|Gegevens imbalance|-|Dit gebeurt wanneer de hoeveelheid van voorbeeld-uitingen aanzienlijk varieert. Alle intents moeten beschikken over _ongeveer_ hetzelfde aantal uitingen van de voorbeeld -, behalve de intentie geen. Er mag slechts 10-15% van de totale hoeveelheid uitingen hebben in de app.<br><br> Als de gegevens imbalanced zijn, maar de nauwkeurigheid van de intentie hoger dan een bepaalde drempelwaarde komt is, wordt deze imbalance niet gerapporteerd als een probleem.<br><br>**Beginnen met dit probleem - de hoofdoorzaak van de andere problemen kan zijn.**|
|Onduidelijk voorspellingen|Oranje|Dit wordt weergegeven wanneer het belangrijkste doel en de volgende bedoeling scores dicht genoeg bij elkaar dat ze op de volgende training vanwege spiegelen kunnen [negatieve steekproeven](luis-how-to-train.md#train-with-all-data) of meer voorbeeld uitingen toegevoegd aan de bedoeling. |
|Onjuiste voorspellingen|Rood|Dit gebeurt wanneer een voorbeeld-utterance niet voor de gelabelde intentie (de bedoeling dat deel uitmaakt) wordt voorspeld.|

Juiste voorspellingen worden weergegeven met de kleur blauw.

Het dashboard ziet u deze problemen en leest u welke intents worden beïnvloed en stelt wat u moet doen om de app verbeteren. 

## <a name="before-app-is-trained"></a>Voordat de app wordt getraind. 

Voordat u de app en trainen bevat het dashboard suggesties voor oplossingen. Uw app om te zien van deze suggesties trainen.  

## <a name="check-your-publishing-status"></a>Controleer de status van uw publiceren

De **publicatiestatus** kaart bevat informatie over de actieve versie het laatst is gepubliceerd. 

Controleer of de actieve versie is de versie die u wilt herstellen. 

![Dashboard toont app externe services, regio's gepubliceerd en eindpunt treffers samengevoegd.](./media/luis-how-to-use-dashboard/analytics-card-1-shows-app-summary-and-endpoint-hits.png)

Dit ook ziet u een externe services, gepubliceerde regio's en eindpunt treffers samengevoegd. 

## <a name="review-training-evaluation"></a>Training-evaluatie bekijken

De **Training evaluatie** kaart bevat samengevoegde samenvatting van de algehele nauwkeurigheid van uw app per gebied. De score geeft aan dat de intentie kwaliteit. 

![De evaluatie Training kaart bevat het eerste gebied van informatie over de algehele nauwkeurigheid van uw app.](./media/luis-how-to-use-dashboard/analytics-card-2-shows-app-overall-accuracy.png)

De grafiek aangeeft dat de juiste voorspelde intents en de probleemgebieden met verschillende kleuren. Als u de app met de suggesties voor deze score toeneemt verbeteren. 

De voorgestelde oplossingen uit worden gescheiden door probleemtype en zijn de belangrijkste voor uw app. Als u liever wilt onderzoeken en oplossen van problemen per doel, gebruik de **[Intents met fouten](#intents-with-errors)** kaart aan de onderkant van de pagina. 

Elk probleemgebied heeft intents die moeten worden hersteld. Wanneer u de naam van de intentie, selecteert de **bedoeling** pagina wordt geopend met een filter toegepast op de uitingen. Dit filter kunt u zich kunt richten op de uitingen die het probleem veroorzaken.

### <a name="compare-changes-across-versions"></a>Wijzigingen van versies met elkaar vergelijken

Maak een nieuwe versie voordat u wijzigingen in de app. Voorbeeld-uitingen van het doel van de voorgestelde wijzigingen aanbrengen in de nieuwe versie vervolgens opnieuw trainen. Op de pagina Dashboard **Training evaluatie** -kaart, gebruikt u de **Show wijziging ten opzichte van getrainde versie** om de wijzigingen te vergelijken. 

![Wijzigingen van versies met elkaar vergelijken](./media/luis-how-to-use-dashboard/compare-improvement-across-versions.png)

### <a name="fix-version-by-adding-or-editing-example-utterances-and-retraining"></a>Versie oplossen door toe te voegen of voorbeeld uitingen bewerken en opnieuw trainen

De primaire methode voor het oplossen van uw app worden toevoegen of bewerken van de voorbeeld-uitingen en opnieuw trainen. De nieuwe of gewijzigde uitingen moeten volgen richtlijnen voor [gevarieerd uitingen](luis-concept-utterance.md).

Toevoegen van voorbeeld-uitingen moet worden uitgevoerd door iemand die:

* heeft een hoge mate van inzicht in wat uitingen in de verschillende intents zijn
* weet hoe uitingen in één doel kunnen worden verward met een ander doel
* kan bepalen of twee intents, die worden vaak verward met elkaar, moeten worden samengevoegd tot een enkel doel en de verschillende gegevens opgehaald uit met entiteiten

### <a name="patterns-and-phrase-lists"></a>Patronen en woordgroep lijsten

De pagina analytics wordt niet aangegeven wanneer u [patronen](luis-concept-patterns.md) of [lijsten woordgroep](luis-concept-feature.md). Als u ze toevoegt, kan helpen met onjuiste of onduidelijk voorspellingen maar kunt geen met gegevens imbalance. 

### <a name="review-data-imbalance"></a>Bekijk gegevens imbalance

Beginnen met dit probleem - de hoofdoorzaak van de andere problemen kan zijn.

De **gegevens imbalance** intentie lijst intents die meer uitingen nodig om te corrigeren van de imbalance gegevens bevat. 

**Om dit probleem te verhelpen**:

* Meer utterances toevoegen aan het doel en opnieuw trainen. 

Voeg geen uitingen op de intentie None, tenzij die wordt voorgesteld in het dashboard.

> [!Tip]
> Gebruik van de derde sectie op de pagina **uitingen per bedoeling** met de **uitingen (getal)** instellen als een snelle visual handleiding die intents meer uitingen nodig.  
    ![Gebruik 'Uitingen (getal)' intents met gegevens evenwicht te vinden.](./media/luis-how-to-use-dashboard/predictions-per-intent-number-of-utterances.png)

### <a name="review-incorrect-predictions"></a>Onjuiste voorspellingen bekijken

De **onjuist voorspelling** intentie lijst bevat intents waarvoor uitingen die worden gebruikt als voorbeelden voor een specifiek doel, maar voor verschillende intents worden voorspeld. 

**Om dit probleem te verhelpen**:

* Uitingen om meer specifieke zodat de intentie en train opnieuw bewerken.
* Combineer intents als uitingen te goed zijn uitgelijnd en opnieuw trainen.

### <a name="review-unclear-predictions"></a>Bekijk onduidelijk voorspellingen

De **onduidelijk voorspelling** intentie lijst bevat intents met uitingen met voorspelling scores die niet ver manier van hun dichtstbijzijnde rivaal, die het belangrijkste doel voor de utterance op de volgende training, vanwege wijzigen kan [ negatieve steekproeven](luis-how-to-train.md#train-with-all-data).

**Om dit probleem te verhelpen**;

* Uitingen om meer specifieke zodat de intentie en train opnieuw bewerken.
* Combineer intents als uitingen te goed zijn uitgelijnd en opnieuw trainen.

## <a name="utterances-per-intent"></a>Uitingen per doel

Deze kaart toont de algemene status van de app via de intents. Als u intents en retrain hebt opgelost, blijven in een oogopslag op deze kaart voor problemen.

Het volgende diagram toont een evenwichtige-app met bijna geen problemen op te lossen.

![Het volgende diagram toont een evenwichtige-app met bijna geen problemen op te lossen.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-balance.png)

Het volgende diagram ziet u een slecht met gelijke taakverdeling app waarin heel wat problemen op te lossen.

![Het volgende diagram toont een evenwichtige-app met bijna geen problemen op te lossen.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-imbalance.png)

Beweeg de muisaanwijzer over de balk voor informatie over het doel van elk doel. 

![Het volgende diagram toont een evenwichtige-app met bijna geen problemen op te lossen.](./media/luis-how-to-use-dashboard/utterances-per-intent-with-details-of-errors.png)

Gebruik de **sorteren op** functie voor het ordenen van de intenties van probleemtype, zodat u zich op de meest problematische intenties met het probleem richten kunt. 

## <a name="intents-with-errors"></a>Intents met fouten

Deze kaart kunt u om problemen voor een specifiek doel te bekijken. De standaardweergave van deze kaart is de meest problematische intenties, zodat u weet waar u uw inspanningen te richten.

![De intenties met fouten kaart kunt u om problemen voor een specifiek doel te bekijken. De kaart wordt gefilterd op de meest problematische intenties standaard, zodat u weet waar u uw inspanningen te richten.](./media/luis-how-to-use-dashboard/most-problematic-intents-with-errors.png)

De bovenste ringdiagram ziet u de problemen met het doel voor de drie typen. Als er problemen in de drie typen zijn, heeft elk type een eigen diagram hieronder, samen met eventuele andere intents. 

### <a name="filter-intents-by-issue-and-percentage"></a>Intents door het probleem en het percentage filteren

Deze sectie van de kaart kunt u voorbeeld-uitingen die zijn die buiten de drempelwaarde voor fout vinden. In het ideale geval wilt u de juiste voorspellingen significant. Percentage is bedrijfs- en basis. 

Bepaal de drempelwaarde percentages die u vertrouwd met voor uw bedrijf bent. 

Het filter kunt u vinden intents met specifieke probleem:

|Filteren|Voorgestelde percentage|Doel|
|--|--|--|
|Meest problematisch zijn eruit intents|-|**Begin hier** -de uitingen gecorrigeerd in deze kunt u lezen wat de app meer dan andere oplossingen wordt verbeterd.|
|Juiste voorspellingen hieronder|60%|Dit is het percentage van uitingen in het geselecteerde doel die juist zijn, maar hebben een betrouwbaarheidsscore lager dan de drempelwaarde. |
|Onduidelijk voorspellingen bovenstaande|15%|Dit is het percentage van uitingen in het geselecteerde doel die worden verward met de dichtstbijzijnde andere intentie.|
|Onjuiste voorspellingen bovenstaande|15%|Dit is het percentage van de geselecteerde intentie uitingen die onjuist worden voorspeld. |

### <a name="correct-prediction-threshold"></a>Juiste voorspelling drempelwaarde

Wat is een betrouwbaarheidsscore vertrouwen voorspelling voor u? 60% mogelijk aan het begin van app-ontwikkeling, het doel. Gebruik de **corrigeren voorspellingen onderstaande** met het percentage van 60% van alle uitingen vinden in het geselecteerde doel dat moet worden hersteld.

### <a name="unclear-or-incorrect-prediction-threshold"></a>Drempelwaarde voor voorspelling onduidelijk of is onjuist

Deze twee filters kunnen u uitingen niet vinden in het geselecteerde doel de drempel overschrijdt. U kunt deze twee percentages beschouwen als percentage van de fout. Als u vertrouwd met een frequentie van 10-15% fouten voor voorspellingen bent, stelt u de drempelwaarde filter 15% alle uitingen boven deze waarde vinden. 

## <a name="next-steps"></a>Volgende stappen

* [Uw Azure-resources beheren](luis-how-to-azure-subscription.md)
