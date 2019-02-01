---
title: Modelprestatie evalueren
titleSuffix: Azure Machine Learning Studio
description: In dit artikel ziet u hoe u de prestaties van een model in Azure Machine Learning Studio evalueren en bevat een korte uitleg van de beschikbare metrische gegevens voor deze taak.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18, previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: b663177a07446b888bc7bf9e919bf180458d36bc
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487005"
---
# <a name="how-to-evaluate-model-performance-in-azure-machine-learning-studio"></a>Hoe modelprestaties evalueren in Azure Machine Learning Studio

In dit artikel ziet u hoe u de prestaties van een model in Azure Machine Learning Studio evalueren en bevat een korte uitleg van de beschikbare metrische gegevens voor deze taak. Drie gangbare scenario's met leren met supervisie worden weergegeven: 

* Regressie
* Binaire classificatie 
* multiklassen classificatie



Evaluatie van de prestaties van een model is een van de core-fasen in het data science process. Hiermee wordt aangegeven hoe succesvol de scoring (voorspellingen) van een gegevensset is door een getraind model. 

Biedt ondersteuning voor Azure Machine Learning model evaluatie via twee van de belangrijkste machine learning-modules: [Model evalueren] [ evaluate-model] en [Kruisvalidatie Model][cross-validate-model]. Deze modules kunnen u zien hoe uw model in termen van een aantal metrische gegevens die vaak worden gebruikt in machine learning en statistische gegevens uitvoert.

## <a name="evaluation-vs-cross-validation"></a>Evaluatie van Visual Studio. Kruisvalidatie
Beoordeling en validatie van kruislings zijn standaard manieren voor het meten van de prestaties van uw model. Ze beide evaluatie metrische gegevens die u kunt controleren of vergelijken met die van andere modellen genereren.

[Model evalueren] [ evaluate-model] wordt verwacht dat een beoordeelde gegevensset als invoer (of 2 in het geval dat u wilt vergelijken van de prestaties van twee verschillende modellen). Dit betekent dat u moet met het trainen van uw model met de [Train Model] [ train-model] -module en het model voorspellingen over het gebruik van bepaalde gegevensset de [Score Model] [ score-model]-module, voordat u de resultaten kunt evalueren. De evaluatie is gebaseerd op de scored labels/kansen, samen met de waarde true labels, die die allemaal worden uitgevoerd door de [Score Model] [ score-model] module.

U kunt ook kunt u cross-validatie uit te voeren van een getal van de trein score evalueren bewerkingen (10 vouwen) automatisch op verschillende subsets van de ingevoerde gegevens. De ingevoerde gegevens gesplitst in 10 delen, waarbij een is gereserveerd voor het testen, en de andere 9 voor training. Dit proces wordt herhaald 10 keer en het gemiddelde genomen van de evaluatie van metrische gegevens. Dit helpt bij het bepalen hoe goed een model zou generaliseren met nieuwe gegevenssets. De [Kruisvalidatie Model] [ cross-validate-model] module neemt een ongetrainde model en sommige gelabelde gegevensset en worden de evaluatieresultaten van elk van de 10 vouwen, naast de gemiddelde resultaten uitgevoerd.

In de volgende secties we eenvoudige regressie en classificatie-modellen bouwen en evalueren van de prestaties, met zowel de [Evaluate Model] [ evaluate-model] en de [Model valideren ] [ cross-validate-model] modules.

## <a name="evaluating-a-regression-model"></a>Een regressiemodel evalueren
Wordt ervan uitgegaan dat we prijs willen voorspellen van een auto met behulp van sommige functies zoals dimensies, paardenkracht en engine-specificaties. Dit is een regressieprobleem typische waar de doelvariabele (*prijs*) is een continue numerieke waarde. We passen een eenvoudige lineair regressiemodel waarmee gegeven van de waarden van de functie van een bepaalde auto de prijs van die auto voorspellen kunt. Dit regressiemodel kan worden gebruikt om te beoordelen we getraind op dezelfde gegevensset. Zodra we de voorspelde prijzen voor alle van de auto's hebben, kunnen we de prestaties van het model beoordelen door te kijken de voorspellingen hoeveel van de werkelijke prijzen gemiddeld afwijken. Ter illustratie gebruiken we de *auto prijs data (Raw) gegevensset* beschikbaar in de **opgeslagen gegevenssets** sectie in Azure Machine Learning Studio.

