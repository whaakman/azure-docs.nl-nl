---
title: Voorbeeld van de machine learning met MLlib Spark in HDInsight - Azure | Microsoft Docs
description: Informatie over het Spark MLlib gebruiken voor het maken van een machine learning-app die een gegevensset met behulp van classificatie via logistic regression analyseert.
keywords: Spark machine learning spark machine learning-voorbeeld
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: jgao
ms.openlocfilehash: 864d34306dad2915a15b032a27600cefdc632bb9
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="use-spark-mllib-to-build-a-machine-learning-application-and-analyze-a-dataset"></a>Spark MLlib gebruiken voor het bouwen van een machine learning-toepassing en analyseren van een gegevensset

Informatie over het gebruik van Spark **MLlib** voor het maken van een machine learning-toepassing te doen eenvoudige predictive Analytics op een open gegevensset. Van Spark van ingebouwde machine learning-bibliotheken, in dit voorbeeld wordt *classificatie* via logistic regression. 

> [!TIP]
> In dit voorbeeld is ook beschikbaar als een Jupyter-notebook op een cluster Spark (Linux) die u in HDInsight maakt. De ervaring voor de notebook kunt u de Python-codefragmenten uitvoeren vanaf de notebook zelf. Wilt u de zelfstudie uit binnen een laptop, een Spark-cluster maken en starten van een Jupyter-notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`). Voer vervolgens de notebook **Spark Machine Learning - Predictive Analytics op voeding inspectie gegevens met behulp van MLlib.ipynb** onder de **Python** map.
>
>

MLlib is een core Spark-bibliotheek met veel nuttige hulpprogramma's voor machine learning taken, waaronder hulpprogramma's die geschikt zijn voor:

* Classificatie
* Regressie
* Clustering
* Onderwerp-model
* Enkelvoudige waarde afbreken (SVD) en analyse van de principal-onderdeel (Pso)
* Hypothese testen en het berekenen van de voorbeeld-statistieken

## <a name="what-are-classification-and-logistic-regression"></a>Wat zijn de classificatie en logistic regression?
*Classificatie*, een populaire machine learning-taak is het proces voor het sorteren van invoergegevens in categorieën. Het is de taak van een classificatie-algoritme om te achterhalen toewijzen 'labels' voor het invoeren van gegevens die u opgeeft. Bijvoorbeeld, u een machine learning-algoritme dat vooraf gedefinieerde gegevens als invoer accepteert en wordt de stock verdeeld in twee categorieën kan zien: bestanden die u moet verkoopt en bestanden die u dient te houden.

Logistic regression is de algoritme die u voor classificatie gebruikt. De Spark logistic regression API is nuttig voor *binaire classificatie*, of de ingevoerde gegevens classificeren in een van twee groepen. Zie voor meer informatie over logistic regressies [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Kortom, het proces van het logistic regression produceert een *logistic functie* kunnen worden gebruikt voor het voorspellen van de kans dat een invoer-vector in één groep of de andere behoort.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Predictive Analytics-voorbeeld op voeding inspectie gegevens
In dit voorbeeld gebruikt u Spark sommige voorspellende analyses uitvoeren op voeding inspectie gegevens (**Food_Inspections1.csv**) die is verkregen via de [Den Haag gegevensportal](https://data.cityofchicago.org/). Deze gegevensset bevat informatie over voeding inrichting controles zijn uitgevoerd in Chicago, inclusief informatie over elke inrichting, de schendingen gevonden (indien aanwezig) en de resultaten van de controle. Het CSV-bestand is al beschikbaar in het opslagaccount dat is gekoppeld aan het cluster **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

In de onderstaande stappen kunt u een model om te zien wat er nodig is om te geven of mislukken van een voeding inspectie ontwikkelen.

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a>Beginnen met het samenstellen van een Spark-MMLib machine learning-app
1. Klik vanaf het Startboard in [Azure Portal](https://portal.azure.com/) op de tegel voor uw Spark-cluster (als u deze aan het Startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.   
1. Klik vanuit de blade Spark-cluster op **Cluster-dashboard** en vervolgens op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   > [!NOTE]
   > Mogelijk bereikt u de Jupyter-notebook voor uw cluster ook door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. Maak een notebook. Klik op **Nieuw** en klik vervolgens op **PySpark**.

    ![Maken van een Jupyter-notebook](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "een nieuwe Jupyter-notebook maken")
1. Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled.pynb. Klik bovenaan op de naam van de notebook en wijzig deze in een beschrijvende naam.

    ![Een naam opgeven voor de notebook](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Een naam opgeven voor de notebook")
1. Omdat u de notebook met behulp van de PySpark-kernel hebt gemaakt, hoeft u niet expliciet contexten te maken. De Spark- en Hive-contexten worden automatisch voor u gemaakt wanneer u de eerste codecel uitvoert. U kunt beginnen met het bouwen van uw machine learning-toepassingen door het importeren van de typen die nodig zijn voor dit scenario. Om dit te doen, plaatst u de cursor in de cel en druk op **SHIFT + ENTER**.

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Maken van een invoer dataframe
We kunnen gebruiken `sqlContext` transformaties uitvoeren op gestructureerde gegevens. De eerste taak is de voorbeeldgegevens laden ((**Food_Inspections1.csv**)) in een Spark SQL *dataframe*.

1. Omdat de onbewerkte gegevens zich in een CSV-indeling, moeten we de Spark-context gebruiken voor het ophalen van elke regel van het bestand in het geheugen als niet-gestructureerde tekst; vervolgens gebruikt u de CSV-bibliotheek van Python elke regel afzonderlijk parseren.

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. We nu hebben het CSV-bestand als een RDD.  Om het schema van de gegevens te begrijpen, ophalen we één rij uit de RDD.

        inspections.take(1)

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

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
1. De voorgaande uitvoer geeft ons een idee van het schema van het invoerbestand. Het bevat de naam van elke instelling, het type van de inrichting, het adres, de gegevens van de controles en de locatie, onder andere. Laten we enkele kolommen selecteren die zijn handig voor onze predictive Analytics en de resultaten groeperen als een dataframe dat we vervolgens gebruiken om een tijdelijke tabel te maken.

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. Nu een *dataframe*, `df` waarop we onze analyse kunt uitvoeren. We hebben ook een aanroep van de tijdelijke tabel **CountResults**. Vindt u vier kolommen in de dataframe van belang: **id**, **naam**, **resultaten**, en **schendingen**.

    We gaan een kort voorbeeld van de gegevens ophalen:

        df.show(5)

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>De gegevens begrijpen
1. Laten we beginnen met een idee van wat onze gegevensset bevat. Bijvoorbeeld, wat zijn de verschillende waarden in de **resultaten** kolom?

        df.select('results').distinct().show()

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. Een snelle visualisatie kan we beter reden over de distributie van deze resultaten. We hebben al de gegevens in een tijdelijke tabel **CountResults**. U kunt de volgende SQL-query uitvoeren op de tabel voor een beter inzicht te krijgen van hoe de resultaten worden gedistribueerd.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    De `%%sql` magic gevolgd door `-o countResultsdf` zorgt ervoor dat de uitvoer van de query lokaal worden bewaard op de Jupyter-server (meestal de headnode van het cluster). De uitvoer is doorgevoerd als een [Pandas](http://pandas.pydata.org/) dataframe met de opgegeven naam **countResultsdf**.

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    ![Uitvoer van de SQL-query](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "uitvoer van de SQL-query")

    Voor meer informatie over de `%%sql`-magic, en andere magics die voor de PySpark-kernel beschikbaar zijn, raadpleegt u [Beschikbare kernels op Jupyter-notebooks met HDInsight Spark-clusters](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
1. U kunt ook Matplotlib, een bibliotheek gebruikt voor het opstellen van visualisatie van gegevens, tekent maken. Omdat de grafiek moet worden gemaakt van de lokaal persistente **countResultsdf** dataframe, het codefragment moet beginnen met de `%%local` verwerkt Magic-pakket. Dit zorgt ervoor dat de code op de Jupyter-server lokaal wordt uitgevoerd.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    ![Spark machine learning-toepassing output - cirkeldiagram met vijf verschillende controleresultaten](./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning-resultaat-uitvoer")
1. U kunt zien dat er 5 verschillende resultaten, waarvoor een inspectie kan zijn:

   * Bedrijven niet vinden
   * Mislukt
   * Doorgeven
   * PSS met voorwaarden
   * Buiten bedrijf

     Laat het ons ontwikkel een model waarmee de uitkomst van een inspectie voeding kan raden schendingen van het gegeven. Aangezien logistic regression een classificatiemethode binaire is, is het verstandig om onze gegevens in twee categorieën: **mislukken** en **doorgeven**. Een 'doorgeven met voorwaarden' nog steeds een op te geven, zodat wanneer we het model trainen, we beide resultaten als gelijkwaardig beschouwt. Gegevens met de andere resultaten ('Bedrijven niet vinden' of 'Out of Business') zijn niet handig zodat we uit onze trainingset verwijderen. Dit mag geen probleem omdat deze twee categorieën gezamenlijk een kleine hoeveelheid van de resultaten toch.
1. Laat het ons opwekken en onze bestaande dataframe converteren (`df`) in een nieuwe dataframe waar elke inspectie wordt weergegeven als een combinatie van schendingen van het label. In ons geval een label van `0.0` vertegenwoordigt een fout optreedt, wordt een label van `1.0` vertegenwoordigt een is voltooid en een label van `-1.0` bepaalde resultaten naast deze twee vertegenwoordigt. We uitfilteren die andere resultaten bij het berekenen van de nieuwe gegevensframe.

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    Als u wilt zien welke lijkt gelabelde gegevens erop, gaan we u één rij ophalen.

        labeledData.take(1)

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]

## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Een logistic regressiemodel maken van de invoer dataframe
De laatste taak is de gelabelde gegevens te converteren naar een indeling die kan worden geanalyseerd door logistic regression. De invoer voor een logistic regression-algoritme moet een reeks *label-functie vector paren*, waarbij 'functie vector' een vector van getallen die de invoer punt. Dus moeten we de kolom 'schendingen', die wordt semi-gestructureerde en bevat veel opmerkingen in vrije tekst, naar een matrix van real-getallen die met een machine gemakkelijk kan begrijpen converteren.

Een standaard machine learning-benadering voor het verwerken van natuurlijke taal is een 'index' van elk woord distinct toewijzen en vervolgens een vector doorgeven aan de machine learning-algoritme dat de waarde voor elke index de relatieve frequentie van dat woord in de tekenreeks bevat.

MLlib biedt een eenvoudige manier om deze bewerking niet uitvoeren. Eerst 'basisvormen' elke tekenreeks schendingen om op te halen van de afzonderlijke woorden in elke tekenreeks. Vervolgens gebruikt u een `HashingTF` elke set van tokens omzetten in een functie-vector die vervolgens kan worden doorgegeven aan de logistic regression-algoritme om te maken van een model. We uitvoeren alle deze stappen in de reeks met behulp van een "pipeline".

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Evalueren van het model op een afzonderlijke testgegevensset
Gebruiken we het model dat we eerder tot gemaakt *voorspellen* wat de resultaten van nieuwe controles worden, op basis van de schendingen die zijn waargenomen. We dit model op de gegevensset getraind **Food_Inspections1.csv**. Laat het ons gebruik van een tweede gegevensset **Food_Inspections2.csv**, naar *evalueren* de sterkte van dit model op nieuwe gegevens. Deze tweede gegevensverzameling (**Food_Inspections2.csv**) moet al zijn in de standaard storage-container die is gekoppeld aan het cluster.

1. Het volgende codefragment maakt u een nieuwe dataframe **predictionsDf** die de voorspelling gegenereerd door het model bevat. Het codefragment maakt ook een tijdelijke tabel genaamd **voorspellingen** op basis van de dataframe.

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

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
1. Bekijk een van de voorspellingen. In dit fragment uitvoeren:

        predictionsDf.take(1)

   Er is een voorspelling voor het eerste item in de testgegevensset.
1. De `model.transform()` methode past de dezelfde transformatie uit op nieuwe gegevens die met hetzelfde schema en een voorspelling van het classificeren van de gegevens aankomen. We kunt enkele eenvoudige statistieken om een beeld van hoe nauwkeurig zijn van onze voorspellingen doen:

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    De uitvoer ziet er als volgt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    Met behulp van logistic regression met Spark biedt ons een nauwkeurige model van de relatie tussen schendingen beschrijvingen in het Engels en een opgegeven bedrijf zou doorgeven of afbreken van een voeding inspectie.

## <a name="create-a-visual-representation-of-the-prediction"></a>Een visuele representatie van de voorspelling te maken
We kunnen een definitieve visualisatie om ons te helpen nu reden over de resultaten van deze test maken.

1. Begin met het uitpakken van de verschillende voorspellingen en de resultaten van de **voorspellingen** tijdelijke tabel die eerder hebt gemaakt. De volgende query's scheiden de uitvoer als *true_positive*, *false_positive*, *true_negative*, en *false_negative*. In de onderstaande query's we uitschakelen visualisatie met `-q` en ook de uitvoer op te slaan (met behulp van `-o`) als dataframes die vervolgens kunnen worden gebruikt met de `%%local` verwerkt Magic-pakket.

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. Ten slotte het volgende fragment gebruiken voor het genereren van het tekengebied via **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    U ziet de volgende uitvoer:

    ![Spark machine learning-uitvoer van de toepassing - cirkeldiagram percentages van mislukte voeding controles. ] (./media/apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning-resultaat-uitvoer")

    In deze grafiek verwijst ' ' positief naar de inspectie mislukte voeding, terwijl een negatief resultaat naar een doorgegeven inspectie verwijst.

## <a name="shut-down-the-notebook"></a>De notebook afsluiten
Nadat u klaar bent met het uitvoeren van de toepassing, moet u de notebook om resources vrij te geven afgesloten. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**. Dit wordt afgesloten en wordt de notebook gesloten.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
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
