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
ms.openlocfilehash: 974531cd5907e4f69e7d064125d3e51fa4974949
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50956380"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Consistentieniveaus en Azure Cosmos DB-API 's

De vijf consistentiemodellen aangeboden door Azure Cosmos DB worden ondersteund door de Cosmos DB SQL API, die de standaard-API is bij het gebruik van Cosmos DB. Naast de SQL-API biedt Cosmos DB ook systeemeigen ondersteuning voor wire-protocol compatibele API's voor populaire databases, zoals MongoDB, Apache Cassandra, Gremlin en Azure-tabellen. Deze databases geen van beide nauwkeurig bieden gedefinieerd consistentiemodellen, noch de SLA's gesteunde garanties voor de consistentieniveaus. Deze databases bevatten doorgaans slechts een subset van de vijf consistentiemodellen aangeboden door Cosmos DB. Voor SQL-API, Gremlin-API en Table-API, wordt het standaardconsistentieniveau die u op de Cosmos-account configureert gebruikt.

De volgende secties ziet u de toewijzing tussen de gegevensconsistentie aangevraagd door een OSS-client-stuurprogramma voor Apache Cassandra 4.x en MongoDB 3.4 bij het gebruik van Cassandra-API en MongoDB-API, respectievelijk en de bijbehorende Cosmos DB-consistentieniveaus.

## <a id="cassandra-mapping"></a>Toewijzing tussen Apache Cassandra en Cosmos DB-consistentieniveaus

De volgende tabel bevat de toewijzing "lezen van consistentie" tussen Apache Cassandra 4.x-client en het standaardconsistentieniveau in Cosmos DB voor zowel meerdere regio's als één regio implementaties.

| **Apache Cassandra 4.x** | **Cosmos DB (meerdere regio's)** | **Cosmos DB (één regio)** |
| - | - | - |
| EEN TWEE DRIE | Consistent prefix | Consistent prefix |
| LOCAL_ONE | Consistent prefix | Consistent prefix |
| QUORUM, ALLE, SERIENUMMER | Gebonden veroudering (standaard) of sterk (in private preview) | Sterk |
| LOCAL_QUORUM | Gebonden veroudering | Sterk |
| LOCAL_SERIAL | Gebonden veroudering | Sterk |

## <a id="mongo-mapping"></a>Toewijzing tussen MongoDB 3.4 en Cosmos DB-consistentieniveaus

De volgende tabel bevat de toewijzing 'lezen problemen' tussen MongoDB 3.4 en het standaardconsistentieniveau in Cosmos DB voor zowel meerdere regio's als één regio implementaties.

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