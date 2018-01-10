---
title: Apache Spark verbinden met Azure Cosmos DB | Microsoft Docs
description: Gebruik deze handleiding voor meer informatie over de Azure Cosmos DB Spark-connector die u kunt verbinding maken Apache Spark op Azure Cosmos DB gedistribueerde aggregaties en gegevens sciences globaal uitvoeren op de multitenant gedistribueerde databasesysteem van Microsoft die ontworpen voor de cloud.
keywords: Apache spark
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: denlee
ms.openlocfilehash: 4ba8a53f2018727cc4fa225b2d4ce14d9f1d7467
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a>Versnellen realtime big data-analyses met de Spark op Azure DB die Cosmos-connector

De Spark op Azure DB die Cosmos-connector kunt Azure Cosmos DB om te fungeren als een invoerbron of uitvoerlocatie voor Apache Spark-taken. Verbinding maken met [Spark](http://spark.apache.org/) naar [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) versnelt de mogelijkheid voor het oplossen van problemen snel bewegende wetenschappelijke waarin u Azure Cosmos DB kunt gebruiken om snel te behouden en gegevens opvragen. De Spark op Azure DB die Cosmos-connector maakt efficiënt gebruik van de systeemeigen Azure Cosmos DB beheerd-indexen. De indexen in te schakelen bij te werken kolommen wanneer u analyses uitvoeren en gegevens die van het Internet der dingen (IoT variëren) push-down-predicaat is voor het filteren op basis van snel veranderende globaal gedistribueerd naar gegevensscenario wetenschappelijke en analyses.

Meer informatie in deze video met Azure Cosmos DB Principal Program Manager Denny Lee. 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T135/player] 

Zie voor het werken met Spark GraphX en de Gremlin graph API's van Azure Cosmos DB, [grafiek analyses met behulp van Spark en Apache TinkerPop Gremlin uitvoeren](spark-connector-graph.md).

## <a name="download"></a>Downloaden

Om te beginnen, downloadt u de Spark naar Azure DB die Cosmos-connector in vanuit de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/) opslagplaats op GitHub.

## <a name="connector-components"></a>Connector-onderdelen

De connector maakt gebruik van de volgende onderdelen:

