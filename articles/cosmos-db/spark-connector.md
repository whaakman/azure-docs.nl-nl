---
title: Verbinding maken met Apache Spark op Azure Cosmos DB
description: Meer informatie over de Azure Cosmos DB Spark-connector waarmee u verbinding maken met Apache Spark op Azure Cosmos DB.
author: tknandu
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: ramkris
ms.openlocfilehash: 5a25d0cb8fe1dffbc0a12f6ef88e2adc037a2393
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56982977"
---
# <a name="accelerate-big-data-analytics-by-using-the-apache-spark-to-azure-cosmos-db-connector"></a>Versnel de analyse van big data met behulp van de Apache Spark op Azure Cosmos DB-connector

U kunt uitvoeren [Spark](https://spark.apache.org/) taken met gegevens die zijn opgeslagen in Azure Cosmos DB met behulp van de Cosmos DB Spark-connector. Cosmos kan worden gebruikt voor batchverwerking en de verwerking van stromen, en als een leveringslaag voor toegang met lage latentie.

U kunt de connector met [Azure Databricks](https://azure.microsoft.com/services/databricks) of [Azure HDInsight, waarmee beheerde Spark-clusters in Azure. De volgende tabel ziet Spark voor ondersteunde versies.

| Onderdeel | Versie |
|---------|-------|
| Apache Spark | 2.4.x, 2.3.x 2.2.x en 2.1.x |
| Scala | 2.11 |
| Azure Databricks-runtimeversie | > 3.4 |

> [!WARNING]
> De core (SQL)-API van Azure Cosmos DB biedt ondersteuning voor deze connector.
> Voor Cosmos DB voor MongoDB-API, gebruikt u de [MongoDB Spark-connector](https://docs.mongodb.com/spark-connector/master/).
> Voor Cosmos DB Cassandra-API, gebruikt u de [Cassandra Spark-connector](https://github.com/datastax/spark-cassandra-connector).
>

## <a name="quickstart"></a>Snelstartgids

* Volg de stappen in [aan de slag met de Java SDK](sql-api-async-java-get-started.md) instellen van een Cosmos DB-account en enkele gegevens invullen.
* Volg de stappen in [Azure Databricks aan de slag](https://docs.azuredatabricks.net/getting-started/index.html) voor het instellen van een Azure Databricks-werkruimte en het cluster.
* U kunt nu nieuwe notitieblokken maken en importeren van de Cosmos DB connector-bibliotheek. Ga naar [werken met de Cosmos DB-connector](#bk_working_with_connector) voor meer informatie over het instellen van uw werkruimte.
* De volgende sectie heeft codefragmenten voor het lezen en schrijven met behulp van de connector.

### <a name="reading-from-cosmos-db"></a>Het lezen van het Cosmos DB

Het volgende codefragment laat zien hoe een Spark DataFrame te lezen uit Cosmos DB in PySpark maken.

```python
# Read Configuration
readConfig = {
  "Endpoint" : "https://doctorwho.documents.azure.com:443/",
  "Masterkey" : "YOUR-KEY-HERE",
  "Database" : "DepartureDelays",
  "Collection" : "flights_pcoll",
  "query_custom" : "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
}

# Connect via azure-cosmosdb-spark to create Spark DataFrame
flights = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**readConfig).load()
flights.count()
```

En de dezelfde codefragment in Scala:

```scala
// Import Necessary Libraries
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
val readConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_pcoll",
  "query_custom" -> "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
))

// Connect via azure-cosmosdb-spark to create Spark DataFrame
val flights = spark.read.cosmosDB(readConfig)
flights.count()
```

### <a name="writing-to-cosmos-db"></a>Schrijven naar het Cosmos DB

Het volgende fragment toont hoe u een gegevensframe met Cosmos DB in PySpark schrijft.

```python
# Write configuration
writeConfig = {
 "Endpoint" : "https://doctorwho.documents.azure.com:443/",
 "Masterkey" : "YOUR-KEY-HERE",
 "Database" : "DepartureDelays",
 "Collection" : "flights_fromsea",
 "Upsert" : "true"
}

# Write to Cosmos DB from the flights DataFrame
flights.write.format("com.microsoft.azure.cosmosdb.spark").options(**writeConfig).save()
```

En de dezelfde codefragment in Scala:

```scala
// Configure connection to the sink collection
val writeConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_fromsea",
  "Upsert" : "true"
))

// Upsert the dataframe to Cosmos DB
import org.apache.spark.sql.SaveMode
flights.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Zie meer codefragmenten en end-to-end-voorbeelden [Jupyter](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/notebooks).

## <a name="bk_working_with_connector"></a> Werken met de connector

U kunt de connector uit de gegevensbron in Github maken of de uber JAR-bestanden downloaden van Maven in de onderstaande koppelingen.

| Spark | Scala | Meest recente versie |
|---|---|---|
| 2.4.0 | 2.11 | [azure-cosmosdb-spark_2.4.0_2.11_1.3.5](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.5/jar)
| 2.3.0 | 2.11 | [azure-cosmosdb-spark_2.3.0_2.11_1.3.3](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.3.0_2.11/1.3.3/jar)
| 2.2.0 | 2.11 | [azure-cosmosdb-spark_2.2.0_2.11_1.1.1](https://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-cosmosdb-spark_2.2.0_2.11%7C1.1.1%7Cjar)
| 2.1.0 | 2.11 | [azure-cosmosdb-spark_2.1.0_2.11_1.2.2](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.1.0_2.11/1.2.2/jar)

### <a name="using-databricks-notebooks"></a>Met behulp van Databricks-notebooks

Maken van een bibliotheek met binnen uw Databricks-werkruimte door de instructies in de Azure Databricks-gids te volgen > [gebruik van de Azure Cosmos DB Spark-connector](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

> [!NOTE]
> Let op: de **gebruik van de Spark-Connector van Azure Cosmos DB** pagina is momenteel niet up-to-date. In plaats van de zes afzonderlijke JAR-bestanden in zes verschillende bibliotheken gedownload, kunt u de jar uber downloaden van maven op https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.5/jar) en deze een jar/bibliotheek installeren.
> 

### <a name="using-spark-cli"></a>Met behulp van spark-cli

Werken met de connector met behulp van de spark-cli (dat wil zeggen, `spark-shell`, `pyspark`, `spark-submit`), kunt u de `--packages` parameter met een van de connector [maven-coördinaten](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11).

```sh
spark-shell --master yarn --packages "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.3.5"

```

### <a name="using-jupyter-notebooks"></a>Met behulp van Jupyter notebooks

Als u een Jupyter-notebooks in HDInsight gebruikt, kunt u spark-magic `%%configure` cel om op te geven van de maven-coördinaten van de connector.

```python
{ "name":"Spark-to-Cosmos_DB_Connector",
  "conf": {
    "spark.jars.packages": "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.3.5",
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
   ...
}
```

> Houd er rekening mee, de opname van de `spark.jars.excludes` is specifiek voor het verwijderen van mogelijke conflicten tussen de connector, Apache Spark en Livy.

### <a name="build-the-connector"></a>De connector bouwen

Op dit moment gebruikmaakt van dit project connector `maven` , zodat als u wilt maken zonder afhankelijkheden, u kunt uitvoeren:

```sh
mvn clean package
```

## <a name="working-with-our-samples"></a>Werken met onze voorbeelden

De [Cosmos DB Spark GitHub-opslagplaats](https://github.com/Azure/azure-cosmosdb-spark) heeft het volgende voorbeeldnotitieblokken en -scripts die u kunt proberen.

* **Prestatiegegevens omtrent op tijd vlucht met Spark en Cosmos DB (Haarlem)** [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.html): Spark verbinden met Cosmos DB met behulp van HDInsight Jupyter notebook-service te presenteren Spark SQL, GraphFrames en voorspellen van vertragingen van vluchten vertragingen met ML-pijplijnen.
* **[Spark verbinding te maken met Cosmos DB-Wijzigingenfeed](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark%2Band%2BCosmos%2BDB%2BChange%2BFeed.ipynb)**: Een snelle showcase op verbinding maken tussen Spark en Cosmos DB-Wijzigingenfeed.
* **Twitter-bron met Apache Spark en Azure Cosmos DB-Wijzigingenfeed**: [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.html)
* **Met Apache Spark op query Cosmos DB grafieken**: [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.html)
* **[Azure Databricks verbinden met Azure Cosmos DB](https://docs.databricks.com/spark/latest/data-sources/azure/cosmosdb-connector.html)**  met behulp van `azure-cosmosdb-spark`.  Gekoppelde hier is ook een Azure Databricks-versie van de [prestatiegegevens omtrent op tijd Flight notebook](https://github.com/dennyglee/databricks/tree/master/notebooks/Users/denny%40databricks.com/azure-databricks).
* **[Lambda-architectuur met Azure Cosmos DB en HDInsight (Apache Spark)](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/readme.md)**: U kunt de operationele overhead van het onderhouden van big data-pijplijnen met Cosmos DB- en Spark kunt beperken.

## <a name="more-information"></a>Meer informatie

We hebben meer informatie de `azure-cosmosdb-spark` [wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki) met inbegrip van:

* [Azure Cosmos DB-gebruikershandleiding voor Spark-Connector](https://github.com/Azure/azure-documentdb-spark/wiki/Azure-Cosmos-DB-Spark-Connector-User-Guide)
* [Aggregaties voorbeelden](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)

### <a name="configuration-and-setup"></a>Configuratie en installatie

* [Configuratie van een Spark-Connector](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuration-references)
* [Spark-Connector-instellingen voor Cosmos DB](https://github.com/Azure/azure-documentdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup) (In behandeling)
* [Power BI directe Query bij Azure Cosmos DB via Apache Spark (HDI) configureren](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuring-Power-BI-Direct-Query-to-Azure-Cosmos-DB-via-Apache-Spark-(HDI))

### <a name="troubleshooting"></a>Problemen oplossen

* [Met behulp van Cosmos DB statistische functies](https://github.com/Azure/azure-documentdb-spark/wiki/Troubleshooting:-Using-Cosmos-DB-Aggregates)
* [Bekende problemen](https://github.com/Azure/azure-cosmosdb-spark/wiki/Known-Issues)

### <a name="performance"></a>Prestaties

* [Tips voor betere prestaties](https://github.com/Azure/azure-cosmosdb-spark/wiki/Performance-tips)
* [Testuitvoeringen voor query](https://github.com/Azure/azure-documentdb-spark/wiki/Query-Test-Runs)
* [Testuitvoeringen voor schrijven](https://github.com/Azure/azure-cosmosdb-spark/wiki/Writing-Test-Runs)

### <a name="change-feed"></a>Feed wijzigen

* [Stream verwerken van wijzigingen met behulp van Azure Cosmos DB-Wijzigingenfeed en Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)
* [Demo's van Feed wijzigen](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Structured Stream demo 's](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)

### <a name="monitoring"></a>Bewaking

* [Bewaking met application insights voor Spark-taken](https://github.com/Azure/azure-cosmosdb-spark/tree/2.3/samples/monitoring)

## <a name="next-steps"></a>Volgende stappen

Als u niet hebt gedaan, downloadt u de Spark op Azure Cosmos DB-connector van de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-opslagplaats. Bekijk de volgende aanvullende resources in de opslagplaats:

* [Aggregaties voorbeelden](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Voorbeeldscripts en -laptops](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

U kunt ook om te controleren de [Apache Spark SQL en DataFrames gegevenssets handleiding](https://spark.apache.org/docs/latest/sql-programming-guide.html), en de [Apache Spark in Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artikel.
