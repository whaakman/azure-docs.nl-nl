---
title: Azure Cosmos DB-resources met behulp van Azure CLI beheren
description: Azure CLI gebruiken voor het beheren van uw Azure Cosmos DB-account, -database en -containers.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 4/8/2019
ms.author: mjbrown
ms.openlocfilehash: 1d19e58b2d1381725de490b68d9e4d00a2ca4cb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66151214"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Beheer van Azure Cosmos-resources met behulp van Azure CLI

De volgende handleiding wordt beschreven veelgebruikte opdrachten voor het automatiseren van beheer van uw Azure Cosmos DB-accounts, -databases en containers met behulp van Azure CLI. Referentiepagina's voor alle Azure Cosmos DB CLI-opdrachten zijn beschikbaar in de [naslaginformatie voor Azure CLI](https://docs.microsoft.com/cli/azure/cosmosdb). U vindt ook meer voorbeelden van [Azure CLI-voorbeelden voor Azure Cosmos DB](cli-samples.md), waaronder het maken en beheren van Cosmos DB-accounts, -databases en -containers voor MongoDB, Gremlin, Cassandra en Table-API.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-an-azure-cosmos-db-account"></a>Een Azure Cosmos DB-account maken

Als u wilt een Azure Cosmos DB-account maken met SQL-API, sessieconsistentie in regio's VS-Oost en VS-West, voer de volgende opdracht:

```azurecli-interactive
az cosmosdb create \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations EastUS=0 WestUS=1 \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> De naam van de Azure Cosmos-account moet een kleine letter.

## <a name="create-a-database"></a>Een database maken

Voer de volgende opdracht voor het maken van een Cosmos DB-database:

```azurecli-interactive
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>Een container maken

Voor het maken van een Cosmos DB-container met RU/s van 400 en een partitiesleutel, voer de volgende opdracht:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --partition-key-path /myPartitionKey \
   --throughput 400
```

## <a name="change-the-throughput-of-a-container"></a>De doorvoer van een container wijzigen

Als u de doorvoer van een Cosmos DB-container wilt 1000 RU/s, kunt u de volgende opdracht uitvoeren:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --throughput 1000
```

## <a name="list-account-keys"></a>Een lijst met accountsleutels

Als u de sleutels voor uw Cosmos-account, moet u de volgende opdracht uitvoeren:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>Lijst met verbindingsreeksen

Als u de verbindingsreeksen voor uw Cosmos-account, moet u de volgende opdracht uitvoeren:

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>Accountsleutel opnieuw genereren

Als u wilt genereren van een nieuwe primaire sleutel voor uw Cosmos-account, moet u de volgende opdracht uitvoeren:

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --key-kind primary
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI:

- [Azure-CLI installeren](/cli/azure/install-azure-cli)
- [Azure CLI-referentie](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Extra Azure CLI-voorbeelden voor Azure Cosmos DB](cli-samples.md)
