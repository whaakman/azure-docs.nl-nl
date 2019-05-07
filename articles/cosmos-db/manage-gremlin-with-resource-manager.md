---
title: Azure Resource Manager-sjablonen voor Gremlin-API van Azure Cosmos DB
description: Gebruik Azure Resource Manager-sjablonen maken en configureren van de Gremlin-API van Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: d1928606a22eba180ebd3f1e979362e75edaf2d7
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077783"
---
# <a name="create-azure-cosmos-db-gremlin-api-resources-from-a-resource-manager-template"></a>Gremlin-API van Azure Cosmos DB-resources van Resource Manager-sjabloon maken

Informatie over het maken van een Gremlin-API van Azure Cosmos DB-resources met behulp van een Azure Resource Manager-sjabloon. Het volgende voorbeeld wordt een Azure Cosmos DB Gremlin-API van een [Azure Quickstart-sjabloon](https://aka.ms/gremlin-arm-qs). Deze sjabloon wordt een Azure Cosmos-account maken voor Gremlin-API met twee grafieken die delen van 400 RU/s doorvoer op het databaseniveau van de.

Hier volgt een kopie van de sjabloon:

[!code-json[create-cosmos-gremlin](~/quickstart-templates/101-cosmosdb-gremlin/azuredeploy.json)]

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

Implementatie van de Resource Manager-sjabloon met behulp van Azure CLI, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first graph name: ' graph1Name
read -p 'Enter the second graph name: ' graph2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   graph1Name=$graph1Name graph2Name=$graph2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

De `az cosmosdb show` opdracht geeft u het zojuist gemaakte Azure-Cosmos-account nadat deze is ingericht. Als u ervoor kiest een lokaal geïnstalleerde versie van Azure CLI gebruiken in plaats van CloudShell gebruikt, Zie [Azure-opdrachtregelinterface (CLI)](/cli/azure/) artikel.

In het vorige voorbeeld hebt u een sjabloon die is opgeslagen in GitHub een waarnaar wordt verwezen. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanvullende resources:

- [Azure Resource Manager-documentatie](/azure/azure-resource-manager/)
- [Azure Cosmos DB-resource providerschema](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB-Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Oplossen van veelvoorkomende fouten bij de implementatie van Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)