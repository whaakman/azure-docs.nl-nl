---
title: Statistische bewerkingen op tabellen uit Spark Cassandra-API van Azure Cosmos DB
description: In dit artikel bevat informatie over eenvoudige aggregatiebewerkingen uit te voeren op basis van de Cassandra-API van Azure Cosmos DB-tabellen uit Spark
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 56cd2284fb4bf7dabb280170757c128b8f985433
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54037307"
---
# <a name="aggregate-operations-on-azure-cosmos-db-cassandra-api-tables-from-spark"></a>Statistische bewerkingen op tabellen uit Spark Cassandra-API van Azure Cosmos DB 

Dit artikel wordt beschreven basic aggregatiebewerkingen uit te voeren op basis van de Cassandra-API van Azure Cosmos DB-tabellen uit Spark. 

> [!NOTE]
> Filteren op de server en server-side-aggregatie wordt momenteel niet ondersteund in Azure Cosmos DB Cassandra-API.

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
## <a name="sample-data-generator"></a>Voorbeeld gegevensgenerator

```scala
// Generate a simple dataset containing five values
val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887,11.33),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890,22.45),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892,19.83),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893,14.22),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901,12.25)
).toDF("book_id", "book_author", "book_name", "book_pub_year","book_price")

booksDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000"))
  .save()
```

## <a name="count-operation"></a>Aantal bewerking


### <a name="rdd-api"></a>RDD-API

```scala
sc.cassandraTable("books_ks", "books").count
```

**De uitvoer:**
```
res48: Long = 5
```

### <a name="dataframe-api"></a>Dataframe API

Tellen mee voor dataframes wordt momenteel niet ondersteund.  Het onderstaande voorbeeld ziet u hoe u een aantal dataframe uitvoert na het opslaan van het gegevensframe in het geheugen als tijdelijke oplossing.

