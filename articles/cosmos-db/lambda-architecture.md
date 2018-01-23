---
title: Lambda-architectuur met Azure Cosmos DB en HDInsight (Apache Spark) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een lambda-architectuur met behulp van Azure DB die Cosmos en HDInsight Spark implementeert
keywords: Lambda-architectuur
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: 273aeae9-e31c-4a43-b216-5751c46f212e
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 01/19/2018
ms.author: denlee
ms.openlocfilehash: f88f3fb05495b0f3330d5a4cde7718fe89b2f694
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="azure-cosmos-db-implement-a-lambda-architecture-on-the-azure-platform"></a>Azure Cosmos DB: Een lambda-architectuur implementeren op de Azure-platform 

Lambda-architecturen inschakelen efficiënte verwerking van grote gegevenssets. Lambda-architecturen gebruiken batchverwerking, stroom-verwerking en een laag voor de latentie die zijn betrokken bij het opvragen van big data te minimaliseren. 

Voor het implementeren van een lambda-architectuur in Azure, kunt u de volgende technologieën om realtime big data-analyses versnellen combineren:
* [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), de branche eerste globaal gedistribueerd en modellen database-service. 
* [Apache Spark voor Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/), een framework voor verwerking die grootschalige gegevens analytics toepassingen worden uitgevoerd
* Azure Cosmos DB [wijzigen van de feed](change-feed.md), waarmee nieuwe gegevens naar de batch-laag voor HDInsight om te verwerken streams
* De [Spark op Azure Cosmos DB-Connector](spark-connector.md)

In dit artikel beschrijft de grondbeginselen van een lambda-architectuur op basis van het oorspronkelijke ontwerp met meerdere lagen en de voordelen van een 'rearchitected' lambda-architectuur die bewerkingen vereenvoudigt.  

Bekijk de volgende video voor een overzicht van de lambda-architectuur en de bronnen die beschikbaar zijn in de steekproef lambda-architectuur:

