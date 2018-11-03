---
title: Kopiëren van een Linux-VM met behulp van Azure CLI | Microsoft Docs
description: Informatie over het maken van een kopie van uw Azure Linux-VM met behulp van Azure CLI en Managed Disks.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/17/2018
ms.author: cynthn
ms.openlocfilehash: a9caaaa0dfe402339ba01be899073bb17de15906
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50962041"
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-and-managed-disks"></a>Een kopie van een Linux-VM maken met behulp van Azure CLI en Managed Disks

In dit artikel wordt beschreven hoe u een kopie van uw Azure-machine (VM) waarop Linux wordt uitgevoerd met behulp van de Azure CLI en het implementatiemodel Azure Resource Manager maken. 

U kunt ook [uploaden en een virtuele machine maken vanaf een VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Vereisten

-   Installeer de [Azure CLI](/cli/azure/install-az-cli2).

-   Meld u aan bij een Azure-account met [az login](/cli/azure/reference-index#az-login).

-   Een Azure-VM moet worden gebruikt als bron voor uw exemplaar hebben.

## <a name="stop-the-source-vm"></a>De bron-VM stoppen

Toewijzing van de bron-VM met behulp van [az vm deallocate](/cli/azure/vm#az-vm-deallocate).
Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken *myVM* in de resourcegroep *myResourceGroup*:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

## <a name="copy-the-source-vm"></a>De bron-VM kopiëren

Als u wilt kopiëren van een virtuele machine, moet u een kopie van de onderliggende virtuele harde schijf maken. Dit proces maakt een gespecialiseerde virtuele harde schijf (VHD) als een beheerde schijf met dezelfde configuratie en instellingen als de bron-VM.

Zie [Overzicht van Azure Managed Disks](../windows/managed-disks-overview.md) voor meer informatie over Azure Managed Disks. 

1.  Lijst met elke virtuele machine en de naam van de OS-schijf met de [az vm list](/cli/azure/vm#az-vm-list). Het volgende voorbeeld worden alle virtuele machines in de resourcegroep met de naam *myResourceGroup*:
    
    ```azurecli
    az vm list -g myResourceGroup \
         --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' \
         --output table
    ```

    De uitvoer lijkt op die in het volgende voorbeeld:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Kopieer de schijf met het maken van een nieuwe beheerde schijf en met behulp van [az schijf maken](/cli/azure/disk#az-disk-create). Het volgende voorbeeld wordt een schijf met de naam *myCopiedDisk* van de beheerde schijf met de naam *myDisk*:

    ```azurecli
    az disk create --resource-group myResourceGroup \
         --name myCopiedDisk --source myDisk
    ``` 

1.  Controleer of de beheerde schijven nu in de resourcegroep met behulp van [az Schijflijst](/cli/azure/disk#az-disk-list). Het volgende voorbeeld worden de beheerde schijven in de resourcegroep met de naam *myResourceGroup*:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```


## <a name="set-up-a-virtual-network"></a>Instellen van een virtueel netwerk

De volgende optionele stappen maakt u een nieuw virtueel netwerk, subnet, openbare IP-adres en virtuele netwerkinterfacekaart (NIC).

Als u een virtuele machine voor het oplossen van doeleinden of aanvullende implementaties kopieert, kunt u niet wilt dat het gebruik van een virtuele machine in een bestaand virtueel netwerk.

Als u de infrastructuur van een virtueel netwerk maken voor de gekopieerde virtuele machines wilt, volgt u de volgende stappen. Als u niet dat een virtueel netwerk maken wilt, gaat u naar [maken van een virtuele machine](#create-a-vm).

1.  Het virtuele netwerk maken met behulp van [az network vnet maken](/cli/azure/network/vnet#az-network-vnet-create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* en een subnet met de naam *mySubnet*:

    ```azurecli
    az network vnet create --resource-group myResourceGroup \
        --location eastus --name myVnet \
        --address-prefix 192.168.0.0/16 \
        --subnet-name mySubnet \
        --subnet-prefix 192.168.1.0/24
    ```

1.  Een openbaar IP-adres maken met behulp van [az network public-ip maken](/cli/azure/network/public-ip#az-network-public-ip-create). Het volgende voorbeeld wordt een openbaar IP-adres met de naam *myPublicIP* met de DNS-naam van *mypublicdns*. (Omdat de DNS-naam moet uniek zijn, Geef een unieke naam.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup \
        --location eastus --name myPublicIP --dns-name mypublicdns \
        --allocation-method static --idle-timeout 4
    ```

1.  Maak de NIC met behulp van [az network nic maken](/cli/azure/network/nic#az-network-nic-create).
    Het volgende voorbeeld wordt een NIC met de naam *myNic* die gekoppeld aan de *mySubnet* subnet:

    ```azurecli
    az network nic create --resource-group myResourceGroup \
        --location eastus --name myNic \
        --vnet-name myVnet --subnet mySubnet \
        --public-ip-address myPublicIP
    ```

## <a name="create-a-vm"></a>Een virtuele machine maken

Een virtuele machine maken met behulp van [az vm maken](/cli/azure/vm#az-vm-create).

Geef de gekopieerde beheerde schijf te gebruiken als de besturingssysteemschijf (`--attach-os-disk`), als volgt:

```azurecli
az vm create --resource-group myResourceGroup \
    --name myCopiedVM --nics myNic \
    --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure CLI voor het beheren van uw nieuwe virtuele machine, [Azure CLI-opdrachten voor het Azure Resource Manager](../azure-cli-arm-commands.md).
