---
title: Azure Cosmos DB-resources met behulp van Azure CLI beheren
description: Azure CLI gebruiken voor het beheren van uw Azure Cosmos DB-account, -database en -containers.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: d79cae16c109b407840b8565eb80fb87bbed6432
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877544"
---
# <a name="manage-azure-cosmos-db-resources-using-azure-cli"></a>Azure Cosmos DB-resources met behulp van Azure CLI beheren

De volgende handleiding wordt beschreven voor het automatiseren van beheer van uw Azure Cosmos DB-accounts, -databases en containers met behulp van Azure CLI-opdrachten. Dit omvat ook opdrachten voor het schalen van containerdoorvoer. Referentiepagina's voor alle Azure Cosmos DB CLI-opdrachten zijn beschikbaar in de [naslaginformatie voor Azure CLI](https://docs.microsoft.com/cli/azure/cosmosdb). U vindt ook meer voorbeelden van [Azure CLI-voorbeelden voor Azure Cosmos DB](cli-samples.md), waaronder het maken en beheren van Cosmos DB-accounts, -databases en -containers voor MongoDB, Gremlin, Cassandra en Table-API.

Met dit CLI-voorbeeldscript maakt u een Azure Cosmos DB SQL API-account, -database en -container.  

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

Open Azure CLI voor het maken van een Azure Cosmos DB-account met SQL-API, sessieconsistentie, meerdere masters ingeschakeld in VS-Oost en VS-West, of in de cloud shell en voer de volgende opdracht uit:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --kind GlobalDocumentDB \
   --default-consistency-level "Session" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="create-a-database"></a>Een database maken

Open Azure CLI voor het maken van een Cosmos DB-database, of in de cloud shell en voer de volgende opdracht uit:

```azurecli-interactive
az cosmosdb database create \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup"
```

## <a name="create-a-container"></a>Een container maken

Open Azure CLI voor het maken van een Cosmos DB-container met 1000 RU/s en een partitiesleutel, of in de cloud shell en voer de volgende opdracht uit:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --partition-key-path = "/myPartitionKey" \
   --throughput 1000
```

## <a name="change-the-throughput-of-a-container"></a>De doorvoer van een container wijzigen

Als u de doorvoer van een Cosmos DB-container wilt RU/s van 400, open Azure CLI of cloud shell en voer de volgende opdracht uit:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --throughput 400
```

## <a name="list-account-keys"></a>Een lijst met accountsleutels

Wanneer u een Azure Cosmos DB-account maakt, genereert de service twee master toegangssleutels die kunnen worden gebruikt voor verificatie wanneer het Azure Cosmos DB-account wordt geopend. Door twee toegangssleutels, kunt Azure Cosmos DB u de sleutels zonder onderbreking naar uw Azure Cosmos DB-account opnieuw genereren. Alleen-lezensleutels voor het verifiëren van alleen-lezen bewerkingen zijn ook beschikbaar. Er zijn twee sleutels voor lezen / schrijven (primaire en secundaire) en twee sleutels voor alleen-lezen (primaire en secundaire). U kunt de sleutels voor uw account krijgen door de volgende opdracht uit:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="list-connection-strings"></a>Lijst met verbindingsreeksen

De verbindingsreeks voor uw toepassing met de Cosmos DB-account verbinding kan worden opgehaald met de volgende opdracht.

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="regenerate-account-key"></a>Accountsleutel opnieuw genereren

Naar uw Azure Cosmos DB-account periodiek om te voorkomen dat uw verbindingen beter te beveiligen, moet u de toegangssleutels wijzigen. Twee toegangssleutels zijn zodat u verbindingen met de Azure Cosmos DB-account met behulp van één toegangssleutel terwijl u de toegangssleutel opnieuw genereren onderhouden toegewezen.

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup" \
   --key-kind primary
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI:

- [Azure-CLI installeren](/cli/azure/install-azure-cli)
- [Azure CLI-referentie](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Extra Azure CLI-voorbeelden voor Azure Cosmos DB](cli-samples.md)
