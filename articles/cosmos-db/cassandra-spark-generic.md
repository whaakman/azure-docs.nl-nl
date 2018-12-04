---
title: Werken met Azure Cosmos DB Cassandra-API van Spark
description: Dit artikel is de hoofdpagina voor Cosmos DB Cassandra-API-integratie van Spark.
services: cosmos-db
author: anagha-microsoft
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: spark-scala
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ankhanol
ms.openlocfilehash: cb58ad60501be43ff4da2db29ab3ad3dfee9aad1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847130"
---
# <a name="connect-to-azure-cosmos-db-cassandra-api-from-spark"></a>Verbinding maken met Azure Cosmos DB Cassandra-API van Spark

In dit artikel is een tussen een reeks artikelen over Cassandra-API van Azure Cosmos DB-integratie van Spark. De artikelen betrekking op connectiviteit, Data Definition Language-bewerkingen, gegevens manipuleren Language(DML) basisbewerkingen en geavanceerde integratie van Spark de Cassandra-API van Azure Cosmos DB. 

## <a name="prerequisites"></a>Vereisten
* [Richt een Cassandra-API van Azure Cosmos DB-account.](create-cassandra-dotnet.md#create-a-database-account)

* Inrichten van uw keuze van Spark-omgeving [[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) | [Azure HDInsight-Spark](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql) | Anderen].

## <a name="dependencies-for-connectivity"></a>Afhankelijkheden voor connectiviteit
* **Spark-connector voor Cassandra:** Spark-connector wordt gebruikt voor verbinding met Azure Cosmos DB Cassandra-API.  Bepalen welke en de versie van de connector zich bevindt [Maven central]( https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) die compatibel is met de Spark- en Scala-versies van uw Spark-omgeving.

* **Azure Cosmos DB-helper-bibliotheek voor Cassandra-API:** naast de Spark-connector, moet u een ander bibliotheektype bespreken [azure-cosmos-cassandra-spark-helper]( https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) van Azure Cosmos DB. Deze bibliotheek bevat klassen beleid van aangepaste verbinding factory en probeer het opnieuw.

  Het beleid voor opnieuw proberen in Azure Cosmos DB is geconfigureerd voor het afhandelen van HTTP-status code 429 ("tarief grote aanvraag') uitzonderingen. De Cassandra-API van Azure Cosmos DB vertaalt deze uitzonderingen overbelaste fouten op het systeemeigen Cassandra-protocol en u kunt het opnieuw met de back-laag. Omdat Azure Cosmos DB ingerichte doorvoer model gebruikt, wordt aanvraag beperkende uitzonderingen optreden wanneer de Inkomend/uitgaand verkeer toename van de tarieven. Het beleid voor opnieuw proberen beschermt uw spark-taken op basis van gegevens pieken die tijdelijk groter is dan de doorvoer die is toegewezen voor uw verzameling.

  > [!NOTE] 
  > Het beleid voor opnieuw proberen kan uw spark-taken op basis van alleen tijdelijke pieken beschermen. Als u niet voldoende ru's vereist voor het uitvoeren van uw workload hebt geconfigureerd, klikt u vervolgens het beleid voor opnieuw proberen is niet van toepassing en het beleid voor opnieuw proberen klasse opnieuw wordt gegenereerd van de uitzondering.

* **Details voor Azure Cosmos DB-account verbinding:** Cassandra-API in uw Azure-accountnaam, -eindpunt-account en -sleutel.
    
## <a name="spark-connector-throughput-configuration-parameters"></a>Parameters voor Spark-connector voor configuratie van doorvoer

De volgende tabel bevat configuratie-parameters voor Azure Cosmos DB Cassandra-API-specifieke doorvoer is geleverd door de connector. Zie voor een gedetailleerde lijst van alle configuratieparameters, [naslaginformatie voor configuratie](https://github.com/datastax/spark-cassandra-connector/blob/master/doc/reference.md) pagina van de Spark Cassandra Connector GitHub-opslagplaats.

| **De naam van eigenschap** | **Standaardwaarde** | **Beschrijving** |
|---------|---------|---------|
| Spark.cassandra.output.batch.Size.Rows |  1 |Het aantal rijen per één batch. Stel deze parameter in op 1. Deze parameter wordt gebruikt voor een hogere doorvoer voor zware workloads. |
| Spark.cassandra.Connection.connections_per_executor_max  | Geen | Maximum aantal verbindingen per knooppunt per executor. 10 * n is gelijk aan 10 verbindingen per knooppunt in een n-knooppunt Cassandra-cluster. Dus als u 5 verbindingen per knooppunt per executor voor een 5 knooppunt Cassandra-cluster nodig hebt, moet klikt u vervolgens stelt u deze configuratie op 25. Deze waarde op basis van de mate van parallelle uitvoering of het nummer van uw spark-taken zijn geconfigureerd voor uitvoerders wijzigen.   |
| Spark.cassandra.output.concurrent.Writes  |  100 | Hiermee definieert u het aantal parallelle schrijfbewerkingen die per executor kunnen optreden. Omdat u "batch.size.rows" ingesteld op 1, zorg ervoor dat deze waarde dienovereenkomstig opschalen. Wijzig deze waarde op basis van de mate van parallelle uitvoering of de doorvoer die u wilt bereiken voor uw workload. |
| Spark.cassandra.concurrent.reads |  512 | Hiermee definieert u het aantal parallelle leesbewerkingen die per executor kunnen optreden. Deze waarde op basis van de mate van parallelle uitvoering of de doorvoer die u wilt bereiken voor uw workload wijzigen  |
| Spark.cassandra.output.throughput_mb_per_sec  | Geen | De totale schrijven-doorvoer per executor definieert. Deze parameter kan worden gebruikt als een hoofdletters beperken voor de doorvoer van uw spark-taak en baseren op de ingerichte doorvoer van uw Cosmos DB-verzameling.   |
| Spark.cassandra.Input.reads_per_sec| Geen   | De totale lezen doorvoer per executor definieert. Deze parameter kan worden gebruikt als een hoofdletters beperken voor de doorvoer van uw spark-taak en baseren op de ingerichte doorvoer van uw Cosmos DB-verzameling.  |
| Spark.cassandra.output.batch.Grouping.buffer.Size |  1000  | Definieert het aantal batches per één spark-taak die in het geheugen kunnen worden opgeslagen voordat ze worden verzonden met Cassandra-API |
| Spark.cassandra.Connection.keep_alive_ms | 60000 | Hiermee definieert u de periode tot welke niet-gebruikte verbindingen beschikbaar zijn. | 

Pas de doorvoer en de mate van parallelle uitvoering van deze parameters op basis van de werkbelasting die u kunt verwachten bij uw spark-taken en de doorvoer die u hebt ingericht voor uw Cosmos DB-account.

## <a name="connecting-to-azure-cosmos-db-cassandra-api-from-spark"></a>Verbinding maken met Azure Cosmos DB Cassandra-API van Spark

### <a name="cqlsh"></a>cqlsh
De volgende opdrachten gedetailleerd beschreven hoe u verbinding maakt met Azure cosmos DB Cassandra-API van cqlsh.  Dit is handig voor als u door de voorbeelden in Spark-validatie.<br>
**Vanaf Linux/Unix/Mac:**

```bash
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false
cqlsh.py YOUR-COSMOSDB-ACCOUNT-NAME.cassandra.cosmosdb.azure.com 10350 -u YOUR-COSMOSDB-ACCOUNT-NAME -p YOUR-COSMOSDB-ACCOUNT-KEY --ssl
```

### <a name="1--azure-databricks"></a>1.  Azure Databricks
Het onderstaande artikel bevat informatie over Azure Databricks-cluster inrichten, configuratie van het cluster voor het verbinden met de Cassandra-API van Azure Cosmos DB en verschillende voorbeeldnotitieblokken die betrekking hebben op DDL-bewerkingen, DML-bewerkingen en meer.<BR>
[Werken met Azure Cosmos DB Cassandra-API van Azure databricks](cassandra-spark-databricks.md)<BR>
  
### <a name="2--azure-hdinsight-spark"></a>2.  Azure HDInsight-Spark
Het onderstaande artikel bevat informatie over HDinsight Spark-service, wordt ingericht, configuratie van het cluster voor het verbinden met de Cassandra-API van Azure Cosmos DB en verschillende voorbeeldnotitieblokken die betrekking hebben op DDL-bewerkingen, DML-bewerkingen en meer.<BR>
[Werken met Azure Cosmos DB Cassandra-API van Azure HDInsight-Spark](cassandra-spark-hdinsight.md)
 
### <a name="3--spark-environment-in-general"></a>3.  Spark-omgeving in het algemeen
Terwijl de bovenstaande secties specifiek voor Azure Spark op basis van PaaS-services zijn, wordt een algemene Spark-omgeving beschreven.  Connector-afhankelijkheden, invoer en configuratie van een Spark-sessie worden hieronder beschreven. De sectie 'Volgende stappen' worden codevoorbeelden voor DDL-bewerkingen, DML-bewerkingen en meer.  

#### <a name="connector-dependencies"></a>Connector-afhankelijkheden:

1. Toevoegen van de maven-coördinaten aan de [Cassandra-connector voor Spark](cassandra-spark-generic.md#dependencies-for-connectivity)
2. Toevoegen van de maven-coördinaten voor de [Azure Cosmos DB-hulpbibliotheek](cassandra-spark-generic.md#dependencies-for-connectivity) voor Cassandra-API

#### <a name="imports"></a>Invoer:

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra
```

#### <a name="spark-session-configuration"></a>Configuratie van een Spark-sessie:

```scala
//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related. You can adjust the values as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen illustratie van Spark-integratie met Azure Cosmos DB Cassandra-API. 
 
* [DDL-bewerkingen](cassandra-spark-ddl-ops.md)
* [Bewerkingen maken/invoegen](cassandra-spark-create-ops.md)
* [leesbewerkingen](cassandra-spark-read-ops.md)
* [Upsert-bewerkingen](cassandra-spark-upsert-ops.md)
* [Verwijderbewerkingen](cassandra-spark-delete-ops.md)
* [Aggregatiebewerkingen uit te voeren](cassandra-spark-aggregation-ops.md)
* [Kopieerbewerkingen tabel](cassandra-spark-table-copy-ops.md)
