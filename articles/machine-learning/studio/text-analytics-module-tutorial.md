---
title: Het maken van de tekst analytics-modellen in Azure Machine Learning Studio | Microsoft Docs
description: Het maken van de tekst analytics-modellen in Azure Machine Learning Studio gebruik van modules voor voorverwerking van de tekst, N g of hash-functies
services: machine-learning
documentationcenter: 
author: rastala
manager: jhubbard
editor: 
ms.assetid: 08cd6723-3ae6-4e99-a924-e650942e461b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: roastala
ms.openlocfilehash: a43dc7162be4b2f3cfd122c6a87921caf2e3e418
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Tekstanalysemodellen maken in Azure Machine Learning Studio
U kunt Azure Machine Learning gebruiken om te bouwen en text analytics-modellen operationeel maken. Deze modellen kunt u, bijvoorbeeld document classificatie of gevoel analysis problemen oplossen.

Een text analytics-experiment zou u meestal:

1. Reinigen en tekst gegevensset voorverwerken
2. Numerieke functie vectoren extraheren uit tekst voorverwerkte
3. De classificatie- of regressiemodel model trainen
4. Beoordelen en valideren van het model
5. Het model implementeren naar productie

In deze zelfstudie leert u deze stappen als we doorlopen een gevoel Analytics-model met Amazon adresboek beoordelingen gegevensset (Zie het onderzoeksrapport ' biografieën, Bollywood, giek vakken en Blenders: domein aanpassing voor gevoel classificatie ' door John Blitzer, Markeren Dredze en Fernando Pereira; Koppeling van rekenkundige Linguistics (ACL) 2007.) Deze gegevensset bestaat uit revisie scores (1-2 of 4 en 5) en vrije tekst. Het doel is het voorspellen van de score revisie: lage (1 - 2) of hoge (4-5).

U vindt experimenten in deze zelfstudie wordt besproken in AI-galerie van Azure:

[Beoordelingen adresboek voorspellen](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Adresboek beoordelingen - Voorspellend Experiment voorspellen](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Stap 1: Opschonen en tekst gegevensset voorverwerken
We beginnen het experiment met het verdelen van de scores controleren in categorische hoge en lage buckets voor het probleem als tweeklasse classificatie formuleren. We gebruiken [metagegevens bewerken](https://msdn.microsoft.com/library/azure/dn905986.aspx) en [Categorische waarden](https://msdn.microsoft.com/library/azure/dn906014.aspx) modules.

![Label maken](./media/text-analytics-module-tutorial/create-label.png)

Vervolgens opschonen van de tekst met [voorverwerken tekst](https://msdn.microsoft.com/library/azure/mt762915.aspx) module. Het schoonmaken vermindert de ruis in de gegevensset, kunt u informatie over de belangrijkste functies en de nauwkeurigheid van het laatste model verbeteren. We verwijderen stopwoorden - veelvoorkomende woorden zoals ' "of"a"- en getallen, speciale tekens, dubbele tekens, e-mailadressen en URL's. We ook tekst naar kleine letters, de woorden lemmatize converteren en detecteren zin grenzen die vervolgens worden aangeduid met ' ||| ' symbool in voorverwerkte tekst.

![Tekst voorverwerken](./media/text-analytics-module-tutorial/preprocess-text.png)

Wat gebeurt er als u wilt gebruiken een aangepaste lijst met stopwoorden? U kunt in doorgeven als optionele invoer. U kunt ook aangepaste C# syntaxis van de reguliere expressie gebruiken subtekenreeksen vervangen en verwijder woorden door spraak: woorden of termen bijvoeglijke naamwoorden.

Nadat de voorverwerking voltooid is, we de gegevens splitsen in train en sets testen.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Stap 2: Haal de numerieke functie vectoren uit voorverwerkte tekst
Een model voor tekstgegevens bouwen, hebt u doorgaans vrije tekst converteren naar numerieke functie vectoren. In dit voorbeeld gebruiken we [N-Gram-functies extraheren uit tekst](https://msdn.microsoft.com/library/azure/mt762916.aspx) module voor het transformeren van de gegevens naar deze indeling. Deze module wordt een kolom met witruimte gescheiden woorden en berekent een dictionary van woorden of N-grams van woorden die worden weergegeven in uw gegevensset. Vervolgens wordt dit geteld hoeveel keren elk woord of N-gram wordt weergegeven in elke record en functie vectoren maakt van de telt. In deze zelfstudie we ingesteld N-gram-grootte op 2, dus onze vectoren functie enkele woorden en combinaties van de twee volgende woorden bevatten.

![N-grams extraheren](./media/text-analytics-module-tutorial/extract-ngrams.png)

We beginnen met TF toepassen * IDF (Term frequentie Inverse Document frequentie) weging van N-gram telt. Deze aanpak wordt gewicht van woorden die vaak worden weergegeven in één record, maar door de volledige gegevensset zeldzame worden toegevoegd. Andere opties omvatten binair TF, en wegen grafiek.

Dergelijke tekstfuncties hebben vaak hoge dimensionaliteit. Bijvoorbeeld, als uw corpus 100.000 unieke woorden heeft, moet de ruimte van de functie 100.000 dimensies of meer als N g worden gebruikt. De module extraheren N-Gram-functies kunt u een set opties voor het beperken van de dimensionaliteit. U kunt woorden die korte of lange, of te vaak of te vaak aanzienlijke voorspellende waarde uitsluiten. We uitsluiten N-grams die worden weergegeven in minder dan 5 records of in meer dan 80% van de records in deze zelfstudie.

U kunt ook Functieselectie gebruiken alleen de functies die het meest gecorreleerde met het doel-voorspelling selecteren. We gebruiken chi-kwadraatverdeling Functieselectie om 1000 onderdelen te selecteren. U kunt de woordenlijst van geselecteerde woorden of N-grams weergeven door te klikken op de juiste uitvoer van Extract N-gram-module.

Als een alternatieve methode om het gebruik van functies met N-Gram extraheren, kunt u hash-functies-module. Let echter [hash-functie](https://msdn.microsoft.com/library/azure/dn906018.aspx) heeft geen ingebouwde functie selectie mogelijkheden of TF * IDF weging.

## <a name="step-3-train-classification-or-regression-model"></a>Stap 3: De classificatie- of regressiemodel model trainen
De tekst is nu getransformeerd aan functie voor numerieke kolommen. De gegevensset bevat nog steeds tekenreekskolommen uit de vorige fasen, zodat we kolommen selecteren in de gegevensset wilt gebruiken.

Vervolgens gebruiken we [Two-Class Logistic Regression](https://msdn.microsoft.com/library/azure/dn905994.aspx) te voorspellen onze doel: hoog of laag revisie score. Op dit moment is het probleem van tekst analytics is omgezet in een reguliere klassificatieprobleem. U kunt de hulpprogramma's die beschikbaar zijn in Azure Machine Learning gebruiken het model te verbeteren. Bijvoorbeeld, kunt u experimenteren met verschillende classificaties om erachter te komen hoe nauwkeurige resultaten ze geven of hyperparameter afstemmen om de nauwkeurigheid te verbeteren.

![Trein en Score](./media/text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Stap 4: Beoordelen en valideren van het model
Hoe wilt u het getrainde model valideren? We deze tegen de testgegevensset te beoordelen en evalueren van de nauwkeurigheid. Het model geleerd echter de woordenlijst van N-grams en hun gewichten uit de gegevensset training. Daarom moeten we die vocabulair en gemeenschappelijke die gewichten gebruiken bij het uitpakken van de functies van testgegevens in plaats van de woordenlijst opnieuw maken. Daarom we N-Gram-functies extraheren module toevoegen aan de score vertakking van het experiment, verbinding maken met de uitvoer-woordenlijst training vertakken en de modus woordenlijst instellen op alleen-lezen. We ook het filteren van N-grams door frequentie door de minimale in te stellen op 1 exemplaar en maximaal 100% uitschakelen en de Functieselectie uitschakelen.

Nadat de kolom in de testgegevens is getransformeerd aan functie voor numerieke kolommen, uitsluiten we van de tekenreekskolommen vorige fasen, zoals in training vertakking. We gebruiken de module Score Model te maken van voorspellingen en de module Evaluate Model om te evalueren, de nauwkeurigheid.

## <a name="step-5-deploy-the-model-to-production"></a>Stap 5: Het model implementeren naar productie
Het model is bijna klaar om te worden geïmplementeerd naar productie. Als geïmplementeerd als webservice, wordt tekst tekenreeks als invoer en retourneren een voorspelling 'hoog' of 'laag'. De geleerde N-gram-woordenlijst wordt gebruikt voor het transformeren van de tekst die moet de functies en getraind logistic regressiemodel om een voorspelling van deze functies te maken. 

Als u de Voorspellend experiment instelt, opslaan we eerst de woordenlijst N-gram als gegevensset en het getrainde logistic regressiemodel van de vertakking training van het experiment. We Sla vervolgens het experiment gebruiken 'OpslaanAls' voor het maken van een experiment grafiek voor Voorspellend experiment. We verwijderen de Split Data-module en de vertakking training uit het experiment. We vervolgens verbinding maken de eerder opgeslagen N-gram-woordenlijst en het model met N-Gram-functies uitpakken en Score Model modules, respectievelijk. We ook verwijderen de module Evaluate Model.

We Invoegkolommen selecteren in de gegevensset module voordat tekst voorverwerken module te verwijderen van de labelkolom en schakel de optie 'Score kolom toevoegen aan gegevensset' in de Module Score. Op die manier de webservice vraagt niet om het label dat deze probeert te voorspellen en werkt niet echo de invoer-functies in het antwoord.

![Voorspellend Experiment](./media/text-analytics-module-tutorial/predictive-text.png)

Nu hebben we een experiment die kan worden gepubliceerd als een webservice en kan worden aangeroepen met behulp van de reactie op aanvragen of batch uitvoering API's.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over tekst analytics modules uit [MSDN-documentatie](https://msdn.microsoft.com/library/azure/dn905886.aspx).

