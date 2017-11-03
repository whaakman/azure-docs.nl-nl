---
title: Evalueren van de prestaties van het model in Machine Learning | Microsoft Docs
description: Legt uit hoe evalueren model prestaties in Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 5dc5348a-4488-4536-99eb-ff105be9b160
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bradsev;garye
ms.openlocfilehash: 48ce4584f7270d78b1d09b848bfdd305d03012b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Modelprestaties evalueren in Azure Machine Learning
Dit artikel laat zien hoe u de prestaties van een model in Azure Machine Learning Studio evalueren en bevat een korte uitleg van de beschikbare metrische gegevens voor deze taak. Drie gangbare scenario's met leren met supervisie worden weergegeven: 

* Regressie
* binaire classificatie 
* multiklassen classificatie

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Evalueren van de prestaties van een model is een van de core fasen in het proces van de wetenschappelijke gegevens. Hiermee wordt aangegeven hoe geslaagde de score (voorspellingen) van een dataset is door een getraind model. 

Azure Machine Learning ondersteunt model evaluatie via twee van de belangrijkste machine learning-modules: [Evaluate Model] [ evaluate-model] en [gekruist te valideren Model][cross-validate-model]. Deze modules kunnen u zien hoe uw model uitvoert in termen van een aantal metrische gegevens die vaak worden gebruikt in machine learning en statistieken.

## <a name="evaluation-vs-cross-validation"></a>Vs evaluatie. Cross-validatie
Evaluatie en cross validatie zijn standaard manieren om de prestaties van uw model te meten. Beide evaluatie metrische gegevens die u kunt controleren of vergelijken met die van andere modellen genereren.

[Model evalueren] [ evaluate-model] een scored gegevensset verwacht als invoer (of 2 in het geval dat u wilt vergelijken de prestaties van 2 verschillende modellen). Dit betekent dat u moet voor het trainen van uw model met de [Model trainen] [ train-model] module en maak voorspellingen op bepaalde gegevensset met behulp van de [Score Model] [ score-model] -module, voordat u de resultaten kunt evalueren. De evaluatie is gebaseerd op de scored labels/waarschijnlijkheid samen met de waarde true labels, die allemaal worden uitgevoerd door de [Score Model] [ score-model] module.

U kunt ook kunt u cross-validatie uit te voeren een aantal bewerkingen train score evalueren (10 vouwen) automatisch op verschillende subsets van de invoergegevens. De invoergegevens opgesplitst 10 delen, waarbij een is gereserveerd voor het testen, en de andere 9 voor training. Dit proces wordt herhaald 10 keer en het gemiddelde genomen van de evaluatie van metrische gegevens. Dit helpt bij het bepalen hoe goed een model zou generalize naar nieuwe gegevenssets. De [gekruist te valideren Model] [ cross-validate-model] module neemt in een ongetrainde model en sommige gelabelde gegevensset en worden de evaluatieresultaten van elk van de 10 vouwen dat, naast de gemiddelde resultaten.

In de volgende secties we eenvoudige regressie en classificatie modellen bouwt en evalueren van de prestaties, met zowel de [Evaluate Model] [ evaluate-model] en de [gekruist te valideren Model] [ cross-validate-model] modules.

## <a name="evaluating-a-regression-model"></a>Een regressiemodel evalueren
Stel dat we willen voorspellen van een auto prijs met bepaalde functies zoals dimensies, paardenkracht en engine-specificaties. Dit is een probleem typische regressie waar de doelvariabele (*prijs*) is een continue numerieke waarde. We past een eenvoudige lineair regressiemodel waarmee basis van de waarden van de functie van een bepaalde auto kan de prijs van die auto's voorspellen. Deze regressiemodel kan worden gebruikt voor de beoordeling van dezelfde gegevensset die we getraind op. Nadat we de voorspelde prijzen voor alle van de auto's hebben, kunnen we de prestaties van het model evalueren door te kijken naar de voorspellingen hoeveel van de werkelijke prijzen gemiddeld afwijken. Een voorbeeld gebruiken we de *Automobile price data (Raw) gegevensset* beschikbaar in de **gegevenssets opgeslagen** sectie in Azure Machine Learning Studio.

### <a name="creating-the-experiment"></a>Het Experiment maken
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

