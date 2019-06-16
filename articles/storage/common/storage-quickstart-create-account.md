---
title: Een opslagaccount maken - Azure Storage
description: In dit artikel procedures leert u een opslagaccount maken met de Azure portal, Azure PowerShell of Azure CLI. Een Azure-opslagaccount biedt een unieke naamruimte in Microsoft Azure voor het opslaan en werken met de gegevensobjecten die u in Azure Storage maakt.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: article
ms.date: 05/06/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8375f4c54dc436ecf0694ec5f629c81d3591594d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65234179"
---
# <a name="create-a-storage-account"></a>Create a storage account

Een Azure storage-account bevat al uw Azure Storage-gegevensobjecten: blobs, bestanden, wachtrijen, tabellen en schijven. Het opslagaccount biedt een unieke naamruimte voor uw Azure Storage-gegevens die toegankelijk is vanaf overal ter wereld via HTTP of HTTPS. Gegevens in uw Azure storage-account zijn duurzame en maximaal beschikbaar, veilig en zeer schaalbare.

In dit artikel met instructies, leert u het maken van een storage account met de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), of een [Azure Resource Manager sjabloon](../../azure-resource-manager/resource-group-overview.md).  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Geen.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

In dit artikel is de Azure PowerShell-module Az 0,7 of hoger vereist. Voer `Get-Module -ListAvailable Az` uit om uw huidige versie te vinden. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps).

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

U kunt zich aanmelden bij Azure en Azure CLI-opdrachten uitvoeren op twee manieren:

- U kunt de CLI-opdrachten uit op de Azure portal kunt uitvoeren in Azure Cloud Shell.
- U kunt de CLI installeren en CLI-opdrachten lokaal uitvoeren.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell gebruiken

Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. De Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Klik op de **Cloud Shell** knop in het menu in de rechterbovenhoek van de Azure-portal:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

De knop Start een interactieve shell die u gebruiken kunt voor het uitvoeren van de stappen die worden beschreven in dit artikel met instructies:

