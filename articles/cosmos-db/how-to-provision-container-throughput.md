---
title: Containerdoorvoer inrichten in Azure Cosmos DB
description: Meer informatie over het inrichten van doorvoer op containerniveau in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 4df8a12581b5d71a76964ca1e3d40c6c53185f67
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860317"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Doorvoer inrichten voor een Azure Cosmos-container

In dit artikel wordt uitgelegd hoe u doorvoer inricht voor een container (collectie, grafiek, tabel) in Azure Cosmos DB. U kunt de doorvoer inrichten voor één container of [inrichten voor een database](how-to-provision-database-throughput.md) en deze delen met de containers in de database. U kunt de doorvoer voor een container inrichten met behulp van de Azure Portal, Azure CLI of Cosmos DB-SDK's.

## <a name="provision-throughput-by-using-azure-portal"></a>Doorvoer inrichten met behulp van de Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos DB-account](create-sql-api-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe verzameling**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer de id in van een collectie (of tabel of grafiek).
   * Voer een waarde voor de partitiesleutel in (bijvoorbeeld `/userid`).
   * Voer een doorvoer in (bijvoorbeeld 1000 RU's).
   * Selecteer **OK**.

![Schermopname van Data Explorer met Nieuwe verzameling gemarkeerd](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-azure-cli"></a>Doorvoer inrichten met behulp van Azure CLI

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

Als u de doorvoer inricht voor een Azure Cosmos DB-account dat is geconfigureerd met de Azure Cosmos DB-API voor MongoDB, gebruikt u `/myShardKey` voor het pad van de partitiesleutel. Als u de doorvoer inricht voor een Azure Cosmos DB-account dat is geconfigureerd voor de Cassandra-API, gebruikt u `/myPrimaryKey` voor het pad van de partitiesleutel.

## <a name="provision-throughput-by-using-net-sdk"></a>Doorvoer inrichten met behulp van .NET SDK

> [!Note]
> Gebruik de SQL API voor het inrichten van doorvoer voor alle API's, met uitzondering van Cassandra-API.

### <a id="dotnet-most"></a>SQL-, MongoDB-, Gremlin- en Table-API's

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het inrichten van doorvoer in Azure Cosmos DB:

* [Doorvoer inrichten voor een database](how-to-provision-database-throughput.md)
* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)
