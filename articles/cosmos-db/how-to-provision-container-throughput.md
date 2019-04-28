---
title: Containerdoorvoer inrichten in Azure Cosmos DB
description: Meer informatie over het inrichten van doorvoer op containerniveau in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: d092844fbd75fc2307cc13ec9b7779ae9237535d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61054631"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Doorvoer inrichten voor een Azure Cosmos-container

In dit artikel wordt uitgelegd hoe u de doorvoer voor een container (verzameling, grafiek of tabel) in Azure Cosmos DB inrichten. U kunt de doorvoer op een enkele container inrichten of [doorvoer voor een database inrichten](how-to-provision-database-throughput.md) en deze delen met de containers in de database. U kunt de doorvoer voor een container met behulp van Azure portal, Azure CLI of Azure Cosmos DB SDK's inrichten.

## <a name="provision-throughput-using-azure-portal"></a>Doorvoer inrichten met behulp van de Azure-portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos-account](create-sql-api-dotnet.md#create-account), of Selecteer een bestaand Azure-Cosmos-account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe verzameling**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer een id-Container (of tabel of grafiek).
   * Voer een waarde voor de partitiesleutel in (bijvoorbeeld `/userid`).
   * Voer een doorvoer die u wilt inrichten (bijvoorbeeld 1000 ru's).
   * Selecteer **OK**.

![Schermopname van Data Explorer met Nieuwe verzameling gemarkeerd](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli"></a>Doorvoer inrichten met behulp van Azure CLI

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

Als u doorvoer voor een container in een Azure Cosmos-account dat is geconfigureerd met de Azure Cosmos DB-API voor MongoDB inricht, gebruikt u `/myShardKey` voor het pad van de partitie. Als u doorvoer voor een container in een Azure Cosmos-account dat is geconfigureerd met de Cassandra-API inricht, gebruikt u `/myPrimaryKey` voor het pad van de partitie.

## <a name="provision-throughput-by-using-net-sdk"></a>Doorvoer inrichten met behulp van .NET SDK

> [!Note]
> Gebruik de Cosmos-SDK's voor SQL-API doorvoer voor het inrichten voor alle Cosmos DB-API's, met uitzondering van de Cassandra-API.

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

* [Hoe u de doorvoer voor een database inrichten](how-to-provision-database-throughput.md)
* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)
