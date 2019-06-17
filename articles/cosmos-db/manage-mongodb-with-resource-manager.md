---
title: Azure Resource Manager-sjablonen voor Azure Cosmos DB-API voor MongoDB
description: Gebruik Azure Resource Manager-sjablonen maken en configureren van Azure Cosmos DB API voor MongoDB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 99f1e41107c277c8b3f1b21f81952d5d5cadaa29
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65968864"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Azure Cosmos DB MongoDB API-resources met behulp van Azure Resource Manager-sjablonen beheren

## Azure Cosmos DB API voor MongoDB-account, -database en verzameling maken <a id="create-resource"></a>

Azure Cosmos DB-resources met behulp van een Azure Resource Manager-sjabloon maken. Deze sjabloon wordt een Azure Cosmos-account voor MongoDB-API maken met twee verzamelingen die delen van 400 RU/s doorvoer op het databaseniveau van de. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[create-cosmos-mongo](~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json)]

### <a name="deploy-via-azure-cli"></a>Implementeren via Azure CLI

Implementatie van de Resource Manager-sjabloon met behulp van Azure CLI, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first collection name: ' collection1Name
read -p 'Enter the second collection name: ' collection2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb/azuredeploy.json \
  --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion \
  databaseName=$databaseName collection1Name=$collection1Name collection2Name=$collection2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

De `az cosmosdb show` opdracht geeft u het zojuist gemaakte Azure-Cosmos-account nadat deze is ingericht. Als u ervoor kiest een lokaal geïnstalleerde versie van Azure CLI gebruiken in plaats van CloudShell gebruikt, Zie [Azure-opdrachtregelinterface (CLI)](/cli/azure/) artikel.

## Doorvoer (RU/s) voor een database bijwerken <a id="database-ru-update"></a>

De volgende sjabloon wordt de doorvoer van een database bijwerken. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-database-ru-update/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[cosmosdb-mongodb-database-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-azure-cli"></a>Databasesjabloon via Azure CLI implementeren

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Doorvoer (RU/s) op een verzameling bijwerken <a id="collection-ru-update"></a>

De volgende sjabloon wordt de doorvoer van een verzameling bijwerken. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-collection-ru-update/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[cosmosdb-mongodb-collection-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json)]

### <a name="deploy-collection-template-via-azure-cli"></a>Verzameling sjabloon via Azure CLI implementeren

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the collection name: ' collectionName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName collectionName=$collectionName throughput=$throughput
```

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanvullende resources:

- [Azure Resource Manager-documentatie](/azure/azure-resource-manager/)
- [Azure Cosmos DB-resource providerschema](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB-Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Oplossen van veelvoorkomende fouten bij de implementatie van Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)