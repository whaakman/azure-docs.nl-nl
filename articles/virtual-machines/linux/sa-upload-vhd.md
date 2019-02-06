---
title: Uploaden van een aangepaste Linux-schijf met de Azure CLI | Microsoft Docs
description: Maken en uploaden van een virtuele harde schijf (VHD) naar Azure met behulp van het Resource Manager-implementatiemodel en Azure CLI
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
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 01d3a20022972b0e18de02bd2730ca31e57cd77a
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55755019"
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli"></a>Uploaden en een Linux-VM maken van een aangepaste schijf met de Azure CLI

In dit artikel wordt beschreven hoe u een virtuele harde schijf (VHD) uploaden naar Azure storage-account met de Azure CLI en Linux-VM's maken van deze aangepaste schijf. Deze functie kunt u installeren en configureren van een Linux-distributie voor uw vereisten en vervolgens die VHD gebruiken om snel virtuele Azure-machines (VM's).

In dit onderwerp wordt gebruikgemaakt van storage-accounts voor de laatste virtuele harde schijven, maar u kunt ook deze stappen met [beheerde schijven](upload-vhd.md). 

## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende sectie gegevens snel de base-opdrachten voor een VHD uploaden naar Azure. Meer gedetailleerde informatie en -context voor elke stap u de rest van het document vindt, [Hier beginnen](#requirements).

Zorg ervoor dat u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `mydisks`.

