---
title: Uploaden van een aangepaste Linux-schijf met Azure CLI 2.0 | Microsoft Docs
description: Maken en uploaden van een virtuele harde schijf (VHD) in Azure maken met het implementatiemodel van Resource Manager en Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9159960af396e89f373da711e0cc46fdd996ab83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Uploaden en Linux-VM te maken van aangepaste schijf met de Azure CLI 2.0
In dit artikel laat zien hoe een virtuele harde schijf (VHD) uploaden naar Azure storage-account met de Azure CLI 2.0 en virtuele Linux-machines te maken van deze aangepaste schijf. U kunt deze stappen ook uitvoeren met de [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Deze functie kunt u installeren en configureren van een Linux-distro aan uw vereisten en gebruik vervolgens deze VHD snel virtuele Azure-machines (VM's) maken.

In dit onderwerp maakt gebruik van storage-accounts voor de laatste virtuele harde schijven, maar u kunt ook doen met behulp van deze stappen [schijven die worden beheerd](upload-vhd.md). 

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende sectie details snel de base opdrachten een VHD uploaden naar Azure. Meer gedetailleerde informatie en context voor elke stap u de rest van het document vindt [vanaf hier](#requirements).

Zorg ervoor dat u de meest recente hebt [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `mydisks`.

Maak eerst een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUs` locatie:

```azurecli
az group create --name myResourceGroup --location westus
```

Maak een opslagaccount voor het opslaan van uw virtuele schijven met [az storage-account maken](/cli/azure/storage/account#create). Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Lijst van de toegangssleutels voor uw opslagaccount met [lijst met opslagaccounts die sleutels az](/cli/azure/storage/account/keys#list). Maak een notitie van `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Maken van een container in uw storage-account met behulp van de opslagsleutel u hebt verkregen met [az storage-container maken](/cli/azure/storage/container#create). Het volgende voorbeeld wordt een container met de naam `mydisks` met behulp van de opslag-sleutelwaarde van `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Ten slotte uw VHD te uploaden naar de container die u hebt gemaakt met [uploaden van blob storage az](/cli/azure/storage/blob#upload). Geef het lokale pad naar uw VHD onder `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Geef de URI naar de schijf (`--image`) met [az vm maken](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` eerder met behulp van de virtuele schijf geüpload:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Het doelopslagaccount moet hetzelfde zijn als waarnaar u de virtuele schijf geüpload. U moet ook opgeven of antwoord vraagt, alle extra parameters die zijn vereist voor de **az vm maken** opdracht zoals virtueel netwerk, openbare IP-adres, gebruikersnaam en een SSH-sleutels. U kunt meer lezen over de [beschikbare parameters van de CLI Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Vereisten
De volgende stappen uit te voeren, hebt u het volgende nodig:

* **Linux-besturingssysteem is geïnstalleerd in een .vhd-bestand** -installeren van een [door Azure goedgekeurde Linux-distributie](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (of Raadpleeg [informatie voor niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) naar een virtuele schijf in de VHD-indeling. Er bestaan meerdere hulpprogramma's om een virtuele machine en de VHD te maken:
  * Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), zorg ervoor dat de VHD gebruiken als uw afbeeldingsindeling. Indien nodig, kunt u [converteren van een installatiekopie van een](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) met `qemu-img convert`.
  * U kunt ook de Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [op Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een virtuele machine maakt, geeft u de VHD als de notatie. Indien nodig, kunt u de VHDX-schijven converteren naar VHD gebruiken [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure biedt bovendien geen ondersteuning dynamische VHD's uploaden, dus u deze schijven worden geconverteerd naar vaste VHD's moet voordat u uploadt. U kunt hulpprogramma's gebruiken zoals [Azure VHD-hulpprogramma's voor Ga](https://github.com/Microsoft/azure-vhd-utils-for-go) naar dynamische schijven converteren tijdens het proces van het uploaden naar Azure.
> 
> 

* Virtuele machines die zijn gemaakt op basis van uw aangepaste schijf moeten zich bevinden in hetzelfde opslagaccount als de schijf zelf
  * Een opslagaccount en container voor het opslaan van uw aangepaste schijf en de gemaakte virtuele machines maken
  * Nadat u uw virtuele machines hebt gemaakt, kunt u veilig de schijf verwijderen

Zorg ervoor dat u de meest recente hebt [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en geregistreerd in het gebruik van een Azure-account [az aanmelding](/cli/azure/#login).

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-the-disk-to-be-uploaded"></a>Voorbereiden van de schijf worden geüpload
Azure biedt ondersteuning voor verschillende Linux-distributies (Zie [goedgekeurde distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). De volgende artikelen helpt u bij het voorbereiden van de verschillende Linux-distributies die worden ondersteund op Azure:

* **[Op basis van centOS distributies](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Andere - niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Zie ook de  **[opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes)**  voor meer algemene tips voor het Linux-installatiekopieën voorbereiden voor Azure.

> [!NOTE]
> De [Azure-platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) is van toepassing op virtuele machines met Linux alleen als een van de aangebracht distributies wordt gebruikt met configuratiegegevens van de opgegeven onder ondersteunde versies' in [Linux op Azure-Endorsed distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Resourcegroepen samenbrengen logisch alle Azure-resources voor de ondersteuning van uw virtuele machines, zoals de virtuele netwerken en opslag. Zie voor meer informatie resourcegroepen [resourcegroepen overzicht](../../azure-resource-manager/resource-group-overview.md). Voordat het uploaden van uw aangepaste schijf en het maken van virtuele machines, moet u eerst een resourcegroep met [az groep maken](/cli/azure/group#create).

Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `westus` locatie:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Maken van een opslagaccount voor uw aangepaste schijf en virtuele machines met [az storage-account maken](/cli/azure/storage/account#create). Geen VM's met niet-beheerde schijven die u vanuit uw aangepaste schijf moet zich in hetzelfde opslagaccount als deze schijf maken. 

Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount` in de resourcegroep die eerder hebt gemaakt:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Lijst met sleutels voor opslagaccount
Azure twee 512-bits toegangstoetsen voor elk opslagaccount genereert. Deze toegangstoetsen worden gebruikt bij het verifiëren van de storage-account, zoals bij het uitvoeren van schrijfbewerkingen. Lees meer over [het beheren van toegang tot opslag hier](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Weergeven van de toegangssleutels met [lijst met opslagaccounts die sleutels az](/cli/azure/storage/account/keys#list).

Bekijk de toegangssleutels voor het opslagaccount dat u hebt gemaakt:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
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
Maak een notitie van `key1` terwijl u deze wordt gebruikt om te communiceren met uw opslagaccount in de volgende stappen.

## <a name="create-a-storage-container"></a>Een opslagcontainer maken
Op dezelfde manier als u andere mappen maken om maken het lokale bestandssysteem logische manier te organiseren, maakt u containers in een opslagaccount voor het ordenen van uw schijven. Een opslagaccount kan een onbeperkt aantal containers bevatten. Maken van een container met [az storage-container maken](/cli/azure/storage/container#create).

Het volgende voorbeeld wordt een container met de naam `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>Virtuele harde schijf geüpload
Nu uw aangepaste schijf met uploaden [uploaden van blob storage az](/cli/azure/storage/blob#upload). U uploaden en de aangepaste schijf opslaan als een pagina-blob.

Geef uw toegangssleutel, voor de container die u hebt gemaakt in de vorige stap en het pad naar de aangepaste schijf op de lokale computer:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a>De virtuele machine maken
Als een virtuele machine maken met niet-beheerde schijven, geeft u de URI naar de schijf (`--image`) met [az vm maken](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` eerder met behulp van de virtuele schijf geüpload:

U geeft de `--image` parameter met [az vm maken](/cli/azure/vm#create) om te verwijzen naar de schijf van uw aangepaste. Zorg ervoor dat `--storage-account` overeenkomt met het opslagaccount waarin de aangepaste schijf wordt opgeslagen. U beschikt niet over dezelfde container gebruiken als de aangepaste schijf voor het opslaan van uw virtuele machines. Zorg ervoor dat eventuele extra containers maken in dezelfde manier als de eerdere stappen voordat u de aangepaste schijf uploadt.

Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` van uw aangepaste schijf:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Nog steeds moet u opgeven of antwoord vraagt, alle extra parameters die zijn vereist voor de **az vm maken** opdracht zoals gebruikersnaam en het SSH-sleutels.


## <a name="resource-manager-template"></a>Resource Manager-sjabloon
Azure Resource Manager-sjablonen zijn JSON JavaScript Object Notation ()-bestanden waarmee de omgeving die u wilt maken. De sjablonen zijn onderverdeeld andere resource providers zoals compute of een netwerk. U kunt bestaande sjablonen gebruiken of uw eigen schrijven. Lees meer over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md).

Binnen de `Microsoft.Compute/virtualMachines` provider van de sjabloon die u hebt een `storageProfile` knooppunt dat de configuratiegegevens voor de virtuele machine bevat. De twee belangrijkste parameters bewerken zijn de `image` en `vhd` URI's die verwijzen naar uw aangepaste en uw nieuwe VM-virtuele schijf. Hier volgt een voorbeeld van de JSON voor het gebruik van een aangepaste schijf:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

U kunt [deze bestaande sjabloon te maken van een virtuele machine van een aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) of lezen over [maken van uw eigen Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md). 

Zodra u een sjabloon die is geconfigureerd hebt, gebruikt u [az implementatie maken](/cli/azure/group/deployment#create) uw virtuele machines maken. Geef de URI van uw JSON-sjabloon met de `--template-uri` parameter:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Als u een JSON-bestand die lokaal zijn opgeslagen op uw computer hebt, kunt u de `--template-file` parameter in plaats daarvan:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt voorbereid en uw aangepaste virtuele schijf wordt geüpload, kunt u meer lezen over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md). U kunt ook [een gegevensschijf toevoegen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) naar uw nieuwe virtuele machines. Als u toepassingen die worden uitgevoerd op uw virtuele machines die u nodig hebt voor toegang tot hebt, moet u [openen van poorten en eindpunten](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

