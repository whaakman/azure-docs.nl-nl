---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: c5cb1c5a005265950f8dcd02c8e7675918751dab
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164253"
---
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [az group create](/cli/azure/group#az_group_create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-a-storage-account"></a>Create a storage account

Maak een algemeen opslagaccount met de opdracht [az storage account create](/cli/azure/storage/account#create). Het algemeen opslagaccount kan voor alle vier de services worden gebruikt: blobs, bestanden, tabellen en wachtrijen. 

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>Opslagaccountreferenties opgeven

Voor de meeste opdrachten in deze zelfstudie heeft Azure CLI uw opslagaccountreferenties nodig. Een van de eenvoudigste manieren om dit te regelen, is door de omgevingsvariabelen `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_ACCESS_KEY` in te stellen.

Geef eerst uw opslagaccountsleutels weer met behulp van de opdracht [az storage account keys list](/cli/azure/storage/account/keys#list):

```azurecli-interactive
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

Stel nu de omgevingsvariabelen `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_ACCESS_KEY` in. U kunt dit in de Bash-shell doen met de opdracht `export`:

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_ACCESS_KEY="myStorageAccountKey"
```