> [!VIDEO https:///channel9.msdn.com/Events/Connect/2017/T135/player]
>

## <a name="what-is-a-lambda-architecture"></a>Wat is er een lambda-architectuur?
Een lambda-architectuur is algemeen, schaalbare en fouttolerante architectuur naar adres gegevensverwerking batch- en scenario's met latentie versnellen, zoals beschreven door [Nathan Marz](https://twitter.com/nathanmarz).

![Diagram van een lambda-architectuur](./media/lambda-architecture/lambda-architecture-intro.png)

Bron: http://lambda-architecture.net/

De grondbeginselen van een lambda-architectuur worden beschreven in het voorgaande diagram conform [https://lambda-architecture.net](http://lambda-architecture.net/).

 1. Alle **gegevens** wordt doorgeschoven, is in *beide* de *batch laag* en *snelheid laag*.
 2. De **batch laag** heeft een master gegevensset (niet-wijzigbaar, alleen toevoegen set onbewerkte gegevens) en vooraf berekent de batch-weergaven.
 3. De **voor laag** batch weergaven voor snelle query's. 
 4. De **snelheid laag** gecompenseerd verwerkingstijd (aan de laag voor) en recente gegevens alleen behandelt.
 5. Alle query's kunnen worden beantwoord door samenvoegen resultaten van batch-weergaven en realtime weergaven of ze afzonderlijk te pingen.

Bij meer informatie, is er mogelijk voor het implementeren van deze architectuur met alleen de volgende:

* Azure DB Cosmos verzameling(en)
* HDInsight (Apache Spark 2.1)-cluster
* Spark Connector [1.0](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark_2.1.0_2.11-1.0.0)

## <a name="speed-layer"></a>Snelheid laag

Twee streams gegevens onderhouden terwijl de juiste status van de gegevens worden een gecompliceerde datacenterbeheer vanuit een perspectief bewerkingen. Gebruiken om bewerkingen te vereenvoudigen, de [Azure Cosmos DB wijzigen feed ondersteuning](change-feed.md) te houden van de status voor de *batch laag* tijdens weer te geven het wijzigingenlogboek Azure Cosmos DB via de *wijzigen Feed API* voor uw *snelheid laag*.  
![Diagram van markering van de nieuwe gegevens, snelheid laag en master gegevensset gedeelte van de lambda-architectuur](./media/lambda-architecture/lambda-architecture-change-feed.png)

Wat is het belangrijk is in deze lagen:

 1. Alle **gegevens** wordt doorgeschoven *alleen* naar Azure Cosmos DB, dus kunt u voorkomen dat meerdere casten problemen.
 2. De **batch laag** heeft een master gegevensset (niet-wijzigbaar, alleen toevoegen set onbewerkte gegevens) en vooraf berekent de batch-weergaven.
 3. De **voor laag** in de volgende sectie wordt besproken.
 4. De **snelheid laag** maakt gebruik van HDInsight (Apache Spark) om te lezen van de feed van Azure Cosmos DB wijzigen. Hiermee kunt u uw gegevens ook behouden over vragen en deze gelijktijdig verwerken.
 5. Alle query's kunnen worden beantwoord door samenvoegen resultaten van batch-weergaven en realtime weergaven of ze afzonderlijk te pingen.
 
### <a name="code-example-spark-structured-streaming-to-an-azure-cosmos-db-change-feed"></a>Voorbeeld: Spark gestructureerd streaming aan een wijziging in de Azure DB die Cosmos-kanaal
Uitvoeren van een snelle prototype van de Azure DB die Cosmos-wijziging feed als onderdeel van de **snelheid laag**, kunt testen uit Twitter-gegevens gebruiken als onderdeel van de [stroom verwerken van wijzigingen met behulp van Azure Cosmos DB wijzigen Feed en Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)voorbeeld. Om snel de Twitter-uitvoer, Zie de voorbeeldcode in [stroom van Twitter-feed aan de database van de Cosmos](https://github.com/tknandu/TwitterCosmosDBFeed). Met het voorgaande voorbeeld, bent u Twitter-gegevens laden in Azure Cosmos DB en u kunt vervolgens uw (Apache Spark) HDInsight-cluster instellen verbinding maken met de wijziging feed. Zie voor meer informatie over het instellen van deze configuratie [Apache Spark op Azure Cosmos DB Connector Setup](https://github.com/Azure/azure-cosmosdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup).  

Het volgende codefragment toont hoe u configureert `spark-shell` om uit te voeren een gestructureerde streaming-taak verbinding maken met een wijziging in de Azure DB die Cosmos-kanaal waarmee het real-time Twitter-gegevensstroom, als u wilt uitvoeren van een actieve interval aantal beoordeelt.

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceConfigMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[MASTER KEY]",
"Database" -> "[DATABASE]",
"Collection" -> "[COLLECTION]",
"ConnectionMode" -> "Gateway",
"ChangeFeedCheckpointLocation" -> "checkpointlocation",
"changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Interval Count")

// Start reading change feed as a stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()

// Start streaming query to console sink
val query = streamData.withColumn("countcol", streamData.col("id").substr(0, 0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
```

Zie voor voorbeelden van de volledige code, [azure-cosmosdb-spark/lambda/samples](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda), waaronder:
* [Streaming Query van de Cosmos DB wijzigen Feed.scala](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala)
* [Streaming labels Query van de Cosmos DB wijzigen Feed.scala](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala)

De uitvoer van dit is een `spark-shell` -console, die continu wordt uitgevoerd een gestructureerde streaming-taak die een interval aantal tegen de Twitter-gegevens van de Azure DB die Cosmos-wijziging feed uitvoert. De volgende afbeelding ziet u de uitvoer van de taak stroom en het interval wordt geteld.

![Streaming-uitvoer met de interval aantal tegen de Twitter-gegevens van de feed van Azure Cosmos DB wijzigen](./media/lambda-architecture/lambda-architecture-speed-layer-twitter-count.png) 

Voor meer informatie over Azure Cosmos DB wijzigen feed, Zie:

* [Werken met de ondersteuning in Azure Cosmos DB feed wijziging](change-feed.md)
* [Inleiding tot de Azure CosmosDB wijziging Feed Processor-bibliotheek](https://azure.microsoft.com/blog/introducing-the-azure-cosmosdb-change-feed-processor-library/)
* [Stroom verwerken wijzigingen: De Azure CosmosDB wijziging feed + Apache Spark](https://azure.microsoft.com/blog/stream-processing-changes-azure-cosmosdb-change-feed-apache-spark/)

## <a name="batch-and-serving-layers"></a>Batch ten behoeve van lagen
Aangezien de nieuwe gegevens worden geladen in Azure Cosmos-DB (waarop de wijziging feed wordt gebruikt voor de laag snelheid), dit is waar de **master gegevensset** (een niet-wijzigbaar, alleen toevoegen set onbewerkte gegevens) zich bevindt. Vanaf dit punt en hoger, HDInsight gebruiken (Apache Spark) vooraf compute-bewerkingen uitvoeren vanuit de **batch laag** naar **voor laag**, zoals wordt weergegeven in de volgende afbeelding:

![Diagram van markering van de batch-laag ten behoeve van laag van de lambda-architectuur](./media/lambda-architecture/lambda-architecture-batch-serve.png)

Wat is het belangrijk is in deze lagen:

 1. Alle **gegevens** wordt doorgeschoven, is alleen naar Azure Cosmos-DB (problemen te vermijden multicast).
 2. De **batch laag** heeft een master gegevensset (niet-wijzigbaar, alleen toevoegen set onbewerkte gegevens) opgeslagen in Azure Cosmos DB. U kunt vooraf een uw aggregaties worden opgeslagen in uw weergaven berekende batch berekenen met behulp van HDI Spark.
 3. De **voor laag** is een Azure DB die Cosmos-database met verzamelingen voor de master gegevensset en batch weergave berekend.
 4. De **snelheid laag** verderop in dit artikel wordt besproken.
 5. Alle query's kunnen worden beantwoord door de resultaten van de batch en realtime weergaven samenvoegen of ze afzonderlijk te pingen.

### <a name="code-example-pre-computing-batch-views"></a>Voorbeeld: vooraf computing batch weergaven
Voor het uitvoeren van vooraf berekende weergaven voor presenteren uw **master gegevensset** van Apache Spark op Azure Cosmos DB, gebruikt u de volgende codefragmenten uit de notitieblokken [Lambda architectuur Rearchitected - Batch-laag ](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) en [Lambda-architectuur Rearchitected - Batch voor de laag](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb). In dit scenario gebruikt u de Twitter-gegevens opgeslagen in Azure Cosmos DB.

Begint met het maken van de configuratieverbinding met de Twitter-gegevens in Azure Cosmos DB met behulp van de PySpark-code hieronder.

```
# Configuration to connect to Azure Cosmos DB
tweetsConfig = {
  "Endpoint" : "[Endpoint URL]",
  "Masterkey" : "[Master Key]",
  "Database" : "[Database]",
  "Collection" : "[Collection]", 
  "preferredRegions" : "[Preferred Regions]",
  "SamplingRatio" : "1.0",
  "schema_samplesize" : "200000",
  "query_custom" : "[Cosmos DB SQL Query]"
}

# Create DataFrame
tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()

# Create Temp View (to run Spark SQL statements)
tweets.createOrReplaceTempView("tweets")
```

Vervolgens laten we de volgende Spark SQL-instructie om te bepalen van de top 10 hashtags van de set tweets worden uitgevoerd. Voor deze query Spark SQL uitvoert we dit in een Jupyter-notebook zonder het uitvoer-staafdiagram direct onder dit codefragment.

```
%%sql
select hashtags.text, count(distinct id) as tweets
from (
  select 
    explode(hashtags) as hashtags,
    id
  from tweets
) a
group by hashtags.text
order by tweets desc
limit 10
```

![Grafiek met het aantal tweets per hashtag](./media/lambda-architecture/lambda-architecture-batch-hashtags-bar-chart.png)

Nu dat u uw query hebt gemaakt, gaan we opslaan naar een verzameling met behulp van de Connector Spark uitvoergegevens opslaan in een andere verzameling.  In dit voorbeeld gebruiken Scala aan de verbinding presenteren. Vergelijkbaar met het vorige voorbeeld wordt de configuratieverbinding maken met de Apache Spark DataFrame opslaan naar een andere Azure DB die Cosmos-verzameling.

```
val writeConfigMap = Map(
    "Endpoint" -> "[Endpoint URL]",
    "Masterkey" -> "[Master Key]",
    "Database" -> "[Database]",
    "Collection" -> "[New Collection]", 
    "preferredRegions" -> "[Preferred Regions]",
    "SamplingRatio" -> "1.0",
    "schema_samplesize" -> "200000"
)

// Configuration to write
val writeConfig = Config(writeConfigMap)

```

Nadat u hebt opgegeven de `SaveMode` (waarmee wordt aangegeven of `Overwrite` of `Append` documenten), maak een `tweets_bytags` DataFrame vergelijkbaar met de Spark SQL-query in het vorige voorbeeld.  Met de `tweets_bytags` DataFrame gemaakt, kunt u opslaan met behulp van de `write` methode met behulp van de eerder opgegeven `writeConfig`.

```
// Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
import org.apache.spark.sql.{Row, SaveMode, SparkSession}

// Create new DataFrame of tweets tags
val tweets_bytags = spark.sql("select hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc")

// Save to Cosmos DB (using Append in this case)
tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Deze laatste instructie nu is uw DataFrame Spark opgeslagen in een nieuwe Azure DB die Cosmos-collectie; Dit is van een perspectief lambda-architectuur uw **batch-weergave** binnen de **voor laag**.
 
#### <a name="resources"></a>Resources

Zie voor voorbeelden van de volledige code, [azure-cosmosdb-spark/lambda/samples](vhttps://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda) met inbegrip van:
* Lambda-architectuur Rearchitected - Batch laag [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb)
* Lambda-architectuur Rearchitected - Batch aan voor de laag [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb)

## <a name="speed-layer"></a>Snelheid laag
Zoals eerder aangegeven, met de Azure Cosmos DB wijzigen Feed Library Hiermee kunt u de bewerkingen tussen de batch- en snelheid lagen te vereenvoudigen. In deze architectuur, gebruikt u Apache Spark (via HDInsight) uit te voeren de *gestructureerd streaming* query's op de gegevens. U kunt ook om tijdelijk de resultaten van uw gestructureerde streaming query's zodat andere systemen toegang krijgen deze gegevens tot kunnen.

![Diagram van markering van de laag van de snelheid van de lambda-architectuur](./media/lambda-architecture/lambda-architecture-speed.png)

U doet dit door een afzonderlijke Azure DB die Cosmos-verzameling voor het opslaan van de resultaten van uw gestructureerde streaming query's te maken.  Hiermee kunt u de toegang van andere systemen hebben deze informatie niet alleen Apache Spark. U kunt ook uw documenten automatisch moeten worden verwijderd na een ingestelde duur configureren met de functie Cosmos DB Time-to-Live (TTL).  Zie voor meer informatie over de functie Azure Cosmos DB TTL [gegevens in Azure Cosmos DB verzamelingen automatisch met TTL verloopt](time-to-live.md)

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceCollectionName = "[SOURCE COLLECTION NAME]"
val sinkCollectionName = "[SINK COLLECTION NAME]"

val configMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[COSMOSDB MASTER KEY]",
"Database" -> "[DATABASE NAME]",
"Collection" -> sourceCollectionName,
"ChangeFeedCheckpointLocation" -> "changefeedcheckpointlocation")

val sourceConfigMap = configMap.+(("changefeedqueryname", "Structured Stream replication streaming test"))

// Start to read the stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
val sinkConfigMap = configMap.-("collection").+(("collection", sinkCollectionName))

// Start the stream writer to new collection
val streamingQueryWriter = streamData.writeStream.format(classOf[CosmosDBSinkProvider].getName).outputMode("append").options(sinkConfigMap).option("checkpointLocation", "streamingcheckpointlocation")
var streamingQuery = streamingQueryWriter.start()

```

## <a name="lambda-architecture-rearchitected"></a>Lambda-architectuur: Rearchitected
Zoals beschreven in de vorige secties, kunt u de oorspronkelijke lambda-architectuur vereenvoudigen met behulp van de volgende onderdelen:
* Azure Cosmos DB
* De Azure Cosmos DB wijzigen Feed Library multicast moet uw om gegevens te vermijden tussen de lagen van de batch- en snelheid
* Apache Spark in HDInsight
* De Spark-Connector voor Azure Cosmos DB

![Diagram van de rearchitecture van de lambda-architectuur met Azure Cosmos DB, Spark en de Azure Cosmos DB wijzigen Feed-API](./media/lambda-architecture/lambda-architecture-re-architected.png)

Bij dit ontwerp hoeft u slechts twee beheerde services, Azure Cosmos DB en HDInsight. Ze omgaan samen met de batch, servers en lagen van de snelheid van de lambda-architectuur. Dit vereenvoudigt het niet alleen de bewerkingen, maar ook de gegevensstroom. 
 1. Alle gegevens voor verwerking geduwd naar Azure Cosmos-DB
 2. De laag batch heeft een master gegevensset (niet-wijzigbaar, alleen toevoegen set onbewerkte gegevens) en vooraf berekent de batch-weergaven
 3. De laag voor heeft batch-weergaven van gegevens voor snelle query's.
 4. De snelheid laag gecompenseerd verwerkingstijd (aan de laag voor) en behandelt alleen recente gegevens.
 5. Alle query's kunnen worden beantwoord door de resultaten van de batch en realtime weergaven samenvoegen.

### <a name="resources"></a>Resources

 * **Nieuwe gegevens**: de [stroom van Twitter-feed naar CosmosDB](https://github.com/tknandu/TwitterCosmosDBFeed), dit is het mechanisme voor de push-nieuwe gegevens naar Azure Cosmos DB.
 * **Batch-laag:** de batch-laag bestaat uit de *master gegevensset* (een niet-wijzigbaar, alleen toevoegen set onbewerkte gegevens) en de mogelijkheid om vooraf compute batch weergaven van de gegevens die worden doorgegeven in de **voor laag** .
    * De **Lambda architectuur Rearchitected - Batch laag** notebook [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) query's de *master gegevensset* de set van batch-weergaven.
 * **Voor laag:** de **voor laag** bestaat uit vooraf berekende gegevens, wat resulteert in een batch-weergaven (bijvoorbeeld aggregaties, specifieke slicers, enz.) voor snelle query's.
    * De **Lambda architectuur Rearchitected - Batch aan voor de laag** notebook [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) duwt de batch-gegevens naar de laag voor; dat wil zeggen, Spark query's van een batch-verzameling van tweets, verwerkt en opgeslagen in een andere verzameling (een berekende batch).
* **Snelheid laag:** de **snelheid laag** bestaat uit Spark met behulp van de Azure DB die Cosmos-wijziging feed lees-en onmiddellijk ondernemen. De gegevens kan ook worden opgeslagen op *berekend RT* zodat andere systemen kunnen opvragen de verwerkte realtime gegevens in plaats van een realtime uitgevoerd query zelf.
    * De [Streaming Query van de Cosmos DB wijzigen Feed](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala) scala script voert u een streaming-query van de Azure DB die Cosmos-wijziging feed voor het berekenen van een interval aantal van de spark-shell.
    * De [labels Query Streaming van Cosmos-DB wijzigen Feed](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala) scala script voert u een streaming-query van de Azure DB die Cosmos-wijziging feed voor het berekenen van een interval aantal tags van de spark-shell.
  
## <a name="next-steps"></a>Volgende stappen
Als u nog niet gedaan hebt, downloadt u de Spark naar Azure DB die Cosmos-connector in vanuit de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-opslagplaats en bekijk de aanvullende bronnen in de opslagplaats:
* [Lambda-architectuur](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda)
* [Voorbeelden van gedistribueerde aggregaties](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)
* [Voorbeelden van scripts en laptops](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)
* [Gestructureerde streaming demo 's](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)
* [Wijziging feed demo 's](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [De stroom verwerken van wijzigingen met behulp van Azure Cosmos DB wijzigen Feed en Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)

U kunt ook om te controleren de [Apache Spark SQL, DataFrames en gegevenssets handleiding](http://spark.apache.org/docs/latest/sql-programming-guide.html) en de [Apache Spark in Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artikel.
