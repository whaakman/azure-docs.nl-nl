---
title: Uploaden of kopiëren van een aangepaste Linux-VM met Azure CLI | Microsoft Docs
description: Uploaden of een aangepaste virtuele machine kopiëren met behulp van het Resource Manager-implementatiemodel en Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/17/2018
ms.author: cynthn
ms.openlocfilehash: 47227b1f9ceb4ba9e35180aa0cb171d1edd5bb9a
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696843"
---
# <a name="create-a-linux-vm-from-a-custom-disk-with-the-azure-cli"></a>Een Linux-VM maken van een aangepaste schijf met de Azure CLI

<!-- rename to create-vm-specialized -->

In dit artikel leest u over het uploaden van een aangepaste virtuele harde schijf (VHD), en om te kopiëren van een bestaande VHD in Azure. De zojuist gemaakte VHD wordt vervolgens gebruikt om te maken van nieuwe virtuele Linux-machines (VM's). U kunt installeren en configureren van een Linux-distributie voor uw vereisten en vervolgens die VHD gebruiken om te maken van een nieuwe Azure-machine.

Voor het maken van meerdere virtuele machines van de aangepaste schijf, moet u eerst een installatiekopie maken van uw virtuele machine of VHD. Zie voor meer informatie, [een aangepaste installatiekopie van een Azure-VM maken met behulp van de CLI](tutorial-custom-images.md).

U hebt twee opties om een aangepaste schijf te maken:
* [Een VHD uploaden](#option-1-upload-a-specialized-vhd)
* [Kopiëren van een bestaande VM in Azure](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Snelle opdrachten

Bij het maken van een nieuwe virtuele machine met [az vm maken](/cli/azure/vm#az-vm-create) vanaf een aangepaste of gespecialiseerde schijf, u **koppelen** de schijf (--koppelen-os-schijf) in plaats van een aangepaste of marketplace-installatiekopie op te geven (--afbeelding). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van de beheerde schijf met de naam *myManagedDisk* gemaakt op basis van uw aangepaste VHD:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Vereisten
Als u wilt de volgende stappen hebt voltooid, hebt u het volgende nodig:

* Een virtuele Linux-machine die is voorbereid voor gebruik in Azure. De [de virtuele machine voorbereiden](#prepare-the-vm) sectie van dit artikel wordt uitgelegd hoe u distributie-specifieke informatie over het installeren van de Azure Linux Agent (waagent), die nodig is voor u verbinding maken met een virtuele machine met SSH.
* Het VHD-bestand van een bestaand [door Azure onderschreven Linux-distributie](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (of Raadpleeg [informatie over niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) op een virtuele schijf in de VHD-indeling. Er bestaan meerdere hulpprogramma's voor het maken van een virtuele machine en de VHD:
  * Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), zorg ervoor dat u VHD gebruiken als uw installatiekopie-indeling. Indien nodig, kunt u [converteren van een installatiekopie van een](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) met `qemu-img convert`.
  * U kunt ook de Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [op Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een virtuele machine maakt, geeft u de VHD als de notatie. Indien nodig, kunt u de VHDX-schijven converteren naar VHD met [qemu img converteren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure biedt geen ondersteuning voor het uploaden van dynamische VHD's, dus u moet deze schijven converteren naar vaste VHD's voordat u uploadt. U kunt hulpprogramma's zoals [VHD hulpprogramma's voor Azure voor GO](https://github.com/Microsoft/azure-vhd-utils-for-go) naar dynamische schijven converteren tijdens het proces van het uploaden naar Azure.
> 
> 


* Zorg ervoor dat u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en u bent aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az-login).

In de volgende voorbeelden voorbeeld parameternamen vervangen door uw eigen waarden, zoals *myResourceGroup*, *mystorageaccount*, en *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>De virtuele machine voorbereiden

Azure biedt ondersteuning voor verschillende Linux-distributies (Zie [onderschreven distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). De volgende artikelen wordt beschreven hoe u de verschillende Linux-distributies die worden ondersteund op Azure voorbereiden:

* [Op basis van centOS-distributies](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Overige: Niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Zie ook de [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer algemene tips voor het maken van Linux-installatiekopieën voor Azure.

> [!NOTE]
> De [Azure-platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) is van toepassing op virtuele machines waarop Linux wordt uitgevoerd alleen als een van de onderschreven distributies wordt gebruikt met de informatie over de configuratie zoals opgegeven bij de 'Ondersteunde versies' [Linux op door Azure onderschreven Distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>Optie 1: Een VHD uploaden

U kunt een aangepaste VHD die u hebt uitgevoerd op een lokale computer of die u hebt geëxporteerd uit een andere cloud uploaden. Voor het gebruik van een VHD te maken van een nieuwe Azure-VM, moet u de VHD uploaden naar een opslagaccount en een beheerde schijf maken op basis van de VHD. Zie [Azure Managed Disks overview](../windows/managed-disks-overview.md) (Overzicht van Azure Managed Disks) voor meer informatie.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat het uploaden van uw aangepaste schijf en VM's maken, moet u een resourcegroep maken met [az-groep maken](/cli/azure/group#az-group-create).

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Create a storage account

Maak een opslagaccount voor uw aangepaste schijf en VM's met [az storage-account maken](/cli/azure/storage/account). Het volgende voorbeeld wordt een opslagaccount met de naam *mystorageaccount* in de resourcegroep die eerder hebt gemaakt:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Een lijst met storage accountsleutels
Azure twee 512-bits toegangssleutels voor elk opslagaccount genereert. Deze toegangssleutels worden gebruikt voor verificatie bij de storage-account, zoals bij het nakomen van schrijfbewerkingen. Zie voor meer informatie, [beheren van toegang tot opslag](../../storage/common/storage-account-manage.md#access-keys). 

Weergeven van de toegangssleutel met [az storage account sleutels lijst](/cli/azure/storage/account/keys#az-storage-account-keys-list). Bijvoorbeeld, om weer te geven account de toegangssleutel voor de opslag dat u hebt gemaakt:

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

De uitvoer is vergelijkbaar met:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
Maak een notitie van **key1** als u dit gebruikt om te communiceren met uw opslagaccount in de volgende stappen.

### <a name="create-a-storage-container"></a>Maak een opslagcontainer
Op dezelfde manier die u verschillende mappen maakt voor het lokale bestandssysteem logische manier te organiseren, maakt u containers in een opslagaccount voor het ordenen van uw schijven. Een opslagaccount kan veel containers bevatten. Maak een container met [az storage container maken](/cli/azure/storage/container#az-storage-container-create).

Het volgende voorbeeld wordt een container met de naam *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a>De VHD uploaden
Uploaden van uw aangepaste schijf met [az storage blob upload](/cli/azure/storage/blob#az-storage-blob-upload). U zult uploaden en uw aangepaste schijf opslaan als een pagina-blob.

Geef uw toegangssleutel, de container die u hebt gemaakt in de vorige stap en het pad naar de aangepaste schijf op de lokale computer:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
Uploaden van de VHD kan even duren.

### <a name="create-a-managed-disk"></a>Een beheerde schijf maken


Een beheerde schijf maken op basis van de VHD met [az schijf maken](/cli/azure/disk#az-disk-create). Het volgende voorbeeld wordt een beheerde schijf met de naam *myManagedDisk* van de VHD die u hebt geüpload naar uw opslagaccount met de naam en de container:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Optie 2: Een bestaande virtuele machine kopiëren

U kunt ook een aangepaste virtuele machine maken in Azure en kopieer vervolgens de besturingssysteemschijf en koppelen aan een nieuwe virtuele machine om een andere kopie te maken. Dit is prima voor het testen, maar als u wilt gebruiken van een bestaande VM in Azure als het model voor meerdere nieuwe virtuele machines, maakt u een *installatiekopie* in plaats daarvan. Zie voor meer informatie over het maken van een installatiekopie van een bestaande Azure-VM [een aangepaste installatiekopie van een Azure-VM maken met behulp van de CLI](tutorial-custom-images.md).

### <a name="create-a-snapshot"></a>Een momentopname maken

Dit voorbeeld maakt u een momentopname van een virtuele machine met de naam *myVM* in resourcegroep *myResourceGroup* en maakt u een momentopname met de naam *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a>De beheerde schijf maken

Een nieuwe beheerde schijf maken op basis van de momentopname.

Haal de ID van de momentopname. In dit voorbeeld wordt de momentopname met de naam *osDiskSnapshot* en deel uitmaakt van de *myResourceGroup* resourcegroep.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

De beheerde schijf maken. In dit voorbeeld maken we een beheerde schijf met de naam *myManagedDisk* van onze momentopname, waar de schijf is in de standard-opslag en de grootte van 128 GB.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>De virtuele machine maken

Maak uw virtuele machine met [az vm maken](/cli/azure/vm#az-vm-create) en koppelen (--koppelen-os-schijf) de beheerde schijf als de besturingssysteemschijf. Het volgende voorbeeld wordt een virtuele machine met de naam *myNewVM* met behulp van de beheerde schijf die u hebt gemaakt van uw geüploade VHD:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

U moet kunnen SSH in de virtuele machine met de referenties van de bron-VM. 

## <a name="next-steps"></a>Volgende stappen
Nadat u hebt voorbereid en uw aangepaste virtuele schijf geüpload, kunt u meer lezen over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md). U kunt ook naar [een gegevensschijf toevoegen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) aan uw nieuwe VM's. Als u toepassingen die worden uitgevoerd op uw virtuele machines die u nodig hebt voor toegang tot hebt, moet u [poorten en eindpunten openen](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