### <a name="creating-the-experiment"></a>Het maken van het Experiment
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

* Auto's prijs data (Raw)
* [Lineaire regressie][linear-regression]
* [Train Model][train-model]
* [Score-Model][score-model]
* [Model evalueren][evaluate-model]

Verbinding maken met de poorten zoals hieronder wordt weergegeven in afbeelding 1 en stelt de Label-kolom van de [Train Model] [ train-model] module *prijs*.

![Een regressiemodel evalueren](./media/evaluate-model-performance/1.png)

Afbeelding 1. Evaluatie van een regressiemodel.

### <a name="inspecting-the-evaluation-results"></a>De resultaten van evaluatie van inspecteren
Nadat het experiment is uitgevoerd, kunt u klikken op de uitvoerpoort van de [Evaluate Model] [ evaluate-model] -module en selecteer *Visualize* om te zien van de evaluatieresultaten. De evaluatie van metrische gegevens beschikbaar is voor regressiemodellen zijn: *Mean Absolute Error*, *Root Mean Absolute Error*, *Relative Absolute Error*, *ten opzichte van het kwadraat fout*, en de *correlatiecoëfficiënt Bepaling*.

De term "error" hier is het verschil tussen de voorspelde waarde en de waarde true. De absolute waarde of het kwadraat van dit verschil wordt gewoonlijk berekend om vast te leggen van de totale omvang van de fout voor alle instanties, zoals het verschil tussen de voorspelde waarde true en de waarde kan niet negatief zijn in sommige gevallen. De fout metrische gegevens over meten de voorspellende prestaties van een regressiemodel in termen van de gemiddelde afwijking van de voorspellingen op basis van de waarden waar. Lagere foutwaarden betekenen dat het model is nauwkeurigere voorspellingen maken. Een algemene fout metrische waarde gelijk is aan nul betekent dat het model de gegevens perfect past.

De determinatiecoëfficiënt, ook wel bekend als R is in het kwadraat, zijn ook een standaardmethode voor het meten hoe goed het model past bij de gegevens. Dit kan worden geïnterpreteerd als het aandeel van de variant worden gedekt door het model. Een hogere deel is beter in dit geval, waarbij 1 geeft aan een perfecte maat.

![Lineaire regressie evaluatie metrische gegevens](./media/evaluate-model-performance/2.png)

Afbeelding 2. Lineaire regressie evaluatie metrische gegevens.

### <a name="using-cross-validation"></a>Met behulp van Cross-validatie
Zoals eerder vermeld, kunt u uitvoeren herhaalde training, beoordeling en evaluaties automatisch met behulp van de [Kruisvalidatie Model] [ cross-validate-model] module. U hoeft in dit geval is een gegevensset, een ongetrainde model en een [Kruisvalidatie Model] [ cross-validate-model] module (Zie afbeelding hieronder). U moet de labelkolom instellen op *prijs* in de [Kruisvalidatie Model] [ cross-validate-model] eigenschappen van de module.

![Een regressiemodel cross-valideren](./media/evaluate-model-performance/3.png)

Afbeelding 3. Cross-Validerende een regressiemodel.

Nadat het experiment is uitgevoerd, kunt u de resultaten van evaluatie van controleren door te klikken op de juiste uitvoerpoort van de [Kruisvalidatie Model] [ cross-validate-model] module. Dit biedt een gedetailleerde weergave van de metrische gegevens voor elke herhaling (delen), en de gemiddelde resultaten van elk van de metrische gegevens (afbeelding 4).

![Resultaten van Kruisvalidatie van een regressiemodel](./media/evaluate-model-performance/4.png)

Afbeelding 4. Kruisvalidatie resultaten van een regressiemodel.

