---
title: Apache Cassandra-functies en opdrachten ondersteund door Azure Cosmos DB Cassandra API
description: Meer informatie over de ondersteuning van Apache Cassandra-functies in Azure Cosmos DB Cassandra-API
services: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: f62afc91f5cb92bd6f8faa866320fdd23b2c818b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53089238"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Door Azure Cosmos DB Cassandra API ondersteunde Apache Cassandra-functies 

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt communiceren met de Azure Cosmos DB Cassandra-API via open-source Cassandra client[stuurprogramma's](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) die compatibel zijn met Cassandra Query Language (CQL) v4 [wire-protocol](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec). 

Door de Azure Cosmos DB Cassandra API te gebruiken, kunt u profiteren van de voordelen van de Apache Cassandra API’s en van de enterprise-mogelijkheden die Azure Cosmos DB biedt. De enterprise-mogelijkheden zijn onder andere [wereldwijde distributie](distribute-data-globally.md), [automatische scale-out partitionering](partition-data.md), beschikbaarheid- en latentiegaranties, versleuteling van gegevens in rust, back-ups en nog veel meer.

## <a name="cassandra-protocol"></a>Cassandra-protocol 

De Cassandra-API van Azure Cosmos DB is compatibel met CQL versie **v4**. De ondersteunde CQL-opdrachten, hulpprogramma's, beperkingen en uitzonderingen worden hieronder vermeld. Elk clientstuurprogramma dat deze protocollen kent, kan verbinding maken met Azure Cosmos DB Cassandra-API.

## <a name="cassandra-driver"></a>Cassandra-stuurprogramma

De volgende versies van de Cassandra-stuurprogramma's worden ondersteund door Azure Cosmos DB Cassandra-API:

* [Java 3.5+](https://github.com/datastax/java-driver)  
* [C# 3.5+](https://github.com/datastax/csharp-driver)  
* [Nodejs 3.5+](https://github.com/datastax/nodejs-driver)  
* [Python 3.15+](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>CQL-gegevenstypen 

Azure Cosmos DB Cassandra-API ondersteunt de volgende CQL-gegevenstypen:

* ascii  
* bigint  
* blob  
* booleaans  
* counter  
* date  
* decimal  
* double  
* float  
* frozen  
* inet  
* int  
* list  
* set  
* smallint  
* tekst  
* tijd  
* tijdstempel  
* timeuuid  
* tinyint  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udts  
* map  

## <a name="cql-functions"></a>CQL-functies

Azure Cosmos DB Cassandra-API ondersteunt de volgende CQL-functies:

* Token  
* Blob-conversiefuncties 
  * typeAsBlob(value)  
  * blobAsType(value)
* UUID- en timeuuid-functies 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toUnixTimestamp(timeuuid)  
  * toDate(timestamp)  
  * toUnixTimestamp(timestamp)  
  * toTimestamp(date)  
  * toUnixTimestamp(date)  


## <a name="cassandra-query-language-limits"></a>Limieten van Cassandra Query Language

Azure Cosmos DB Cassandra-API heeft geen limiet wat betreft de grootte van gegevens die in een tabel worden opgeslagen. Er kunnen honderden terabytes of petabytes aan gegevens worden opgeslagen terwijl de partitiesleutellimieten worden gerespecteerd. Evenzo heeft elke entiteit of rij-equivalent geen limieten voor het aantal kolommen; maar de totale omvang van de entiteit mag niet groter zijn dan 2 MB.

## <a name="tools"></a>Hulpprogramma's 

Azure Cosmos DB Cassandra-API is een beheerd serviceplatform. Het vereist geen beheeroverhead of hulpprogramma's zoals Garbage Collector, Java Virtual Machine (JVM) en nodetool om het cluster te beheren. Het ondersteunt hulpprogramma’s zoals cqlsh, dat binaire CQLv4-compatibiliteit gebruikt. 

* Andere ondersteunde mechanismen voor het beheren van het account zijn Azure Portal’s Data Explorer, metrische gegevens, logboekdiagnose, PowerShell en CLI.

## <a name="cql-shell"></a>CQL Shell  

Het CQLSH opdrachtregel-hulpprogramma wordt geleverd met Apache Cassandra 3.1.1 en werkt standaard met de volgende omgevingsvariabelen ingeschakeld:

Voordat u de volgende opdrachten uitvoert, [voegt u een Baltimore-basiscertificaat toe aan het cacerts-archief](https://docs.microsoft.com/java/azure/java-sdk-add-certificate-ca-store?view=azure-java-stable#to-add-a-root-certificate-to-the-cacerts-store). 

**Windows:** 

```bash
set SSL_VERSION=TLSv1_2 
SSL_CERTIFICATE=<path to Baltimore root ca cert>
set CQLSH_PORT=10350 
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> –ssl 
```
**UNIX/Linux/Mac:**

```bash
export SSL_VERSION=TLSv1_2 
export SSL_CERTFILE=<path to Baltimore root ca cert>
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> –ssl 
```

## <a name="cql-commands"></a>CQL-opdrachten

Azure Cosmos DB ondersteunt de volgende databaseopdrachten op Cassandra-API-accounts.

* CREATE KEYSPACE 
* CREATE TABLE 
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH - alleen niet-geregistreerde opdrachten worden ondersteund 
* DELETE

Alle crud-bewerkingen die worden uitgevoerd via een SDK die compatibel is met CQLV4 retourneren extra informatie over fouten, verbruikte aanvraageenheden, activiteits-id. Bij het afhandelen van verwijder- en updateopdrachten moet rekening worden gehouden met resourcebeheer, om overmatig gebruik van ingerichte resources te voorkomen. 
* Let wel: de waarde gc_grace_seconds moet nul zijn als deze is opgegeven.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            string value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($“CustomPayload:  {key}: {value}”); 
        } 
```

## <a name="consistency-mapping"></a>Consistentietoewijzing 

Azure Cosmos DB Cassandra-API biedt een keuze aan consistentie voor leesbewerkingen. Alle schrijfbewerkingen, ongeacht de consistentie van het account, worden altijd geschreven met SLA's voor schrijfprestaties.

## <a name="permission-and-role-management"></a>Machtigings- en rolbeheer

Azure Cosmos DB biedt op rollen gebaseerd toegangsbeheer (RBAC), wachtwoorden voor lezen/schrijven en wachtwoorden met het kenmerk alleen-lezen die kunnen worden verkregen via [Azure Portal](https://portal.azure.com). Azure Cosmos DB biedt nog geen ondersteuning voor gebruikers en rollen voor gegevensvlakactiviteiten. 

## <a name="planned-support"></a>Geplande ondersteuning 
* Tijdstempel en TTL samen gebruiken  
* De naam van de regio in de opdracht create keyspace wordt momenteel genegeerd: de distributie van gegevens wordt geïmplementeerd in onderliggend Cosmos DB-platform en de gegevens worden beschikbaar gesteld via de portal of powershell voor het account. 





## <a name="next-steps"></a>Volgende stappen

- Ga aan de slag met het [maken van een Cassandra-API-account, -database en een tabel](create-cassandra-api-account-java.md) met behulp van een Java toepassing

