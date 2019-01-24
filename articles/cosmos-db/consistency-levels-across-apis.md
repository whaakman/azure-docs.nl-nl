---
title: Consistentieniveaus en Azure Cosmos DB-API's
description: Informatie over de consistentieniveaus voor API's in Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: 4d2994ea6ab6d6472ec56f0f2e378062590c8920
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54806994"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Consistentieniveaus en Azure Cosmos DB-API's

Vijf consistentiemodellen aangeboden door Azure Cosmos DB worden ondersteund door de Azure Cosmos DB SQL API. Wanneer u Azure Cosmos DB gebruikt, is de SQL-API de standaardinstelling. 

Azure Cosmos DB biedt ook ingebouwde ondersteuning voor wire-protocol-compatibele API's voor populaire-databases. Databases omvatten MongoDB, Apache Cassandra, Gremlin en Azure Table storage. Deze databases bieden niet nauwkeurig omschreven consistentiemodellen of SLA's gesteunde garanties voor consistentieniveaus. Ze bieden doorgaans slechts een subset van de vijf consistentiemodellen aangeboden door Azure Cosmos DB. Voor de SQL-API, Gremlin-API en Table-API, wordt het standaardconsistentieniveau geconfigureerd op het Azure Cosmos DB-account gebruikt. 

De volgende secties ziet u de toewijzing tussen de consistentie van de gegevens aangevraagd door een OSS-client-stuurprogramma voor Apache Cassandra 4.x en MongoDB 3.4. Dit document bevat ook de bijbehorende Azure Cosmos DB-consistentieniveaus voor Apache Cassandra en MongoDB.

## <a id="cassandra-mapping"></a>Toewijzing tussen Apache Cassandra en Azure Cosmos DB-consistentieniveaus

Deze tabel bevat de toewijzing van de consistentie tussen de Apache Cassandra en consistentieniveaus in Azure Cosmos DB. Voor elk van de Cassandra-lezen en schrijven consistentieniveaus, de bijbehorende Consistentieniveau voor Cosmos DB biedt betere, dat wil zeggen, strengere garanties.

De volgende tabel toont de **schrijven consistentie toewijzing** tussen Azure Cosmos DB en Cassandra:

| Cassandra | Azure Cosmos DB | Guarantee |
| - | - | - |
|ALLE|Sterk  | Linearisabiliteit |
| EACH_QUORUM   | Sterk    | Linearisabiliteit | 
| QUORUM, SERIËLE |  Sterk |    Linearisabiliteit |
| LOCAL_QUORUM, 3, 2, ÉÉN, LOCAL_ONE, | Consistent prefix |Globale Consistent Prefix |
| EACH_QUORUM   | Sterk    | Linearisabiliteit |
| QUORUM, SERIËLE |  Sterk |    Linearisabiliteit |
| LOCAL_QUORUM, 3, 2, ÉÉN, LOCAL_ONE, | Consistent prefix | Globale Consistent Prefix |
| QUORUM, SERIËLE | Sterk   | Linearisabiliteit |
| LOCAL_QUORUM, 3, 2, ÉÉN, LOCAL_ONE, | Consistent prefix | Globale Consistent Prefix |
| LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE    | Gebonden veroudering | <ul><li>Gebonden veroudering.</li><li>Maximaal K versies of t keer achter.</li><li>De meest recente toegezegde waarde in de regio lezen.</li></ul> |
| EEN LOCAL_ONE,   | Consistent prefix | Consistent Prefix per regio |

De volgende tabel toont de **lezen van consistentie toewijzing** tussen Azure Cosmos DB en Cassandra:

| Cassandra | Azure Cosmos DB | Guarantee |
| - | - | - |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO, ONE, LOCAL_ONE | Sterk  | Linearisabiliteit|
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |Sterk |   Linearisabiliteit |
|LOCAL_ONE, ÉÉN | Consistent prefix | Globale Consistent Prefix |
| ALLE, QUORUM, SERIENUMMER   | Sterk    | Linearisabiliteit |
| LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  Consistent prefix   | Globale Consistent Prefix |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM |    Consistent prefix   | Globale Consistent Prefix |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |Sterk |   Linearisabiliteit |
| LOCAL_ONE, ÉÉN    | Consistent prefix | Globale Consistent Prefix|
| ALLE, QUORUM, seriële sterke verwerkingen
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |Consistent prefix  | Globale Consistent Prefix |
|ALLE    |Sterk |Linearisabiliteit |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  |Consistent prefix  |Globale Consistent Prefix|
|ALLE, QUORUM, seriële sterke verwerkingen
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |Consistent prefix  |Globale Consistent Prefix |
|ALLE    |Sterk | Linearisabiliteit |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | Consistent prefix | Globale Consistent Prefix |
| QUORUM, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  Gebonden veroudering   | <ul><li>Gebonden veroudering.</li><li>Maximaal K versies of t keer achter. </li><li>De meest recente toegezegde waarde in de regio lezen.</li></ul>
| LOCAL_ONE, ÉÉN |Consistent prefix | Consistent Prefix per regio |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | Consistent prefix | Consistent Prefix per regio |


## <a id="mongo-mapping"></a>Toewijzing tussen MongoDB 3.4 en Azure Cosmos DB-consistentieniveaus

De volgende tabel ziet u de toewijzing 'lezen problemen' tussen MongoDB 3.4 en het standaardconsistentieniveau in Azure Cosmos DB. De tabel ziet u implementaties in meerdere regio's en één regio.

| **MongoDB 3.4** | **Azure Cosmos DB (multi-region)** | **Azure Cosmos DB (één regio)** |
| - | - | - |
| Linearizable | Sterk | Sterk |
| Meerderheid | Gebonden veroudering | Sterk |
| Lokaal | Consistent voorvoegsel | Consistent voorvoegsel |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over consistentieniveaus en compatibiliteit tussen Azure Cosmos DB-API's met de open-source-API's. Zie de volgende artikelen:

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [MongoDB-functies worden ondersteund door de Azure Cosmos DB-API voor MongoDB](mongodb-feature-support.md)
* [Apache Cassandra-functies worden ondersteund door de Cassandra-API van Azure Cosmos DB](cassandra-support.md)