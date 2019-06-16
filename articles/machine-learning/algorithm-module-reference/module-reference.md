---
title: Naslaginformatie over algoritmen en modules
titleSuffix: Azure Machine Learning service
description: Meer informatie over de modules die beschikbaar zijn in de visuele interface van Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 8825f1dc3b66a5c4981ba25a90813aec63975b1f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65145136"
---
# <a name="algorithm--module-reference-overview"></a>Overzicht van de naslaginformatie over algoritmen en modules

Deze referentie-inhoud biedt de technische achtergrond op elk van de machine learning-algoritmen en modules die beschikbaar zijn in de visuele interface (preview) van Azure Machine Learning-service.

Elke module vertegenwoordigt een set van code die u kunt onafhankelijk uitgevoerd en uitvoeren van een machine learning-taak, de vereiste invoergegevens gegeven. Een module kan een bepaald algoritme bevatten of een taak uitvoeren die is belangrijk in machine learning, zoals ontbrekende waarde vervanging of statistische analyse.

> [!TIP]
> In een experiment in de visuele interface krijgt u informatie over een specifieke module. Selecteer de module en selecteer vervolgens de **meer hulp** herstelkoppeling in de **Snelle Help** deelvenster.

## <a name="modules"></a>Modules

Modules worden geordend op functionaliteit:

| Functionaliteit | Description | Module |
| --- |--- | ---- |
| Conversies gegevensindeling | Converteren van gegevens tussen verschillende bestandsindelingen die worden gebruikt in machine learning | [Converteren naar CSV](convert-to-csv.md) |
| Invoer en uitvoer | Gegevens verplaatsen van cloudbronnen in uw experiment. Uw resultaten of tussenliggende gegevens schrijven naar Azure Storage, een SQL-database of Hive, tijdens het uitvoeren van een experiment, of opslag in de cloud gebruiken voor het uitwisselen van gegevens tussen experimenten.  | [Gegevens importeren](import-data.md)<br/>[Gegevens exporteren](export-data.md)<br/>[Gegevens handmatig invoeren](enter-data-manually.md) |
| Gegevenstransformatie | Bewerkingen op gegevens die uniek zijn voor machine learning, zoals normaliseren of binning van gegevens, functieselectie en dimensionaliteitsvermindering.| [Kolommen in gegevensset selecteren](select-columns-in-dataset.md) <br/> [Metagegevens bewerken](edit-metadata.md) <br/> [De ontbrekende gegevens opschonen](clean-missing-data.md) <br/> [Kolommen toevoegen](add-columns.md) <br/> [Rijen toevoegen](add-rows.md) <br/> [Dubbele rijen verwijderen](remove-duplicate-rows.md) <br/> [Split Data](split-data.md) <br/> [Gegevens normaliseren](normalize-data.md) <br/> [Partitie en steekproef](partition-and-sample.md) |
| Python-module | Code schrijven en is ingesloten in een module Python integreren in uw experiment. | [Python-Script uitvoeren](execute-python-script.md)   <br/> [Python-Model maken](create-python-model.md)
|  | **Machine learning-algoritmen**: | |
| Classificatie | Een klasse te voorspellen.  Kies in het binaire bestand (twee klassen) of algoritmen voor multiklassen.| [Beslissingsforest met multiklasse](multiclass-decision-forest.md) <br/> [Multiklassen Logistic Regression](multiclass-logistic-regression.md)  <br/> [Multiklassen Neural Network](multiclass-neural-network.md)  <br/>  [Two-Class Logistic Regression](two-class-logistic-regression.md)  <br/>[Gemiddelde Perceptron Two-Class](two-class-averaged-perceptron.md) <br/> [Two-Class&nbsp;heeft de Klantenopbrengst&nbsp;besluit&nbsp;structuur](two-class-boosted-decision-tree.md)  <br/> [Beslissingsforest met twee klassen](two-class-decision-forest.md)  <br/> [Two-Class Neural Network](two-class-neural-network.md)  <br/> [Twee&#8209;klasse&nbsp;ondersteuning&nbsp;Vector&nbsp;Machine](two-class-support-vector-machine.md) 
| Clustering | Gegevens groeperen.| [K-Means Clustering](k-means-clustering.md)
| Regressie | Een waarde te voorspellen. | [Lineaire regressie](linear-regression.md)  <br/> [Neural Network regressie](neural-network-regression.md)  <br/> [Beslissingsforest met regressie](decision-forest-regression.md)  <br/> [Heeft de klantenopbrengst&nbsp;besluit&nbsp;structuur&nbsp;regressie](boosted-decision-tree-regression.md)
|  | **Bouwen en evalueren van modellen**: | |
| Trainen   | Gegevens worden uitgevoerd door de algoritme. | [Train Model](train-model.md)  <br/> [Clustering-Model trainen](train-clustering-model.md)    |
| Model evalueren | Meet de nauwkeurigheid van het getrainde model. |  [Model evalueren](evaluate-model.md)
| Score | Voorspellingen ophalen uit het model dat u zojuist hebt getraind. | [Transformatie toepassen](apply-transformation.md)<br/>[Toewijzen&nbsp;gegevens&nbsp;naar&nbsp;Clusters](assign-data-to-clusters.md) <br/>[Score-Model](score-model.md)

## <a name="error-messages"></a>Foutberichten

Meer informatie over de [foutberichten en uitzondering codes](machine-learning-module-error-codes.md) die optreden met behulp van modules in de visuele interface van Azure Machine Learning-service.
