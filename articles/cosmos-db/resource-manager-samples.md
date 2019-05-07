---
title: Azure Resource Manager-sjablonen voor Azure Cosmos DB
description: Gebruik Azure Resource Manager-sjablonen maken en configureren van Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: c907de019b64a6a9d03206514a1147fd43c78106
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078458"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Resource Manager-sjablonen voor Azure Cosmos DB

De volgende tabel bevat koppelingen naar Azure Resource Manager-sjablonen voor Azure Cosmos DB:

|**API-type** | **koppeling naar voorbeeld**| **Beschrijving** |
|---|---| ---|
|Core (SQL)-API| [Maak een Azure Cosmos DB-account (meerdere masters)](manage-sql-with-resource-manager.md) | Deze sjabloon maakt een SQL API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee containers die op databaseniveau doorvoer delen. |
| MongoDB-API | [Maak een Azure Cosmos DB-account (meerdere masters)](manage-mongodb-with-resource-manager.md) | Deze sjabloon maakt een account met behulp van Azure Cosmos DB-API voor MongoDB in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee containers die op databaseniveau doorvoer delen. |
| Cassandra-API | [Maak een Azure Cosmos DB-account (meerdere masters)](manage-cassandra-with-resource-manager.md) | Deze sjabloon maakt een Cassandra-API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee tabellen die keyspace-niveau doorvoer delen. |
| Gremlin-API| [Maak een Azure Cosmos DB-account (meerdere masters)](manage-gremlin-with-resource-manager.md) | Deze sjabloon maakt een Gremlin-API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft twee grafieken die op databaseniveau doorvoer delen. |
| Tabel-API | [Maak een Azure Cosmos DB-account (meerdere masters)](manage-table-with-resource-manager.md) | Deze sjabloon maakt een tabel-API-account in twee regio's met meerdere masters ingeschakeld. De Azure Cosmos-account heeft één tabel. |

> [!TIP]
> Als u gedeelde doorvoer bij het gebruik van Table-API, wilt inschakelen op accountniveau doorvoer in de Azure Portal.

Zie [ARM-verwijzing voor Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) -pagina voor de referentiedocumentatie.