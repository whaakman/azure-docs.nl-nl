---
title: 'Pijp lijnen: machine learning werk stromen optimaliseren'
titleSuffix: Azure Machine Learning service
description: In dit artikel meer informatie over de machine learning-pijplijnen die u kunt maken met de Azure Machine Learning-SDK voor Python en de voordelen van pijplijnen. Machine learning (ML) pijplijnen worden gebruikt door de datawetenschappers te bouwen, te optimaliseren en beheren van de machine learning-werkstromen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: a9965dbbca939f566048312af921061a188ee50d
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884251"
---
# <a name="build-reusable-ml-pipelines-in-azure-machine-learning-service"></a>Herbruikbare ML-pijp lijnen bouwen in Azure Machine Learning-service

In dit artikel vindt u informatie over de machine learning-pijp lijnen die u kunt bouwen met de Azure Machine Learning SDK voor python en de voor delen van het gebruik van pijp lijnen.

## <a name="what-are-machine-learning-pipelines"></a>Wat is machine learning-pijplijnen?

Met behulp van machine learning (ML) pijplijnen, gegevenswetenschappers, gegevensengineers en IT-professionals kan samenwerken in de stappen voor het:
+ Voorbereiden van gegevens, zoals normalizations en transformaties
+ Modeltraining
+ Model-evaluatie
+ Implementatie

In het volgende diagram ziet u een voor beeld van een pijplijn proces:

