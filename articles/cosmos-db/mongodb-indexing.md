---
title: Indexeren in Azure Cosmos DB-API voor MongoDB
description: Geeft een overzicht van de Indexeringsmogelijkheden met Azure Cosmos DB-API voor MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: de037316efa50dd25ea04c370fa0e5878fb52ba1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54040401"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Indexeren met Azure Cosmos DB-API voor MongoDB

Azure Cosmos DB API voor MongoDB maakt gebruik van mogelijkheden voor automatische index van Cosmos DB. Als gevolg hiervan hebben gebruikers toegang tot de standaardbeleidsregels voor indexering van Cosmos DB. Dus als er geen indexen zijn gedefinieerd door de gebruiker, of geen indexen zijn verwijderd, worden klikt u vervolgens alle velden automatisch geïndexeerd wanneer ze in een verzameling ingevoegd. Voor de meeste scenario's wordt het aangeraden om het standaardindexeringsbeleid te gebruiken dat is ingesteld voor het account.

## <a name="dropping-the-default-indexes"></a>De standaardindexen verwijderen

De volgende opdracht kan worden gebruikt voor het verwijderen van de standaardindexen voor een verzameling ```coll```:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

## <a name="creating-compound-indexes"></a>Samengestelde indexen maken

Samengestelde indexen bevatten verwijzingen naar meerdere velden van een document. Logisch gezien zijn ze te vergelijken met het maken van meerdere afzonderlijke indexen per veld. Als u gebruik wilt maken van de optimalisaties die het resultaat zijn van de indexeringstechnieken van Cosmos DB, adviseren we om meerdere afzonderlijke indexen te maken in plaats van één (niet-unieke) samengestelde index.

## <a name="creating-unique-indexes"></a>Unieke indexen maken

[Unieke indexen](unique-keys.md) zijn handig om af te dwingen dat twee of meer documenten niet dezelfde waarde bevatten voor een of meer geïndexeerde velden. 
>[!important] 
> Op dit moment kunnen alleen unieke indexen worden gemaakt wanneer de verzameling leeg is (bevat geen documenten). 

De volgende opdracht maakt u een unieke index voor het veld 'student_id':

```JavaScript
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} ) 
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Overeenkomstig het gedrag van MongoDB is voor het maken van een unieke index voor shard-verzamelingen de shard-sleutel (partitiesleutel) nodig. In andere woorden, alle unieke indexen voor een shard-verzameling zijn samengestelde indexen waarbij een van de velden de partitiesleutel is.

Met de volgende opdrachten maakt u een shard verzameling ```coll``` (de shard-sleutel is ```university```) met een unieke index voor de velden student_id en university:

```JavaScript
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

Als in het bovenstaande voorbeeld de component ```"university":1``` wordt weggelaten, treedt er een fout op en ziet u dit foutbericht:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

## <a name="ttl-indexes"></a>TTL indexen

Als u wilt instellen dat documenten in een bepaalde verzameling verlopen na bepaalde tijd, moet u een [TTL-index (Time-To-Live)](../cosmos-db/time-to-live.md) maken. Een TTL-index is een index van het veld _ts met een waarde voor 'expireAfterSeconds'.
 
Voorbeeld:
```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Met de bovenstaande opdracht worden alle documenten in de verzameling ```db.coll``` verwijderd die de afgelopen 10 seconden niet zijn gewijzigd. 
 
> [!NOTE]
> **_ts** is een veld dat specifiek is voor Cosmos DB en dat niet toegankelijk is vanuit MongoDB-clients. Het is een gereserveerde eigenschap (systeemeigenschap) met het tijdstempel van de laatste wijziging van het document.
>

## <a name="migrating-collections-with-indexes"></a>Verzamelingen migreren met indexen

Op dit moment kunnen alleen unieke indexen worden gemaakt wanneer de verzameling geen documenten bevat. Veelgebruikte MongoDB-migratieprogramma's proberen de unieke indexen te maken nadat de gegevens zijn geïmporteerd. Om dit probleem te voorkomen, is het nodig dat gebruikers handmatig de bijbehorende verzamelingen en unieke indexen maken, in plaats van dit te laten doen door het migratieprogramma (voor ```mongorestore``` kan dit worden gedaan door de vlag--noIndexRestore te gebruiken op de opdrachtregel).

## <a name="next-steps"></a>Volgende stappen
* [Indexering in Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Gegevens in Azure Cosmos DB met time to live van automatisch laten verlopen](../cosmos-db/time-to-live.md)