## <a name="evaluating-a-binary-classification-model"></a>Een binair classificeringsmodel evalueren
In een scenario voor binaire classificatie, is de doelvariabele slechts twee mogelijke resultaten, bijvoorbeeld: {0, 1} of {false, true}, {negatieve, positieve}. Wordt ervan uitgegaan dat krijgt u een gegevensset van volwassenen werknemers met een aantal demografische en werkgelegenheid variabelen en u wordt gevraagd om te voorspellen van het niveau van inkomsten, een binaire variabele met de waarden {"< 50 K =", "> 50 K '}. Met andere woorden, de negatieve klasse vertegenwoordigt de werknemers die kleiner dan of gelijk aan 50 K per jaar en de positieve klasse alle andere werknemers. Zoals in het scenario regressie zouden we een model te trainen, bepaalde gegevens te beoordelen en evalueren van de resultaten. Het belangrijkste verschil is hier is de keuze van de metrische gegevens over die Azure Machine Learning worden berekend en uitvoer. Ter illustratie van het scenario voor het niveau voorspelling van inkomsten, gebruiken we de [volwassenen](http://archive.ics.uci.edu/ml/datasets/Adult) gegevensset naar een Azure Machine Learning-experiment maken en de prestaties van een model voor logistieke regressie van twee klassen, een veelgebruikte binair bestand evalueren classificatie.

### <a name="creating-the-experiment"></a>Het maken van het Experiment
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

* Volwassenen telling inkomsten binaire classificatie-gegevensset
* [Two-Class Logistic Regression][two-class-logistic-regression]
* [Train Model][train-model]
* [Score-Model][score-model]
* [Model evalueren][evaluate-model]

Verbinding maken met de poorten zoals hieronder wordt weergegeven in afbeelding 5 en stelt de Label-kolom van de [Train Model] [ train-model] module *inkomsten*.

![Een binair classificeringsmodel evalueren](./media/evaluate-model-performance/5.png)

Afbeelding 5. Evaluatie van een binair classificeringsmodel.

### <a name="inspecting-the-evaluation-results"></a>De resultaten van evaluatie van inspecteren
Nadat het experiment is uitgevoerd, kunt u klikken op de uitvoerpoort van de [Evaluate Model] [ evaluate-model] -module en selecteer *Visualize* om te zien van de evaluatieresultaten (afbeelding 7). De evaluatie van metrische gegevens beschikbaar voor binaire classificatie-modellen zijn: *Nauwkeurigheid*, *precisie*, *intrekken*, *F1 Score*, en *AUC*. Bovendien de module een verwarringsmatrix met het aantal echt positieven, false negatieven, fout-positieven en de waarde true negatieven levert, evenals *ROC*, *precisie/intrekken*, en  *Lift* curven.

Nauwkeurigheid is gewoon de verhouding van correct ingedeeld exemplaren. Het is doorgaans de eerste metrische gegevens die u bekijkt bij het evalueren van een classificatie. Wanneer de testgegevens is echter niet-regelmatige (waar de meeste van de exemplaren die deel uitmaken van een van de klassen), of u meer geïnteresseerd bent in de prestaties op een van de klassen, nauwkeurigheid echt de effectiviteit van een classificatie niet vastleggen. In het scenario inkomsten niveau classificatie wordt ervan uitgegaan dat u wilt testen op sommige gegevens waar 99% van de exemplaren van mensen die kleiner dan of gelijk aan 50K per jaar verdienen vertegenwoordigen. Het is mogelijk om een 0.99 nauwkeurigheid door te voorspellen van de klasse ' < = 50K ' voor alle exemplaren. De classificatie in dat geval tot het uitvoeren van een algemene goed worden weergegeven, maar in werkelijkheid niet aan een van de hoog inkomen personen (de % 1) correct te classificeren.

Om die reden is het nuttig zijn voor het berekenen van aanvullende metrische gegevens die meer specifieke aspecten van de evaluatie vastleggen. Voordat u doorgaat op de details van deze metrische gegevens, is het belangrijk om te begrijpen van de verwarringsmatrix van de evaluatie van een binaire indeling. De klasse labels in de trainingsset kunnen uitvoeren op alleen 2 mogelijke waarden, dit is meestal als positief of negatief zijn. De positieve en negatieve-instanties die een classificatie correct voorspelt worden genoemd echt positieven (TP) en de waarde true negatieven (TN), respectievelijk. De onjuist ingedeeld exemplaren worden op dezelfde manier, fout-positieven (FP) en false negatieven (FN) genoemd. De verwarringsmatrix is gewoon een tabel met het aantal exemplaren die bij elk van deze 4 categorieën vallen. Azure Machine Learning bepaalt automatisch welke van de twee klassen in de gegevensset is de positieve klasse. Als de klasse labels Booleaanse waarde of gehele getallen zijn, worden de gelabelde exemplaren 'true' of '1' de positieve klasse toegewezen. Als de labels tekenreeksen zijn, zoals in het geval van de gegevensset inkomsten, de labels worden alfabetisch gesorteerd en het eerste niveau is gekozen als de negatieve klasse terwijl het tweede niveau de positieve klasse is.

![Binaire classificatie Verwarringsmatrix](./media/evaluate-model-performance/6a.png)

Afbeelding 6. Binaire classificatie Verwarringsmatrix.

Ga terug naar het probleem van de classificatie inkomsten, zou willen we vragen verschillende evaluatievragen die ons helpen inzicht in de prestaties van de classificatie die wordt gebruikt. Er is een zeer natuurlijke vraag: "Buiten de personen die het model met het verdienen worden voorspeld > 50 K (TP + FP), hoeveel zijn correct ingedeeld (TP)?" Deze vraag kan worden beantwoord door te kijken de **precisie** van het model, dit is de verhouding van positieve items verhoogd die correct zijn geclassificeerd: TP/(TP+FP). Een andere veelgestelde vraag is ' buiten alle hoge verdienen werknemers met inkomsten > 50 k (TP + FN), hoeveel de classificatie classificeren correct (TP) '. Dit is eigenlijk de **intrekken**, of de waarde true-positief-ratio: TP/(TP+fn) van de classificatie. U ziet u mogelijk dat er een duidelijke balans tussen precisie en intrekken is. Bijvoorbeeld, een relatief met gelijke taakverdeling gegevensset worden gegeven, zou een classificatie waarbij voorspelt voornamelijk positieve exemplaren hebben een hoge intrekken, maar een relatief laag precisie als met de exemplaren van het negatieve zou worden verkeerd geclassificeerd leidt tot een groot aantal fout-positieven. U ziet een diagram van hoe deze twee metrische gegevens verschillen, kunt u klikken op de **precisie/INTREKKEN** kromme op de pagina evaluatie resultaat uitvoer (linksboven onderdeel van de afbeelding 7).

![Resultaten van evaluatie van binaire classificatie](./media/evaluate-model-performance/7.png)

Afbeelding 7. Resultaten van evaluatie van binaire classificatie.

Een andere gerelateerde metrische gegevens die vaak wordt gebruikt is de **F1 Score**, die duurt precisie- en intrekken in acht genomen. Dit is het gemiddelde harmonische van deze 2 metrische gegevens en als zodanig wordt berekend: F1 = 2 (precisie x intrekken) / (precisie + intrekken). De score F1 is een goede manier om samen te vatten van de evaluatie in een enkel getal, maar het is altijd een goede gewoonte om te kijken naar precisie- en terughalen samen om beter te begrijpen hoe een classificatie zich gedraagt.

Bovendien een de waarde true-positief-ratio vergeleken met de fout-positief-ratio in kunt inspecteren de **ontvanger operationele kenmerken (ROC)** curve en de bijbehorende **gebied onder de Curve (AUC)** waarde. Hoe dichter deze curve is in de linkerbovenhoek, hoe beter de prestaties van de classificatie is (dat is het optimaliseren van de waarde true-positief-ratio geminimaliseerd, de fout-positief-ratio). Curven die zich dicht bij de diagonaal van de grafiek, resultaat van de classificaties die doorgaans voorspellingen die zich dicht bij willekeurige raden.

### <a name="using-cross-validation"></a>Met behulp van Cross-validatie
Zoals in het voorbeeld regressie kunnen we cross validatie voor herhaaldelijk trainen, score en beoordelen van verschillende subsets van de gegevens automatisch uitvoeren. Op deze manier kunnen we gebruiken de [Kruisvalidatie Model] [ cross-validate-model] -module, een ongetrainde logistieke regressiemodel en een gegevensset. De labelkolom moet worden ingesteld op *inkomsten* in de [Kruisvalidatie Model] [ cross-validate-model] eigenschappen van de module. Na het uitvoeren van het experiment en het recht op de uitvoerpoort van de [Kruisvalidatie Model] [ cross-validate-model] -module, ziet de binaire classificatie metrische waarden voor elke vouwen, verder met de gemiddelde en standaarddeviatie van elk. 

![Een binair classificeringsmodel cross-valideren](./media/evaluate-model-performance/8.png)

Afbeelding 8. Een binair classificeringsmodel cross valideren.

![Resultaten van Kruisvalidatie van een binaire classificatie](./media/evaluate-model-performance/9.png)

Afbeelding 9. Kruisvalidatie resultaten van een binaire classificatie.

## <a name="evaluating-a-multiclass-classification-model"></a>Evaluatie van een Model Multiklassen classificatie
In dit experiment gebruiken we de populaire [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") gegevensset waarin exemplaren van 3 verschillende typen (klassen) van de iris-installaties. Er zijn 4 functie waarden (petal length/width en petal length/breedte) voor elk exemplaar. In de vorige experimenten we getraind en de modellen met behulp van de dezelfde gegevenssets getest. Hier gebruiken we de [Split Data] [ split] module 2 subsets van de gegevens maken, trainen op de eerste en beoordelen en evalueren in het tweede. De Iris-gegevensset is openbaar beschikbaar op de [UCI Machine Learning-opslagplaats](http://archive.ics.uci.edu/ml/index.html), en kan worden gedownload met behulp van een [importgegevens] [ import-data] module.

### <a name="creating-the-experiment"></a>Het maken van het Experiment
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

* [Gegevens importeren][import-data]
* [Beslissingsforest met multiklasse][multiclass-decision-forest]
* [Split Data][split]
* [Train Model][train-model]
* [Score-Model][score-model]
* [Model evalueren][evaluate-model]

Verbinding maken met de poorten zoals hieronder wordt weergegeven in afbeelding 10.

Instellen van de index van de kolom Label van de [Train Model] [ train-model] module tot en met 5. De gegevensset heeft geen rij met koppen, maar we weten dat de klasse-labels in de vijfde kolom zijn.

Klik op de [gegevens importeren] [ import-data] -module en stel de *gegevensbron* eigenschap *Web-URL via HTTP*, en de *URL* naar http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Stel de van exemplaren moet worden gebruikt voor de training in de [Split Data] [ split] module (0,7 bijvoorbeeld).

![Een classificatie voor Multiklassen evalueren](./media/evaluate-model-performance/10.png)

Afbeelding 10. Een classificatie voor Multiklassen evalueren

### <a name="inspecting-the-evaluation-results"></a>De resultaten van evaluatie van inspecteren
Voer het experiment uit en klik op de uitvoerpoort van [Evaluate Model][evaluate-model]. Resultaten van de beoordeling worden weergegeven in de vorm van een verwarringsmatrix in dit geval. De matrix bevat de werkelijke omzet versus voorspelde instanties voor alle 3 klassen.

![Resultaten van evaluatie van multiklassen classificatie](./media/evaluate-model-performance/11.png)

Afbeelding 11. Resultaten van evaluatie van multiklassen classificatie.

### <a name="using-cross-validation"></a>Met behulp van Cross-validatie
Zoals eerder vermeld, kunt u uitvoeren herhaalde training, scores en evaluaties automatisch met behulp van de [Kruisvalidatie Model] [ cross-validate-model] module. Moet u een gegevensset, een ongetrainde model en een [Kruisvalidatie Model] [ cross-validate-model] module (Zie afbeelding hieronder). Opnieuw moet u instellen de labelkolom van de [Kruisvalidatie Model] [ cross-validate-model] module (kolomindex 5 in dit geval). Na het uitvoeren van het experiment en het recht op de uitvoerpoort van de [Kruisvalidatie Model][cross-validate-model], controleert u de metrische waarden voor elke vouwen, evenals de afwijking mean- en standard. De metrische gegevens wordt hier weergegeven zijn de vergelijkbaar met die in het geval binaire classificatie besproken. Houd er echter rekening mee dat in multiklassen classificatie de echte positieven/negatieven en fout-positieven/negatieven computing wordt uitgevoerd door tellen op basis van per klasse, omdat er geen algemene positieve of negatieve klasse. Bijvoorbeeld bij het berekenen van de precisie of intrekken van de klasse "Iris-setosa", wordt ervan uitgegaan dat dit de positieve klasse en alle andere als negatief zijn is.

![Een Multiklassen classificeringsmodel cross-valideren](./media/evaluate-model-performance/12.png)

Afbeelding 12. Een Multiklassen classificeringsmodel cross valideren.

![Resultaten van Kruisvalidatie van een Model Multiklassen classificatie](./media/evaluate-model-performance/13.png)

Afbeelding 13. Kruisvalidatie resultaten van een Model Multiklassen classificatie.

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

