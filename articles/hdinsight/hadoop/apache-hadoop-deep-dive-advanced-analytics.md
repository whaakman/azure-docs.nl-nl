---
title: Diepgaand - geavanceerde analyses - Azure HDInsight | Microsoft Docs
description: Meer informatie over hoe geavanceerde analyses algoritmen gebruikt voor het verwerken van big data.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 61a00c902be5cd3e37dabba09c15f9226e5e88b4
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="deep-dive---advanced-analytics"></a>Diepgaand - geavanceerde analyses

## <a name="what-is-advanced-analytics-for-hdinsight"></a>Wat is een geavanceerde analyses voor HDInsight?

HDInsight biedt de mogelijkheid om op te halen inzicht van grote hoeveelheden gestructureerde, niet-gestructureerde en gegevens snel verplaatsen. Geavanceerde analyses is het gebruik van uiterst schaalbare architecturen statistische en machine learning-modellen en intelligente dashboards om te voorzien van betekenisvolle inzichten. Machine learning, of *predictive analytics*, identificeren en te leren van relaties in uw gegevens om voorspellingen en helpen uw beslissingen algoritmen gebruikt.

## <a name="advanced-analytics-process"></a>Geavanceerde analyses proces

![Proces](./media/apache-hadoop-deep-dive-advanced-analytics/process.png)

Nadat u het zakelijke probleem hebt geïdentificeerd en hebt gestart met het verzamelen en verwerken van uw gegevens die u wilt maken van een model voor de vraag die u wilt voorspellen. Het model wordt een of meer machine learning-algoritmen gebruiken om het type van de voorspelling die het beste past bij de behoeften van uw bedrijf.  Het merendeel van uw gegevens moet worden gebruikt voor het trainen van het model met de rest gebruikt om te testen of deze evalueren. 

Nadat u maken, laden, testen en evalueren van uw model, worden de volgende stap is voor het implementeren van uw model zodat deze begint met het verstrekken van antwoorden op vragen. De laatste stap is om te controleren van de prestaties van uw model en optimaliseren het indien nodig. 

## <a name="common-types-of-algorithms"></a>Algemene typen algoritmen

Geavanceerde analytics-oplossingen bieden een set van machine learning-algoritmen. Hier volgt een samenvatting van de categorieën van algoritmen en de gekoppelde algemene zakelijke gebruiksvoorbeelden.

![Gebruiksvoorbeelden voor machine Learning](./media/apache-hadoop-deep-dive-advanced-analytics/ml-use-cases.png)

Samen met de best passende verifiëringsalgoritme (s) selecteren, moet u rekening houden of moet u gegevens opgeven voor training. Machine learning-algoritmen zijn als volgt worden ingedeeld:

* Onder supervisie - moet algoritme worden getraind voor een set gelabelde gegevens voordat deze resultaten kan opgeven
* Semi onder supervisie - algoritme kan worden uitgebreid door extra doelen via interactieve query door een trainer die niet beschikbaar tijdens de eerste fase van de training waren
* Zonder supervisie - hoeft algoritme niet te trainen van gegevens 
* Versterking - algoritme maakt gebruik van softwareagents om te bepalen ideaal gedrag binnen een specifieke context (vaak gebruikt in robotics)


| Algoritme-categorie| Gebruiken | Learning Type | Algoritmen |
| --- | --- | --- | -- |
| Classificatie | Personen of dingen classificeren in groepen | Onder supervisie | Beslissingsstructuren, Logistic regression, neural networks |
| Clustering | Een reeks voorbeelden verdelen in groepen homogene | Zonder supervisie | K-means clustering |
| Patroon detectie | Veelvuldig koppelingen in de gegevens identificeren | Zonder supervisie | Koppeling regels |
| Regressie | Numerieke resultaten te voorspellen | Onder supervisie | Lineaire regressie, neural networks |
| Versterking | Optimale gedrag voor robots bepalen | Versterking | Monte Carlo-simulaties, DeepMind |

## <a name="machine-learning-on-hdinsight"></a>Machine learning in HDInsight

HDInsight heeft meerdere machine learning-opties voor een geavanceerde analyses werkstroom:

