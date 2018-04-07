---
title: Uploaden of kopiëren van een aangepaste Linux-VM met Azure CLI 2.0 | Microsoft Docs
description: Uploaden of kopiëren van een aangepaste virtuele machine met behulp van het implementatiemodel van Resource Manager en de Azure CLI 2.0
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
ms.openlocfilehash: fc3d72ace6398b69a5efa5543c590bba166baaf0
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Linux-VM te maken van aangepaste schijf met de Azure CLI 2.0

<!-- rename to create-vm-specialized -->

Dit artikel ziet u het uploaden van een aangepaste virtuele harde schijf (VHD) of een kopie van een een bestaande VHD in Azure en het maken van nieuwe virtuele Linux-machines (VM's) van de aangepaste schijf. U kunt installeren en configureren van een Linux-distro aan uw vereisten en gebruik vervolgens deze VHD snel maken van een nieuwe virtuele machine van Azure.

Als u maken van meerdere virtuele machines van de aangepaste schijf wilt, maakt u een installatiekopie van de virtuele machine of de VHD. Zie voor meer informatie [maken van een aangepaste installatiekopie van een virtuele machine in Azure met behulp van de CLI](tutorial-custom-images.md).

U hebt hiervoor twee opties:
* [Een VHD uploaden](#option-1-upload-a-specialized-vhd)
* [Kopiëren van een bestaande virtuele machine in Azure](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Snelle opdrachten

Bij het maken van een nieuwe virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create) van een aangepaste of gespecialiseerde schijf u **koppelen** de schijf (--koppelen-os-disk) in plaats van een aangepaste of marketplace-installatiekopie (--afbeelding). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* met behulp van de beheerde schijf met de naam *myManagedDisk* gemaakt op basis van uw aangepaste VHD:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Vereisten
De volgende stappen uit te voeren, hebt u het volgende nodig:

* Een virtuele Linux-machine die is voorbereid voor gebruik in Azure. De [voorbereiden van de virtuele machine](#prepare-the-vm) sectie van dit artikel wordt uitgelegd hoe u specifieke informatie over het installeren van de Azure Linux Agent (waagent) is vereist voor de virtuele machine goed in Azure te laten werken en u kunt verbinding maken met het gebruik van SSH distro vinden.
* Het VHD-bestand van een bestaand [door Azure goedgekeurde Linux-distributie](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (of Raadpleeg [informatie voor niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) naar een virtuele schijf in de VHD-indeling. Er bestaan meerdere hulpprogramma's om een virtuele machine en de VHD te maken:
  * Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), zorg ervoor dat de VHD gebruiken als uw afbeeldingsindeling. Indien nodig, kunt u [converteren van een installatiekopie van een](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) met **qemu img converteren**.
  * U kunt ook de Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [op Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een virtuele machine maakt, geeft u de VHD als de notatie. Indien nodig, kunt u de VHDX-schijven converteren naar VHD gebruiken [qemu img converteren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure biedt bovendien geen ondersteuning dynamische VHD's uploaden, dus u deze schijven worden geconverteerd naar vaste VHD's moet voordat u uploadt. U kunt hulpprogramma's gebruiken zoals [Azure VHD-hulpprogramma's voor Ga](https://github.com/Microsoft/azure-vhd-utils-for-go) naar dynamische schijven converteren tijdens het proces van het uploaden naar Azure.
> 
> 


* Zorg ervoor dat u de meest recente hebt [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/reference-index#az_login).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *mystorageaccount*, en *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>De virtuele machine voorbereiden

Azure biedt ondersteuning voor verschillende Linux-distributies (Zie [goedgekeurde distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). De volgende artikelen helpt u bij het voorbereiden van de verschillende Linux-distributies die worden ondersteund op Azure:

* [Op basis van centOS distributies](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Andere - niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Zie ook de [opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes) voor meer algemene tips voor het Linux-installatiekopieën voorbereiden voor Azure.

> [!NOTE]
> De [Azure-platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) is van toepassing op virtuele machines met Linux alleen als een van de aangebracht distributies wordt gebruikt met configuratiegegevens van de opgegeven onder ondersteunde versies' in [Linux op Azure-Endorsed distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>Optie 1: Een VHD uploaden

U kunt een aangepaste VHD die u hebt uitgevoerd op een lokale computer of die u hebt geëxporteerd uit een andere cloud uploaden. Als u de VHD wilt maken van een nieuwe virtuele machine in Azure, moet u de VHD te uploaden naar een opslagaccount en een beheerde schijf van de VHD te maken. 

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat het uploaden van uw aangepaste schijf en het maken van virtuele machines, moet u eerst een resourcegroep met [az groep maken](/cli/azure/group#az_group_create).

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie: [overzicht van beheerde Azure-schijven](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Create a storage account

Maken van een opslagaccount voor uw aangepaste schijf en virtuele machines met [az storage-account maken](/cli/azure/storage/account#az_storage_account_create). 

Het volgende voorbeeld wordt een opslagaccount met de naam *mystorageaccount* in de resourcegroep die eerder hebt gemaakt:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Lijst met sleutels voor opslagaccount
Azure twee 512-bits toegangstoetsen voor elk opslagaccount genereert. Deze toegangstoetsen worden gebruikt voor verificatie bij de storage-account, zoals de uitvoering van schrijfbewerkingen. Lees meer over [het beheren van toegang tot opslag hier](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Weergeven van de toegangssleutels met [lijst met opslagaccounts die sleutels az](/cli/azure/storage/account/keys#az_storage_account_keys_list).

Bekijk de toegangssleutels voor het opslagaccount dat u hebt gemaakt:

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
Maak een notitie van **key1** terwijl u deze wordt gebruikt om te communiceren met uw opslagaccount in de volgende stappen.

### <a name="create-a-storage-container"></a>Een opslagcontainer maken
Op dezelfde manier als u andere mappen maken om maken het lokale bestandssysteem logische manier te organiseren, maakt u containers in een opslagaccount voor het ordenen van uw schijven. Een opslagaccount kan een onbeperkt aantal containers bevatten. Maken van een container met [az storage-container maken](/cli/azure/storage/container#az_storage_container_create).

Het volgende voorbeeld wordt een container met de naam *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a>De VHD te uploaden
Nu uw aangepaste schijf met uploaden [uploaden van blob storage az](/cli/azure/storage/blob#az_storage_blob_upload). U uploaden en de aangepaste schijf opslaan als een pagina-blob.

Geef uw toegangssleutel, voor de container die u hebt gemaakt in de vorige stap en het pad naar de aangepaste schijf op de lokale computer:

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


Maak een beheerde schijf van de VHD met behulp [az schijf maken](/cli/azure/disk#az_disk_create). Het volgende voorbeeld wordt een beheerde schijf met de naam *myManagedDisk* van de VHD die u hebt geüpload naar uw benoemde opslagaccount en container:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Optie 2: Een bestaande virtuele machine kopiëren

U kunt ook de aangepaste virtuele machine in Azure maken en kopieer de OS-schijf en koppelt u dit aan een nieuwe virtuele machine om een andere kopie te maken. Gebruiken voor het testen, maar als u gebruiken van een bestaande virtuele machine van Azure als het model voor meerdere nieuwe virtuele machines wilt, echt moet u een **installatiekopie** in plaats daarvan. Zie voor meer informatie over het maken van een installatiekopie van een bestaande virtuele machine van Azure [een aangepaste installatiekopie van een virtuele machine met behulp van de CLI van Azure maken](tutorial-custom-images.md)

### <a name="create-a-snapshot"></a>Een momentopname maken

In dit voorbeeld wordt een momentopname van een virtuele machine met de naam *myVM* in de resourcegroep *myResourceGroup* en maakt een momentopname met de naam *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a>De beheerde-schijf maken

Maak een nieuwe beheerde schijf van de momentopname.

De ID van de momentopname niet ophalen. In dit voorbeeld wordt de momentopname heet *osDiskSnapshot* en is in de *myResourceGroup* resourcegroep.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

De beheerde schijf maken. In dit voorbeeld wordt een beheerde schijf met de naam maakt *myManagedDisk* van onze momentopname die is 128 GB groot in standard-opslag.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>De virtuele machine maken

Maak nu uw virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create) en koppelt (--koppelen-os-disk) de beheerde schijf als de besturingssysteemschijf. Het volgende voorbeeld wordt een virtuele machine met de naam *myNewVM* met de beheerde schijf gemaakt op basis van uw geüploade VHD:

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
Nadat u hebt voorbereid en uw aangepaste virtuele schijf wordt geüpload, kunt u meer lezen over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md). U kunt ook [een gegevensschijf toevoegen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) naar uw nieuwe virtuele machines. Als u toepassingen die worden uitgevoerd op uw virtuele machines die u nodig hebt voor toegang tot hebt, moet u [openen van poorten en eindpunten](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

