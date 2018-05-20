---
title: 'Zelfstudie: Een Spark-toepassing voor machine learning bouwen in HDInsight | Microsoft Docs'
description: Hier vindt u stapsgewijze instructies voor het bouwen van een Apache Spark-toepassing voor machine learning in HDInsight Spark-clusters met behulp van een Jupyter-notebook.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: jgao
ms.openlocfilehash: 70876196eb6b37065a663afa56ed496a0e9755db
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-build-a-spark-machine-learning-application-in-hdinsight"></a>Zelfstudie: Een Spark-toepassing voor machine learning bouwen in HDInsight 

In deze zelfstudie leert u hoe u het Jupyter-notebook gebruikt voor het bouwen van een Apache Spark-toepassing voor machine learning voor Azure HDInsight. 

[MLib](https://spark.apache.org/docs/1.1.0/mllib-guide.html) is de schaalbare bibliotheek voor machine learning van Spark die bestaat uit veelgebruikte leeralgoritmen en hulpprogramma's, zoals voor classificatie, regressie, clustering, gezamenlijke filterfuncties, dimensionaliteitsvermindering, evenals onderliggende optimalisatieprimitieven.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een Spark-toepassing voor machine learning ontwikkelen

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten:

U moet het volgende hebben gedaan:

* De snelstart [Een Apache Spark-cluster maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) uitvoeren.

## <a name="understand-the-data-set"></a>Informatie over de gegevensset

De toepassing gebruikt de voorbeeldgegevens uit HVAC.csv. Dit bestand is standaard beschikbaar in alle clusters. Het bestand bevindt zich op **\HdiSamples\HdiSamples\SensorSampleData\hvac**. De gegevens hebben betrekking op de gewenste temperatuur en de werkelijke temperatuur in enkele gebouwen waarin HVAC-systemen zijn geïnstalleerd. De kolom **System** bevat de id van het betreffende systeem en de kolom **SystemAge** geeft het aantal jaren aan dat het HVAC-systeem wordt gebruikt in het gebouw. Aan de hand van deze gegevens kunt u voorspellen of het in een gebouw warmer of kouder zal zijn gebaseerd op de richttemperatuur, een systeem-id en een leeftijd van het systeem.

![Momentopname van de gegevens die worden gebruikt voor het voorbeeld van machine learning met Spark](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Momentopname van de gegevens die worden gebruikt voor het voorbeeld van machine learning met Spark")

## <a name="develop-a-spark-machine-learning-application-using-spark-mllib"></a>Een Spark-toepassing voor machine learning ontwikkelen met Spark MLib

In deze toepassing gebruikt u een [ML-pijplijn](https://spark.apache.org/docs/2.2.0/ml-pipeline.html) van Spark om een documentclassificatie uit te voeren. ML-pijplijnen bieden een uniforme set van algemene API's die zijn gebaseerd op DataFrames waarmee gebruikers praktische pijplijnen voor machine learning kunnen maken en afstemmen. In de pijplijn splitst u het document op in woorden, converteert u de woorden naar een numerieke functievector en bouwt u ten slotte een voorspellend model met behulp van de functievectoren en labels. Voer de volgende stappen uit om de toepassing te maken.

1. Maak een Jupyter-notebook met behulp van de PySpark-kernel. Zie [Een Jupyter-notebook maken](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook) voor de instructies.
2. Importeer de typen die nodig zijn voor dit scenario. Plak het volgende codefragment in een lege cel en druk op **Shift+Enter**. 

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
3. Laad de gegevens (hvac.csv), parseer de gegevens en gebruik ze voor het trainen van het model. 

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

    In het codefragment definieert u een functie waarmee de werkelijke temperatuur wordt vergeleken met de gewenste temperatuur. Als de werkelijke temperatuur hoger is, is het warm in het gebouw, aangegeven door de waarde **1.0**. Anders is het koud in het gebouw, wat wordt aangegeven door de waarde **0.0**. 

4. Configureer de pijplijn voor de Spark-toepassing voor machine learning. Deze bestaat uit drie fasen: tokenizer, hashingTF en lr. 

    ```PySpark
    tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    ```

    Zie de Engelstalige handleiding over <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">de blibliotheek voor machine learning</a> voor meer informatie over pijplijnen en hoe het werkt.

5. Koppel de pijplijn aan het trainingsdocument.
   
    ```PySpark
    model = pipeline.fit(training)
    ```

6. Controleer het trainingsdocument om de voortgang met de toepassing te controleren.
   
    ```PySpark
    training.show()
    ```
   
    De uitvoer is vergelijkbaar met:

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

    Vergelijk de uitvoer met het onbewerkte CSV-bestand. De eerste rij van het CSV-bestand bevat bijvoorbeeld deze gegevens:

    ![Momentopname van uitvoergegevens voor het voorbeeld van een Spark-toepassing voor machine learning](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Momentopname van uitvoergegevens voor het voorbeeld van een Spark-toepassing voor machine learning")

    U ziet hoe de werkelijke temperatuur lager is dan de richttemperatuur, wat betekent dat het koud is in het gebouw. In de uitvoer van de training is de waarde voor **label** in de eerste rij daarom **0.0**, wat betekent dat het niet warm is in het gebouw.

7. Bereid een gegevensset voor die kan worden uitgevoerd op het getrainde model. Hiervoor geeft u een systeem-id en leeftijd van het systeem door (aangeduid als **SystemInfo** in de trainingsuitvoer), waarna het model voorspelt of het in het gebouw met die systeem-id en die leeftijd warmer (aangeduid met 1.0) of koeler (aangeduid met 0.0) zal zijn.
   
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
8. Genereer als laatste voorspellingen op basis van de testgegevens. 
   
    ```PySpark
    # Make predictions on test documents and print columns of interest
    prediction = model.transform(test)
    selected = prediction.select("SystemInfo", "prediction", "probability")
    for row in selected.collect():
        print row
    ```

    De uitvoer is vergelijkbaar met:

    ```   
    Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
    Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
    Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
    Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
    Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
    Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
    ```
   
   In de eerste rij in de voorspelling kunt u zien dat voor een HVAC-systeem met de id 20 en een leeftijd van 25 jaar, het warm is in het gebouw (**prediction=1.0**). De eerste waarde voor DenseVector (0.49999) komt overeen met de voorspelling 0.0 en de tweede waarde (0.5001) komt overeen met de voorspelling 1.0. In de uitvoer toont het model **prediction=1.0**, ook al is de tweede waarde maar een fractie hoger.
10. Sluit het notebook om de resources vrij te geven. Selecteer hiervoor **Sluiten en stoppen** in het menu **Bestand** van het notebook. Met deze actie wordt het notebook afgesloten en gesloten.

## <a name="use-anaconda-scikit-learn-library-for-spark-machine-learning"></a>scikit-learn-bibliotheek van Anaconda gebruiken voor een Spark-toepassing voor machine learning
Apache Spark-clusters in HDInsight bevatten Anaconda-bibliotheken, waaronder de bibliotheek **scikit-learn** voor machine learning. De bibliotheek bevat ook verschillende gegevenssets die u kunt gebruiken om rechtstreeks vanuit een Jupyter-notebook voorbeeldtoepassingen te bouwen. Zie [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html) voor voorbeelden van het gebruik van de bibliotheek scikit-learn.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

* Een Spark-toepassing voor machine learning ontwikkelen

Ga naar de volgende zelfstudie voor meer informatie over het gebruik van IntelliJ IDEA voor Spark-taken. 

> [!div class="nextstepaction"]
> [Een Scala Maven-toepassing maken met behulp van IntelliJ](./apache-spark-create-standalone-application.md)

