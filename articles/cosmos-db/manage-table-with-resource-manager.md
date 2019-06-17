---
title: Azure Resource Manager-sjablonen voor Azure Cosmos DB Table-API
description: Gebruik Azure Resource Manager-sjablonen maken en configureren van Azure Cosmos DB Table-API.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 82e2a436bf6b25b6164d845d234896390a262292
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65968821"
---
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>Azure Cosmos DB Table-API-resources met behulp van Azure Resource Manager-sjablonen beheren

## Azure Cosmos-account en een tabel maken <a id="create-resource"></a>

Azure Cosmos DB-resources met behulp van een Azure Resource Manager-sjabloon maken. Deze sjabloon wordt een Azure Cosmos-account maken voor tabel-API met een tabel met een doorvoer van 400 RU/s. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-table/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[create-cosmos-table](~/quickstart-templates/101-cosmosdb-table/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>Implementeren via PowerShell

Implementatie van de Resource Manager-sjabloon met behulp van PowerShell, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$tableName = Read-Host -Prompt "Enter the table name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json" `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -tableName $tableName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Als u ervoor kiest een lokaal geïnstalleerde versie van PowerShell in plaats van uit de Azure Cloud shell te gebruiken, hebt u [installeren](/powershell/azure/install-az-ps) de Azure PowerShell-module. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken.

### <a name="deploy-via-azure-cli"></a>Implementeren via Azure CLI

Implementatie van de Resource Manager-sjabloon met behulp van Azure CLI, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the table name: ' tableName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion tableName=$tableName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

De `az cosmosdb show` opdracht geeft u het zojuist gemaakte Azure-Cosmos-account nadat deze is ingericht. Als u ervoor kiest een lokaal geïnstalleerde versie van Azure CLI gebruiken in plaats van CloudShell gebruikt, Zie [Azure-opdrachtregelinterface (CLI)](/cli/azure/) artikel.

## Doorvoer (RU/s) in een tabel bijwerken <a id="table-ru-update"></a>

De volgende sjabloon wordt de doorvoer van een tabel bijgewerkt. De sjabloon te kopiëren en implementeren zoals hieronder wordt weergegeven of Ga naar [Azure Snelstartgalerie](https://azure.microsoft.com/resources/templates/101-cosmosdb-table-ru-update/) en implementeren vanuit Azure portal. U kunt ook de sjabloon downloaden naar uw lokale computer of maak een nieuwe sjabloon en geef het lokale pad op met de `--template-file` parameter.

[!code-json[cosmosdb-table-ru-update](~/quickstart-templates/101-cosmosdb-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-throughput-via-powershell"></a>Tabel doorvoer via PowerShell implementeren

Implementatie van de Resource Manager-sjabloon met behulp van PowerShell, **kopie** het script en selecteer **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$tableName = Read-Host -Prompt "Enter the table name"
$throughput = Read-Host -Prompt "Enter new throughput for table"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -tableName $tableName `
    -throughput $throughput
```

### <a name="deploy-table-template-via-azure-cli"></a>Sjabloon via Azure CLI implementeren

Selecteer voor het implementeren van de Resource Manager-sjabloon met behulp van Azure CLI, **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanvullende resources:

- [Azure Resource Manager-documentatie](/azure/azure-resource-manager/)
- [Azure Cosmos DB-resource providerschema](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB-Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Oplossen van veelvoorkomende fouten bij de implementatie van Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)