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
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 756711abb014829971af126c5cb60c12e79c920e
ms.contentlocale: nl-nl
ms.lasthandoff: 05/11/2017


---
# <a name="different-ways-to-create-a-linux-vm"></a>Verschillende manieren om een virtuele Linux-machine te maken
In Azure hebt u de flexibiliteit om een virtuele Linux-machine te maken met behulp van hulpprogramma's en werkstromen die u prettig vindt. Dit artikel bevat een overzicht van deze verschillen en voorbeelden voor het maken van de virtuele Linux-machines, inclusief de Azure CLI 2.0. U kunt ook opties voor het maken bekijken, inclusief de [Azure CLI 1.0](creation-choices-nodejs.md).

De [Azure CLI 2.0](/cli/azure/install-az-cli2) is beschikbaar op diverse platforms via een NPM-pakket, door distributeurs geleverde pakketten of Docker-container. Installeer de meest geschikte build voor uw omgeving en meld u aan bij een Azure-account met [az login](/cli/azure/#login)

* [Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0](quick-create-cli.md)
  
  * Maak een resourcegroep met [az group create](/cli/azure/group#create) met de naam *myResourceGroup*: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * Maak met [az vm create](/cli/azure/vm#create) een virtuele machine met de naam *myVM* met behulp van de meest recente *UbuntuLTS*-installatiekopie en genereer SSH-sleutels als deze al bestaan in *~/.ssh*:

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Een virtuele Linux-machine maken met een Azure-sjabloon](create-ssh-secured-vm-from-template.md)
  
  * In het volgende voorbeeld wordt [az group deployment create](/cli/azure/group/deployment#create) gebruikt om een virtuele machine te maken op basis van een sjabloon die is opgeslagen op GitHub:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Een virtuele Linux-machine maken en aanpassen met cloud-init](tutorial-automate-vm-deployment.md)

* [Een maximaal beschikbare toepassing met gelijke taakverdeling maken op meerdere virtuele Linux-machines](tutorial-load-balancer.md)


## <a name="azure-portal"></a>Azure Portal
In [Azure Portal](https://portal.azure.com) kunt u snel een VM maken omdat u niets hoeft te installeren op uw systeem. Azure Portal gebruiken om een virtuele machine te maken:

* [Een virtuele Linux-machine maken met behulp van Azure Portal](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a>Opties voor besturingssysteem en installatiekopieën
Bij het maken van een virtuele machine kiest u een installatiekopie op basis van het besturingssysteem dat u wilt uitvoeren. Azure en de diverse partners bieden een groot aantal installatiekopieën. Sommige hiervan bevatten toepassingen en hulpprogramma’s die vooraf zijn geïnstalleerd. U kunt ook een van uw eigen installatiekopieën uploaden (zie [het volgende gedeelte](#use-your-own-image)).

### <a name="azure-images"></a>Installatiekopieën van Azure
Gebruik de [az vm image](/cli/azure/vm/image)-opdrachten om te zien wat er beschikbaar is per uitgever, distributierelease en build.

Beschikbare uitgevers vermelden:

```azurecli
az vm image list-publishers --location eastus
```

Beschikbare producten (aanbiedingen) voor een bepaalde uitgever vermelden:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Beschikbare SKU's (distributiereleases) van een bepaalde aanbieding vermelden:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Alle beschikbare installatiekopieën voor een bepaalde release vermelden:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Zie [Navigate and select Azure virtual machine images with the Azure CLI](cli-ps-findimage.md) (Navigeren door en selecteren van installatiekopieën voor virtuele machines in Azure met de Azure CLI) voor meer voorbeelden van hoe u door de beschikbare installatiekopieën bladert en deze gebruikt.

De opdracht [az vm create](/cli/azure/vm#create) heeft aliassen waarmee u snel toegang krijgt tot de meest voorkomende distributies en de meest recente versies hiervan. Het is gemakkelijker aliassen te gebruiken dan steeds weer de uitgever, aanbieding, SKU en versie op te geven wanneer u een VM maakt:

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
Als u specifieke aanpassingen nodig hebt, kunt u een installatiekopie gebruiken op basis van een bestaande VM in Azure door de betreffende VM vast te leggen. U kunt ook een installatiekopie uploaden die u on-premises hebt gemaakt. Raadpleeg de volgende artikelen voor meer informatie over ondersteunde distributies en over hoe u uw eigen installatiekopieën kunt gebruiken:

* [Door Azure goedgekeurde distributies](endorsed-distros.md)
* [Informatie over niet-goedgekeurde distributies](create-upload-generic.md)
* [Een installatiekopie maken van een bestaande virtuele Azure-machine](tutorial-custom-images.md).
  
  * Snelstartvoorbeeldopdrachten voor het maken van een installatiekopie van een bestaande virtuele Azure-machine:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a>Volgende stappen
* Maak een virtuele Linux-machine met de [CLI](quick-create-cli.md), vanuit de [portal](quick-create-portal.md) of met behulp van een Azure Resource Manager-[sjabloon](../windows/cli-deploy-templates.md).
* Lees na het maken van een virtuele Linux-machine [meer informatie over Azure-schijven en opslag](tutorial-manage-disks.md).
* Snelle stappen om [een wachtwoord of SSH-sleutels opnieuw in te stellen en gebruikers te beheren](using-vmaccess-extension.md).

