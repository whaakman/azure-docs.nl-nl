---
title: Voorbeeld van een machine learning met Spark MLlib op HDInsight - Azure
description: Informatie over het gebruik van Spark MLlib te maken van een machine learning-app die met een gegevensset met behulp van classificatie via logistieke regressie worden geanalyseerd.
keywords: machine learning in Spark, spark machine learning voorbeeld
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: jasonh
ms.openlocfilehash: 068d5ee7200f9597da11c0a2850ef4941f4ea900
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619529"
---
# <a name="use-spark-mllib-to-build-a-machine-learning-application-and-analyze-a-dataset"></a>Gebruik Spark MLlib een machine learning-toepassing bouwen en analyseren van een gegevensset

Informatie over het gebruik van Spark [MLlib](https://spark.apache.org/mllib/) te maken van een machine learning-toepassing eenvoudig voorspellende analyse uitvoeren op een open-gegevensset. Van Spark van ingebouwde machine learning-bibliotheken, in dit voorbeeld wordt *classificatie* via logistieke regressie. 

> [!TIP]
> In dit voorbeeld is ook beschikbaar als een Jupyter-notebook in een Spark (Linux)-cluster die u in HDInsight maakt. De notebookervaring kunt u de Python-codefragmenten uitvoeren vanuit het notitieblok zelf. Volg de zelfstudie uit binnen een laptop, een Spark-cluster maken en starten van een Jupyter-notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`). Vervolgens voert u de notebook **Spark Machine Learning - voorspellende analyse van voedsel inspectie gegevens met behulp van MLlib.ipynb** onder de **Python** map.
>
>

MLlib is een core Spark-bibliotheek met vele nuttige hulpprogramma's voor machine learning-taken, waaronder hulpprogramma's die geschikt zijn voor:

* Classificatie
* Regressie
* Clustering
* Modellering van onderwerp
* Enkelvoudige waarde ontleding (SVD) en -analyse van de principal-onderdeel (Pso)
* Hypothese testen en berekenen van de voorbeeld-statistieken

## <a name="understand-classification-and-logistic-regression"></a>Classificatie en logistieke regressie begrijpen
*Classificatie*, een populaire machine learning-taak, is het proces voor het sorteren van invoergegevens in categorieën. Het is de taak van een classificatiealgoritme om te achterhalen toewijzen 'labels' voor het invoeren van gegevens die u opgeeft. Bijvoorbeeld, u een machine learning-algoritme dat aandeel gegevens als invoer accepteert en het aandeel verdeeld in twee categorieën kan zien: bestanden die u moet verkopen en aandelen worden weergegeven die u moet bewaren.

Logistieke regressie is de algoritme die u voor classificatie gebruikt. De Spark logistieke regressie API is handig voor *binaire classificatie*, of de ingevoerde gegevens classificeren in een van twee groepen. Zie voor meer informatie over logistieke regressie, [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Kortom, het proces voor logistieke regressie produceert een *logistieke functie* die kunnen worden gebruikt om te voorspellen van de kans dat een invoer vector deel van een groep of de andere uitmaakt.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Voorbeeld van de voorspellende analyse op food inspectie gegevens
In dit voorbeeld gebruikt u Spark voor sommige voorspellende analyses uitvoeren op gegevens van voedsel inspectie (**Food_Inspections1.csv**) die is verkregen via de [Den Haag gegevensportal](https://data.cityofchicago.org/). Deze gegevensset bevat informatie over voedingsmiddelen inrichting controles die zijn uitgevoerd in Chicago, met inbegrip van informatie over elke inrichting, de schendingen gevonden (indien aanwezig) en de resultaten van de controle. Het CSV-bestand is al beschikbaar in het opslagaccount dat is gekoppeld aan het cluster op **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

In de onderstaande stappen kunt u een model om te zien wat die nodig is voor het doorgeven of mislukken van een controle food ontwikkelen.

## <a name="create-a-spark-mllib-machine-learning-app"></a>Een MLlib Spark machine learning-app maken

1. Maak een Jupyter-notebook met behulp van de PySpark-kernel. Zie [Een Jupyter-notebook maken](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook) voor de instructies.

2. Importeer de typen die zijn vereist voor deze toepassing. Kopieer en plak de volgende code in een lege cel en druk vervolgens op **SHIRT + ENTER**.

    ```PySpark
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    ```
    Vanwege de PySpark-kernel hoeft u niet expliciet contexten maken. De Spark- en Hive-contexten worden automatisch voor u gemaakt wanneer u de eerste codecel uitvoert. 

## <a name="construct-the-input-dataframe"></a>Om de invoer dataframe voor te bereiden

Omdat de onbewerkte gegevens in een CSV-indeling, kunt u de Spark-context gebruiken voor het ophalen van het bestand in het geheugen als ongestructureerde tekst en vervolgens de Python CSV-clientbibliotheek gebruiken om u te parseren van elke regel van de gegevens.

1. Voer de volgende regels voor het maken van een robuuste gedistribueerd gegevensset (RDD) door te importeren en bij het parseren van de ingevoerde gegevens.

    ```PySpark
    def csvParse(s):
        import csv
        from StringIO import StringIO
        sio = StringIO(s)
        value = csv.reader(sio).next()
        sio.close()
        return value
    
    inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                    .map(csvParse)
    ```

2. Voer de volgende code om op te halen van één rij van de RDD, zodat u kunt een overzicht van het schema:

    ```PySpark
    inspections.take(1)
    ```

    De uitvoer is:

    ```
    [['413707',
        'LUNA PARK INC',
        'LUNA PARK  DAY CARE',
        '2049789',
        "Children's Services Facility",
        'Risk 1 (High)',
        '3250 W FOSTER AVE ',
        'CHICAGO',
        'IL',
        '60625',
        '09/21/2010',
        'License-Task Force',
        'Fail',
        '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
        '41.97583445690982',
        '-87.7107455232781',
        '(41.97583445690982, -87.7107455232781)']]
    ```

    De uitvoer geeft u een idee van het schema van het invoerbestand. Het bevat de naam van elke inrichting, het type van de inrichting, het adres, de gegevens van de controles en de locatie, onder andere. 

3. Voer de volgende code voor het maken van een dataframe (*df*) en een tijdelijke tabel (*CountResults*) met een paar kolommen die nuttig voor de voorspellende analyse zijn. `sqlContext` wordt gebruikt voor het uitvoeren van transformaties voor gestructureerde gegevens. 

    ```PySpark
    schema = StructType([
    StructField("id", IntegerType(), False),
    StructField("name", StringType(), False),
    StructField("results", StringType(), False),
    StructField("violations", StringType(), True)])
    
    df = spark.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
    df.registerTempTable('CountResults')
    ```

    De vier kolommen in het dataframe van belang zijn **id**, **naam**, **resultaten**, en **schendingen**.

4. Voer de volgende code om op te halen van een voorbeeld van de gegevens:

    ```PySpark
    df.show(5)
    ```

    De uitvoer is:

    ```
    +------+--------------------+-------+--------------------+
    |    id|                name|results|          violations|
    +------+--------------------+-------+--------------------+
    |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
    |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
    |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
    |413708|BENCHMARK HOSPITA...|   Pass|                    |
    |413722|           JJ BURGER|   Pass|                    |
    +------+--------------------+-------+--------------------+
    ```

## <a name="understand-the-data"></a>Informatie over de gegevens

Laten we beginnen aan een idee van wat de gegevensset bevat. 

1. Voer de volgende code om weer te geven van de afzonderlijke waarden in de **resultaten** kolom:

    ```PySpark
    df.select('results').distinct().show()
    ```

    De uitvoer is:

    ```
    +--------------------+
    |             results|
    +--------------------+
    |                Fail|
    |Business Not Located|
    |                Pass|
    |  Pass w/ Conditions|
    |     Out of Business|
    +--------------------+
    ```

2. Voer de volgende code voor het visualiseren van de distributie van deze resultaten:

    ```PySpark
    %%sql -o countResultsdf
    SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results
    ```

    De `%%sql` magic gevolgd door `-o countResultsdf` zorgt ervoor dat de uitvoer van de query lokaal worden bewaard op de Jupyter-server (meestal het hoofdknooppunt van het cluster). De uitvoer wordt opgeslagen als een [Pandas](http://pandas.pydata.org/) gegevensframe met de opgegeven naam **countResultsdf**. Voor meer informatie over de `%%sql`-magic, en andere magics die voor de PySpark-kernel beschikbaar zijn, raadpleegt u [Beschikbare kernels op Jupyter-notebooks met HDInsight Spark-clusters](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

    De uitvoer is:

    ![SQL-query-uitvoer](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL query-uitvoer")


3. U kunt ook [Matplotlib](https://en.wikipedia.org/wiki/Matplotlib), een bibliotheek gebruikt voor het maken van de visualisatie van gegevens om het maken van een diagram. Omdat de grafiek moet worden gemaakt van de lokaal persistente **countResultsdf** dataframe, het codefragment moet beginnen met de `%%local` Magic-pakket. Dit zorgt ervoor dat de code lokaal op de Jupyter-server wordt uitgevoerd.

    ```PySpark
    %%local
    %matplotlib inline
    import matplotlib.pyplot as plt

    labels = countResultsdf['results']
    sizes = countResultsdf['cnt']
    colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
    plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
    plt.axis('equal')
    ```

    De uitvoer is:

    ![Spark machine learning-toepassing-uitvoer - cirkeldiagram met vijf verschillende resultaten](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning-resultaat uitvoer")

    Er zijn 5 verschillende resultaten genereren die een controle kan hebben:

    - Bedrijven niet vinden
    - Mislukt
    - Geslaagd
    - Doorgeven met voorwaarden
    - Buiten het bedrijf

    Om te voorspellen een resultaat van de inspectie food, moet u een model op basis van de schendingen te ontwikkelen. Omdat logistieke regressie een binaire classificatie-methode is, het zinvol zijn het om resultaatgegevens te groeperen in twee categorieën: **mislukken** en **doorgeven**:

    - Geslaagd
        - Geslaagd
        - Doorgeven met voorwaarden
    - Mislukt
        - Mislukt
    - Verwijderen
        - Bedrijven niet vinden
        - Buiten het bedrijf

    Gegevens met de andere resultaten ("Bedrijf niet vinden" of ' Out-of Business') zijn niet nuttig en ze een zeer klein percentage van de resultaten toch gezamenlijk.

4. Voer de volgende code om te converteren van de bestaande dataframe (`df`) in een nieuwe dataframe waar elke inspectie wordt weergegeven als een combinatie van schendingen van label. In dit geval een label van `0.0` vertegenwoordigt een storing optreedt, een label van `1.0` vertegenwoordigt een geslaagde pogingen en een label van `-1.0` vertegenwoordigt bepaalde resultaten naast deze twee. 

    ```PySpark
    def labelForResults(s):
        if s == 'Fail':
            return 0.0
        elif s == 'Pass w/ Conditions' or s == 'Pass':
            return 1.0
        else:
            return -1.0
    label = UserDefinedFunction(labelForResults, DoubleType())
    labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')
    ```

5. Voer de volgende code om een rij van de gelabelde gegevens weer te geven:

    ```PySpark
    labeledData.take(1)
    ```

    De uitvoer is:

    ```
    [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]
    ```

## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Een model voor logistieke regressie uit het dataframe dat invoer maken

De laatste taak is de gelabelde gegevens te converteren naar een indeling die door logistieke regressie kan worden geanalyseerd. De invoer naar een algoritme voor logistieke regressie moet bestaan uit een reeks *label-functie vector paren*, waarbij de "functie vector" een vector van getallen die het punt invoer. Daarom moet u de kolom 'schendingen', dat is semi-gestructureerde en bevat veel opmerkingen in vrije tekst, naar een matrix van de reële getallen die een virtuele machine kan gemakkelijk begrijpen converteren.

Een standard machine learning-benadering voor de verwerking van natuurlijke taal is het toewijzen van elk woord distinct een 'index', en klikt u vervolgens een vector doorgeven aan de machine learning-algoritme dat de waarde van elke index de relatieve frequentie van dat woord in de tekenreeks met tekst bevat.

MLlib biedt een eenvoudige manier om deze bewerking niet uitvoeren. Eerst 'basisvormen"elke tekenreeks schendingen voor de afzonderlijke woorden in elke tekenreeks. Vervolgens gebruikt u een `HashingTF` elke set van tokens converteren naar een functie-vector die vervolgens kan worden doorgegeven aan het algoritme voor logistieke regressie te maken van een model. U uitvoeren al deze stappen in de reeks met behulp van een "pipeline".

```PySpark
tokenizer = Tokenizer(inputCol="violations", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(labeledData)
```

## <a name="evaluate-the-model-using-another-dataset"></a>Het model met behulp van een andere gegevensset evalueren

U kunt het model dat u eerder naar gemaakt *voorspellen* wat de resultaten van de nieuwe inspecties zal zijn, op basis van de schendingen die zijn waargenomen. U dit model op de gegevensset getraind **Food_Inspections1.csv**. U kunt een tweede gegevensset **Food_Inspections2.csv**naar *evalueren* de kracht van dit model op de nieuwe gegevens. Deze tweede gegevensset (**Food_Inspections2.csv**) is in de standaardcontainer van de opslag die is gekoppeld aan het cluster.

1. Voer de volgende code voor het maken van een nieuwe dataframe **predictionsDf** die de voorspelling die worden gegenereerd door het model bevat. Het fragment maakt ook een tijdelijke tabel genaamd **voorspellingen** op basis van het gegevensframe.

    ```PySpark
    testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                .map(csvParse) \
                .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
    testDf = spark.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
    predictionsDf = model.transform(testDf)
    predictionsDf.registerTempTable('Predictions')
    predictionsDf.columns
    ```

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    ```
    # -----------------
    # THIS IS AN OUTPUT
    # -----------------

    ['id',
        'name',
        'results',
        'violations',
        'words',
        'features',
        'rawPrediction',
        'probability',
        'prediction']
    ```

1. Bekijk een van de voorspellingen. Voer dit fragment:

    ```PySpark
    predictionsDf.take(1)
    ```

   Er is een voorspelling voor het eerste item in de test-gegevensset.
1. De `model.transform()` methode van de transformatie van dezelfde toepassing op nieuwe gegevens die met hetzelfde schema en een voorspelling van het classificeren van de gegevens aankomen. Enkele eenvoudige statistische gegevens om een idee van hoe nauwkeurig de voorspellingen zijn, kunt u doen:

    ```PySpark
    numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                            (prediction = 1 AND (results = 'Pass' OR
                                                                results = 'Pass w/ Conditions'))""").count()
    numInspections = predictionsDf.count()

    print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
    print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"
    ```

    De uitvoer ziet er als volgt uit:

    ```
    # -----------------
    # THIS IS AN OUTPUT
    # -----------------

    There were 9315 inspections and there were 8087 successful predictions
    This is a 86.8169618894% success rate
    ```

    Logistieke regressie gebruiken met Spark, biedt u een nauwkeurige model van de relatie tussen schendingen beschrijvingen in het Engels en of een opgegeven bedrijf zou doorgeven of een inspectie food mislukken.

## <a name="create-a-visual-representation-of-the-prediction"></a>Maken van een visuele weergave van de voorspelling
U kunt nu een definitieve visualisatie te maken u over de resultaten van deze test reden.

1. U start door de verschillende voorspellingen en de resultaten te extraheren uit de **voorspellingen** tijdelijke tabel die eerder hebt gemaakt. De volgende query's uit de uitvoer als afzonderlijke *true_positive*, *false_positive*, *true_negative*, en *false_negative*. In de onderstaande query's u uitschakelen visualisatie met behulp van `-q` en de uitvoer ook opslaan (met behulp van `-o`) als dataframes die vervolgens kunnen worden gebruikt met de `%%local` Magic-pakket.

    ```PySpark
    %%sql -q -o true_positive
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'
    ```

    ```PySpark
    %%sql -q -o false_positive
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
    ```

    ```PySpark
    %%sql -q -o true_negative
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'
    ```

    ```PySpark
    %%sql -q -o false_negative
    SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
    ```

1. Ten slotte het volgende codefragment gebruiken voor het genereren van het diagram met behulp van **Matplotlib**.

    ```PySpark
    %%local
    %matplotlib inline
    import matplotlib.pyplot as plt

    labels = ['True positive', 'False positive', 'True negative', 'False negative']
    sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
    colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
    plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
    plt.axis('equal')
    ```

    Hier ziet u de volgende uitvoer:

    ![Spark machine learning-uitvoer van de toepassing - cirkeldiagram percentages van mislukte food controles. ] (./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning-resultaat uitvoer")

    In deze grafiek verwijst "" positief naar de mislukte food inspectie, terwijl een negatief resultaat naar een doorgegeven controle verwijst.

## <a name="shut-down-the-notebook"></a>De notebook afsluiten
Wanneer u klaar bent met het uitvoeren van de toepassing, moet u het notitieblok om de resources vrij te geven afsluiten. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**. Dit wordt afgesloten en sluit u de notebook.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
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
