---
title: Machine learning-overzichtskaart - Azure | Microsoft Docs
description: Een afdrukbaar machine learning-overzichtskaart helpt u bij de juiste algoritme voor uw voorspellende model kiezen in Azure Machine Learning Studio.
keywords: overzichtskaart algoritmen, referentiemateriaal voor, machine learning-algoritme
services: machine-learning
author: pakalra
ms.custom: (previous ms.author pakalra)
ms.author: amlstudiodocs
manager: cgronlun
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.component: studio
ms.topic: article
ms.date: 12/18/2017
ms.openlocfilehash: b43cc1ea84eefc53c9d51d4c7acfb9dd7390836d
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51822232"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-azure-machine-learning-studio"></a>Machine learning-overzichtskaart voor Azure Machine Learning Studio

De **Azure Machine Learning Algorithm Cheat Sheet** helpt u bij het kiezen van het juiste algoritme voor een predictive analytics-model.

[Azure Machine Learning Studio](https://studio.azureml.net/) heeft een grote bibliotheek met algoritmen van de ***regressie***, ***classificatie***, ***clustering***, en  ***detectie van afwijkingen*** families. Elk is erop gericht om een ander type machine learning-probleem.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Download: Machine learning-overzichtskaart algoritmen

**Het referentiemateriaal voor hier downloaden: [Machine Learning Algorithm Cheat blad (11 x 17.)](https://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v7.pdf)**

![Machine learning-overzichtskaart: meer informatie over het kiezen van een Machine Learning-algoritme.][cheat-sheet]

[cheat-sheet]: ./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

Downloaden en afdrukken van het Machine Learning Studio-algoritme referentiemateriaal blad in a3-grootte voor het houden bij de hand en hulp bij het kiezen van een algoritme.

> [!NOTE]
> Zie voor meer informatie over het gebruik van deze overzichtskaart voor het kiezen van het juiste algoritme, plus een uitgebreidere bespreking van de verschillende soorten machine learning-algoritmen en hoe ze worden gebruikt, [algoritmen kiezen voor Microsoft Azure Machine Learning](algorithm-choice.md).

## <a name="notes-and-terminology-definitions-for-the-machine-learning-studio-algorithm-cheat-sheet"></a>Notities en termen die voor het algoritme voor Machine Learning Studio Cheatsheet

* De aangeboden in deze overzichtskaart suggesties zijn bij benadering regels van de miniatuur. Sommige kunt worden verbogen en sommige flagrantly kunt worden geschonden. Dit is bedoeld om een beginpunt voorstellen. Wees niet bang voor het uitvoeren van een man competitie tussen verschillende algoritmen voor uw gegevens. Er is gewoon geen vervanging voor informatie over de beginselen van elk algoritme en inzicht in het systeem dat uw gegevens gegenereerd.

* Elke machine learning-algoritme heeft een eigen stijl of *inductieve vertekening*. Verschillende algoritmen is wellicht beter voor een specifiek probleem, en één algoritme mogelijk beter geschikt dan andere. Maar het is niet altijd mogelijk tevoren weten wat het meest geschikt is. In dergelijke gevallen worden verschillende algoritmes samen weergegeven in het referentiemateriaal voor. Een geschikte strategie zou worden omgezet naar één algoritme, en als de resultaten nog niet voldoende zijn, probeer de andere. Hier volgt een voorbeeld van de [Azure AI Gallery](http://gallery.azure.ai/) van een experiment dat probeert verschillende algoritmes op basis van dezelfde gegevens en vergelijkt de resultaten: [vergelijken ROC classificaties: Letter erkenning](http://gallery.azure.ai/Details/a635502fc98b402a890efe21cec65b92).

* Er zijn drie hoofdcategorieën van machine learning: **onder supervisie learning**, **leren zonder supervisie**, en **bekrachtigingen**.

  * In **onder supervisie learning**, elk gegevenspunt is met het label of die zijn gekoppeld aan een categorie of de waarde van belang zijn.  Een voorbeeld van een categorische label met het toewijzen van een afbeelding als een 'cat' of een 'hond'.  Een voorbeeld van een label voor waarde is de verkoopprijs die zijn gekoppeld aan een gebruikte auto. Het doel van leren met supervisie is om te bestuderen veel gelabelde voorbeelden zoals deze, en vervolgens te kunnen maken van voorspellingen over toekomstig gegevenspunten. Bijvoorbeeld, een nieuwe foto's identificeren met de juiste dieren of nauwkeurige verkoopprijzen toewijzen aan andere gebruikte auto's. Dit is een populaire en nuttige type machine learning. Zijn alle modules in Azure Machine Learning-algoritmen, behalve voor leren met supervisie [K-Means Clustering][k-means-clustering].

  * In **leren zonder supervisie**, gegevenspunten hebben geen labels worden gekoppeld. In plaats daarvan is het doel van een algoritme zonder supervisie learning om de gegevens op een bepaalde manier te ordenen of om te beschrijven van de structuur. Dit kan betekenen dat groeperen in clusters, zoals K-means of kijken naar complexe gegevens zodat deze wordt weergegeven eenvoudiger op verschillende manieren vinden.

  * In **bekrachtigingen**, de algoritme een actie te kiezen in reactie op elk gegevenspunt opgehaald. Het is een gebruikelijke aanpak in robotiek, waarbij de set sensorwaarden, serverlogs, op één punt in tijd een gegevenspunt, en het algoritme moet de volgende actie van de robot kiezen. Het is ook een natuurlijke geschikt voor Internet of Things-toepassingen. Het learning-algoritme ontvangt ook een signaal beloning korte tijd later, dat aangeeft hoe goed de beslissing is. Het algoritme wijzigt op basis hiervan de strategie om te realiseren van de hoogste prijs. Er zijn momenteel geen leren met bekrachtigingen beschikt learning-algoritme modules in Azure ML.

* **Bayesiaanse methoden** ervan uitgegaan van statistisch onafhankelijke gegevenspunten. Dit betekent dat de unmodeled variabiliteit bij één gegevenspunt is niet-gerelateerde met anderen, dat wil zeggen, niet kan worden voorspeld. Bijvoorbeeld, als de gegevens worden opgenomen, is het aantal minuten op waarna de volgende train van de trein binnenkomt, twee metingen per dag uit elkaar gehouden statistisch onafhankelijk zijn. Echter twee metingen bij een minuut bij elkaar niet statistisch onafhankelijk zijn: de waarde van een zeer voorspelling van de waarde van de andere is.

* **Boosted decision tree regressie** maakt gebruik van de functie overlapping of interactie tussen functies. Dit betekent dat dat in een bepaalde gegevenspunt, de waarde van een functie is enigszins voorspelling van de waarde van een andere. Bijvoorbeeld in temperatuurgegevens van dagelijkse hoog/laag kunt weet de laagste temperatuur voor die dag u een redelijke schatting met de hoge maken. De informatie in de twee functies is enigszins overbodig.

* Classificatie van gegevens in meer dan twee categorieën kan worden gedaan met behulp van een inherent ROC-classificatie of door een combinatie van een set van twee klassen classificaties in een **ensembles**. In de methode ensembles, er is een afzonderlijke classificatie van twee klassen voor elke klasse - elkaar scheidt de gegevens in twee categorieën: "deze class" en "niet deze klasse." Vervolgens worden deze classificaties stemmen op de juiste toewijzing van het gegevenspunt. Dit is de operationele principe achter [One-vs-All-Multiklasse][one-vs-all-multiclass].

* Verschillende methoden, met inbegrip van logistieke regressie en de Bayes point machine, wordt ervan uitgegaan dat **lineaire klassengrenzen**. Dat wil zeggen, ze gaan ervan uit dat de grenzen tussen klassen zijn ongeveer rechte lijnen (of hyperplanes in het algemene geval). Vaak is dit een kenmerk van de gegevens die u niet weet wat pas nadat u hebt geprobeerd endfirewall als afscheiding, maar er is iets dat normaal gesproken kan worden geleerd door het visualiseren van tevoren. Als de klassengrenzen van de zeer onregelmatige, houd u beslissingsstructuren, decision jungles, vector machines of neurale netwerken ondersteunen.

* Neurale netwerken kunnen worden gebruikt met categorische variabelen met het maken van een **dummy-variabele** voor elke categorie instellen op 1 in gevallen waar de categorie van toepassing is, 0 indien dit niet het geval.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een downloadbare infographic die worden beschreven van algoritmen en voorbeelden, [downloadbare Infographic: Machine learning-basisbeginselen met algoritmevoorbeelden](basics-infographic-with-algorithm-examples.md).

* Zie voor een lijst op categorie van alle de machine learning-algoritmen in Machine Learning Studio, [Initialize Model] [ initialize-model] in de algoritme voor Machine Learning Studio en de Module Help.

* Zie voor een volledige alfabetische lijst van algoritmen en modules in Machine Learning Studio, [alfabetische lijst van Machine Learning Studio-modules] [ a-z-list] in de Module Help en algoritme voor Machine Learning Studio.

* Zie [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](studio-overview-diagram.md) als u een diagram wilt downloaden en afdrukken met een overzicht van de mogelijkheden van Machine Learning Studio.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

<!-- Module References -->
[a-z-list]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/a-z-module-list
[initialize-model]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model
[k-means-clustering]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/k-means-clustering
[one-vs-all-multiclass]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/one-vs-all-multiclass
