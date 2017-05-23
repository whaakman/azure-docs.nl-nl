---
title: Aan de slag - Apache Spark & interactieve Spark SQL-query&quot;s - Azure HDInsight | Microsoft Docs
description: "HDInsight Spark-snelstartgids over het maken van een Apache Spark-cluster in HDInsight en het uitvoeren van interactieve query’s met Jupyter-notebooks."
keywords: spark-snelstartgids,interactieve spark,interactieve query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/12/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 0c59792423bfe2848ab5773746db466890228ddc
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="get-started-create-an-apache-spark-cluster-in-hdinsight-and-run-interactive-spark-sql-queries"></a>Aan de slag: een Apache Spark-cluster in Azure HDInsight maken en interactieve Spark SQL-query's uitvoeren

Lees hoe u een [Apache Spark](hdinsight-apache-spark-overview.md)-cluster maakt in Azure HDInsight en interactieve Spark SQL-query's uitvoert met behulp van een [Jupyter](https://jupyter.org)-notebook.

   ![Snelstartdiagram waarin de stappen worden beschreven voor het maken van een Apache Spark-cluster in Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark-snelstartgids met behulp van Apache Spark in HDInsight. Geïllustreerde stappen: maken van een cluster; interactieve Spark-query uitvoeren")

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>HDInsight Spark-cluster maken

In deze sectie maakt u een HDInsight Spark-cluster met behulp van een [Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor andere methoden voor het maken van clusters.

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende waarden in:

    ![HDInsight Spark-cluster maken met behulp van een Azure Resource Manager-sjabloon](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Spark-cluster maken in HDInsight met behulp van een Azure Resource Manager-sjabloon")

    * **Abonnement**: selecteer uw Azure-abonnement voor dit cluster.
    * **Resourcegroep**: maak een nieuwe resourcegroep of selecteer een bestaande. Deze resourcegroep wordt gebruikt om Azure-resources voor uw projecten te beheren.
    * **Locatie**: selecteer een locatie voor de resourcegroep.  Deze locatie wordt ook gebruikt voor de standaardclusteropslag en het HDInsight-cluster.
    * **Clusternaam**: voer een naam in voor het Hadoop-cluster dat u maakt.
    * **Spark versie**: selecteer de Spark-versie die u wilt installeren op het cluster.
    * **Clusteraanmeldgegevens**: de standaardaanmeldnaam is admin.
    * **SSH-aanmeldgegevens**.

   Noteer deze waarden.  U hebt ze later in de zelfstudie nodig.

3. Selecteer **Ik ga akkoord met de bovenstaande voorwaarden**, selecteer **Vastmaken aan dashboard** en klik op **Kopen**. U ziet een nieuwe tegel met de titel Implementatie indienen voor Sjabloonimplementatie. Het duurt ongeveer 20 minuten om het cluster te maken.

> [!NOTE]
> In dit artikel wordt een Spark-cluster gemaakt dat [Azure Storage-blobs als clusteropslag](hdinsight-hadoop-use-blob-storage.md) gebruikt. U kunt ook een Spark-cluster maken dat Azure Storage-blobs als standaardopslag gebruikt en daarnaast [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) als extra opslag. Zie [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (Een HDInsight-cluster maken met Data Lake Store) voor instructies.
>
>

## <a name="run-an-interactive-spark-sql-query"></a>Een interactieve Spark SQL-query uitvoeren

In deze sectie gebruikt u een Jupyter-notebook om interactieve Spark SQL-query's uit te voeren op het Spark-cluster dat u eerder hebt gemaakt. HDInsight Spark-clusters bieden drie kernels die u met de Jupyter-notebook kunt gebruiken. Dit zijn:

* **PySpark** (voor toepassingen die zijn geschreven in Python)
* **PySpark3** (voor toepassingen die zijn geschreven in Python3)
* **Spark** (voor toepassingen die zijn geschreven in Scala)

In dit artikel gebruikt u de **PySpark**-kernel op de notebook van waar u de interactieve Spark SQL-query uitvoert. Zie [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md) (Jupyter-notebookkernels gebruiken met Apache Spark-clusters in HDInsight) voor meer informatie over de kernels. Enkele van de belangrijkste voordelen van het gebruik van de PySpark-kernel zijn:

* De contexten voor Spark en Hive worden automatisch ingesteld.
* U kunt magic-pakketten voor cellen gebruiken, zoals `%%sql` om uw interactieve SQL- of Hive-query's direct uit te voeren, zonder eventuele voorgaande codefragmenten.
* De uitvoer van de interactieve query's wordt automatisch weergegeven.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Een Jupyter-notebook maken met PySpark-kernel

1. Open de [Azure Portal](https://portal.azure.com/).

2. Als u ervoor hebt gekozen om het cluster vast te maken aan het dashboard, klikt u vanuit het dashboard op de clustertegel om de clusterblade te starten.

    Als u het cluster niet hebt vastgemaakt aan het dashboard, klikt u in het linkerdeelvenster op **HDInsight clusters** en vervolgens op het cluster dat u hebt gemaakt.

3. Klik in **Snelkoppelingen** op **Clusterdashboards** en klik vervolgens op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   ![Jupyter-notebook openen om de interactieve Spark SQL-query uit te voeren](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Jupyter-notebook openen om de interactieve Spark SQL-query uit te voeren")

   > [!NOTE]
   > Mogelijk kunt u de Jupyter-notebook voor uw cluster ook openen door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Maak een notebook. Klik op **Nieuw** en klik vervolgens op **PySpark**.

   ![Jupyter-notebook maken om de interactieve Spark SQL-query uit te voeren](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Jupyter-notebook maken om de interactieve Spark SQL-query uit te voeren")

   Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled (Untitled.pynb).

4. Klik bovenaan op de naam van de notebook en wijzig deze desgewenst in een beschrijvende naam.

    ![Een naam opgeven voor de Jupyter-notebook waarop de interactieve Spark-query wordt uitgevoerd](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Een naam opgeven voor de Jupyter-notebook waarop de interactieve Spark-query wordt uitgevoerd")

5. Plak de volgende code in een lege cel en druk op **Shift+Enter** om de code uit te voeren. Met de code importeert u de typen die voor dit scenario zijn vereist:

        from pyspark.sql.types import *

    Omdat u de notebook met behulp van de PySpark-kernel hebt gemaakt, hoeft u niet expliciet contexten te maken. De Spark- en Hive-contexten worden automatisch voor u gemaakt wanneer u de eerste codecel uitvoert.

    ![Status van interactieve Spark SQL-query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-interactive-spark-query-status.png "Status van interactieve Spark SQL-query")

    Telkens wanneer u een interactieve query in Jupyter uitvoert, toont de venstertitel van uw webbrowser de status **(Bezet)** samen met de notebooktitel. Ook ziet u een gevulde cirkel naast de **PySpark**-tekst in de rechterbovenhoek. Wanneer de taak is voltooid, verandert deze in een lege cirkel.

6. Registreer een set met voorbeeldgegevens als een tijdelijke tabel (**hvac**) door de volgende code uit te voeren.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

        # Register the data frame as a table to run queries against
        hvacdf.registerTempTable("hvac")

    Spark-clusters in HDInsight worden geleverd met een bestand met voorbeeldgegevens, **hvac.csv**. U vindt dit onder **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

7. Gebruik de volgende code voor het uitvoeren van een interactieve query op de gegevens.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Omdat u een PySpark-kernel gebruikt, kunt u nu rechtstreeks een interactieve SQL-query uitvoeren op de tijdelijke tabel **hvac**, die u hebt gemaakt met behulp van de `%%sql`-magic. Voor meer informatie over de `%%sql`-magic, en andere magics die voor de PySpark-kernel beschikbaar zijn, raadpleegt u [Beschikbare kernels op Jupyter-notebooks met HDInsight Spark-clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Standaard wordt de volgende tabelvormige uitvoer weergegeven.

     ![Tabeluitvoer van resultaat van interactieve Spark-query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-interactive-spark-query-result.png "Tabeluitvoer van resultaat van interactieve Spark-query")

    U kunt de resultaten ook in andere visualisaties bekijken. Zo ziet een gebiedsgrafiek voor dezelfde uitvoer er als volgt uit.

    ![Gebiedsgrafiek van resultaat van interactieve Spark-query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-interactive-spark-query-result-area-chart.png "Gebiedsgrafiek van resultaat van interactieve Spark-query")

9. Sluit de notebook af om de clusterresources vrij te geven nadat u de toepassing hebt uitgevoerd. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**.

## <a name="delete-the-cluster"></a>Het cluster verwijderen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot-access-control"></a>Problemen met toegangsbeheer oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u een probleem ondervindt met het maken van HDInsight-clusters.

## <a name="see-also"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](hdinsight-apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Analyse van Application Insights-telemetriegegevens met behulp van Spark in HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

