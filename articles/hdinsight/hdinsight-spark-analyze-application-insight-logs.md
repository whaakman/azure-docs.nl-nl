---
title: Logboeken van de toepassing inzicht te analyseren met Spark - Azure HDInsight | Microsoft Docs
description: Informatie over het exporteren van Application Insight Logboeken om de blob-opslag en vervolgens de logboeken met Spark in HDInsight analyseren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 883beae6-9839-45b5-94f7-7eb0f4534ad5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: d98e403683618ef6115372f99e4949af87af4490
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="analyze-application-insights-telemetry-logs-with-spark-on-hdinsight"></a>Application Insights telemetrie logboeken met Spark in HDInsight analyseren

Informatie over het gebruik van Spark in HDInsight voor het analyseren van Application Insight telemetrische gegevens.

[Visual Studio Application Insights](../application-insights/app-insights-overview.md) is een Analyseservice die uw webtoepassingen bewaakt. Telemetriegegevens die zijn gegenereerd door de Application Insights kunnen worden geëxporteerd naar Azure Storage. Nadat de gegevens Azure Storage is, kan de HDInsight worden gebruikt voor het analyseren van het.

## <a name="prerequisites"></a>Vereisten

* Een toepassing die is geconfigureerd voor gebruik van Application Insights.

* Als u bekend bent met het maken van een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie [maken Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

  > [!IMPORTANT]
  > De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Een webbrowser.

De volgende bronnen werden gebruikt in ontwikkelen en testen van dit document:

* Application Insights-telemetriegegevens is gegenereerd met een [Node.js-web-app geconfigureerd voor het gebruik van Application Insights](../application-insights/app-insights-nodejs.md).

* Een Spark op basis van Linux in HDInsight-cluster versie 3.5 is gebruikt om de gegevens te analyseren.

## <a name="architecture-and-planning"></a>Architectuur en planning

Het volgende diagram illustreert de service-architectuur van dit voorbeeld:

![diagram van de gegevens die binnenkomen in Application Insights naar blob storage en vervolgens wordt verwerkt door Spark in HDInsight](./media/hdinsight-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Azure-opslag

Application Insights kunnen worden geconfigureerd voor het continu telemetrie gegevens exporteren naar blobs. Gegevens opgeslagen in de blobs kan vervolgens door HDInsight worden gelezen. Er zijn echter enkele vereisten die u moet volgen:

* **Locatie**: als de Storage-Account en HDInsight zich in verschillende locaties, kan deze latentie toenemen. Het verhoogt kosten, ook als uitgaande kosten worden toegepast op gegevens verplaatsen tussen regio's.

    > [!WARNING]
    > Met behulp van een Opslagaccount in een andere locatie dan HDInsight wordt niet ondersteund.

* **BLOB-type**: HDInsight ondersteunt alleen blok-blobs. Application Insights standaardinstellingen voor het gebruik van blok-blobs, moet dus samenwerken met HDInsight standaard.

Zie voor meer informatie over het toevoegen van extra opslagruimte aan een bestaand HDInsight-cluster de [extra opslagaccounts toevoegen](hdinsight-hadoop-add-storage.md) document.

### <a name="data-schema"></a>Gegevensschema

Application Insights biedt [exporteren gegevensmodel](../application-insights/app-insights-export-data-model.md) informatie voor de indeling van de telemetrie geëxporteerd naar blobs. De stappen in dit document Spark SQL gebruiken om te werken met de gegevens. Spark SQL kan automatisch genereren van een schema voor de structuur van de JSON-gegevens vastgelegd door Application Insights.

## <a name="export-telemetry-data"></a>Exporteren van telemetriegegevens

Volg de stappen in [configureren met continue Export](../application-insights/app-insights-export-telemetry.md) voor het configureren van uw Application Insights om te exporteren van telemetrie informatie naar een Azure storage-blob.

## <a name="configure-hdinsight-to-access-the-data"></a>HDInsight toegang tot de gegevens configureren

Als u een HDInsight-cluster maakt, moet u de storage-account toevoegen tijdens het maken van het cluster.

Als de Azure Storage-Account toevoegen aan een bestaand cluster, kunt u de informatie in de [extra Opslagaccounts toevoegen](hdinsight-hadoop-add-storage.md) document.

## <a name="analyze-the-data-pyspark"></a>De gegevens analyseren: PySpark

1. Van de [Azure-portal](https://portal.azure.com), selecteer uw Spark in HDInsight-cluster. Van de **snelkoppelingen** sectie **Clusterdashboards**, en selecteer vervolgens **Jupyter-Notebook** uit de Cluster Dashboard__-blade.

    ![De clusterdashboards](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)

2. Selecteer in de rechterbovenhoek van de pagina Jupyter **nieuw**, en vervolgens **PySpark**. Hiermee opent u een nieuw browsertabblad met een Jupyter-Notebook voor op basis van Python.

3. In het eerste veld (aangeroepen een **cel**) op de pagina, voer de volgende tekst:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Deze code configureert Spark op recursief toegang de mapstructuur voor de invoergegevens. Application Insights telemetry wordt vastgelegd in een directory-structuur die vergelijkbaar is met de `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Gebruik **SHIFT + ENTER** de code uit te voeren. Aan de linkerkant van de cel een '\*' wordt weergegeven tussen de haakjes om aan te geven dat de code in deze cel wordt uitgevoerd. Nadat deze is voltooid, de '\*' wijzigingen in een aantal en de uitvoer ziet er als de volgende tekst onder de cel wordt weergegeven:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Een nieuwe cel wordt gemaakt onder het eerste item. Voer de volgende tekst in de nieuwe cel. Vervang `CONTAINER` en `STORAGEACCOUNT` met de naam van het Azure-opslagaccount en containernaam blob met Application Insights-gegevens.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Gebruik **SHIFT + ENTER** uitvoeren van deze cel. Er is een resultaat vergelijkbaar met de volgende tekst:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Het geretourneerde wasb pad is de locatie van de telemetriegegevens Application Insights. Wijzig de `hdfs dfs -ls` regel in de cel in het wasb pad geretourneerd en gebruik vervolgens **SHIFT + ENTER** opnieuw uitvoeren van de cel. Deze tijd de resultaten moeten de mappen met telemetrische gegevens worden weergegeven.

   > [!NOTE]
   > Voor het restant van de stappen in deze sectie de `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory gebruikt. De directorystructuur van uw zijn anders.

6. Voer de volgende code in de volgende cel: Vervang `WASB_PATH` met het pad van de vorige stap.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Deze code maakt een dataframe van de JSON-bestanden dat is geëxporteerd door de continue export-proces. Gebruik **SHIFT + ENTER** om uit te voeren deze cel.
7. Voer in de volgende cel en voer de volgende om het schema dat Spark voor de JSON-bestanden gemaakt:

   ```python
   jsonData.printSchema()
   ```

    Het schema voor elk type telemetrie verschilt. Het volgende voorbeeld wordt het schema dat is gegenereerd voor webaanvragen (gegevens die zijn opgeslagen de `Requests` submap):

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
8. Gebruik de volgende de dataframe registreren als een tijdelijke tabel en een query uitvoert op de gegevens:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Deze query retourneert de stad-gegevens voor de top 20 records waar context.location.city niet null is.

   > [!NOTE]
   > De structuur van de context is aanwezig in alle telemetrie vastgelegd door Application Insights. Het element stad kan niet worden gevuld in de logboeken. Het schema gebruiken om andere elementen die u kunt een query die gegevens voor uw logboeken bevatten mogelijk te identificeren.

    Deze query retourneert informatie vergelijkbaar met de volgende tekst:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-the-data-scala"></a>De gegevens analyseren: Scala

1. Van de [Azure-portal](https://portal.azure.com), selecteer uw Spark in HDInsight-cluster. Van de **snelkoppelingen** sectie **Clusterdashboards**, en selecteer vervolgens **Jupyter-Notebook** uit de Cluster Dashboard__-blade.

    ![De clusterdashboards](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)
2. Selecteer in de rechterbovenhoek van de pagina Jupyter **nieuw**, en vervolgens **Scala**. Een nieuw browsertabblad met een Scala gebaseerde Jupyter-Notebook wordt weergegeven.
3. In het eerste veld (aangeroepen een **cel**) op de pagina, voer de volgende tekst:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Deze code configureert Spark op recursief toegang de mapstructuur voor de invoergegevens. Application Insights telemetry wordt vastgelegd in een mapstructuur lijkt op `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Gebruik **SHIFT + ENTER** de code uit te voeren. Aan de linkerkant van de cel een '\*' wordt weergegeven tussen de haakjes om aan te geven dat de code in deze cel wordt uitgevoerd. Nadat deze is voltooid, de '\*' wijzigingen in een aantal en de uitvoer ziet er als de volgende tekst onder de cel wordt weergegeven:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Een nieuwe cel wordt gemaakt onder het eerste item. Voer de volgende tekst in de nieuwe cel. Vervang `CONTAINER` en `STORAGEACCOUNT` registreert met de Azure-opslag-accountnaam en de naam van de blob-container met Application Insights.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Gebruik **SHIFT + ENTER** uitvoeren van deze cel. Er is een resultaat vergelijkbaar met de volgende tekst:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Het geretourneerde wasb pad is de locatie van de telemetriegegevens Application Insights. Wijzig de `hdfs dfs -ls` regel in de cel in het wasb pad geretourneerd en gebruik vervolgens **SHIFT + ENTER** opnieuw uitvoeren van de cel. Deze tijd de resultaten moeten de mappen met telemetrische gegevens worden weergegeven.

   > [!NOTE]
   > Voor het restant van de stappen in deze sectie de `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` directory gebruikt. Deze map bestaat niet, tenzij de telemetriegegevens voor een web-app.

6. Voer de volgende code in de volgende cel: Vervang `WASB\_PATH` met het pad van de vorige stap.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Deze code maakt een dataframe van de JSON-bestanden dat is geëxporteerd door de continue export-proces. Gebruik **SHIFT + ENTER** om uit te voeren deze cel.

7. Voer in de volgende cel en voer de volgende om het schema dat Spark voor de JSON-bestanden gemaakt:

   ```scala
   jsonData.printSchema
   ```

    Het schema voor elk type telemetrie verschilt. Het volgende voorbeeld wordt het schema dat is gegenereerd voor webaanvragen (gegevens die zijn opgeslagen de `Requests` submap):

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

8. Gebruik de volgende de dataframe registreren als een tijdelijke tabel en een query uitvoert op de gegevens:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    Deze query retourneert de stad-gegevens voor de top 20 records waar context.location.city niet null is.

   > [!NOTE]
   > De structuur van de context is aanwezig in alle telemetrie vastgelegd door Application Insights. Het element stad kan niet worden gevuld in de logboeken. Het schema gebruiken om andere elementen die u kunt een query die gegevens voor uw logboeken bevatten mogelijk te identificeren.
   >
   >

    Deze query retourneert informatie vergelijkbaar met de volgende tekst:

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

Zie de volgende documenten voor meer voorbeelden van het gebruik van Spark werken met gegevens en services in Azure:

* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Spark in HDInsight voor het bouwen van streamingtoepassingen gebruiken](hdinsight-apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

Zie de volgende documenten voor meer informatie over het maken en uitvoeren van Spark-toepassingen:

* [Een zelfstandige toepassing maken met behulp van Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](hdinsight-apache-spark-livy-rest-interface.md)
