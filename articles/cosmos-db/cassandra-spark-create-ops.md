---
title: Gegevens in Azure Cosmos DB Cassandra-API van Spark maken/invoegen
description: Dit artikel wordt uitgelegd hoe u voorbeeldgegevens ingevoegd in Cassandra-API van Azure Cosmos DB-tabellen
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: aea646e7a390d5b53f0d4b388cfecd0c80fb19da
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54036610"
---
# <a name="createinsert-data-into-azure-cosmos-db-cassandra-api-from-spark"></a>Gegevens in Azure Cosmos DB Cassandra-API van Spark maken/invoegen
 
In dit artikel wordt beschreven hoe u voorbeeldgegevens ingevoegd in een tabel in Azure Cosmos DB Cassandra-API van Spark.

## <a name="cassandra-api-configuration"></a>Configuratie van de Cassandra-API

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")
//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```
## <a name="dataframe-api"></a>Dataframe API

### <a name="create-a-dataframe-with-sample-data"></a>Een Dataframe maken met voorbeeldgegevens

```scala
// Generate a dataframe containing five records
val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
).toDF("book_id", "book_author", "book_name", "book_pub_year")

//Review schema
booksDF.printSchema

//Print
booksDF.show
```

> [!NOTE]
> 'Maak if not exists'-functionaliteit, op het rijniveau van een, wordt nog niet ondersteund.

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Bewaard in een Azure Cosmos DB Cassandra-API

Bij het opslaan van gegevens, kunt u ook instellen time-to-live en consistentie beleidsinstellingen zoals wordt weergegeven in het volgende voorbeeld:

```scala
//Persist
booksDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000"))
  .save()
```

> [!NOTE]
> De TTL op kolomniveau wordt nog niet ondersteund.

#### <a name="validate-in-cqlsh"></a>In cqlsh valideren

```sql
use books_ks;
select * from books;
```

## <a name="resilient-distributed-database-rdd-api"></a>Flexibele gedistribueerde Database (RDD)-API

### <a name="create-a-rdd-with-sample-data"></a>Een RDD maken met voorbeeldgegevens
```scala
//Delete records created in the previous section 
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("delete from books_ks.books where book_id in ('b00300','b00001','b00023','b00501','b09999','b01001','b00999','b03999','b02999');"))

//Create RDD
val booksRDD = sc.parallelize(Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
))

//Review
booksRDD.take(2).foreach(println)
```

> [!NOTE]
> Maken als deze niet bestaat functionaliteit is nog niet ondersteund.

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Bewaard in een Azure Cosmos DB Cassandra-API

Bij het opslaan van gegevens met Cassandra-API, kunt u ook instellen time-to-live en consistentie beleidsinstellingen zoals wordt weergegeven in het volgende voorbeeld:

```scala
import com.datastax.spark.connector.writer._

//Persist
booksRDD.saveToCassandra("books_ks", "books", SomeColumns("book_id", "book_author", "book_name", "book_pub_year"),writeConf = WriteConf(ttl = TTLOption.constant(900000),consistencyLevel = ConsistencyLevel.ALL))
```

#### <a name="validate-in-cqlsh"></a>In cqlsh valideren

```sql
use books_ks;
select * from books;
```

## <a name="next-steps"></a>Volgende stappen

Nadat de gegevens in de Cassandra-API van Azure Cosmos DB-tabel invoegen, gaat u verder met de volgende artikelen voor andere bewerkingen uitvoeren op de gegevens die zijn opgeslagen in Cosmos DB Cassandra-API:
 
* [leesbewerkingen](cassandra-spark-read-ops.md)
* [Upsert-bewerkingen](cassandra-spark-upsert-ops.md)
* [Verwijderbewerkingen](cassandra-spark-delete-ops.md)
* [Aggregatiebewerkingen uit te voeren](cassandra-spark-aggregation-ops.md)
* [Kopieerbewerkingen tabel](cassandra-spark-table-copy-ops.md)

