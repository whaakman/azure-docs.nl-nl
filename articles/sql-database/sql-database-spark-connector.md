---
title: Spark-Connector met Azure SQL Database en SQL Server | Microsoft Docs
description: Meer informatie over het gebruik van de Spark-Connector voor Azure SQL Database en SQL Server
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: 8e531de34302ef8aee571c960955d33a4832aa11
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013710"
---
# <a name="accelerate-real-time-big-data-analytics-with-spark-connector-for-azure-sql-database-and-sql-server"></a>Versnel de analyse van realtime big data met Spark-connector voor Azure SQL Database en SQL Server

De Spark-connector voor Azure SQL Database en SQL Server kunnen SQL-databases, met inbegrip van Azure SQL Database en SQL Server, om te fungeren als invoergegevens bron- of uitvoer gegevenssink voor Spark-taken. Hiermee kunt u gebruikmaken van realtime transactionele gegevens in big data-analyses en resultaten voor ad-hoc-query's of rapporten behouden. Deze connector is vergeleken met de ingebouwde JDBC-connector, biedt de mogelijkheid bulksgewijs invoegen gegevens in SQL-databases. Het kan leveren betere prestaties dan per rij invoegen met 10 x 20 x snellere prestaties. De Spark-connector voor Azure SQL Database en SQL Server biedt ook ondersteuning voor AAD-verificatie. Hiermee kunt u veilig verbinding maken met uw Azure SQL database van Azure Databricks met behulp van uw AAD-account. Het biedt vergelijkbare interfaces met de ingebouwde JDBC-connector. Het is gemakkelijk om te migreren van uw bestaande Spark-taken voor het gebruik van deze nieuwe connector.

## <a name="download"></a>Downloaden
Om te beginnen, downloadt u de Spark naar SQL DB-connector van de [opslagplaats voor azure-sqldb-spark](https://github.com/Azure/azure-sqldb-spark) op GitHub.

## <a name="official-supported-versions"></a>Officieel ondersteunde versies

| Onderdeel                            |Versie                  |
| :----------------------------------- | :---------------------- |
| Apache Spark                         |2.0.2 of hoger           |
| Scala                                |2,10 of hoger            |
| Microsoft JDBC-stuurprogramma voor SQL Server |6.2 of hoger             |
| Microsoft SQL Server                 |SQL Server 2008 of hoger |
| Azure SQL Database                   |Ondersteund                |

De Spark-connector voor Azure SQL Database en SQL Server gebruikmaakt van het Microsoft JDBC-stuurprogramma voor SQL Server om gegevens te verplaatsen tussen Spark worker-knooppunten en SQL-databases:
 
De gegevensstroom is als volgt:
1. Het Spark-hoofdknooppunt maakt verbinding met SQL Server of Azure SQL Database en gegevens worden geladen vanaf een bepaalde tabel of met behulp van een specifieke SQL-query
2. Het hoofdknooppunt van Spark gegevens op de worker-knooppunten voor transformatie worden verdeeld. 
3. De Worker-knooppunt maakt verbinding met SQL Server of Azure SQL Database en schrijft gegevens naar de database. Gebruiker kan kiezen voor het gebruik van per rij invoegen of bulk insert.

Het volgende diagram illustreert de gegevensstroom.

   ![architectuur](./media/sql-database-spark-connector/architecture.png)

### <a name="build-the-spark-to-sql-db-connector"></a>De Spark SQL-database-connector maken
Op dit moment het connector-project maakt gebruik van maven. Voor het bouwen van de connector zonder afhankelijkheden, kunt u het volgende uitvoeren:

- mvn opschonen pakket
- De nieuwste versies van de JAR downloaden vanaf de releasemap
- De SQL DB Spark JAR opnemen

## <a name="connect-spark-to-sql-db-using-the-connector"></a>Verbinding maken met Spark SQL-database met behulp van de connector
U kunt verbinding maken met Azure SQL Database of SQL Server vanuit een Spark-taken, lezen of schrijven van gegevens. U kunt ook een DML- of DDL-query uitvoeren in een Azure SQL-database of SQL Server-database.

### <a name="read-data-from-azure-sql-database-or-sql-server"></a>Lezen van gegevens van Azure SQL Database of SQL Server

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "dbTable"        -> "dbo.Clients"
  "user"           -> "username",
  "password"       -> "*********",
  "connectTimeout" -> "5", //seconds
  "queryTimeout"   -> "5"  //seconds
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```
### <a name="reading-data-from-azure-sql-database-or-sql-server-with-specified-sql-query"></a>Lezen van gegevens uit Azure SQL Database of SQL Server met de opgegeven SQL-query
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "queryCustom"  -> "SELECT TOP 100 * FROM dbo.Clients WHERE PostalCode = 98074" //Sql query
  "user"         -> "username",
  "password"     -> "*********",
))

//Read all data in table dbo.Clients
val collection = sqlContext.read.sqlDb(config)
collection.show()
```

### <a name="write-data-to-azure-sql-database-or-sql-server"></a>Schrijven van gegevens naar Azure SQL Database of SQL Server
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._
 
// Aquire a DataFrame collection (val collection)

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "dbTable"      -> "dbo.Clients"
  "user"         -> "username",
  "password"     -> "*********"
))

