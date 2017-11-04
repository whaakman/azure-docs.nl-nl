---
title: Geavanceerde gegevensverkenning en modellering met Spark | Microsoft Docs
description: Gebruik HDInsight Spark doen gegevensverkenning en kruisvalidatie en hyperparameter optimalisatie van binaire classificatie en regressie modellen trainen.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f90d9a80-4eaf-437b-a914-23514390cd60
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 016d7760895e9b8cca082bac4e14388680fbbc05
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Met Spark verkennen en modelleren van geavanceerde gegevens
[!INCLUDE [machine-learning-spark-modeling](../../../includes/machine-learning-spark-modeling.md)]

In dit scenario maakt gebruik van HDInsight Spark gegevensverkenning doen en binaire classificatie en met behulp van kruisvalidatie regressie-modellen trainen en hyperparameter optimalisatie van een steekproef van de NYC taxi reis en ritbedrag 2013 gegevensset. Dit leidt u door de stappen van de [gegevens wetenschap proces](http://aka.ms/datascienceprocess)end-to- end, met behulp van een HDInsight Spark-cluster voor verwerking en Azure blobs voor het opslaan van de gegevens en de modellen. Het proces wordt verkend en gebracht van een Azure Storage-Blob gegevens visualiseren en vervolgens worden de gegevens voor het bouwen van voorspellende modellen voorbereid. Code van de oplossing en weergeven van de relevante waarnemingspunten is Python gebruikt. Deze modellen zijn build binaire classificatie en regressie modellering taken uitvoeren met de toolkit Spark MLlib. 

* De **binaire classificatie** taak is om te voorspellen of een tip voor de reis wordt betaald. 
* De **regressie** taak is het voorspellen van de hoeveelheid de tip op basis van andere tip-functies. 

De stappen modellering ook bevatten code waarin wordt getoond hoe te trainen, evalueren en opslaan van elk type model. Het onderwerp worden enkele van de grond dezelfde als de [gegevensverkenning en modellering met Spark](spark-data-exploration-modeling.md) onderwerp. Maar deze is 'geavanceerder' in de betreffende kruisvalidatie worden ook met hyperparameter verstrekkende gebruikt om optimaal nauwkeurige classificatie en regressie modellen trainen. 

**Kruisvalidatie (k/l)** is een techniek die hoe goed een op een bekende set gegevens getraind model generaliseert evalueert te voorspellen van de functies van gegevenssets waarop deze is niet getraind.  Een veelvoorkomende implementatie hier gebruikt is het verdelen van een gegevensset in K vouwen en vervolgens het model round-robin toewijst op alle maar een van de vouwen trainen. De mogelijkheid van het model prediction nauwkeurig bij de onafhankelijke gegevensset in deze niet gebruikt voor het model trainen Vouw getest wordt beoordeeld.

**Hyperparameter optimalisatie** is het probleem van het kiezen van een reeks hyperparameters voor een leeralgoritme meestal met het doel van een meting van de algoritme op de prestaties van een onafhankelijke set van gegevens te optimaliseren. **Hyperparameters** zijn waarden die buiten de training model procedure moeten worden opgegeven. Veronderstellingen over deze waarden kunnen invloed hebben op de flexibiliteit en de nauwkeurigheid van de modellen. Beslissingsstructuren hebben hyperparameters, bijvoorbeeld, zoals de gewenste omvang en aantal bladeren in de boomstructuur. Ondersteuning Vector Machines (SVMs) vereist dat een misclassification sancties term. 

Een veelgebruikte manier voor het uitvoeren van hyperparameter optimalisatie hier gebruikt een zoekopdracht raster is of een **parameter vegen**. Dit proces bestaat uit het uitvoeren van een intensieve zoekactie waarden voor een opgegeven subset van de ruimte hyperparameter voor een leeralgoritme. Cross-validatie kan voorzien van een metriek prestaties te sorteren van de optimale resultaten geproduceerd door de algoritme van de zoekopdracht raster. K/l gebruikt met hyperparameter ingrijpende helpt limiet problemen zoals het model trainingsgegevens overfitting zodat het model behoudt de capaciteit om te passen op de algemene set gegevens weer waaruit de trainingsgegevens is opgehaald.

De modellen we gebruiken omvatten logistic en lineaire regressie, willekeurige forests en kleurovergang gestimuleerd structuren:

* [Lineaire regressie met SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is een lineair regressiemodel die gebruikmaakt van een methode stochastische kleurovergang Daalgradiënt (SGD) en voor optimalisatie en het onderdeel schalen om te voorspellen van de bedragen tip betaald. 
* [Logistic regression met LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) of regressie 'logit', is een regressiemodel dat kan worden gebruikt wanneer de afhankelijke variabele categorische doen gegevensclassificatie is. LBFGS is een quasi toegepast optimalisatie-algoritme dat benadert de Broyden – Fletcher – Goldfarb – Shanno (BFGS)-algoritme met een beperkte hoeveelheid computergeheugen en die wordt veel gebruikt in machine learning.
* [Willekeurige forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) ensembles van beslissingsstructuren zijn.  Ze combineren veel beslissingsstructuren om het risico te beperken. Willekeurige forests worden gebruikt voor regressie en classificatie en categorische functies kunnen verwerken en kunnen worden uitgebreid naar de instelling multiklassen classificatie. Ze hoeven niet functie schalen en kunnen niet met mogelijkheid tot vastleggen en interacties functie zijn. Willekeurige forests zijn een van de meest succesvolle machine learning-modellen voor de indeling en regressie.
* [Verloop boosted structuren](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ensembles van beslissingsstructuren zijn. GBTs training beslissingsstructuren iteratief aan een functie gegevensverlies te minimaliseren. GBTs worden gebruikt voor regressie en classificatie en categorische functies kan verwerken, hoeven niet functie schalen en kunnen niet met mogelijkheid tot vastleggen en interacties functie. Ze kunnen ook worden gebruikt in een setting multiklasse classificatie.

Voorbeelden met k/l en Hyperparameter modelleren vegen worden weergegeven voor het probleem binaire classificatie. Eenvoudiger voorbeelden worden (zonder de parameter duplicaten) weergegeven in het onderwerp voor regressie taken. Maar in de bijlage validatie met elastische net voor lineaire regressie en k/l met het gebruik van parameter vegen voor willekeurige forest regressie ook worden weergegeven. De **elastische net** is een regressiemethode overgegaan voor de metrische gegevens L1 en L2 als sancties van lineaire regressie modellen die lineair aanpassen combineert de [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) en [gleuf](https://en.wikipedia.org/wiki/Tikhonov_regularization) methoden.   

> [!NOTE]
> Hoewel de toolkit Spark MLlib is ontworpen om te werken op grote gegevenssets, wordt hier een voorbeeld van een relatief klein (ongeveer 30 Mb met behulp van 170K rijen, ongeveer 0,1% van de oorspronkelijke gegevensset van de NYC) gebruikt voor het gemak. De hier opgegeven oefening efficiënt (in ongeveer 10 minuten) op een HDInsight-cluster met 2 worker-knooppunten wordt uitgevoerd. De dezelfde code, met kleine wijzigingen kan worden gebruikt voor het verwerken van grotere gegevenssets-wordt met de juiste wijzigingen voor het cachen van gegevens in het geheugen en het wijzigen van de clustergrootte.
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a>Instellen: De Spark-clusters en laptops
Instellingsstappen en code vindt u in dit scenario voor het gebruik van een HDInsight Spark 1.6. Maar Jupyter-notebooks zijn opgegeven voor zowel HDInsight Spark 1.6 en 2.0 Spark-clusters. Een beschrijving van de laptops en koppelingen naar deze vindt u in de [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) voor de GitHub-opslagplaats met deze. Bovendien moet de code hier en in de gekoppelde laptops is algemeen en moet werken op een Spark-cluster. Als u HDInsight Spark niet gebruikt, is het mogelijk dat de cluster-installatie en beheer stappen enigszins afwijken van wat hier moet worden weergegeven. Hier volgen de koppelingen naar de Jupyter-notebooks voor Spark 1.6 en 2.0 worden uitgevoerd in de pyspark-kernel van de server Jupyter-Notebook voor het gemak:

### <a name="spark-16-notebooks"></a>Spark 1.6 laptops

[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): bevat onderwerpen in de notebook #1 en de ontwikkeling van het model met hyperparameter afstemmen en kruisvalidatie.

### <a name="spark-20-notebooks"></a>Spark 2.0-laptops

[Spark2.0-pySpark3-machine-Learning-Data-Science-Spark-Advanced-Data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): dit bestand bevat informatie over het uitvoeren van gegevensverkenning modelleren en scores in 2.0 Spark-clusters.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Setup: opslaglocaties, bibliotheken en de vooraf ingestelde Spark-context
Spark kan lezen en schrijven naar Azure Storage-Blob (ook wel bekend als WASB). Zodat uw bestaande gegevens opgeslagen kunnen er worden verwerkt met behulp van Spark en de resultaten opgeslagen opnieuw in WASB.

Voor het opslaan van modellen of bestanden in WASB, moet het pad correct worden opgegeven. De standaard-container die is gekoppeld aan het Spark-cluster kan worden verwezen met behulp van een pad die begint met: ' wasb: / / / '. Andere locaties wordt verwezen door ' wasb: / / '.

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Directory-paden voor opslaglocaties in WASB instellen
Het volgende codevoorbeeld geeft de locatie van de gegevens moeten worden gelezen en het pad voor het model opslagmap waarin de uitvoer van het model is opgeslagen:

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**UITVOER**

DateTime.DateTime (2016, 4, 18, 17, 36, 27, 832799)

### <a name="import-libraries"></a>Bibliotheken importeren
Importeer de benodigde bibliotheken met de volgende code:

    # LOAD PYSPARK LIBRARIES
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
De PySpark-kernels die worden geleverd met Jupyter-notebooks hebben een vooraf ingestelde context. Dus u hoeft niet in te stellen de Spark of Hive-contexten expliciet voordat u begint met het werken met de toepassing die u ontwikkelt. Deze contexten zijn standaard voor u beschikbaar. Deze contexten zijn:

* SC - voor Spark 
* sqlContext - voor Hive

De PySpark-kernel biedt een aantal vooraf gedefinieerde 'magics', die zijn speciale opdrachten die u met aanroepen kunt %%. Er zijn twee dergelijke opdrachten die worden gebruikt in deze codevoorbeelden.

* **%% lokale** geeft aan dat de code in de volgende regels wordt lokaal uitgevoerd. De sitecode moet geldige Python-code.
* **%% sql -o <variable name>**  een Hive-query op de sqlContext wordt uitgevoerd. Als de parameter -o is doorgegeven, het resultaat van de query wordt bewaard de %% lokale Python context als een DataFrame Pandas.

Voor meer informatie over de kernels voor Jupyter-notebooks en de vooraf gedefinieerde 'magics' die ze bieden, Zie [beschikbare Kernels voor Jupyter-notebooks met HDInsight Spark Linux-clusters in HDInsight](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

## <a name="data-ingestion-from-public-blob"></a>Gegevensopname van een openbare blob:
De eerste stap in het proces van de wetenschappelijke gegevens is om op te nemen van de gegevens van bronnen waarin deze zich in uw gegevensverkenning en modellering omgeving bevindt worden geanalyseerd. Deze omgeving is Spark in dit scenario. Deze sectie bevat de code voor het voltooien van een reeks taken:

* de steekproef gemodelleerd opnemen
* Lees in de invoergegevensset (opgeslagen als een bestand .tsv)
* indeling en de gegevens opgeschoond
* maken en objecten (RDDs of gegevens frames) in het geheugen in de cache
* registreren als een temp-tabel in SQL-context.

Hier volgt de code voor gegevensopname.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
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

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd uitvoering boven cel: 276.62 seconden

## <a name="data-exploration--visualization"></a>Gegevensverkenning & visualisatie
Nadat de gegevens in Spark is ingesteld, is de volgende stap in het proces van de wetenschappelijke gegevens dieper inzicht van de gegevens via de exploratie en visualisatie te krijgen. In deze sectie we de taxi gegevens met behulp van SQL-query's controleren en de doelvariabelen en potentiële functies voor visuele inspectie tekenen. In het bijzonder voert u de frequentie van de passagiers aantallen in taxi reizen, de frequentie van tip bedragen en hoe tips is afhankelijk van de hoeveelheid betaling en het type.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Een histogram van passagiers aantal frequenties in de steekproef van taxi heen wordt getekend
Deze code en de volgende codefragmenten gebruik van SQL-magic query uitvoeren op de lokale magic de gegevens worden uitgezet en voorbeelden.

* **SQL-magic (`%%sql`)** eenvoudig inline HiveQL query's op de sqlContext biedt ondersteuning voor het HDInsight-PySpark-kernel. De (-o naam_variabele) argument als een DataFrame Pandas op de Jupyter-server de uitvoer van de SQL-query zich blijft voordoen. Dit betekent dat deze beschikbaar zijn in de lokale modus.
* De  **`%%local` magic** code lokaal uitvoeren op de Jupyter-server, waarop de headnode van het HDInsight-cluster wordt gebruikt. Normaal gesproken gebruikt u `%%local` magische na de `%%sql -o` verwerkt Magic-pakket wordt gebruikt voor het uitvoeren van een query. De parameter -o zou de uitvoer van de SQL-query lokaal bewaren. Vervolgens wordt de `%%local` magic activeert de volgende set van codefragmenten lokaal uitvoeren op basis van de uitvoer van de SQL-query's die lokaal is opgeslagen. De uitvoer wordt automatisch weergegeven nadat u de code hebt uitgevoerd.

Deze query haalt de reizen op het aantal van de passagiers. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Deze code maakt een lokale gegevens tijdskader van de query-uitvoer en de gegevens zijn getekend. De `%%local` magic maakt een lokale gegevens-frame, `sqlResults`, die kan worden gebruikt voor het uitzetten van matplotlib. 

> [!NOTE]
> Deze PySpark-magic wordt meerdere malen gebruikt in dit scenario. Als de hoeveelheid gegevens te groot is, moet u een steekproef nemen voor het maken van een gegevens-frame dat past in het lokale geheugen.
> 
> 

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Hier volgt de code voor het uitzetten van de reizen door passagiers aantallen

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**UITVOER**

![Frequentie van reizen op het aantal passagiers](./media/spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

U kunt kiezen uit verschillende soorten visualisaties (tabel-, cirkel, regel, gebied of balk) met behulp van de **Type** menuknoppen in de notebook. De grafiek balk wordt hier weergegeven.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Zet een histogram van de tip bedragen en hoe tip bedrag hangt af van de passagiers telling en het tarief bedragen.
Gebruik een SQL-query om de voorbeeldgegevens...

    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25


Deze codecel gebruikt de SQL-query om de gegevens van drie waarnemingspunten maken.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline

    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()


**UITVOER:** 

![Tip bedragdistributie](./media/spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Hoeveelheid op het aantal passagiers Tip](./media/spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip bedrag door tarief bedrag](./media/spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Functie-engineering, transformatie en gegevens voorbereiding voor modellering
Deze sectie wordt beschreven en vindt u de code voor procedures voor het voorbereiden van gegevens voor gebruik in ML-model. Er wordt weergegeven hoe de volgende taken uitvoeren:

* Maken van een nieuwe functie door uren in verkeer tijd opslaglocaties partitioneren
* Index en categorische functies op hot coderen
* Gelabelde point-objecten voor invoer in ML functies maken
* Een onderliggende steekproeven van de gegevens maken en deze te splitsen in trainings- en testdoeleinden sets
* Functie schalen
* Cacheobjecten in het geheugen

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a>Een nieuwe functie maken door het verkeer keren in opslaglocaties partitioneren
Deze code laat zien hoe u maakt een nieuwe functie verkeer keren in opslaglocaties partitioneren en hoe u in de cache van het resulterende gegevensframe in het geheugen. Opslaan in cache leidt tot verbeterde uitvoeringstijd waar flexibele gegevenssets gedistribueerd (RDDs) en gegevens frames herhaaldelijk worden gebruikt. Dus cache we RDDs en gegevensframes in verschillende stadia in dit scenario.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**UITVOER**

126050

### <a name="index-and-one-hot-encode-categorical-features"></a>Index en één hot categorische functies coderen
Deze sectie wordt beschreven hoe index of categorische functies voor invoer in de functies modellering coderen. Het model en het voorspellen van de functies van MLlib vereist dat functies met categorische invoergegevens worden geïndexeerd of worden gecodeerd vóór gebruik. 

Afhankelijk van het model moet u indexeren of ze op verschillende manieren te coderen. Bijvoorbeeld: Logistic en lineaire regressie modellen vereisen een hot codering, waarbij, bijvoorbeeld, een onderdeel met drie categorieën kan worden uitgebreid naar drie kolommen van de functie, waarbij elke met 0 of 1, afhankelijk van de categorie van een observatie. MLlib biedt [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) doen van één hot codering. Een kolom van een label indexen deze encoder toegewezen aan een kolom van de binaire aanvalsvectoren, met maximaal één one-waarde. Met deze codering kunt algoritmen die numerieke waarden functies, zoals logistic regression, moet worden toegepast op categorische functies verwacht.

Dit is de code om te indexeren en coderen categorische functies:

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd uitvoering boven cel: 3.14 seconden

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Gelabelde point-objecten voor invoer in ML functies maken
Deze sectie bevat de code die wordt beschreven hoe categorische gegevens als een gegevenstype gelabelde punt index en het coderen van deze. Dit wordt voorbereid om te worden gebruikt om te trainen en te testen MLlib logistic regression en andere classificatie-modellen. Gelabelde point-objecten zijn robuuste gedistribueerd gegevenssets (RDD) geformatteerd op een manier die is vereist als de invoergegevens voor de meeste ML algoritmen in MLlib. Een [punt gelabeld](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is een lokale vector, dense of sparse, die is gekoppeld aan een label/antwoord.

Dit is de code om te indexeren en het coderen van tekstfuncties voor binaire classificatie.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Dit is de code voor het coderen en indexeren categorische tekstfuncties voor lineaire regressie-analyse.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Een onderliggende steekproeven van de gegevens maken en deze te splitsen in trainings- en testdoeleinden sets
Deze code maakt een willekeurige steekproef van de gegevens (25% wordt hier gebruikt). Hoewel dit niet vereist voor dit voorbeeld vanwege de grootte van de gegevensset, ziet u hoe u hier de gegevens kunt controleren. Weet u het gebruik van uw eigen probleem indien nodig. Als voorbeelden groot zijn, kan dit aanzienlijke tijd tijdens de training modellen opslaan. Naast we het voorbeeld in een training gedeelte (hier 75%) en een test deel (25% hier) moet worden gebruikt in de classificatie en regressie modellering opgesplitst.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand

    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Tijd uitvoering boven cel: 0.31 seconden

### <a name="feature-scaling"></a>Functie schalen
Functie schaal, ook wel bekend als gegevensnormalisatie, weet u zeker dat functies met veel betaald waarden zijn niet opgegeven overmatige afwegen in de beoogde functie. De code voor de functie schalen maakt gebruik van de [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) schalen van de functies verschillen eenheid. Het wordt geleverd door MLlib voor gebruik in lineaire regressie met stochastische kleurovergang Daalgradiënt (SGD). SGD is een populair algoritme voor het trainen van een breed scala aan andere machine learning-modellen zoals overgegaan regressies of ondersteuning vector machines (SVM).   

> [!TIP]
> Er is vastgesteld dat het algoritme LinearRegressionWithSGD gevoelig functie schalen.   
> 
> 

Dit is de code op schaal variabelen voor gebruik met de regularized lineaire SGD-algoritme.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Tijd uitvoering boven cel: 11.67 seconden

### <a name="cache-objects-in-memory"></a>Cacheobjecten in het geheugen
De benodigde tijd voor trainings- en testdoeleinden ML algoritmen kan worden verkleind door het frame invoergegevens objecten gebruikt voor classificatie, regressie en uitgebreide functies opslaan in cache.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER** 

Tijd uitvoering boven cel: 0.13 seconden

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Voorspellen of een tip wordt betaald met binaire classificatie modellen
Deze sectie wordt beschreven hoe gebruik drie modellen voor de classificatietaak binaire van het voorspellen van al dan niet een tip voor een reis taxi wordt betaald. De modellen die zijn gepresenteerd zijn:

* Logistic regression 
* Willekeurige forest
* Kleurovergang prestatieverbetering structuren

Elk model bouwen sectie code opgedeeld in stappen: 

1. **Training model** gegevens met één parameterset
2. **Evaluatie model** op een test-gegevensset met metrische gegevens
3. **Opslaan van model** in blob voor toekomstige verbruik

Laten we zien hoe naar kruisvalidatie (k/l) te komen met de parameter verstrekkende op twee manieren:

1. Met behulp van **algemene** aangepaste code die kan worden toegepast voor elk algoritme in MLlib en aan elke parameter wordt ingesteld in een algoritme. 
2. Met behulp van de **pySpark CrossValidator pijplijn functie**. Houd er rekening mee dat CrossValidator enkele beperkingen voor Spark 1.5.0 heeft: 
   
   * Pijplijn modellen mag niet zijn opgeslagen/persistent voor toekomstig gebruik.
   * Kan niet worden gebruikt voor elke parameter in een model.
   * Kan niet worden gebruikt voor elk algoritme MLlib.

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Algemene cross-validatie en hyperparameter sweeping gebruikt met de logistic regression-algoritme voor binaire classificatie
De code in deze sectie ziet u hoe te trainen, evalueren en opslaan van een regressiemodel logistic met [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) die voorspelt al dan niet een tip voor een reis in de gegevensset NYC taxi reis en tarief wordt betaald. Het model wordt getraind met kruisverwijzingen validatie (k/l) en hyperparameter sweeping geïmplementeerd met aangepaste code die kan worden toegepast op een van de learning-algoritmen in MLlib.   

> [!NOTE]
> De uitvoering van deze aangepaste k/l-code kan enkele minuten duren.
> 
> 

**Het gebruik van k/l en hyperparameter sweeping logistic regressiemodel trainen**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics

    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)

    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric

    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)


    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Coëfficiënten: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Intercept:-0.0111216486893

Tijd uitvoering boven cel: 14.43 seconden

**Evalueren van het model binaire classificatie met standaard metrische gegevens**

De code in deze sectie wordt beschreven hoe een logistic regressiemodel op basis van een test-gegevensset, met inbegrip van de ROC-curve tekent evalueren.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Gebied onder PR 0.985336538462 =

Gebied onder ROC 0.983383274312 =

Samenvattende statistieken

Precisie 0.984174341679 =

Intrekken 0.984174341679 =

F1 Score 0.984174341679 =

Tijd uitvoering boven cel: 2.67 seconden

**De ROC-curve wordt getekend.**

De *predictionAndLabelsDF* is geregistreerd als een tabel, *tmp_results*, in de vorige cel. *tmp_results* kan worden gebruikt voor query's wilt en uitvoer resultaten in het sqlResults data-frame voor het uitzetten van. Dit is de code.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier volgt de code voor het maken van voorspellingen en de ROC-curve tekenen.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVES
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**UITVOER**

![Logistic regression ROC-curve voor algemene methode](./media/spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

**Model in een blob voor toekomstige consumptie behouden**

De code in deze sectie wordt beschreven hoe de logistic regressiemodel voor het opslaan.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;

    logitBest.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**UITVOER**

Tijd uitvoering boven cel: 34.57 seconden

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>Gebruik van MLlib CrossValidator pipeline-functie met logistic regressiemodel (elastische regressie)
De code in deze sectie ziet u hoe te trainen, evalueren en opslaan van een regressiemodel logistic met [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) die voorspelt al dan niet een tip voor een reis in de gegevensset NYC taxi reis en tarief wordt betaald. Het model wordt getraind met kruisverwijzingen validatie (k/l) en hyperparameter sweeping geïmplementeerd met de functie van de pijplijn MLlib CrossValidator voor k/l met parameter vegen.   

> [!NOTE]
> De uitvoering van deze code MLlib k/l kan enkele minuten duren.
> 
> 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc

    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)

    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER**

Tijd uitvoering boven cel: 107.98 seconden

**De ROC-curve wordt getekend.**

De *predictionAndLabelsDF* is geregistreerd als een tabel, *tmp_results*, in de vorige cel. *tmp_results* kan worden gebruikt voor query's wilt en uitvoer resultaten in het sqlResults data-frame voor het uitzetten van. Dit is de code.

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Hier volgt de code voor het uitzetten van de ROC-curve.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc

    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    #PLOT
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**UITVOER**

![Logistic regression ROC-curve met behulp van MLlib CrossValidator](./media/spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a>Classificatie van willekeurige forest
De code in deze sectie wordt beschreven hoe trainen, evalueren en opslaan van een willekeurige forest regressie die voorspelt al dan niet een tip voor een reis in de gegevensset NYC taxi reis en tarief wordt betaald.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Gebied onder ROC 0.985336538462 =

Tijd uitvoering boven cel: 26.72 seconden

### <a name="gradient-boosting-trees-classification"></a>Kleurovergang prestatieverbetering structuren classificatie
De code in deze sectie wordt beschreven hoe trainen, evalueren, en een kleurovergang prestatieverbetering structuren model die al dan niet een tip wordt betaald voor een reis in de NYC taxi reis voorspelt opslaat en ritbedrag gegevensset.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Gebied onder ROC 0.985336538462 =

Tijd uitvoering boven cel: 28.13 seconden

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Tip bedrag voorspellen met regressie modellen (niet via k/l)
Deze sectie wordt beschreven hoe gebruik drie modellen voor de taak regressie: het aankoopbedrag tip voor een taxi reis op basis van andere functies tip voorspellen. De modellen die zijn gepresenteerd zijn:

* Lineaire regressie overgegaan
* Willekeurige forest
* Kleurovergang prestatieverbetering structuren

Deze modellen zijn beschreven in de introductie. Elk model bouwen sectie code opgedeeld in stappen: 

1. **Training model** gegevens met één parameterset
2. **Evaluatie model** op een test-gegevensset met metrische gegevens
3. **Opslaan van model** in blob voor toekomstige verbruik   

> AZURE Opmerking: Kruisvalidatie wordt niet gebruikt met de drie regressie-modellen in deze sectie, omdat dit is weergegeven in de details voor de logistic regression-modellen. Een voorbeeld van hoe u k/l met elastische Net voor lineaire regressie wordt vermeld in de bijlage van dit onderwerp.
> 
> AZURE Opmerking: In onze ervaring kunnen er problemen optreden bij convergentie van LinearRegressionWithSGD modellen en parameters moeten worden gewijzigd/geoptimaliseerd zorgvuldig voor het verkrijgen van een geldig model. Schalen van variabelen aanzienlijk helpt bij convergentie. Elastische net regressie wordt weergegeven in de bijlage naar dit onderwerp kan ook worden gebruikt in plaats van LinearRegressionWithSGD.
> 
> 

### <a name="linear-regression-with-sgd"></a>Lineaire regressie met SGD
De code in deze sectie leest hoe u geschaalde functies voor het trainen van een lineaire regressie die gebruikmaakt van stochastische kleurovergang daalgradiënt (SGD) voor optimalisatie en te beoordelen, evalueren en sla het model in Azure Blob Storage (WASB).

> [!TIP]
> In onze ervaring kunnen er problemen met de convergentie van LinearRegressionWithSGD modellen en parameters moeten worden gewijzigd/geoptimaliseerd zorgvuldig voor het verkrijgen van een geldig model. Schalen van variabelen aanzienlijk helpt bij convergentie.
> 
> 

    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

Coëfficiënten: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Intercept: 0.854507624459

RMSE 1.23485131376 =

R sqr 0.597963951127 =

Tijd uitvoering boven cel: 38.62 seconden

### <a name="random-forest-regression"></a>Willekeurige Forest regressie
De code in deze sectie wordt beschreven hoe trainen, evalueren en een willekeurige forestmodel die tip bedrag van de NYC taxi reis gegevens voorspelt opslaan.   

> [!NOTE]
> Kruisvalidatie met de parameter verstrekkende met aangepaste code is opgegeven in de bijlage.
> 
> 

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER**

RMSE 0.931981967875 =

R sqr 0.733445485802 =

Tijd uitvoering boven cel: 25.98 seconden

### <a name="gradient-boosting-trees-regression"></a>Kleurovergang prestatieverbetering structuren regressie
De code in deze sectie wordt beschreven hoe trainen, evalueren en een kleurovergang prestatieverbetering structuren model die tip bedrag van de NYC taxi reis gegevens voorspelt opslaat.

** Trainen en evalueren **

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

RMSE 0.928172197114 =

R sqr 0.732680354389 =

Tijd uitvoering boven cel: 20.9 seconden

**Tekenen**

*tmp_results* is geregistreerd als een Hive-tabel in de vorige cel. Resultaten van de tabel worden uitgevoerd in de *sqlResults* tijdskader voor het uitzetten van gegevens. Dit is de code

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Hier volgt de code voor het uitzetten van de gegevens met behulp van de Jupyter-server.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np

    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Werkelijke-vs-voorspeld-tip-bedragen](./media/spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Bijlage: Extra regressie taken met meerdere validatie met parameter krijgen
Deze bijlage bevat de code die laat zien hoe u k/l met elastische net voor lineaire regressie doet en hoe k/l met parameter vegen met aangepaste code voor willekeurige forest regressie.

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Cross-validatie met elastische net voor lineaire regressie
De code in deze sectie toont hoe overschrijden validatie met elastische net voor lineaire regressie en hoe het model met de testgegevens evalueren.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder

    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 

    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

Tijd uitvoering boven cel: 161.21 seconden

**Met R SQR metriek evalueren**

*tmp_results* is geregistreerd als een Hive-tabel in de vorige cel. Resultaten van de tabel worden uitgevoerd in de *sqlResults* tijdskader voor het uitzetten van gegevens. Dit is de code

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Dit is de code voor het berekenen van R sqr.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**UITVOER**

R sqr 0.619184907088 =

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Cross-validatie met parameter vegen met aangepaste code voor willekeurige forest regressie
De code in deze sectie toont hoe overschrijden validatie met parameter vegen met aangepaste code voor willekeurige forest regressie en hoe het model met de testgegevens evalueren.

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid

    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))

    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric

    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER**

RMSE 0.906972198262 =

R sqr 0.740751197012 =

Tijd uitvoering boven cel: 69.17 seconden

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Opschonen van objecten uit het geheugen en de locaties van het model met print
Gebruik `unpersist()` verwijderen van objecten in het cachegeheugen opgeslagen.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


**UITVOER**

PythonRDD [122] op RDD op PythonRDD.scala: 43

** Afdruk pad naar modelbestanden moet worden gebruikt in de notebook verbruik. ** Wilt gebruiken en beoordelen van een onafhankelijke gegevensset, moet u kopieert en plakt deze bestandsnamen in de notebook' verbruik'.

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**UITVOER**

logisticRegFileLoc = modelDir + 'LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528'

linearRegFileLoc = modelDir + 'LinearRegressionWithSGD_2016-05-0316_51_28.433670'

randomForestClassificationFileLoc = modelDir + 'RandomForestClassification_2016-05-0316_50_17.454440'

randomForestRegFileLoc = modelDir + 'RandomForestRegression_2016-05-0316_51_57.331730'

BoostedTreeClassificationFileLoc = modelDir + 'GradientBoostingTreeClassification_2016-05-0316_50_40.138809'

BoostedTreeRegressionFileLoc = modelDir + 'GradientBoostingTreeRegression_2016-05-0316_52_18.827237'

## <a name="whats-next"></a>Volgende stappen
Nu u regressie en classificatie modellen met de MlLib Spark hebt gemaakt, bent u klaar om te leren hoe u kunt beoordelen en evalueren van deze modellen.

**Model verbruik:** voor informatie over het beoordelen en evalueren van de classificatie en regressie modellen in dit onderwerp hebt gemaakt, Zie [Score en evalueren van Spark is gebouwd machine learning-modellen](spark-model-consumption.md).

