---
title: Application Insights-logboekbestanden analyseren met Spark - Azure HDInsight
description: Informatie over het exporteren van Application Insights-logboeken voor blob-opslag en vervolgens de logboeken analyseren met Spark in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/09/2018
ms.openlocfilehash: 806e5b6f764797d2e038cc7ed58ec1d04f678e2b
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120373"
---
# <a name="analyze-application-insights-telemetry-logs-with-apache-spark-on-hdinsight"></a>Application Insights-logboekbestanden met telemetrie met Apache Spark in HDInsight analyseren

Meer informatie over het gebruik van [Apache Spark](https://spark.apache.org/) op HDInsight naar Application Insights-telemetriegegevens te analyseren.

[Visual Studio Application Insights](../../azure-monitor/app/app-insights-overview.md) is een Analyseservice die uw webtoepassingen bewaakt. Telemetrische gegevens die worden gegenereerd door Application Insights kunnen worden geëxporteerd naar Azure Storage. Wanneer de gegevens in Azure Storage, kan HDInsight worden gebruikt om te analyseren.

## <a name="prerequisites"></a>Vereisten

* Een toepassing die is geconfigureerd voor het gebruik van Application Insights.

* Als u bekend bent met het maken van een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie, [maakt Apache Spark in HDInsight](apache-spark-jupyter-spark-sql.md).

  > [!IMPORTANT]  
  > Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een webbrowser.

De volgende resources zijn gebruikt bij het ontwikkelen en testen van dit document:

* Application Insights-telemetriegegevens is gegenereerd met een [Node.js-web-app geconfigureerd voor het gebruik van Application Insights](../../azure-monitor/app/nodejs.md).

* Een Spark op basis van Linux in HDInsight-clusterversie 3.5 is gebruikt om de gegevens te analyseren.

## <a name="architecture-and-planning"></a>Architectuur en planning

Het volgende diagram illustreert de servicearchitectuur van dit voorbeeld:

![diagram van gegevensstromen van Application Insights naar blob-opslag, en vervolgens wordt verwerkt door Spark in HDInsight](./media/apache-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure Storage

Application Insights kunnen worden geconfigureerd voor het continu telemetriegegevens exporteren naar blobs. HDInsight kan vervolgens lezen van gegevens die zijn opgeslagen in de blobs. Er zijn echter enkele vereisten die u moet volgen:

* **Locatie**: Als het Opslagaccount en HDInsight zich in verschillende locaties, kan deze latentie verhogen. Het verhoogt ook kosten, als uitgaande kosten worden toegepast op gegevens verplaatsen tussen regio's.

    > [!WARNING]  
    > Met behulp van een Storage-Account in een andere locatie dan HDInsight wordt niet ondersteund.

* **Blobtype**: HDInsight biedt alleen ondersteuning voor blok-blobs. Application Insights-standaardinstellingen voor het gebruik van blok-blobs, dus kunnen worden gebruikt standaard met HDInsight.

Zie voor meer informatie over het toevoegen van opslag aan een bestaand cluster de [extra opslagaccounts toevoegen](../hdinsight-hadoop-add-storage.md) document.

### <a name="data-schema"></a>Gegevensschema

Application Insights biedt [gegevensmodel exporteren](../../azure-monitor/app/export-data-model.md) informatie voor de indeling van de telemetrie naar blobs worden geëxporteerd. Spark SQL de stappen in dit document gebruiken om te werken met de gegevens. Spark SQL kan automatisch genereren van een schema voor de JSON-gegevensstructuur die door Application Insights geregistreerd.

## <a name="export-telemetry-data"></a>Telemetrie exporteren

Volg de stappen in [continue Export configureren](../../azure-monitor/app/export-telemetry.md) naar uw Application Insights configureren voor telemetriegegevens exporteren naar een Azure storage-blob.

## <a name="configure-hdinsight-to-access-the-data"></a>HDInsight voor toegang tot de gegevens configureren

Als u een HDInsight-cluster maakt, moet u de storage-account toevoegen tijdens het maken van clusters.

Gebruik de informatie in de Azure Storage-Account toevoegen aan een bestaand cluster, de [extra Opslagaccounts toevoegen](../hdinsight-hadoop-add-storage.md) document.

## <a name="analyze-the-data-pyspark"></a>De gegevens worden geanalyseerd: PySpark

1. Uit de [Azure-portal](https://portal.azure.com), selecteert u uw Spark in HDInsight-cluster. Uit de **snelkoppelingen** sectie, selecteer **Clusterdashboards**, en selecteer vervolgens **Jupyter-Notebook** in het gedeelte Cluster Dashboard__.

    ![De clusterdashboards](./media/apache-spark-analyze-application-insight-logs/clusterdashboards.png)

2. Selecteer in de rechterbovenhoek van de pagina Jupyter **nieuw**, en vervolgens **PySpark**. Een nieuw browsertabblad met een Python gebaseerde Jupyter-Notebook wordt geopend.

3. In het eerste veld (met de naam een **cel**) op de pagina, voert u de volgende tekst:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Deze code wordt Spark geconfigureerd voor recursief toegang tot de directory-structuur voor de invoergegevens. Telemetrie Application Insights wordt vastgelegd in een directory-structuur die vergelijkbaar is met de `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Gebruik **SHIFT + ENTER** de code uit te voeren. Aan de linkerkant van de cel, een '\*' wordt weergegeven tussen de haakjes om aan te geven dat de code in deze cel wordt uitgevoerd. Zodra deze is voltooid, de '\*' wijzigingen in een aantal en de uitvoer is vergelijkbaar met de volgende tekst wordt weergegeven onder de cel:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Een nieuwe cel wordt gemaakt onder het eerste item. Voer de volgende tekst in de nieuwe cel. Vervang `CONTAINER` en `STORAGEACCOUNT` met de Azure storage-accountnaam en de naam van de blob-container met Application Insights-gegevens.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Gebruik **SHIFT + ENTER** om uit te voeren van deze cel. U ziet een resultaat vergelijkbaar met de volgende tekst:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Het geretourneerde wasb-pad is de locatie van de telemetriegegevens van Application Insights. Wijzig de `hdfs dfs -ls` regel in de cel in het wasb-pad dat is geretourneerd, en gebruik vervolgens **SHIFT + ENTER** opnieuw uit te voeren de cel. Deze tijd het resultaat moeten de mappen met de telemetrische gegevens worden weergegeven.

   > [!NOTE]  
   > Voor de rest van de stappen in deze sectie de `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory is gebruikt. De directorystructuur van uw kan afwijken.

6. Voer de volgende code in de volgende cel: Vervang `WASB_PATH` met het pad van de vorige stap.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Deze code maakt een dataframe uit de JSON-bestanden die door het proces van continue export worden geëxporteerd. Gebruik **SHIFT + ENTER** om uit te voeren van deze cel.
7. Voer in de volgende cel en voer de volgende om het schema dat Spark voor de JSON-bestanden gemaakt weer te geven:

   ```python
   jsonData.printSchema()
   ```

    Het schema voor elk type telemetrie verschilt. Het volgende voorbeeld wordt het schema dat is gegenereerd voor de webservice-aanvragen (gegevens die zijn opgeslagen de `Requests` submap):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)
8. Gebruik de volgende het gegevensframe registreren als een tijdelijke tabel en een query uitvoeren op de gegevens:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Deze query retourneert de stad-informatie voor de top 20 records waarin context.location.city niet null is.

   > [!NOTE]  
   > De structuur van de context is aanwezig zijn in alle telemetrie die zijn vastgelegd door Application Insights. De stad-element kan niet worden ingevuld in uw Logboeken. Het schema gebruiken voor het identificeren van de andere elementen die u kunt een query die gegevens voor uw logboeken kunnen bevatten.

    Deze query wordt informatie weergegeven die vergelijkbaar is met de volgende tekst:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-the-data-scala"></a>De gegevens worden geanalyseerd: Scala

1. Uit de [Azure-portal](https://portal.azure.com), selecteert u uw Spark in HDInsight-cluster. Uit de **snelkoppelingen** sectie, selecteer **Clusterdashboards**, en selecteer vervolgens **Jupyter-Notebook** in het gedeelte Cluster Dashboard__.

    ![De clusterdashboards](./media/apache-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Selecteer in de rechterbovenhoek van de pagina Jupyter **nieuw**, en vervolgens **Scala**. Een nieuw browsertabblad met een Scala gebaseerde Jupyter-Notebook wordt weergegeven.
3. In het eerste veld (met de naam een **cel**) op de pagina, voert u de volgende tekst:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Deze code wordt Spark geconfigureerd voor recursief toegang tot de directory-structuur voor de invoergegevens. Telemetrie Application Insights wordt vastgelegd in een directory-structuur die vergelijkbaar is met `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Gebruik **SHIFT + ENTER** de code uit te voeren. Aan de linkerkant van de cel, een '\*' wordt weergegeven tussen de haakjes om aan te geven dat de code in deze cel wordt uitgevoerd. Zodra deze is voltooid, de '\*' wijzigingen in een aantal en de uitvoer is vergelijkbaar met de volgende tekst wordt weergegeven onder de cel:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Een nieuwe cel wordt gemaakt onder het eerste item. Voer de volgende tekst in de nieuwe cel. Vervang `CONTAINER` en `STORAGEACCOUNT` met de Azure storage-accountnaam en de naam van de blob-container met Application Insights-Logboeken.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Gebruik **SHIFT + ENTER** om uit te voeren van deze cel. U ziet een resultaat vergelijkbaar met de volgende tekst:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Het geretourneerde wasb-pad is de locatie van de telemetriegegevens van Application Insights. Wijzig de `hdfs dfs -ls` regel in de cel in het wasb-pad dat is geretourneerd, en gebruik vervolgens **SHIFT + ENTER** opnieuw uit te voeren de cel. Deze tijd het resultaat moeten de mappen met de telemetrische gegevens worden weergegeven.

   > [!NOTE]  
   > Voor de rest van de stappen in deze sectie de `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory is gebruikt. Deze map bestaat niet, tenzij de telemetriegegevens voor een web-app.

6. Voer de volgende code in de volgende cel: Vervang `WASB\_PATH` met het pad van de vorige stap.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Deze code maakt een dataframe uit de JSON-bestanden die door het proces van continue export worden geëxporteerd. Gebruik **SHIFT + ENTER** om uit te voeren van deze cel.

7. Voer in de volgende cel en voer de volgende om het schema dat Spark voor de JSON-bestanden gemaakt weer te geven:

   ```scala
   jsonData.printSchema
   ```

    Het schema voor elk type telemetrie verschilt. Het volgende voorbeeld wordt het schema dat is gegenereerd voor de webservice-aanvragen (gegevens die zijn opgeslagen de `Requests` submap):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Gebruik de volgende het gegevensframe registreren als een tijdelijke tabel en een query uitvoeren op de gegevens:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    Deze query retourneert de stad-informatie voor de top 20 records waarin context.location.city niet null is.

   > [!NOTE]  
   > De structuur van de context is aanwezig zijn in alle telemetrie die zijn vastgelegd door Application Insights. De stad-element kan niet worden ingevuld in uw Logboeken. Het schema gebruiken voor het identificeren van de andere elementen die u kunt een query die gegevens voor uw logboeken kunnen bevatten.
   >
   >

    Deze query wordt informatie weergegeven die vergelijkbaar is met de volgende tekst:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor meer voorbeelden van het gebruik van Apache Spark voor het werken met gegevens en services in Azure:

* [Apache Spark met BI: Interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

Zie de volgende documenten voor meer informatie over het maken en uitvoeren van Spark-toepassingen:

* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren op een Apache Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)
