---
title: Microsoft cognitieve Toolkit met Azure HDInsight Spark voor grondige learning | Microsoft Docs
description: Meer informatie over hoe een getraind Microsoft cognitieve Toolkit grondige learning-model kan worden toegepast op een gegevensset met de API van de Python Spark in Azure HDInsight Spark-cluster.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: 036efd040370a821befbbd57beec24372fd0d204
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Gebruik Microsoft cognitieve Toolkit grondige learning-model met Azure HDInsight Spark-cluster

In dit artikel leert uitvoeren u de volgende stappen.

1. Voer een aangepast script voor het installeren van Microsoft cognitieve Toolkit op een Azure HDInsight Spark-cluster.

2. Een Jupyter-notebook uploaden naar het Spark-cluster om te zien hoe een getraind Microsoft cognitieve Toolkit grondige learning-model toepassen op bestanden in een Azure Blob Storage-Account met behulp van de [Spark Python-API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/free).

* **Azure HDInsight Spark-cluster**. Voor dit artikel, een 2.0 Spark-cluster te maken. Zie voor instructies [maken Apache Spark-cluster in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Hoe deze oplossing stromen

Deze oplossing wordt verdeeld over dit artikel en een Jupyter-notebook die u als onderdeel van deze zelfstudie uploadt. In dit artikel leert uitvoeren u de volgende stappen:

* Een scriptactie uitgevoerd op een HDInsight Spark-cluster om Microsoft cognitieve Toolkit en Python-pakketten te installeren.
* Upload de Jupyter-notebook dat wordt uitgevoerd van de oplossing met HDInsight Spark-cluster.

De volgende overige stappen worden in de Jupyter-notebook besproken.

- Installatiekopieën van het voorbeeld in een Spark Resiliant gedistribueerd gegevensset of RDD geladen
   - Laden van modules en standaardinstellingen definiëren
   - Downloaden van de gegevensset lokaal op het Spark-cluster
   - De gegevensset converteren naar een RDD
- De afbeeldingen met een getraind cognitieve Toolkit model beoordelen
   - Het getrainde model voor cognitieve Toolkit downloaden naar het Spark-cluster
   - Definieer functies kunnen worden gebruikt door de worker-knooppunten
   - Score van de installatiekopieën op de worker-knooppunten
   - De nauwkeurigheid model evalueren


## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft cognitieve Toolkit installeren

U kunt Microsoft cognitieve Toolkit installeren op een Spark-cluster met behulp van de scriptactie. Scriptactie maakt gebruik van aangepaste scripts om onderdelen te installeren op het cluster die niet standaard beschikbaar. U kunt de aangepaste scripts vanuit de Azure-Portal met behulp van HDInsight .NET SDK of met behulp van Azure PowerShell. U kunt het script ook gebruiken voor het installeren van de toolkit als onderdeel van het maken van het cluster, of nadat het cluster actief is. 

In dit artikel gebruiken we de portal voor het installeren van de toolkit nadat het cluster is gemaakt. Zie voor andere manieren om uit te voeren van het aangepaste script [aanpassen HDInsight-clusters met behulp van de scriptactie](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

Zie voor instructies over het gebruik van de Azure Portal om uit te voeren scriptactie [aanpassen HDInsight-clusters met behulp van de scriptactie](../hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Zorg ervoor dat u het volgende invoeren voor het installeren van Microsoft cognitieve Toolkit opgeven.

* Geef een waarde voor de naam van de scriptactie.

* Voor **Bash script URI**, voer `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Zorg ervoor dat u het script uitvoert op de hoofd- en werkrollen knooppunten alleen en schakel de selectievakjes.

* Klik op **Create**.

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>De Jupyter-notebook uploaden naar Azure HDInsight Spark-cluster

Voor het gebruik van de Microsoft cognitieve Toolkit met het Azure HDInsight Spark-cluster, moet u de Jupyter-notebook laden **CNTK_model_scoring_on_Spark_walkthrough.ipynb** aan het Azure HDInsight Spark-cluster. Deze laptop is beschikbaar op GitHub op [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. Kloon de GitHub-opslagplaats [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Zie voor instructies voor het klonen [een opslagplaats klonen](https://help.github.com/articles/cloning-a-repository/).

2. Open de blade Spark-cluster dat u al ingericht, klikt u op in de Azure-Portal **Cluster-Dashboard**, en klik vervolgens op **Jupyter-notebook**.

    U kunt ook de Jupyter-notebook starten door te gaan naar de URL `https://<clustername>.azurehdinsight.net/jupyter/`. Vervang \<clustername > met de naam van uw HDInsight-cluster.

3. Klik op de Jupyter-notebook **uploaden** in de rechterbovenhoek hoek en navigeer vervolgens naar de locatie waar u de GitHub-opslagplaats gekloond.

    ![Jupyter-notebook uploaden naar Azure HDInsight Spark-cluster](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "uploaden Jupyter-notebook met Azure HDInsight Spark-cluster")

4. Klik op **uploaden** opnieuw.

5. Nadat de laptop is geüpload, klikt u op de naam van de notebook en volg de instructies in de notebook zelf voor het laden van de gegevensset en het uitvoeren van de zelfstudie.

## <a name="see-also"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Analyse van Application Insights-telemetriegegevens met behulp van Spark in HDInsight](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