* [Azure Cosmos DB](http://documentdb.com) kunnen klanten in te richten en te schalen zowel doorvoer en opslag via verschillende geografische regio's. De service biedt:
   * Schakel sleutel [globale distributie](distribute-data-globally.md) en horizontale schaal
   * Gegarandeerd één cijfer latenties op het 99th percentiel
   * [Meerdere goed gedefinieerde consistentie modellen](consistency-levels.md)
   * Hoge beschikbaarheid met mogelijkheden voor multihoming gegarandeerd
   * Alle functies worden ondersteund door de toonaangevende, uitgebreide [serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db) (Sla's).

* [Apache Spark](http://spark.apache.org/) is een krachtige open-source-verwerkingsengine die gebaseerd op snelheid, gebruiksgemak en geavanceerde analyses,.

* [Apache Spark in Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) zodat u Apache Spark in de cloud voor essentiële implementaties implementeren met behulp van kunt [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Officieel ondersteunde versies:

| Onderdeel | Versie |
|---------|-------|
|Apache Spark|2.0.2, 2.1.0, 2.2.0|
| Scala| 2.10, 2.11|
| Azure Cosmos DB SQL Java SDK | 1.14.0, 1.15.0 |

In dit artikel kunt u enkele eenvoudige voorbeelden uitvoeren met behulp van Python (via pyDocumentDB) en de Scala-interfaces.

Er zijn twee benaderingen Apache Spark en Azure Cosmos DB verbinden:
- Gebruik pyDocumentDB via de [Cosmos SQL DB-Python SDK van Azure](https://github.com/Azure/azure-documentdb-python).
- Een op Java gebaseerde Spark op Azure DB die Cosmos-connector maken door het gebruik van de [Java SDK voor Azure Documentdb SQL Cosmos](https://github.com/Azure/azure-documentdb-java).

## <a name="pydocumentdb-implementation"></a>pyDocumentDB-implementatie
De huidige [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) kunt u verbinding maken met Spark Azure Cosmos DB zoals weergegeven in het volgende diagram:

![Spark op Azure Cosmos DB gegevensoverdracht via pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-the-pydocumentdb-implementation"></a>Gegevensstroom van de implementatie van de pyDocumentDB

De gegevensstroom is als volgt:

1. Het hoofdknooppunt Spark verbindt met het Azure Cosmos DB gateway-knooppunt via pyDocumentDB. Een gebruiker geeft alleen de Spark en Azure Cosmos DB-verbindingen. Verbindingen met de bijbehorende hoofd- en gateway-knooppunten zijn transparant voor de gebruiker.
2. Het gateway-knooppunt wordt de query op Azure Cosmos DB waarbij de query later wordt uitgevoerd op de verzameling van partities in de gegevensknooppunten. Het antwoord voor de query's wordt verzonden naar het gateway-knooppunt en die resultatenset wordt geretourneerd naar het hoofdknooppunt van Spark.
3. Volgende query's (bijvoorbeeld op een Spark-DataFrame) worden verzonden met de werkrolknooppunten Spark voor verwerking.

Communicatie tussen Spark en Azure Cosmos DB is beperkt tot de Spark-hoofdknooppunt en Azure Cosmos DB gateway-knooppunten.  De query's gaan snel de transportlaag tussen deze twee knooppunten kunt.

### <a name="install-pydocumentdb"></a>PyDocumentDB installeren
U kunt pyDocumentDB in uw Stuurprogrammaknooppunt installeren met behulp van **pip**, bijvoorbeeld:

```bash
pip install pyDocumentDB
```


### <a name="connect-spark-to-azure-cosmos-db-via-pydocumentdb"></a>Spark verbinden met Azure Cosmos DB via pyDocumentDB
De eenvoud van het transport communicatie maakt uitvoering van een query uit Spark bij Azure Cosmos DB met behulp van pyDocumentDB relatief eenvoudig.

Het volgende codefragment laat zien hoe pyDocumentDB gebruiken in een Spark-context.

```python
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

Zoals vermeld in het codefragment:

* De Azure-SDK voor Python Cosmos DB (`pyDocumentDB`) bevat de alle benodigde verbindingsparameters. De parameter voorkeur locaties kiest bijvoorbeeld de lezen volgorde van de replica en prioriteit.
*  Importeer de benodigde bibliotheken en configureer uw **hoofdsleutel** en **host** voor het maken van de Cosmos Azure DB *client* (**pydocumentdb.document_client**).


### <a name="execute-spark-queries-via-pydocumentdb"></a>Spark-query's uitvoeren via pyDocumentDB
De volgende voorbeelden gebruiken de Azure DB die Cosmos-exemplaar dat in het vorige codefragment is gemaakt met de opgegeven sleutels voor alleen-lezen. Het volgende codefragment maakt verbinding met de **airports.codes** verzameling in het account DoctorWho als die eerder is opgegeven en een query uitgevoerd waarmee de luchthaven plaatsen in de staat Washington extraheren.

```python
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Push into list `elements`
elements = list(query)
```

Nadat de query is uitgevoerd **query**, het resultaat is een **query_iterable. QueryIterable** die wordt geconverteerd naar een Python-lijst. Een lijst met Python kan eenvoudig worden geconverteerd naar een Spark-DataFrame met behulp van de volgende code:

```python
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-azure-cosmos-db"></a>Waarom gebruikt de pyDocumentDB Spark verbinding maken met de Azure DB die Cosmos?
Spark bij Azure Cosmos DB verbinding te maken met behulp van pyDocumentDB is doorgaans voor scenario's waarbij:

* Wilt u Python gebruiken.
* U kunt een relatief klein resultaatset uit Azure Cosmos DB met Spark wilt retourneren. Houd er rekening mee dat de onderliggende gegevensset in Azure Cosmos DB kan behoorlijk groot. U zijn filters zijn toegepast, dat wil zeggen, met het predikaat filters uitvoeren in uw Azure Cosmos DB-gegevensbron.  

## <a name="spark-to-azure-cosmos-db-connector"></a>Spark op Azure DB die Cosmos-connector

De Spark op Azure DB die Cosmos-connector maakt gebruik van de [Java SDK voor Azure Documentdb SQL Cosmos](https://github.com/Azure/azure-documentdb-java) en verplaatst van gegevens tussen de Spark worker-knooppunten en Azure Cosmos DB, zoals wordt weergegeven in het volgende diagram:

![Gegevensoverdracht in de Spark op Azure DB die Cosmos-connector](./media/spark-connector/spark-connector.png)

De gegevensstroom is als volgt:

1. Het hoofdknooppunt Spark verbindt met het Azure Cosmos DB gateway-knooppunt ophalen van de partitiekaart. Een gebruiker geeft alleen de Spark en Azure Cosmos DB-verbindingen. Verbindingen met de bijbehorende hoofd- en gateway-knooppunten zijn transparant voor de gebruiker.
2. Deze informatie wordt verstrekt terug naar het hoofdknooppunt van Spark.  Op dit moment moet u kunnen parseren van de query voor het bepalen van de partities en de locaties in Azure Cosmos-database die u nodig hebt voor toegang tot.
3. Deze informatie wordt verzonden naar de Spark worker-knooppunten.
4. De werkrolknooppunten Spark verbinding met de Azure DB die Cosmos-partities rechtstreeks naar de gegevens ophalen en de gegevens terug naar de Spark-partities in Spark worker-knooppunten.

Communicatie tussen Spark en Azure Cosmos DB is aanzienlijk sneller omdat de verplaatsing van gegevens tussen Spark worker-knooppunten en de Azure DB die Cosmos gegevensknooppunten (partities).

### <a name="build-the-spark-to-azure-cosmos-db-connector"></a>De Spark op Azure DB die Cosmos-connector maken
Op dit moment wordt gebruikt in het project connector maven. De connector zonder de afhankelijkheden bouwen, kunt u het volgende uitvoeren:
```
mvn clean package
```
U kunt ook downloaden de nieuwste versies van de JAR uit de [ *releases* map](https://github.com/Azure/azure-cosmosdb-spark/releases).

### <a name="include-the-azure-cosmos-db-spark-jar"></a>De Spark in Azure Cosmos DB JAR opnemen
Voordat u code uitvoert, moet u de Azure Cosmos DB Spark JAR bevatten.  Als u de **spark-shell**, en vervolgens u de JAR opnemen met behulp van kunt de **--potten** optie.  

```bash
spark-shell --master $master --jars /$location/azure-cosmosdb-spark_2.1.0_2.11-1.0.0-uber.jar
```

Als u uitvoeren van de JAR zonder afhankelijkheden wilt, gebruikt u de volgende code:

```bash
spark-shell --master $master --jars /$location/azure-cosmosdb-spark_2.1.0_2.11-1.0.0.jar,/$location/azure-documentdb-1.14.0.jar,/$location/azure-documentdb-rx-0.9.0-rc2.jar,/$location/json-20140107.jar,/$location/rxjava-1.3.0.jar,/$location/rxnetty-0.4.20.jar 
```

Als u een service notebook zoals Azure HDInsight Jupyter-notebook service gebruikt, kunt u de **spark magic** opdrachten:

```
%%configure
{ "name":"Spark-to-Cosmos_DB_Connector", 
  "jars": ["wasb:///example/jars/1.0.0/azure-cosmosdb-spark_2.1.0_2.11-1.0.0.jar", "wasb:///example/jars/1.0.0/azure-documentdb-1.14.0.jar", "wasb:///example/jars/1.0.0/azure-documentdb-rx-0.9.0-rc2.jar", "wasb:///example/jars/1.0.0/json-20140107.jar", "wasb:///example/jars/1.0.0/rxjava-1.3.0.jar", "wasb:///example/jars/1.0.0/rxnetty-0.4.20.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

De **potten** opdracht kunt u de twee potten die nodig zijn voor opnemen **azure-cosmosdb-spark** (zelf en de Java SDK voor Azure Cosmos DB SQL) en wilt uitsluiten **scala-weerspiegelen**zodat deze niet van invloed op de Livy-aanroepen (Jupyter-notebook > Livy > Spark).

### <a name="connect-spark-to-azure-cosmos-db-using-the-connector"></a>Verbinding maken met Spark op Azure Cosmos DB met behulp van de connector
Hoewel het communicatie-transport iets gecompliceerder is, is uitvoeren van een query uit Spark bij Azure Cosmos DB via de connector aanzienlijk sneller.

Het volgende codefragment laat zien hoe het gebruik van de connector in een sessie Spark; Raadpleeg `azure-cosmosdb-spark` [GitHub-repo-](https://github.com/Azure/azure-cosmosdb-spark) voor Python-voorbeelden.

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
val baseConfig = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(baseConfig)
coll.createOrReplaceTempView("c")
```

Zoals vermeld in het codefragment:

- **Azure-cosmosdb-spark** bevat de alle benodigde verbindingsparameters, waaronder de voorkeur locaties. U kunt bijvoorbeeld de volgorde van de gelezen replica en prioriteit.
- Alleen de benodigde bibliotheken importeren en configureren van uw hoofdsleutel en de host voor het maken van de Azure DB die Cosmos-client.

### <a name="execute-spark-queries-via-the-connector"></a>Spark-query's via de connector uitvoeren

Het volgende voorbeeld wordt de Azure DB die Cosmos-exemplaar dat in het vorige codefragment is gemaakt met de opgegeven sleutels voor alleen-lezen. Het volgende codefragment maakt verbinding met de DepartureDelays.flights_pcoll-verzameling (in het account van de DoctorWho zoals eerder opgegeven) en een query uitgevoerd waarmee de vluchtgegevens vertraging van vlucht die zijn afwijkend van Seattle extraheren.

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-azure-cosmos-db-connector-implementation"></a>Waarom de Spark aan de implementatie van Azure DB die Cosmos-connector gebruiken?

Spark verbinding te maken met Azure Cosmos DB via de connector is doorgaans voor scenario's waarbij:

* U wilt gebruiken van Python en/of Scala.
* U hebt een grote hoeveelheid gegevens overdragen tussen Apache Spark en Azure Cosmos DB.

Als u wilt geven u een idee van de query prestatieverschil, Zie de [testruns Query wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="distributed-aggregation-example"></a>Voorbeeld van gedistribueerde samenvoeging
Deze sectie vindt enkele voorbeelden van hoe u gedistribueerde aggregaties en analytics doen kunt met behulp van Apache Spark en Azure Cosmos DB samen. Azure Cosmos DB al ondersteunt aggregaties, die wordt beschreven in de [wereld scale statistische functies met Azure Cosmos DB blog](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/). Hier ziet u hoe u het kunt nemen voor het volgende niveau met Apache Spark.

Deze aggregaties zijn met betrekking tot de [Spark op Azure Cosmos DB Connector notebook](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).

### <a name="connect-to-flights-sample-data"></a>Verbinding maken met de voorbeeldgegevens vlucht
Deze voorbeelden aggregatie toegang tot sommige vlucht prestatiegegevens die zijn opgeslagen in onze **DoctorWho** Azure DB die Cosmos-database. Voor verbinding met het, moet u gebruikmaken van het volgende codefragment:

```
// Import Spark to Azure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect to Azure Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US 2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Met dit fragment gaan we ook een base query die de set gefilterde gegevens overgedragen van Azure DB die Cosmos naar Spark waar de laatste kunt uitvoeren gedistribueerde statistische functies uit te voeren. In dit geval vragen we voor vlucht die van Seattle (SEA afwijken).

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

De volgende resultaten zijn gegenereerd door het uitvoeren van de query's van de Jupyter-notebook-service.  Houd er rekening mee dat de codefragmenten algemene en niet specifiek zijn voor elke service zijn.

### <a name="running-limit-and-count-queries"></a>Actieve LIMIET en het aantal query 's
Net zoals u hebt gebruikt om in SQL/Spark SQL, laten we beginnen met een **LIMIET** query:

![Spark LIMIET query](./media/spark-connector/spark-sql-query.png)

De volgende query is een eenvoudig en snel **aantal** query:

![Spark COUNT-query](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a>GROUP BY-query
In deze volgende set we eenvoudig kunt uitvoeren **GROUP BY** query's op onze Azure DB die Cosmos-database:

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY-query-grafiek](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>UNIEK is, ORDER BY-query
Hier vindt een **DISTINCT, ORDER BY** query:

![Spark GROUP BY-query-grafiek](./media/spark-connector/order-by-query.png)

### <a name="continue-the-flight-data-analysis"></a>Doorgaan met de zwarte data-analyse
U kunt de volgende voorbeeldquery's gebruiken om door te gaan van de analyse van de vluchtgegevens:

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a>Top 5 vertraagde bestemmingen (steden) afwijkend van Seattle
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark bovenste vertragingen grafiek](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>Berekenen gemiddelde vertragingen door bestemming steden afwijkend van Seattle
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark mediaan vertragingen grafiek](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a>Volgende stappen

Als u nog niet gedaan hebt, downloadt u de Spark naar Azure DB die Cosmos-connector in vanuit de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-opslagplaats en bekijk de aanvullende bronnen in de opslagplaats:

* [Voorbeelden van gedistribueerde aggregaties](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Voorbeelden van Scripts en laptops](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

U kunt ook om te controleren de [Apache Spark SQL, DataFrames en gegevenssets handleiding](http://spark.apache.org/docs/latest/sql-programming-guide.html) en de [Apache Spark in Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artikel.