Kies een [opslagoptie]( https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#which-storage-level-to-choose) uit de volgende beschikbare opties om te voorkomen dat wordt uitgevoerd in het 'buiten het geheugen' problemen:

* MEMORY_ONLY: Dit is de standaardoptie voor opslag. Winkels RDD als gedeserialiseerde Java-objecten in de JVM. Als de RDD niet in het geheugen past, aantal partities niet in cache en ze worden herberekend op elk gewenst moment telkens wanneer die ze zijn later nodig.

* MEMORY_AND_DISK: Winkels RDD als gedeserialiseerde Java-objecten in de JVM. Als de RDD niet in het geheugen past, slaat u de partities die niet passen op schijf, en indien nodig, lees de hulpbronnen vanaf de locatie die zijn opgeslagen.

* MEMORY_ONLY_SER (Java/Scala): Winkels RDD als geserialiseerde objecten-1-byte-matrix met Java per partitie. Deze optie is de ruimte-efficiëntie in vergelijking met gedeserialiseerde objecten, met name wanneer u een snelle serializer, maar meer CPU-intensieve te lezen.

* MEMORY_AND_DISK_SER (Java/Scala): Deze opslagoptie, zoals MEMORY_ONLY_SER, het enige verschil is dat deze morsen partities die niet in het schijfgeheugen van de in plaats van ze recomputing passen wanneer ze zijn later nodig.

* DISK_ONLY: Slaat de RDD-partities op de schijf alleen.

* MEMORY_ONLY_2, MEMORY_AND_DISK_2...: Hetzelfde als de bovenstaande niveaus repliceert, maar elke partitie op twee clusterknooppunten.

* OFF_HEAP (experimenteel): Net als bij MEMORY_ONLY_SER, maar de gegevens worden opgeslagen in off-heap-geheugen en hiervoor uit heap-geheugen moet vooraf worden ingeschakeld. 

```scala
//Workaround
import org.apache.spark.storage.StorageLevel

//Read from source
val readBooksDF = spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()

//Explain plan
readBooksDF.explain

//Materialize the dataframe
readBooksDF.persist(StorageLevel.MEMORY_ONLY)

//Subsequent execution against this DF hits the cache 
readBooksDF.count

//Persist as temporary view
readBooksDF.createOrReplaceTempView("books_vw")
```

### <a name="sql"></a>SQL

```sql
select * from books_vw;
select count(*) from books_vw where book_pub_year > 1900;
select count(book_id) from books_vw;
select book_author, count(*) as count from books_vw group by book_author;
select count(*) from books_vw;
```

## <a name="average-operation"></a>Bewerking Average

### <a name="rdd-api"></a>RDD-API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Double) => c).mean
```

**De uitvoer:**
```
res24: Double = 16.016000175476073
```

### <a name="dataframe-api"></a>Dataframe API

```scala
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(avg("book_price"))
  .show
```

**De uitvoer:**
```
+------------------+
|   avg(book_price)|
+------------------+
|16.016000175476073|
+------------------+
```

### <a name="sql"></a>SQL

```sql
select avg(book_price) from books_vw;
```
**De uitvoer:**
```
16.016000175476073
```

## <a name="min-operation"></a>Min-bewerking

### <a name="rdd-api"></a>RDD-API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Float) => c).min
```

**De uitvoer:**
```
res31: Float = 11.33
```

### <a name="dataframe-api"></a>Dataframe API

```scala
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_id","book_price")
  .agg(min("book_price"))
  .show
```

**De uitvoer:**
```
+---------------+
|min(book_price)|
+---------------+
|          11.33|
+---------------+
```

### <a name="sql"></a>SQL

```sql
select min(book_price) from books_vw;
```

**De uitvoer:**
```
11.33
```

## <a name="max-operation"></a>Max-bewerking

### <a name="rdd-api"></a>RDD-API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Float) => c).max
```

### <a name="dataframe-api"></a>Dataframe API

```scala 
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(max("book_price"))
  .show
```

**De uitvoer:**
```
+---------------+
|max(book_price)|
+---------------+
|          22.45|
+---------------+
```

### <a name="sql"></a>SQL

```sql
select max(book_price) from books_vw;
```
**De uitvoer:**
```22.45 ```

## <a name="sum-operation"></a>Sum-bewerking

### <a name="rdd-api"></a>RDD-API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Float) => c).sum
```

**De uitvoer:**
```
res46: Double = 80.08000087738037
```

### <a name="dataframe-api"></a>Dataframe API

```scala
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(sum("book_price"))
  .show
```
**De uitvoer:**
```
+-----------------+
|  sum(book_price)|
+-----------------+
|80.08000087738037|
+-----------------+
```

### <a name="sql"></a>SQL

```sql
select sum(book_price) from books_vw;
```

**De uitvoer:**
```
80.08000087738037
```

## <a name="top-or-comparable-operation"></a>Boven- of vergelijkbare bewerking

### <a name="rdd-api"></a>RDD-API

```scala
val readCalcTopRDD = sc.cassandraTable("books_ks", "books").select("book_name","book_price").sortBy(_.getFloat(1), false)
readCalcTopRDD.zipWithIndex.filter(_._2 < 3).collect.foreach(println)
//delivers the first top n items without collecting the rdd to the driver.
```
**De uitvoer:**
```
(CassandraRow{book_name: A sign of four, book_price: 22.45},0)
(CassandraRow{book_name: The adventures of Sherlock Holmes, book_price: 19.83},1)
(CassandraRow{book_name: The memoirs of Sherlock Holmes, book_price: 14.22},2)
readCalcTopRDD: org.apache.spark.rdd.RDD[com.datastax.spark.connector.CassandraRow] = MapPartitionsRDD[430] at sortBy at command-2371828989676374:1
```
### <a name="dataframe-api"></a>Dataframe API

```scala
import org.apache.spark.sql.functions._

val readBooksDF = spark.read.format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load
  .select("book_name","book_price")
  .orderBy(desc("book_price"))
  .limit(3)

//Explain plan
readBooksDF.explain

//Top
readBooksDF.show
```

**De uitvoer:**
```
== Physical Plan ==
TakeOrderedAndProject(limit=3, orderBy=[book_price#1840 DESC NULLS LAST], output=[book_name#1839,book_price#1840])
+- *(1) Scan org.apache.spark.sql.cassandra.CassandraSourceRelation@29cd5f58 [book_name#1839,book_price#1840] PushedFilters: [], ReadSchema: struct<book_name:string,book_price:float>
+--------------------+----------+
|           book_name|book_price|
+--------------------+----------+
|      A sign of four|     22.45|
|The adventures of...|     19.83|
|The memoirs of Sh...|     14.22|
+--------------------+----------+

import org.apache.spark.sql.functions._
readBooksDF: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [book_name: string, book_price: float]
```

### <a name="sql"></a>SQL

```sql
select book_name,book_price from books_vw order by book_price desc limit 3;
```

## <a name="next-steps"></a>Volgende stappen

Om te tabel kopieerbewerkingen uitvoert, Zie:

* [Kopieerbewerkingen tabel](cassandra-spark-table-copy-ops.md)
