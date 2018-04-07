---
title: Maak een kopie van uw Linux-VM met de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken van een kopie van uw Azure Linux virtuele machine met de Azure CLI 1.0 in het Resource Manager-implementatiemodel
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: bb350f8d14ad451ad3ff7cd617ca3f90967aaa4b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-the-azure-cli-10"></a>Maak een kopie van een virtuele Linux-machine uitgevoerd op Azure met Azure CLI 1.0
In dit artikel leest u hoe een kopie van uw Azure-machine (VM) met het Resource Manager-implementatiemodel met Linux maken. Eerst u via het besturingssysteem en gegevensschijven kopiëren naar een nieuwe container klikt en vervolgens de netwerkbronnen instellen en de nieuwe virtuele machine maken.

U kunt ook [uploaden en een virtuele machine maken van aangepaste schijfimage](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- Azure CLI 1.0: onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel (dit artikel)
- [Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u voldoet aan met de volgende vereisten voordat u de stappen uit:

* U hebt de [Azure CLI](../../cli-install-nodejs.md) gedownload en geïnstalleerd op uw computer. 
* Ook moet u enkele gegevens over uw bestaande Azure Linux VM:

| Informatie over het bron-VM | Waar downloaden |
| --- | --- |
| VM-naam |`azure vm list` |
| De naam van resourcegroep |`azure vm list` |
| Locatie |`azure vm list` |
| Naam van het opslagaccount |`azure storage account list -g <resourceGroup>` |
| Containernaam |`azure storage container list -a <sourcestorageaccountname>` |
| Bronnaam VM VHD-bestand |`azure storage blob list --container <containerName>` |

* U moet enkele keuzes maken over de nieuwe virtuele machine:    <br> -Containernaam    <br> VM - naam    <br> VM - grootte    <br> -vNet-naam    <br> -Subnetnaam    <br> -IP-naam    <br> De naam van de - NIC

## <a name="login-and-set-your-subscription"></a>Aanmelding en stel uw abonnement
1. Aanmelden bij de CLI.

    ```azurecli
    azure login
    ```
2. Zorg ervoor dat u in de modus Resource Manager.

    ```azurecli
    azure config mode arm
    ```
3. Stel het juiste abonnement. U kunt de lijst met azure-account gebruiken om te zien van al uw abonnementen.

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-the-vm"></a>De virtuele machine stoppen
Stop en toewijzing van de bron-VM. U kunt de lijst met azure vm gebruiken voor een lijst van alle van de virtuele machines in uw abonnement en hun namen.

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-the-vhd"></a>Kopieer de VHD
U kunt de VHD van de bron-opslag kopiëren met het doel de `azure storage blob copy start`. In dit voorbeeld gaan we Kopieer de VHD naar hetzelfde opslagaccount, maar een andere container.

Als u wilt de VHD naar een andere container in hetzelfde opslagaccount kopieert, typt u:

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Het virtuele netwerk instellen voor uw nieuwe virtuele machine
Stel een virtueel netwerk en de NIC in voor uw nieuwe virtuele machine. 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-the-new-vm"></a>De nieuwe virtuele machine maken
U kunt nu een virtuele machine maken van de geüploade virtuele schijf [met behulp van een resource manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) of via de CLI door de URI naar de gekopieerde schijf te typen:

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van Azure CLI voor het beheren van uw nieuwe virtuele machine, [Azure CLI-opdrachten voor het Azure Resource Manager](../azure-cli-arm-commands.md).

