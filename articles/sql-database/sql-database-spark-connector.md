---
title: Spark Connector met Azure SQL Database en SQL Server | Microsoft Docs
description: Informatie over het gebruik van de Spark-Connector voor Azure SQL Database en SQL Server
services: sql-database
author: allenwux
manager: craigg
ms.service: sql-database
ms.custom: ''
ms.topic: article
ms.date: 04/23/2018
ms.author: xiwu
ms.openlocfilehash: 393af463c4145e1d865c14f2ace7d5123ab12cfa
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="accelerate-real-time-big-data-analytics-with-spark-connector-for-azure-sql-database-and-sql-server"></a>Versnellen realtime big data-analyses met Spark-connector voor Azure SQL Database en SQL Server

De Spark-connector voor Azure SQL Database en SQL Server kunnen SQL-databases, met inbegrip van Azure SQL Database en SQL Server om te fungeren als de bron- of -gegevens sink invoergegevens voor Spark-taken. Hiermee kunt u realtime transactionele gegevens in big data-analyses te gebruiken en resultaten voor ad-hoc-query's of reporting behouden. Vergeleken met de ingebouwde JDBC-connector, biedt deze connector de mogelijkheid om het bulksgewijs invoegen gegevens in SQL-databases. Deze kunt leveren betere prestaties dan per rij invoegen met 10 x 20 x betere prestaties. De Spark-connector voor Azure SQL Database en SQL Server ondersteunt ook de AAD-verificaties. Hiermee kunt u veilig verbinding maken met uw Azure SQL database via Azure Databricks met uw AAD-account. Het biedt vergelijkbare interfaces met de ingebouwde JDBC-connector. Het is gemakkelijk om te migreren van uw bestaande Spark-taken voor het gebruik van deze nieuwe connector.

## <a name="download"></a>Downloaden
Om te beginnen, downloadt u de Spark naar SQL DB-connector in vanuit de [azure-sqldb-spark opslagplaats](https://github.com/Azure/azure-sqldb-spark) op GitHub.

## <a name="official-supported-versions"></a>Officiële ondersteunde versies

| Onderdeel                            |Versie                  |
| :----------------------------------- | :---------------------- |
| Apache Spark                         |2.0.2 of hoger           |
| Scala                                |2,10 of hoger            |
| Microsoft JDBC-stuurprogramma voor SQL Server |6.2 of hoger             |
| Microsoft SQL Server                 |SQL Server 2008 of hoger |
| Azure SQL Database                   |Ondersteund                |

De Spark-connector voor Azure SQL Database en SQL Server gebruikmaakt van de Microsoft JDBC-stuurprogramma voor SQL Server om gegevens te verplaatsen tussen Spark worker-knooppunten en SQL-databases:
 
De gegevensstroom is als volgt:
1. Het hoofdknooppunt Spark maakt verbinding met SQL Server of Azure SQL Database en gegevens worden geladen uit een specifieke tabel of met behulp van een specifieke SQL-query
2. Het hoofdknooppunt Spark verdeelt gegevens met de werkrolknooppunten voor transformatie. 
3. Het werkrolknooppunt maakt verbinding met SQL Server of Azure SQL Database en schrijft gegevens naar de database. Gebruiker kan kiezen voor het gebruik van per rij invoegen of bulksgewijs invoegen.

Het volgende diagram illustreert de gegevensstroom.

   ![architectuur](./media/sql-database-spark-connector/architecture.png)

### <a name="build-the-spark-to-sql-db-connector"></a>De Spark naar SQL DB-connector maken
Op dit moment wordt gebruikt in het project connector maven. De connector zonder de afhankelijkheden bouwen, kunt u het volgende uitvoeren:

- schone mvn-pakket
- Download de nieuwste versies van de JAR in releasemap
- De SQL DB Spark JAR opnemen

## <a name="connect-spark-to-sql-db-using-the-connector"></a>Verbinding maken met Spark SQL-database met behulp van de connector
U kunt verbinding maken met Azure SQL Database of SQL Server uit Spark taken, lezen of schrijven van gegevens. U kunt ook een DML- of DDL-query uitvoeren in een Azure SQL database of SQL Server-database.

### <a name="read-data-from-azure-sql-database-or-sql-server"></a>Gegevens lezen uit Azure SQL Database of SQL Server

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
### <a name="reading-data-from-azure-sql-database-or-sql-server-with-specified-sql-query"></a>Lezen van gegevens van Azure SQL Database of SQL Server met de opgegeven SQL-query
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

## <a name="connect-spark-to-azure-sql-database-using-aad-authentication"></a>Verbinding maken met Spark op Azure SQL Database met AAD-verificaties
U kunt verbinding maken met Azure SQL Database met Azure Active Directory (AAD)-verificatie. AAD-verificatie gebruiken voor het centraal beheren van identiteiten van databasegebruikers en als alternatief voor SQL Server-verificatie.
### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Verbinding maken met behulp van ActiveDirectoryPassword verificatiemodus
#### <a name="setup-requirement"></a>Vereiste installatie
Als u de verificatiemodus ActiveDirectoryPassword gebruikt, moet u downloaden [azure Active Directory-bibliotheek-voor-java-](https://github.com/AzureAD/azure-activedirectory-library-for-java) en de bijbehorende afhankelijkheden en deze opnemen in het pad van de build Java.

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

### <a name="connecting-using-access-token"></a>Verbinding maken met behulp van Access Token
#### <a name="setup-requirement"></a>Vereiste installatie
Als u de toegang op basis van tokens verificatiemodus gebruikt, moet u downloaden [azure Active Directory-bibliotheek-voor-java-](https://github.com/AzureAD/azure-activedirectory-library-for-java) en de bijbehorende afhankelijkheden en deze opnemen in het pad van de build Java.

Zie [gebruik Azure Active Directory-verificatie voor verificatie met SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) voor meer informatie over hoe ze toegang krijgen token aan uw Azure SQL database.

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

## <a name="write-data-to-azure-sql-database-or-sql-server-using-bulk-insert"></a>Schrijven van gegevens naar Azure SQL database of SQL Server gebruikt u de bulksgewijs invoegen
De connector traditioneel jdbc schrijft gegevens naar Azure SQL database of SQL Server met behulp van per rij invoegen. U kunt Spark SQL DB-connector om gegevens te schrijven met SQL database met bulksgewijs invoegen. Dit verbetert de prestaties schrijven aanzienlijk bij het laden van grote gegevenssets of laden van gegevens in tabellen waarbij een columnstore-index wordt gebruikt.

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
Als u nog niet gedaan hebt, downloadt u de Spark-connector voor Azure SQL Database en SQL Server van [GitHub-opslagplaats voor azure-sqldb-spark](https://github.com/Azure/azure-sqldb-spark) en bekijk de aanvullende bronnen in de opslagplaats:

-   [Voorbeeld Azure Databricks laptops](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Voorbeeldscripts (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

U kunt ook om te controleren de [Apache Spark SQL, DataFrames en gegevenssets handleiding](http://spark.apache.org/docs/latest/sql-programming-guide.html) en de [Azure Databricks documentatie](https://docs.microsoft.com/azure/azure-databricks/).

