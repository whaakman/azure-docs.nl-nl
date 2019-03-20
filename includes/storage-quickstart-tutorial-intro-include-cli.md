---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 44ee258567ca357687feb24337f2d5974e2532b0
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57786444"
---
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [az group create](/cli/azure/group). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-a-storage-account"></a>Create a storage account

Maak een algemeen opslagaccount met de opdracht [az storage account create](/cli/azure/storage/account). Het algemeen opslagaccount kan voor alle vier de services worden gebruikt: blobs, bestanden, tabellen en wachtrijen. 

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>Opslagaccountreferenties opgeven

Voor de meeste opdrachten in deze zelfstudie heeft Azure CLI uw opslagaccountreferenties nodig. Een van de eenvoudigste manieren om dit te regelen, is door de omgevingsvariabelen `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_KEY` in te stellen.

Geef eerst uw opslagaccountsleutels weer met behulp van de opdracht [az storage account keys list](/cli/azure/storage/account/keys):

```azurecli-interactive
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

Stel nu de omgevingsvariabelen `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_KEY` in. U kunt dit in de Bash-shell doen met de opdracht `export`:

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_KEY="myStorageAccountKey"
```
