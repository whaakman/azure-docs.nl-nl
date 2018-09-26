---
title: Azure Machine Learning-service van machine learning-pijplijnen - bouwen
description: In dit artikel meer informatie over de machine learning-pijplijnen die u kunt maken met de Azure Machine Learning-SDK voor Python en de voordelen van pijplijnen. Machine learning (ML) pijplijnen worden gebruikt door de datawetenschappers te bouwen, te optimaliseren en beheren van de machine learning-werkstromen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 09/24/2018
ms.openlocfilehash: e1181d07ed51b1edf5da68d759d871703bd1540f
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162777"
---
# <a name="pipelines-and-azure-machine-learning"></a>Pijplijnen en Azure Machine Learning

In dit artikel meer informatie over de machine learning-pijplijnen die u kunt maken met de Azure Machine Learning-SDK voor Python en de voordelen van pijplijnen.

## <a name="what-are-machine-learning-pipelines"></a>Wat is machine learning-pijplijnen?

Machine learning (ML) pijplijnen worden gebruikt door de datawetenschappers te bouwen, te optimaliseren en beheren van de machine learning-werkstromen. Een typische pijplijn bestaat uit een reeks stappen die betrekking hebben op de volgende gebieden:

+ Voorbereiden van gegevens, zoals normalizations en transformaties
+ Modeltraining, zoals hyper-parameter afstemmen en valideren
+ Implementatie van modellen en evaluatie  

Het volgende diagram toont een voorbeeld van de pijplijn:

[ ![Machine learning-pijplijnen in Azure Machine Learning-service](./media/concept-ml-pipelines/pipelines.png) ] (. / media/concept-ml-pipelines/machine-learning-pipelines-big.png#lightbox)

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Waarom zou u pijplijnen met Azure Machine Learning bouwen?

De [Azure Machine Learning-SDK voor Python](#the-python-sdk-for-pipelines) maken ook ML-pijplijnen te verzenden en volgen van afzonderlijke pijplijnuitvoeringen kunnen worden gebruikt.

Met pijplijnen, kunt u uw werkstroom met de eenvoud, de snelheid, draagbaarheid en hergebruik optimaliseren. Bij het bouwen van pijplijnen met Azure Machine Learning, kunt u zich richten op wat u het beste kent &mdash; machine learning &mdash; in plaats van infrastructuur.

Met behulp van afzonderlijke stappen, maakt het mogelijk alleen de stappen die u nodig hebt bij het aanpassen en testen van de werkstroom opnieuw uit te voeren. Een stap is een rekenkundige eenheid in de pijplijn. Zoals u in het bovenstaande diagram, wordt de taak van het voorbereiden van gegevens kan betrekking hebben op veel stappen, waaronder, maar niet beperkt tot normalisering, transformatie, validatie en parametrisatie.

Zodra de pijplijn is ontworpen, is er vaak meer aan te passen om de lus training van de pijplijn. Wanneer u een pijplijn, de run koppelingen naar de stappen die opnieuw worden gestart moeten, zoals een bijgewerkte trainingsscript opnieuw uitvoeren en slaat over wat er nog niet is gewijzigd. De dezelfde paradigma is van toepassing op ongewijzigd scripts die worden gebruikt voor het uitvoeren van de stap. 

Met Azure Machine Learning, kunt u verschillende toolkits en frameworks, zoals Microsoft Cognitive Toolkit of TensorFlow voor elke stap in de pijplijn. Azure coördinaten tussen de verschillende [compute-doelen](concept-azure-machine-learning-architecture.md) u gebruiken zodat de tussentijdse gegevens kunnen worden gedeeld met de downstream compute-doelen eenvoudig. 

## <a name="key-advantages"></a>Belangrijkste voordelen

De belangrijkste voordelen voor het bouwen van pijplijnen voor uw machine learning-werkstromen is:

|Groot voordeel|Beschrijving|
|:-------:|-----------|
|**Zonder toezicht&nbsp;wordt uitgevoerd**|Plannen van een paar stappen voor het parallel of op volgorde worden uitgevoerd in een betrouwbare manier zonder toezicht. Sinds gegevensvoorbereiding en modellen kunt laatste dagen of weken, u kunt nu richten op andere taken terwijl uw pijplijn wordt uitgevoerd. |
|**Gemengde en diverse compute**|Gebruik meerdere pijplijnen die op een betrouwbare manier worden gecoördineerd in heterogene en schaalbare berekeningen en opslagruimten. Stappen voor afzonderlijke pijplijn kunnen worden uitgevoerd op andere compute updoelen, zoals HDInsight, GPU Data Science-VM's en Databricks, efficiënt gebruik van de beschikbare compute-opties.|
|**Herbruikbaarheid**|Pijplijnen kunnen worden templatized voor specifieke scenario's zoals opnieuw trainen en batch scoren.  Ze kunnen worden geactiveerd in externe systemen via eenvoudige REST-aanroepen.|
|**Wijzigingen bijhouden en versiebeheer**|In plaats van handmatig bijhouden van gegevens en het resultaat paden als u herhalen, met de SDK van de pijplijnen expliciet een naam en versie uw gegevens gegevensbronnen, invoer en uitvoer ook scripts en gegevens voor meer productiviteit afzonderlijk beheren.|

## <a name="the-python-sdk-for-pipelines"></a>De Python-SDK voor pijplijnen

Python gebruiken om te maken van uw ML-pijplijnen. De SDK van Azure Machine Learning biedt imperatieve constructies voor sequentiëren en gebruik de stappen in uw pijplijnen wanneer niet afhankelijk van de gegevens aanwezig is. U kunt er interactie mee in Jupyter-notebooks of in een andere gewenste IDE. 

Met behulp van declaratieve gegevensafhankelijkheden, kunt u uw taken optimaliseren. De SDK bevat een raamwerk van vooraf gemaakte modules voor algemene taken, zoals gegevens overdragen,-doel het maken COMPUTE en model van publiceren. Het framework kan worden uitgebreid voor het modelleren van uw eigen conventies door het implementeren van aangepaste stappen die herbruikbare voor pijplijnen zijn.

Pijplijnen kunnen worden opgeslagen als sjabloon en kunnen worden geïmplementeerd op een REST-eindpunt, zodat u kunt batch scoring of retraining taken plannen.

Bekijk de [Python SDK-referentiedocumenten voor pijplijnen](http://aka.ms/aml-sdk) en het notitieblok in de volgende sectie voor informatie over het bouwen van uw eigen.

## <a name="example-notebooks"></a>Voorbeeld-laptops
 
De volgende notebook ziet u pijplijnen met Azure Machine Learning: `pipeline/pipeline-batch-scoring.ipynb`.
 
Deze laptops ophalen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
