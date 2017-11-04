---
title: Spark is gebouwd machine learning-modellen operationeel maken | Microsoft Docs
description: Hoe worden geladen en beoordelen van learning-modellen die zijn opgeslagen in Azure Blob Storage (WASB) met behulp van Python.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 626305a2-0abf-4642-afb0-dad0f6bd24e9
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 9ff633b4543fbc537ffdb721756706e8de5e8e88
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="operationalize-spark-built-machine-learning-models"></a>Spark is gebouwd machine learning-modellen operationeel maken
[!INCLUDE [machine-learning-spark-modeling](../../../includes/machine-learning-spark-modeling.md)]

Dit onderwerp leest hoe u een opgeslagen machine learning-model (ML) met behulp van Python op HDInsight Spark-clusters operationeel. Dit wordt beschreven hoe u machine learning-modellen die zijn gebouwd met behulp van Spark MLlib laden en opgeslagen in Azure Blob Storage (WASB) en hoe ze met gegevenssets die ook zijn opgeslagen in WASB te beoordelen. Het toont hoe de invoergegevens vooraf verwerken, transformeren onderdelen met de functies voor indexering en codering in de werkset MLlib en het maken van een gegevensobject gelabelde punt die kan worden gebruikt als invoer voor score berekenen met de ML-modellen. De modellen die wordt gebruikt voor score berekenen bevatten lineaire regressie, Logistic Regression willekeurige Forest modellen en kleurovergang versterking structuur modellen.

## <a name="spark-clusters-and-jupyter-notebooks"></a>Spark-clusters en Jupyter-notebooks
Instellingsstappen en de code voor het ML-model operationeel vindt u in dit scenario voor het gebruik van een HDInsight Spark 1.6-cluster, evenals een 2.0 Spark-cluster. De code voor deze procedures is ook beschikbaar in Jupyter-notebooks.

### <a name="notebook-for-spark-16"></a>Laptop voor Spark 1.6
De [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter-notebook ziet u hoe u een opgeslagen model met behulp van Python op HDInsight-clusters operationeel. 

### <a name="notebook-for-spark-20"></a>Laptop voor Spark 2.0
Voor het wijzigen van de Jupyter-notebook voor Spark 1.6 voor gebruik met een HDInsight Spark 2.0-cluster, vervangt het Python-code-bestand met de [dit bestand](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py). Deze code laat zien hoe de modellen die zijn gemaakt in Spark 2.0 gebruiken.


## <a name="prerequisites"></a>Vereisten

1. U moet een Azure-account en een 1.6 Spark (of Spark 2.0) HDInsight-cluster voor dit scenario. Zie de [overzicht van Gegevenswetenschap met Spark op Azure HDInsight](spark-overview.md) voor instructies voor het voldoen aan deze eisen. Dit onderwerp bevat ook een beschrijving van de NYC 2013 Taxi gegevens hier gebruikt en instructies over het uitvoeren van code van een Jupyter-notebook in Spark-cluster. 
2. U moet ook maken met de machine learning-modellen moet hier worden berekend door het uitvoeren van de [gegevensverkenning en modellering met Spark](spark-data-exploration-modeling.md) onderwerp voor het 1.6 Spark-cluster of de laptops Spark 2.0. 
3. De laptops Spark 2.0 gebruiken een aanvullende gegevensset voor de classificatietaak, de bekende luchtvaartmaatschappij tijdige vertrek gegevensset van 2011 en 2012. Een beschrijving van de laptops en koppelingen naar deze vindt u in de [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) voor de GitHub-opslagplaats met deze. Bovendien moet de code hier en in de gekoppelde laptops is algemeen en moet werken op een Spark-cluster. Als u HDInsight Spark niet gebruikt, is het mogelijk dat de cluster-installatie en beheer stappen enigszins afwijken van wat hier moet worden weergegeven. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Setup: opslaglocaties, bibliotheken en de vooraf ingestelde Spark-context
Spark is kunnen lezen en schrijven naar een Azure Storage-Blob (WASB). Zodat uw bestaande gegevens opgeslagen kunnen er worden verwerkt met behulp van Spark en de resultaten opgeslagen opnieuw in WASB.

Voor het opslaan van modellen of bestanden in WASB, moet het pad correct worden opgegeven. De standaard-container die is gekoppeld aan het Spark-cluster kan worden verwezen met behulp van een pad die begint met: *'wasb / / /'*. Het volgende codevoorbeeld geeft de locatie van de gegevens moeten worden gelezen en het pad voor het model opslagmap waarin de uitvoer van het model is opgeslagen. 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Directory-paden voor opslaglocaties in WASB instellen
Modellen worden opgeslagen in: "wasb: / / / gebruiker/remoteuser/NYCTaxi/modellen '. Als dit pad is niet correct ingesteld, worden de modellen niet voor score berekenen geladen.

De scored resultaten zijn opgeslagen in: "wasb: / / / gebruiker/remoteuser/NYCTaxi/ScoredResults '. Als het pad naar map onjuist is, worden resultaten niet opgeslagen in de map.   

> [!NOTE]
> De locaties van de pad kunnen worden gekopieerd en geplakt in de tijdelijke aanduidingen in deze code uit de uitvoer van de laatste cel van de **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebook.   
> 
> 

Dit is de code in te stellen mappaden: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**UITVOER:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)

