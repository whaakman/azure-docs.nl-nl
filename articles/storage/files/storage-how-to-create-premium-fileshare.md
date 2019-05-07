---
title: Een premium Azure-bestandsshare maken
description: In dit artikel leert u hoe u een premium Azure-bestandsshare maken.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 05/05/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 265a1cf0a8a5e1e099a4ec7a9f0d674e0c474dd4
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190099"
---
# <a name="how-to-create-an-premium-azure-file-share"></a>Over het maken van een premium Azure-bestandsshare
Premium-bestandsshares (preview) worden aangeboden op opslagmedia SSD-schijf (SSD) en zijn nuttig voor i/o-intensieve werkbelastingen, met inbegrip van databases en high performance computing (HPC) te hosten. Premium-bestandsshares worden gehost in een speciaal type opslagaccount, een FileStorage-account genoemd. Premium-bestandsshares zijn ontworpen voor hoge prestaties en bedrijfstoepassingen van schaal, consistente lage latentie, hoge IOPS en hoge doorvoer shares te leveren.

Dit artikel leest u over het maken van dit nieuwe account met behulp van [Azure-portal](https://portal.azure.com/), Azure PowerShell en Azure CLI.

## <a name="prerequisites"></a>Vereisten

Voor toegang tot Azure-resources met inbegrip van premium Azure-bestandsshares, moet u een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

## <a name="create-a-premium-file-share-using-the-azure-portal"></a>Een premium-bestandsshare maken met de Azure-portal

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com/).

### <a name="create-a-filestorage-preview-storage-account"></a>Maak een opslagaccount filestorage (preview)

U bent nu klaar om uw opslagaccount te maken.

Elk opslagaccount moet behoren tot een Azure-resourcegroep. Een resourcegroep is een logische container voor het groeperen van uw Azure-services. Wanneer u een opslagaccount maakt, kunt u een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken. In dit artikel laat zien hoe een nieuwe resourcegroep maken.

1. Selecteer in de Azure portal, **Opslagaccounts** in het menu links.

    ![Azure portal hoofdpagina opslagaccount selecteren](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. Kies in het venster **Opslagaccounts** dat wordt weergegeven de optie **Toevoegen**.
1. Selecteer het abonnement waarin u het opslagaccount wilt maken.
1. Selecteer **Nieuwe maken** onder het veld **Resourcegroep**. Voer een naam in voor de nieuwe resourcegroep, zoals in de volgende afbeelding wordt weergegeven.

1. Voer vervolgens een naam in voor het opslagaccount. De naam die u kiest, moet uniek zijn binnen Azure. Verder moet de naam 3 tot 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten.
1. Selecteer een locatie voor uw opslagaccount of gebruik de standaardlocatie.
1. Voor **prestaties** Selecteer **Premium**.
1. Selecteer **soort Account** en kies **FileStorage (preview)**.
1. Laat **replicatie** ingesteld op de standaardwaarde van **lokaal redundante opslag (LRS)**.

    ![Over het maken van een storage-account voor een premium-bestandsshare](media/storage-how-to-create-premium-fileshare/premium-files-storage-account.png)

1. Selecteer **Beoordelen en maken** om uw opslagaccountinstellingen te bekijken en het account te maken.
1. Selecteer **Maken**.

Wanneer de resource van uw opslagaccount is gemaakt, navigeert u naar deze.

### <a name="create-a-premium-file-share"></a>Een Premium-bestandsshare maken

1. Schuif in het menu links voor het opslagaccount naar de **File-service** sectie en selecteer vervolgens **Files (preview)**.
1. Selecteer **+ bestandsshare** om een premium-bestandsshare te maken.
1. Voer een naam en een gewenst quotum voor de bestandsshare en selecteer vervolgens **maken**.

> [!NOTE]
> Share ingerichte grootte is opgegeven door het quotum voor de bestandsshare, bestandsshares worden in rekening gebracht voor de ingerichte grootte, raadpleegt u de [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/files/) voor meer informatie.

   ![Een Premium-bestandsshare maken](media/storage-how-to-create-premium-fileshare/create-premium-file-share.png)

### <a name="clean-up-resources"></a>Resources opschonen

Als u wilt voor het opschonen van de resources die in dit artikel is gemaakt, verwijdert u gewoon de resourcegroep. Als u de resourcegroep verwijdert u ook het bijbehorende opslagaccount, evenals alle andere resources die zijn gekoppeld aan de resourcegroep.

## <a name="create-a-premium-file-share-using-powershell"></a>Maken van een premium-bestandsshare met behulp van PowerShell

### <a name="create-an-account-using-powershell"></a>Een account maken met PowerShell

Installeer eerst de meest recente versie van de [ PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget)-module.

Klik, upgrade van uw powershell-module, zich aanmelden bij uw Azure-abonnement een resourcegroep maken en vervolgens een opslagaccount maken.

### <a name="upgrade-your-powershell-module"></a>Upgrade van uw PowerShell-module

Om te communiceren met een bestandsshare premium uit met PowerShell, moet u de meest recente Az.Storage-module installeren.

Begin door een PowerShell-sessie met verhoogde bevoegdheden te openen.

De Az.Storage-module installeren:

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowPrerelease -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>Aanmelden bij uw Azure-abonnement

Gebruik de opdracht `Connect-AzAccount` en volg de instructies op het scherm om te verifiëren.

```powershell
Connect-AzAccount
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Gebruik de opdracht [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) om een nieuwe resourcegroep met PowerShell te maken:

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-how-to-resource-group"
$location = "westus2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-filestorage-preview-storage-account"></a>Maak een opslagaccount FileStorage (preview)

U kunt een filestorage (preview) storage-account maken vanuit PowerShell met de [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) opdracht:

```powershell
$storageAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name "fileshowto" -SkuName "Premium_LRS" -Location "westus2" -Kind "FileStorage"
```

### <a name="create-a-premium-file-share"></a>Een Premium-bestandsshare maken

Nu dat u een FileStorage-account hebt, kunt u een premium-bestandsshare maken. Gebruik de [New-AzStorageShare](/powershell/module/az.storage/New-AzStorageShare) cmdlet een te maken.

> [!NOTE]
> Share ingerichte grootte is opgegeven door het quotum voor de bestandsshare, bestandsshares worden in rekening gebracht voor de ingerichte grootte, raadpleegt u de [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/files/) voor meer informatie.

```powershell
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

### <a name="clean-up-resources"></a>Resources opschonen

Gebruik de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle bijbehorende resources te verwijderen, inclusief het nieuwe opslagaccount: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-a-premium-file-share-using-azure-cli"></a>Maken van een premium-bestandsshare met behulp van Azure CLI

Voor het starten van Azure Cloud Shell, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).

