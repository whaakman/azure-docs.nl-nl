---
title: Maken en beheren van Azure Cosmos DB met behulp van Azure Resource Manager-sjablonen
description: Azure Resource Manager-sjablonen maken en configureren van Azure Cosmos DB voor SQL (Core) API gebruiken
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: mjbrown
ms.openlocfilehash: 5683fd072961c7793d8f4bbeb9ecc16a93dd7373
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242593"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>Azure Cosmos DB SQL (Core) API-resources met behulp van Azure Resource Manager-sjablonen beheren

## Een Azure Cosmos-account, -database en -container maken <a id="create-resource"></a>

Azure Cosmos DB-resources met behulp van een Azure Resource Manager-sjabloon maken. Deze sjabloon wordt een Azure Cosmos-account maken met twee containers die delen van 400 RU/s doorvoer op het databaseniveau van de. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

> [!NOTE]
> U niet op dit moment de gebruiker gedefinieerd Functions(UDFs), opgeslagen procedures en triggers implementeren met behulp van Resource Manager-sjablonen. 

[!code-json[create-cosmosdb-sql](~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>Implementeren via PowerShell

Implementatie van de Resource Manager-sjabloon met behulp van PowerShell, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$container1Name = Read-Host -Prompt "Enter the first container name"
$container2Name = Read-Host -Prompt "Enter the second container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -container1Name $container1Name `
    -container2Name $container2Name

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Als u ervoor kiest een lokaal geïnstalleerde versie van PowerShell in plaats van uit de Azure Cloud shell te gebruiken, hebt u [installeren](/powershell/azure/install-az-ps) de Azure PowerShell-module. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken.

### <a name="deploy-via-azure-cli"></a>Implementeren via Azure CLI

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first container name: ' container1Name
read -p 'Enter the second container name: ' container2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   container1Name=$container1Name container2Name=$container2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

De `az cosmosdb show` opdracht geeft u het zojuist gemaakte Azure-Cosmos-account nadat deze is ingericht. Als u ervoor kiest een lokaal geïnstalleerde versie van Azure CLI gebruiken in plaats van CloudShell gebruikt, Zie [Azure-opdrachtregelinterface (CLI)](/cli/azure/) artikel.

## Doorvoer (RU/s) voor een database bijwerken <a id="database-ru-update"></a>

De volgende sjabloon wordt de doorvoer van een database bijwerken. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-database-ru-update/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[cosmosdb-sql-database-ru-update](~/quickstart-templates/101-cosmosdb-sql-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-powershell"></a>Databasesjabloon via PowerShell implementeren

Implementatie van de Resource Manager-sjabloon met behulp van PowerShell, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$throughput = Read-Host -Prompt "Enter new throughput for database"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -throughput $throughput
```

### <a name="deploy-database-template-via-azure-cli"></a>Databasesjabloon via Azure CLI implementeren

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Doorvoer (RU/s) voor een container bijwerken <a id="container-ru-update"></a>

De volgende sjabloon wordt de doorvoer van een container bijgewerkt. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-ru-update/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[cosmosdb-sql-container-ru-update](~/quickstart-templates/101-cosmosdb-sql-container-ru-update/azuredeploy.json)]

### <a name="deploy-container-template-via-powershell"></a>Containersjabloon via PowerShell implementeren

Implementatie van de Resource Manager-sjabloon met behulp van PowerShell, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"
$throughput = Read-Host -Prompt "Enter new throughput for container"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -containerName $containerName `
    -throughput $throughput
```

### <a name="deploy-container-template-via-azure-cli"></a>Containersjabloon via Azure CLI implementeren

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName containerName=$containerName throughput=$throughput
```

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanvullende resources:

- [Azure Resource Manager-documentatie](/azure/azure-resource-manager/)
- [Azure Cosmos DB-resource providerschema](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB-Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Oplossen van veelvoorkomende fouten bij de implementatie van Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)