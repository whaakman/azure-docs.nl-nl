---
title: Een Apache Spark machine learning-pijplijn - Azure HDInsight maken
description: De Apache Spark machine learning-bibliotheek gebruiken om gegevenspijplijnen te maken.
services: hdinsight
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: 8a1148bbb04bbad2ff25be9bcbe68d086e91027c
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049250"
---
# <a name="create-a-spark-machine-learning-pipeline"></a>Een machine learning-pijplijn in Spark maken

Apache Spark van schaalbare machine learning-bibliotheek (MLlib) biedt mogelijkheden voor modellen in een gedistribueerde omgeving. Het Spark-pakket [ `spark.ml` ](http://spark.apache.org/docs/latest/ml-pipeline.html) is een set op hoog niveau API's die zijn gebouwd op gegevensframes. Deze API's te maken en af te stemmen praktische machine learning-pijplijnen.  *Machine learning in Spark* verwijst naar deze MLlib DataFrame-API, niet de oudere op basis van een RDD pijplijn API.

Een machine learning-pijplijn (ML) is een complete werkstroom meerdere machine learning-algoritmen samen combineren. Er zijn veel stappen die nodig zijn om te verwerken en leer van gegevens, waarvoor een reeks algoritmen is vereist. Pijplijnen definiëren de fasen en ordening van een machine learning-proces. Fasen van een pijplijn worden in MLlib vertegenwoordigd door een specifieke reeks PipelineStages, waar een transformator en een Estimator taken uitvoeren.

Een transformator is een algoritme waarmee een DataFrame naar de andere worden getransformeerd met behulp van de `transform()` methode. Een functie transformator kan bijvoorbeeld één kolom met een DataFrame lezen, deze toewijzen aan een andere kolom en een nieuwe DataFrame uitvoer met de toegewezen kolom toegevoegd aan het.

Een Estimator is een abstractie van de learning-algoritmen en is verantwoordelijk voor het aanpassen of op een gegevensset voor het produceren van een transformator training. Een Estimator implementeert een methode met de naam `fit()`, die een DataFrame accepteert en een DataFrame, dit een transformator is produceert.

Elke stateless exemplaar van een transformator of een Estimator heeft zijn eigen unieke id die wordt gebruikt als parameters op te geven. Beide een uniforme API gebruiken voor het opgeven van deze parameters.

## <a name="pipeline-example"></a>Voorbeeld van de pijplijn

Om te demonstreren een praktische gebruik van een ML-pijplijn, in dit voorbeeld wordt het voorbeeld `HVAC.csv` gegevensbestand dat vooraf geladen geleverd op de standaardopslag voor uw HDInsight-cluster, Azure Storage of Data Lake Store. Als u wilt weergeven van de inhoud van het bestand, gaat u naar de `/HdiSamples/HdiSamples/SensorSampleData/hvac` directory. `HVAC.csv` bevat een verzameling van tijden met zowel de doel- en de werkelijke temperaturen voor HVAC (*verwarming, ventilatie en airconditioning*) systemen in verschillende gebouwen. Het doel is om het model van de gegevens te trainen en produceren van een prognose temperatuur voor een bepaald gebouw.

De volgende code:

1. Definieert een `LabeledDocument`, welke winkels de `BuildingID`, `SystemInfo` (van een systeem-id en leeftijd), en een `label` (1.0 als het gebouw te hoog is is, anders 0,0).
2. Hiermee maakt u een aangepaste parser functie `parseDocument` die neemt een regel (rij) van gegevens en bepaalt of het gebouw 'hot' is door het vergelijken van de temperatuur doel met de werkelijke temperatuur.
3. Van toepassing is de parser tijdens het uitpakken van de brongegevens.
4. Hiermee maakt u trainingsgegevens.

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

Deze pijplijn voorbeeld bestaat uit drie fasen: `Tokenizer` en `HashingTF` (beide transformatoren) en `Logistic Regression` (een Estimator).  De uitgepakte en geparseerde gegevens in de `training` DataFrame loopt via de pijplijn als `pipeline.fit(training)` wordt genoemd.

1. De eerste fase `Tokenizer`, splitst de `SystemInfo` invoerkolom (bestaande uit het systeem-id en leeftijd waarden) in een `words` uitvoerkolom. Deze nieuwe `words` kolom wordt toegevoegd aan het DataFrame. 
2. De tweede fase `HashingTF`, converteert de nieuwe `words` kolom in de functie vectoren. Deze nieuwe `features` kolom wordt toegevoegd aan het DataFrame. Deze eerste twee fasen zijn transformatoren. 
3. De derde fase `LogisticRegression`, is een Estimator en dus de pijplijn roept de `LogisticRegression.fit()` methode voor het produceren van een `LogisticRegressionModel`. 

```python
tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)

# Build the pipeline with our tokenizer, hashingTF, and logistic regression stages
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(training)
```

Om te zien van de nieuwe `words` en `features` kolommen toegevoegd door de `Tokenizer` en `HashingTF` transformatoren en een voorbeeld van de `LogisticRegression` estimator, voer een `PipelineModel.transform()` methode op het oorspronkelijke gegevensframe. Bij de productiecode moet de volgende stap is om door te geven in een test DataFrame voor het valideren van de training.

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

De `model` object kan nu worden gebruikt om voorspellingen te doen. Zie voor het volledige voorbeeld van deze machine learning-toepassing en stapsgewijze instructies voor het uitvoeren van deze [bouwen Apache Spark machine learning-toepassingen op Azure HDInsight](apache-spark-ipython-notebook-machine-learning.md).

## <a name="see-also"></a>Zie ook

* [Gegevenswetenschap met Scala en Spark op Azure](../../machine-learning/team-data-science-process/scala-walkthrough.md)
