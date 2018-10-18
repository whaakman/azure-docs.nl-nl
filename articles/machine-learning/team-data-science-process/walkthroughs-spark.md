---
title: HDInsight Spark-scenario's met PySpark en Scala op Azure | Microsoft Docs
description: Voorbeelden van het Team Data Science Process die stapsgewijs door het gebruik van PySpark en Scala in een Azure HDInsight Spark te doen, voorspellende analyses.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: deguhath
ms.openlocfilehash: 2fd6d556e908c47baf3c58c46172c0684b66cdd9
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394249"
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>HDInsight Spark datatechnologische rondleidingen met PySpark en Scala op Azure

Deze scenario's gebruik PySpark en Scala op een Azure Spark-cluster voor voorspellende analyses. Ze stappen de die worden beschreven in het Team Data Science Process. Zie voor een overzicht van het Team Data Science Process [Data Science Process](overview.md). Zie voor een overzicht van Spark in HDInsight [Inleiding tot Spark in HDInsight](../../hdinsight/spark/apache-spark-overview.md).

Aanvullende gegevens datatechnologische rondleidingen die het Team Data Science Process worden uitgevoerd zijn gegroepeerd op de **platform** die ze gebruiken. Zie [scenario's voor het uitvoeren van het Team Data Science Process](walkthroughs.md) voor een specificatie van deze voorbeelden.

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>Met behulp van de PySpark op Azure Spark taxiritjes voorspellen

De [Spark op Azure HDInsight gebruiken](spark-overview.md) scenario worden gegevens uit de New York taxi's gebruikt om te voorspellen of een tip wordt betaald en het bereik van de bedragen te betalen verwacht. Het maakt gebruik van het Team Data Science Process in een scenario met een [Azure HDInsight Spark-cluster](https://azure.microsoft.com/services/hdinsight/) wilt opslaan, verkennen, en functie-engineering-gegevens van de openbaar beschikbare NYC taxi reis en ritbedrag gegevensset. In dit overzichtsonderwerp van de stelt u met een HDInsight Spark-cluster en de Jupyter PySpark notebooks gebruikt in de rest van de procedure. Deze laptops ziet u hoe u om uw gegevens te verkennen en vervolgens op over het maken en gebruiken van modellen. De geavanceerde gegevens verkennen en modelleren notebook laat zien hoe het opnemen van kruisvalidatie, hyper-parameter verstrekkende, en model van de evaluatie.

### <a name="data-exploration-and-modeling-with-spark"></a>Met Spark gegevens verkennen en modelleren 
De gegevensset te verkennen en maken, te beoordelen en evalueren van de machine learning-modellen door het uitvoeren van de [binaire classificatie- en regressiemodellen modellen voor gegevens maken in de werkset Spark MLlib](spark-data-exploration-modeling.md) onderwerp.

### <a name="model-consumption"></a>Model-verbruik
Zie voor meer informatie over de classificatie- en regressiemodellen modellen die zijn gemaakt in dit onderwerp te beoordelen, [Score en evalueren met Spark gebouwde machine learning-modellen](spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Kruisvalidatie en hyperparameter sweeping
Zie [geavanceerde met Spark gegevens verkennen en modelleren](spark-advanced-data-exploration-modeling.md) op hoe de modellen kunnen worden getraind met behulp van kruisvalidatie en hyper-parameter sweeping.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Met behulp van Scala op Azure Spark taxiritjes voorspellen

De [Scala gebruiken met Spark op Azure](scala-walkthrough.md) scenario worden gegevens uit de New York taxi's gebruikt om te voorspellen of een tip wordt betaald en het bereik van de bedragen te betalen verwacht. Deze ziet u hoe u Scala voor beheerde machine learning-taken met de Spark machine learning-bibliotheek (MLlib) en SparkML pakketten op een Azure HDInsight Spark-cluster. Dit helpt u bij de taken die deel uitmaken van de [Data Science Process](http://aka.ms/datascienceprocess): gegevensopname en verkennen, visualisatie, feature-engineering, modellen en model verbruik. De gebouwde modellen zijn logistieke en lineaire regressie, willekeurige forests en kleurovergang boosted structuren.


## <a name="next-steps"></a>Volgende stappen

Zie voor een bespreking van de belangrijkste onderdelen die deel uitmaken van het Team Data Science Process [Team Data Science Process overzicht](overview.md).

Zie voor een bespreking van de levenscyclus van het Team Data Science Process die u gebruiken kunt voor uw data science-projecten structureren, [Team Data Science Process lifecycle](lifecycle.md). De levenscyclus van geeft een overzicht van de stappen van begin tot eind, projecten gewoonlijk volgen wanneer ze worden uitgevoerd. 

