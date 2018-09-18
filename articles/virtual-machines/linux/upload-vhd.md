---
title: Uploaden of kopiëren van een aangepaste Linux-VM met Azure CLI 2.0 | Microsoft Docs
description: Uploaden of kopiëren van een aangepaste virtuele machine met behulp van de Resource Manager-implementatiemodel en Azure CLI 2.0
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
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 3fb6957cf6af5c09a355b61c7c2440a929d1b837
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736664"
---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Een Linux-VM maken van een aangepaste schijf met de Azure CLI 2.0

<!-- rename to create-vm-specialized -->

Dit artikel leest u over het uploaden van een aangepaste virtuele harde schijf (VHD) of een kopie een een bestaande VHD in Azure en het maken van nieuwe virtuele Linux-machines (VM's) van de aangepaste schijf. U kunt installeren en configureren van een Linux-distributie voor uw vereisten en vervolgens die VHD gebruiken om snel een nieuwe Azure-machine.

Als u maken van meerdere virtuele machines van uw aangepaste schijf wilt, moet u een installatiekopie maken van uw virtuele machine of VHD. Zie voor meer informatie, [maken van een aangepaste installatiekopie van een Azure-VM met behulp van de CLI](tutorial-custom-images.md).

U hebt hiervoor twee opties:
* [Een VHD uploaden](#option-1-upload-a-specialized-vhd)
* [Kopiëren van een bestaande VM in Azure](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Snelle opdrachten

Bij het maken van een nieuwe virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create) van een aangepaste of gespecialiseerde schijf u **koppelen** de schijf (--koppelen-os-schijf) in plaats van een aangepaste of marketplace-installatiekopie op te geven (--afbeelding). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van de beheerde schijf met de naam *myManagedDisk* gemaakt op basis van uw aangepaste VHD:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Vereisten
Als u wilt de volgende stappen hebt voltooid, hebt u het volgende nodig:

* Een virtuele Linux-machine die is voorbereid voor gebruik in Azure. De [de virtuele machine voorbereiden](#prepare-the-vm) sectie van dit artikel wordt uitgelegd hoe u voor specifieke informatie over het installeren van de Azure Linux Agent (waagent) die vereist voor de virtuele machine is goed in Azure en u geen verbinding maken met het gebruik van SSH distributie.
* Het VHD-bestand van een bestaand [door Azure onderschreven Linux-distributie](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (of Raadpleeg [informatie over niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) op een virtuele schijf in de VHD-indeling. Er bestaan meerdere hulpprogramma's voor het maken van een virtuele machine en de VHD:
  * Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), zorg ervoor dat u VHD gebruiken als uw installatiekopie-indeling. Indien nodig, kunt u [converteren van een installatiekopie van een](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) met behulp van **qemu img converteren**.
  * U kunt ook de Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [op Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een virtuele machine maakt, geeft u de VHD als de notatie. Indien nodig, kunt u de VHDX-schijven converteren naar VHD met behulp van [qemu img converteren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure biedt bovendien geen ondersteuning voor dynamische VHD's, uploaden, dus u deze schijven converteren naar vaste VHD's moet voordat u uploadt. U kunt hulpprogramma's zoals [VHD hulpprogramma's voor Azure voor GO](https://github.com/Microsoft/azure-vhd-utils-for-go) naar dynamische schijven converteren tijdens het proces van het uploaden naar Azure.
> 
> 


* Zorg ervoor dat u de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen *myResourceGroup*, *mystorageaccount*, en *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>De virtuele machine voorbereiden

Azure biedt ondersteuning voor verschillende Linux-distributies (Zie [onderschreven distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). De volgende artikelen helpen u bij het voorbereiden van de verschillende Linux-distributies die worden ondersteund op Azure:

* [Op basis van centOS-distributies](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Andere - niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Zie ook de [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer algemene tips voor het maken van Linux-installatiekopieën voor Azure.

> [!NOTE]
> De [Azure-platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) is van toepassing op virtuele machines waarop Linux wordt uitgevoerd alleen als een van de onderschreven distributies wordt gebruikt met de informatie over de configuratie zoals opgegeven bij de ondersteunde versies' in [Linux op door Azure onderschreven Distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>Optie 1: Een VHD uploaden

U kunt een aangepaste VHD die u hebt uitgevoerd op een lokale computer of die u hebt geëxporteerd uit een andere cloud uploaden. Als u de VHD wilt maken van een nieuwe Azure-VM, moet u de VHD uploaden naar een opslagaccount en een beheerde schijf maken op basis van de VHD. 

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u uw aangepaste schijf uploaden en het maken van virtuele machines, moet u eerst een resourcegroep maken met [az-groep maken](/cli/azure/group#az_group_create).

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie: [overzicht Azure Managed Disks](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Create a storage account

Maak een opslagaccount voor uw aangepaste schijf en VM's met [az storage-account maken](/cli/azure/storage/account#az_storage_account_create). 

Het volgende voorbeeld wordt een opslagaccount met de naam *mystorageaccount* in de resourcegroep die eerder hebt gemaakt:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Een lijst met storage accountsleutels
Azure twee 512-bits toegangssleutels voor elk opslagaccount genereert. Deze toegangssleutels worden gebruikt voor verificatie bij de storage-account, zoals de uitvoering van bewerkingen voor schrijven. Meer informatie over [beheren van toegang tot opslag hier](../../storage/common/storage-account-manage.md#access-keys). Weergeven van de toegangssleutel met [az storage account sleutels lijst](/cli/azure/storage/account/keys#az_storage_account_keys_list).

De toegangssleutel voor het opslagaccount dat u hebt gemaakt bekijken:

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
Op dezelfde manier die u verschillende mappen maakt voor het lokale bestandssysteem logische manier te organiseren, door containers in een opslagaccount voor het ordenen van uw schijven te maken. Een opslagaccount kan een onbeperkt aantal containers bevatten. Maak een container met [az storage container maken](/cli/azure/storage/container#az_storage_container_create).

Het volgende voorbeeld wordt een container met de naam *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a>De VHD uploaden
Nu de aangepaste schijf met uploaden [az storage blob upload](/cli/azure/storage/blob#az_storage_blob_upload). U uploaden en uw aangepaste schijf opslaan als een pagina-blob.

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


Maak een beheerde schijf van de VHD met behulp [az schijf maken](/cli/azure/disk#az_disk_create). Het volgende voorbeeld wordt een beheerde schijf met de naam *myManagedDisk* van de VHD die u hebt geüpload naar uw opslagaccount met de naam en de container:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Optie 2: Een bestaande virtuele machine kopiëren

U kunt ook de aangepaste VM maken in Azure en kopieer vervolgens de besturingssysteemschijf en koppelen aan een nieuwe virtuele machine om een andere kopie te maken. Dit is prima voor het testen, maar als u gebruiken van een bestaande VM in Azure als het model voor meerdere nieuwe virtuele machines wilt, echt moet u een **installatiekopie** in plaats daarvan. Zie voor meer informatie over het maken van een installatiekopie van een bestaande Azure-VM [maken van een aangepaste installatiekopie van een Azure-VM met behulp van de CLI](tutorial-custom-images.md)

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

De beheerde schijf maken. In dit voorbeeld maken we een beheerde schijf met de naam *myManagedDisk* uit onze momentopname is die 128 GB groot in standard-opslag.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>De virtuele machine maken

Maak nu uw virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create) en koppelen (--koppelen-os-schijf) de beheerde schijf als de besturingssysteemschijf. Het volgende voorbeeld wordt een virtuele machine met de naam *myNewVM* met behulp van de beheerde schijf gemaakt op basis van uw geüploade VHD:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

U moet mogelijk SSH in de virtuele machine met de referenties van de bron-VM. 

## <a name="next-steps"></a>Volgende stappen
Nadat u hebt voorbereid en uw aangepaste virtuele schijf geüpload, kunt u meer lezen over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md). U kunt ook naar [een gegevensschijf toevoegen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) aan uw nieuwe VM's. Als u toepassingen die worden uitgevoerd op uw virtuele machines die u nodig hebt voor toegang tot hebt, moet u [poorten en eindpunten openen](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