* Auto price data (Raw)
* [Lineaire regressie][linear-regression]
* [Train Model][train-model]
* [Score-Model][score-model]
* [Model evalueren][evaluate-model]

Verbinding maken met de poorten, zoals hieronder wordt weergegeven in afbeelding 1 en stelt de Label-kolom van de [Train Model] [ train-model] module *prijs*.

![Een regressiemodel evalueren](./media/evaluate-model-performance/1.png)

Afbeelding 1. Evalueren van een regressiemodel.

### <a name="inspecting-the-evaluation-results"></a>De resultaten van de evaluatie te bekijken
Nadat het experiment is uitgevoerd, kunt u klikken op de uitvoerpoort van de [Evaluate Model] [ evaluate-model] module en selecteer *Visualize* om te zien van de evaluatieresultaten. De evaluatie van metrische gegevens beschikbaar voor regressie modellen zijn: *Absolute Error betekenen*, *hoofdmap betekenen Absolute Error*, *Relative Absolute Error*, *Relative Squared Error*, en de *determinatiecoëfficiënt*.

De term "error" Hier geeft het verschil tussen de voorspelde waarde en de waarde ' True '. De absolute waarde of het kwadraat van dit verschil zijn meestal berekend voor het vastleggen van de totale omvang van de fout in alle exemplaren, zoals het verschil tussen de voorspelde en waar de waarde kan niet negatief zijn in sommige gevallen. De fout metrische gegevens meten de voorspellende prestaties van een regressiemodel in termen van de gemiddelde afwijking van de voorspellingen van de waarden true. Lagere foutwaarden betekent dat het model nauwkeurigere bij het maken van voorspellingen is. Een algemene fout metriek 0 betekent dat het model past bij de gegevens perfect.

De determinatiecoëfficiënt, wat ook wel bekend als R kwadraat, is ook een standaardmanier meten hoe goed het model geschikt is voor de gegevens. Het kan worden geïnterpreteerd als het aandeel van de variatie worden verklaard in het model. Een hogere verhouding is beter in dit geval waarbij 1 geeft aan een past.

![Lineaire regressie evaluatie metrische gegevens](./media/evaluate-model-performance/2.png)

Afbeelding 2. Lineaire regressie evaluatie metrische gegevens.

### <a name="using-cross-validation"></a>Met behulp van Cross-validatie
Zoals eerder gezegd, kunt u uitvoeren herhaalde training, scores en evaluaties automatisch met de [gekruist te valideren Model] [ cross-validate-model] module. In dit geval hoeft u een gegevensset, een ongetrainde model en een [gekruist te valideren Model] [ cross-validate-model] module (Zie afbeelding hieronder). Houd er rekening mee dat u de labelkolom ingesteld moet op *prijs* in de [gekruist te valideren Model] [ cross-validate-model] eigenschappen van de module.

![Een regressiemodel cross valideren](./media/evaluate-model-performance/3.png)

Afbeelding 3. Cross-valideren van een regressiemodel.

Nadat het experiment is uitgevoerd, kunt u de resultaten van evaluatie van controleren door te klikken op de juiste uitvoerpoort van de [gekruist te valideren Model] [ cross-validate-model] module. Dit biedt een gedetailleerde weergave van de metrische gegevens voor elke herhaling (vouwen) en de gemiddelde resultaten van elk van de metrische gegevens (afbeelding 4).

![Kruisvalidatie resultaten van een regressiemodel](./media/evaluate-model-performance/4.png)

Afbeelding 4. Kruisvalidatie resultaten van een regressiemodel.