[![Schermopname van het Cloud Shell-venster in de portal](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>De CLI lokaal installeren

U kunt Azure CLI ook lokaal installeren en gebruiken. In dit artikel met instructies moet u de Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

# <a name="templatetabtemplate"></a>[Sjabloon](#tab/template)

Geen.

---

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Meld u aan bij [Azure Portal](https://portal.azure.com).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Aanmelden bij uw Azure-abonnement met de `Connect-AzAccount` opdracht en volg de aanwijzingen aanwijzingen om te verifiëren.

```powershell
Connect-AzAccount
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Als u wilt Open Azure Cloud Shell, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).

Uitvoeren als u wilt aanmelden bij de lokale installatie van de CLI, de [az login](/cli/azure/reference-index#az-login) opdracht:

```cli
az login
```

# <a name="templatetabtemplate"></a>[Sjabloon](#tab/template)

N/A

---

## <a name="create-a-storage-account"></a>Create a storage account

U kunt nu uw opslagaccount maken.

Elk opslagaccount moet behoren tot een Azure-resourcegroep. Een resourcegroep is een logische container voor het groeperen van uw Azure-services. Wanneer u een opslagaccount maakt, kunt u een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken. In dit artikel laat zien hoe een nieuwe resourcegroep maken.

Een v2-opslagaccount **voor algemeen gebruik** biedt toegang tot alle services van Azure Storage: blobs, bestanden, wachtrijen, tabellen en schijven. De stappen die hier worden beschreven, een opslagaccount voor algemeen gebruik v2 maken, maar de stappen voor het maken van elk type opslagaccount zijn vergelijkbaar.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Maak eerst een nieuwe resourcegroep met PowerShell met behulp van de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup):

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hard-coding it repeatedly
$resourceGroup = "storage-resource-group"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

Als u niet zeker weet welke regio u moet opgeven voor de parameter `-Location`, kunt u een lijst met ondersteunde regio's voor uw abonnement ophalen met de opdracht [Get-AzLocation](/powershell/module/az.resources/get-azlocation):

```powershell
Get-AzLocation | select Location
$location = "westus"
```

Maak vervolgens een opslagaccount voor algemeen gebruik v2 met geo-redundante opslag met leestoegang (RA-GRS) met behulp van de [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) opdracht. Houd er rekening mee dat de naam van uw storage-account moet uniek zijn in Azure, dus de aanduidingswaarde tussen vierkante haken vervangen door uw eigen unieke waarde:

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2 
```

Vervang in de onderstaande tabel voor de gewenste waarde voor het maken van een opslagaccount voor algemeen gebruik v2 met een andere replicatieoptie, de **SkuName** parameter.

|Replicatie-optie  |De parameter SkuName  |
|---------|---------|
|Lokaal redundante opslag (LRS)     |Standard_LRS         |
|Zone-redundante opslag (ZRS)     |Standard_ZRS         |
|Geografisch redundante opslag (GRS)     |Standard_GRS         |
|Geografisch redundante opslag met leestoegang (GRS)     |Standard_RAGRS         |

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Gebruik eerst de opdracht [az group create](/cli/azure/group#az_group_create) om een nieuwe resourcegroep te maken met Azure CLI.

```azurecli-interactive
az group create \
    --name storage-resource-group \
    --location westus
```

Als u niet zeker weet welke regio u moet opgeven voor de parameter `--location`, kunt u een lijst met ondersteunde regio's voor uw abonnement ophalen met de opdracht [az account list-locations](/cli/azure/account#az_account_list).

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Maak vervolgens een opslagaccount voor algemeen gebruik v2 met geografisch redundante opslag met leestoegang met behulp van de [az storage-account maken](/cli/azure/storage/account#az_storage_account_create) opdracht. Houd er rekening mee dat de naam van uw storage-account moet uniek zijn in Azure, dus de aanduidingswaarde tussen vierkante haken vervangen door uw eigen unieke waarde:

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group storage-resource-group \
    --location westus \
    --sku Standard_RAGRS \
    --kind StorageV2
```

Vervang in de onderstaande tabel voor de gewenste waarde voor het maken van een opslagaccount voor algemeen gebruik v2 met een andere replicatieoptie, de **sku** parameter.

|Replicatie-optie  |sku-parameter  |
|---------|---------|
|Lokaal redundante opslag (LRS)     |Standard_LRS         |
|Zone-redundante opslag (ZRS)     |Standard_ZRS         |
|Geografisch redundante opslag (GRS)     |Standard_GRS         |
|Geografisch redundante opslag met leestoegang (GRS)     |Standard_RAGRS         |

# <a name="templatetabtemplate"></a>[Sjabloon](#tab/template)

U kunt Azure Powershell of Azure CLI gebruiken voor het implementeren van een Resource Manager-sjabloon voor het maken van een storage-account. De sjabloon die wordt gebruikt in dit artikel is van [Azure Resource Manager quickstart-sjablonen](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Als u wilt de scripts uitvoeren, selecteert u **uitproberen** openen van de Azure Cloud shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-file "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

Voor informatie over het maken van sjablonen, Zie:

- [Documentatie voor Azure Resource Manager](/azure/azure-resource-manager/).
- [Naslaginformatie voor Storage-account sjabloon](/azure/templates/microsoft.storage/allversions).
- [Voorbeeldsjablonen van extra opslag account](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Storage).

---

Zie [Storage-replicatieopties](storage-redundancy.md) voor meer informatie over beschikbare replicatieopties.

## <a name="clean-up-resources"></a>Resources opschonen

Als u opschonen van de resources die in dit artikel met instructies gemaakt wilt, kunt u de resourcegroep verwijderen. Als u de resourcegroep verwijdert, worden ook het bijbehorende opslagaccount en eventuele andere resources die zijn gekoppeld aan de resourcegroep, verwijderd.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Ga als volgt te werk om een resourcegroep te verwijderen in Azure Portal:

1. Vouw het menu aan de linkerkant in Azure Portal uit om het menu met services te openen en kies **Resourcegroepen** om de lijst met resourcegroepen weer te geven.
2. Zoek de resourcegroep die u wilt verwijderen, en klik met de rechtermuisknop op de knop **Meer** ( **...** ) aan de rechterkant van de lijst.
3. Selecteer **Resourcegroep verwijderen** en bevestig dit.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Gebruik de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle bijbehorende resources te verwijderen, inclusief het nieuwe opslagaccount:

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Gebruik de opdracht [az group delete](/cli/azure/group#az_group_delete) om de resourcegroep en alle bijbehorende resources te verwijderen, inclusief het nieuwe opslagaccount.

```azurecli-interactive
az group delete --name storage-resource-group
```

# <a name="templatetabtemplate"></a>[Sjabloon](#tab/template)

Als u wilt verwijderen van de resourcegroep en alle bijbehorende resources, met inbegrip van het nieuwe opslagaccount, Azure PowerShell of Azure CLI te gebruiken.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

---

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies, hebt u een standaardopslagaccount voor algemeen gebruik v2 gemaakt. Voor meer informatie over het uploaden en downloaden van blobs naar en van uw opslagaccount, blijven een van de Blob storage-Quick-starts.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Werken met blobs met behulp van de Azure-portal](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [Werken met blobs met behulp van PowerShell](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Werken met blobs met behulp van Azure CLI](../blobs/storage-quickstart-blobs-cli.md)

# <a name="templatetabtemplate"></a>[Sjabloon](#tab/template)

> [!div class="nextstepaction"]
> [Werken met blobs met behulp van de Azure-portal](../blobs/storage-quickstart-blobs-portal.md)

---
