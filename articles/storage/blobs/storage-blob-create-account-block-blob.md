---
title: Maak een block blob storage-account - Azure Storage | Microsoft Docs
description: Laat zien hoe een Azure block blob storage-account maken met premium-prestatiekenmerken.
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
author: twooley
ms.author: twooley
ms.subservice: blobs
ms.openlocfilehash: f4d3e3ad923b6a603902bc007107cb41dae8cf6c
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400469"
---
# <a name="create-a-block-blob-storage-account"></a>Een block blob storage-account maken

Het accounttype van block blob storage kunt u het maken van blok-blobs met premium-prestatiekenmerken. Dit type opslagaccount dat is geoptimaliseerd voor workloads met hoge transacties tarieven of waarvoor tijden met zeer snelle toegang. In dit artikel laat zien hoe een block blob storage-account maken met behulp van de Azure-portal, Azure CLI of Azure PowerShell.

Zie voor meer informatie over block blob storage-accounts, [overzicht van Azure storage-account](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

## <a name="create-account-in-the-azure-portal"></a>Account maken in Azure portal

Als u wilt een block blob storage-account maken in Azure portal, de volgende stappen uit:

1. Selecteer in de Azure portal, **alle services** > de **opslag** categorie > **opslagaccounts**.

1. Onder **opslagaccounts**, selecteer **toevoegen**.

1. In de **abonnement** veld, selecteert u het abonnement waarin u wilt maken van het storage-account.

1. In de **resourcegroep** veld, selecteert u een bestaande resourcegroep of **nieuw**, en voer een naam voor de nieuwe resourcegroep.

1. In de **opslagaccountnaam** en voer een naam voor het account. Houd rekening met de volgende richtlijnen:

   - De naam moet uniek zijn in Azure.
   - De naam moet tussen 3 en 24 tekens lang zijn.
   - De naam mag alleen cijfers en kleine letters bestaan.

1. In de **locatie** veld, selecteer een locatie voor het opslagaccount of gebruik de standaardlocatie.

1. Voor de rest van de instellingen, het volgende configureren:

   |Veld     |Value  |
   |---------|---------|
   |**Prestaties**    |  Selecteer **Premium**.   |
   |**Type account**    | Selecteer **BlockBlobStorage**.      |
   |**Replicatie**    |  Laat de standaardinstelling van **lokaal redundante opslag (LRS)**.      |

   ![Portal UI te maken van een block blob storage-account bevat](media/storage-blob-create-account-block-blob/create-block-blob-storage-account.png)

1. Selecteer **bekijken + maken** om te controleren van de instellingen voor het opslagaccount.

1. Selecteer **Maken**.

## <a name="create-account-using-azure-powershell"></a>Met behulp van PowerShell voor Azure-account maken

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. Open een verhoogde Windows PowerShell-sessie (als administrator uitvoeren).

1. Voer de volgende opdracht uit om te controleren of de nieuwste versie van de `Az` PowerShell-module is geïnstalleerd.

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

1. Open een nieuwe PowerShell-console en meld u aan met uw Azure-account.

   ```powershell
   Connect-AzAccount -SubscriptionId <SubscriptionID>
   ```

1. Indien nodig, maakt u een nieuwe resourcegroep. Vervang de waarden in offertes, en voer de volgende opdracht uit.

   ```powershell
   $resourcegroup = "new_resource_group_name"
   $location = "region_name"
   New-AzResourceGroup -Name $resourceGroup -Location $location
   ```

1. Maak de block blob storage-account. Vervang de waarden in offertes, en voer de volgende opdracht uit.

   ```powershell
   $resourcegroup = "resource_group_name"
   $storageaccount = "new_storage_account_name"
   $location = "region_name"

   New-AzStorageAccount -ResourceGroupName $resourcegroup -Name $storageaccount -Location $location -Kind "BlockBlobStorage" -SkuName "Premium_LRS"
   ```

## <a name="create-account-using-azure-cli"></a>Account maken met Azure CLI

Voor het maken van een blok-blob-account met behulp van de Azure CLI, moet u eerst de Azure CLI-versie installeren. 2.0.46 of een latere versie. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

1. Aanmelden bij uw Azure-abonnement.

   ```azurecli
   az login
   ```

1. Indien nodig, maakt u een nieuwe resourcegroep. Vervang de waarden tussen vierkante haken (inclusief de punthaken) en voer de volgende opdracht uit.

   ```azurecli
   az group create \
    --name "<new_resource_group_name>" \
    --location "<location>"
   ```

1. Maak de block blob storage-account. Vervang de waarden tussen vierkante haken (inclusief de punthaken) en voer de volgende opdracht uit.

   ```azurecli
   az storage account create \
    --location "<location>" \
    --name "<new_storage_account_name>" \
    --resource-group "<resource_group_name>" \
    --kind "BlockBlobStorage" \
    --sku "Premium_LRS"
   ```

## <a name="next-steps"></a>Volgende stappen

- Zie [Overzicht van Azure-opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-account-overview) voor meer informatie over opslagaccounts.

- Zie [Overzicht van Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) voor meer informatie over resourcegroepen.
