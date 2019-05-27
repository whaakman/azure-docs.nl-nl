---
title: Azure Resource Manager-sjablonen voor Azure Cosmos DB
description: Gebruik Azure Resource Manager-sjablonen maken en configureren van Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 4a32b0497d2457a740e9c082f990bb9112208bfd
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969180"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Resource Manager-sjablonen voor Azure Cosmos DB

De volgende tabellen bevatten koppelingen naar Azure Resource Manager-sjablonen voor Azure Cosmos DB:

## <a name="sql-core-api"></a>SQL (Core) API

|**Sjabloon**|**Beschrijving**|
|---| ---|
|[Een Azure Cosmos-account, de database, de container maken](manage-sql-with-resource-manager.md#create-resource) | Deze sjabloon maakt een SQL (Core) API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee containers die op databaseniveau doorvoer delen. |
|[Doorvoer (RU/s) voor een database bijwerken](manage-sql-with-resource-manager.md#database-ru-update) | Deze sjabloon de doorvoer voor een database in een SQL (Core) API-account bijgewerkt. |
|[Doorvoer (RU/s) voor een container bijwerken](manage-sql-with-resource-manager.md#container-ru-update) | Deze sjabloon de doorvoer voor een container in een SQL (Core) API-account bijgewerkt. |

## <a name="mongodb-api"></a>MongoDB-API

|**Sjabloon**|**Beschrijving**|
|---| ---|
|[Een Azure Cosmos-account, -database en verzameling maken](manage-mongodb-with-resource-manager.md#create-resource) | Deze sjabloon maakt een account met behulp van Azure Cosmos DB API voor MongoDB in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee containers die op databaseniveau doorvoer delen. |
|[Doorvoer (RU/s) voor een database bijwerken](manage-mongodb-with-resource-manager.md#database-ru-update) | Deze sjabloon de doorvoer voor een database in een MongoDB-API-account bijgewerkt. |
|[Doorvoer (RU/s) voor een verzameling bijwerken](manage-mongodb-with-resource-manager.md#collection-ru-update) | Deze sjabloon de doorvoer voor een container in een MongoDB-API-account bijgewerkt. |

## <a name="cassandra-api"></a>Cassandra-API

|**Sjabloon**|**Beschrijving**|
|---| ---|
|[Een Azure Cosmos-account, een keyspace en een tabel maken](manage-cassandra-with-resource-manager.md#create-resource) | Deze sjabloon maakt een Cassandra-API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee tabellen die keyspace-niveau doorvoer delen. |
|[Doorvoer (RU/s) voor een keyspace bijwerken](manage-cassandra-with-resource-manager.md#keyspace-ru-update) | Deze sjabloon de doorvoer voor een keyspace in een Cassandra-API-account bijgewerkt. |
|[Doorvoer (RU/s) voor een tabel bijwerken](manage-cassandra-with-resource-manager.md#table-ru-update) | Deze sjabloon de doorvoer voor een tabel in een Cassandra-API-account bijgewerkt. |

## <a name="gremlin-api"></a>Gremlin-API

|**Sjabloon**|**Beschrijving**|
|---| ---|
|[Een Azure Cosmos-account, een database, een grafiek maken](manage-gremlin-with-resource-manager.md#create-resource) | Deze sjabloon maakt een Gremlin-API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee grafieken die op databaseniveau doorvoer delen. |
|[Doorvoer (RU/s) voor een database bijwerken](manage-gremlin-with-resource-manager.md#database-ru-update) | Deze sjabloon de doorvoer voor een database in een Gremlin-API-account bijgewerkt. |
|[Doorvoer (RU/s) voor een grafiek bijwerken](manage-gremlin-with-resource-manager.md#graph-ru-update) | Deze sjabloon de doorvoer voor een grafiek in een Gremlin-API-account bijgewerkt. |

## <a name="table-api"></a>Tabel-API

|**Sjabloon**|**Beschrijving**|
|---| ---|
|[Maak een Azure Cosmos-account, tabel](manage-table-with-resource-manager.md#create-resource) | Deze sjabloon maakt een tabel-API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft één tabel. |
|[Doorvoer (RU/s) voor een tabel bijwerken](manage-table-with-resource-manager.md#table-ru-update) | Deze sjabloon de doorvoer voor een tabel in een tabel-API-account bijgewerkt. |

> [!TIP]
> Als u gedeelde doorvoer bij het gebruik van Table-API, wilt inschakelen op accountniveau doorvoer in de Azure Portal.

Zie [ARM-verwijzing voor Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) -pagina voor de referentiedocumentatie.