import org.apache.spark.sql.SaveMode
collection.write.mode(SaveMode.Append).sqlDB(config)
```

### <a name="run-dml-or-ddl-query-in-azure-sql-database-or-sql-server"></a>DML- of DDL-query uitvoeren in Azure SQL Database of SQL Server
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.query._
val query = """
              |UPDATE Customers
              |SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
              |WHERE CustomerID = 1;
            """.stripMargin

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "user"         -> "username",
  "password"     -> "*********",
  "queryCustom"  -> query
))

sqlContext.SqlDBQuery(config)
```

## <a name="connect-spark-to-azure-sql-database-using-aad-authentication"></a>Verbinding maken met Spark in Azure SQL-Database met behulp van AAD-verificatie
U kunt verbinding maken met Azure SQL Database met behulp van Azure Active Directory (AAD)-verificatie. AAD-verificatie gebruiken voor het centraal beheren van identiteiten van databasegebruikers en als alternatief voor SQL Server-verificatie.
### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Verbinding maken met behulp van ActiveDirectoryPassword-verificatiemodus
#### <a name="setup-requirement"></a>Vereiste installatie
Als u de verificatiemodus ActiveDirectoryPassword gebruikt, moet u voor het downloaden van [azure Active Directory-bibliotheek-voor-java-](https://github.com/AzureAD/azure-activedirectory-library-for-java) en de bijbehorende afhankelijkheden, en deze opnemen in de Java build path.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "user"           -> "username ",
  "password"       -> "*********",
  "authentication" -> "ActiveDirectoryPassword",
  "encrypt"        -> "true"
))

val collection = sqlContext.read.SqlDB(config)
collection.show()
```

### <a name="connecting-using-access-token"></a>Verbinding maken met behulp van toegangstoken
#### <a name="setup-requirement"></a>Vereiste installatie
Als u de toegangsmodus voor verificatie op basis van tokens gebruikt, moet u voor het downloaden van [azure Active Directory-bibliotheek-voor-java-](https://github.com/AzureAD/azure-activedirectory-library-for-java) en de bijbehorende afhankelijkheden, en deze opnemen in de Java build path.

Zie [gebruik Azure Active Directory-verificatie voor verificatie met SQL Database](sql-database-aad-authentication.md) voor informatie over het toegangstoken voor uw Azure SQL database ophalen.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"                   -> "mysqlserver.database.windows.net",
  "databaseName"          -> "MyDatabase",
  "accessToken"           -> "access_token ",
  "hostNameInCertificate" -> "*.database.windows.net",
  "encrypt"               -> "true"
))

val collection = sqlContext.read.SqlDB(config)
collection.show()
```

## <a name="write-data-to-azure-sql-database-or-sql-server-using-bulk-insert"></a>Schrijven van gegevens naar Azure SQL database of SQL Server met behulp van Bulk Insert
De traditionele jdbc-connector schrijft gegevens naar Azure SQL database of SQL Server met behulp van per rij invoegen. U kunt Spark naar SQL DB-connector gebruiken om gegevens te schrijven met SQL-database met behulp van bulksgewijs invoegen. Dit verbetert de prestaties schrijven aanzienlijk bij het laden van grote gegevenssets of het laden van gegevens in tabellen waarbij een columnstore-index wordt gebruikt.

```scala
import com.microsoft.azure.sqldb.spark.bulkcopy.BulkCopyMetadata
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

/** 
  Add column Metadata.
  If not specified, metadata is automatically added
  from the destination table, which may suffer performance.
*/
var bulkCopyMetadata = new BulkCopyMetadata
bulkCopyMetadata.addColumnMetadata(1, "Title", java.sql.Types.NVARCHAR, 128, 0)
bulkCopyMetadata.addColumnMetadata(2, "FirstName", java.sql.Types.NVARCHAR, 50, 0)
bulkCopyMetadata.addColumnMetadata(3, "LastName", java.sql.Types.NVARCHAR, 50, 0)

val bulkCopyConfig = Config(Map(
  "url"               -> "mysqlserver.database.windows.net",
  "databaseName"      -> "MyDatabase",
  "user"              -> "username",
  "password"          -> "*********",
  "databaseName"      -> "zeqisql",
  "dbTable"           -> "dbo.Clients",
  "bulkCopyBatchSize" -> "2500",
  "bulkCopyTableLock" -> "true",
  "bulkCopyTimeout"   -> "600"
))

df.bulkCopyToSqlDB(bulkCopyConfig, bulkCopyMetadata)
//df.bulkCopyToSqlDB(bulkCopyConfig) if no metadata is specified.
```

## <a name="next-steps"></a>Volgende stappen
Als u niet hebt gedaan, downloadt u de Spark-connector voor Azure SQL Database en SQL-Server van [GitHub-opslagplaats voor azure-sqldb-spark](https://github.com/Azure/azure-sqldb-spark) en Verken de extra resources in de opslagplaats:

-   [Voorbeeld van Azure Databricks-notebooks](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Voorbeelden van scripts (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

U kunt ook om te controleren de [Apache Spark SQL en DataFrames gegevenssets handleiding](https://spark.apache.org/docs/latest/sql-programming-guide.html) en de [documentatie voor Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/).

