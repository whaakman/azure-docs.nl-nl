---
title: Overzicht van machine learning - Azure HDInsight
description: Beschrijving van de machine learning-opties in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: hrasheed
ms.openlocfilehash: 84ce13f78a9c597c780cb0dfa0fa5d16e6175b1c
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55077723"
---
# <a name="machine-learning-on-hdinsight"></a>Machine learning in HDInsight

HDInsight kunt machine learning met big data, biedt de mogelijkheid om op te halen van waardevolle inzichten van grote hoeveelheden (petabytes, of zelfs exabytes) en ongestructureerd, en gegevens ontdekt. Er zijn meerdere machine learning-opties in HDInsight:  SparkML en Apache Spark MLlib, R, Apache Hive en de Microsoft Cognitive Toolkit.

## <a name="sparkml-and-mllib"></a>SparkML and MLlib

[HDInsight Spark](spark/apache-spark-overview.md) is een aanbieding wordt gehost op Azure van [Apache Spark](https://spark.apache.org/), een uniforme, open source, een framework voor parallelle gegevensverwerking ondersteunen in-memory verwerking om te verbeteren van de analyse van big data. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak, en geavanceerde analyses. Gedistribueerde berekening in-memory-mogelijkheden van Spark kunnen u een goede keuze voor de zich herhalende algoritmen in machine learning- en grafiekberekeningen gebruikt. Er zijn twee schaalbare machine learning-bibliotheken die algoritmische modellering naar deze gedistribueerde omgeving meenemen: MLlib en SparkML. MLlib bevat de oorspronkelijke API die is gebaseerd op rdd's. SparkML is een nieuwere pakket dat een hoger niveau API die is gebaseerd op DataFrames biedt voor het maken van ML-pijplijnen. SparkML nog ondersteunt niet alle van de functies van MLlib, maar vervangt MLlib als standard van Spark-machine learning-bibliotheek.

De Microsoft Machine Learning-bibliotheek voor Apache Spark is [MMLSpark](https://github.com/Azure/mmlspark). Deze bibliotheek is ontworpen voor de datawetenschappers productiever te maken op Spark, verhogen de snelheid van experimenten en gebruikmaken van geavanceerde machine learning-technieken, met inbegrip van deep learning, op zeer grote gegevenssets. MMLSpark biedt een laag bovenop SparkML van laag niveau API's bij het bouwen van schaalbare ML-modellen, zoals indexering tekenreeksen bij gegevens in een indeling die werd verwacht door machine learning-algoritmen en aan elkaar te koppelen functie vectoren toewijzen. De MMLSpark-bibliotheek vereenvoudigt deze en andere algemene taken voor het ontwikkelen van modellen in PySpark.

## <a name="r"></a>R

[R](https://www.r-project.org/) is momenteel de meest populaire statistische programmeertaal in de hele wereld. Het is een hulpmiddel voor gegevensvisualisatie open-source met een community van meer dan 2,5 miljoen gebruikers en groeit. R is de snel groeiende gebruikersgroep en meer dan 8000 ingebrachte pakketten, waarschijnlijk kiezen voor veel bedrijven die machine learning nodig hebben. U kunt een HDInsight-cluster maken met ML-Services gereed om te worden gebruikt met grote gegevenssets en -modellen. Deze mogelijkheid biedt gegevenswetenschappers en statistici met een bekend R-interface die kunnen worden geschaald op aanvraag via HDInsight, zonder de overhead van het cluster instellen en beheren.

![Training voor voorspelling met R server](./media/hdinsight-machine-learning-overview/r-training.png)

Het edge-knooppunt van een cluster biedt een handige locatie verbinding maken met het cluster en uw R-scripts uit te voeren.  Hebt u ook de mogelijkheid om uit te voeren van R-scripts op de knooppunten van het cluster met behulp van de ScaleR Hadoop-Mapreduce of Spark compute-context.

Met ML-Services op HDInsight met Spark, kunt u training parallel op de knooppunten van een cluster met behulp van een Spark-compute-context. U kunt R-scripts uitvoeren rechtstreeks op het edge-knooppunt, met alle beschikbare cores parallel, indien nodig. U kunt ook de code uitvoeren van het edge-knooppunt te verwerken die zijn verdeeld over alle knooppunten in het cluster een vliegende start. ML-Services op HDInsight met Spark kunnen ook functies gebruik van open source R-pakketten, indien gewenst.

## <a name="azure-machine-learning-and-apache-hive"></a>Azure Machine Learning en Apache Hive

Azure Machine Learning biedt hulpprogramma's waarmee Voorspellend model, evenals een volledig beheerde service die u kunt gebruiken voor het implementeren van uw voorspellende modellen als webservices gereed om te gebruiken. Azure Machine Learning is een volledige predictive analytics-oplossing in de cloud die u gebruiken kunt om te maken, testen, operationeel maken en beheren van voorspellende modellen. Selecteer in een grote bibliotheek met algoritmen, een webgebaseerde studio gebruiken voor het ontwikkelen van modellen en eenvoudig uw modellen vervolgens als een webservice implementeren.

![Geavanceerde analyses voor Hadoop met Microsoft Azure Machine Learning toegankelijk maken](./media/hdinsight-machine-learning-overview/hadoop-azure-ml.png)

Functies maken voor gegevens in een HDInsight Hadoop-cluster met behulp van [Hive-query's](../machine-learning/team-data-science-process/create-features-hive.md). *Functie-engineering* probeert te verhogen van de voorspellende kracht van de learning-algoritmen met het maken van de functies van onbewerkte gegevens die het leerproces te vergemakkelijken. U kunt HiveQL query's uitvoeren vanuit Azure Machine Learning studio en toegang tot gegevens in Hive verwerkt en opgeslagen in blob-opslag met behulp van de [module gegevens importeren](../machine-learning/studio/import-data.md).

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

[Deep learning](https://www.microsoft.com/en-us/research/group/dltc/) wordt er een vertakking van machine learning die gebruikmaakt van neurale netwerken, inspiratie gegarandeerd door de biologische processen van het menselijk brein moeite heeft. Deep learning zien veel onderzoekers als een veelbelovende benadering voor het verbeteren van kunstmatige intelligentie. Voorbeelden van diepteleren zijn gesproken taal vertalers, image recognition systemen en machine redenering.

Om te gaan van een eigen werk in deep learning, Microsoft ontwikkelde de gratis, eenvoudig te gebruiken, open-source [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/). Deze toolkit wordt gebruikt door een groot aantal Microsoft-producten, bedrijven over de hele wereld met een hoeft te implementeren van deep learning op schaal, en door studenten die geïnteresseerd zijn in de meest recente algoritmen en technieken.

## <a name="see-also"></a>Zie ook

### <a name="scenarios"></a>Scenario's

* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Genereer filmaanbevelingen met Apache Mahout](hadoop/apache-hadoop-mahout-linux-mac.md)
* [Apache Hive en Azure Machine Learning](../machine-learning/team-data-science-process/create-features-hive.md)
* [Apache Hive en Azure Machine Learning end-to-end](../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Machine learning met Apache Spark in HDInsight](../machine-learning/team-data-science-process/spark-overview.md)

### <a name="deep-learning-resources"></a>Deep learning-resources

* [Geavanceerde Toolkit met Apache Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/25/using-microsofts-deep-learning-toolkit-with-spark-on-azure-hdinsight-clusters/)
* [Afbeeldingsclassificatie met Cognitive toolkit + Tensorflow op Apache Spark perfect parallel](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
