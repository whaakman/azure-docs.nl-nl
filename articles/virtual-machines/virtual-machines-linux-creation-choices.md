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
ms.sourcegitcommit: 44c46fff9ccf9c7dba9ee380faf5f8213b58e3c3
ms.openlocfilehash: 4397d84ef4d97bdee387777a193ec0b969f2d5e1


---
# <a name="different-ways-to-create-a-linux-vm-including-azure-cli-20-preview"></a>Verschillende manieren om een virtuele Linux-machine te maken in Azure met inbegrip van Azure CLI 2.0 (Preview)
In Azure hebt u de flexibiliteit om een virtuele Linux-machine te maken met behulp van hulpprogramma's en werkstromen die u prettig vindt. Dit artikel bevat een overzicht van de verschillen en voorbeelden voor het maken van de Linux-VM's.

## <a name="azure-cli"></a>Azure CLI

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- Azure CLI 1.0: onze CLI voor het klassieke implementatiemodel en het implementatiemodel voor resourcebeheer
- [Azure CLI 2.0 (Preview)](../xplat-cli-install.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

De Azure CLI 2.0 (Preview) is op diverse platforms beschikbaar via een npm-pakket, door distributeurs geleverde pakketten of Docker-container. Zorg ervoor dat u bent aangemeld via **az login**.

In de volgende zelfstudies vindt u voorbeelden van het gebruik van de Azure CLI 2.0 (Preview). Lees elk artikel voor meer informatie over de vermelde opdrachten:

* [Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0 (Preview)](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * In dit voorbeeld wordt een resourcegroep met de naam myResourceGroup gemaakt: 
    
    ```azurecli
    az group create -n myResourceGroup -l westus
    ```

  * In dit voorbeeld wordt een virtuele machine in de nieuwe resourcegroep gemaakt met behulp van de meest recente Debian-installatiekopie met een openbare sleutel met de naam `id_rsa.pub`:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
    --admin-username ops \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --public-ip-address-dns-name mydns \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

* [Een beveiligde virtuele Linux-machine maken met een Azure-sjabloon](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * In het volgende voorbeeld wordt een VM gemaakt met behulp van een op GitHub opgeslagen sjabloon:
    
    ```azurecli
    az group deployment create -g myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Een volledige Linux-omgeving maken met de Azure-CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Omvat het maken van een load balancer en meerdere VM’s in een beschikbaarheidsset.

* [Een schijf toevoegen aan een virtuele Linux-machine](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * In het volgende voorbeeld wordt een schijf van 5 GB toegevoegd aan een bestaande VM met de naam `myVM`:
    
    ```azurecli
    az vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
      --disk-size 5 --vhd https://myStorage.blob.core.windows.net/vhds/myDataDisk1.vhd
    ```

## <a name="azure-portal"></a>Azure Portal
In de [Azure-portal](https://portal.azure.com) kunt u snel een VM maken omdat u niets hoeft te installeren op uw systeem. De Azure-portal gebruiken om een virtuele machine te maken:

* [Een virtuele Linux-machine maken met behulp van Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Een schijf koppelen met behulp van Azure Portal](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>Opties voor besturingssysteem en installatiekopieën
Bij het maken van een virtuele machine kiest u een installatiekopie op basis van het besturingssysteem dat u wilt uitvoeren. Azure en de diverse partners bieden een groot aantal installatiekopieën. Sommige hiervan bevatten toepassingen en hulpprogramma’s die vooraf zijn geïnstalleerd. U kunt ook een van uw eigen installatiekopieën uploaden (zie [het volgende gedeelte](#use-your-own-image)).

### <a name="azure-images"></a>Installatiekopieën van Azure
Gebruik de CLI-opdrachten van `az vm image` om te zien wat er beschikbaar is per uitgever, distributierelease en build.

Beschikbare uitgevers vermelden:

```azurecli
az vm image list-publishers -l WestUS
```

Beschikbare producten (aanbiedingen) voor een bepaalde uitgever vermelden:

```azurecli
az vm image list-offers --publisher-name Canonical -l WestUS
```

Beschikbare SKU's (distributiereleases) van een bepaalde aanbieding vermelden:

```azurecli
az vm image list-skus --publisher-name Canonical --offer UbuntuServer -l WestUS
```

Alle beschikbare installatiekopieën voor een bepaalde release vermelden:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS -l WestUS
```

Zie [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Navigeren door en selecteren van installatiekopieën voor virtuele machines in Azure met de Azure CLI) voor meer voorbeelden van hoe u door de beschikbare installatiekopieën bladert en deze gebruikt.

De opdracht `az vm create` heeft aliassen waarmee u snel toegang kunt krijgen tot de meest voorkomende distributies en hun meest recente versies. Het is gemakkelijker aliassen te gebruiken dan steeds weer de uitgever, aanbieding, SKU en versie op te geven wanneer u een VM maakt:

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
  
  * Voorbeelden van snelstartopdrachten voor het vastleggen van een bestaande VM:
    
    ```azurecli
    az vm deallocate -g myResourceGroup -n myVM
    az vm generalize -g myResourceGroup -n myVM
    az vm capture -g myResourceGroup -n myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Volgende stappen
* Maak een virtuele Linux-machine via de [portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), met de [CLI](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of met behulp van een Azure Resource Manager-[sjabloon](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Nadat u een Linux-VM hebt gemaakt, [voegt u een gegevensschijf toe](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Snelle stappen om [een wachtwoord of SSH-sleutels opnieuw in te stellen +en gebruikers te beheren](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)




<!--HONumber=Jan17_HO1-->


