---
title: 'Pijplijnen: machine learning-werkstromen optimaliseren'
titleSuffix: Azure Machine Learning service
description: In dit artikel meer informatie over de machine learning-pijplijnen die u kunt maken met de Azure Machine Learning-SDK voor Python en de voordelen van pijplijnen. ML-pijplijnen (machine learning) worden door datawetenschappers gebruikt om hun machine learning-werkstromen te ontwikkelen, optimaliseren en beheren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 12/4/2018
ms.custom: seodec18
ms.openlocfilehash: 917bac81923650405c37dfee500c9606dc7c54ca
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56816603"
---
# <a name="build-machine-learning-pipelines-with-the-azure-machine-learning-service"></a>Machine learning-pijplijnen met de Azure Machine Learning-service bouwen

In dit artikel meer informatie over de machine learning-pijplijnen die u kunt maken met de Azure Machine Learning-SDK voor Python en de voordelen van pijplijnen.

## <a name="what-are-machine-learning-pipelines"></a>Wat is machine learning-pijplijnen?

Met behulp van machine learning (ML) pijplijnen, gegevenswetenschappers, gegevensengineers en IT-professionals kan samenwerken in de stappen voor het:
+ Voorbereiden van gegevens, zoals normalizations en transformaties
+ Modeltraining
+ Model-evaluatie
+ Implementatie 

Het volgende diagram toont een voorbeeld van de pijplijn:

![Machine learning-pijplijnen in Azure Machine Learning-service](./media/concept-ml-pipelines/pipelines.png)

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Waarom zou u pijplijnen met Azure Machine Learning bouwen?

U kunt de [Azure Machine Learning-SDK voor Python](#the-python-sdk-for-pipelines) ML pijplijnen, evenals te verzenden en volgen van afzonderlijke pijplijnuitvoeringen maken.

Met pijplijnen, kunt u uw werkstroom met de eenvoud, de snelheid, draagbaarheid en hergebruik optimaliseren. Bij het bouwen van pijplijnen met Azure Machine Learning, kunt u zich richten op uw expertise, machine learning, in plaats van op de infrastructuur.

Met behulp van afzonderlijke stappen, maakt het mogelijk alleen de stappen die u nodig hebt, zoals u aanpassen en testen van de werkstroom opnieuw uit te voeren. Een stap is een rekenkundige eenheid in de pijplijn. Zoals weergegeven in het vorige diagram, kan de taak van het voorbereiden van gegevens hebben betrekking op veel stappen. Deze omvatten, maar niet beperkt tot normalisering, transformatie, validatie en parametrisatie. Gegevensbronnen en tussenliggende gegevens worden hergebruikt voor de pijplijn, welke bespaart tijd en resources berekenen. 

Nadat de pijplijn is ontworpen, is er vaak meer aan te passen om de lus training van de pijplijn. Wanneer u een pijplijn, de run koppelingen naar de stappen die opnieuw worden gestart moeten, zoals een bijgewerkte trainingsscript opnieuw uitvoeren en slaat over wat er nog niet is gewijzigd. De dezelfde paradigma is van toepassing op ongewijzigd scripts die worden gebruikt voor het uitvoeren van de stap. 

Met Azure Machine Learning, kunt u verschillende toolkits en frameworks, zoals Microsoft Cognitive Toolkit of TensorFlow, om voor elke stap in de pijplijn. Azure coördinaten tussen de verschillende [compute-doelen](concept-azure-machine-learning-architecture.md) u gebruikt, zodat de tussentijdse gegevens kunnen worden gedeeld met de downstream compute-doelen eenvoudig. 

U kunt [bijhouden van de metrische gegevens voor uw experimenten pijplijn](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) rechtstreeks in Azure portal. 

## <a name="key-advantages"></a>Belangrijkste voordelen

De belangrijkste voordelen voor het bouwen van pijplijnen voor uw machine learning-werkstromen zijn:

|Groot voordeel|Description|
|:-------:|-----------|
|**Zonder toezicht&nbsp;wordt uitgevoerd**|Plan een aantal stappen die betrouwbaar en zonder toezicht gelijktijdig of na elkaar moeten worden uitgevoerd. Omdat gegevensvoorbereiding en modellen kunnen afgelopen dagen of weken, u kunt nu richten op andere taken terwijl uw pijplijn wordt uitgevoerd. |
|**Gemengde en diverse compute**|Gebruik meerdere pijplijnen die op een betrouwbare manier worden gecoördineerd in heterogene en schaalbare berekeningen en opslagruimten. U kunt afzonderlijke pijplijn stappen uitvoeren op verschillende compute-doelen, zoals HDInsight, GPU Data Science-VM's en Databricks. Dit maakt efficiënter gebruik van beschikbare compute-opties.|
|**Herbruikbaarheid**|U kunt sjablonen gebruiken voor pijplijnen voor specifieke scenario's, zoals het opnieuw trainen en batch scoren. Deze activeren van externe systemen via eenvoudige REST-aanroepen.|
|**Wijzigingen bijhouden en versiebeheer**|In plaats van handmatig bijhouden van gegevens en het resultaat paden als u herhalen, met de SDK van de pijplijnen expliciet een naam en versie van uw gegevens gegevensbronnen, invoer en uitvoer. U kunt ook scripts en gegevens voor meer productiviteit afzonderlijk beheren.|

## <a name="the-python-sdk-for-pipelines"></a>De Python-SDK voor pijplijnen

Python gebruiken om te maken van uw ML-pijplijnen. De SDK van Azure Machine Learning biedt imperatieve constructies voor sequentiëren en gebruik de stappen in uw pijplijnen wanneer niet afhankelijk van de gegevens aanwezig is. U kunt er interactie mee in Jupyter-notebooks, of in een andere gewenste geïntegreerde ontwikkelomgeving. 

Met behulp van declaratieve gegevensafhankelijkheden, kunt u uw taken optimaliseren. De SDK bevat een raamwerk van vooraf gemaakte modules voor algemene taken, zoals de overdracht van gegevens en model publiceren. U kunt het framework voor het modelleren van uw eigen conventies, door het implementeren van herbruikbare aangepaste stappen in pijplijnen kunt uitbreiden. U kunt ook rechtstreeks vanuit de SDK met compute-doelen en storage-resources beheren.

U kunt pijplijnen opslaan als sjabloon en deze implementeren naar een REST-eindpunt, zodat u kunt batch scoring of retraining taken plannen.

Voor informatie over het bouwen van uw eigen, Zie de [Python SDK-referentiedocumenten voor pijplijnen](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) en het notitieblok in de volgende sectie.

## <a name="example-notebooks"></a>Voorbeeld-laptops
 
De volgende notebooks demonstreren pijplijnen met Azure Machine Learning: [how-to-use-azureml/machine-learning-pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines).
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [uw eerste pijplijn](how-to-create-your-first-pipeline.md).
