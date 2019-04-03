---
title: MongoDB-extensie-opdrachten voor het beheren van gegevens die zijn opgeslagen in Azure Cosmos DB-API voor MongoDB
description: In dit artikel wordt beschreven hoe u MongoDB extensie-opdrachten gebruiken voor het beheren van gegevens die zijn opgeslagen in Azure Cosmos DB-API voor MongoDB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sngun
ms.openlocfilehash: aef77f121f20d867c8ec5e764d8c9639c961713d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58876885"
---
# <a name="use-mongodb-extension-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB-extensie-opdrachten gebruiken voor het beheren van gegevens die zijn opgeslagen in Azure Cosmos DB-API voor MongoDB 

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt communiceren met de Azure Cosmos DB-API voor MongoDB met behulp van de open-source [MongoDB-clientstuurprogramma's](https://docs.mongodb.org/ecosystem/drivers). De Azure Cosmos DB-API voor MongoDB kunt u gebruikmaken van bestaande clientstuurprogramma door aan de [wire-protocol voor MongoDB](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

U kunt profiteren van de voordelen Cosmos DB, zoals globale distributie, automatische sharding, hoge beschikbaarheid, gegarandeerde latentie, automatische, versleuteling-at-rest, back-ups, met behulp van de Azure Cosmos DB-API voor MongoDB, en veel meer, terwijl uw investeringen te behouden in uw MongoDB-app.

## <a name="mongodb-protocol-support"></a>Protocolondersteuning voor MongoDB

Standaard de Azure Cosmos DB-API voor MongoDB is compatibel met MongoDB-serverversie 3.2, voor meer informatie, Zie [ondersteunde functies en -syntaxis](mongodb-feature-support.md). De functies of de standaardoperators voor query's die worden toegevoegd in MongoDB versie 3.4 zijn momenteel beschikbaar als preview in de Azure Cosmos DB-API voor MongoDB. De volgende opdrachten in de extensie voor ondersteuning van specifieke functionaliteit van Azure Cosmos DB bij het uitvoeren van CRUD-bewerkingen op de gegevens die zijn opgeslagen in Azure Cosmos DB-API voor MongoDB:

* [Database maken](#create-database)
* [Database bijwerken](#update-database)
* [Database ophalen](#get-database)
* [Verzameling maken](#create-collection)
* [Verzameling bijwerken](#update-collection)
* [Verzameling ophalen](#get-collection)

## <a id="create-database"></a> Database maken

De opdracht create database extensie maakt een nieuwe MongoDB-database. Naam van de database wordt gebruikt in de context van de databases op basis waarvan u de opdracht wordt uitgevoerd. De indeling van de opdracht CreateDatabase is als volgt:

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

De volgende tabel beschrijft de parameters in de opdracht:

|**Veld**|**Type** |**Description** |
|---------|---------|---------|
| CustomAction   |  string  |   De naam van de aangepaste opdracht, moet deze 'CreateDatabase'.      |
| offerThroughput | int  | Ingerichte doorvoer die u op de database instelt. Deze parameter is optioneel. |

### <a name="output"></a>Uitvoer

Retourneert een antwoord van de aangepaste opdracht standaard. Zie de [uitvoer standaard](#default-output) van aangepaste opdracht voor de parameters in de uitvoer.

### <a name="examples"></a>Voorbeelden

**Een database maken**

Gebruik de volgende opdracht voor het maken van een database met de naam 'test':

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**Een database maken met doorvoer**

Een database te maken met de naam 'test' en de ingerichte doorvoer van 1000 ru's, gebruikt u de volgende opdracht uit:

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

## <a id="update-database"></a> Database bijwerken

De opdracht voor gegevensextensies database update werkt de eigenschappen die zijn gekoppeld aan de opgegeven database. U kunt op dit moment alleen de eigenschap 'offerThroughput' bijwerken.

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

De volgende tabel beschrijft de parameters in de opdracht:

|**Veld**|**Type** |**Beschrijving** |
|---------|---------|---------|
| CustomAction    |    string     |   Naam van de aangepaste opdracht. Moet 'UpdateDatabase'.      |
|  offerThroughput   |  int       |     Nieuw ingerichte doorvoer die u wilt instellen op de database.    |

### <a name="output"></a>Uitvoer

Retourneert een antwoord van de aangepaste opdracht standaard. Zie de [uitvoer standaard](#default-output) van aangepaste opdracht voor de parameters in de uitvoer.

### <a name="examples"></a>Voorbeelden

**De ingerichte doorvoer die is gekoppeld aan een database bijwerken**

Voor het bijwerken van de ingerichte doorvoer van een database met de naam 'test' 1200 ru's, gebruik de volgende opdracht:

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

## <a id="get-database"></a> Database ophalen

Met de opdracht get-database extension retourneert het databaseobject. Naam van de database wordt gebruikt in de databasecontext op basis waarvan u de opdracht wordt uitgevoerd.

```
{
  customAction: "GetDatabase"
}
```

De volgende tabel beschrijft de parameters in de opdracht:


|**Veld**|**Type** |**Description** |
|---------|---------|---------|
|  CustomAction   |   string      |   Naam van de aangepaste opdracht. Moet "GetDatabase"|
        
### <a name="output"></a>Uitvoer

Als de opdracht is geslaagd, bevat het antwoord een document met de volgende velden:

|**Veld**|**Type** |**Beschrijving** |
|---------|---------|---------|
|  `ok`   |   `int`     |   De status van de reactie. 1 == geslaagd. 0 mislukte ==.      |
| `database`    |    `string`        |   Naam van de database.      |
|   `provisionedThroughput`  |    `int`      |    Ingerichte doorvoer die is ingesteld op de database. Dit is een optionele antwoord-parameter.     |

Als de opdracht mislukt, wordt een standaard aangepaste opdrachtantwoord geretourneerd. Zie de [uitvoer standaard](#default-output) van aangepaste opdracht voor de parameters in de uitvoer.

### <a name="examples"></a>Voorbeelden

**De database ophalen**

Als u het database-object voor een database met de naam 'test', gebruikt u de volgende opdracht uit:

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

## <a id="create-collection"></a> Verzameling maken

De opdracht voor gegevensextensies verzameling maken maakt een nieuwe MongoDB-verzameling. Naam van de database wordt gebruikt in de context van de databases op basis waarvan u de opdracht wordt uitgevoerd. De indeling van de opdracht CreateCollection is als volgt:

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

De volgende tabel beschrijft de parameters in de opdracht:

|**Veld**|**Type** |**Description** |
|---------|---------|---------|
| CustomAction    | string | Naam van de aangepaste opdracht. 'CreateDatabase' moet zijn     |
| verzameling      | string | Naam van de verzameling                                   |
| offerThroughput | int    | Ingerichte doorvoer om in te stellen op de database. Er is een optionele parameter |
| shardKey        | string | Shard-sleutelpad voor het maken van een shard-verzameling. Er is een optionele parameter |

### <a name="output"></a>Uitvoer

Retourneert een antwoord van de aangepaste opdracht standaard. Zie de [uitvoer standaard](#default-output) van aangepaste opdracht voor de parameters in de uitvoer.

### <a name="examples"></a>Voorbeelden

**Een verzameling unsharded maken**

Gebruik de volgende opdracht voor het maken van een verzameling unsharded met de naam 'testCollection' en ingerichte doorvoer van 1000 ru's: 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**Een shard verzameling maken**

Gebruik de volgende opdracht voor het maken van een shard verzameling met de naam 'testCollection' en ingerichte doorvoer van 1000 ru's:

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

## <a id="update-collection"></a> Verzameling bijwerken

De opdracht voor gegevensextensies verzameling update werkt de eigenschappen die zijn gekoppeld aan de opgegeven verzameling.

```
{
  customAction: "UpdateCollection",
  collection: <Name of the collection that you want to update>,
  offerThroughput: <New throughput that you want to provision on the collection> 
}
```

De volgende tabel beschrijft de parameters in de opdracht:

|**Veld**|**Type** |**Description** |
|---------|---------|---------|
|  CustomAction   |   string      |   Naam van de aangepaste opdracht. Moet 'UpdateCollection'.      |
|  verzameling   |   string      |   Naam van de verzameling.       |
| offerThroughput   |int|   Ingerichte doorvoer om in te stellen op de verzameling.|

## <a name="output"></a>Uitvoer

Retourneert een antwoord van de aangepaste opdracht standaard. Zie de [uitvoer standaard](#default-output) van aangepaste opdracht voor de parameters in de uitvoer.

### <a name="examples"></a>Voorbeelden

**De ingerichte doorvoer die is gekoppeld aan een verzameling bijwerken**

Als u wilt de ingerichte doorvoer van een verzameling met de naam 'testCollection' bijwerken naar 1200 ru's, gebruik de volgende opdracht:

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

## <a id="get-collection"></a> Verzameling ophalen

De opdracht get-verzameling aangepaste retourneert het verzamelingsobject.

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

De volgende tabel beschrijft de parameters in de opdracht:


|**Veld**|**Type** |**Description** |
|---------|---------|---------|
| CustomAction    |   string      |   Naam van de aangepaste opdracht. Moet 'GetCollection'.      |
| verzameling    |    string     |    Naam van de verzameling.     |

### <a name="output"></a>Uitvoer

Als de opdracht is geslaagd, bevat het antwoord een document met de volgende velden


|**Veld**|**Type** |**Description** |
|---------|---------|---------|
|  `ok`   |    `int`     |   De status van de reactie. 1 == geslaagd. 0 mislukte ==.      |
| `database`    |    `string`     |   Naam van de database.      |
| `collection`    |    `string`     |    Naam van de verzameling.     |
|  `shardKeyDefinition`   |   `document`      |  Index-specificatiedocument gebruikt als een shard-sleutel. Dit is een optionele antwoord-parameter.       |
|  `provisionedThroughput`   |   `int`      |    Ingerichte doorvoer om in te stellen op de verzameling. Dit is een optionele antwoord-parameter.     |

Als de opdracht mislukt, wordt een standaard aangepaste opdrachtantwoord geretourneerd. Zie de [uitvoer standaard](#default-output) van aangepaste opdracht voor de parameters in de uitvoer.

### <a name="examples"></a>Voorbeelden

**De verzameling ophalen**

Voor het verzamelingsobject voor een verzameling met de naam 'testCollection', gebruikt u de volgende opdracht uit:

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

## <a id="default-output"></a> Standaarduitvoer van een aangepaste opdracht

Als niet is opgegeven, bevat een aangepast antwoord een document met de volgende velden:

|**Veld**|**Type** |**Description** |
|---------|---------|---------|
|  `ok`   |    `int`     |   De status van de reactie. 1 == geslaagd. 0 mislukte ==.      |
| `code`    |   `int`      |   Alleen geretourneerd wanneer de opdracht is mislukt (dat wil zeggen ok == 0). Bevat de MongoDB-foutcode. Dit is een optionele antwoord-parameter.      |
|  `errMsg`   |  `string`      |    Alleen geretourneerd wanneer de opdracht is mislukt (dat wil zeggen ok == 0). Bevat een beschrijvend foutbericht. Dit is een optionele antwoord-parameter.      |

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan met het leren van de volgende Azure Cosmos DB-concepten: 

* [Indexeren in Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Gegevens in Azure Cosmos DB met time to live van automatisch laten verlopen](../cosmos-db/time-to-live.md)