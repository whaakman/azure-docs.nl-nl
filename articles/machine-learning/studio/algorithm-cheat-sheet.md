---
title: Machine learning-algoritme-referentieoverzicht | Microsoft Docs
description: Een afdrukbaar machine learning-algoritme-referentieoverzicht kunt u de juiste algoritme voor het voorspellende model kiezen in Azure Machine Learning Studio.
keywords: algoritme referentieoverzicht, referentieoverzicht, machine learning-algoritme
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: garye
ms.openlocfilehash: 5ecd569f68c2eada921193f2b7b88fe788120a9b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="machine-learning-algorithm-cheat-sheet-for-microsoft-azure-machine-learning-studio"></a>Overzichtskaart voor Machine Learning-algoritme voor Microsoft Azure Machine Learning Studio
De **Microsoft Azure Machine Learning-algoritme cheats blad** helpt u de juiste algoritme voor een predictive analytics-model kiezen.

[Azure Machine Learning Studio](https://studio.azureml.net/) heeft een grote bibliotheek met algoritmen uit de ***regressie***, ***classificatie***, ***clustering***, en ***afwijkingsdetectie*** families. Elk is ontworpen voor het oplossen van een ander type van machine learning probleem.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Download: Machine learning-algoritme-referentieoverzicht
**De referentieoverzicht hier downloaden: [Machine Learning-algoritme cheats blad (11 x 17.)](http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)**

![Machine Learning-algoritme-referentieoverzicht: informatie over het kiezen van een Machine Learning-algoritme.][cheat-sheet]

[cheat-sheet]: ./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

Downloaden en afdrukken van het Machine Learning-algoritme cheats blad in a3-grootte om te zorgen dat deze handige en hulp bij het kiezen van een algoritme.

> [!NOTE]
> Zie het artikel [algoritmen kiezen voor Microsoft Azure Machine Learning](algorithm-choice.md) voor gedetailleerde richtlijnen voor het gebruik van deze referentieoverzicht.
> 
> 

## <a name="more-help-with-algorithms"></a>Meer hulp bij algoritmen
* Zie voor informatie over het gebruik van deze referentieoverzicht voor het kiezen van het juiste algoritme, plus een meer gedetailleerde bespreking van de verschillende typen van machine learning-algoritmen en hoe deze worden gebruikt, [algoritmen kiezen voor het Microsoft Azure Machine Learning](algorithm-choice.md).
* Zie voor een downloadbare infographic die beschrijving algoritmen en voorbeelden, [downloadbare Infographic: Machine learning-algoritme voorbeelden voor alledaagse](basics-infographic-with-algorithm-examples.md).
* Zie voor een lijst door de categorie van alle de machine learning-algoritmen in Machine Learning Studio [Model initialiseren] [ initialize-model] in de Machine Learning Studio algoritme en de Module Help.
* Zie voor een volledige alfabetische lijst van algoritmen en modules in Machine Learning Studio [lijst A-Z van Machine Learning Studio-modules] [ a-z-list] in de Help van de Module en Machine Learning Studio-algoritme.
* Zie [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](studio-overview-diagram.md) als u een diagram wilt downloaden en afdrukken met een overzicht van de mogelijkheden van Machine Learning Studio.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="notes-and-terminology-definitions-for-the-machine-learning-algorithm-cheat-sheet"></a>Opmerkingen en termen die voor de machine learning-algoritme cheats blad

* De suggesties aangeboden in dit algoritme-referentieoverzicht zijn bij benadering regels van de miniatuur. Sommige kunnen worden verbogen en sommige flagrantly kunnen worden geschonden. Dit is bedoeld als een beginpunt voorstellen. Wees niet bang voor het uitvoeren van een man concurrentie tussen verschillende algoritmen voor uw gegevens. Er is gewoon geen vervanging voor meer informatie over de beginselen van elk algoritme en inzicht in het systeem dat uw gegevens gegenereerd.

* Elke machine learning-algoritme heeft een eigen stijl of *inductieve afwijking*. Voor een specifiek probleem verschillende algoritmen is wellicht beter en één algoritme is mogelijk beter geschikt dan andere. Maar het is niet altijd mogelijk weet die geschikt is. In dergelijke gevallen worden verschillende algoritmen samen weergegeven in de referentieoverzicht. Een strategie voor een juiste kan worden één algoritme, en als de resultaten nog niet voldoende, probeer de andere. Hier volgt een voorbeeld van de [AI-galerie van Azure](http://gallery.cortanaintelligence.com/) van een experiment die probeert verschillende algoritmen tegen dezelfde gegevens en de resultaten vergelijkt: [vergelijken met meerdere klasse classificaties: Letter erkenning](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

* Er zijn drie hoofdcategorieën van machine learning: **onder supervisie learning**, **leren zonder supervisie**, en **versterking learning**.

  * In **onder supervisie learning**, elk gegevenspunt is met het label of die zijn gekoppeld aan een categorie of de waarde van belang.  Een voorbeeld van een categorische label met het toewijzen van een afbeelding als een 'cat' of een 'aquaduct'.  Een voorbeeld van een Waardelabel is de verkoopprijs die zijn gekoppeld aan een gebruikte auto. Het doel van leren met supervisie is om te bestuderen veel gelabelde voorbeelden zoals deze, en vervolgens te kunnen maken van voorspellingen over toekomstige gegevenspunten. Bijvoorbeeld, nieuwe foto's identificeren met de juiste dieren of nauwkeurige verkoopprijzen toewijzen aan andere gebruikte auto's. Dit is een populair en nuttige type machine learning. Worden alle modules in Azure Machine Learning-algoritmen, behalve voor leren met supervisie [K-Means Clustering][k-means-clustering].

  * In **leren zonder supervisie**, gegevenspunten hebben geen labels worden gekoppeld. In plaats daarvan is het doel van een leeralgoritme zonder supervisie Organiseer de gegevens op een bepaalde manier of om de structuur te beschrijven. Dit kan betekenen groeperen in clusters, zoals het K-means of het vinden van verschillende manieren van complexe gegevens bekijkt, zodat deze eenvoudiger weergegeven.

  * In **versterking learning**, de algoritme een actie te kiezen in reactie op elk gegevenspunt opgehaald. Er is een algemene benadering in robotics, waarbij de set sensormetingen op één punt in tijd is een gegevenspunt en het algoritme moet de volgende actie van de robot kiezen. Het is ook een natuurlijke geschikt is voor Internet der dingen-toepassingen. Het leeralgoritme ontvangt ook een signaal derden korte tijd later, dat aangeeft hoe goed het besluit is. De algoritme wijzigt op basis van dit, de strategie met het oog op de hoogste prijs. Er zijn momenteel geen versterking learning-algoritme-modules in Azure ML.

* **Bayesiaanse methoden** veronderstelt van statistisch onafhankelijke gegevenspunten. Dit betekent dat de unmodeled variabiliteit in één gegevenspunt is niet-gerelateerde met anderen, dat wil zeggen, niet kan worden voorspeld. Bijvoorbeeld, als de gegevens die wordt opgenomen is het aantal minuten op waarna de volgende trein trein binnenkomt, twee metingen bij een dag bij elkaar statistisch onafhankelijk zijn. Echter twee metingen bij een minuut bij elkaar zijn niet statistisch onafhankelijke: de waarde van een maximaal voorspellende van de waarde van de andere is.

* **Boosted decision tree regressie** wordt gebruikgemaakt van de functie overlapping of interactie tussen onderdelen. Dat betekent dat, in een bepaalde gegevenspunt de waarde van een functie enigszins voorspellende van de waarde van een andere. Bijvoorbeeld dagelijks hoge en lage temperatuur gegevens kunt de lage temperatuur weten voor de dag u een redelijke schatting met de hoge maken. De informatie in de twee functies is enigszins overbodig.

* Gegevens classificeren in meer dan twee categorieën kan worden gedaan met behulp van een classificatie inherent meerdere klasse, of door een combinatie van een set van twee klasse classificaties in een **ensemble**. In de methode ensemble, is een afzonderlijke tweeklasse classificatie voor elke objectklasse - elkaar scheidt de gegevens in twee categorieën: 'dit class' en "niet deze klasse." Deze classificaties stem vervolgens de juiste toewijzing van het gegevenspunt. Dit is de operationele principe achter [One-vs-All-Multiklasse][one-vs-all-multiclass].

* Verschillende methoden, waaronder logistic regression en de Bayes point machine, wordt ervan uitgegaan dat **lineaire klassengrenzen**. Dat wil zeggen, ze wordt ervan uitgegaan dat de grenzen tussen klassen zijn ongeveer rechte regels (of hyperplanes in het algemene geval). Vaak is dit een kenmerk van de gegevens die u niet weet pas nadat u hebt geprobeerd te afscheiding, maar er is iets dat doorgaans kan worden geleerd door het visualiseren van tevoren. Als de klassengrenzen zeer onregelmatige uitziet, houd beslissingsstructuren, besluit jungles, ondersteuning voor vector machines of neural netwerken.

* Neural networks met categorische variabelen kunnen worden gebruikt door het maken van een **dummy-variabele** voor elke categorie instellen op 1 in gevallen waarin de categorie van toepassing zijn, 0, waar dit niet het geval.


<!-- This is how you can embed a link in an image in HTML. Don't know how to do this in markdown.
<a href="http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet.pdf">
<img src="C:\Users\garye\azure-docs-pr\articles\media\machine-learning-algorithm-cheat-sheet\cheat-sheet-small.png">
</a>
-->

<!-- Module References -->
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx
[initialize-model]: https://msdn.microsoft.com/library/azure/0c67013c-bfbc-428b-87f3-f552d8dd41f6/
[k-means-clustering]: https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/
[one-vs-all-multiclass]: https://msdn.microsoft.com/library/azure/7191efae-b4b1-4d03-a6f8-7205f87be664/
