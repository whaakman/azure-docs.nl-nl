---
title: Interactieve query's uitvoeren op een Azure HDInsight Spark-cluster | Microsoft Docs
description: HDInsight Spark-snelstartgids over het maken van een Apache Spark-cluster in HDInsight.
keywords: spark-snelstartgids,interactieve spark,interactieve query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 78ab44a7afa6523e1e9e4082b3f45b1a28affe77
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="run-interactive-queries-on-spark-clusters-in-hdinsight"></a>Interactieve query's uitvoeren op Spark-clusters in HDInsight

Informatie over het gebruik van Jupyter-notebook voor interactieve Spark SQL-query's uitvoeren op een Spark-cluster. 

[Jupyter-notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html) is een browsertoepassing die uitgebreider is dan de interactieve ervaring via de console op het Web. Spark in HDInsight omvat ook [Zeppelin-Notebook](apache-spark-zeppelin-notebook.md). Jupyter-Notebook wordt in deze zelfstudie gebruikt.

Ondersteuning voor Jupyter-notebooks op HDInsight-clusters drie kernels - **PySpark**, **PySpark3**, en **Spark**. De **PySpark** kernel in deze zelfstudie wordt gebruikt. Voor meer informatie over de kernels en de voordelen van het gebruik van **PySpark**, Zie [gebruik Jupyter-notebook kernels met Apache Spark-clusters in HDInsight](apache-spark-jupyter-notebook-kernels.md). Zie voor het gebruik van de Notebook Zeppelin [gebruiken Zeppelin-notebooks met Apache Spark in Azure HDInsight-cluster](./apache-spark-zeppelin-notebook.md).

In deze zelfstudie maakt u een query gegevens in een csv-bestand. Eerst moet u die gegevens laden in Spark als een dataframe. Vervolgens kunt u query's uitvoeren op de dataframe met Jupyter-Notebook. 

## <a name="prerequisites"></a>Vereisten

* **Een Azure HDInsight Spark-cluster**. Zie voor instructies [een Apache Spark-cluster maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* **Een Jupyter-notebook met behulp van de PySpark**. Zie voor instructies [maken van een Jupyter-Notebook](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

## <a name="create-a-dataframe-from-a-csv-file"></a>Een dataframe uit een csv-bestand maken

Met een SQLContext maken toepassingen dataframes van een bestaande RDD, een Hive-tabel of gegevensbronnen. 

**Een dataframe uit een csv-bestand maken**

1. Maak een Jupyter-notebook met PySpark als u niet hebt. Zie voor instructies [maken van een Jupyter-Notebook](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

2. Plak de volgende code in een lege cel van de notebook en druk vervolgens op **SHIFT + ENTER** de code uit te voeren. Met de code importeert u de typen die voor dit scenario zijn vereist:

    ```PySpark
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```
    Met behulp van de PySpark-kernel een laptop, de Spark maken en Hive-worden contexten automatisch voor u gemaakt tijdens het uitvoeren van de eerste codecel. U hoeft niet expliciet contexten te maken.

    Wanneer een interactieve query uitgevoerd in Jupyter, ziet u het bijschrift web browser venster of tabblad een **(bezet)** status samen met de notebooktitel. Ook ziet u een gevulde cirkel naast de **PySpark**-tekst in de rechterbovenhoek. Wanneer de taak is voltooid, verandert deze in een lege cirkel.

    ![Status van interactieve Spark SQL-query](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status van interactieve Spark SQL-query")

3. Voer de volgende code om een dataframe en een tijdelijke tabel te maken (**hvac**) door het uitvoeren van de volgende code: de code niet alle kolommen beschikbaar in het CSV-bestand extraheren. 

    ```PySpark
    # Create an RDD from sample data
    hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
    # Create a schema for our data
    Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')
    
    # Parse the data and create a schema
    hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
    hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
    
    # Infer the schema and create a table       
    hvacTable = sqlContext.createDataFrame(hvac)
    hvacTable.registerTempTable('hvactemptable')
    dfw = DataFrameWriter(hvacTable)
    dfw.saveAsTable('hvac')
    ```
    De volgende schermafbeelding ziet een momentopname van het bestand HVAC.csv. Het csv-bestand wordt geleverd met alle HDInsigt Spark-clusters. De gegevens worden vastgelegd variaties van de temperatuur van een gebouw.

    ![Momentopname van gegevens voor interactieve Spark SQL-query](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "momentopname voor interactieve Spark SQL-query")

## <a name="run-queries-on-the-dataframe"></a>Query's uitvoeren op de dataframe

Zodra de tabel is gemaakt, kunt u een interactieve query uitvoeren op de gegevens.

**Een query uit te voeren**

1. Voer de volgende code in een lege cel van de notebook:

    ```PySpark
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Omdat de PySpark-kernel wordt gebruikt in de notebook, u kunt nu rechtstreeks een interactieve SQL query uitvoeren op de tijdelijke tabel **hvac** die u hebt gemaakt met behulp van de `%%sql` verwerkt Magic-pakket. Voor meer informatie over de `%%sql`-magic, en andere magics die voor de PySpark-kernel beschikbaar zijn, raadpleegt u [Beschikbare kernels op Jupyter-notebooks met HDInsight Spark-clusters](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Standaard wordt de volgende tabelvormige uitvoer weergegeven.

     ![Tabeluitvoer van resultaat van interactieve Spark-query](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabeluitvoer van resultaat van interactieve Spark-query")

3. U kunt de resultaten ook in andere visualisaties bekijken. U ziet een gebiedsgrafiek voor dezelfde uitvoer, selecteer **gebied** Stel u andere waarden, zoals wordt weergegeven.

    ![Gebiedsgrafiek van resultaat van interactieve Spark-query](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Gebiedsgrafiek van resultaat van interactieve Spark-query")

10. Van de **bestand** menu aan de laptop, klikt u op **opslaan en controlepunt**. 

11. Als u begint de [volgende zelfstudie](apache-spark-use-bi-tools.md) nu, laat u de notebook open. Als dit niet het geval is, sluit de notebook voor het vrijgeven van de clusterbronnen: van de **bestand** menu aan de laptop, klikt u op **sluiten en stoppen**.

## <a name="next-step"></a>Volgende stap

In dit artikel hebt u geleerd hoe u interactieve query's uitvoeren in Spark met Jupyter-notebook. Ga naar het volgende artikel om te zien hoe de gegevens die u in Spark geregistreerd in een BI-hulpprogramma voor analyse zoals Power BI en Tableau kan worden opgevraagd. 

> [!div class="nextstepaction"]
>[Gebruik van de hulpmiddelen voor gegevensvisualisatie met Azure HDInsight Spark-BI](apache-spark-use-bi-tools.md)