Als u zich wilt aanmelden bij de lokale installatie van de CLI, voert u de opdracht voor aanmelden uit:

```cli
az login
```

### <a name="add-the-preview-storage-cli-extension"></a>De preview-opslag CLI-extensie toevoegen

Omdat premium-bestandsshares een preview-functie zijn, hebt u de preview-extensie toevoegen aan uw shell. Dit doet u als volgt: voer met behulp van Cloud Shell of een lokale shell de volgende opdracht in: `az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Gebruik de opdracht [az group create](/cli/azure/group) om een nieuwe resourcegroep te maken met Azure CLI.

```azurecli-interactive
az group create `
    --name files-howto-resource-group `
    --location westus2
```

### <a name="create-a-filestorage-storage-account"></a>Een FileStorage storage-account maken

Gebruik voor het maken van een FileStorage storage-account van de Azure CLI de [az storage-account maken](/cli/azure/storage/account) opdracht.

```azurecli-interactive
az storage account create `
    --name fileshowto `
    --resource-group files-howto-resource-group `
    --location westus `
    --sku Premium_LRS `
    --kind FileStorage
```

### <a name="get-the-storage-account-key"></a>Opslagaccountsleutel opvragen

Opslagaccountsleutels regelen de toegang tot resources in een storage-account in dit artikel, gebruiken we de sleutel om te kunnen maken van een premium-bestandsshare. Wanneer u een opslagaccount maakt, worden de sleutels automatisch gemaakt. U kunt de opslagaccountsleutels voor uw opslagaccount opvragen met de opdracht [az storage account keys list](/cli/azure/storage/account/keys):

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

### <a name="create-a-premium-file-share"></a>Een Premium-bestandsshare maken

Nu dat u een filestorage-account hebt, kunt u een premium-bestandsshare maken. Gebruik de [az storage share maken](/cli/azure/storage/share) opdracht een te maken.

> [!NOTE]
> Share ingerichte grootte is opgegeven door het quotum voor de bestandsshare, bestandsshares worden in rekening gebracht voor de ingerichte grootte, raadpleegt u de [pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/files/) voor meer informatie.

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

### <a name="clean-up-resources"></a>Resources opschonen

Gebruik de opdracht [az group delete](/cli/azure/group) om de resourcegroep en alle bijbehorende resources te verwijderen, inclusief het nieuwe opslagaccount.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een premium-bestandsshare gemaakt. Voor meer informatie over de prestaties van die dit account biedt, blijven de prestaties laag-sectie van de handleiding voor capaciteitsplanning.

> [!div class="nextstepaction"]
> [File share prestatielagen](storage-files-planning.md#file-share-performance-tiers)