![Machine learning-pijp lijnen in Azure Machine Learning-service](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Welke Azure pipeline-technologie moet ik gebruiken?

De Azure-Cloud biedt verschillende andere pijp lijnen, elk met een ander doel. De volgende tabel geeft een lijst van de verschillende pijp lijnen en waarvoor ze worden gebruikt:

| Pijplijn | Wat het doet | Canonieke pijp |
| ---- | ---- | ---- |
| Azure Machine Learning pijp lijnen | Hiermee definieert u herbruikbare machine learning werk stromen die kunnen worden gebruikt als sjabloon voor uw machine learning scenario's. | Model voor gegevens > |
| [Azure Data Factory-pijplijnen](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Groepeert gegevens verplaatsing, trans formatie en controle activiteiten die nodig zijn om een taak uit te voeren.  | Gegevens > gegevens |
| [Azure-pijp lijnen](https://azure.microsoft.com/services/devops/pipelines/) | Continue integratie en levering van uw toepassing op elk platform/elke Cloud  | Code-> app/service |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Waarom zou u pijplijnen met Azure Machine Learning bouwen?

U kunt de [Azure machine learning SDK voor python](#the-python-sdk-for-pipelines) gebruiken om ml-pijp lijnen te maken, en om afzonderlijke pijplijn uitvoeringen te verzenden en bij te houden.

Met pijplijnen, kunt u uw werkstroom met de eenvoud, de snelheid, draagbaarheid en hergebruik optimaliseren. Wanneer u pijp lijnen met Azure Machine Learning bouwt, kunt u zich richten op uw expertise, machine learning in plaats van de infra structuur en Automation.

Pijp lijnen worden samengesteld uit meerdere **stappen**, die afzonderlijke reken kundige eenheden in de pijp lijn zijn. Elke stap kan onafhankelijk worden uitgevoerd en geïsoleerde reken bronnen gebruiken. Op deze manier kunnen meerdere gegevens wetenschappers tegelijkertijd op dezelfde pijp lijn werken zonder dat er meer belasting bronnen hoeft te worden gebruikt, en kunt u voor elke stap eenvoudig verschillende reken typen of-grootten gebruiken.

Nadat de pijp lijn is ontworpen, is er vaak meer nauw keuriger om de training van de pijp lijn te verfijnen. Wanneer u een pijp lijn opnieuw uitvoert, springt de uitvoering naar de afzonderlijke stappen die opnieuw moeten worden uitgevoerd, zoals een bijgewerkt trainings script, en wordt wat er niet is gewijzigd, overgeslagen. De dezelfde paradigma is van toepassing op ongewijzigd scripts die worden gebruikt voor het uitvoeren van de stap. Deze functionaliteit helpt te voor komen dat er dure en tijdrovende stappen zoals gegevens opname en trans formatie worden uitgevoerd als de onderliggende gegevens niet zijn gewijzigd.

Met Azure Machine Learning kunt u verschillende tool kits en frameworks, zoals PyTorch of tensor flow, gebruiken voor elke stap in de pijp lijn. Azure-coördinaten tussen de verschillende [reken doelen](concept-azure-machine-learning-architecture.md) die u gebruikt, zodat uw tussenliggende gegevens eenvoudig kunnen worden gedeeld met de downstream-reken doelen.

U kunt [bijhouden van de metrische gegevens voor uw experimenten pijplijn](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) rechtstreeks in Azure portal. Nadat een pijp lijn is gepubliceerd, kunt u een REST-eind punt configureren waarmee u de pijp lijn vanaf elk platform of elke stack kunt uitvoeren.

## <a name="key-advantages"></a>Belangrijkste voordelen

De belangrijkste voor delen van het gebruik van pijp lijnen voor uw machine learning-werk stromen zijn:

|Groot voordeel|Description|
|:-------:|-----------|
|**Zonder toezicht&nbsp;wordt uitgevoerd**|Stappen plannen om parallel of op een betrouw bare en zonder toezicht te worden uitgevoerd. De voor bereiding en het model leren van gegevens kunnen de afgelopen dagen of weken zijn en met pijp lijnen kunt u zich richten op andere taken terwijl het proces wordt uitgevoerd. |
|**Heterogene compute**|Gebruik meerdere pijp lijnen die betrouwbaar zijn afgestemd op heterogene en schaal bare reken resources en opslag locaties. Voer afzonderlijke pijplijn stappen uit op verschillende reken doelen, zoals HDInsight, GPU data Science Vm's en Databricks. Dit maakt efficiënt gebruik van beschik bare reken opties.|
|**Herbruikbaarheid**|Maak pijplijn sjablonen voor specifieke scenario's, zoals retraining en batch-scores. Activeer gepubliceerde pijp lijnen van externe systemen via eenvoudige REST-aanroepen.|
|**Wijzigingen bijhouden en versiebeheer**|In plaats van gegevens en resultaat paden hand matig te traceren tijdens het herhalen, gebruikt u de SDK van de pijp lijnen om uw gegevens bronnen, invoer en uitvoer expliciet een naam en versie te gegeven. U kunt ook scripts en gegevens afzonderlijk beheren voor een verhoogde productiviteit.|
|**Werking**|Met pijp lijnen kunnen gegevens wetenschappers samen werken aan alle gebieden van het machine learning-ontwerp proces, terwijl ze gelijktijdig kunnen werken aan pijplijn stappen.|

## <a name="the-python-sdk-for-pipelines"></a>De Python-SDK voor pijplijnen

Python gebruiken om te maken van uw ML-pijplijnen. De SDK van Azure Machine Learning biedt imperatieve constructies voor sequentiëren en gebruik de stappen in uw pijplijnen wanneer niet afhankelijk van de gegevens aanwezig is. U kunt hiermee communiceren in Jupyter-notebooks of in een andere voor Keurs-IDE.

Met behulp van declaratieve gegevensafhankelijkheden, kunt u uw taken optimaliseren. De SDK bevat een Framework met vooraf ontwikkelde modules voor algemene taken, zoals gegevens overdracht en model publicatie. U kunt het framework uitbreiden om uw eigen conventies te model leren door aangepaste stappen te implementeren die opnieuw kunnen worden gebruikt in pijp lijnen. U kunt ook Compute-doelen en opslag bronnen rechtstreeks vanuit de SDK beheren.

U kunt pijp lijnen opslaan als sjablonen en deze implementeren in een REST-eind punt, zodat u taken voor batch scores of herscholing kunt plannen.

Zie de [documentatie van PYTHON SDK voor pijp lijnen](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) en het notitie blok in de volgende sectie voor meer informatie over het bouwen van uw eigen document.

## <a name="example-notebooks"></a>Voorbeeld-laptops

De volgende notebooks demonstreren pijplijnen met Azure Machine Learning: [how-to-use-azureml/machine-learning-pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [uw eerste pijplijn](how-to-create-your-first-pipeline.md).
