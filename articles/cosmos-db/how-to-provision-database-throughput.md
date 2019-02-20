---
title: Databasedoorvoer inrichten in Azure Cosmos DB
description: Meer informatie over het inrichten van doorvoer op databaseniveau in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 759adf95604e66209cf3ec5083246d16e952114a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884185"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>Doorvoer inrichten voor een database in Azure Cosmos DB

In dit artikel wordt uitgelegd hoe u doorvoer inricht voor een database in Azure Cosmos DB. U kunt de doorvoer inrichten voor één [container](how-to-provision-container-throughput.md) of inrichten voor een database en de doorvoer delen met de containers in de database. U kunt doorvoer op databaseniveau inrichten met behulp van de Azure-portal of Azure Cosmos DB-SDK's.

## <a name="provision-throughput-by-using-azure-portal"></a>Doorvoer inrichten met behulp van de Azure-portal

### <a id="portal-sql"></a>SQL (Core) API

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos DB-account](create-sql-api-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe database**. Geef de volgende gegevens op:

   * Voer een database-id in. 
   * Selecteer **Doorvoer inrichten**.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![Schermafbeelding van het dialoogvenster Nieuwe database](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-net-sdk"></a>Doorvoer inrichten met behulp van .NET SDK

> [!Note]
> Gebruik de SQL API voor het inrichten van doorvoer voor alle API's. U kunt ook het volgende voorbeeld voor Cassandra-API gebruiken.

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

Zie de volgende artikelen voor meer informatie over het inrichten van doorvoer in Azure Cosmos DB:

* [Doorvoer inrichten voor een container](how-to-provision-container-throughput.md)
* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)