## <a name="evaluating-a-binary-classification-model"></a>Een binaire indeling Model evalueren
In een binaire classificatie-scenario is de doelvariabele slechts twee mogelijke resultaten, bijvoorbeeld: {0, 1} of {false, true}, {negatief, positieve}. Wordt ervan uitgegaan dat u krijgt u een gegevensset volwassenen werknemers met enkele demografische gegevens en arbeid variabelen en u wordt gevraagd om te voorspellen van het niveau van inkomsten, een binaire variabele met de waarden {"< = 50K ', ' > 50K '}. Met andere woorden, de negatieve klasse vertegenwoordigt de werknemers die kleiner dan of gelijk zijn aan 50 kB per jaar en de positieve klasse geeft alle andere werknemers. Net zoals in het scenario regressie zou we een model te trainen, bepaalde gegevens te beoordelen en evalueren van de resultaten. Het belangrijkste verschil is hier is de keuze van metrische gegevens die Azure Machine Learning wordt berekend en uitvoer. Ter illustratie van het niveau voorspelling inkomsten scenario, gebruiken we de [volwassenen](http://archive.ics.uci.edu/ml/datasets/Adult) gegevensset voor het maken van een Azure Machine Learning-experiment en evalueren van de prestaties van een tweeklasse logistic regressiemodel, een veelgebruikte binaire classificatie.

### <a name="creating-the-experiment"></a>Het Experiment maken
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

* Volwassenen inventarisering Income binaire classificatie gegevensset
* [Two-Class Logistic Regression][two-class-logistic-regression]
* [Train Model][train-model]
* [Score-Model][score-model]
* [Model evalueren][evaluate-model]

Verbinding maken met de poorten, zoals hieronder wordt weergegeven in afbeelding 5 en stelt de Label-kolom van de [Train Model] [ train-model] module *inkomsten*.

![Een binaire indeling Model evalueren](./media/evaluate-model-performance/5.png)

Afbeelding 5. Een binaire indeling Model evalueren.

### <a name="inspecting-the-evaluation-results"></a>De resultaten van de evaluatie te bekijken
Nadat het experiment is uitgevoerd, kunt u klikken op de uitvoerpoort van de [Evaluate Model] [ evaluate-model] module en selecteer *Visualize* om te zien van de evaluatieresultaten (afbeelding 7). De evaluatie van metrische gegevens beschikbaar voor binaire classificatie modellen zijn: *nauwkeurigheid*, *precisie*, *intrekken*, *F1 Score*, en *AUC*. Bovendien de module een verwarring matrix met het aantal positieven true, false negatieve valse positieven en waar negatieve levert, evenals *ROC*, *precisie/intrekken*, en *til* curven.

Nauwkeurigheid is het aantal exemplaren correct ingedeeld. Meestal is dit de eerste metrische gegevens die u bekijkt bij het evalueren van een classificatie. Wanneer de testgegevens is echter niet in balans (waar de meeste van de exemplaren behoren tot een van de klassen) of als u meer geïnteresseerd in de prestaties op een van de klassen, nauwkeurigheid echt de effectiviteit van een classificatie niet vastleggen. In het scenario inkomsten niveau classificatie wordt ervan uitgegaan dat u wilt testen op bepaalde gegevens waar 99% exemplaren van mensen die kleiner dan of gelijk zijn aan 50 kB per jaar verdienen vertegenwoordigen. Het is mogelijk een 0.99 nauwkeurigheid bereiken door het voorspellen van de klasse ' < = 50K ' voor alle exemplaren. De classificatie in dit geval wordt weergegeven in algemene goed werk, maar in werkelijkheid is mislukt voor het classificeren van een van de hoog inkomen personen (1%) correct.

Daarom is het handig zijn voor het berekenen van aanvullende gegevens die meer specifieke aspecten van de evaluatie vastlegt. Voordat u doorgaat naar de details van deze metrische gegevens, is het belangrijk om te begrijpen van de matrix verwarring van de evaluatie van een binaire indeling. De klasse labels in de trainingset kunnen alleen 2 mogelijke waarden die we meestal naar verwijzen overnemen als positief of negatief. Het positieve en negatieve instanties die een classificatie correct voorspelt worden genoemd waar positieven (TP) en waar negatieve (TN), respectievelijk. Op deze manier worden de onjuist ingedeelde exemplaren valse positieven (EP) en fout-negatieve resultaten (FN) genoemd. De matrix verwarring is gewoon een tabel met het nummer van exemplaren die bij elk van deze 4 categorieën vallen. Azure Machine Learning besluit automatisch welke van de twee klassen in de gegevensset is positief klasse. Als de klasse-labels Boolean of gehele getallen zijn zijn, zijn de positieve klasse toegewezen door de gelabelde exemplaren 'true' of '1'. Als de labels tekenreeksen zijn, in het geval van de gegevensset inkomsten, de labels alfabetische volgorde worden gesorteerd en het eerste niveau is gekozen als de negatieve klasse terwijl het tweede niveau positief klasse is.

![Binaire classificatie verwarring Matrix](./media/evaluate-model-performance/6a.png)

Afbeelding 6. Binaire classificatie verwarring Matrix.

Ga terug naar de klassificatieprobleem inkomsten, zou willen we vragen verschillende evaluatievragen die help ons inzicht in de prestaties van de classificatie die wordt gebruikt. Een logisch vraag is: ' buiten de personen die het model met het verdienen worden voorspeld > 50 K (TP + FrontPage), hoeveel zijn correct geclassificeerd (TP)?' Kan deze vraag worden beantwoord door te kijken naar de **precisie** van het model, is het aandeel van positieven die correct zijn geclassificeerd: TP/(TP+FP). Vraag is ' buiten de hoge verdienen werknemers met inkomsten > 50 k (TP + FN), hoeveel de classificatie classificeren correct (TP) '. Dit is in werkelijkheid de **intrekken**, of de waarde true positief snelheid: TP/(TP+FN) van de classificatie. Er is een duidelijke compromis tussen precision en intrekken, zult u merken. Bijvoorbeeld, een relatief taakverdeling gegevensset wordt opgegeven, heeft een classificatie die voornamelijk positief exemplaren voorspelt een hoge intrekken, maar een relatief laag precisie zo veel mogelijk negatieve exemplaren van zou worden verkeerd geclassificeerd wat resulteert in een groot aantal fout-positieven. Overzicht van hoe deze twee metrische gegevens variëren tekent, kunt u op de curve precisie/INTREKKEN in de pagina evaluatie resultaat uitvoer (boven links deel uit van de afbeelding 7).

![Resultaten van evaluatie van binaire classificatie](./media/evaluate-model-performance/7.png)

Afbeelding 7. Resultaten van evaluatie van binaire classificatie.

Een andere gerelateerde metriek die vaak wordt gebruikt is het **F1 Score**, wat zowel precision en terughalen in aanmerking duurt. Dit is het gemiddelde harmonische van deze 2 metrische gegevens en als zodanig wordt berekend: F1 = 2 (precisie x intrekken) / (precisie + intrekken). De score F1 is een uitstekende manier om samen te vatten de evaluatie van een enkel getal, maar het is altijd een goede gewoonte om te kijken naar zowel precision en terughalen samen om beter te begrijpen hoe een classificatie zich gedraagt.

Bovendien een de waarde true positief snelheid versus de false positief frequentie in kunt inspecteren de **ontvanger besturingssysteem kenmerk (ROC)** curve en de bijbehorende **gebied onder de Curve (AUC)** waarde. Hoe dichter deze curve is in de linkerbovenhoek, hoe beter de classificatie prestaties (die is het optimaliseren van de waarde true positief snelheid terwijl het minimaliseert de false positief snelheid). Curven die zich dicht bij de diagonaal van de grafiek, het resultaat van de classificaties die doorgaans de voorspellingen die zich dicht bij willekeurige raden.

### <a name="using-cross-validation"></a>Met behulp van Cross-validatie
Zoals in het voorbeeld regressie kunnen we cross validatie herhaaldelijk trainen, scores en andere subsets van de gegevens automatisch evalueren als u wilt uitvoeren. Op deze manier kunnen we gebruiken de [gekruist te valideren Model] [ cross-validate-model] module, een ongetrainde logistic regressiemodel en een dataset. De labelkolom moet worden ingesteld op *inkomsten* in de [gekruist te valideren Model] [ cross-validate-model] eigenschappen van de module. Nadat het experiment uitvoeren en te klikken op de juiste van uitvoerpoort de [gekruist te valideren Model] [ cross-validate-model] -module, ziet u de binaire classificatie metrische waarden voor elke vouwen bovendien voor de gemiddelde en de standaarddeviatie van elke. 

![Een binaire indeling Model cross valideren](./media/evaluate-model-performance/8.png)

Afbeelding 8. Een binaire indeling Model cross valideren.

![Kruisvalidatie resultaten van een binaire classificatie](./media/evaluate-model-performance/9.png)

Afbeelding 9. Kruisvalidatie resultaten van een binaire classificatie.

## <a name="evaluating-a-multiclass-classification-model"></a>Evaluatie van een Model Multiklassen classificatie
In dit experiment gebruiken we de populaire [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") dataset die exemplaren van 3 soorten (klassen) het bedrijf iris bevat. Er zijn 4 functie waarden (sepal lengte/breedte en lengte Bloemblad/breedte) voor elk exemplaar. In de vorige experimenten we getraind en de modellen met behulp van de dezelfde gegevenssets getest. Hier gebruiken we de [Split Data] [ split] module 2 subsets van de gegevens maken, op de eerste trainen en beoordelen en evalueren in het tweede. De gegevensset Iris openbaar beschikbaar is op de [UCI Machine Learning-opslagplaats](http://archive.ics.uci.edu/ml/index.html), en kan worden gedownload met behulp van een [importgegevens] [ import-data] module.

### <a name="creating-the-experiment"></a>Het Experiment maken
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

* [Gegevens importeren][import-data]
* [Multiklasse besluit Forest][multiclass-decision-forest]
* [Gegevens splitsen][split]
* [Train Model][train-model]
* [Score-Model][score-model]
* [Model evalueren][evaluate-model]

Verbinding maken met de poorten zoals hieronder wordt weergegeven in afbeelding 10.

Stel de kolomindex Label van de [Train Model] [ train-model] module tot 5. De gegevensset heeft geen veldnamenrij, maar we weten dat de klasse-labels in de vijfde kolom zijn.

Klik op de [gegevens importeren] [ import-data] module en stel de *gegevensbron* eigenschap *via HTTP-URL voor webinhoud*, en de *URL* naar http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Stel de fractie van exemplaren moet worden gebruikt voor training in de [Split Data] [ split] module (0,7 bijvoorbeeld).

![Evaluatie van een Multiklassen classificatie](./media/evaluate-model-performance/10.png)

Afbeelding 10. Evaluatie van een Multiklassen classificatie

### <a name="inspecting-the-evaluation-results"></a>De resultaten van de evaluatie te bekijken
Voer het experiment uit en klik op de uitvoerpoort van [Evaluate Model][evaluate-model]. De evaluatieresultaten worden weergegeven in de vorm van een matrix verwarring in dit geval. De matrix geeft de werkelijke waarden versus voorspelde exemplaren voor alle 3 klassen.

![Resultaten van evaluatie van multiklassen classificatie](./media/evaluate-model-performance/11.png)

Afbeelding 11. Resultaten van evaluatie van multiklassen classificatie.

### <a name="using-cross-validation"></a>Met behulp van Cross-validatie
Zoals eerder gezegd, kunt u uitvoeren herhaalde training, scores en evaluaties automatisch met de [gekruist te valideren Model] [ cross-validate-model] module. U moet een gegevensset, een ongetrainde model en een [gekruist te valideren Model] [ cross-validate-model] module (Zie afbeelding hieronder). U moet opnieuw instellen van de labelkolom van de [gekruist te valideren Model] [ cross-validate-model] module (kolomindex 5 in dit geval). Nadat het experiment uitgevoerd en het recht te klikken op de uitvoerpoort van de [gekruist te valideren Model][cross-validate-model], kunt u de metrische waarden voor elke vouwen, evenals de afwijking van gemiddelde en standard controleren. De metrische gegevens hier weergegeven zijn de vergelijkbaar met die in het geval van de binaire indeling besproken. Let echter op dat in multiklassen classificatie computing de waar positieven/negatieve en fout-positieven/negatieve resultaten wordt gerealiseerd door tellen op basis van per-klasse als er geen klasse algemene positief of negatief is. Bijvoorbeeld bij het berekenen van de precisie of intrekken van de klasse 'Iris setosa', wordt ervan uitgegaan dat dit de positieve klasse en alle andere negatief is.

![Een Model Multiklassen classificatie cross valideren](./media/evaluate-model-performance/12.png)

Afbeelding 12. Een Model Multiklassen classificatie cross valideren.

![Kruisvalidatieresultaten van een Model Multiklassen classificatie](./media/evaluate-model-performance/13.png)

Afbeelding 13. De Kruisvalidatieresultaten van een Model Multiklassen classificatie.

<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/