Maak eerst een resourcegroep met [az group create](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `WestUs`:

```azurecli
az group create --name myResourceGroup --location westus
```

Maak een opslagaccount voor het opslaan van de virtuele schijven met [az storage-account maken](/cli/azure/storage/account). Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Lijst van de toegangssleutels voor uw opslagaccount met [az storage account sleutels lijst](/cli/azure/storage/account/keys). Maak een notitie van `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Maak een container in uw storage-account met behulp van de opslagsleutel u hebt verkregen met [az storage container maken](/cli/azure/storage/container). Het volgende voorbeeld wordt een container met de naam `mydisks` met behulp van de opslag-sleutelwaarde van `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Ten slotte uw VHD uploaden naar de container die u hebt gemaakt met [az storage blob upload](/cli/azure/storage/blob). Geef het lokale pad naar uw VHD onder `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Geef de URI naar de schijf (`--image`) met [az vm maken](/cli/azure/vm). Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` met behulp van de virtuele schijf eerder hebt geüpload:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Het doelopslagaccount is gelijk aan waar u de virtuele schijf om te uploaden. U moet ook opgeven of antwoord vraagt, alle extra parameters die zijn vereist voor de **az vm maken** opdracht, zoals virtuele netwerken, openbare IP-adres, gebruikersnaam en SSH-sleutels. U kunt meer lezen over de [beschikbare CLI Resource Manager-parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Vereisten
Als u wilt de volgende stappen hebt voltooid, hebt u het volgende nodig:

* **Linux-besturingssysteem die is geïnstalleerd in een VHD-bestand** -installatie van een [door Azure onderschreven Linux-distributie](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (of Raadpleeg [informatie over niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) op een virtuele schijf in de VHD-indeling . Er bestaan meerdere hulpprogramma's voor het maken van een virtuele machine en de VHD:
  * Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), zorg ervoor dat u VHD gebruiken als uw installatiekopie-indeling. Indien nodig, kunt u [converteren van een installatiekopie van een](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) met behulp van `qemu-img convert`.
  * U kunt ook de Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [op Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een virtuele machine maakt, geeft u de VHD als de notatie. Indien nodig, kunt u de VHDX-schijven converteren naar VHD met behulp van [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure biedt bovendien geen ondersteuning voor dynamische VHD's, uploaden, dus u deze schijven converteren naar vaste VHD's moet voordat u uploadt. U kunt hulpprogramma's zoals [VHD hulpprogramma's voor Azure voor GO](https://github.com/Microsoft/azure-vhd-utils-for-go) naar dynamische schijven converteren tijdens het proces van het uploaden naar Azure.
> 
> 

* Virtuele machines die zijn gemaakt op basis van uw aangepaste schijf moeten zich bevinden in hetzelfde opslagaccount als de schijf zelf optreedt
  * Een storage-account en een container voor het opslaan van uw aangepaste schijf en de gemaakte virtuele machines maken
  * Nadat u alle virtuele machines hebt gemaakt, kunt u veilig de schijf verwijderen

Zorg ervoor dat u de meest recente [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index).

In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-the-disk-to-be-uploaded"></a>Voorbereiden van de schijf moet worden geüpload
Azure biedt ondersteuning voor verschillende Linux-distributies (Zie [onderschreven distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). De volgende artikelen helpen u bij het voorbereiden van de verschillende Linux-distributies die worden ondersteund op Azure:

* **[Op basis van centOS-distributies](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Andere - niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Zie ook de **[opmerkingen bij de installatie van Linux](create-upload-generic.md#general-linux-installation-notes)** voor meer algemene tips voor het maken van Linux-installatiekopieën voor Azure.

> [!NOTE]
> De [Azure-platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) is van toepassing op virtuele machines waarop Linux wordt uitgevoerd alleen als een van de onderschreven distributies wordt gebruikt met de informatie over de configuratie zoals opgegeven bij de ondersteunde versies' in [Linux op door Azure onderschreven Distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Resourcegroepen samenbrengen logisch alle Azure-resources ter ondersteuning van uw virtuele machines, zoals de virtuele netwerken en opslag. Zie voor meer informatie resourcegroepen, [resourcegroepen overzicht](../../azure-resource-manager/resource-group-overview.md). Voordat u uw aangepaste schijf uploaden en het maken van virtuele machines, moet u eerst een resourcegroep maken met [az-groep maken](/cli/azure/group).

In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westus`:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>Create a storage account

Maak een opslagaccount voor uw aangepaste schijf en VM's met [az storage-account maken](/cli/azure/storage/account). Virtuele machines met niet-beheerde schijven die u van uw aangepaste schijf moet maakt zich in hetzelfde opslagaccount als de schijf. 

Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount` in de resourcegroep die eerder hebt gemaakt:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Een lijst met storage accountsleutels
Azure twee 512-bits toegangssleutels voor elk opslagaccount genereert. Deze toegangssleutels worden gebruikt bij het verifiëren van de storage-account, zoals bij het uitvoeren van bewerkingen voor schrijven. Meer informatie over [beheren van toegang tot opslag hier](../../storage/common/storage-account-manage.md#access-keys). Weergeven van de toegangssleutel met [az storage account sleutels lijst](/cli/azure/storage/account/keys).

De toegangssleutel voor het opslagaccount dat u hebt gemaakt bekijken:

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
Maak een notitie van `key1` als u dit gebruikt om te communiceren met uw opslagaccount in de volgende stappen.

## <a name="create-a-storage-container"></a>Maak een opslagcontainer
Op dezelfde manier die u verschillende mappen maakt voor het lokale bestandssysteem logische manier te organiseren, door containers in een opslagaccount voor het ordenen van uw schijven te maken. Een opslagaccount kan een onbeperkt aantal containers bevatten. Maak een container met [az storage container maken](/cli/azure/storage/container).

Het volgende voorbeeld wordt een container met de naam `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>VHD uploaden
Nu de aangepaste schijf met uploaden [az storage blob upload](/cli/azure/storage/blob). U uploaden en uw aangepaste schijf opslaan als een pagina-blob.

Geef uw toegangssleutel, de container die u hebt gemaakt in de vorige stap en het pad naar de aangepaste schijf op de lokale computer:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a>De virtuele machine maken
Voor het maken van een virtuele machine met niet-beheerde schijven, geeft u de URI naar de schijf (`--image`) met [az vm maken](/cli/azure/vm). Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` met behulp van de virtuele schijf eerder hebt geüpload:

U geeft de `--image` parameter met de [az vm maken](/cli/azure/vm) om te verwijzen naar uw aangepaste schijf. Zorg ervoor dat `--storage-account` komt overeen met de storage-account waar uw aangepaste schijf wordt opgeslagen. U hoeft niet te gebruiken dezelfde container als de aangepaste schijf voor het opslaan van uw virtuele machines. Zorg ervoor dat u het maken van andere containers op dezelfde manier als de eerdere stappen voordat u uw aangepaste schijf uploadt.

Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` van uw aangepaste schijf:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

U moet nog steeds om op te geven of antwoord vraagt, alle extra parameters die zijn vereist voor de **az vm maken** opdracht zoals gebruikersnaam en SSH-sleutels.


## <a name="resource-manager-template"></a>Resource Manager-sjabloon
Azure Resource Manager-sjablonen zijn JavaScript Object Notation (JSON)-bestanden die definiëren van de omgeving die u wilt maken. De sjablonen worden onderverdeeld in andere resource-providers, zoals rekencapaciteit of netwerk. U kunt bestaande sjablonen gebruiken of schrijf uw eigen. Meer informatie over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md).

Binnen de `Microsoft.Compute/virtualMachines` provider van de sjabloon die u hebt een `storageProfile` knooppunt dat informatie over de configuratie voor uw virtuele machine bevat. De twee belangrijkste parameters bewerken zijn de `image` en `vhd` URI's die verwijzen naar uw aangepaste schijf en de virtuele schijf van de nieuwe virtuele machine. Hier volgt een voorbeeld van de JSON voor het gebruik van een aangepaste schijf:

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

U kunt [deze bestaande sjabloon voor het maken van een virtuele machine van een aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) of lezen over [het maken van uw eigen Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md). 

Zodra u een sjabloon die is geconfigureerd hebt, gebruikt u [az group deployment maken](/cli/azure/group/deployment) uw virtuele machines maken. Geef de URI van de JSON-sjabloon met de `--template-uri` parameter:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Als u een JSON-bestand lokaal opgeslagen op uw computer hebt, kunt u de `--template-file` parameter in plaats daarvan:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt voorbereid en uw aangepaste virtuele schijf geüpload, kunt u meer lezen over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md). U kunt ook naar [een gegevensschijf toevoegen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) aan uw nieuwe VM's. Als u toepassingen die worden uitgevoerd op uw virtuele machines die u nodig hebt voor toegang tot hebt, moet u [poorten en eindpunten openen](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

