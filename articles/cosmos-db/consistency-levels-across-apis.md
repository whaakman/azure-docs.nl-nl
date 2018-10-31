---
title: Consistentieniveaus en Azure Cosmos DB-API's | Microsoft Docs
description: Informatie over de consistentieniveaus voor API's in Azure Cosmos DB.
keywords: consistentie, azure cosmosdb, azure, modellen, mongodb, cassandra, grafiek, tabel, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: ed08b90b9e216ee8713bfe445e98144bf2ba02d4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244190"
---
# <a name="consistency-levels-and-cosmos-db-apis"></a>Consistentieniveaus en Cosmos DB-API 's

De vijf consistentiemodellen systeemeigen worden ondersteund door de SQL-API, die de standaard-API is bij het gebruik van Cosmos DB. Naast de SQL-API biedt Cosmos DB ook systeemeigen ondersteuning voor wire-protocol compatibele API's voor populaire databases, zoals MongoDB, Apache Cassandra, Gremlin en Azure-tabellen. Deze databases bieden geen van beide consistentiemodellen nauwkeurig gedefinieerd en de SLA's gesteunde garanties voor de consistentieniveaus en bieden doorgaans slechts een subset van de vijf consistentiemodellen Cosmos DB biedt. Voor SQL-API, Gremlin-API en Table-API, wordt het standaardconsistentieniveau geconfigureerd op de Cosmos-account gebruikt.

De volgende tabel ziet u de toewijzing tussen de gegevensconsistentie aangevraagd door een OSS-client-stuurprogramma voor Apache Cassandra 4.x en MongoDB 3.4 bij het gebruik van Cassandra-API en MongoDB-API, respectievelijk en de bijbehorende Cosmos DB-consistentieniveaus.

## <a id="cassandra-mapping"></a>Toewijzing van Apache Cassandra en Cosmos DB-consistentieniveaus

De onderstaande tabel ziet u de toewijzing voor het lezen van consistentie tussen Apache Cassandra 4.x-client en Cosmos DB 'Standaard' consistentieniveau voor zowel een implementatie van meerdere regio's en één regio.

| **Apache Cassandra 4.x** | **Cosmos DB (meerdere regio's)** | **Cosmos DB (één regio)** |
| - | - | - |
| EEN TWEE DRIE | Consistent prefix | Consistent prefix |
| LOCAL_ONE | Consistent prefix | Consistent prefix |
| QUORUM, ALLE, SERIENUMMER | Gebonden veroudering (standaard) of sterk (in private preview) | Sterk |
| LOCAL_QUORUM | Gebonden veroudering | Sterk |
| LOCAL_SERIAL | Gebonden veroudering | Sterk |

## <a id="mongo-mapping"></a>Toewijzing tussen MongoDB 3.4 en Cosmos DB-consistentieniveaus

De onderstaande tabel ziet u de toewijzing voor 'lezen problemen"van MongoDB 3.4 en Cosmos DB 'Standaard' consistentieniveau voor zowel een implementatie van meerdere regio's en één regio.

| **MongoDB 3.4** | **Cosmos DB (meerdere regio's)** | **Cosmos DB (één regio)** |
| - | - | - |
| Linearizable | Sterk | Sterk |
| Meerderheid | Gebonden veroudering | Sterk |
| Lokaal | Consistent prefix | Consistent prefix |

## <a name="next-steps"></a>Volgende stappen

Lees meer informatie over consistentieniveaus en compatibiliteit tussen Cosmos DB-API's met de open-source-API's in de volgende artikelen:

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [MongoDB-functies worden ondersteund door Cosmos DB MongoDB-API](mongodb-feature-support.md)
* [Apache Cassandra-functies worden ondersteund door Cosmos DB Cassandra-API](cassandra-support.md)