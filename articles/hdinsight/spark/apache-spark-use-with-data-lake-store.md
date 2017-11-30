---
title: Apache Spark gebruiken voor het analyseren van gegevens in Azure Data Lake Store | Microsoft Docs
description: Spark-taken voor het analyseren van gegevens die zijn opgeslagen in Azure Data Lake Store uitvoeren
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 5a4c7d944577685182240a24c522db4542c09fc2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-store"></a>HDInsight Spark-cluster gebruiken voor het analyseren van gegevens in Data Lake Store

In deze zelfstudie maakt u Jupyter-notebook beschikbaar met HDInsight Spark-clusters gebruiken om uit te voeren van een taak die gegevens uit een Data Lake Store-account leest.

## <a name="prerequisites"></a>Vereisten

* Azure Data Lake Store-account. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](../../data-lake-store/data-lake-store-get-started-portal.md).

* Azure HDInsight Spark-cluster met Data Lake Store als opslag. Volg de instructies voor [een HDInsight-cluster maken met Data Lake Store met Azure-portal](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    
## <a name="prepare-the-data"></a>De gegevens voorbereiden

> [!NOTE]
> U hoeft niet in deze stap uitvoeren als u het HDInsight-cluster met Data Lake Store als standaard opslag hebt gemaakt. Een proces voor het maken van het cluster wordt toegevoegd voorbeeldgegevens in de Data Lake Store-account dat u opgeeft tijdens het maken van het cluster. Ga naar de sectie [gebruik HDInsight Spark-cluster met Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).
>
>

Als u een HDInsight-cluster met Data Lake Store als extra opslagruimte en Azure Storage-Blob als standaard opslag gemaakt, moet u eerst via voorbeeldgegevens kopiëren naar het Data Lake Store-account. U kunt het voorbeeld dat gegevens van de Azure Storage-Blob die is gekoppeld aan het HDInsight-cluster gebruiken. U kunt de [ADLCopy hulpprogramma](http://aka.ms/downloadadlcopy) om dit te doen. Download en installeer het hulpprogramma via de koppeling.

1. Open een opdrachtprompt en navigeer naar de map waar AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht om een specifieke blob kopiëren van de Broncontainer naar een Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Kopieer de **HVAC.csv** voorbeeldgegevens op het bestand **/HdiSamples/HdiSamples/SensorSampleData/hvac/** aan het Azure Data Lake Store-account. Het codefragment moet eruitzien als:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > Zorg ervoor dat u die de namen van de bestands- en in het juiste geval zijn.
   >
   >
3. U wordt gevraagd de referenties voor de Azure-abonnement waaronder die u een Data Lake Store-account hebt invoeren. U ziet een uitvoer die vergelijkbaar is met het volgende fragment:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    Het gegevensbestand (**HVAC.csv**) in een map worden gekopieerd **/hvac** in het Data Lake Store-account.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a>Een HDInsight Spark-cluster gebruiken met Data Lake Store

1. Klik vanuit de [Azure Portal](https://portal.azure.com/), vanaf het startboard, op de tegel voor uw Spark-cluster (als u deze aan het startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.

2. Klik vanuit de blade Spark-cluster op **Snelkoppelingen**. Klik vervolgens vanuit het **Cluster-dashboard** op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   > [!NOTE]
   > Mogelijk bereikt u de Jupyter-notebook voor uw cluster ook door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Maak een nieuwe notebook. Klik op **Nieuw** en klik vervolgens op **PySpark**.

    ![Een nieuwe Jupyter-notebook maken](./media/apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Een nieuwe Jupyter-notebook maken")

4. Omdat u de notebook met behulp van de PySpark-kernel hebt gemaakt, hoeft u niet expliciet contexten te maken. De Spark- en Hive-contexten worden automatisch voor u gemaakt tijdens het uitvoeren van de eerste codecel. Als eerste stap importeert u de typen die voor dit scenario zijn vereist. Plak hiertoe het volgende codefragment in een cel en druk op **SHIFT + ENTER**.

        from pyspark.sql.types import *

    Telkens wanneer u een taak in Jupyter uitvoert, toont de venstertitel van uw webbrowser de status **(Bezet)** samen met de notebooktitel. Ook ziet u een gevulde cirkel naast de **PySpark**-tekst in de rechterbovenhoek. Nadat de taak is voltooid, verandert deze in een lege cirkel.

     ![Status van een Jupyter-notebooktaak](./media/apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status van een Jupyter-notebooktaak")

5. Voorbeeldgegevens laden in een tijdelijke tabel met de **HVAC.csv** bestand dat u hebt gekopieerd naar het Data Lake Store-account. U kunt toegang tot de gegevens in de Data Lake Store-account met behulp van de volgende URL-patroon.

    * Als u Data Lake Store als standaard opslag hebt, zijn HVAC.csv in het pad dat vergelijkbaar is met de volgende URL:

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        Of u kunt ook een kortere indeling, zoals de volgende gebruiken:

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * Als u Data Lake Store als extra opslagruimte hebt, zijn HVAC.csv op de locatie waar u hebt gekopieerd, zoals:

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     Plak het volgende codevoorbeeld in een lege cel, vervangen door **MYDATALAKESTORE** met uw Data Lake Store-accountnaam en druk op **SHIFT + ENTER**. Dit codevoorbeeld registreert de gegevens in een tijdelijke tabel genaamd **hvac**.

            # Load the data. The path below assumes Data Lake Store is default storage for the Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create the schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse the data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register the data fram as a table to run queries against
            hvacdf.registerTempTable("hvac")

6. Omdat u een PySpark-kernel gebruikt, kunt u nu rechtstreeks een SQL-query uitvoeren op de tijdelijke tabel **hvac**, die u zojuist hebt gemaakt met behulp van de `%%sql`-magic. Voor meer informatie over de `%%sql`-magic, evenals over andere magics die voor de PySpark-kernel beschikbaar zijn, raadpleegt u [Beschikbare kernels op Jupyter-notebooks met HDInsight Spark-clusters](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. Nadat de taak is voltooid, wordt standaard de volgende uitvoer in tabelvorm weergegeven.

      ![Tabeluitvoer van het queryresultaat](./media/apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabeluitvoer van het queryresultaat")

     U kunt de resultaten ook in andere visualisaties bekijken. Zo ziet een gebiedsgrafiek voor dezelfde uitvoer er als volgt uit.

     ![Gebiedsgrafiek van het queryresultaat](./media/apache-spark-use-with-data-lake-store/jupyter-area-output.png "Gebiedsgrafiek van het queryresultaat")

8. Wanneer u klaar bent met het uitvoeren van de toepassing, moet u de notebook afsluiten om de resources vrij te geven. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**. Hiermee wordt de notebook afgesloten.


## <a name="next-steps"></a>Volgende stappen

* [Een zelfstandige Scala toepassing uit te voeren op Apache Spark-cluster maken](apache-spark-create-standalone-application.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ Spark-toepassingen voor HDInsight Spark Linux-cluster maken](apache-spark-intellij-tool-plugin.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen voor HDInsight Spark Linux-cluster maken](apache-spark-eclipse-tool-plugin.md)
