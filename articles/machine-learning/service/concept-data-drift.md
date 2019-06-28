---
title: Afwijking van gegevens controleren (Preview)
titleSuffix: Azure Machine Learning service
description: Meer informatie over hoe Azure Machine Learning-service voor gegevens drift kunt bewaken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cody-dkdc
ms.author: copeters
ms.date: 06/20/2019
ms.openlocfilehash: 56761c32484d4f5b27800e56143c62d3731da852
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333185"
---
# <a name="what-is-data-drift-monitoring-preview"></a>Wat is data drift bewaking (Preview)?

Afwijking van gegevens is de wijziging in de distributie van gegevens. Getrainde machine learning-modellen kan voorspelling van verminderde prestaties optreden vanwege drift in de context van machine learning. Bewaking van de afwijking tussen het trainen van gegevens en gegevens die worden gebruikt voor het maken van voorspellingen kan u helpen prestatieproblemen detecteren.

Machine learning-modellen zijn alleen nuttig als de gegevens die worden gebruikt met het trainen van deze. Implementeren van modellen voor productie zonder toezicht van de prestaties kan leiden tot niet-gevonden en nadelige gevolgen. U kunt met gegevens drift bewaking, detecteren en kan worden aangepast aan de afwijking van de gegevens. 

## <a name="when-to-monitor-for-data-drift"></a>Wanneer om te controleren op gegevens drift?

Metrische gegevens die we kunnen controleren zijn onder andere:

+ Omvang van de afwijking (Drift coëfficiënt)
+ Oorzaak van de afwijking (Drift bijdrage per functie)
+ Afstand metrische gegevens (Wasserstein, energie, enz.)

Met deze controle op locatie, kunnen waarschuwingen of acties worden ingesteld wanneer afwijking wordt gedetecteerd en de gegevenswetenschapper de hoofdoorzaak van het probleem kunt onderzoeken. 

Als u denkt dat de invoergegevens voor uw geïmplementeerde model kunnen worden gewijzigd, moet u overwegen gegevens afwijking detectie.

## <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Hoe gegevens drift in Azure Machine Learning-service wordt bewaakt

Met behulp van **Azure Machine Learning-service**, afwijking van gegevens wordt bewaakt met gegevenssets of implementaties. Om te controleren op gegevens drift, is een basislijn-gegevensset - meestal de training gegevensset voor een model - opgegeven. Een tweede gegevensset - meestal model invoergegevens die zijn verzameld van een implementatie - is getest op basis van de basislijn-gegevensset. Beide gegevenssets zijn [geprofileerd](how-to-create-dataset-snapshots.md) en invoer voor de gegevens beginnen af te wijken monitoring-service. Een machine learning-model wordt getraind voor het detecteren van de verschillen tussen de twee gegevenssets. Prestaties van het model wordt geconverteerd naar de coëfficiënt drift, waarvan de omvang van de afwijking tussen de twee gegevenssets maatregelen. Met behulp van [interpretability model](machine-learning-interpretability-explainability.md) de functies die hebben bijgedragen aan de coëfficiënt drift worden berekend. Van het profiel gegevensset wordt statistische informatie over elke functie bijgehouden. 

## <a name="data-drift-metric-output"></a>Metrische gegevens drift-uitvoer

Er zijn meerdere manieren om drift metrische gegevens weer te geven:

* Gebruik de widget Jupyter.
* Gebruik de `get_metrics()` functie op een `datadriftRun` object.
* De metrische gegevens weergeven in de Azure-portal voor uw model

De volgende metrische gegevens worden opgeslagen in elke iteratie uitvoeren voor een taak van de afwijking gegevens:

|Gegevens|Description|
--|--|
wasserstein_distance|Statistische afstand gedefinieerd voor eendimensionale numerieke distributie.|
energy_distance|Statistische afstand gedefinieerd voor eendimensionale numerieke distributie.|
datadrift_coefficient|Formeel Matthews correlatiecoëfficiënt, een reëel getal tussen -1 en 1. 0 geeft aan dat er geen afwijking en 1 geeft aan dat de maximale drift in de context van de afwijking.|
datadrift_contribution|De urgentie van de functie van de functies die bijdragen aan het drift.|

## <a name="next-steps"></a>Volgende stappen

Zie de voorbeelden en informatie over het bewaken van gegevens drift:

+ [Informatie over het bewaken van gegevens drift in modellen die zijn geïmplementeerd via Azure Kubernetes Service (AKS)](how-to-monitor-data-drift.md)
+ Probeer [Jupyter-Notebook-voorbeelden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/data-drift/)