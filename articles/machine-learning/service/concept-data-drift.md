---
title: Gegevensdrijf bewaking (preview-versie)
titleSuffix: Azure Machine Learning service
description: Meer informatie over hoe Azure Machine Learning-service kan controleren op gegevens drift.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cody-dkdc
ms.author: copeters
ms.date: 06/20/2019
ms.openlocfilehash: 13f73718fabd711e9c71a56ac4537b2ebef8a411
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371082"
---
# <a name="what-is-data-drift-monitoring-preview"></a>Wat is gegevensdrijf bewaking (preview)?

Data drift is de wijziging in de distributie van gegevens. In de context van machine learning kunnen getrainde machine learning modellen gedegradeerde Voorspellings prestaties ondervinden vanwege drift. Het bewaken van de gegevens die worden gebruikt voor het maken van voor spellingen kunnen helpen bij het detecteren van prestatie problemen.

Machine learning-modellen zijn alleen net zo goed als de gegevens die worden gebruikt om ze te trainen. Het implementeren van modellen naar productie zonder de prestaties te bewaken, kunnen leiden tot niet-Detecteer bare en nadelige gevolgen. Met de bewaking van gegevens opslag kunt u gegevens drift detecteren en aanpassen. 

## <a name="when-to-monitor-for-data-drift"></a>Wanneer moet worden bewaakt voor gegevens drift?

De metrische gegevens die u kunt bewaken, zijn onder andere:

+ Grootte van de Drift (drijf coëfficiënt)
+ Oorzaak van de Drift (drift-bijdrage per functie)
+ Afstands metrieken (Wasserstein, energie, enz.)

Met deze bewaking kunnen waarschuwingen of acties worden ingesteld wanneer er drijf kracht wordt gedetecteerd en kan de gegevens wetenschapper de hoofd oorzaak van het probleem onderzoeken. 

Als u denkt dat de invoer gegevens voor uw geïmplementeerde model kunnen veranderen, kunt u overwegen de detectie van gegevens drift te gebruiken.

## <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Hoe gegevens drift wordt bewaakt in Azure Machine Learning-service

Met behulp van **Azure machine learning-service**wordt data drift bewaakt via gegevens sets of implementaties. Als u wilt controleren op gegevens drift, een basislijn gegevensset, meestal de trainings gegevensset voor een model, is opgegeven. Een tweede gegevensset-meestal model invoer gegevens die zijn verzameld van een implementatie, wordt getest op basis van de gegevensset van de basislijn planning. Beide gegevens sets [](how-to-explore-prepare-data.md#explore-with-summary-statistics) worden profileeerd en ingevoerd in de data drift-bewakings service. Een machine learning model wordt getraind om verschillen tussen de twee gegevens sets te detecteren. De prestaties van het model worden geconverteerd naar de snelheids coëfficiënt, die de grootte van de gegevens in de twee data sets meet. Het gebruik van [model](machine-learning-interpretability-explainability.md) interpreteert de functies die zijn bijgedragen aan de capaciteits coëfficiënt worden berekend. Vanuit het profiel gegevensset wordt statistische informatie over elke functie bijgehouden. 

## <a name="data-drift-metric-output"></a>Metrische uitvoer voor gegevens drift

Er zijn meerdere manieren om gegevens over de drift weer te geven:

* Gebruik de `RunDetails` [widget Jupyter](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py).
* Gebruik de `get_metrics()` functie voor elk `datadriftRun` object.
* De metrische gegevens in het Azure Portal in uw model weer geven

De volgende metrische gegevens worden opgeslagen in elke uitvoerings herhaling voor een Data drift-taak:

|Gegevens|Description|
--|--|
wasserstein_distance|Statistische afstand gedefinieerd voor eendimensionale sprei ding.|
energy_distance|Statistische afstand gedefinieerd voor eendimensionale sprei ding.|
datadrift_coefficient|Formeel Matthews correlatie coëfficiënt, een reëel getal tussen-1 en 1. In de context van drift duidt 0 op geen drift en 1 geeft de maximale waarde voor de drift aan.|
datadrift_contribution|Belang rijk onderdeel van functies die bijdragen aan drift.|

## <a name="next-steps"></a>Volgende stappen

Zie voor beelden en meer informatie over het bewaken van gegevens drift:

+ [Meer informatie over het bewaken van gegevens drift op modellen die worden geïmplementeerd via de Azure Kubernetes-service (AKS)](how-to-monitor-data-drift.md)
+ Voor [beelden van Jupyter notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/data-drift/) uitproberen