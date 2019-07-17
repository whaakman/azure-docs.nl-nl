---
title: Databasedoorvoer inrichten in Azure Cosmos DB
description: Meer informatie over het inrichten van doorvoer op databaseniveau in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/03/2019
ms.author: rimman
ms.openlocfilehash: 2744422e2e082c5bc6f63975b1100f336d32d5fa
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68250065"
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


## <a name="provision-throughput-using-azure-cli"></a>Doorvoer inrichten met behulp van Azure CLI

```azcli-interactive
az cosmosdb database create --db-name
                            [--key]
                            [--name]
                            [--resource-group-name]
                            [--subscription]
                            [--throughput]
                            [--url-connection]
```




## <a name="provision-throughput-using-powershell"></a>Doorvoer van inrichten met behulp van PowerShell

```azurepowershell-interactive
# Create a database and provision throughput of 400 RU/s
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$databaseResourceName = $accountName + "/sql/" + $databaseName

$databaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"= 400 }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName -PropertyObject $databaseProperties
```

## <a name="provision-throughput-using-net-sdk"></a>Doorvoer inrichten met behulp van .NET SDK

> [!Note]
> U kunt Cosmos-SDK's voor SQL-API gebruiken doorvoer voor het inrichten voor alle API's. U kunt ook het volgende voorbeeld voor Cassandra-API gebruiken.

### <a id="dotnet-all"></a>Alle API's
### <a name="net-v2-sdk"></a>.Net V2 SDK

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a name="net-v3-sdk"></a>.NET V3 SDK
[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400);
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over de ingerichte doorvoer in Azure Cosmos DB:

* [Ingerichte doorvoer wereldwijd schalen](scaling-throughput.md)
* [Doorvoer voor containers en databases inrichten](set-throughput.md)
* [Doorvoer inrichten voor een container](how-to-provision-container-throughput.md)
* [Aanvraageenheden en doorvoer in Azure Cosmos DB](request-units.md)