### <a name="import-libraries"></a>Bibliotheken importeren
Context voor spark instellen en benodigde bibliotheken met de volgende code importeren

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Vooraf context voor Spark en PySpark magics
De PySpark-kernels die worden geleverd met Jupyter-notebooks hebben een vooraf ingestelde context. Dus u hoeft niet in te stellen de Spark of Hive-contexten expliciet voordat u begint met het werken met de toepassing die u ontwikkelt. Dit zijn standaard beschikbaar voor u. Deze contexten zijn:

* SC - voor Spark 
* sqlContext - voor Hive

De PySpark-kernel biedt een aantal vooraf gedefinieerde 'magics', die zijn speciale opdrachten die u met aanroepen kunt %%. Er zijn twee dergelijke opdrachten die worden gebruikt in deze codevoorbeelden.

* **%% lokale** opgegeven dat de code in de volgende regels lokaal wordt uitgevoerd. De sitecode moet geldige Python-code.
* **%% sql -o<variable name>** 
* Een Hive-query op de sqlContext worden uitgevoerd. Als de parameter -o is doorgegeven, het resultaat van de query wordt bewaard de %% lokale Python context als een dataframe Pandas.

Voor meer informatie over de kernels voor Jupyter-notebooks en de vooraf gedefinieerde 'magics' die ze bieden, Zie [beschikbare Kernels voor Jupyter-notebooks met HDInsight Spark Linux-clusters in HDInsight](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Voor het opnemen van gegevens en een kader opgeschoonde gegevens maken
Deze sectie bevat de code voor een reeks vereiste taken voor het opnemen van de gegevens moet worden berekend. In de steekproef van een gekoppelde 0,1% van het taxi reis en tarief bestand (opgeslagen als een bestand .tsv), indeling van de gegevens niet lezen en maakt vervolgens een schone gegevensframe.

De taxi reis en tarief bestanden zijn lid op basis van de procedure in de: [het Team gegevens wetenschappelijke processen in actie: met behulp van HDInsight Hadoop-clusters](hive-walkthrough.md) onderwerp.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd uitvoering boven cel: 46.37 seconden

## <a name="prepare-data-for-scoring-in-spark"></a>Gegevens voorbereiden voor score berekenen in Spark
Deze sectie wordt beschreven hoe indexeren, coderen en schalen categorische functies in MLlib onder supervisie learning-algoritmen worden gebruikt voor de classificatie en regressie voor te bereiden.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Functie transformatie: te indexeren en coderen categorische functies voor invoer in modellen voor score berekenen
Deze sectie wordt beschreven hoe u een index categorische gegevens met behulp van een `StringIndexer` en het coderen van functies met `OneHotEncoder` invoer voor de modellen.

De [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) coderen van een kolom met tekenreeksen van labels aan een kolom van een label-indexen. De indexen zijn geordend op label frequenties. 

De [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) een kolom van een label indexen wordt toegewezen aan een kolom van de binaire aanvalsvectoren, met maximaal één one-waarde. Met deze codering kunt algoritmen die continue belangrijke functies, zoals logistic regression, moet worden toegepast op categorische functies verwacht.

    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()

    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd uitvoering boven cel: 5,37 seconden

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>RDD objecten met de functie matrices voor invoer in modellen maken
In deze sectie bevat de code die wordt beschreven hoe categorische gegevens als een object RDD index en deze een hot coderen zodat deze kan worden gebruikt om te trainen en te testen MLlib logistic regression en modellen op basis van een structuur. De geïndexeerde gegevens worden opgeslagen [robuuste gedistribueerd gegevensset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objecten. Dit zijn de fundamentele abstrahering in Spark. Een object RDD vertegenwoordigt een niet-wijzigbaar, gepartitioneerde verzameling van elementen die kunnen worden bediend op, in combinatie met Spark.

Code die wordt beschreven hoe gegevens met schalen bevat ook de `StandardScalar` geleverd door MLlib voor gebruik in lineaire regressie met stochastische kleurovergang Daalgradiënt (SGD), een populaire algoritme voor het trainen van een breed scala aan machine learning-modellen. De [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) wordt gebruikt voor het schalen van de functies verschillen eenheid. Functie schaal, ook wel bekend als gegevensnormalisatie, weet u zeker dat functies met veel betaald waarden zijn niet opgegeven overmatige afwegen in de beoogde functie. 

    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)

    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)

    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)

    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd uitvoering boven cel: 11.72 seconden

## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Met het Logistic Regression-Model beoordelen en uitvoer naar de blob opslaan
De code in deze sectie wordt beschreven hoe een Logistic regressiemodel dat is opgeslagen in Azure blob-opslag laden en gebruiken om te voorspellen of een tip op reis taxi wordt betaald, het score met standaard classificatie metrische gegevens, en opslaan op en de resultaten naar de blobopslag stora tekenen ge. De scored resultaten worden opgeslagen in RDD objecten. 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:**

Tijd uitvoering boven cel: 19.22 seconden

## <a name="score-a-linear-regression-model"></a>Een lineair regressiemodel beoordelen
We hebben gebruikt [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) voor het trainen van een lineair regressiemodel met stochastische kleurovergang Daalgradiënt (SGD) voor optimalisatie te voorspellen, de hoeveelheid tip betaald. 

De code in deze sectie wordt beschreven hoe een lineair regressiemodel uit Azure blob-opslag laden en slaat u de resultaten teruggestuurd naar de blob score geschaalde variabelen gebruiken.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel

    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER:**

Tijd uitvoering boven cel: 16.63 seconden

## <a name="score-classification-and-regression-random-forest-models"></a>Classificatie en regressie willekeurige Forest modellen beoordelen
De code in deze sectie ziet u hoe de opgeslagen classificatie laden en regressie willekeurige Forest modellen die zijn opgeslagen in Azure blob-opslag, de prestaties met standaard classificatie en regressie metingen beoordelen en slaat u de resultaten terug naar de blob-opslag.

[Willekeurige forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) ensembles van beslissingsstructuren zijn.  Ze combineren veel beslissingsstructuren om het risico te beperken. Willekeurige forests kunnen verwerken categorische functies uitbreiden naar de instelling multiklassen classificatie, vereisen geen functie schalen en kunnen niet met mogelijkheid tot vastleggen en interacties functie. Willekeurige forests zijn een van de meest succesvolle machine learning-modellen voor de indeling en regressie.

