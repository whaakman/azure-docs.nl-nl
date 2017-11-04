---
title: HDInsight Spark-scenario's met PySpark en Scala in Azure | Microsoft Docs
description: Voorbeelden van het Team gegevens wetenschap proces om op een Azure HDInsight Spark predictive analytics doen met behulp van de PySpark en Scala.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: bradsev
ms.openlocfilehash: 76c9382cc5210bb5ac8551ebb7b16ecabe0dcd39
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>HDInsight Spark gegevens wetenschappelijke scenario's met PySpark en Scala op Azure

Deze scenario's gebruik PySpark en Scala in een Azure Spark-cluster voor predictive analytics. Ze stappen de die worden beschreven in de procedure voor wetenschappelijke gegevens Team. Zie voor een overzicht van het Team gegevens wetenschap proces [gegevens wetenschap proces](overview.md). Zie voor een overzicht van Spark in HDInsight [Inleiding tot Spark in HDInsight](../../hdinsight/spark/apache-spark-overview.md).

Aanvullende gegevens wetenschap-scenario's die het Team gegevens wetenschap proces worden uitgevoerd zijn gegroepeerd op de **platform** die ze gebruiken. Zie [scenario's voor het uitvoeren van het Team gegevens wetenschap proces](walkthroughs.md) voor een specificatie van deze voorbeelden.

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>Taxi tips PySpark met op Azure Spark voorspellen

De [Spark op Azure HDInsight gebruiken](spark-overview.md) Stapsgewijze instructies wordt gebruikgemaakt van gegevens van de New York taxi's te voorspellen of een tip wordt betaald en het bereik van de bedragen te betalen verwacht. Dit maakt gebruik van de procedure van wetenschappelijke gegevens Team in een scenario met een [Azure HDInsight Spark-cluster](https://azure.microsoft.com/services/hdinsight/) om op te slaan, verkennen, en functie engineer van gegevens uit de openbaar NYC taxi reis en ritbedrag gegevensset. In dit onderwerp wordt ingesteld u up met een HDInsight Spark-cluster en de Jupyter PySpark notebooks in de rest van deze stapsgewijze Kennismaking gebruikt. Deze laptops ziet u hoe u uw gegevens verkennen en hoe u maken en gebruiken van modellen. De geavanceerde gegevens te verkennen en modellering notebook laat zien hoe naar kruisvalidatie, hyper-parameter verstrekkende, opnemen en model van de evaluatie.

### <a name="data-exploration-and-modeling-with-spark"></a>Gegevensverkenning en modellering met Spark 
De gegevensset te verkennen en maken, beoordelen en evalueren van de machine learning-modellen doorloopt de [binaire classificatie en regressie modellen voor gegevens maken met de toolkit Spark MLlib](spark-data-exploration-modeling.md) onderwerp.

### <a name="model-consumption"></a>Model verbruik
Zie voor meer informatie over de classificatie en regressie modellen gemaakt in dit onderwerp te beoordelen, [Score en evalueren van Spark is gebouwd machine learning-modellen](spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Kruisvalidatie en hyperparameter sweeping
Zie [geavanceerde gegevensverkenning en modellering met Spark](spark-advanced-data-exploration-modeling.md) over de manier waarop modellen kunnen getraind met sweeping kruisvalidatie en hyper-parameter.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Taxi tips met behulp van Scala op Azure Spark voorspellen

De [gebruik Scala met Spark op Azure](scala-walkthrough.md) Stapsgewijze instructies wordt gebruikgemaakt van gegevens van de New York taxi's te voorspellen of een tip wordt betaald en het bereik van de bedragen te betalen verwacht. Er wordt weergegeven hoe Scala gebruiken voor bewaakte machine learning-taken met de Spark-machine learning-bibliotheek (MLlib) en SparkML pakketten in een Azure HDInsight Spark-cluster. Dit leidt u door de taken die deel uitmaken van de [gegevens wetenschap proces](http://aka.ms/datascienceprocess): gegevensopname en verkennen, visualisatie, functie-engineering, modellering en model verbruik. De modellen gebouwd zijn logistic en lineaire regressie, willekeurige forests en kleurovergang gestimuleerd structuren.


## <a name="next-steps"></a>Volgende stappen

Zie voor een bespreking van de belangrijke onderdelen waaruit het Team gegevens wetenschap proces [Team gegevens wetenschappelijke procesoverzicht](overview.md).

Zie voor een bespreking van de levensduur van het Team gegevens wetenschappelijke processen die u gebruiken kunt voor de structuur van uw gegevens wetenschappelijke projecten [Team gegevens wetenschap proces lifecycle](lifecycle.md). De levenscyclus van bevat de stappen, van begin tot eind, projecten meestal volgen wanneer ze worden uitgevoerd. 

