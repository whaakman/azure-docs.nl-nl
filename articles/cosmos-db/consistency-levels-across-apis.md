---
title: Consistentieniveaus en Azure Cosmos DB-API's
description: Informatie over de consistentieniveaus voor API's in Azure Cosmos DB.
keywords: consistentie, azure cosmosdb, azure, modellen, mongodb, cassandra, grafiek, tabel, Microsoft azure
services: cosmos-db
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.openlocfilehash: 2fde29111ae1a95aec0cfef93762d3ae4f0e0dd0
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53809383"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Consistentieniveaus en Azure Cosmos DB-API's

Vijf consistentiemodellen aangeboden door Azure Cosmos DB worden ondersteund door de Azure Cosmos DB SQL API. Wanneer u Azure Cosmos DB gebruikt, is de SQL-API de standaardinstelling. 

Azure Cosmos DB biedt ook ingebouwde ondersteuning voor wire-protocol-compatibele API's voor populaire-databases. Databases omvatten MongoDB, Apache Cassandra, Gremlin en Azure Table storage. Deze databases bieden niet nauwkeurig omschreven consistentiemodellen of SLA's gesteunde garanties voor consistentieniveaus. Ze bieden doorgaans slechts een subset van de vijf consistentiemodellen aangeboden door Azure Cosmos DB. Voor de SQL-API, Gremlin-API en Table-API, wordt het standaardconsistentieniveau geconfigureerd op het Azure Cosmos DB-account gebruikt. 

De volgende secties ziet u de toewijzing tussen de consistentie van de gegevens aangevraagd door een OSS-client-stuurprogramma voor Apache Cassandra 4.x en MongoDB 3.4. Dit document bevat ook de bijbehorende Azure Cosmos DB-consistentieniveaus voor Apache Cassandra en MongoDB.

## <a id="cassandra-mapping"></a>Toewijzing tussen Apache Cassandra en Azure Cosmos DB-consistentieniveaus

Deze tabel bevat de toewijzing 'lezen consistentie' tussen de Apache Cassandra 4.x-client en het standaardconsistentieniveau in Azure Cosmos DB. De tabel ziet u implementaties in meerdere regio's en één regio.

| **Apache Cassandra 4.x** | **Azure Cosmos DB (meerdere regio's)** | **Azure Cosmos DB (één regio)** |
| - | - | - |
| EEN TWEE DRIE | Consistent voorvoegsel | Consistent voorvoegsel |
| LOCAL_ONE | Consistent voorvoegsel | Consistent voorvoegsel |
| QUORUM, ALLE, SERIENUMMER | Gebonden veroudering is de standaardinstelling. Er is een sterke in private preview. | Sterk |
| LOCAL_QUORUM | Gebonden veroudering | Sterk |
| LOCAL_SERIAL | Gebonden veroudering | Sterk |

## <a id="mongo-mapping"></a>Toewijzing tussen MongoDB 3.4 en Azure Cosmos DB-consistentieniveaus

De volgende tabel ziet u de toewijzing 'lezen problemen' tussen MongoDB 3.4 en het standaardconsistentieniveau in Azure Cosmos DB. De tabel ziet u implementaties in meerdere regio's en één regio.

| **MongoDB 3.4** | **Azure Cosmos DB (meerdere regio's)** | **Azure Cosmos DB (één regio)** |
| - | - | - |
| Linearizable | Sterk | Sterk |
| Meerderheid | Gebonden veroudering | Sterk |
| Lokaal | Consistent voorvoegsel | Consistent voorvoegsel |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over consistentieniveaus en compatibiliteit tussen Azure Cosmos DB-API's met de open-source-API's. Zie de volgende artikelen:

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [MongoDB-functies worden ondersteund door de Azure Cosmos DB-API voor MongoDB](mongodb-feature-support.md)
* [Apache Cassandra-functies worden ondersteund door de Cassandra-API van Azure Cosmos DB](cassandra-support.md)