---
title: Verschillende manieren om een virtuele Linux-machine te maken in Azure | Microsoft Azure
description: Informatie over de verschillende manieren om een virtuele Linux-machine te maken op Azure, waaronder koppelingen naar hulpprogramma&quot;s en zelfstudies voor elke methode.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/03/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 42ee74ac250e6594616652157fe85a9088f4021a
ms.openlocfilehash: 23862762fcf0939ce84859fdae0274421c0bb5fe


---
# <a name="different-ways-to-create-a-linux-vm-including-the-azure-cli-20-preview"></a>Verschillende manieren om een virtuele Linux-machine te maken in Azure met inbegrip van de Azure CLI 2.0 (Preview)
In Azure hebt u de flexibiliteit om een virtuele Linux-machine te maken met behulp van hulpprogramma's en werkstromen die u prettig vindt. Dit artikel bevat een overzicht van de verschillen en voorbeelden voor het maken van de Linux-VM's.

## <a name="azure-cli"></a>Azure CLI
Met een van de volgende CLI-versies kunt u virtuele machines maken in Azure:

- [Azure CLI 1.0](virtual-machines-linux-creation-choices-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel
- Azure CLI 2.0 (Preview): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel (dit artikel)

De [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2) is op diverse platforms beschikbaar via een npm-pakket, door distributeurs geleverde pakketten of Docker-container. Installeer de meest geschikte build voor uw omgeving en meld u aan bij een Azure-account met [az login](/cli/azure/#login)

In de volgende voorbeelden wordt de Azure CLI 2.0 (Preview) gebruikt. Lees elk artikel voor meer informatie over de vermelde opdrachten. U kunt ook voorbeelden vinden voor de Linux-opties met de [Azure CLI 1.0](virtual-machines-linux-creation-choices-nodejs.md).

* [Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0 (Preview)](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * In dit voorbeeld wordt [az group create](/cli/azure/group#create) gebruikt om een resourcegroep te maken met de naam `myResourceGroup`: 
    
    ```azurecli
    az group create --name myResourceGroup --location westus
    ```
    
  * In dit voorbeeld wordt [az vm create](/cli/azure/vm#create) gebruikt om een virtuele machine te maken met de naam `myVM`. Hierbij wordt de meest recente Debian-installatiekopie met Azure Managed Disks gebruikt, evenals de openbare sleutel `id_rsa.pub`:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --public-ip-address-dns-name myPublicDNS \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

    * Als u niet-beheerde schijven wilt gebruiken, voegt u de vlag `--use-unmanaged-disks` toe aan de bovenstaande opdracht. Er wordt een opslagaccount voor u gemaakt. Zie [Azure Managed Disks overview](../storage/storage-managed-disks-overview.md) (Overzicht van Azure Managed Disks) voor meer informatie.

* [Een beveiligde virtuele Linux-machine maken met een Azure-sjabloon](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * In het volgende voorbeeld wordt [az group deployment create](/cli/azure/group/deployment#create) gebruikt om een virtuele machine te maken met een sjabloon die is opgeslagen op GitHub:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Een volledige Linux-omgeving maken met de Azure-CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Omvat het maken van een load balancer en meerdere VM’s in een beschikbaarheidsset.

* [Een schijf toevoegen aan een virtuele Linux-machine](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * In het volgende voorbeeld wordt [az vm disk attach-new](/cli/azure/vm/disk#attach-new) gebruikt om een niet-beheerde schijf van 5 GB met de naam `myDataDisk.vhd` toe te voegen aan een bestaande virtuele machine met de naam `myVM`:
  
    ```azurecli
    az vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
      --disk-size 5 --vhd https://mystorageaccount.blob.core.windows.net/vhds/myDataDisk.vhd
    ```

## <a name="azure-portal"></a>Azure Portal
In [Azure Portal](https://portal.azure.com) kunt u snel een VM maken omdat u niets hoeft te installeren op uw systeem. Azure Portal gebruiken om een virtuele machine te maken:

* [Een virtuele Linux-machine maken met behulp van Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Een schijf koppelen met behulp van Azure Portal](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>Opties voor besturingssysteem en installatiekopieën
Bij het maken van een virtuele machine kiest u een installatiekopie op basis van het besturingssysteem dat u wilt uitvoeren. Azure en de diverse partners bieden een groot aantal installatiekopieën. Sommige hiervan bevatten toepassingen en hulpprogramma’s die vooraf zijn geïnstalleerd. U kunt ook een van uw eigen installatiekopieën uploaden (zie [het volgende gedeelte](#use-your-own-image)).

### <a name="azure-images"></a>Installatiekopieën van Azure
Gebruik de [az vm image](/cli/azure/vm/image)-opdrachten om te zien wat er beschikbaar is per uitgever, distributierelease en build.

Beschikbare uitgevers vermelden:

```azurecli
az vm image list-publishers --location WestUS
```

Beschikbare producten (aanbiedingen) voor een bepaalde uitgever vermelden:

```azurecli
az vm image list-offers --publisher-name Canonical --location WestUS
```

Beschikbare SKU's (distributiereleases) van een bepaalde aanbieding vermelden:

```azurecli
az vm image list-skus --publisher-name Canonical --offer UbuntuServer --location WestUS
```

Alle beschikbare installatiekopieën voor een bepaalde release vermelden:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location WestUS
```

Zie [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Navigeren door en selecteren van installatiekopieën voor virtuele machines in Azure met de Azure CLI) voor meer voorbeelden van hoe u door de beschikbare installatiekopieën bladert en deze gebruikt.

De opdracht **az vm create** heeft aliassen waarmee u snel toegang krijgt tot de meest voorkomende distributies en de meest recente versies hiervan. Het is gemakkelijker aliassen te gebruiken dan steeds weer de uitgever, aanbieding, SKU en versie op te geven wanneer u een VM maakt:

| Alias | Uitgever | Aanbieding | SKU | Versie |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |meest recente |
| CoreOS |CoreOS |CoreOS |Stabiel |meest recente |
| Debian |credativ |Debian |8 |meest recente |
| openSUSE |SUSE |openSUSE |13.2 |meest recente |
| RHEL |RedHat |RHEL |7.2 |meest recente |
| SLES |SLES |SLES |12-SP1 |meest recente |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |meest recente |

### <a name="use-your-own-image"></a>Uw eigen installatiekopie gebruiken
Als u specifieke aanpassingen nodig hebt, kunt u een installatiekopie gebruiken op basis van een bestaande VM in Azure door de betreffende VM *vast te leggen*. U kunt ook een installatiekopie uploaden die u on-premises hebt gemaakt. Raadpleeg de volgende artikelen voor meer informatie over ondersteunde distributies en over hoe u uw eigen installatiekopieën kunt gebruiken:

* [Door Azure goedgekeurde distributies](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Informatie over niet-goedgekeurde distributies](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een virtuele Linux-machine vastleggen als Resource Manager-sjabloon](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
  
  * Voorbeelden van **az vm**-snelstartopdrachten voor het vastleggen van een bestaande virtuele machine met niet-beheerde schijven:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm capture --resource-group myResourceGroup --name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Volgende stappen
* Maak een virtuele Linux-machine via de [portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), met de [CLI](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of met behulp van een Azure Resource Manager-[sjabloon](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Nadat u een Linux-VM hebt gemaakt, [voegt u een gegevensschijf toe](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Snelle stappen om [een wachtwoord of SSH-sleutels opnieuw in te stellen en gebruikers te beheren](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Feb17_HO2-->


