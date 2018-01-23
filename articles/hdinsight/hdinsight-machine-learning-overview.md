---
title: Overzicht van machine learning - Azure HDInsight | Microsoft Docs
description: Beschrijving van de machine learning-opties in HDInsight.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: nitinme
ms.openlocfilehash: ff99a7a60573cad5e6dd30d4ca48903423e9f87f
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="machine-learning-on-hdinsight"></a>Machine learning in HDInsight

HDInsight kunt machine learning met big data, de mogelijkheid om toegang te verkrijgen van inzicht van grote hoeveelheden (petabytes of zelfs exabytes) gestructureerde, niet-gestructureerde, en gegevens snel verplaatsen. Er zijn meerdere machine learning-opties in HDInsight: SparkML en MLlib, R, Hive en de Microsoft cognitieve Toolkit.

## <a name="sparkml-and-mllib"></a>SparkML en MLlib

[HDInsight Spark](spark/apache-spark-overview.md) is een aanbieding voor Azure gehoste van [Spark](http://spark.apache.org/), een uniforme, open-source, framework voor parallelle verwerking van de gegevens in-memory verwerking om te stimuleren van big data-analyses ondersteunen. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak en geavanceerde analyses. De Spark in-memory gedistribueerde rekencapaciteiten kunnen u een goede keuze voor de herhalende algoritmen in machine learning- en grafiekberekeningen. Er zijn twee schaalbare machine learning-bibliotheken die algoritmische modellering mogelijkheden naar deze gedistribueerde omgeving meenemen: MLlib en SparkML. MLlib bevat de oorspronkelijke API RDDs ingebouwd. SparkML is een nieuwere pakket een hoger niveau API gebouwd op DataFrames biedt voor het maken van ML pijplijnen. SparkML nog niet ondersteunt alle functies van MLlib, maar vervangt MLlib als standaard van Spark-machine learning-bibliotheek.

De Microsoft Machine Learning-bibliotheek voor Apache Spark is [MMLSpark](https://github.com/Azure/mmlspark). Deze bibliotheek is ontworpen voor gegevenswetenschappers productiever te maken op Spark, verhoogt de snelheid van experimenteren en gebruikmaken van geavanceerde machine learning-technieken, waaronder grondige kennis, op zeer grote gegevenssets. MMLSpark biedt een laag bovenop op laag niveau van SparkML-API's tijdens het bouwen van schaalbare ML modellen, zoals indexering tekenreeksen bij gegevens in een indeling die werd verwacht door machine learning-algoritmen en functie vectoren montage toewijzen. De bibliotheek MMLSpark vereenvoudigt deze en andere algemene taken voor het bouwen van modellen in PySpark.

## <a name="r"></a>R

[R](https://www.r-project.org/) is momenteel de meest populaire statistische programmeertaal in de hele wereld. Er is een open-source gegevensvisualisatie hulpprogramma met een community van meer dan 2,5 miljoen gebruikers en groeit. Met de snel groeiende gebruikersgroep en meer dan 8000 ingebrachte pakketten is het R waarschijnlijk kiezen voor veel bedrijven die machine learning nodig hebben. U kunt een HDInsight-cluster maken met R Server gereed om te worden gebruikt met grote gegevenssets en -modellen. Deze mogelijkheid biedt gegevenswetenschappers en statistici met een bekend R-interface die kunnen worden geschaald op aanvraag via HDInsight, zonder de overhead van het cluster instellen en beheren.

![Training voor de prognose met R server](./media/hdinsight-machine-learning-overview/r-training.png)

Het edge-knooppunt van een cluster is een handige locatie verbinding maken met het cluster en voor het uitvoeren van uw R-scripts.  U hebt ook de optie voor het R-scripts uitvoeren op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verminderen of Spark compute-contexten.

Met op HDInsight met Spark R Server, kunt u training parallelize op de knooppunten van een cluster met behulp van een Spark compute-context. U kunt R scripts uitvoeren rechtstreeks op de edge-knooppunt met alle beschikbare kernen parallel, indien nodig. U kunt ook uw code uitvoeren van het edge-knooppunt moeten starten verwerking die is verdeeld over alle knooppunten in het cluster. R Server op HDInsight met Spark kunnen ook parallelizing functies van open-source R-pakketten, indien gewenst.

## <a name="azure-machine-learning-and-hive"></a>Azure Machine Learning en Hive

Azure Machine Learning biedt hulpprogramma's waarmee model predictive analytics, evenals een volledig beheerde service die u kunt gebruiken voor het implementeren van voorspellende modellen als webservices gereed om te gebruiken. Azure Machine Learning is een volledige predictive analytics-oplossing in de cloud die u kunt maken, testen, operationeel maken en beheren van voorspellende modellen. Selecteer in een grote bibliotheek met algoritmen, een webgebaseerde studio gebruiken voor het bouwen van modellen en uw model als een webservice eenvoudig te implementeren.

![Geavanceerde analyses voor Hadoop met Microsoft Azure Machine Learning toegankelijk maken](./media/hdinsight-machine-learning-overview/hadoop-azure-ml.png)

Functies maken voor gegevens in een HDInsight Hadoop-cluster met behulp van [Hive-query's](../machine-learning/team-data-science-process/create-features-hive.md). *Functie-engineering* probeert te verhogen van de voorspellende kracht van learning-algoritmen door het maken van de functies van ruwe gegevens die het leerproces vergemakkelijken. U kunt HiveQL query's uitvoeren vanaf Azure ML en toegang tot gegevens in de component verwerkt en opgeslagen in blob storage, met behulp van de [gegevens importeren-module](../machine-learning/studio/import-data.md).

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

[Grondige learning](https://www.microsoft.com/en-us/research/group/dltc/) een vertakking van machine learning die gebruikmaakt van neural networks, die is geïnspireerd op de biologische processen van de menselijke brain is. Grondige learning zien veel onderzoekers als een ordertoezegging benadering voor het verbeteren van de kunstmatige intelligence. Voorbeelden van grondige learning zijn gesproken vertalers, afbeelding erkenning systemen en machine redenering.

Om te helpen een eigen werk in grondige learning gaan, de gratis, eenvoudig te gebruiken, open-source Microsoft heeft ontwikkeld [Microsoft cognitieve Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/). Deze toolkit wordt gebruikt door een groot aantal verschillende Microsoft-producten, die een hebben voor het implementeren van grondige learning op grote schaal wereldwijd bedrijven en studenten geïnteresseerd in de meest recente algoritmen en technieken. 

## <a name="see-also"></a>Zie ook

### <a name="scenarios"></a>Scenario's

* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Filmaanbevelingen met Mahout genereren](hadoop/apache-hadoop-mahout-linux-mac.md)
* [Hive en Azure Machine Learning](../machine-learning/team-data-science-process/create-features-hive.md)
* [Hive en Azure Machine Learning end-to-end](../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Machine learning-met Spark in HDInsight](../machine-learning/team-data-science-process/spark-overview.md)

### <a name="deep-learning-resources"></a>Grondige trainingsmateriaal

* [Grondige learning toolkit met Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/25/using-microsofts-deep-learning-toolkit-with-spark-on-azure-hdinsight-clusters/)
* [Classificatie van de installatiekopie met cognitieve toolkit + Tensorflow op Spark perfect parallel](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
