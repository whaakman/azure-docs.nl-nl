---
title: Toegang tot Azure Cosmos DB Cassandra-API uit Spark op YARN met HDInsight
description: In dit artikel wordt uitgelegd hoe u werkt met Azure Cosmos DB Cassandra-API van Spark op YARN met HDInsight
services: cosmos-db
author: anagha-microsoft
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: spark-scala
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ankhanol
ms.openlocfilehash: f71f5fa71d685af103bd82b3ccd2a910ab81d90f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962910"
---
# <a name="access-azure-cosmos-db-cassandra-api-from-spark-on-yarn-with-hdinsight"></a>Toegang tot Azure Cosmos DB Cassandra-API uit Spark op YARN met HDInsight

In dit artikel wordt uitgelegd hoe u toegang krijgen tot Azure Cosmos DB Cassandra-API uit Spark op YARN met HDInsight Spark van spark-shell. HDInsight is van Microsoft Hortonworks Hadoop PaaS op Azure die maakt gebruik van opslag van objecten voor HDFS, en is beschikbaar in verschillende versies, met inbegrip van [Spark](../hdinsight/spark/apache-spark-overview.md).  Terwijl de inhoud in dit document wordt verwezen naar HDInsight Spark, is het van toepassing op alle Hadoop-distributies.  

## <a name="prerequisites"></a>Vereisten

