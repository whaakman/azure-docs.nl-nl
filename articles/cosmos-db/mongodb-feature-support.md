---
title: De API van Azure Cosmos DB gebruiken voor ondersteuning van de MongoDB-functie
description: Meer informatie over de functieondersteuning die de API van Azure Cosmos DB biedt voor MongoDB 3.4.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 05/21/2019
author: sivethe
ms.author: sivethe
ms.openlocfilehash: 09d12c817e9e8678cf96c63c0bff623923f7074d
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978827"
---
# <a name="azure-cosmos-dbs-api-for-mongodb-supported-features-and-syntax"></a>De API van Azure Cosmos DB voor MongoDB: ondersteunde functies en syntaxis

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt met de API van Azure Cosmos DB voor MongoDB communiceren via een van de open-source MongoDB-[clientstuurprogramma's](https://docs.mongodb.org/ecosystem/drivers). De API van Azure Cosmos DB voor MongoDB maakt het gebruik van bestaande clientstuurprogramma's mogelijk doordat de API functioneert conform het MongoDB-[wireprotocol](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

Door gebruik te maken van de API van Azure Cosmos DB voor MongoDB hebt u de beschikking over de voordelen van de vertrouwde MongoDB, met alle zakelijke mogelijkheden die Cosmos DB biedt: [wereldwijde distributie](distribute-data-globally.md), [automatische sharding](partition-data.md), garanties voor beschikbaarheid en latentie, automatisch indexeren van alle velden, versleuteling van niet-actieve gegevens, het maken van back-ups, en meer.

## <a name="protocol-support"></a>Ondersteuning voor protocol

De API van Azure Cosmos DB voor MongoDB is standaard compatibel met MongoDB-serverversie **3.2**. De ondersteunde operators en eventuele beperkingen of uitzonderingen worden hieronder vermeld. Toegevoegde functies of operators voor query's in MongoDB-versie **3.4** zijn momenteel beschikbaar als preview-functie. Elk clientstuurprogramma dat deze protocollen kent, kan verbinding maken met de API van Azure Cosmos DB voor MongoDB.

De [MongoDB-samenvoegingspijplijn](#aggregation-pipeline) is ook beschikbaar als een afzonderlijke preview-functie.

## <a name="query-language-support"></a>Ondersteuning voor querytaal

De API van Azure Cosmos DB voor MongoDB biedt uitgebreide ondersteuning voor MongoDB-querytaalconstructs. Hieronder ziet u de gedetailleerde lijst met momenteel ondersteunde bewerkingen, operators, fasen, opdrachten en opties.

## <a name="database-commands"></a>Databaseopdrachten

De API van Azure Cosmos DB voor MongoDB biedt ondersteuning voor de volgende databaseopdrachten:

### <a name="query-and-write-operation-commands"></a>Opdrachten voor query- en schrijfbewerkingen
- delete
- find
- findAndModify
- getLastError
- getMore
- insert
- update

### <a name="authentication-commands"></a>Verificatieopdrachten
- logout
- authenticate
- getnonce

### <a name="administration-commands"></a>Beheeropdrachten
- dropDatabase
- listCollections
- drop
- maken
- filemd5
- createIndexes
- listIndexes
- dropIndexes
- connectionStatus
- reIndex

### <a name="diagnostics-commands"></a>Diagnostics commands
- buildInfo
- collStats
- dbStats
- hostInfo
- listDatabases
- whatsmyuri

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Samenvoegingspijplijn</a>

Cosmos DB biedt ondersteuning voor samenvoegingspijplijnen in de openbare preview-versie. Lees het [Azure-blog](https://aka.ms/mongodb-aggregation) voor instructies over deelname aan de openbare preview-versie.

### <a name="aggregation-commands"></a>Samenvoegingsopdrachten
- aggregate
- count
- distinct

### <a name="aggregation-stages"></a>Samenvoegingsfasen
- $project
- $match
- $limit
- $skip
- $unwind
- $group
- $sample
- $sort
- $lookup
- $out
- $count
- $addFields

### <a name="aggregation-expressions"></a>Expressies voor samenvoegen

#### <a name="boolean-expressions"></a>Booleaanse expressies
- $and
- $or
- $not

#### <a name="set-expressions"></a>Expressies voor instellen
- $setEquals
- $setIntersection
- $setUnion
- $setDifference
- $setIsSubset
- $anyElementTrue
- $allElementsTrue

#### <a name="comparison-expressions"></a>Expressies voor vergelijken
- $cmp
- $eq
- $gt
- $gte
- $lt
- $lte
- $ne

#### <a name="arithmetic-expressions"></a>Rekenkundige expressies
- $abs
- $add
- $ceil
- $divide
- $exp
- $floor
- $ln
- $log
- $log10
- $mod
- $multiply
- $pow
- $sqrt
- $subtract
- $trunc

#### <a name="string-expressions"></a>Tekenreeksexpressies
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

#### <a name="array-expressions"></a>Matrixexpressies
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
- $second
- $millisecond
- $isoDayOfWeek
- $isoWeek

#### <a name="conditional-expressions"></a>Voorwaardelijke expressies
- $cond
- $ifNull

## <a name="aggregation-accumulators"></a>Samenvoegingsaccumulators
- $sum
- $avg
- $first
- $last
- $max
- $min
- $push
- $addToSet

## <a name="operators"></a>Operators

De volgende operators worden ondersteund met de bijbehorende gebruiksvoorbeelden. Overweeg om dit voorbeelddocument te gebruiken in de onderstaande query’s:

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
$eq | `{ "Volcano Name": { $eq: "Rainier" } }` |  | -
$gt | `{ "Elevation": { $gt: 4000 } }` |  | -
$gte | `{ "Elevation": { $gte: 4392 } }` |  | -
$lt | `{ "Elevation": { $lt: 5000 } }` |  | -
$lte | `{ "Elevation": { $lte: 5000 } }` | | -
$ne | `{ "Elevation": { $ne: 1 } }` |  | -
$in | `{ "Volcano Name": { $in: ["St. Helens", "Rainier", "Glacier Peak"] } }` |  | -
$nin | `{ "Volcano Name": { $nin: ["Lassen Peak", "Hood", "Baker"] } }` | | -
$or | `{ $or: [ { Elevation: { $lt: 4000 } }, { "Volcano Name": "Rainier" } ] }` |  | -
$and | `{ $and: [ { Elevation: { $gt: 4000 } }, { "Volcano Name": "Rainier" } ] }` |  | -
$not | `{ "Elevation": { $not: { $gt: 5000 } } }`|  | -
$nor | `{ $nor: [ { "Elevation": { $lt: 4000 } }, { "Volcano Name": "Baker" } ] }` |  | -
$exists | `{ "Status": { $exists: true } }`|  | -
$type | `{ "Status": { $type: "string" } }`|  | -
$mod | `{ "Elevation": { $mod: [ 4, 0 ] } }` |  | -
$regex | `{ "Volcano Name": { $regex: "^Rain"} }`|  | -

### <a name="notes"></a>Opmerkingen

In $regex-query’s is zoeken in de index toegestaan op basis van links-verankerde expressies. Als u echter de aanpassingsfuncties i (geen hoofdlettergevoeligheid) en m (meerdere regels) gebruikt, wordt de verzameling gescand in alle expressies.
Wanneer $ of | moet worden opgenomen, kunt u het beste twee (of meer) regex-query’s maken. Bijvoorbeeld, bij de volgende oorspronkelijke query: ```find({x:{$regex: /^abc$/})```, moet dit als volgt worden gewijzigd: ```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.
Het eerste deel maakt gebruik van de index om alleen te zoeken naar de documenten die beginnen met ^abc en het tweede deel zoekt naar een overeenkomst tussen de exacte vermeldingen. De streepoperator | fungeert als de functie: or. De query ```find({x:{$regex: /^abc|^def/})``` komt overeen met de documenten waarin het veld x waarden heeft die beginnen met abc of def. Als u de index wilt gebruiken, kunt u de query het beste opsplitsen in twee verschillende query’s die zijn verbonden met de operator $or: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.

### <a name="update-operators"></a>Operators voor updates

#### <a name="field-update-operators"></a>Operators voor veldupdates
- $inc
- $mul
- $rename
- $setOnInsert
- $set
- $unset
- $min
- $max
- $currentDate

#### <a name="array-update-operators"></a>Operators voor matrixupdates
- $addToSet
- $pop
- $pullAll
- $pull  (Let op: $pull met voorwaarde wordt niet ondersteund)
- $pushAll
- $push
- $each
- $slice
- $sort
- $position

#### <a name="bitwise-update-operator"></a>Operators voor bitwise-updates
- $bit

### <a name="geospatial-operators"></a>Georuimtelijke operators

Operator | Voorbeeld | |
--- | --- | --- |
$geoWithin | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Ja |
$geoIntersects |  ```{ "Location.coordinates": { $geoIntersects: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja |
$near | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja |
$nearSphere | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Ja |
$geometry | ```{ "Location.coordinates": { $geoWithin: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja |
$minDistance | ```{ "Location.coordinates": { $nearSphere : { $geometry: {type: "Point", coordinates: [ -121, 46 ]}, $minDistance: 1000, $maxDistance: 1000000 } } }``` | Ja |
$maxDistance | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Ja |
$center | ```{ "Location.coordinates": { $geoWithin: { $center: [ [-121, 46], 1 ] } } }``` | Ja |
$centerSphere | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Ja |
$box | ```{ "Location.coordinates": { $geoWithin: { $box:  [ [ 0, 0 ], [ -122, 47 ] ] } } }``` | Ja |
$polygon | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Ja |

## <a name="sort-operations"></a>Bewerkingen sorteren
Tijdens het gebruik van bewerking `findOneAndUpdate`, worden sorteerbewerkingen op één veld ondersteund, maar sorteerbewerkingen op meerdere velden worden niet ondersteund.

## <a name="additional-operators"></a>Andere operators

Operator | Voorbeeld | Opmerkingen 
--- | --- | --- |
$all | ```{ "Location.coordinates": { $all: [-121.758, 46.87] } }``` | 
$elemMatch | ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } } }``` |  
$size | ```{ "Location.coordinates": { $size: 2 } }``` | 
$comment |  ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } }, $comment: "Negative values"}``` | 
$text |  | Wordt niet ondersteund. Gebruik in plaats hiervan $regex.

## <a name="unsupported-operators"></a>Niet-ondersteunde operators

De operators ```$where``` en ```$eval``` worden niet ondersteund door Azure Cosmos DB.

### <a name="methods"></a>Methoden

De volgende methoden worden ondersteund:

#### <a name="cursor-methods"></a>Cursormethoden

Methode | Voorbeeld | Opmerkingen 
--- | --- | --- |
cursor.sort() | ```cursor.sort({ "Elevation": -1 })``` | Documenten zonder sorteersleutel worden niet geretourneerd

## <a name="unique-indexes"></a>Unieke indexen

Met Cosmos DB worden alle velden geïndexeerd in documenten die standaard naar de database zijn geschreven. Unieke indexen zorgen ervoor dat een specifiek veld geen dubbele waarden bevat in alle documenten van een verzameling, net zoals de uniekheid van de standaard-id-sleutel behouden blijft. U kunt met behulp van de opdracht createIndex aangepaste indexen maken in Cosmos DB, met inbegrip van de uniekheidsbeperking.

Unieke indexen zijn beschikbaar voor alle Cosmos-accounts die de API van Azure Cosmos DB voor MongoDB gebruiken.

## <a name="time-to-live-ttl"></a>TTL (time-to-live)

Cosmos DB biedt ondersteuning voor een TTL (time-to-live) op basis van de tijdstempel van het document. TTL kan worden ingeschakeld voor verzamelingen door naar [Azure Portal](https://portal.azure.com) te gaan.

## <a name="user-and-role-management"></a>Gebruikers- en rolbeheer

Cosmos DB biedt nog geen ondersteuning voor gebruikers en rollen. Cosmos DB biedt echter op rollen gebaseerd toegangsbeheer (RBAC), wachtwoorden voor lezen/schrijven en wachtwoorden met het kenmerk Alleen-lezen die kunnen worden verkregen via [Azure Portal](https://portal.azure.com) (pagina Verbindingsreeks).

## <a name="replication"></a>Replicatie

Cosmos DB biedt ondersteuning voor automatische, systeemeigen replicatie op de laagste lagen. Deze logica is uitgebreid om tevens globale replicatie met een lage latentie te bereiken. Cosmos DB biedt geen ondersteuning voor handmatige replicatieopdrachten.

## <a name="write-concern"></a>Schrijfprobleem

Bepaalde toepassingen vertrouwen op een [schrijfprobleem](https://docs.mongodb.com/manual/reference/write-concern/) dat het aantal vereiste reacties tijdens een schrijfbewerking opgeeft. Vanwege de manier waarop replicatie op de achtergrond in Cosmos DB wordt afgehandeld, zijn alle schrijfbewerkingen automatisch Quorum-bewerkingen. Alle schrijfproblemen die in clientcode zijn opgegeven, worden genegeerd. Zie [Consistentieniveaus gebruiken om de beschikbaarheid en prestaties te maximaliseren](consistency-levels.md) voor meer informatie.

## <a name="sharding"></a>Sharding

Cosmos DB biedt ondersteuning voor automatische sharding aan serverzijde. Cosmos DB biedt geen ondersteuning voor handmatige sharding-opdrachten.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Studio 3T](mongodb-mongochef.md) met de API voor MongoDB van Azure Cosmos DB.
- Meer informatie over het [gebruik van Robo 3T](mongodb-robomongo.md) met de API voor MongoDB van Azure Cosmos DB.
- Verken [voorbeelden](mongodb-samples.md) van MongoDB met de API van Azure Cosmos DB voor MongoDB.

<sup>Opmerking: In dit artikel wordt een functie van Azure Cosmos DB beschreven waarmee compatibiliteit met wire-protocollen met MongoDB-databases kan worden geboden. MongoDB-databases worden niet door Microsoft uitgevoerd om deze service te kunnen leveren. Azure Cosmos DB is niet verbonden aan MongoDB, Inc.</sup>
