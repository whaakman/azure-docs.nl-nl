---
title: Databasedoorvoer inrichten in Azure Cosmos DB
description: Meer informatie over het inrichten van doorvoer op databaseniveau in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: rimman
ms.openlocfilehash: d73dd5ffe8cc8ed00288209b628d7175b795b335
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243758"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>Doorvoer van een database in Azure Cosmos DB inrichten

In dit artikel wordt uitgelegd hoe u de doorvoer voor een database in Azure Cosmos DB inrichten. U kunt de doorvoer inrichten voor één [container](how-to-provision-container-throughput.md) of inrichten voor een database en de doorvoer delen met de containers in de database. Zie voor meer informatie wanneer u de container op serverniveau en databaseniveau doorvoer, de [Use cases voor het inrichten van doorvoer voor containers en databases](set-throughput.md) artikel. U kunt doorvoer op databaseniveau inrichten met behulp van de Azure-portal of Azure Cosmos DB-SDK's.

## <a name="provision-throughput-using-azure-portal"></a>Doorvoer inrichten met behulp van de Azure-portal

### <a id="portal-sql"></a>SQL (Core) API

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-sql-api-dotnet.md#create-account), of Selecteer een bestaand Azure-Cosmos-account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe database**. Geef de volgende gegevens op:

   * Voer een database-id in. 
   * Selecteer **Doorvoer inrichten**.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![Schermafbeelding van het dialoogvenster Nieuwe database](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>Doorvoer inrichten met behulp van .NET SDK

> [!Note]
> U kunt Cosmos-SDK's voor SQL-API gebruiken doorvoer voor het inrichten voor alle API's. U kunt ook het volgende voorbeeld voor Cassandra-API gebruiken.

### <a id="dotnet-all"></a>Alle API's

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over de ingerichte doorvoer in Azure Cosmos DB:

* [Ingerichte doorvoer wereldwijd schalen](scaling-throughput.md)
* [Doorvoer voor containers en databases inrichten](set-throughput.md)
* [Doorvoer inrichten voor een container](how-to-provision-container-throughput.md)
* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)