* [Machine Learning en Spark](#machine-learning-and-spark)
* [R en R Server](#r-and-r-server)
* [Azure Machine Learning en Hive](#azure-machine-learning-and-hive)
* [Spark en grondige leren](#spark-and-deep-learning)

### <a name="machine-learning-and-spark"></a>Machine Learning en Spark

[HDInsight Spark](../spark/apache-spark-overview.md) is een aanbieding voor Azure gehoste van [Spark](http://spark.apache.org/), een uniforme, open-source, framework voor parallelle verwerking van de gegevens die in-memory verwerking gebruikt voor het verbeteren van Big Data-analyses. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak en geavanceerde analyses. De Spark in-memory gedistribueerde rekencapaciteiten kunnen u een goede keuze voor de herhalende algoritmen in machine learning- en grafiekberekeningen. 

Er zijn drie schaalbare machine learning-bibliotheken die algoritmische modellering mogelijkheden naar deze gedistribueerde omgeving meenemen:

* [**MLlib** ](https://spark.apache.org/docs/latest/ml-guide.html) -MLlib bevat de oorspronkelijke API boven op Spark RDDs gebouwd.
* [**SparkML** ](https://spark.apache.org/docs/1.2.2/ml-guide.html) -SparkML is een nieuwere pakket een hoger niveau API gebouwd boven op Spark DataFrames biedt voor het maken van ML pijplijnen.
* [**MMLSpark** ](https://github.com/Azure/mmlspark) : de Microsoft-Machine Learning-bibliotheek voor Apache Spark (MMLSpark) is ontworpen om gegevenswetenschappers productiever op Spark, verhoogt de snelheid van experimenteren en gebruik van geavanceerde machine learning technieken, waaronder grondige kennis, op zeer grote gegevenssets. De bibliotheek MMLSpark vereenvoudigt algemene modellering taken voor het bouwen van modellen in PySpark. 

### <a name="r-and-r-server"></a>R en R Server

Als onderdeel van HDInsight, kunt u een HDInsight-cluster met [R Server](../r-server/r-server-overview.md) gereed om te worden gebruikt met grote gegevenssets en -modellen. Deze nieuwe mogelijkheid biedt gegevenswetenschappers en statistici met een bekend R-interface die kunnen worden geschaald op aanvraag via HDInsight, zonder de overhead van het cluster instellen en beheren.

### <a name="azure-machine-learning-and-hive"></a>Azure Machine Learning en Hive

[Azure Machine Learning Studio](https://studio.azureml.net/) biedt hulpprogramma's waarmee model predictive analytics, evenals een volledig beheerde service die u kunt gebruiken voor het implementeren van voorspellende modellen als webservices gereed om te gebruiken. Azure Machine Learning biedt hulpprogramma's voor het maken van volledige predictive analytics-oplossingen in de cloud snel maken, testen, operationeel maken en beheren van voorspellende modellen. Selecteer in een grote bibliotheek met algoritmen, een webgebaseerde studio gebruiken voor het bouwen van modellen en uw model als een webservice eenvoudig te implementeren.

### <a name="spark-and-deep-learning"></a>Spark en grondige leren

[Grondige learning](https://www.microsoft.com/research/group/dltc/) is een vertakking van machine learning die gebruikmaakt van *diep neural networks* (DNNs) naar aanleiding van de biologische processen van de menselijke brain. Grondige learning zien veel onderzoekers als een ordertoezegging benadering voor kunstmatige intelligence. Enkele voorbeelden van grondige learning zijn gesproken vertalers, afbeelding erkenning systemen en machine redenering. Om te gaan een eigen werk in grondige learning, Microsoft de gratis, eenvoudig te gebruiken, open-source heeft ontwikkeld [Microsoft cognitieve Toolkit](https://www.microsoft.com/cognitive-toolkit/). De toolkit wordt intensief gebruikt door een groot aantal verschillende Microsoft-producten, die een hebben voor het implementeren van grondige learning op grote schaal wereldwijd bedrijven en studenten geïnteresseerd in de meest recente algoritmen en technieken. 

## <a name="scenario---score-images-to-identify-patterns-in-urban-development"></a>Scenario - Score installatiekopieën naar patronen in stedelijk ontwikkeling identificeren

We bekijken een voorbeeld van een geavanceerde analyses machine learning-pijplijn met HDInsight.

In dit scenario ziet u hoe DNNs geproduceerd in het kader van een grondige learning Microsoft cognitieve Toolkit (CNTK), van kan worden geoperationaliseerd voor score berekenen voor grote afbeelding verzamelingen die zijn opgeslagen in een Azure Blob Storage-account met PySpark op een HDInsight Spark-cluster. Deze methode wordt toegepast op een algemene DNN gebruiksvoorbeeld lucht installatiekopie-indeling, en kan worden gebruikt voor het identificeren van recente patronen in stedelijk ontwikkeling.  Gebruikt u een installatiekopie van het vooraf getraind classificatie-model. Het model is vooraf getraind op de [CIFAR 10 gegevensset](https://www.cs.toronto.edu/~kriz/cifar.html) en tot 10.000 ingehouden afbeeldingen is toegepast.

Er zijn drie belangrijke taken in dit scenario voor geavanceerde analyses:

1. Maak een Azure HDInsight Hadoop-cluster met een Apache Spark 2.1.0 distributiepunt. 
2. Voer een aangepast script voor het installeren van Microsoft cognitieve Toolkit op alle knooppunten van een Azure HDInsight Spark-cluster. 
3. Een vooraf samengestelde Jupyter-notebook met uw HDInsight Spark-cluster te uploaden naar het toepassen van een getraind Microsoft cognitieve Toolkit grondige learning-model voor bestanden in een Azure Blob Storage-Account met de Spark Python-API (PySpark). 

In dit voorbeeld wordt de installatiekopie van het CIFAR 10 gecompileerde en wordt geleverd door Alex Krizhevsky Vinod Nair en Geoffrey Hinton ingesteld. De CIFAR-10-gegevensset bevat 60.000 32 x 32 kleur installatiekopieën die behoren tot 10 sluiten elkaar wederzijds uit klassen:

![Installatiekopieën](./media/apache-hadoop-deep-dive-advanced-analytics/ml-images.png)

Zie voor meer informatie over de dataset van Alex Krizhevsky [Learning meerdere lagen van functies van kleine afbeeldingen](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf).

De gegevensset is gepartitioneerd in een trainingset 50.000 installatiekopieën en geen testset van 10.000 afbeeldingen. De eerste set is gebruikt voor het gebruik van Microsoft cognitieve Toolkit door twintig laag diepe convolutional achtergebleven netwerk (ResNet) model trainen [in deze zelfstudie](https://github.com/Microsoft/CNTK/tree/master/Examples/Image/Classification/ResNet) van de cognitieve Toolkit GitHub-opslagplaats. De resterende 10.000 afbeeldingen werden gebruikt voor het testen van de nauwkeurigheid van het model. Dit is waar gedistribueerde computeromgevingen binnenkomt in play: de taak van vooraf verwerken en score berekenen voor de afbeeldingen is maximaal worden. Met het opgeslagen getrainde model in de hand we gebruikt:

* PySpark om de installatiekopieën en het getrainde model op de worker-knooppunten van het cluster te distribueren.
* Python vooraf verwerken van de installatiekopieën op elk knooppunt van het HDInsight Spark-cluster.
* Cognitieve Toolkit laden van het model en score voorverwerkte afbeeldingen op elk knooppunt.
* Jupyter-Notebooks de PySpark-script uit te voeren de resultaten te aggregeren en gebruik [Matplotlib](https://matplotlib.org/) voor het visualiseren van de prestaties van het model.

Het hele voorverwerking/scoren van 10.000 afbeeldingen, duurt minder dan één minuut op een cluster met 4 worker-knooppunten. Het model wordt nauwkeurig voorspelt de labels van installatiekopieën voor ~ 9,100 (91%). Een matrix verwarring ziet u de meest voorkomende classificatiefouten. De matrix wordt bijvoorbeeld getoond dat honden als katten en vice versa etikettering plaatsvindt meer vaak dan voor andere paren label.

![Resultaten](./media/apache-hadoop-deep-dive-advanced-analytics/ml-results.png)

### <a name="try-it-out"></a>Probeer het nu!

Ga als volgt [in deze zelfstudie](../spark/apache-spark-microsoft-cognitive-toolkit.md) voor het implementeren van deze oplossing voor end-to-end: een HDInsight Spark-cluster instellen, cognitieve Toolkit installeren en uitvoeren van de Jupyter-Notebook die scores 10.000 CIFAR afbeeldingen.

## <a name="next-steps"></a>Volgende stappen

Hive en Azure Machine Learning

* [Hive en Azure Machine Learning end-to-end](../../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Met behulp van een Azure HDInsight Hadoop-Cluster op een gegevensset 1 TB](../../machine-learning/team-data-science-process/hive-criteo-walkthrough.md)

Spark en MLLib

* [Machine learning-met Spark in HDInsight](../../machine-learning/team-data-science-process/spark-overview.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](../spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](../spark/apache-spark-machine-learning-mllib-ipython.md)

Grondige Learning, cognitieve Toolkit en anderen

* [Indeling van de installatiekopie, gebruik cognitieve Toolkit en TensorFlow op Azure HDInsight Spark perfect parallel](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
* [Wetenschappelijke gegevens virtuele Machine van Azure](../../machine-learning/data-science-virtual-machine/overview.md)
* [Inleiding tot H2O.ai in Azure HDInsight](https://azure.microsoft.com/blog/introducing-h2o-ai-with-on-azure-hdinsight-to-bring-the-most-robust-ai-platform-for-enterprises/)
