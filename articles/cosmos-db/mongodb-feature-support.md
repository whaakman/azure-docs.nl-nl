---
title: Ondersteuning van Azure DB Cosmos-functie voor MongoDB | Microsoft Docs
description: Meer informatie over de functies die worden ondersteund die de MongoDB-API van Azure Cosmos DB voor MongoDB 3.4 biedt.
services: cosmos-db
author: alekseys
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 29b6547c-3201-44b6-9e0b-e6f56e473e24
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: alekseys
ms.openlocfilehash: 007b530cd7a14f063ae4f86d18daa9742c6655c2
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="mongodb-api-support-for-mongodb-features-and-syntax"></a>MongoDB-API-ondersteuning voor MongoDB-functies en syntaxis

Azure Cosmos-database is de service van Microsoft wereldwijd gedistribueerde database voor meerdere model. U kunt communiceren met de MongoDB-API van de database via een van de open-source MongoDB-client [stuurprogramma's](https://docs.mongodb.org/ecosystem/drivers). De MongoDB-API maakt het gebruik van de bestaande client-stuurprogramma's door aan de MongoDB [wire-protocol](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

Met behulp van de Azure-API voor MongoDB Cosmos DB, kunt u profiteren van de voordelen van de MongoDB-APIs bent u gewend om, met alle mogelijkheden van Azure Cosmos DB enterprise: [globale distributie](distribute-data-globally.md), [automatische sharding](partition-data.md), beschikbaarheid en latentie garanties, automatische indexering van elk veld, versleuteling bij rust, back-ups en nog veel meer.

## <a name="mongodb-query-language-support"></a>Taalondersteuning voor MongoDB-query

Azure Cosmos DB MongoDB-API biedt uitgebreide ondersteuning voor taalconstructs MongoDB-query. Hieronder vindt u de gedetailleerde lijst met ondersteunde bewerkingen, operators, fasen, opdrachten en opties.


## <a name="database-commands"></a>Databaseopdrachten

Azure Cosmos DB ondersteunt de volgende databaseopdrachten op alle MongoDB-API-accounts. 

### <a name="query-and-write-operation-commands"></a>Vragen en in te schrijven bewerking opdrachten
- verwijderen
- zoeken
- findAndModify
- getLastError
- getMore
- Invoegen
- bijwerken

### <a name="authentication-commands"></a>Verificatie-opdrachten
- afmelden
- Verifiëren
- getnonce

### <a name="administration-commands"></a>Van beheeropdrachten
- dropDatabase
- listCollections
- vervolgkeuzelijst
- maken
- filemd5
- createIndexes
- listIndexes
- dropIndexes
- ConnectionStatus
- REINDEX (Engelstalig)

### <a name="diagnostics-commands"></a>Opdrachten van diagnostische gegevens
- buildInfo
- collStats
- dbStats
- hostInfo
- listDatabases
- whatsmyuri

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Aggregatie-pipeline</a>

Azure Cosmos DB ondersteunt aggregatie pijplijn in openbare preview. Zie de [Azure blog](https://aka.ms/mongodb-aggregation) voor instructies over het voorbereiden voor de openbare preview.

### <a name="aggregation-commands"></a>Aggregatie van opdrachten
- statistische functie
- aantal
- Afzonderlijke

### <a name="aggregation-stages"></a>Aggregatie-fasen
- $project
- $match
- $limit
- $skip
- $afwikkelbewerking
- $group
- $sample
- $sort
- $lookup
- $out
- $count

### <a name="aggregation-expressions"></a>Statistische expressies

#### <a name="boolean-expressions"></a>Booleaanse expressies
- $en
- $of
- $not

#### <a name="set-expressions"></a>Set-expressies
- $setEquals
- $setIntersection
- $setUnion
- $setDifference
- $setIsSubset
- $anyElementTrue
- $allElementsTrue

#### <a name="comparison-expressions"></a>Vergelijkingsexpressies
- $cmp
- $eq
- $gt
- $gte
- $lt
- $lte
- $ne

#### <a name="arithmetic-expressions"></a>Aritmetische expressies
- $abs
- $toevoegen
- $ceil
- $divide
- $exp
- $floor
- $ln
- $log
- $log10
- $mod
- $vermenigvuldigen
- $pow
- $sqrt
- $aftrekken
- $trunc

#### <a name="string-expressions"></a>Expressies voor verbindingsreeksen
- $concat
- $indexOfBytes
- $indexOfCP
- $split
- $strLenBytes
- $strLenCP
- $strcasecmp
- $substr
- $substrBytes
- $substrCP
- $toLower
- $toUpper

#### <a name="array-expressions"></a>Matrix-expressies
- $arrayElemAt
- $concatArrays
- $filter
- $indexOfArray
- $isArray
- $range
- $reverseArray
- $size
- $slice
- $in

#### <a name="date-expressions"></a>Datumexpressies
- $dayOfYear
- $dayOfMonth
- $dayOfWeek
- $year
- $month
- $week
- $hour
- $minute
- $tweede
- $millisecond
- $isoDayOfWeek
- $isoWeek

#### <a name="conditional-expressions"></a>Voorwaardelijke expressies
- $cond
- $ifNull

## <a name="aggregation-accumulators"></a>Aggregatie accu 's
- $sum
- $avg
- $eerste
- $laatste
- $max
- $min
- $push
- $addToSet

## <a name="operators"></a>Operators

Volgende operatoren worden ondersteund met de bijbehorende voorbeelden van hun gebruik. Houd rekening met dit voorbeelddocument in de onderstaande query's gebruikt:

```json
{
  "Volcano Name": "Rainier",
  "Country": "United States",
  "Region": "US-Washington",
  "Location": {
    "type": "Point",
    "coordinates": [
      -121.758,
      46.87
    ]
  },
  "Elevation": 4392,
  "Type": "Stratovolcano",
  "Status": "Dendrochronology",
  "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
}
```

Operator | Voorbeeld |
--- | --- |
$eq | ``` { "Volcano Name": { $eq: "Rainier" } } ``` |  | -
$gt | ``` { "Elevation": { $gt: 4000 } } ``` |  | -
$gte | ``` { "Elevation": { $gte: 4392 } } ``` |  | -
$lt | ``` { "Elevation": { $lt: 5000 } } ``` |  | -
$lte | ``` { "Elevation": { $lte: 5000 } } ``` | | -
$ne | ``` { "Elevation": { $ne: 1 } } ``` |  | -
$in | ``` { "Volcano Name": { $in: ["St. Helens", "Rainier", "Glacier Peak"] } } ``` |  | -
$nin | ``` { "Volcano Name": { $nin: ["Lassen Peak", "Hood", "Baker"] } } ``` | | -
$of | ``` { $or: [ { Elevation: { $lt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$en | ``` { $and: [ { Elevation: { $gt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$not | ``` { "Elevation": { $not: { $gt: 5000 } } } ```|  | -
$noch | ``` { $nor: [ { "Elevation": { $lt: 4000 } }, { "Volcano Name": "Baker" } ] } ``` |  | -
$bestaat | ``` { "Status": { $exists: true } } ```|  | -
$type | ``` { "Status": { $type: "string" } } ```|  | -
$mod | ``` { "Elevation": { $mod: [ 4, 0 ] } } ``` |  | -
$regex | ``` { "Volcano Name": { $regex: "^Rain"} } ```|  | -

### <a name="notes"></a>Opmerkingen

In $regex query's kunnen expressies links verankerd search index. Echter met behulp van 'i' aanpassingsfunctie (hoofdlettergevoeligheid) en 'M ' aanpassingsfunctie (multiline) zorgt ervoor dat de verzameling scan in alle expressies.
Wanneer er een nodig om '$' of ' |', het is raadzaam om twee (of meer) regex-query's maken. Bijvoorbeeld, krijgt de volgende query in de oorspronkelijke: ```find({x:{$regex: /^abc$/})```, er als volgt worden gewijzigd: ```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.
Het eerste deel wordt de index met de zoekopdracht beperken tot deze documenten vanaf ^ abc en het tweede gedeelte komt overeen met de exacte vermeldingen. De balk operator ' |' fungeert als een functie 'of' - de query ```find({x:{$regex: /^abc|^def/})``` overeenkomt met de documenten whin welk veld 'x' heeft de waarde die met "abc" of "def begint". Als u wilt gebruikmaken van de index, verdient het aanbeveling om de query in twee verschillende query's die worden toegevoegd door de $of operator: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.


### <a name="geospatial-operators"></a>Georuimtelijke operators

Operator | Voorbeeld 
--- | --- |
$geoWithin | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Ja
$geoIntersects |  ```{ "Location.coordinates": { $geoIntersects: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja
$bijna | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja
$nearSphere | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Ja
$geometry | ```{ "Location.coordinates": { $geoWithin: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja
$minDistance | ```{ "Location.coordinates": { $nearSphere : { $geometry: {type: "Point", coordinates: [ -121, 46 ]}, $minDistance: 1000, $maxDistance: 1000000 } } }``` | Ja
$maxDistance | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Ja
$center | ```{ "Location.coordinates": { $geoWithin: { $center: [ [-121, 46], 1 ] } } }``` | Ja
$centerSphere | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Ja
$box | ```{ "Location.coordinates": { $geoWithin: { $box:  [ [ 0, 0 ], [ -122, 47 ] ] } } }``` | Ja
$polygon | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja

## <a name="additional-operators"></a>Andere operators

Operator | Voorbeeld | Opmerkingen 
--- | --- | --- |
$all | ```{ "Location.coordinates": { $all: [-121.758, 46.87] } }``` | 
$elemMatch | ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } } }``` |  
$size | ```{ "Location.coordinates": { $size: 2 } }``` | 
$comment |  ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } }, $comment: "Negative values"}``` | 
$text |  | Niet ondersteund. Gebruik in plaats daarvan $regex 

### <a name="methods"></a>Methoden

Volgende methoden worden ondersteund:

#### <a name="cursor-methods"></a>Cursor-methoden

Methode | Voorbeeld | Opmerkingen 
--- | --- | --- |
cursor.sort() | ```cursor.sort({ "Elevation": -1 })``` | Documenten zonder sorteersleutel komen niet geretourneerd

## <a name="unique-indexes"></a>Unieke indexen

Azure Cosmos DB indexeert elk veld in de documenten die standaard worden geschreven naar de database. Unieke indexen Zorg dat een bepaald veld geen dubbele waarden op alle documenten in een verzameling, vergelijkbaar met de uniekheid van de manier blijft behouden op de standaardwaarde '_id'-sleutel. U kunt nu aangepaste indexen in Azure Cosmos-database maken met de opdracht createIndex, met inbegrip van de 'unique-beperking.

Unieke indexen zijn beschikbaar voor alle accounts voor MongoDB-API.

## <a name="time-to-live-ttl"></a>Time-to-live (TTL)

Azure Cosmos DB biedt ondersteuning voor een relatieve time to live (TTL) op basis van de tijdstempel van het document. TTL kan worden ingeschakeld voor MongoDB-API-verzamelingen via de [Azure-portal](https://portal.azure.com).

## <a name="user-and-role-management"></a>Gebruiker-en rolbeheer

Azure Cosmos DB ondersteunt nog geen gebruikers en rollen. Azure Cosmos DB biedt ondersteuning voor op rollen gebaseerd toegangsbeheer (RBAC) en alleen-lezen en alleen-lezen wachtwoorden/sleutels die kunnen worden verkregen via de [Azure-portal](https://portal.azure.com) (pagina verbindingsreeks).

## <a name="replication"></a>Replicatie

Azure Cosmos DB ondersteunt automatische systeemeigen replicatie op de laagste lagen. Deze logica wordt uitgebreid uit als u de lage latentie, globale replicatie ook. Azure Cosmos DB biedt geen ondersteuning voor handmatige replicatieopdrachten.

## <a name="sharding"></a>Sharding

Azure Cosmos DB ondersteunt automatische, serverzijde sharding. Azure Cosmos DB biedt geen ondersteuning voor handmatige sharding-opdrachten.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [Studio 3T gebruiken](mongodb-mongochef.md) met een API voor MongoDB-database.
- Meer informatie over hoe [Robo 3T gebruiken](mongodb-robomongo.md) met een API voor MongoDB-database.
- Azure Cosmos DB verkennen met protocolondersteuning voor MongoDB [voorbeelden](mongodb-samples.md).
