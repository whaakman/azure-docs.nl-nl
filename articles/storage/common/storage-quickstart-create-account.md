---
title: 'Quickstart: Een opslagaccount maken - Azure Storage'
description: In deze snelstart leert u hoe u een opslagaccount maakt met de Azure-portal, Azure PowerShell of de Azure CLI. Een Azure-opslagaccount biedt een unieke naamruimte in Microsoft Azure voor het opslaan en werken met de gegevensobjecten die u in Azure Storage maakt.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 09/18/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5266ca3f50a2d8163dbab95109cb967fb5a63ed8
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474578"
---
# <a name="create-a-storage-account"></a>Create a storage account

In deze snelstart leert u hoe u een opslagaccount maakt met de [Azure-portal](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) of [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest).  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Geen.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Voor deze snelstart is Az-versie 0.7 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable Az` uit om uw huidige versie te vinden. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps).

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

U kunt u aanmelden bij Azure en Azure CLI-opdrachten uitvoeren. Dit kan op twee manieren:

- U kunt CLI-opdrachten uitvoeren vanuit Azure Portal, in Azure Cloud Shell 
- U kunt de CLI installeren en CLI-opdrachten lokaal uitvoeren  

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell gebruiken

Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Klik op de knop **Cloud Shell** in het menu rechtsboven in Azure Portal:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Met de knop start u een interactieve shell waarmee u alle stappen in deze snelstartgids kunt uitvoeren:

[![Schermopname van het Cloud Shell-venster in de portal](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>De CLI lokaal installeren

U kunt Azure CLI ook lokaal installeren en gebruiken. Voor deze snelstartgids moet u de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

---

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Meld u aan bij [Azure Portal](https://portal.azure.com).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzAccount` en volg de instructies op het scherm.

```powershell
Connect-AzAccount
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Meld u aan bij [Azure Portal](https://portal.azure.com) om Azure Cloud Shell te starten.

Als u zich wilt aanmelden bij de lokale installatie van de CLI, voert u de opdracht voor aanmelden uit:

```cli
az login
```

---

## <a name="create-a-storage-account"></a>Create a storage account

U kunt nu uw opslagaccount maken.

Elk opslagaccount moet behoren tot een Azure-resourcegroep. Een resourcegroep is een logische container voor het groeperen van uw Azure-services. Wanneer u een opslagaccount maakt, kunt u een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken. Deze snelstart laat zien hoe u een nieuwe resource groep maakt. 

Een v2-opslagaccount **voor algemeen gebruik** biedt toegang tot alle services van Azure Storage: blobs, bestanden, wachtrijen, tabellen en schijven. Met deze snelstart maakt u een v2-opslagaccount voor algemeen gebruik, maar de stappen voor het maken van elk type opslagaccount zijn vergelijkbaar.   

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Maak eerst een nieuwe resourcegroep met PowerShell met behulp van de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup): 

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
New-AzResourceGroup -Name $resourceGroup -Location $location 
```

Als u niet zeker weet welke regio u moet opgeven voor de parameter `-Location`, kunt u een lijst met ondersteunde regio's voor uw abonnement ophalen met de opdracht [Get-AzLocation](/powershell/module/az.resources/get-azlocation):

```powershell
Get-AzLocation | select Location 
$location = "westus"
```

Maak vervolgens een v2-opslagaccount voor algemeen gebruik met lokaal redundante opslag (LRS). Gebruik de opdracht [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount): 

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 
```

Om een v2-opslagaccount voor algemeen gebruik te maken met zone-redundante opslag (ZRS) (preview), geografisch redundante opslag (GRS) of geografisch redundante opslag met leestoegang (RA-GRS), vervangt u de **SkuName**-parameter in de onderstaande tabel met de gewenste waarde. 

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
    --name storage-quickstart-resource-group \
    --location westus
```

Als u niet zeker weet welke regio u moet opgeven voor de parameter `--location`, kunt u een lijst met ondersteunde regio's voor uw abonnement ophalen met de opdracht [az account list-locations](/cli/azure/account#az_account_list).

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Maak vervolgens een v2-opslagaccount voor algemeen gebruik met lokaal redundante opslag. Gebruik de opdracht [az storage account create](/cli/azure/storage/account#az_storage_account_create):

```azurecli-interactive
az storage account create \
    --name storagequickstart \
    --resource-group storage-quickstart-resource-group \
    --location westus \
    --sku Standard_LRS \
    --kind StorageV2
```

Om een v2-opslagaccount voor algemeen gebruik te maken met zone-redundante opslag (ZRS Preview), geografisch redundante opslag (GRS) of geografisch redundante opslag met leestoegang (RA-GRS), vervangt u de parameter **sku** in de onderstaande tabel met de gewenste waarde. 

|Replicatie-optie  |sku-parameter  |
|---------|---------|
|Lokaal redundante opslag (LRS)     |Standard_LRS         |
|Zone-redundante opslag (ZRS)     |Standard_ZRS         |
|Geografisch redundante opslag (GRS)     |Standard_GRS         |
|Geografisch redundante opslag met leestoegang (GRS)     |Standard_RAGRS         |

---

Zie [Storage-replicatieopties](storage-redundancy.md) voor meer informatie over beschikbare replicatieopties.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources die in deze snelstartgids zijn gemaakt, wilt opschonen, verwijdert u gewoon de resourcegroep. Als u de resourcegroep verwijdert, worden ook het bijbehorende opslagaccount en eventuele andere resources die zijn gekoppeld aan de resourcegroep, verwijderd.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Ga als volgt te werk om een resourcegroep te verwijderen in Azure Portal:

1. Vouw het menu aan de linkerkant in Azure Portal uit om het menu met services te openen en kies **Resourcegroepen** om de lijst met resourcegroepen weer te geven.
2. Zoek de resourcegroep die u wilt verwijderen, en klik met de rechtermuisknop op de knop **Meer** (**...** ) aan de rechterkant van de lijst.
3. Selecteer **Resourcegroep verwijderen** en bevestig dit.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Gebruik de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle bijbehorende resources te verwijderen, inclusief het nieuwe opslagaccount: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

Gebruik de opdracht [az group delete](/cli/azure/group#az_group_delete) om de resourcegroep en alle bijbehorende resources te verwijderen, inclusief het nieuwe opslagaccount.

```azurecli-interactive
az group delete --name storage-quickstart-resource-group
```

---

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een standaardopslagaccount voor algemeen gebruik (v2) gemaakt. Voor informatie over het uploaden en downloaden van blobs naar en uit uw opslagaccount gaat u verder met de snelstartgids over blob-opslag.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Werken met blobs met behulp van de Azure-portal](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [Werken met blobs met behulp van PowerShell](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure-CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Werken met blobs met behulp van Azure CLI](../blobs/storage-quickstart-blobs-cli.md)

---
