---
title: Een container maken in Azure Cosmos DB
description: Leer hoe u een container maakt in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 445d9f220a215eb17436d52f637b57bd3492aaae
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864771"
---
# <a name="create-an-azure-cosmos-container"></a>Een Azure Cosmos-container maken

In dit artikel worden de verschillende manieren uitgelegd waarop u een container (verzameling, tabel of grafiek) kunt maken. U gebruikt de Azure-portal, de Azure-CLI of ondersteunde SDK's. In dit artikel ziet u hoe u een container maakt, de partitiesleutel opgeeft en doorvoer inricht.

## <a name="create-a-container-by-using-azure-portal"></a>Een container maken met behulp van de Azure-portal

### <a id="portal-sql"></a>SQL-API

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos DB-account](create-sql-api-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe verzameling**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer een verzamelings-id in.
   * Voer een partitiesleutel in.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![Schermopname van het Data Explorer-deelvenster met Nieuwe verzameling gemarkeerd](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>Azure Cosmos DB-API voor MongoDB

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos DB-account](create-mongodb-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe verzameling**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer een verzamelings-id in.
   * Selecteer **onbeperkte** opslagcapaciteit.
   * Voer een shardsleutel in.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![Schermopname van Azure Cosmos DB-API voor MongoDB, dialoogvenster Verzameling toevoegen](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra-API

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos DB-account](create-cassandra-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe tabel**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe keyspace maakt of een bestaande keyspace gebruikt.
   * Voer een tabelnaam in.
   * Voer de eigenschappen in en geef een primaire sleutel op.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![Schermopname van Cassandra-API, dialoogvenster Tabel toevoegen](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Voor de Cassandra-API wordt de primaire sleutel gebruikt als de partitiesleutel.

### <a id="portal-gremlin"></a>Gremlin-API

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos DB-account](create-graph-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe grafiek**. Geef de volgende gegevens op:

   * Geef aan of u een nieuwe database maakt of een bestaande database gebruikt.
   * Voer een grafiek-id in.
   * Selecteer **onbeperkte** opslagcapaciteit.
   * Voer een partitiesleutel in voor hoekpunten.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![Schermopname van Gremlin-API, dialoogvenster Grafiek toevoegen](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Table-API

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure Cosmos DB-account](create-table-dotnet.md#create-a-database-account) of selecteer een bestaand account.

1. Open het deelvenster **Data Explorer** en selecteer **Nieuwe tabel**. Geef de volgende gegevens op:

   * Voer een tabel-id in.
   * Selecteer **onbeperkte** opslagcapaciteit.
   * Voer een doorvoer in, bijvoorbeeld 1000 RU's.
   * Selecteer **OK**.

![Schermopname van Table-API, dialoogvenster Grafiek toevoegen](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Voor de Table-API wordt de partitiesleutel opgegeven telkens wanneer u een nieuwe rij toevoegt.

## <a name="create-a-container-by-using-azure-cli"></a>Een container maken met behulp van Azure-CLI

### <a id="cli-sql"></a>SQL-API

```azurecli-interactive
# Create a container with a partition key and provision 1000 RU/s throughput.

az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-mongodb"></a>Azure Cosmos DB-API voor MongoDB

```azurecli-interactive
# Create a collection with a shard key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myShardKey \
    --throughput 1000
```

### <a id="cli-cassandra"></a>Cassandra-API

```azurecli-interactive
# Create a table with a partition/primary key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $keyspaceName \
    --partition-key-path /myPrimaryKey \
    --throughput 1000
```

### <a id="cli-gremlin"></a>Gremlin-API

```azurecli-interactive
# Create a graph with a partition key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $graphName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-table"></a>Table-API

```azurecli-interactive
# Create a table with 1000 RU/s
# Note: you don't need to specify partition key in the following command because the partition key is set on each row.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $databaseName \
    --throughput 1000
```

## <a name="create-a-container-by-using-net-sdk"></a>Een container maken met behulp van .NET SDK

### <a id="dotnet-sql-graph"></a>SQL-API en Gremlin-API

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-mongodb"></a>Azure Cosmos DB-API voor MongoDB

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
MongoDB-wire-protocol vraagt geen eenheden aan. Gebruik de Azure-portal of de SQL-API om een nieuwe verzameling te maken met doorvoer.

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Volgende stappen

- [Partitionering in Azure Cosmos DB](partitioning-overview.md)
