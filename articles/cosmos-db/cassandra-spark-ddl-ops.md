---
title: DDL-bewerkingen in Azure Cosmos DB Cassandra-API van Spark
description: Dit artikel wordt uitgelegd keyspace en een tabel DDL-bewerkingen op Azure Cosmos DB Cassandra-API van Spark.
services: cosmos-db
author: anagha-microsoft
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: spark-scala
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ankhanol
ms.openlocfilehash: a799d85cc27575badda6892ba7baf68ca1eb1dfb
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226072"
---
# <a name="ddl-operations-in-azure-cosmos-db-cassandra-api-from-spark"></a>DDL-bewerkingen in Azure Cosmos DB Cassandra-API van Spark

Dit artikel wordt uitgelegd keyspace en een tabel DDL-bewerkingen op Azure Cosmos DB Cassandra-API van Spark.

## <a name="cassandra-api-related-configuration"></a>Cassandra-API-gerelateerde configuratie 

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

## <a name="keyspace-ddl-operations"></a>Keyspace DDL-bewerkingen

### <a name="create-a-keyspace"></a>Een keyspace maken

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

// Create keyspace
cdbConnector.withSessionDo(session => session.execute("CREATE KEYSPACE IF NOT EXISTS books_ks WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 } "))
```

#### <a name="validate-in-cqlsh"></a>In cqlsh valideren

Voer de volgende opdracht in cqlsh en ziet u de keyspace die u eerder hebt gemaakt.

```bash
DESCRIBE keyspaces;
```

### <a name="drop-a-keyspace"></a>Een keyspace verwijderen

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP KEYSPACE books_ks"))
```

#### <a name="validate-in-cqlsh"></a>In cqlsh valideren

```bash
DESCRIBE keyspaces;
```
## <a name="table-ddl-operations"></a>Tabel-DDL-bewerkingen

**Overwegingen met betrekking tot:**  

- Doorvoer kan worden toegewezen op het tabelniveau van de met behulp van de instructie create table.  
- Een partitiesleutel kunt 10 GB aan gegevens opslaan.  
- Een record kan maximaal 2 MB aan gegevens opslaan.  
- Een partitiesleutelbereik kan meerdere partitiesleutels opslaan.

### <a name="create-a-table"></a>Een tabel maken

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))
```

#### <a name="validate-in-cqlsh"></a>In cqlsh valideren

Voer de volgende opdracht in cqlsh en ziet u de tabel met de naam ' boeken: 

```bash
USE books_ks;
DESCRIBE books;
```

Ingerichte doorvoer en de standaard TTL-waarden worden niet weergegeven in de uitvoer van de vorige opdracht, kunt u deze waarden ophalen uit de portal.

### <a name="alter-table"></a>De tabel wijzigen

U kunt de volgende waarden wijzigen met behulp van de opdracht alter tabel:

* ingerichte doorvoer 
* Time-to-live-waarde
<br>Kolom wijzigingen worden momenteel niet ondersteund.

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("ALTER TABLE books_ks.books WITH cosmosdb_provisioned_throughput=8000, WITH default_time_to_live=0;"))
```

### <a name="drop-table"></a>Tabel verwijderen

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))
```

#### <a name="validate-in-cqlsh"></a>In cqlsh valideren

Voer de volgende opdracht in cqlsh en ziet u de tabel 'boeken' is niet meer beschikbaar:

```bash
USE books_ks;
DESCRIBE tables;
```

## <a name="next-steps"></a>Volgende stappen

Na het maken van de keyspace en de tabel, gaat u verder met de volgende artikelen voor CRUD-bewerkingen en nog veel meer:
 
* [Bewerkingen maken/invoegen](cassandra-spark-create-ops.md)  
* [leesbewerkingen](cassandra-spark-read-ops.md)  
* [Upsert-bewerkingen](cassandra-spark-upsert-ops.md)  
* [Verwijderbewerkingen](cassandra-spark-delete-ops.md)  
* [Aggregatiebewerkingen uit te voeren](cassandra-spark-aggregation-ops.md)  
* [Kopieerbewerkingen tabel](cassandra-spark-table-copy-ops.md)  
