---
title: Het gebruik van MMLSpark Machine Learning | Microsoft Docs
description: Handleiding over het gebruik van MMLSpark-bibliotheek met Azure Machine Learning.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 6167e10219791466ca275ff02cc051e3227634e8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980033"
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Microsoft Machine Learning-bibliotheek voor Apache Spark gebruiken

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

## <a name="introduction"></a>Inleiding

[Microsoft Machine Learning-bibliotheek voor Apache Spark](https://github.com/Azure/mmlspark) (MMLSpark) bevat hulpprogramma's waarmee u eenvoudig kunnen maken van schaalbare machine learning-modellen voor grote gegevenssets. Dit omvat integratie van SparkML pijplijnen met de [Microsoft Cognitive Toolkit ](https://github.com/Microsoft/CNTK) en [OpenCV](http://www.opencv.org/), zodat u kunt: 
 * Ingangs- en voorverwerking image-gegevens
 * Parametriseer afbeeldingen en tekst met behulp van vooraf getrainde deep learning-modellen
 * Trainen en classificatie- en regressiemodellen modellen met behulp van de impliciete parametrisatie te beoordelen.

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze gebruiksaanwijzing kunt doorlopen, moet u naar:
- [Azure Machine Learning Workbench installeren](quickstart-installation.md)
- [Azure HDInsight Spark-cluster instellen](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Uw Experiment uitvoeren in Docker-Container

Uw Azure Machine Learning Workbench is geconfigureerd voor het gebruik van MMLSpark wanneer u experimenten in Docker-container, lokaal of in externe virtuele machine uitvoert. Op deze manier kunt u gemakkelijk fouten opsporen en experimenteren met uw modellen PySpark voordat ze op schaal op een cluster wordt uitgevoerd. 

Aan de slag met een voorbeeld, een nieuw project maken en 'MMLSpark op volwassenen telling' galerie selecteert. Selecteer 'Docker' als de compute-context van het projectdashboard, en klik op 'Uitvoeren'. Azure Machine Learning Workbench bouwt de Docker-container als de PySpark-programma wilt uitvoeren, en vervolgens het programma wordt uitgevoerd.

Nadat de uitvoering is voltooid, kunt u de resultaten weergeven in de weergave van de uitvoeringsgeschiedenis van Azure Machine Learning Workbench.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Installeer MMLSpark in Azure HDInsight Spark-Cluster.

Als u wilt dit en de volgende stap hebt voltooid, moet u eerst [maken van een Azure HDInsight Spark-cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Azure Machine Learning Workbench installeert standaard MMLSpark-pakket in uw cluster wanneer u uw experiment uitvoert. U kunt dit gedrag beheren en andere Spark-pakketten installeren met het bewerken van een bestand met de naam _aml_config/spark_dependencies.yml_ in de projectmap.

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

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>Azure HDInsight Spark-Cluster instellen als Compute-doel

CLI-venster openen vanuit Azure Machine Learning Workbench door te gaan naar het Menu 'File' en ' Open Command Prompt ' op

Voer de volgende opdrachten in CLI-venster:

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

Het cluster is nu beschikbaar als compute-doel voor het project.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Voer experiment in Azure HDInsight Spark-cluster.

Ga terug naar het projectdashboard van 'MMLSpark op volwassenen telling'-voorbeeld. Selecteer uw cluster als de compute-doel, en klik op uitvoeren.

Azure Machine Learning Workbench verzendt de spark-taak aan het cluster. U kunt de voortgang en bekijk de resultaten in de uitvoeringsgeschiedenis bekijken.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over MMLSpark-bibliotheek en voorbeelden, [MMLSpark GitHub-opslagplaats](https://github.com/Azure/mmlspark)

*Apache®, Apache Spark en Spark® zijn gedeponeerde handelsmerken of handelsmerken van de Apache Software Foundation in de Verenigde Staten en/of andere landen.*
