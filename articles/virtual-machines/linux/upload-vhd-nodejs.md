---
title: Uploaden van een aangepaste installatiekopie Linux met Azure CLI 1.0 | Microsoft Docs
description: Maken en uploaden van een virtuele harde schijf (VHD) naar Azure met een aangepaste Linux-installatiekopie met behulp van het implementatiemodel van Resource Manager en de Azure CLI 1.0.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: ca4c6cb9296028275b2b032af0c94baabeec1223
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-the-azure-cli-10"></a>Uploaden en een Linux-VM uit aangepaste installatiekopie maken met behulp van de Azure CLI 1.0
In dit artikel laat zien hoe een virtuele harde schijf (VHD) uploaden naar Azure met behulp van het Resource Manager-implementatiemodel en virtuele Linux-machines van deze aangepaste installatiekopie maken. Deze functie kunt u installeren en configureren van een Linux-distro aan uw vereisten en gebruik vervolgens deze VHD snel virtuele Azure-machines (VM's) maken.


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-commands) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="quick-commands"></a>Snelle opdrachten
Als u nodig hebt voor de taak, de volgende sectie details snel de base-opdrachten voor het uploaden van een virtuele machine in Azure. Meer gedetailleerde informatie en context voor elke stap u de rest van het document vindt [vanaf hier](#requirements).

Zorg ervoor dat u hebt [de Azure CLI 1.0](../../cli-install-nodejs.md) aangemeld en het gebruik van Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myimages`.

Maak eerst een resourcegroep. Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUs` locatie:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

Een opslagaccount voor het opslaan van uw virtuele schijven maken. Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount`:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Lijst van de toegangssleutels voor uw opslagaccount. Maak een notitie van `key1`:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Maak een container in uw storage-account met de opslagsleutel die u hebt verkregen. Het volgende voorbeeld wordt een container met de naam `myimages` met behulp van de opslag-sleutelwaarde van `key1`:

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Ten slotte uw harde schijf geüpload naar de container die u hebt gemaakt. Geef het lokale pad naar uw VHD onder `/path/to/disk/mydisk.vhd`:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

U kunt nu een virtuele machine maken van de geüploade virtuele schijf [met een Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). U kunt ook de CLI gebruiken door op te geven van de URI naar de schijf (`--image-urn`). Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` eerder met behulp van de virtuele schijf geüpload:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Het doelopslagaccount moet hetzelfde zijn als waarnaar u de virtuele schijf geüpload. U moet ook opgeven of antwoord vraagt, alle extra parameters die zijn vereist voor de `azure vm create` opdracht zoals virtueel netwerk, openbare IP-adres, gebruikersnaam en een SSH-sleutels. U kunt meer lezen over de [beschikbare parameters van de CLI Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Vereisten
De volgende stappen uit te voeren, hebt u het volgende nodig:

* **Linux-besturingssysteem is geïnstalleerd in een .vhd-bestand** -installeren van een [door Azure goedgekeurde Linux-distributie](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (of Raadpleeg [informatie voor niet-goedgekeurde distributies](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) naar een virtuele schijf in de VHD-indeling. Er bestaan meerdere hulpprogramma's om een virtuele machine en de VHD te maken:
  * Installeer en configureer [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) of [KVM](http://www.linux-kvm.org/page/RunningKVM), zorg ervoor dat de VHD gebruiken als uw afbeeldingsindeling. Indien nodig, kunt u [converteren van een installatiekopie van een](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) met `qemu-img convert`.
  * U kunt ook de Hyper-V [op Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) of [op Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. Wanneer u een virtuele machine maakt, geeft u de VHD als de notatie. Indien nodig, kunt u de VHDX-schijven converteren naar VHD gebruiken [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) of de [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Azure biedt bovendien geen ondersteuning dynamische VHD's uploaden, dus u deze schijven worden geconverteerd naar vaste VHD's moet voordat u uploadt. U kunt hulpprogramma's gebruiken zoals [Azure VHD-hulpprogramma's voor Ga](https://github.com/Microsoft/azure-vhd-utils-for-go) naar dynamische schijven converteren tijdens het proces van het uploaden naar Azure.
> 
> 

* Virtuele machines die zijn gemaakt op basis van uw aangepaste installatiekopie moeten zich bevinden in hetzelfde opslagaccount als de afbeelding zelf
  * Een opslagaccount en container voor het opslaan van uw aangepaste installatiekopie en de gemaakte virtuele machines maken
  * Nadat u uw virtuele machines hebt gemaakt, kunt u veilig uw installatiekopie verwijderen

Zorg ervoor dat u hebt [de Azure CLI 1.0](../../cli-install-nodejs.md) aangemeld en het gebruik van Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myimages`.

<a id="prepimage"> </a>

## <a name="prepare-the-image-to-be-uploaded"></a>Voorbereiden van de afbeelding die moet worden geüpload
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


## <a name="create-a-resource-group"></a>Een resourcegroep maken
Resourcegroepen samenbrengen logisch alle Azure-resources voor de ondersteuning van uw virtuele machines, zoals de virtuele netwerken en opslag. Lees meer over [Azure resourcegroepen hier](../../azure-resource-manager/resource-group-overview.md). Voordat u uw aangepaste schijfimage uploaden en het maken van virtuele machines, moet u eerst een resourcegroep maken. 

Het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` in de `WestUS` locatie:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken
Virtuele machines worden opgeslagen als pagina-blobs binnen een opslagaccount. Lees meer over [hier Azure blob-opslag](../../storage/common/storage-introduction.md#blob-storage). U maken een opslagaccount voor uw aangepaste schijfimage en virtuele machines. Alle virtuele machines die u van uw aangepaste schijfimage maakt moeten zich in hetzelfde opslagaccount als installatiekopie.

Het volgende voorbeeld wordt een opslagaccount met de naam `mystorageaccount` in de resourcegroep die eerder hebt gemaakt:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Lijst met sleutels voor opslagaccount
Azure twee 512-bits toegangstoetsen voor elk opslagaccount genereert. Deze toegangstoetsen worden gebruikt bij het verifiëren van de storage-account, zoals bij het uitvoeren van schrijfbewerkingen. Lees meer over [het beheren van toegang tot opslag hier](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Vindt u sneltoetsen met de `azure storage account keys list` opdracht.

Bekijk de toegangssleutels voor het opslagaccount dat u hebt gemaakt:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
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
Op dezelfde manier als u andere mappen maken om maken het lokale bestandssysteem logische manier te organiseren, maakt u containers in een opslagaccount voor het ordenen van uw virtuele schijven en afbeeldingen. Een opslagaccount kan een onbeperkt aantal containers bevatten. 

Het volgende voorbeeld wordt een container met de naam `myimages`, geven de toegangssleutel in de vorige stap hebt verkregen (`key1`):

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Virtuele harde schijf geüpload
Nu kunt u uw aangepaste schijfimage daadwerkelijk uploaden. Als met alle virtuele schijven die door virtuele machines, gebruikt u uploaden en uw aangepaste schijfimage opslaan als een pagina-blob.

Geef uw toegangssleutel, voor de container die u hebt gemaakt in de vorige stap en het pad naar het aangepaste schijfimage op uw lokale computer:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Virtuele machine van de aangepaste installatiekopie maken
Wanneer u virtuele machines van de installatiekopie van uw aangepaste schijf maakt, geeft u de URI op de schijfinstallatiekopie van de. Zorg ervoor dat de bestemming storage-account komt overeen met waar de installatiekopie van uw aangepaste schijf wordt opgeslagen. U kunt uw virtuele machine met de Azure CLI of Resource Manager JSON-sjabloon maken.

### <a name="create-a-vm-using-the-azure-cli"></a>Een virtuele machine maken met de Azure CLI
U geeft de `--image-urn` parameter met de `azure vm create` opdracht uit om te verwijzen naar de schijfinstallatiekopie van uw aangepaste. Zorg ervoor dat `--storage-account-name` overeenkomt met het opslagaccount waar de installatiekopie van uw aangepaste schijf wordt opgeslagen. U beschikt niet over dezelfde container gebruiken als de aangepaste schijfimage voor het opslaan van uw virtuele machines. Zorg ervoor dat eventuele extra containers maken in dezelfde manier als de eerdere stappen voordat u uw aangepaste schijfkopieën uploadt.

Het volgende voorbeeld wordt een virtuele machine met de naam `myVM` van uw aangepaste schijfimage:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Nog steeds moet u opgeven of antwoord vraagt, alle extra parameters die zijn vereist voor de `azure vm create` opdracht zoals virtueel netwerk, openbare IP-adres, gebruikersnaam en een SSH-sleutels. Meer informatie over de [beschikbare parameters van de CLI Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Een virtuele machine met een JSON-sjabloon maken
Azure Resource Manager-sjablonen zijn JSON JavaScript Object Notation ()-bestanden waarmee de omgeving die u wilt maken. De sjablonen zijn onderverdeeld andere resource providers zoals compute of een netwerk. U kunt bestaande sjablonen gebruiken of uw eigen schrijven. Lees meer over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md).

Binnen de `Microsoft.Compute/virtualMachines` provider van de sjabloon die u hebt een `storageProfile` knooppunt dat de configuratiegegevens voor de virtuele machine bevat. De twee belangrijkste parameters bewerken zijn de `image` en `vhd` URI's die verwijzen naar de installatiekopie van uw aangepaste schijf en de virtuele schijf van de nieuwe VM. Hier volgt een voorbeeld van de JSON voor het gebruik van een aangepaste installatiekopie:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

U kunt [deze bestaande sjabloon te maken van een virtuele machine van een aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) of lezen over [maken van uw eigen Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md). 

Zodra u een sjabloon die is geconfigureerd hebt, hebt u uw virtuele machines met behulp van de `azure group deployment create` opdracht. Geef de URI van uw JSON-sjabloon met de `--template-uri` parameter:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Als u een JSON-bestand die lokaal zijn opgeslagen op uw computer hebt, kunt u de `--template-file` parameter in plaats daarvan:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt voorbereid en uw aangepaste virtuele schijf wordt geüpload, kunt u meer lezen over [met Resource Manager en sjablonen](../../azure-resource-manager/resource-group-overview.md). U kunt ook [een gegevensschijf toevoegen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) naar uw nieuwe virtuele machines. Als u toepassingen die worden uitgevoerd op uw virtuele machines die u nodig hebt voor toegang tot hebt, moet u [openen van poorten en eindpunten](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