* [Inrichten van Azure Cosmos DB Cassandra-API](create-cassandra-dotnet.md#create-a-database-account)

* [Bekijk de basisbeginselen van het verbinden met Azure Cosmos DB Cassandra-API](cassandra-spark-generic.md)

* [Een HDInsight Spark-cluster inrichten](../hdinsight/spark/apache-spark-jupyter-spark-sql.md)

* [Bekijk de voorbeelden van code voor het werken met de Cassandra-API](cassandra-spark-generic.md#next-steps)

* [Cqlsh voor validatie gebruiken als u dus liever](cassandra-spark-generic.md##connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Configuratie van de Cassandra-API in Spark2** -connector van de Datastax voor Cassandra vereist dat de Cassandra-verbinding om details te worden geïnitialiseerd als onderdeel van de Spark-context. Wanneer u een Jupyter-notebook start, wordt de spark-sessie en context zijn al geïnitialiseerd en wordt niet aangeraden om te stoppen en opnieuw initialiseren van de Spark-context, tenzij deze voltooid met elke configuratie ingesteld als onderdeel van de HDInsight standaard Jupyter-notebook Start is. Een tijdelijke oplossing is het toevoegen van de details van de Cassandra-exemplaar aan Ambari, rechtstreeks Spark2-serviceconfiguratie. Dit is een eenmalige activiteit per cluster dat een Spark2 service moet opnieuw worden opgestart.
 
  1. Ga naar Ambari, Spark2 service en klik op de peeringconfiguraties

  2. Vervolgens gaat u naar aangepaste spark2-standaardinstellingen en voegt u de eigenschap met de volgende nieuwe en Spark2-service opnieuw starten:

  ```scala
  spark.cassandra.connection.host=YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com<br>
  spark.cassandra.connection.port=10350<br>
  spark.cassandra.connection.ssl.enabled=true<br>
  spark.cassandra.auth.username=YOUR_COSMOSDB_ACCOUNT_NAME<br>
  spark.cassandra.auth.password=YOUR_COSMOSDB_KEY<br>
  ```

## <a name="access-azure-cosmos-db-cassandra-api-from-spark-shell"></a>Toegang tot Azure Cosmos DB Cassandra-API van Spark-shell

Spark-shell wordt gebruikt voor testen/verkennen.

* Spark-shell met de vereiste maven-afhankelijkheden die compatibel is met Spark-versie van uw cluster starten.

  ```scala
  spark-shell --packages "com.datastax.spark:spark-cassandra-connector_2.11:2.3.0,com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0"
  ```

* Sommige DDL en DML-bewerkingen uitvoeren

  ```scala
  import org.apache.spark.rdd.RDD
  import org.apache.spark.{SparkConf, SparkContext}

  import spark.implicits._
  import org.apache.spark.sql.functions._
  import org.apache.spark.sql.Column
  import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType,LongType,FloatType,DoubleType, TimestampType}
  import org.apache.spark.sql.cassandra._

  //datastax Spark connector
  import com.datastax.spark.connector._
  import com.datastax.spark.connector.cql.CassandraConnector

  //CosmosDB library for multiple retry
  import com.microsoft.azure.cosmosdb.cassandra

  // Specify connection factory for Cassandra
  spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

  // Parallelism and throughput configs
  spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
  spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
  spark.conf.set("spark.cassandra.output.concurrent.writes", "100")
  spark.conf.set("spark.cassandra.concurrent.reads", "512")
  spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
  spark.conf.set("spark.cassandra.connection.keep_alive_ms", "60000000") //Increase this number as needed
  ```

* CRUD-bewerkingen worden uitgevoerd

  ```scala
  //1) Create table if it does not exist
  val cdbConnector = CassandraConnector(sc)
  cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000;"))

  //2) Delete data from potential prior runs
  cdbConnector.withSessionDo(session => session.execute("DELETE FROM books_ks.books WHERE book_id IN ('b00300','b00001','b00023','b00501','b09999','b01001','b00999','b03999','b02999','b000009');"))

  //3) Generate a few rows
  val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887,11.33),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890,22.45),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892,19.83),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893,14.22),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901,12.25)
  ).toDF("book_id", "book_author", "book_name", "book_pub_year","book_price")

  //4) Persist
  booksDF.write.mode("append").format("org.apache.spark.sql.cassandra").options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000")).save()

  //5) Read the data in the table
  spark.read.format("org.apache.spark.sql.cassandra").options(Map( "table" -> "books", "keyspace" -> "books_ks")).load.show
  ```

## <a name="access-azure-cosmos-db-cassandra-api-from-jupyter-notebooks"></a>Toegang tot Azure Cosmos DB Cassandra-API via Jupyter-notebooks

HDInsight Spark wordt geleverd met Jupyter- en Zeppelin notebook. Ze zijn beide web gebaseerde notebook-omgevingen die ondersteuning bieden voor Scala en Python. Notitieblokken zijn ideaal voor interactieve verkennende analyses en samenwerking, maar niet is bedoeld voor operationele/productionized processen.

De volgende Jupyter-notebooks in uw HDInsight Spark-cluster kunnen worden geüpload en klaar voorbeelden voor het werken met Azure Cosmos DB Cassandra API bieden. Lees de eerste notebook `1.0-ReadMe.ipynb` om te controleren van de configuratie van een Spark-service voor het verbinden met de Cassandra-API van Azure Cosmos DB.

Download deze notebooks onder [azure-cosmos-db-cassandra-api-spark-notebooks-jupyter](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-jupyter/blob/master/scala/) naar uw computer.
  
### <a name="how-to-upload"></a>Over het uploaden van:
Wanneer u Jupyter starten, navigeert u naar Scala. Maak eerst een map en klikt u vervolgens de notebooks uploaden naar de map. De knop voor uploaden is in de rechterbovenhoek hoek aan clientzijde.  

### <a name="how-to-run"></a>Hoe u kunt om uit te voeren:
Via de notebooks en elke cel notebook worden opeenvolgend uitgevoerd.  Klik op de knop uitvoeren aan de bovenkant van elke-notebook geïnstrueerd om alle cellen, uitvoeren of shift + enter voor elke cel.

## <a name="access-with-azure-cosmos-db-cassandra-api-from-your-spark-scala-program"></a>Toegang met Azure Cosmos DB Cassandra-API van uw Spark Scala-programma

Voor geautomatiseerde processen in productie, Spark-programma's worden verzonden naar het cluster via [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html).

## <a name="next-steps"></a>Volgende stappen

* [Over het bouwen van een Spark Scala in een IDE-programma en het verzenden naar het HDInsight Spark-cluster via Livy voor uitvoering](../hdinsight/spark/apache-spark-create-standalone-application.md)

* [Verbinding maken met Azure Cosmos DB Cassandra-API van een Spark Scala-programma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)

* [Volledige lijst met voorbeelden van code voor het werken met de Cassandra-API](cassandra-spark-generic.md)
