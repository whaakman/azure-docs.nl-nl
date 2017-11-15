---
title: Met behulp van Scala en Spark op Azure Gegevenswetenschap | Microsoft Docs
description: Het gebruik van Scala voor bewaakte machine learning taken met de Spark schaalbare MLlib en Spark ML pakketten op een Azure HDInsight Spark-cluster.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: a7c97153-583e-48fe-b301-365123db3780
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev;deguhath
ms.openlocfilehash: 940911144993f30723ad395722742c81a4b0a71c
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="data-science-using-scala-and-spark-on-azure"></a>Gegevenswetenschap met Scala en Spark op Azure
In dit artikel leest u hoe Scala gebruiken voor beheerde machine learning taken met de Spark schaalbare MLlib en Spark ML pakketten op een Azure HDInsight Spark-cluster. Dit leidt u door de taken die deel uitmaken van de [proces voor Gegevenswetenschap](http://aka.ms/datascienceprocess): gegevensopname en verkennen, visualisatie, functie-engineering, modellering en model verbruik. De modellen in het artikel zijn logistic en lineaire regressie, willekeurige forests en verloop boosted structuren (GBTs), naast de twee algemene beheerde machine learning-taken:

* Regressie probleem: voorspelling van de hoeveelheid tip ($) voor een reis taxi
* Binaire classificatie: voorspelling van tip of geen tip (1/0) voor een reis taxi

Het modelleringsproces is vereist voor trainings- en evaluatie van een testgegevensset en relevante nauwkeurigheid metrische gegevens. In dit artikel kunt u informatie over het opslaan van deze modellen in Azure Blob-opslag en het beoordelen en evalueren hun voorspellende prestaties. In dit artikel omvat tevens de geavanceerdere onderwerpen van modellen met behulp van kruisvalidatie en hyper-parameter sweeping optimaliseren. De gegevens die worden gebruikt, is een voorbeeld van de 2013 NYC taxi reis en tarief gegevensset beschikbaar op GitHub.

[Scala](http://www.scala-lang.org/), een taal die op basis van de virtuele Java-machine objectgeoriënteerd en functionele Taalconcepten integreert. Er is een schaalbare taal die zeer geschikt voor gedistribueerde verwerking in de cloud en wordt uitgevoerd op Azure Spark-clusters.

[Spark](http://spark.apache.org/) is een open source parallelle verwerking framework die ondersteuning biedt voor in-memory verwerking om zo de prestaties van toepassingen voor big data-analyses. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak en geavanceerde analyses. De Spark in-memory gedistribueerde rekencapaciteiten kunnen u een goede keuze voor zich herhalende algoritmen in machine learning- en grafiekberekeningen. De [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) pakket biedt een uniforme set op hoog niveau API's die zijn gebouwd op gegevens frames waarmee u kunnen maken en praktische machine learning-pijplijnen afstemmen. [MLlib](http://spark.apache.org/mllib/) is Spark van schaalbare machine learning-bibliotheek, dat het modellering mogelijkheden voor deze gedistribueerde omgeving.

[HDInsight Spark](../../hdinsight/spark/apache-spark-overview.md) is de oplossing Azure gehoste van open-source Spark. Ook biedt ondersteuning voor Jupyter Scala notitieblokken op het Spark-cluster en interactieve Spark SQL query transformeren en opgeslagen in Azure Blob storage-gegevens visualiseren filteren's kunnen worden uitgevoerd. De Scala codefragmenten in dit artikel die voorzien van de oplossingen en weergeven van de relevante waarnemingspunten om de gegevens te visualiseren in Jupyter-notebooks geïnstalleerd op de Spark-clusters worden uitgevoerd. De stappen modelleren in de volgende onderwerpen hebben code die wordt beschreven hoe trainen, evalueren, opslaan en gebruiken van elk type model u.

De installatiestappen en code in dit artikel zijn voor Azure HDInsight 3.4 Spark 1.6. Echter, de code in dit artikel en in de [Scala Jupyter-Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) algemeen zijn en moet werken op een Spark-cluster. De instructies voor het cluster setup en beheer mogelijk enigszins afwijken van wat wordt weergegeven in dit artikel als u geen van HDInsight Spark gebruikmaakt.

> [!NOTE]
> Zie voor een onderwerp waarin wordt getoond hoe u Python in plaats van Scala gebruikt om taken voor een end-to-end Gegevenswetenschap proces te voltooien, [Gegevenswetenschap met Spark op Azure HDInsight](spark-overview.md).
> 
> 

## <a name="prerequisites"></a>Vereisten
* U moet een Azure-abonnement hebben. Als u geen hebt nog, [ophalen van een gratis proefversie van Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* U moet een Azure HDInsight 3.4 Spark 1.6 cluster om de volgende procedures te voltooien. Zie de instructies in een cluster maken [aan de slag: Apache Spark maken in Azure HDInsight](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Instellen van het clustertype en de versie op de **clustertype Selecteer** menu.

![Configuratie van het HDInsight-cluster](./media/scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

Zie voor een beschrijving van de NYC taxi reis gegevens en instructies over hoe u code uitvoeren van een Jupyter-notebook in Spark-cluster, de relevante secties in [overzicht van Gegevenswetenschap met Spark op Azure HDInsight](spark-overview.md).  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Scala code uitvoeren van een Jupyter-notebook in Spark-cluster
U kunt een Jupyter-notebook vanuit de Azure-portal op te starten. Zoek het Spark-cluster op uw dashboard en klik erop om in te voeren van de beheerpagina voor uw cluster. Klik vervolgens op **Clusterdashboards**, en klik vervolgens op **Jupyter-Notebook** openen van de notebook die zijn gekoppeld aan het Spark-cluster.

![Cluster-dashboard en de Jupyter-notebooks](./media/scala-walkthrough/spark-jupyter-on-portal.png)

U ook toegang tot Jupyter-notebooks op https://&lt;clustername&gt;.azurehdinsight.net/jupyter. Vervang *clustername* met de naam van het cluster. U moet het wachtwoord voor uw beheerdersaccount voor toegang tot de Jupyter-notebooks.

![Ga naar de Jupyter-notebooks met behulp van de naam van het cluster](./media/scala-walkthrough/spark-jupyter-notebook.png)

Selecteer **Scala** om te zien van een map met een paar voorbeelden van voorverpakte laptops die gebruikmaken van de PySpark-API. De exploratie modelleren en score berekenen met behulp van Scala.ipynb notebook met de code voor deze reeks Spark onderwerpen is beschikbaar op voorbeelden [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).

U kunt de notebook rechtstreeks vanuit GitHub op de server Jupyter-Notebook in Spark-cluster uploaden. Klik op uw startpagina Jupyter op de **uploaden** knop. Plak de URL GitHub (onbewerkte inhoud) van de notebook Scala in de Verkenner en klik vervolgens op **Open**. De notebook Scala is beschikbaar op de volgende URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Instellen: Voorinstelling Spark en Hive-contexten, Spark magics en Spark bibliotheken
### <a name="preset-spark-and-hive-contexts"></a>Vooraf ingestelde Spark en Hive-contexten
    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


De Spark kernels die worden geleverd met Jupyter-notebooks hebben vooraf ingestelde contexten. U hoeft niet te de Spark expliciet zijn ingesteld of Hive-contexten voordat u begint met het werken met de toepassing die u ontwikkelt. De vooraf ingestelde contexten zijn:

* `sc`voor SparkContext
* `sqlContext`voor HiveContext

### <a name="spark-magics"></a>Spark magics
De kernel Spark biedt een aantal vooraf gedefinieerde 'magics', die zijn speciale opdrachten die u met aanroepen kunt `%%`. Twee van deze opdrachten worden gebruikt in de volgende codevoorbeelden.

* `%%local`Hiermee geeft u op dat de code in de volgende regels lokaal worden uitgevoerd. De sitecode moet geldige Scala-code.
* `%%sql -o <variable name>`voert een Hive-query op `sqlContext`. Als de `-o` parameter is doorgegeven, het resultaat van de query wordt bewaard de `%%local` Scala context als een tijdskader Spark-gegevens.

Voor meer informatie over de kernels voor Jupyter-notebooks en hun vooraf gedefinieerde 'magics' die u aanroept met `%%` (bijvoorbeeld `%%local`), Zie [beschikbare Kernels voor Jupyter-notebooks met HDInsight Spark Linux-clusters in HDInsight](../../hdinsight/spark/apache-spark-jupyter-notebook-kernels.md).

### <a name="import-libraries"></a>Bibliotheken importeren
Importeer de Spark, MLlib en andere bibliotheken u moet met behulp van de volgende code.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Gegevensopname
De eerste stap in het proces voor Gegevenswetenschap is het opnemen van de gegevens die u wilt analyseren. U brengt de gegevens van externe bronnen of systemen waarin deze zich bevindt in uw gegevens te verkennen en modellering omgeving. In dit artikel is de gegevens die u wilt opnemen een gekoppelde 0,1% voorbeeld van het taxi reis en tarief-bestand (opgeslagen als een bestand .tsv). De gegevens te verkennen en modellering omgeving is Spark. Deze sectie bevat de code voor het voltooien van de volgende reeks taken:

1. Instellen van directory-paden voor opslag van gegevens en het model.
2. Lees in de invoerset van de gegevens (opgeslagen als een bestand .tsv).
3. Een schema voor de gegevens definiëren en de gegevens opgeschoond.
4. Een kader opgeschoonde gegevens maken en deze in de cache in het geheugen.
5. De gegevens als een tijdelijke tabel in SQLContext registreren.
6. Query uitvoeren op de tabel en de resultaten in een gegevensframe importeren.

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Directory-paden voor opslaglocaties in Azure Blob-opslag instellen
Spark kunt lezen en schrijven naar Azure Blob-opslag. U kunt gebruiken voor het verwerken van uw bestaande gegevens en slaan de resultaten weer in Blob-opslag.

Voor het opslaan van modellen of bestanden in Blob-opslag, moet u het pad correct opgeven. Verwijst naar de standaard-container die is gekoppeld aan het Spark-cluster met behulp van een pad dat met begint `wasb:///`. Verwijzen naar andere locaties met behulp van `wasb://`.

Het volgende codevoorbeeld geeft de locatie van de invoergegevens om te lezen en het pad naar een Blob-opslag die is gekoppeld aan het Spark-cluster waarin het model moet worden opgeslagen.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Gegevens importeren, maakt u een RDD en een gegevensframe volgens het schema definiëren
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 8 seconden.

### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Query uitvoeren op de tabel en resulteert in een gegevensframe importeren
Vervolgens wordt de querytabel voor tarief, passagiers en tip gegevens; de gegevens beschadigd en afgelegen; uitfilteren en afdrukken van meerdere rijen.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Uitvoer:**

| fare_amount | passenger_count | tip_amount | punt |
| --- | --- | --- | --- |
|        13.5 |1.0 |2.9 |1.0 |
|        16.0 |2.0 |3.4 |1.0 |
|        10.5 |2.0 |1.0 |1.0 |

## <a name="data-exploration-and-visualization"></a>Gegevensverkenning en visualisatie
Nadat u de gegevens in Spark brengt, wordt de volgende stap in het proces voor Gegevenswetenschap is een beter begrip van de gegevens via de exploratie en visualisatie krijgen. In deze sectie kunt u de gegevens taxi controleren met behulp van SQL-query's. De resultaten vervolgens importeren in een kader gegevens om te tekenen van de doelvariabelen en potentiële functies voor visuele controle met behulp van de functie automatisch visualisatie van Jupyter.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Gebruik van lokale en SQL-magic gegevens worden uitgezet
De uitvoer van een codefragment die u vanaf een Jupyter-notebook uitvoert is standaard beschikbaar in de context van de sessie die is opgeslagen op de worker-knooppunten. Als u wilt een reis opslaan met de werkrolknooppunten voor elke berekeningen en als alle gegevens die u nodig hebt voor de berekening is lokaal beschikbaar op het serverknooppunt Jupyter (dit is het hoofdknooppunt), kunt u de `%%local` magic het codefragment uitvoeren op de Jupyter-server.

* **SQL-magic** (`%%sql`). De kernel HDInsight Spark ondersteunt eenvoudig inline HiveQL query's op SQLContext. De (`-o VARIABLE_NAME`) argument als een frame Pandas gegevens op de Jupyter-server de uitvoer van de SQL-query zich blijft voordoen. Dit betekent waarschijnlijk beschikbaar in de lokale modus.
* `%%local`**magic**. De `%%local` magic de code wordt lokaal uitgevoerd op de Jupyter-server, waarop het hoofdknooppunt van het HDInsight-cluster. Normaal gesproken gebruikt u `%%local` magische in combinatie met de `%%sql` magische met de `-o` parameter. De `-o` parameter zou de uitvoer van de SQL-query lokaal behouden en vervolgens `%%local` magic zou de volgende reeks codefragment lokaal uitvoeren op basis van de uitvoer van de SQL-query's die lokaal wordt bewaard.

### <a name="query-the-data-by-using-sql"></a>De gegevens met behulp van SQL-query
Deze query haalt de reizen taxi door tarief, passagiers aantal, en de tip.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

In de volgende code, de `%%local` magic lokale gegevensframe sqlResults maakt. U kunt sqlResults moet worden getekend met behulp van matplotlib gebruiken.

> [!TIP]
> Lokale magic wordt meerdere malen gebruikt in dit artikel. Voorbeeld voor het maken van een gegevensframe dat past in het lokale geheugen als uw gegevensset te groot is, is.
> 
> 

### <a name="plot-the-data"></a>De gegevens uitzetten
U kunt met behulp van Python-code nadat het kader van de gegevens in de lokale context als een Pandas gegevensframe is uitzetten.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 De uitvoer van SQL (HiveQL)-query's visualiseren de kernel Spark automatisch nadat u de code hebt uitgevoerd. U kunt kiezen tussen verschillende soorten visualisaties:

* Tabel
* Cirkel
* Regel
* Onderwerp
* Balk

Hier volgt de code voor het uitzetten van de gegevens:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Uitvoer:**

![Tip bedrag histogram](./media/scala-walkthrough/plot-tip-amount-histogram.png)

![Hoeveelheid op het aantal passagiers Tip](./media/scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tip bedrag door tarief bedrag](./media/scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Functies maken en functies transformeren en vervolgens voorbereiding-gegevens voor de invoer in het modelleren van functies
U moet voorbereiden doel en functies met tal van technieken zoals binning indexeren, één hot codering en vectorization voor functies op basis van een structuur modellering van Spark ML en MLlib. Hier volgen de procedures in deze sectie volgen:

1. Maak een nieuwe functie door **binning** uren in verkeer tijd buckets.
2. Van toepassing **indexeren en één hot codering** categorische functies.
3. **Voorbeeld en splitst de gegevensset** in trainings- en testset breuken.
4. **Geef variabele training en functies**, en vervolgens maken geïndexeerde of op één hot gecodeerd training en testen invoer gelabelde punt flexibele gegevenssets (RDDs) of gegevensframes gedistribueerd.
5. Automatisch **categoriseren en aan functies en doelen vectorize** moet worden gebruikt als invoer voor machine learning-modellen.

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Maakt een nieuwe functie met binning uur in verkeer tijd buckets
Deze code leest u hoe u een nieuwe functie maakt met binning uren in verkeer tijd buckets en in de cache van het resulterende gegevensframe in het geheugen. Waar RDDs en gegevens frames zo vaak worden gebruikt in cache opslaan leidt tot verbeterde uitvoeringstijden. Dienovereenkomstig, zult u RDDs en gegevensframes cache op verschillende fasen uitgevoerd in de volgende procedures.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indexeren en één hot codering van categorische functies
Het model en het voorspellen van de functies van MLlib functies met categorische invoergegevens worden geïndexeerd of gecodeerd vóór gebruik vereist. Deze sectie wordt beschreven hoe u indexeren of categorische functies voor invoer in de functies modellering coderen.

U moet de index of uw modellen coderen op verschillende manieren, afhankelijk van het model. Logistic en lineaire regressie modellen vereisen bijvoorbeeld dat een hot codering. Bijvoorbeeld, kan een onderdeel met drie categorieën worden uitgebreid naar drie kolommen van de functie. 0 of 1, afhankelijk van de categorie van een observatie bevat elke kolom. MLlib biedt de [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) functie voor het coderen van één hot. Een kolom van een label indexen deze encoder toegewezen aan een kolom van de binaire vectoren met maximaal één one-waarde. Met deze codering kunnen algoritmen die verwacht dat de numerieke waarden functies, zoals logistic regression categorische functies worden toegepast.

Hier kunt u slechts vier variabelen om weer te geven voorbeelden die tekenreeksen transformeren. U kunt ook andere variabelen, zoals weekdag, vertegenwoordigd door numerieke waarden als categorische variabelen index.

Gebruik voor het indexeren, `StringIndexer()`, en gebruiken voor het coderen van één hot, `OneHotEncoder()` functies van MLlib. Dit is de code om te indexeren en coderen categorische functies:

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 4 seconden.

### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>De gegevensset in trainings- en testset breuken splitsen en voorbeelden
Deze code maakt een willekeurige steekproef van de gegevens (in dit voorbeeld 25%). Hoewel steekproeven niet vereist voor dit voorbeeld vanwege de grootte van de gegevensset is, het artikel ziet u hoe u kunt een steekproef nemen zodat u hoe u weet gebruiken voor uw eigen problemen wanneer deze nodig is. Als voorbeelden groot zijn, kan dit aanzienlijke tijd besparen tijdens het trainen van modellen. Splits vervolgens het voorbeeld in een training delen (in dit voorbeeld 75%) en een test (in dit voorbeeld 25%) als u wilt gebruiken in de classificatie en regressie modellering.

Een willekeurig getal (tussen 0 en 1) toevoegen aan elke rij (in een kolom 'RNG') die naar kruisvalidatie vouwen selecteren tijdens de training kan worden gebruikt.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 2 seconden.

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Geef variabele training en onderdelen, en maak vervolgens geïndexeerde of op één hot gecodeerd trainings- en testdoeleinden invoer met het label punt RDDs of gegevens frames
Deze sectie bevat de code die u hoe categorische gegevens als een gegevenstype gelabelde punt index en deze coderen ziet, zodat u deze gebruiken kunt om te trainen en te testen MLlib logistic regression en andere classificatie-modellen. Gelabelde point-objecten zijn RDDs die zijn geformatteerd op een manier die is vereist als de invoergegevens voor de meeste machine learning-algoritmen in MLlib. Een [punt gelabeld](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is een lokale vector, dense of sparse, die is gekoppeld aan een label/antwoord.

In deze code geeft u de doelvariabele (afhankelijke) en de functies te gebruiken voor het trainen van modellen. Vervolgens maakt u geïndexeerde of één hot gecodeerd trainings- en testdoeleinden invoer met het label punt RDDs of gegevens frames.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 4 seconden.

### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automatisch categoriseren en aan functies en doelen moet worden gebruikt als invoer voor machine learning-modellen vectorize
Spark ML gebruiken om het doel en -functies gebruiken in de boomstructuur gebaseerde modellering functies te categoriseren. De code is voltooid twee taken:

* Maakt een binaire doel voor classificatie door de waarde 0 of 1 toewijzen aan elk gegevenspunt tussen 0 en 1 met behulp van een drempelwaarde van 0,5.
* Automatisch categoriseren functies. Als het aantal afzonderlijke numerieke waarden voor de functies die kleiner is dan 32 is, wordt deze functie wordt gecategoriseerd.

Dit is de code voor deze twee taken.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Model van de binaire classificatie: voorspellen of een tip moet worden betaald
In deze sectie maakt maken u drie soorten binaire classificatie modellen te voorspellen of een tip moet worden betaald:

* Een **logistic regressiemodel** met behulp van de Spark ML `LogisticRegression()` functie
* Een **classificatie van willekeurige forestmodel** met behulp van de Spark ML `RandomForestClassifier()` functie
* Een **kleurovergang prestatieverbetering classificatie boomstructuur** met behulp van de MLlib `GradientBoostedTrees()` functie

### <a name="create-a-logistic-regression-model"></a>Een logistic regressiemodel maken
Maak vervolgens een logistic regressiemodel met behulp van de Spark ML `LogisticRegression()` functie. Maken van het model bouwen code in een reeks stappen:

1. **Het model trainen** gegevens met één parameter is ingesteld.
2. **Het model evalueren** op een test-gegevensset met metrische gegevens.
3. **Sla het model** in Blob storage voor toekomstig gebruik.
4. **Het model beoordelen** tegen testgegevens.
5. **Tekenen van de resultaten** met ontvanger kenmerk (ROC) curven besturingssysteem.

Hier volgt de code voor deze procedures:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Laden, beoordelen en de resultaten worden opgeslagen.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Uitvoer:**

ROC op testgegevens 0.9827381497557599 =

Gebruik Python op lokale Pandas gegevensframes waarmee de ROC-curve wordt uitgezet.

    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
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


**Uitvoer:**

![Tip of geen tip ROC-curve](./media/scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a>Maken van een willekeurige indeling forestmodel
Maak vervolgens een willekeurige forestmodel classificatie met behulp van de Spark ML `RandomForestClassifier()` functioneren en vervolgens het model op de testgegevens evalueren.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Uitvoer:**

ROC op testgegevens 0.9847103571552683 =

### <a name="create-a-gbt-classification-model"></a>Een classificatie GBT model maken
Maak vervolgens een classificatie GBT model met behulp van MLlib `GradientBoostedTrees()` functioneren en vervolgens het model op de testgegevens evalueren.

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Uitvoer:**

Gebied onder ROC-curve: 0.9846895479241554

## <a name="regression-model-predict-tip-amount"></a>Regressiemodel: tip bedrag voorspellen
In deze sectie maakt maken u twee soorten regressie modellen te voorspellen, de hoeveelheid tip:

* Een **overgegaan lineair regressiemodel** met behulp van de Spark ML `LinearRegression()` functie. U Sla het model en het model op de testgegevens evalueren.
* Een **structuur regressiemodel verloop versterking** met behulp van de Spark ML `GBTRegressor()` functie.

### <a name="create-a-regularized-linear-regression-model"></a>Een overgegaan lineair regressiemodel maken
    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 13 seconden.

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Uitvoer:**

R-sqr op testgegevens 0.5960320470835743 =

Vervolgens de testresultaten niet opvragen als een gegevensframe en AutoVizWidget en matplotlib gebruiken voor het visualiseren van deze.

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

De code maakt een frame lokale gegevens van de query-uitvoer en de gegevens zijn getekend. De `%%local` magic maakt een frame lokale gegevens `sqlResults`, dat u moet worden getekend met matplotlib kunt gebruiken.

> [!NOTE]
> Deze Spark-magic wordt meerdere malen gebruikt in dit artikel. Als de hoeveelheid gegevens te groot is, moet u een steekproef nemen voor het maken van een gegevensframe dat past in het lokale geheugen.
> 
> 

Waarnemingspunten maken met behulp van Python matplotlib.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Uitvoer:**

![Tip bedrag: werkelijk vs. voorspelde](./media/scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a>Een regressiemodel GBT maken
Een regressiemodel GBT maken met behulp van de Spark ML `GBTRegressor()` functioneren en vervolgens het model op de testgegevens evalueren.

[Structuren verloop boosted](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) ensembles van beslissingsstructuren zijn. GBTs training beslissingsstructuren iteratief aan een functie gegevensverlies te minimaliseren. U kunt GBTs gebruiken voor regressie en classificatie. Ze categorische functies kunnen verwerken, hoeven niet functie schalen en nonlinearities en functie interacties kunnen vastleggen. Ook kunt u ze in een setting multiklasse classificatie.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Uitvoer:**

Test R-sqr is: 0.7655383534596654

## <a name="advanced-modeling-utilities-for-optimization"></a>Hulpprogramma's voor geavanceerde modellering voor optimalisatie
In deze sectie gebruikt u de machine learning-hulpprogramma's die ontwikkelaars vaak voor de optimalisatie van het model gebruikt. U kunt in het bijzonder machine learning-modellen drie verschillende manieren optimaliseren met behulp van de parameter sweeping en kruisvalidatie:

* De gegevens splitsen in train en validatie sets, het model met behulp van hyper-parameter sweeping op een trainingset te optimaliseren en evalueren in een set validatie (lineaire regressie)
* Het model optimaliseren door kruisvalidatie en hyper-parameter verstrekkende met behulp van Spark ML CrossValidator functie (binaire classificatie)
* Het model optimaliseren door aangepaste kruisvalidatie en parameter sweeping code voor het gebruik van een machine learning-functie en de parameter set (lineaire regressie)

**Kruisvalidatie** is een techniek die evalueert hoe goed een op een bekende set gegevens getraind model wordt generalize om te voorspellen van de functies van gegevenssets waarop deze is niet getraind. Het algemeen idee achter deze techniek is dat een model wordt getraind van een gegevensset van bekende gegevens, en vervolgens de nauwkeurigheid van de voorspellingen op basis van een onafhankelijke set van gegevens wordt getest. Een veelvoorkomende implementatie is om te delen van een gegevensset in *k*-vouwen en vervolgens het model round-robin toewijst op alle maar een van de vouwen trainen.

**Hyper-parameter optimalisatie** is het probleem van het kiezen van een set van hyper-parameters voor een leeralgoritme meestal met het doel van een meting van de algoritme op de prestaties van een onafhankelijke set van gegevens te optimaliseren. Een hyper-parameter is een waarde die u buiten de training model procedure moet opgeven. Veronderstellingen over hyper-parameterwaarden kunnen van invloed op de flexibiliteit en de nauwkeurigheid van het model. Beslissingsstructuren hyper-parameters hebben, bijvoorbeeld, zoals de gewenste omvang en aantal bladeren in de boomstructuur. U moet een misclassification sancties term voor een vectormachine ondersteuning (SVM) instellen.

Een veelgebruikte manier voor het uitvoeren van de optimalisatie van de hyper-parameter is het gebruik van een zoekopdracht raster, ook wel een **parameter vegen**. In een zoekopdracht raster wordt een intensieve zoekactie uitgevoerd via de waarden van een opgegeven subset van de ruimte van de hyper-parameter voor een leeralgoritme. Kruisvalidatie kan voorzien van een metriek prestaties te sorteren van de optimale resultaten geproduceerd door de algoritme van de zoekopdracht raster. Als u kruisvalidatie hyper parameter sweeping gebruikt, kunt u problemen zoals het model trainingsgegevens overfitting limiet. Deze manier kan het model behoudt de capaciteit om te passen op de algemene set gegevens weer waaruit de trainingsgegevens is opgehaald.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Een lineair regressiemodel met hyper-parameter sweeping optimaliseren
Vervolgens gegevens splitsen in train en validatie sets, gebruik hyper-parameter verstrekkende in een trainingset voor het model te optimaliseren en evalueren in een set validatie (lineaire regressie).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Uitvoer:**

Test R-sqr is: 0.6226484708501209

### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Het model van de binaire indeling met behulp van kruisvalidatie en hyper-parameter sweeping optimaliseren
Deze sectie wordt beschreven hoe u een model binaire classificatie optimaliseren door sweeping kruisvalidatie en hyper-parameter. Dit maakt gebruik van de Spark ML `CrossValidator` functie.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 33 seconden.

### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>De lineair regressiemodel met behulp van aangepaste kruisvalidatie en parameter sweeping code optimaliseren
Vervolgens het model met behulp van aangepaste code te optimaliseren en de beste Modelparameters identificeren met behulp van het criterium van hoogste nauwkeurig. Vervolgens maakt u het laatste model, het model op de testgegevens evalueren en het model niet opslaan in Blob-opslag. Ten slotte het model niet laden, testgegevens beoordelen en evalueren nauwkeurig.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Uitvoer:**

Tijd voor het uitvoeren van de cel: 61 seconden.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Spark is gebouwd machine learning-modellen automatisch met Scala gebruiken
Zie voor een overzicht van onderwerpen die helpt u stapsgewijs door de taken die het proces voor Gegevenswetenschap in Azure omvatten, [Team gegevens wetenschap proces](http://aka.ms/datascienceprocess).

[Gegevens wetenschap proces scenario's in een team](walkthroughs.md) beschrijft andere end-to-end-scenario's die de stappen in de procedure van wetenschappelijke gegevens Team voor specifieke scenario's laten zien. De scenario's ook te laten zien hoe cloud en on-premises hulpprogramma's en services combineren in een werkstroom of pijplijn een intelligente toepassing maken.

[Score Spark is gebouwd machine learning-modellen](spark-model-consumption.md) ziet u hoe Scala code gebruiken om automatisch te laden en beoordelen van nieuwe gegevens sets met machine learning-modellen in Spark is gebouwd en opgeslagen in Azure Blob-opslag. U kunt de instructies volgen en gewoon de Python-code te vervangen door Scala code in dit artikel voor geautomatiseerde verbruik.

