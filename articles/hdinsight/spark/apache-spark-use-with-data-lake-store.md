---
title: Apache Spark gebruiken voor het analyseren van gegevens in Azure Data Lake Storage
description: Spark-taken voor het analyseren van gegevens die zijn opgeslagen in Azure Data Lake Storage uitvoeren
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.openlocfilehash: 4f50cc46244a4fb205e0ccf60665ba892d3aa8fc
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310037"
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-storage"></a>HDInsight Spark-cluster gebruiken voor het analyseren van gegevens in Data Lake-opslag

In deze zelfstudie gebruikt u [Jupyter-Notebook](https://jupyter.org/) beschikbaar met HDInsight Spark-clusters een taak die gegevens uit een Data Lake Storage-account leest uit te voeren.

## <a name="prerequisites"></a>Vereisten

* Azure Data Lake Storage-account. Volg de instructies op [aan de slag met Azure Data Lake Storage met behulp van de Azure-portal](../../data-lake-store/data-lake-store-get-started-portal.md).

* Azure HDInsight Spark-cluster met Data Lake-opslag als opslag. Volg de instructies in [Quick Start: clusters instellen in HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

    
## <a name="prepare-the-data"></a>De gegevens voorbereiden

> [!NOTE]  
> U hoeft niet in deze stap uitvoeren als u het HDInsight-cluster hebt gemaakt met Data Lake Storage als standaardopslag. Het proces voor het cluster maken wordt enkele voorbeeldgegevens in de Data Lake Storage-account dat u opgeeft tijdens het maken van het cluster toegevoegd. Ga naar de sectie met HDInsight Spark-cluster met Data Lake-opslag.

Als u een HDInsight-cluster met Data Lake-opslag als extra opslag en Azure Storage-Blob als standaardopslag gemaakt, moet u eerst via enkele voorbeeldgegevens kopiëren naar het Data Lake Storage-account. U kunt het voorbeeld van gegevens uit de Azure Storage-Blob die is gekoppeld aan het HDInsight-cluster gebruiken. U kunt de [ADLCopy hulpprogramma](https://aka.ms/downloadadlcopy) om dit te doen. Download en installeer het hulpprogramma via de koppeling.

1. Open een opdrachtprompt en navigeer naar de map waarin AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht om te kopiëren van een specifieke blob uit de Broncontainer naar Data Lake-opslag:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Kopieer de **HVAC.csv** voorbeeldgegevens op het bestand **/HdiSamples/HdiSamples/SensorSampleData/hvac/** aan het Azure Data Lake Storage-account. Het codefragment moet er als volgt uitzien:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]  
   > Zorg ervoor dat u die de namen van de bestanden en paden in het juiste hoofdlettergebruik zijn.

3. U wordt gevraagd de referenties invoert voor de Azure-abonnement waarmee u uw Data Lake Storage-account hebt. Het volgende (of een vergelijkbaar) codefragment wordt weergegeven:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    Het gegevensbestand (**HVAC.csv**) moeten worden gekopieerd in een map **/hvac** in het Data Lake Storage-account.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-storage"></a>Een HDInsight Spark-cluster met Data Lake-opslag gebruiken

1. Uit de [Azure Portal](https://portal.azure.com/), vanaf het startboard, klikt u op de tegel voor uw Apache Spark-cluster (als u deze aan het startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.

2. Klik vanuit de blade Spark-cluster op **Snelkoppelingen**. Klik vervolgens vanuit het **Cluster-dashboard** op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   > [!NOTE]  
   > Mogelijk bereikt u de Jupyter-notebook voor uw cluster ook door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. Maak een nieuwe notebook. Klik op **Nieuw** en klik vervolgens op **PySpark**.

    ![Een nieuwe Jupyter-notebook maken](./media/apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Een nieuwe Jupyter-notebook maken")

4. Omdat u de notebook met behulp van de PySpark-kernel hebt gemaakt, hoeft u niet expliciet contexten te maken. De Spark- en Hive-contexten worden automatisch voor u gemaakt tijdens het uitvoeren van de eerste codecel. Als eerste stap importeert u de typen die voor dit scenario zijn vereist. Plak hiertoe het volgende codefragment in een cel en druk op **SHIFT + ENTER**.

        from pyspark.sql.types import *

    Telkens wanneer u een taak in Jupyter uitvoert, toont de venstertitel van uw webbrowser de status **(Bezet)** samen met de notebooktitel. Ook ziet u een gevulde cirkel naast de **PySpark**-tekst in de rechterbovenhoek. Nadat de taak is voltooid, verandert deze in een lege cirkel.

     ![Status van een Jupyter-notebooktaak](./media/apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status van een Jupyter-notebooktaak")

5. Voorbeeldgegevens laden in een tijdelijke tabel met de **HVAC.csv** bestand dat u hebt gekopieerd naar het Data Lake Storage-account. U kunt toegang tot de gegevens in het Data Lake Storage-account met behulp van de volgende URL-patroon.

    * Als u Data Lake Storage Gen1 als standaardopslag, zijn HVAC.csv op de locatie die vergelijkbaar is met de volgende URL:

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        Of u kunt ook een afgekorte indeling, zoals het volgende gebruiken:

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * Als u Data Lake Storage als extra opslag hebt, wordt HVAC.csv zich op de locatie waar u hebt gekopieerd, zoals:

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     Plak het volgende codevoorbeeld in een lege cel, Vervang **MYDATALAKESTORE** met uw Data Lake Storage-accountnaam en druk op **SHIFT + ENTER**. Dit codevoorbeeld registreert de gegevens in een tijdelijke tabel genaamd **hvac**.

            # Load the data. The path below assumes Data Lake Storage is default storage for the Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create the schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse the data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register the data fram as a table to run queries against
            hvacdf.registerTempTable("hvac")

6. Omdat u een PySpark-kernel gebruikt, kunt u nu rechtstreeks een SQL-query uitvoeren op de tijdelijke tabel **hvac**, die u zojuist hebt gemaakt met behulp van de `%%sql`-magic. Voor meer informatie over de `%%sql` magic, evenals andere magics die beschikbaar zijn met de PySpark-kernel, Zie [beschikbare Kernels op Jupyter-notebooks met Apache Spark HDInsight-clusters](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. Nadat de taak is voltooid, wordt standaard de volgende uitvoer in tabelvorm weergegeven.

      ![Tabeluitvoer van het queryresultaat](./media/apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabeluitvoer van het queryresultaat")

     U kunt de resultaten ook in andere visualisaties bekijken. Zo ziet een gebiedsgrafiek voor dezelfde uitvoer er als volgt uit.

     ![Gebiedsgrafiek van het queryresultaat](./media/apache-spark-use-with-data-lake-store/jupyter-area-output.png "Gebiedsgrafiek van het queryresultaat")

8. Wanneer u klaar bent met het uitvoeren van de toepassing, moet u de notebook afsluiten om de resources vrij te geven. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**. Hiermee wordt de notebook afgesloten.


## <a name="next-steps"></a>Volgende stappen

* [Een zelfstandig Scala-toepassing uit te voeren op Apache Spark-cluster maken](apache-spark-create-standalone-application.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ gebruiken om toepassingen van Apache Spark voor HDInsight Spark Linux-cluster te maken](apache-spark-intellij-tool-plugin.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken om toepassingen van Apache Spark voor HDInsight Spark Linux-cluster te maken](apache-spark-eclipse-tool-plugin.md)
* [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
