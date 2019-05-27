---
title: Azure Resource Manager-sjablonen voor Azure Cosmos DB Cassandra-API
description: Gebruik Azure Resource Manager-sjablonen maken en configureren van de Cassandra-API van Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: db754adbe60acfa155400910c47de556db793eef
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968914"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>Cassandra-API van Azure Cosmos DB-resources met behulp van Azure Resource Manager-sjablonen beheren

## Azure Cosmos-account, keyspace en tabel maken <a id="create-resource"></a>

Azure Cosmos DB-resources met behulp van een Azure Resource Manager-sjabloon maken. Deze sjabloon wordt een Azure Cosmos-account maken voor Cassandra-API met twee tabellen die delen van 400 RU/s doorvoer op de keyspace-niveau. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[create-cosmos-Cassandra](~/quickstart-templates/101-cosmosdb-cassandra/azuredeploy.json)]

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

Implementatie van de Resource Manager-sjabloon met behulp van Azure CLI, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the keyset name: ' keysetName
read -p 'Enter the first table name: ' table1Name
read -p 'Enter the second table name: ' table2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion keysetName=$keysetName \
   table1Name=$table1Name table2Name=$table2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

De `az cosmosdb show` opdracht geeft u het zojuist gemaakte Azure-Cosmos-account nadat deze is ingericht. Als u ervoor kiest een lokaal geïnstalleerde versie van Azure CLI gebruiken in plaats van CloudShell gebruikt, Zie [Azure-opdrachtregelinterface (CLI)](/cli/azure/) artikel.

## Doorvoer (RU/s) op een keyspace bijwerken <a id="keyspace-ru-update"></a>

De volgende sjabloon wordt de doorvoer van een keyspace bijgewerkt. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra-keyspace-ru-update/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[cosmosdb-cassandra-keyspace-ru-update](~/quickstart-templates/101-cosmosdb-cassandra-keyspace-ru-update/azuredeploy.json)]

### <a name="deploy-keyspace-template-via-azure-cli"></a>Keyspace-sjabloon via Azure CLI implementeren

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the keyspace name: ' keyspaceName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra-keyspace-ru-update/azuredeploy.json \
   --parameters accountName=$accountName keyspaceName=$keyspaceName throughput=$throughput
```

## Doorvoer (RU/s) in een tabel bijwerken <a id="table-ru-update"></a>

De volgende sjabloon wordt de doorvoer van een tabel bijgewerkt. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra-table-ru-update/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[cosmosdb-cassandra-table-ru-update](~/quickstart-templates/101-cosmosdb-cassandra-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-template-via-azure-cli"></a>Sjabloon via Azure CLI implementeren

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the keyspace name: ' keyspaceName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName keyspaceName=$keyspaceName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanvullende resources:

- [Azure Resource Manager-documentatie](/azure/azure-resource-manager/)
- [Azure Cosmos DB-resource providerschema](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB-Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Oplossen van veelvoorkomende fouten bij de implementatie van Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)