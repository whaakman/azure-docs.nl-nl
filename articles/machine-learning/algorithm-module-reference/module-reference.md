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
ms.openlocfilehash: 6602eb4bacdc3b6382c1b6873a465cdfc0632693
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029338"
---
# <a name="algorithm--module-reference-overview"></a>Overzicht van de naslaginformatie over algoritmen en modules

Deze referentie-inhoud biedt de technische achtergrond op elk van de machine learning-algoritmen en modules die beschikbaar zijn in de visuele interface (preview) van Azure Machine Learning-service. 

Elke module vertegenwoordigt een set van code die u kunt onafhankelijk uitgevoerd en uitvoeren van een machine learning-taak, de vereiste invoergegevens gegeven. Een module kan een bepaald algoritme bevatten of een taak uitvoeren die is belangrijk in machine learning, zoals ontbrekende waarde vervanging of statistische analyse. 

> [!TIP]
> In een experiment in de visuele interface krijgt u informatie over een specifieke module. Selecteer de module en selecteer vervolgens de **meer hulp** herstelkoppeling in de **Snelle Help** deelvenster.

Modules worden geordend op functionaliteit:

**Indeling gegevensconversies**

  + [Converteren naar CSV ](convert-to-csv.md)

**Gegevens invoer en uitvoer modules** het werk van het verplaatsen van gegevens van cloudbronnen in uw experiment. U kunt uw resultaten of tussenliggende gegevens schrijven naar Azure Storage, een SQL-database of Hive, tijdens het uitvoeren van een experiment of opslag in de cloud gebruiken voor het uitwisselen van gegevens tussen experimenten.  

  + [Gegevens importeren](import-data.md)

  + [Gegevens exporteren](export-data.md)

  + [Gegevens handmatig invoeren](enter-data-manually.md)


**Data transformation modules** bewerkingen op gegevens die uniek zijn voor machine learning, zoals normaliseren of binning van gegevens, functieselectie en dimensionaliteitsvermindering ondersteunen.

  + [Kolommen in gegevensset selecteren](select-columns-in-dataset.md)

  + [Metagegevens bewerken](edit-metadata.md)

  + [De ontbrekende gegevens opschonen](clean-missing-data.md)

  + [Kolommen toevoegen](add-columns.md)

  + [Rijen toevoegen](add-rows.md)

  + [Dubbele rijen verwijderen](remove-duplicate-rows.md)

  + [Split Data](split-data.md)

  + [Gegevens normaliseren](normalize-data.md)

  + [Partitie en steekproef](partition-and-sample.md)


**Machine learning-algoritmen** zoals clustering, voor ondersteuningsvectormachines of neurale netwerken, zijn beschikbaar in afzonderlijke modules waarmee u de machine learning-taak met de juiste parameters aanpassen.  
  + [Score-Model](score-model.md)

  + [Gegevens toewijzen aan Clusters ](assign-data-to-clusters.md)

  + [Train Model](train-model.md)

  + [Clustering-Model trainen](train-clustering-model.md)

  + [Model evalueren](evaluate-model.md)

  + [Transformatie toepassen](apply-transformation.md)

  + [Lineaire regressie](linear-regression.md)

  + [Neural Network regressie](neural-network-regression.md)

  + [Beslissingsforest met regressie](decision-forest-regression.md)

  + [Boosted Decision Tree regressie](boosted-decision-tree-regression.md)

  + [Two-Class Boosted-beslisboom](two-class-boosted-decision-tree.md)

  + [Two-Class Logistic Regression](two-class-logistic-regression.md)

  + [Multiklassen Logistic Regression](multiclass-logistic-regression.md)

  + [Multiklassen Neural Network](multiclass-neural-network.md)

  + [Beslissingsforest met multiklasse](multiclass-decision-forest.md)

  + [Gemiddelde Perceptron Two-Class](two-class-averaged-perceptron.md)

  + [Beslissingsforest met twee klassen](two-class-decision-forest.md)

  + [Two-Class Neural Network](two-class-neural-network.md)

  + [Two-Class Support Vector Machine](two-class-support-vector-machine.md)
  
  + [K-Means Clustering](k-means-clustering.md)


**Python-module** kunt u eenvoudig een aangepaste functie uitvoert. U de code schrijven en insluiten in een module, Python integreren in een experiment-service.
  + [Python-Script uitvoeren](execute-python-script.md)

  + [Python-Model maken](create-python-model.md)


