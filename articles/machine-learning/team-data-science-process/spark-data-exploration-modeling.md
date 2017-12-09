---
title: Gegevensverkenning en modellering met Spark | Microsoft Docs
description: De gegevens te verkennen en modellering mogelijkheden van de toolkit MLlib Spark op Azure gepresenteerd.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b989b918-5ba5-4696-b8d0-76ae510a23f4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 4b8023b8b2c33ce9fc1a6294e21b1518abbc392b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="data-exploration-and-modeling-with-spark"></a>Met Spark gegevens verkennen en modelleren
[!INCLUDE [machine-learning-spark-modeling](../../../includes/machine-learning-spark-modeling.md)]

In dit scenario worden gebruikt voor gegevensverkenning HDInsight Spark en binaire classificatie en taken van een steekproef van de NYC modelleren regressie taxi reis en ritbedrag 2013 gegevensset.  Dit leidt u door de stappen van de [gegevens wetenschap proces](http://aka.ms/datascienceprocess)end-to- end, met behulp van een HDInsight Spark-cluster voor verwerking en Azure blobs voor het opslaan van de gegevens en de modellen. Het proces wordt verkend en gebracht van een Azure Storage-Blob gegevens visualiseren en vervolgens worden de gegevens voor het bouwen van voorspellende modellen voorbereid. Deze modellen zijn build binaire classificatie en regressie modellering taken uitvoeren met de toolkit Spark MLlib.

* De **binaire classificatie** taak is om te voorspellen of een tip voor de reis wordt betaald. 
* De **regressie** taak is het voorspellen van de hoeveelheid de tip op basis van andere tip-functies. 

De modellen we gebruiken omvatten logistic en lineaire regressie, willekeurige forests en kleurovergang gestimuleerd structuren:

* [Lineaire regressie met SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is een lineair regressiemodel die gebruikmaakt van een methode stochastische kleurovergang Daalgradiënt (SGD) en voor optimalisatie en het onderdeel schalen om te voorspellen van de bedragen tip betaald. 
* [Logistic regression met LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) of regressie 'logit', is een regressiemodel dat kan worden gebruikt wanneer de afhankelijke variabele categorische doen gegevensclassificatie is. LBFGS is een quasi toegepast optimalisatie-algoritme dat benadert de Broyden – Fletcher – Goldfarb – Shanno (BFGS)-algoritme met een beperkte hoeveelheid computergeheugen en die wordt veel gebruikt in machine learning.
* [Willekeurige forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) ensembles van beslissingsstructuren zijn.  Ze combineren veel beslissingsstructuren om het risico te beperken. Willekeurige forests worden gebruikt voor regressie en classificatie en categorische functies kunnen verwerken en kunnen worden uitgebreid naar de instelling multiklassen classificatie. Ze hoeven niet functie schalen en kunnen niet met mogelijkheid tot vastleggen en interacties functie zijn. Willekeurige forests zijn een van de meest succesvolle machine learning-modellen voor de indeling en regressie.
* [Verloop boosted structuren](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ensembles van beslissingsstructuren zijn. GBTs training beslissingsstructuren iteratief aan een functie gegevensverlies te minimaliseren. GBTs worden gebruikt voor regressie en classificatie en categorische functies kan verwerken, hoeven niet functie schalen en kunnen niet met mogelijkheid tot vastleggen en interacties functie. Ze kunnen ook worden gebruikt in een setting multiklasse classificatie.

De stappen modellering ook bevatten code waarin wordt getoond hoe te trainen, evalueren en opslaan van elk type model. Code van de oplossing en weergeven van de relevante waarnemingspunten is Python gebruikt.   

> [!NOTE]
> Hoewel de toolkit Spark MLlib is ontworpen om te werken op grote gegevenssets, wordt hier een voorbeeld van een relatief klein (ongeveer 30 Mb met behulp van 170K rijen, ongeveer 0,1% van de oorspronkelijke gegevensset van de NYC) gebruikt voor het gemak. De hier opgegeven oefening efficiënt (in ongeveer 10 minuten) op een HDInsight-cluster met 2 worker-knooppunten wordt uitgevoerd. De dezelfde code, met kleine wijzigingen kan worden gebruikt voor het verwerken van grotere gegevenssets-wordt met de juiste wijzigingen voor het cachen van gegevens in het geheugen en het wijzigen van de clustergrootte.
> 
> 

## <a name="prerequisites"></a>Vereisten
U moet een Azure-account en een 1.6 Spark (of Spark 2.0) HDInsight-cluster voor dit scenario. Zie de [overzicht van Gegevenswetenschap met Spark op Azure HDInsight](spark-overview.md) voor instructies voor het voldoen aan deze eisen. Dit onderwerp bevat ook een beschrijving van de NYC 2013 Taxi gegevens hier gebruikt en instructies over het uitvoeren van code van een Jupyter-notebook in Spark-cluster. 

## <a name="spark-clusters-and-notebooks"></a>Spark-clusters en laptops
Instellingsstappen en code vindt u in dit scenario voor het gebruik van een HDInsight Spark 1.6. Maar Jupyter-notebooks zijn opgegeven voor zowel HDInsight Spark 1.6 en 2.0 Spark-clusters. Een beschrijving van de laptops en koppelingen naar deze vindt u in de [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) voor de GitHub-opslagplaats met deze. Bovendien moet de code hier en in de gekoppelde laptops is algemeen en moet werken op een Spark-cluster. Als u HDInsight Spark niet gebruikt, is het mogelijk dat de cluster-installatie en beheer stappen enigszins afwijken van wat hier moet worden weergegeven. Voor het gemak zijn hier de koppelingen naar de Jupyter-notebooks voor Spark 1.6 (om te worden uitgevoerd in de pySpark-kernel van de server Jupyter-Notebook) en Spark 2.0 (om te worden uitgevoerd in de kernel pySpark3 van de server Jupyter-Notebook):

### <a name="spark-16-notebooks"></a>Spark 1.6 laptops

[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): bevat informatie over het uitvoeren van gegevensverkenning modelleren en batchscoreberekening met diverse verschillende algoritmen.

### <a name="spark-20-notebooks"></a>Spark 2.0-laptops
De regressie en classificatie taken die zijn geïmplementeerd met een 2.0 Spark-cluster in afzonderlijke notitieblokken zijn en de classificatie-notebook gebruikt een andere gegevensset:

- [Spark2.0-pySpark3-machine-Learning-Data-Science-Spark-Advanced-Data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): dit bestand bevat informatie over het uitvoeren van gegevensverkenning, modelleren, en met behulp van de NYC Taxi reis scores in Spark 2.0-clusters en tarief gegevensset-beschreven [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Deze laptop is mogelijk een goed uitgangspunt voor het snel verkennen van de code die we voor Spark 2.0 hebt opgegeven. Voor een meer gedetailleerde notebook de gegevens van de NYC Taxi analyseert, Zie de volgende notebook in deze lijst. Zie de opmerkingen na deze lijst die deze laptops vergelijken. 
- [Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): dit bestand ziet u hoe u gegevens worsteling (Spark SQL en dataframe bewerkingen), exploratie, model en score berekenen met behulp van de NYC Taxi reis en tarief set gegevens die worden beschreven [hier ](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): dit bestand wordt beschreven hoe gegevens worsteling (Spark SQL en dataframe bewerkingen), exploratie, model en score berekenen met behulp van de bekende luchtvaartmaatschappij tijdige afwijking uitvoeren de gegevensset van 2011 en 2012. Wij de luchtvaartmaatschappij gegevensset met de luchthaven weergegevens (bijvoorbeeld windsnelheid, temperatuur, hoogte enz.) geïntegreerd vóór modelleren, zodat deze weer-functies kunnen worden opgenomen in het model.

<!-- -->

> [!NOTE]
> De gegevensset luchtvaartmaatschappij is toegevoegd aan de laptops Spark 2.0 ter illustratie van het gebruik van bestandsclassificatie-algoritmen beter. Zie de volgende koppelingen voor meer informatie over luchtvaartmaatschappij tijdige vertrek gegevensset en gegevensset weer:

>- Luchtvaartmaatschappij tijdige vertrek gegevens: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Luchthaven weergegevens: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
De notitieblokken Spark 2.0 op de NYC taxi en luchtvaartmaatschappij vlucht vertraging-gegevenssets duurt 10 minuten of langer om uit te voeren (afhankelijk van de grootte van uw HDI-cluster). De eerste laptop in de bovenstaande lijst geeft veel aspecten van de gegevensverkenning, visualisatie en ML-model opleiding in een laptop die het kost minder tijd om uit te voeren met een lagere actieve NYC gegevensset, waarin de bestanden taxi en tarief vooraf lid zijn: [ Spark2.0-pySpark3-machine-Learning-Data-Science-Spark-Advanced-Data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) deze notebook neemt een veel kortere tijd om te voltooien (2-3 minuten) en kan worden een goed startpunt voor de code die we hebben opgegeven snel verkennen voor Spark 2.0. 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
De onderstaande beschrijvingen zijn gerelateerd aan met behulp van Spark 1.6. Gebruik de laptops beschreven en bovenstaande voor Spark 2.0-versies. 

<!-- -->

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
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Bibliotheken importeren
Instellen is ook vereist nodig bibliotheken importeren. Context voor spark instellen en importeer nodig bibliotheken met de volgende code:

    # IMPORT LIBRARIES
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

## <a name="data-ingestion-from-public-blob"></a>Gegevensopname van een openbare blob
De eerste stap in het proces van de wetenschappelijke gegevens is om op te nemen van de gegevens te analyseren van bronnen waar is bevindt zich in uw gegevens te verkennen en modellering omgeving. De omgeving is Spark in dit scenario. Deze sectie bevat de code voor het voltooien van een reeks taken:

* de steekproef gemodelleerd opnemen
* Lees in de invoergegevensset (opgeslagen als een bestand .tsv)
* indeling en de gegevens opgeschoond
* maken en objecten (RDDs of gegevens frames) in het geheugen in de cache
* registreren als een temp-tabel in SQL-context.

Hier volgt de code voor gegevensopname.

    # INGEST DATA

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


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:**

Tijd uitvoering boven cel: 51.72 seconden

## <a name="data-exploration--visualization"></a>Gegevensverkenning & visualisatie
Nadat de gegevens in Spark is ingesteld, is de volgende stap in het proces van de wetenschappelijke gegevens dieper inzicht van de gegevens via de exploratie en visualisatie te krijgen. In deze sectie we de taxi gegevens met behulp van SQL-query's controleren en de doelvariabelen en potentiële functies voor visuele inspectie tekenen. In het bijzonder voert u de frequentie van de passagiers aantallen in taxi reizen, de frequentie van tip bedragen en hoe tips is afhankelijk van de hoeveelheid betaling en het type.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Een histogram van passagiers aantal frequenties in de steekproef van taxi heen wordt getekend
Deze code en de volgende codefragmenten gebruik van SQL-magic query uitvoeren op de lokale magic de gegevens worden uitgezet en voorbeelden.

* **SQL-magic (`%%sql`)** eenvoudig inline HiveQL query's op de sqlContext biedt ondersteuning voor het HDInsight-PySpark-kernel. De (-o naam_variabele) argument als een DataFrame Pandas op de Jupyter-server de uitvoer van de SQL-query zich blijft voordoen. Dit betekent dat deze beschikbaar zijn in de lokale modus.
* De  **`%%local` magic** code lokaal uitvoeren op de Jupyter-server, waarop de headnode van het HDInsight-cluster wordt gebruikt. Normaal gesproken gebruikt u `%%local` magische in combinatie met de `%%sql` met -o parameter magic. De uitvoer van de SQL-query lokaal wilt behouden door de parameter -o en vervolgens %% lokale magic zou de volgende reeks codefragment lokaal uitvoeren op basis van de uitvoer van de SQL-query's die lokaal wordt bewaard

De uitvoer wordt automatisch weergegeven nadat u de code hebt uitgevoerd.

Deze query haalt de reizen op het aantal van de passagiers. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Deze code maakt een lokale gegevens tijdskader van de query-uitvoer en de gegevens zijn getekend. De `%%local` magic maakt een lokale gegevens-frame, `sqlResults`, die kan worden gebruikt voor het uitzetten van matplotlib. 

> [!NOTE]
> Deze PySpark-magic wordt meerdere malen gebruikt in dit scenario. Als de hoeveelheid gegevens te groot is, moet u een steekproef nemen voor het maken van een gegevens-frame dat past in het lokale geheugen.
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Hier volgt de code voor het uitzetten van de reizen door passagiers aantallen

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**UITVOER:**

![De frequentie reis op het aantal passagiers](./media/spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

U kunt kiezen uit verschillende soorten visualisaties (tabel-, cirkel, regel, gebied of balk) met behulp van de **Type** menuknoppen in de notebook. De grafiek balk wordt hier weergegeven.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Zet een histogram van de tip bedragen en hoe tip bedrag hangt af van de passagiers telling en het tarief bedragen.
Gebruik een SQL-query om de voorbeeldgegevens.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST THE sqlContext
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

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**UITVOER:** 

![Tip bedragdistributie](./media/spark-data-exploration-modeling/tip-amount-distribution.png)

![Hoeveelheid op het aantal passagiers Tip](./media/spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tip bedrag door tarief bedrag](./media/spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Functie-engineering, transformatie en gegevens voorbereiding voor modellering
Deze sectie wordt beschreven en vindt u de code voor procedures voor het voorbereiden van gegevens voor gebruik in ML-model. Er wordt weergegeven hoe de volgende taken uitvoeren:

* Maakt een nieuwe functie met binning uur in verkeer tijd buckets
* Index en categorische functies coderen
* Gelabelde point-objecten voor invoer in ML functies maken
* Een onderliggende steekproeven van de gegevens maken en deze te splitsen in trainings- en testdoeleinden sets
* Functie schalen
* Cacheobjecten in het geheugen

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Maakt een nieuwe functie met binning uur in verkeer tijd buckets
Deze code laat zien hoe u een nieuwe functie maakt met binning uren in verkeer tijd buckets en hoe u in de cache van het resulterende gegevensframe in het geheugen. Flexibele gegevenssets gedistribueerd (RDDs) en gegevens frames herhaaldelijk gebruikt, leidt opslaan in cache tot verbeterde uitvoeringstijden. Dienovereenkomstig, we RDDs en gegevensframes cache in verschillende fasen in het overzicht. 

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

**UITVOER:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Index en het coderen van categorische functies voor invoer in het modelleren van functies
Deze sectie wordt beschreven hoe index of categorische functies voor invoer in de functies modellering coderen. Het model en het voorspellen van de functies van MLlib functies met categorische invoergegevens worden geïndexeerd of gecodeerd vóór gebruik vereist. Afhankelijk van het model moet u de index of ze coderen op verschillende manieren:  

* **Op basis van een structuur modellering** vereist categorieën moeten worden gecodeerd als numerieke waarden (bijvoorbeeld, een onderdeel met drie categorieën kan worden gecodeerd met 0, 1, 2). Dit wordt geleverd door de MLlib [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) functie. Deze functie codeert een tekenreekskolom van de labels aan een kolom van een label-indexen die zijn besteld op het label frequenties. Hoewel geïndexeerd met numerieke waarden voor de invoer- en de verwerking van gegevens, kunnen de structuur gebaseerde algoritmen worden opgegeven op de juiste manier behandelen als categorieën. 
* **Logistic en lineaire regressie modellen** vereisen een hot codering, where, bijvoorbeeld, een onderdeel met drie categorieën kan worden uitgebreid naar drie kolommen van de functie, waarbij elke met 0 of 1, afhankelijk van de categorie van een observatie. MLlib biedt [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) doen van één hot codering. Een kolom van een label indexen deze encoder toegewezen aan een kolom van de binaire aanvalsvectoren, met maximaal één one-waarde. Met deze codering kunt algoritmen die numerieke waarden functies, zoals logistic regression, moet worden toegepast op categorische functies verwacht.

Dit is de code om te indexeren en coderen categorische functies:

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

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

**UITVOER:**

Tijd uitvoering boven cel: 1.28 seconden

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Gelabelde point-objecten voor invoer in ML functies maken
In deze sectie bevat de code die wordt beschreven hoe categorische gegevens als een gegevenstype gelabelde punt index en deze coderen zodat deze kan worden gebruikt voor het trainen en te testen MLlib logistic regression en andere classificatie-modellen. Gelabelde point-objecten zijn robuuste gedistribueerd gegevenssets (RDD) geformatteerd op een manier die is vereist als de invoergegevens voor de meeste ML algoritmen in MLlib. Een [punt gelabeld](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is een lokale vector, dense of sparse, die is gekoppeld aan een label/antwoord.  

Deze sectie bevat de code die laat zien hoe index categorische gegevens als een [punt gelabeld](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) gegevenstype en deze te coderen zodat deze kan worden gebruikt voor het trainen en te testen MLlib logistic regression en andere classificatie-modellen. Gelabelde point-objecten zijn robuuste gedistribueerd gegevenssets (RDD) die bestaan uit een label (doel/antwoord-variabele) en de functie vector. Deze indeling is nodig als invoer door veel ML algoritmen in MLlib.

Dit is de code om te indexeren en het coderen van tekstfuncties voor binaire classificatie.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
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
Deze code maakt een willekeurige steekproef van de gegevens (25% wordt hier gebruikt). Hoewel dit niet vereist voor dit voorbeeld vanwege de grootte van de gegevensset, ziet u hoe u kunt een steekproef nemen hier dus u kunt gebruiken voor uw eigen probleem wanneer deze nodig is. Als voorbeelden groot zijn, kan dit aanzienlijke tijd tijdens de training modellen opslaan. Naast we het voorbeeld in een training gedeelte (hier 75%) en een test deel (25% hier) moet worden gebruikt in de classificatie en regressie modellering opgesplitst.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

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

**UITVOER:**

Tijd uitvoering boven cel: 0,24 seconden

### <a name="feature-scaling"></a>Functie schalen
Functie schaal, ook wel bekend als gegevensnormalisatie, weet u zeker dat functies met veel betaald waarden zijn niet opgegeven overmatige afwegen in de beoogde functie. De code voor de functie schalen maakt gebruik van de [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) schalen van de functies verschillen eenheid. Het wordt geleverd door MLlib voor gebruik in lineaire regressie met stochastische kleurovergang Daalgradiënt (SGD), een populaire algoritme voor het trainen van een breed scala aan andere machine learning-modellen zoals overgegaan regressies of ondersteuning vector machines (SVM).

> [!NOTE]
> Er is vastgesteld dat het algoritme LinearRegressionWithSGD gevoelig functie schalen.
> 
> 

Dit is de code op schaal variabelen voor gebruik met de regularized lineaire SGD-algoritme.

    # FEATURE SCALING

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

**UITVOER:**

Tijd uitvoering boven cel: 13.17 seconden

### <a name="cache-objects-in-memory"></a>Cacheobjecten in het geheugen
De benodigde tijd voor trainings- en testdoeleinden ML algoritmen kan worden verkleind door het frame invoergegevens objecten voor classificatie, regressie, gebruikt en de uitgebreide functies opslaan in cache.

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

**UITVOER:** 

Tijd uitvoering boven cel: 0,15 seconden

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Voorspellen of een tip wordt betaald met binaire classificatie modellen
Deze sectie wordt beschreven hoe gebruik drie modellen voor de classificatietaak binaire van het voorspellen van al dan niet een tip voor een reis taxi wordt betaald. De modellen die zijn gepresenteerd zijn:

* Logistic regression overgegaan 
* Willekeurige forestmodel
* Kleurovergang prestatieverbetering structuren

Elk model bouwen sectie code opgedeeld in stappen: 

1. **Training model** gegevens met één parameterset
2. **Evaluatie model** op een test-gegevensset met metrische gegevens
3. **Opslaan van model** in blob voor toekomstige verbruik

### <a name="classification-using-logistic-regression"></a>Classificatie met logistic regression
De code in deze sectie ziet u hoe te trainen, evalueren en opslaan van een regressiemodel logistic met [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) die voorspelt al dan niet een tip voor een reis in de gegevensset NYC taxi reis en tarief wordt betaald.

**Het gebruik van k/l en hyperparameter sweeping logistic regressiemodel trainen**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics


    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**UITVOER:** 

Coëfficiënten: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Intercept:-0.0111216486893

Tijd uitvoering boven cel: 14.43 seconden

**Evalueren van het model binaire classificatie met standaard metrische gegevens**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))

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


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**UITVOER:** 

Gebied onder PR 0.985297691373 =

Gebied onder ROC 0.983714670256 =

Samenvattende statistieken

Precisie 0.984304060189 =

Intrekken 0.984304060189 =

F1 Score 0.984304060189 =

Tijd uitvoering boven cel: 57.61 seconden

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

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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


**UITVOER:**

![Logistic regression ROC curve.png](./media/spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a>Classificatie van willekeurige forest
De code in deze sectie wordt beschreven hoe trainen, evalueren en opslaan van een willekeurige forestmodel die voorspelt al dan niet een tip voor een reis in de gegevensset NYC taxi reis en tarief wordt betaald.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

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
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
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

**UITVOER:**

Gebied onder ROC 0.985297691373 =

Tijd uitvoering boven cel: 31.09 seconden

### <a name="gradient-boosting-trees-classification"></a>Kleurovergang prestatieverbetering structuren classificatie
De code in deze sectie wordt beschreven hoe trainen, evalueren, en een kleurovergang prestatieverbetering structuren model die al dan niet een tip wordt betaald voor een reis in de NYC taxi reis voorspelt opslaat en ritbedrag gegevensset.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
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


**UITVOER:**

Gebied onder ROC 0.985297691373 =

Tijd uitvoering boven cel: 19.76 seconden

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Tip bedragen voor taxi reizen met regressie modellen voorspellen
Deze sectie wordt beschreven hoe drie modellen gebruiken voor de taak regressie van het voorspellen van de hoeveelheid de tip voor een taxi reis op basis van andere functies tip betaald. De modellen die zijn gepresenteerd zijn:

* Lineaire regressie overgegaan
* Willekeurige forest
* Kleurovergang prestatieverbetering structuren

Deze modellen zijn beschreven in de introductie. Elk model bouwen sectie code opgedeeld in stappen: 

1. **Training model** gegevens met één parameterset
2. **Evaluatie model** op een test-gegevensset met metrische gegevens
3. **Opslaan van model** in blob voor toekomstige verbruik

### <a name="linear-regression-with-sgd"></a>Lineaire regressie met SGD
De code in deze sectie leest hoe u geschaalde functies voor het trainen van een lineaire regressie die gebruikmaakt van stochastische kleurovergang daalgradiënt (SGD) voor optimalisatie en te beoordelen, evalueren en sla het model in Azure Blob Storage (WASB).

> [!TIP]
> In onze ervaring kunnen er problemen met de convergentie van LinearRegressionWithSGD modellen en parameters moeten worden gewijzigd/geoptimaliseerd zorgvuldig voor het verkrijgen van een geldig model. Schalen van variabelen aanzienlijk helpt bij convergentie. 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

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

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

Coëfficiënten: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,- 0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

Intercept: 0.853872718283

RMSE 1.24190115863 =

R sqr 0.608017146081 =

Tijd uitvoering boven cel: 58,42 seconden

### <a name="random-forest-regression"></a>Willekeurige Forest regressie
De code in deze sectie wordt beschreven hoe trainen, evalueren en een willekeurige forest regressie die tip bedrag van de NYC taxi reis gegevens voorspelt opslaan.

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
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

**UITVOER:**

RMSE 0.891209218139 =

R sqr 0.759661334921 =

Tijd uitvoering boven cel: 49.21 seconden

### <a name="gradient-boosting-trees-regression"></a>Kleurovergang prestatieverbetering structuren regressie
De code in deze sectie wordt beschreven hoe trainen, evalueren en een kleurovergang prestatieverbetering structuren model die tip bedrag van de NYC taxi reis gegevens voorspelt opslaat.

** Trainen en evalueren **

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**UITVOER:**

RMSE 0.908473148639 =

R sqr 0.753835096681 =

Tijd uitvoering boven cel: 34.52 seconden

**Tekenen**

*tmp_results* is geregistreerd als een Hive-tabel in de vorige cel. Resultaten van de tabel worden uitgevoerd in de *sqlResults* tijdskader voor het uitzetten van gegevens. Dit is de code

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Hier volgt de code voor het uitzetten van de gegevens met behulp van de Jupyter-server.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)


**UITVOER:**

![Werkelijke-vs-voorspeld-tip-bedragen](./media/spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a>Opschonen van objecten uit het geheugen
Gebruik `unpersist()` verwijderen van objecten in het cachegeheugen opgeslagen.

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()

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


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>De opslaglocaties record van de modellen voor gebruiks- en score berekenen
Om te gebruiken en beoordelen van een onafhankelijke gegevensset wordt beschreven in de [Score en evalueren van Spark is gebouwd machine learning-modellen](spark-model-consumption.md) onderwerp, moet u kopieert en plakt u deze met de opgeslagen modellen die hier worden gemaakt in het verbruik bestandsnamen Jupyter-notebook. Dit is de code de paden naar modelbestanden u er moet afdrukken.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**UITVOER**

logisticRegFileLoc = modelDir + 'LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568'

linearRegFileLoc = modelDir + 'LinearRegressionWithSGD_2016-05-0317_05_21.577773'

randomForestClassificationFileLoc = modelDir + 'RandomForestClassification_2016-05-0317_04_11.950206'

randomForestRegFileLoc = modelDir + 'RandomForestRegression_2016-05-0317_06_08.723736'

BoostedTreeClassificationFileLoc = modelDir + 'GradientBoostingTreeClassification_2016-05-0317_04_36.346583'

BoostedTreeRegressionFileLoc = modelDir + 'GradientBoostingTreeRegression_2016-05-0317_06_51.737282'

## <a name="whats-next"></a>Volgende stappen
Nu u regressie en classificatie modellen met de MlLib Spark hebt gemaakt, bent u klaar om te leren hoe u kunt beoordelen en evalueren van deze modellen. De geavanceerde gegevensverkenning en laptop modelleren diepere dives naar kruisvalidatie, hyper-parameter verstrekkende, inclusief en model van evaluatie. 

**Model verbruik:** voor informatie over het beoordelen en evalueren van de classificatie en regressie modellen in dit onderwerp hebt gemaakt, Zie [Score en evalueren van Spark is gebouwd machine learning-modellen](spark-model-consumption.md).

**Kruisvalidatie en hyperparameter sweeping**: Zie [geavanceerde gegevensverkenning en modellering met Spark](spark-advanced-data-exploration-modeling.md) over de manier waarop modellen kunnen getraind met behulp van kruisvalidatie en hyper-parameter sweeping