[Spark.mllib](http://spark.apache.org/mllib/) willekeurige forests ondersteunt voor binaire en multiklassen classificatie en voor regressie, continue en categorische functies gebruikt. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:**

Tijd uitvoering boven cel: 31.07 seconden

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Classificatie en regressie kleurovergang versterking structuur modellen beoordelen
De code in deze sectie wordt beschreven hoe classificatie en regressie kleurovergang versterking structuur modellen uit Azure blob-opslag laden en slaat u de resultaten weer naar blob storage score van de prestaties met standaard classificatie en regressie metingen. 

**Spark.mllib** GBTs voor binaire classificatie en voor regressie, met behulp van de functies voor continue en categorische ondersteunt. 

[Kleurovergang versterking structuren](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ensembles van beslissingsstructuren zijn. GBTs training beslissingsstructuren iteratief aan een functie gegevensverlies te minimaliseren. GBTs categorische functies kan verwerken, hoeven niet functie schalen en kunnen niet met mogelijkheid tot vastleggen en interacties functie. Ze kunnen ook worden gebruikt in een setting multiklasse classificatie.

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Tijd uitvoering boven cel: 14.6 seconden

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Opschonen van objecten uit het geheugen en scored bestandslocaties afdrukken
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**UITVOER:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt

## <a name="consume-spark-models-through-a-web-interface"></a>Spark modellen gebruiken via een webinterface
Spark biedt een mechanisme voor het op afstand verzenden batchtaken of interactieve query's via een REST-interface met een component Livy genoemd. Livy is standaard ingeschakeld op uw HDInsight Spark-cluster. Zie voor meer informatie over Livy: [Spark verzenden van taken op afstand met behulp van Livy](../../hdinsight/spark/apache-spark-livy-rest-interface.md). 

U kunt Livy op afstand verzenden van een taak die door de batch scores een bestand dat is opgeslagen in een Azure-blob en schrijft vervolgens de resultaten naar een andere blob. Dit doet uploaden u het Python-script uit  
[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) naar de blob van het Spark-cluster. U kunt een hulpprogramma zoals **Microsoft Azure Storage Explorer** of **AzCopy** het script kopiëren naar de blob van het cluster. In ons geval we het script hebt geüpload ***wasb:///example/python/ConsumeGBNYCReg.py***.   

> [!NOTE]
> De sneltoetsen die u nodig kunt u vinden op de portal voor het opslagaccount is gekoppeld aan het Spark-cluster. 
> 
> 

Na het uploaden naar deze locatie is dit script wordt uitgevoerd binnen het Spark-cluster in een gedistribueerde context. Het model geladen en voorspellingen op invoerbestanden op basis van het model wordt uitgevoerd.  

U kunt dit script extern aanroepen door een eenvoudige HTTPS/REST-aanvraag op Livy.  Hier volgt een curl-opdracht om de HTTP-aanvraag om aan te roepen het Python-script op afstand samen te stellen. CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME vervangen door de juiste waarden voor uw Spark-cluster.

    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

U kunt een andere taal op het externe systeem gebruiken om aan te roepen van de taak Spark via Livy door het maken van een eenvoudige HTTPS-aanroep met basisverificatie.   

> [!NOTE]
> Het normaal zou zijn gemakkelijk te gebruiken van de bibliotheek Python aanvragen wanneer deze HTTP-aanroep, maar dit momenteel niet standaard ingeschakeld in Azure Functions is geïnstalleerd. Zodat oudere HTTP-bibliotheken in plaats daarvan worden gebruikt.   
> 
> 

Dit is de Python-code voor de HTTP-aanroep:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


U kunt ook deze Python code toevoegen [Azure Functions](https://azure.microsoft.com/documentation/services/functions/) voor het activeren van de verzending van een Spark-taak die scores een blob op basis van verschillende gebeurtenissen, zoals een timer, maken of bijwerken van een blob. 

Als u liever een code gratis Clientervaring, gebruikt u de [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) aan te roepen de Spark-batchscoreberekening met het definiëren van een HTTP-bewerking op de **Logic Apps Designer** en het instellen van de parameters. 

* Maken vanuit Azure-portal een nieuwe logische App door te selecteren **+ nieuw** -> **Web en mobiel** -> **logische App**. 
* Online zetten van de **Logic Apps Designer**, voer de naam van de logische App en App Service-Plan.
* Selecteer een HTTP-actie en voer de parameters die worden weergegeven in de volgende afbeelding:

![Logic Apps Designer](./media/spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a>Volgende stappen
**Kruisvalidatie en hyperparameter sweeping**: Zie [geavanceerde gegevensverkenning en modellering met Spark](spark-advanced-data-exploration-modeling.md) over de manier waarop modellen kunnen getraind met sweeping kruisvalidatie en hyper-parameter.

