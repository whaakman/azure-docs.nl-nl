---
title: Analyseren van websitelogboeken met Python-bibliotheken in Spark - Azure
description: Dit notitieblok demonstreert hoe u voor het analyseren van logboekgegevens met behulp van een aangepaste bibliotheek met Spark op Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: 5492f4865e464cf8bedaee6e9b0ab25532e21459
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448760"
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-apache-spark-cluster-on-hdinsight"></a>Websitelogboeken analyseren met een aangepaste Python-bibliotheek met Apache Spark-cluster in HDInsight

Dit notitieblok demonstreert hoe u voor het analyseren van logboekgegevens met behulp van een aangepaste bibliotheek met Apache Spark in HDInsight. De aangepaste bibliotheek die we gebruiken is een Python-bibliotheek met de naam **iislogparser.py**.

> [!TIP]  
> In dit artikel is ook beschikbaar als een Jupyter-notebook in een Spark (Linux)-cluster die u in HDInsight maakt. De notebookervaring kunt u de Python-codefragmenten uitvoeren vanuit het notitieblok zelf. Als u het artikel van binnen een laptop, een Spark-cluster maken, start u een Jupyter-notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), en voer vervolgens de notebook **logboeken analyseren met Spark met behulp van een aangepaste library.ipynb** onder de **PySpark** map.
>
>

**Vereisten:**

U hebt het volgende:

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

## <a name="save-raw-data-as-an-rdd"></a>Onbewerkte gegevens opslaan als een RDD
In deze sectie gebruiken we de [Jupyter](https://jupyter.org) laptop die is gekoppeld aan een Apache Spark-cluster in HDInsight taken die de onbewerkte voorbeeldgegevensset verwerken en sla deze op als een Hive-tabel uit te voeren. De voorbeeldgegevens is een CSV-bestand (hvac.csv) beschikbaar in alle clusters standaard.

Wanneer uw gegevens is opgeslagen als een Apache Hive-tabel, in de volgende sectie verbinding we met de Hive-tabel met BI-tools, zoals Power BI en Tableau.

1. Klik vanaf het Startboard in [Azure Portal](https://portal.azure.com/) op de tegel voor uw Spark-cluster (als u deze aan het Startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.   
2. Klik vanuit de blade Spark-cluster op **Cluster-dashboard** en vervolgens op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   > [!NOTE]
   > Mogelijk bereikt u de Jupyter-notebook voor uw cluster ook door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Maak een nieuwe notebook. Klik op **Nieuw** en klik vervolgens op **PySpark**.

    ![Een nieuwe Jupyter-notebook maken](./media/apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Een nieuwe Jupyter-notebook maken")
4. Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled.pynb. Klik bovenaan op de naam van de notebook en wijzig deze in een beschrijvende naam.

    ![Een naam opgeven voor de notebook](./media/apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Een naam opgeven voor de notebook")
5. Omdat u de notebook met behulp van de PySpark-kernel hebt gemaakt, hoeft u niet expliciet contexten te maken. De Spark- en Hive-contexten worden automatisch voor u gemaakt tijdens het uitvoeren van de eerste codecel. U kunt starten door het importeren van de typen die vereist voor dit scenario zijn. Plak het volgende codefragment in een lege cel en druk op **Shift+Enter**.

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. Maak een RDD met behulp van de voorbeeld-logboekgegevens die al beschikbaar op het cluster. U kunt toegang tot de gegevens in het standaardopslagaccount dat is gekoppeld aan het cluster op **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. Haal een voorbeeldlogboek instellen om te controleren of de vorige stap is voltooid.

        logs.take(5)

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analyseren van logboekgegevens met behulp van een aangepaste Python-bibliotheek
1. De eerste paar regels bevatten de headerinformatie in de bovenstaande uitvoer en elke resterende regel komt overeen met het schema dat wordt beschreven in deze header. Deze logboeken parseren kan ingewikkeld zijn. Daarom gebruiken we een aangepaste Python-bibliotheek (**iislogparser.py**) Hiermee kunt u dergelijke veel eenvoudiger logboeken parseren. Deze bibliotheek is standaard opgenomen in uw Spark-cluster in HDInsight op **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Deze bibliotheek is echter niet in de `PYTHONPATH` , zodat we deze niet gebruiken met behulp van een instructie importeren, zoals `import iislogparser`. Voor het gebruik van deze bibliotheek, moeten we deze distribueren naar alle worker-knooppunten. Voer het volgende codefragment.

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. `iislogparser` biedt een functie `parse_log_line` die retourneert `None` als een regel een rij met koppen wordt en een exemplaar van retourneert de `LogLine` klasse als er een regel. Gebruik de `LogLine` klasse om op te halen van alleen de regels van het logboek van de RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. Ophalen van een aantal opgehaalde log regels om te controleren of de stap is voltooid.

       logLines.take(2)

   De uitvoer moet er ongeveer als volgt uitzien:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. De `LogLine` op zijn beurt heeft enkele nuttige methoden, zoals klasse, `is_error()`, die wordt geretourneerd of een logboekvermelding een fout op te geven heeft. Gebruik deze optie voor het berekenen van het aantal fouten in de uitgepakte log-regels en meld u vervolgens alle fouten naar een ander bestand.

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. U kunt ook **Matplotlib** om een visualisatie van de gegevens samen te stellen. Als u isoleren van de oorzaak van de aanvragen die gedurende een lange periode worden uitgevoerd wilt, wilt u mogelijk zoeken naar de bestanden die de meeste tijd voor het bieden van gemiddeld in beslag nemen.
   Het onderstaande codefragment wordt de bovenste 25 resources die de meeste tijd voor het bieden van een aanvraag heeft opgehaald.

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. U kunt ook deze informatie in de vorm van diagram aanwezig. Als een eerste stap voor het maken van een diagram laat het ons eerst maken een tijdelijke tabel **AverageTime**. De tabel de logboeken gegroepeerd op tijd om te zien of er ongebruikelijke latentiepieken op een bepaald moment zijn.

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. U kunt vervolgens uitvoeren met de volgende SQL-query om op te halen van alle records in de **AverageTime** tabel.

       %%sql -o averagetime
       SELECT * FROM AverageTime

   De `%%sql` magic gevolgd door `-o averagetime` zorgt ervoor dat de uitvoer van de query lokaal worden bewaard op de Jupyter-server (meestal het hoofdknooppunt van het cluster). De uitvoer wordt opgeslagen als een [Pandas](https://pandas.pydata.org/) gegevensframe met de opgegeven naam **averagetime**.

   Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

   ![SQL-query-uitvoer](./media/apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL query-uitvoer")

   Voor meer informatie over de `%%sql` magic, Zie [Parameters ondersteund met de %% sql magic](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
7. U kunt nu Matplotlib, een bibliotheek gebruikt om de visualisatie van gegevens, samen te gebruiken voor het maken van een diagram. Omdat de grafiek moet worden gemaakt van de lokaal persistente **averagetime** dataframe, het codefragment moet beginnen met de `%%local` Magic-pakket. Dit zorgt ervoor dat de code lokaal op de Jupyter-server wordt uitgevoerd.

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

   ![Matplotlib uitvoer](./media/apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib uitvoer")
8. Wanneer u klaar bent met het uitvoeren van de toepassing, moet u de notebook afsluiten om de resources vrij te geven. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**. Hiermee wordt de notebook afgesloten.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met BI: Interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](apache-spark-machine-learning-mllib-ipython.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om te maken en verzenden van Apache Spark Scala-toepassingen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op te sporen Apache Spark-toepassingen op afstand](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
