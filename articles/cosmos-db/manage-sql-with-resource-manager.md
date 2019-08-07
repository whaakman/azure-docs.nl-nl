---
title: Azure Cosmos DB maken en beheren met behulp van Azure Resource Manager sjablonen
description: Azure Resource Manager sjablonen gebruiken om Azure Cosmos DB te maken en te configureren voor de SQL-API (core)
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: b4d121e0628512f7bbd6aedc0a9067b31d46d0ed
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814969"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>Azure Cosmos DB SQL-API-bronnen (kern geheugen) beheren met Azure Resource Manager sjablonen

## Een Azure Cosmos-account,-data base en-container maken<a id="create-resource"></a>

Azure Cosmos DB resources maken met behulp van een Azure Resource Manager sjabloon. Met deze sjabloon wordt een Azure Cosmos-account gemaakt met twee containers die een door Voer van 400 RU/s op database niveau delen. Kopieer de sjabloon en implementeer deze zoals hieronder wordt weer gegeven of ga naar de [Galerie van Azure Quick](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) start en implementeer deze vanuit de Azure Portal. U kunt de sjabloon ook downloaden naar uw lokale computer of een nieuwe sjabloon maken en het lokale pad met de `--template-file` para meter opgeven.

> [!NOTE]
>
> - U kunt op dit moment geen door de gebruiker gedefinieerde functies (Udf's), opgeslagen procedures en triggers implementeren met behulp van Resource Manager-sjablonen.
> - U kunt geen locaties tegelijkertijd toevoegen aan of verwijderen uit een Azure Cosmos-account en andere eigenschappen wijzigen. Deze moeten als afzonderlijke bewerkingen worden uitgevoerd.
> - Account namen moeten kleine letters en < 31 tekens zijn.

[!code-json[create-cosmosdb-sql](~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>Implementeren via Power shell

Als u de Resource Manager-sjabloon wilt implementeren met behulp van Power shell, **kopieert** u het script en selecteert u **proberen** de Azure Cloud shell te openen. Als u het script wilt plakken, klikt u met de rechter muisknop op de shell en selecteert u vervolgens **Plakken**:

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

Als u ervoor kiest om een lokaal geïnstalleerde versie van Power shell te gebruiken in plaats van vanuit de Azure Cloud shell, moet u de Azure PowerShell-module [installeren](/powershell/azure/install-az-ps) . Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken.

### <a name="deploy-via-azure-cli"></a>Implementeren via Azure CLI

Als u de Resource Manager-sjabloon wilt implementeren met behulp van Azure CLI, selecteert u **proberen** de Azure Cloud shell te openen. Als u het script wilt plakken, klikt u met de rechter muisknop op de shell en selecteert u vervolgens **Plakken**:

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

De `az cosmosdb show` opdracht toont het zojuist gemaakte Azure Cosmos-account nadat het is ingericht. Als u ervoor kiest een lokaal geïnstalleerde versie van Azure CLI te gebruiken in plaats van Cloud shell, raadpleegt u het artikel [Azure Command-Line Interface (CLI)](/cli/azure/) .

## De door Voer (RU/s) bijwerken voor een Data Base<a id="database-ru-update"></a>

Met de volgende sjabloon wordt de door Voer van een Data Base bijgewerkt. Kopieer de sjabloon en implementeer deze zoals hieronder wordt weer gegeven of ga naar de [Galerie van Azure Quick](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-database-ru-update/) start en implementeer deze vanuit de Azure Portal. U kunt de sjabloon ook downloaden naar uw lokale computer of een nieuwe sjabloon maken en het lokale pad met de `--template-file` para meter opgeven.

[!code-json[cosmosdb-sql-database-ru-update](~/quickstart-templates/101-cosmosdb-sql-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-powershell"></a>Database sjabloon implementeren via Power shell

Als u de Resource Manager-sjabloon wilt implementeren met behulp van Power shell, **kopieert** u het script en selecteert u **proberen** de Azure Cloud shell te openen. Als u het script wilt plakken, klikt u met de rechter muisknop op de shell en selecteert u vervolgens **Plakken**:

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

### <a name="deploy-database-template-via-azure-cli"></a>Database sjabloon implementeren via Azure CLI

Als u de Resource Manager-sjabloon wilt implementeren met behulp van Azure CLI, selecteert u **proberen** de Azure Cloud shell te openen. Als u het script wilt plakken, klikt u met de rechter muisknop op de shell en selecteert u vervolgens **Plakken**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## De door Voer (RU/s) bijwerken voor een container<a id="container-ru-update"></a>

Met de volgende sjabloon wordt de door Voer van een container bijgewerkt. Kopieer de sjabloon en implementeer deze zoals hieronder wordt weer gegeven of ga naar de [Galerie van Azure Quick](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-ru-update/) start en implementeer deze vanuit de Azure Portal. U kunt de sjabloon ook downloaden naar uw lokale computer of een nieuwe sjabloon maken en het lokale pad met de `--template-file` para meter opgeven.

[!code-json[cosmosdb-sql-container-ru-update](~/quickstart-templates/101-cosmosdb-sql-container-ru-update/azuredeploy.json)]

### <a name="deploy-container-template-via-powershell"></a>Container sjabloon implementeren via Power shell

Als u de Resource Manager-sjabloon wilt implementeren met behulp van Power shell, **kopieert** u het script en selecteert u **proberen** de Azure Cloud shell te openen. Als u het script wilt plakken, klikt u met de rechter muisknop op de shell en selecteert u vervolgens **Plakken**:

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

### <a name="deploy-container-template-via-azure-cli"></a>Container sjabloon implementeren via Azure CLI

Als u de Resource Manager-sjabloon wilt implementeren met behulp van Azure CLI, selecteert u **proberen** de Azure Cloud shell te openen. Als u het script wilt plakken, klikt u met de rechter muisknop op de shell en selecteert u vervolgens **Plakken**:

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

Hier volgen enkele aanvullende bronnen:

- [Documentatie over Azure Resource Manager](/azure/azure-resource-manager/)
- [Resource provider-schema Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Quick Start-sjablonen Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Veelvoorkomende fouten bij Azure Resource Manager implementatie oplossen](../azure-resource-manager/resource-manager-common-deployment-errors.md)