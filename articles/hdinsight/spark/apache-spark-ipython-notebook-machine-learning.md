---
title: Apache Spark machine learning-toepassingen in Azure HDInsight bouwen | Microsoft Docs
description: Stapsgewijze instructies voor het bouwen van Apache Spark machine learning-toepassing op HDInsight Spark-clusters met Jupyter-notebook
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: jgao
ms.openlocfilehash: 74dcd368d8696df26c5ad294c5657161fbe7f408
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Apache Spark machine learning-toepassingen in Azure HDInsight bouwen

Informatie over het bouwen van een Apache Spark-machine learning-toepassing met behulp van een Spark-cluster in HDInsight. Dit artikel laat zien hoe u de Jupyter-notebook beschikbaar met het cluster te bouwen en testen van deze toepassing. De toepassing de voorbeeldgegevens HVAC.csv die beschikbaar is in alle clusters standaard gebruikt.

[MLlib](https://spark.apache.org/docs/1.1.0/mllib-guide.html) is die bestaat uit algemene learning-algoritmen en hulpprogramma's, met inbegrip van classificatie, regressie, clustering, samenwerking filteren, dimensionaliteit vermindering, evenals de onderliggende van Spark van schaalbare machine learning-bibliotheek optimalisatie van primitieven.

**Vereisten:**

U hebt het volgende:

* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Inzicht in de gegevensset

De volgende gegevens ziet u de temperatuur van het doel en de werkelijke temperatuur van sommige gebouwen waarvoor HVAC-systemen die zijn geïnstalleerd. De **System** kolom vertegenwoordigt de systeem-ID en de **SystemAge** kolom vertegenwoordigt het aantal jaren dat het systeem HVAC is al aanwezig op het gebouw. U gebruikt de gegevens in deze zelfstudie te voorspellen of een gebouw wordt hotter of colder op basis van de doel-temperatuur, basis van een systeem-ID en het systeem leeftijd.

![Momentopname van de gegevens die worden gebruikt voor Spark machine learning voorbeeld](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "momentopname van de gegevens die worden gebruikt voor Spark machine learning-voorbeeld")

Het gegevensbestand **HVAC.csv**, bevindt zich op **\HdiSamples\HdiSamples\SensorSampleData\hvac** op alle HDInsight-clusters.

## <a name="app"></a>Schrijven van een Spark machine learning-toepassing met behulp van Spark MLlib
In deze toepassing, gebruikt u een Spark [ML pijplijn](https://spark.apache.org/docs/2.2.0/ml-pipeline.html) om uit te voeren van een document-classificatie. ML pijplijnen bieden een uniforme set op hoog niveau API's die zijn gebouwd op DataFrames waarmee gebruikers maken en praktische machine learning-pijplijnen afstemmen. In de pijplijn splitsen van het document in woorden, de woorden converteren naar een numeriek functie vector en ten slotte een Voorspellend model met behulp van de functie vectoren en labels maken. De volgende stappen voor het maken van de toepassing uitvoeren.

1. Maak een Jupyter-notebook met behulp van de PySpark-kernel. Zie voor instructies [maken van een Jupyter-notebook](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).
2. Importeer de typen die nodig zijn voor dit scenario. Plak het volgende codefragment in een lege cel en druk vervolgens op **SHIFT + ENTER**. 

    ```PySpark
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer
    from pyspark.sql import Row

    import os
    import sys
    from pyspark.sql.types import *

    from pyspark.mllib.classification import LogisticRegressionWithSGD
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array
    ```
3. De gegevens worden geladen (hvac.csv), het parseren en gebruiken voor het trainen van het model. 

    ```PySpark
    # Define a type called LabelDocument
    LabeledDocument = Row("BuildingID", "SystemInfo", "label")

    # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
    def parseDocument(line):
        values = [str(x) for x in line.split(',')]
        if (values[3] > values[2]):
            hot = 1.0
        else:
            hot = 0.0        

        textValue = str(values[4]) + " " + str(values[5])

        return LabeledDocument((values[6]), textValue, hot)

    # Load the raw HVAC.csv file, parse it using the function
    data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
    training = documents.toDF()
    ```

    In het codefragment definieert u een functie waarmee de werkelijke temperatuur met de doel-temperatuur wordt vergeleken. Als de werkelijke temperatuur groter is, het gebouw is hot, aangegeven door de waarde **1.0**. Anders het gebouw is koude, aangegeven door de waarde **0,0**. 

4. Configureer de Spark machine learning-pijplijn die uit drie fasen bestaat: tokenizer hashingTF en lr. 

    ```PySpark
    tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    ```

    Voor meer informatie over wat een pijplijn is en hoe het werkt Zie <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pijplijn</a>.

5. De pijplijn voor het document training passen.
   
    ```PySpark
    model = pipeline.fit(training)
    ```

6. Controleer of het document training controlepunt uw voortgang met de toepassing.
   
    ```PySpark
    training.show()
    ```
   
    Dit geeft de uitvoer ziet er als volgt:

    ```
    +----------+----------+-----+
    |BuildingID|SystemInfo|label|
    +----------+----------+-----+
    |         4|     13 20|  0.0|
    |        17|      3 20|  0.0|
    |        18|     17 20|  1.0|
    |        15|      2 23|  0.0|
    |         3|      16 9|  1.0|
    |         4|     13 28|  0.0|
    |         2|     12 24|  0.0|
    |        16|     20 26|  1.0|
    |         9|      16 9|  1.0|
    |        12|       6 5|  0.0|
    |        15|     10 17|  1.0|
    |         7|      2 11|  0.0|
    |        15|      14 2|  1.0|
    |         6|       3 2|  0.0|
    |        20|     19 22|  0.0|
    |         8|     19 11|  0.0|
    |         6|      15 7|  0.0|
    |        13|      12 5|  0.0|
    |         4|      8 22|  0.0|
    |         7|      17 5|  0.0|
    +----------+----------+-----+
    ```

    Vergelijking van de uitvoer tegen de onbewerkte CSV-bestand. De eerste rij van het CSV-bestand heeft bijvoorbeeld deze gegevens:

    ![Uitvoergegevens van de momentopname voor Spark machine learning voorbeeld](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "momentopname van de uitvoer-gegevens voor Spark machine learning-voorbeeld")

    U ziet hoe de werkelijke temperatuur kleiner is dan het doel temperatuur voorstellen dat het gebouw koude is. Daarom in de training uitvoer de waarde voor **label** in de eerste rij is **0,0**, wat betekent dat het gebouw is geen hot.

7. Bereid een gegevensset om uit te voeren van het getrainde model op. Om dit te doen, die u op een systeem-ID en het systeem leeftijd doorgeeft (aangeduid als **SystemInfo** in de uitvoer training), en het model wordt voorspeld of het gebouw met dat systeem-ID en het systeem leeftijd hotter wordt (aangeduid met 1.0) of koeler (aangeduid met 0,0).
   
    ```PySpark   
    # SystemInfo here is a combination of system ID followed by system age
    Document = Row("id", "SystemInfo")
    test = sc.parallelize([(1L, "20 25"),
                    (2L, "4 15"),
                    (3L, "16 9"),
                    (4L, "9 22"),
                    (5L, "17 10"),
                    (6L, "7 22")]) \
        .map(lambda x: Document(*x)).toDF() 
    ```
8. Ten slotte de voorspellingen op de testgegevens. 
   
    ```PySpark
    # Make predictions on test documents and print columns of interest
    prediction = model.transform(test)
    selected = prediction.select("SystemInfo", "prediction", "probability")
    for row in selected.collect():
        print row
    ```

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    ```   
    Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
    Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
    Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
    Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
    Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
    Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
    ```
   
   In de eerste rij in de voorspelling, kunt u zien dat voor een HVAC-systeem met ID 20 en system leeftijd van 25 jaar, het gebouw zal hot (**voorspelling = 1.0**). De eerste waarde voor DenseVector (0.49999) komt overeen met de voorspelling 0,0 en de tweede waarde (0.5001) komt overeen met de voorspelling 1.0. In de uitvoer, ondanks dat de tweede waarde alleen marginaal hoger is, wordt het model bevat **voorspelling = 1.0**.
10. De notebook afsluiten om de resources vrij te geven. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**. Hiermee wordt de notebook afgesloten.

## <a name="anaconda"></a>Gebruik scikit Anaconda-bibliotheek voor Spark machine learning meer
Apache Spark-clusters in HDInsight bevatten Anaconda-bibliotheken. Dit omvat ook de **scikit-meer** -bibliotheek voor machine learning. De bibliotheek bevat ook verschillende gegevenssets die u voor het bouwen van de voorbeeldtoepassingen rechtstreeks vanuit een Jupyter-notebook kunt gebruiken. Voor voorbeelden van het gebruik van de scikit-bibliotheek Lees [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

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

[hdinsight-weblogs-sample]:../hadoop/apache-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]:../hadoop/apache-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
