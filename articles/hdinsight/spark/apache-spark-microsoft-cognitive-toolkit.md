---
title: Microsoft Cognitive Toolkit met Azure HDInsight Spark voor deep learning
description: Meer informatie over hoe een getrainde Microsoft Cognitive Toolkit deep learning-model kan worden toegepast op een gegevensset met behulp van de Spark-Python-API in een Azure HDInsight Spark-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: f3e590ec5dded03ce9c6ab2f7a21d51048020622
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53012586"
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Microsoft Cognitive Toolkit deep learning-model met Azure HDInsight Spark-cluster gebruiken

In dit artikel hebt uitvoeren u de volgende stappen.

1. Uitvoeren van een aangepast script voor het installeren van [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) op een Azure HDInsight Spark-cluster.

2. Upload een [Jupyter-Notebook](https://jupyter.org/) naar de [Apache Spark](https://spark.apache.org/) cluster om te zien hoe u een ervaren Microsoft Cognitive Toolkit deep learning-model toepassen op bestanden in een Azure Blob Storage-Account met behulp van de [ Spark-Python-API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/free).

* **Azure HDInsight Spark-cluster**. In dit artikel een Spark 2.0-cluster te maken. Zie voor instructies [Apache Spark-cluster maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Hoe deze oplossing stromen?

Deze oplossing is verdeeld over de in dit artikel en een Jupyter-notebook die u als onderdeel van deze zelfstudie uploadt. In dit artikel hebt uitvoeren u de volgende stappen:

* Een scriptactie uitvoeren op een HDInsight Spark-cluster om Microsoft Cognitive Toolkit en Python-pakketten te installeren.
* Upload de Jupyter-notebook waarop de oplossing naar de HDInsight Spark-cluster.

De volgende resterende stappen worden behandeld in de Jupyter-notebook.

- Voorbeeldafbeeldingen laden in een Spark Resiliant gedistribueerd gegevensset of RDD
   - Laden van modules en voorinstellingen voor definiëren
   - Downloaden van de gegevensset lokaal op het Spark-cluster
   - De gegevensset converteren naar een RDD
- De afbeeldingen met behulp van een getraind Cognitive Toolkit-model beoordelen
   - Het getrainde model van de Cognitive Toolkit downloaden naar het Spark-cluster
   - Definieer functies kunnen worden gebruikt door de worker-knooppunten
   - De afbeeldingen te beoordelen op worker-knooppunten
   - De nauwkeurigheid van model evalueren


## <a name="install-microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit installeren

U kunt Microsoft Cognitive Toolkit installeren op een Spark-cluster met script action. Scriptactie maakt gebruik van aangepaste scripts om onderdelen te installeren op het cluster die niet standaard beschikbaar zijn. U kunt het aangepaste script vanuit de Azure-Portal kunt gebruiken met behulp van HDInsight .NET SDK of met behulp van Azure PowerShell. U kunt het script ook gebruiken voor het installeren van de toolkit als onderdeel van het maken van clusters, of nadat het cluster is actief en werkend. 

In dit artikel gebruiken we de portal voor het installeren van de toolkit nadat het cluster is gemaakt. Zie voor andere manieren om uit te voeren van het aangepaste script [aanpassen HDInsight-clusters met Script Action](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

Zie voor instructies over het gebruik van de Azure-Portal om uit te voeren scriptactie [aanpassen HDInsight-clusters met Script Action](../hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Zorg ervoor dat u het volgende invoeren voor het installeren van Microsoft Cognitive Toolkit opgeven.

* Geef een waarde voor de naam van de scriptactie.

* Voor **Bash-script-URI**, voer `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Zorg ervoor dat u het script uitvoert alleen op de hoofd- en worker-knooppunten en schakel de selectievakjes.

* Klik op **Create**.

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>De Jupyter-notebook uploaden naar Azure HDInsight Spark-cluster

Voor het gebruik van de Microsoft Cognitive Toolkit met het Azure HDInsight Spark-cluster, moet u de Jupyter-notebook laden **CNTK_model_scoring_on_Spark_walkthrough.ipynb** naar de Azure HDInsight Spark-cluster. Dit notitieblok is beschikbaar op GitHub op [ https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration ](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. Kloon de GitHub-opslagplaats [ https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration ](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Zie voor instructies voor het klonen van [een opslagplaats klonen](https://help.github.com/articles/cloning-a-repository/).

2. Open in de Azure-Portal de blade van de Spark-cluster die u al hebt ingericht, klikt u op **Clusterdashboard**, en klik vervolgens op **Jupyter-notebook**.

    U kunt ook de Jupyter-notebook starten door te gaan naar de URL `https://<clustername>.azurehdinsight.net/jupyter/`. Vervang \<clustername > met de naam van uw HDInsight-cluster.

3. Klik in de Jupyter-notebook op **uploaden** in de rechterbovenhoek hoek en navigeer vervolgens naar de locatie waar u de GitHub-opslagplaats hebt gekloond.

    ![Jupyter-notebook uploaden naar Azure HDInsight Spark-cluster](./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "uploaden Jupyter-notebook op Azure HDInsight Spark-cluster")

4. Klik op **uploaden** opnieuw.

5. Nadat de laptop is geüpload, klikt u op de naam van de notebook en volg de instructies in het notitieblok zelf voor het laden van de gegevensset en het uitvoeren van de zelfstudie.

## <a name="see-also"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met BI: interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens gebruiken](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight op de resultaten van voedingsinspectie voorspellen gebruiken](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Telemetrie analyse van Application Insights met Apache Spark in HDInsight](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren op een Apache Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op te sporen Apache Spark-toepassingen op afstand](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
