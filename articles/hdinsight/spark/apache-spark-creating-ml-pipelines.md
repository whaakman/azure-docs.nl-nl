---
title: Maken van een Apache Spark machine learning-pipeline - Azure HDInsight | Microsoft Docs
description: Gebruik de Apache Spark machine learning-bibliotheek gegevenspijplijnen maken.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: maxluk
ms.openlocfilehash: 238ab5f940fbea836b75e20b015ae16f22eef3e9
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="create-a-spark-machine-learning-pipeline"></a>Een Spark machine learning-pijplijn maken

Apache Spark van schaalbare machine learning-bibliotheek (MLlib) brengt modellering mogelijkheden voor een gedistribueerde omgeving. Het pakket Spark [ `spark.ml` ](http://spark.apache.org/docs/latest/ml-pipeline.html) is een set op hoog niveau API's die zijn gebouwd op DataFrames. Deze API's te maken en praktische machine learning pijplijnen afstemmen.  *Machine learning Spark* verwijst naar deze MLlib DataFrame-API, niet de oudere RDD gebaseerde pijplijn API.

Een machine learning (ML) pijplijn is een complete werkstroom meerdere machine learning-algoritmen samen combineren. Er zijn veel stappen die nodig zijn om te verwerken en te leren van gegevens, waarvoor een reeks van algoritmen is vereist. Pijplijnen definiëren de fasen en ordening van een machine learning-proces. In MLlib, worden de fasen van een pijplijn vertegenwoordigd door een specifieke reeks PipelineStages, waar een transformator en een Estimator taken uitvoeren.

Een transformator is een algoritme waarmee een DataFrame naar de andere worden omgezet met behulp van de `transform()` methode. Een transformator functie kan bijvoorbeeld één kolom van een DataFrame lezen, wijs deze toe aan een andere kolom en een nieuwe DataFrame uitvoer met de toegewezen kolom hieraan toegevoegd.

Een Estimator is een abstractie van learning-algoritmen en is verantwoordelijk voor het aanpassen of op een gegevensset met de training voor het produceren van een transformator. Een Estimator implementeert een methode met de naam `fit()`, die een DataFrame accepteert en een DataFrame, een transformator is produceert.

Elke staatloze exemplaar van een transformator of een Estimator heeft zijn eigen unieke id die wordt gebruikt bij het opgeven van parameters. Beide een uniform API voor het opgeven van deze parameters gebruiken.

## <a name="pipeline-example"></a>Pipeline-voorbeeld

Als u wilt een praktische gebruik van een pijplijn ML demonstreren, in dit voorbeeld wordt de steekproef `HVAC.csv` gegevensbestand dat op de standaard-opslag voor uw HDInsight-cluster, Azure Storage of Data Lake Store vooraf geladen is. Als u wilt weergeven van de inhoud van het bestand, gaat u naar de `/HdiSamples/HdiSamples/SensorSampleData/hvac` directory. `HVAC.csv`bevat een aantal keren met doel- en werkelijke temperaturen voor HVAC (*verwarming, ventilatie en airconditioning*) systemen in verschillende gebouwen. Het doel is voor het trainen van het model van de gegevens en een prognose temperatuur voor een bepaalde gebouw produceren.

De volgende code:

1. Definieert een `LabeledDocument`, welke slaat de `BuildingID`, `SystemInfo` (van een systeem-id en leeftijd) en een `label` (1.0 als het gebouw te hoog is 0,0 anders).
2. Hiermee maakt u een aangepaste parser functie `parseDocument` die nodig is een regel (rij) van gegevens en wordt bepaald of het gebouw 'hot' door het vergelijken van de doel-temperatuur met de werkelijke temperatuur.
3. Van toepassing is de parser bij het uitpakken van de brongegevens.
4. Trainingsgegevens maakt.

```python
# The data structure (column meanings) of the data array:
# 0 Date
# 1 Time
# 2 TargetTemp
# 3 ActualTemp
# 4 System
# 5 SystemAge
# 6 BuildingID

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
data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
training = documents.toDF()
```

In dit voorbeeld pijplijn bestaat uit drie fasen: `Tokenizer` en `HashingTF` (zowel transformatoren) en `Logistic Regression` (een Estimator).  De uitgepakte en geparseerde gegevens in de `training` DataFrame loopt via de pijplijn wanneer `pipeline.fit(training)` wordt aangeroepen.

1. De eerste fase `Tokenizer`, splitst de `SystemInfo` invoerkolom (bestaande uit de systeem-id en leeftijd waarden) in een `words` uitvoerkolom. Dit nieuwe `words` kolom wordt toegevoegd aan de DataFrame. 
2. De tweede fase `HashingTF`, converteert u de nieuwe `words` kolom in de functie vectoren. Dit nieuwe `features` kolom wordt toegevoegd aan de DataFrame. Deze eerste twee fasen zijn transformatoren. 
3. De derde fase `LogisticRegression`, een Estimator is en dus de pijplijn roept de `LogisticRegression.fit()` methode voor het produceren van een `LogisticRegressionModel`. 

```python
tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)

# Build the pipeline with our tokenizer, hashingTF, and logistic regression stages
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(training)
```

Om te zien van de nieuwe `words` en `features` kolommen die zijn toegevoegd door de `Tokenizer` en `HashingTF` transformatoren en een voorbeeld van de `LogisticRegression` estimator, voer een `PipelineModel.transform()` methode op de oorspronkelijke DataFrame. De volgende stap zou in productiecode moet zijn om door te geven in een test DataFrame de training te valideren.

```python
peek = model.transform(training)
peek.show()

# Outputs the following:
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|BuildingID|SystemInfo|label|   words|            features|       rawPrediction|         probability|prediction|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|         4|     13 20|  0.0|[13, 20]|(262144,[250802,2...|[0.11943986671420...|[0.52982451901740...|       0.0|
|        17|      3 20|  0.0| [3, 20]|(262144,[89074,25...|[0.17511205617446...|[0.54366648775222...|       0.0|
|        18|     17 20|  1.0|[17, 20]|(262144,[64358,25...|[0.14620993833623...|[0.53648750722548...|       0.0|
|        15|      2 23|  0.0| [2, 23]|(262144,[31351,21...|[-0.0361327091023...|[0.49096780538523...|       1.0|
|         3|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|         4|     13 28|  0.0|[13, 28]|(262144,[69821,25...|[0.14630166986618...|[0.53651031790592...|       0.0|
|         2|     12 24|  0.0|[12, 24]|(262144,[187043,2...|[-0.0509556393066...|[0.48726384581522...|       1.0|
|        16|     20 26|  1.0|[20, 26]|(262144,[128319,2...|[0.33829638728900...|[0.58377663577684...|       0.0|
|         9|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|        12|       6 5|  0.0|  [6, 5]|(262144,[18659,89...|[0.07513008136562...|[0.51877369045183...|       0.0|
|        15|     10 17|  1.0|[10, 17]|(262144,[64358,25...|[-0.0291988646553...|[0.49270080242078...|       1.0|
|         7|      2 11|  0.0| [2, 11]|(262144,[212053,2...|[0.03678030020834...|[0.50919403860812...|       0.0|
|        15|      14 2|  1.0| [14, 2]|(262144,[109681,2...|[0.06216423725633...|[0.51553605651806...|       0.0|
|         6|       3 2|  0.0|  [3, 2]|(262144,[89074,21...|[0.00565582077537...|[0.50141395142468...|       0.0|
|        20|     19 22|  0.0|[19, 22]|(262144,[139093,2...|[-0.0769288695989...|[0.48077726176073...|       1.0|
|         8|     19 11|  0.0|[19, 11]|(262144,[139093,2...|[0.04988910033929...|[0.51246968885151...|       0.0|
|         6|      15 7|  0.0| [15, 7]|(262144,[77099,20...|[0.14854929135994...|[0.53706918109610...|       0.0|
|        13|      12 5|  0.0| [12, 5]|(262144,[89689,25...|[-0.0519932532562...|[0.48700461408785...|       1.0|
|         4|      8 22|  0.0| [8, 22]|(262144,[98962,21...|[-0.0120753606650...|[0.49698119651572...|       1.0|
|         7|      17 5|  0.0| [17, 5]|(262144,[64358,89...|[-0.0721054054871...|[0.48198145477106...|       1.0|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+

only showing top 20 rows
```

De `model` object kan nu worden gebruikt voor het maken van voorspellingen. Zie voor het volledige voorbeeld van deze machine learning-toepassing en stapsgewijze instructies voor het uitvoeren van deze [bouwen Apache Spark machine learning-toepassingen in Azure HDInsight](apache-spark-ipython-notebook-machine-learning.md).

## <a name="see-also"></a>Zie ook

* [Met behulp van Scala en Spark op Azure Gegevenswetenschap](../../machine-learning/team-data-science-process/scala-walkthrough.md)
