---
title: Het gebruik van MMLSpark Machine Learning | Microsoft Docs
description: Handleiding voor MMLSpark bibliotheek gebruiken met Azure Machine Learning.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: f978805f800a35908629a6febb59d7db50d14023
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Het gebruik van Microsoft-Machine Learning-bibliotheek voor Apache Spark

## <a name="introduction"></a>Inleiding

[Machine Learning-bibliotheek van Microsoft voor Apache Spark](https://github.com/Azure/mmlspark) (MMLSpark) bevat hulpprogramma's waarmee u eenvoudig schaalbare machine learning-modellen voor grote gegevenssets maken. Hierin zijn de integratie van SparkML pijplijnen met de [Microsoft cognitieve Toolkit ](https://github.com/Microsoft/CNTK) en [OpenCV](http://www.opencv.org/), zodat u kunt: 
 * Inkomende en vooraf verwerken image-gegevens
 * Featurize afbeeldingen en tekst met behulp van vooraf getraind deep learning-modellen
 * Trainen en classificatie en regressie modellen met impliciete featurization beoordelen.

## <a name="prerequisites"></a>Vereisten

Om de stap in deze handleiding instructies, moet u:
- [Azure Machine Learning-Workbench installeren](quickstart-installation.md)
- [Azure HDInsight Spark-cluster instellen](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Voer uw Experiment in Docker-Container

Uw Azure Machine Learning-Workbench is geconfigureerd voor gebruik van MMLSpark wanneer u experimenten in Docker-container, lokaal of in externe virtuele machine uitvoert. Deze mogelijkheid kunt u gemakkelijk fouten opsporen en Experimenteer met de PySpark-modellen, voordat u ze op schaal op een cluster uitvoert. 

Aan de slag met een voorbeeld, maak een nieuw project en selecteer 'MMLSpark op volwassenen telling' Galerievoorbeeld. 'Docker' als de compute-context van het projectdashboard selecteren en klik op 'Uitvoeren'. Azure Machine Learning Workbench maakt de Docker-container als de PySpark-programma wilt uitvoeren en vervolgens het programma wordt uitgevoerd.

Nadat de uitvoering is voltooid, kunt u de resultaten weergeven in de weergave uitvoeringsgeschiedenis van Azure Machine Learning-Workbench.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Installeer MMLSpark op Azure HDInsight Spark-Cluster.

Als u dit en de volgende stap, moet u eerst [maken van een Azure HDInsight Spark-cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Azure Machine Learning Workbench installeert standaard MMLSpark pakket op het cluster wanneer u uw experiment uitvoert. U kunt dit gedrag beheren en installeren van andere pakketten Spark door het bewerken van een bestand met de naam _aml_config/spark_dependencies.yml_ in de projectmap.

```
# Spark configuration properties.
configuration:
  "spark.app.name": "Azure ML Experiment"
  "spark.yarn.maxAppAttempts": 1

repositories:
  - "https://mmlspark.azureedge.net/maven"
packages:
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.9.9"
```

U kunt ook MMLSpark installeren rechtstreeks op uw HDInsight Spark-cluster met [scriptactie](https://github.com/Azure/mmlspark#hdinsight).

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>Azure HDInsight Spark-Cluster instellen als doel berekenen

CLI-venster openen vanuit Azure Machine Learning-Workbench door te gaan naar de Menu 'File' en ' opdrachtprompt openen ' op

Voer de volgende opdrachten in het venster van de CLI:

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

Het cluster is nu beschikbaar als doel voor het project te berekenen.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Voer experiment in Azure HDInsight Spark-cluster.

Ga terug naar het projectdashboard van 'MMLSpark op volwassenen telling'-voorbeeld. Selecteer uw cluster als doel voor de berekening en klik op uitvoeren.

Azure Machine Learning Workbench verzonden door de taak spark aan het cluster. U kunt de voortgang en bekijk de resultaten in uitvoeringsgeschiedenis weergeven.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over MMLSpark tapewisselaar en voorbeelden [MMLSpark GitHub-opslagplaats](https://github.com/Azure/mmlspark)

*Apache®, Apache Spark en Spark® zijn gedeponeerde handelsmerken of handelsmerken van de Apache Software Foundation in de Verenigde Staten en/of andere landen.*
