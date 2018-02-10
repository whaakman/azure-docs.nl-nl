---
title: Een virtuele Linux-machine in Azure van niet-beheerde schijven converteren naar beheerde schijven - beheerde Azure-schijven | Microsoft Docs
description: Linux-VM van niet-beheerde schijven converteren naar beheerde schijven met behulp van Azure CLI 2.0 in het Resource Manager-implementatiemodel
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 75031b6189710286625406246e6dcde6f1c2b938
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Een virtuele Linux-machine van niet-beheerde schijven converteren naar beheerde schijven

Als u bestaande Linux virtuele machines (VM's) die gebruikmaken van niet-beheerde schijven hebt, kunt u de virtuele machines te gebruiken converteren [Azure beheerd schijven](../linux/managed-disks-overview.md). Dit proces converteert zowel schijf met het besturingssysteem en eventuele aangesloten gegevensschijven.

In dit artikel leest u hoe het converteren van virtuele machines met behulp van de Azure CLI. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Voordat u begint
* Bekijk [de veelgestelde vragen over de migratie naar schijven beheerd](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Single instance VMs converteren
Deze sectie wordt uitgelegd hoe single instance Azure VM's van niet-beheerde schijven converteren naar beheerde schijven. (Als uw virtuele machines zich in een beschikbaarheidsset, Zie de volgende sectie.) Dit proces kunt u de virtuele machines van premium (SSD) zonder begeleiding schijven beheerd premium-schijven, of naar standard (HDD) zonder begeleiding schijven converteren naar beheerde standaardschijven.

1. Toewijzing van de virtuele machine met behulp van [az vm ongedaan](/cli/azure/vm#az_vm_deallocate). Het volgende voorbeeld de virtuele machine met de naam deallocates `myVM` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. De virtuele machine converteren naar beheerde schijven met behulp van [az vm converteren](/cli/azure/vm#az_vm_convert). Het volgende proces zet de virtuele machine met de naam `myVM`, inclusief de besturingssysteemschijf en alle gegevensschijven:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Start de virtuele machine na de conversie naar beheerde schijven met behulp van [az vm start](/cli/azure/vm#az_vm_start). Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Converteren van virtuele machines in een beschikbaarheidsset

Als de virtuele machines die u converteren wilt naar beheerde schijven zich bevinden in een beschikbaarheidsset, moet u eerst de beschikbaarheidsset aan een beheerde beschikbaarheidsset converteren.

Alle virtuele machines in de beschikbaarheidsset moeten ongedaan voordat u de beschikbaarheidsset converteert. Plan alle VM's converteren naar beheerde schijven nadat de beschikbaarheidsset zelf is geconverteerd naar een beheerde beschikbaarheidsset. Vervolgens start u de virtuele machines en verder te werken die normaal werken.

1. Lijst van alle virtuele machines in een beschikbaarheidsset met behulp van [az vm beschikbaarheidsset lijst](/cli/azure/vm/availability-set#az_vm_availability_set_list). Het volgende voorbeeld worden alle virtuele machines in de beschikbaarheidsset benoemde `myAvailabilitySet` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Toewijzing van de virtuele machines met behulp van [az vm ongedaan](/cli/azure/vm#az_vm_deallocate). Het volgende voorbeeld de virtuele machine met de naam deallocates `myVM` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Converteren van de beschikbaarheidsset met behulp van [az vm-beschikbaarheidsset converteren](/cli/azure/vm/availability-set#az_vm_availability_set_convert). Het volgende voorbeeld wordt de beschikbaarheid van de set met de naam geconverteerd `myAvailabilitySet` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. De virtuele machines converteren naar beheerde schijven met behulp van [az vm converteren](/cli/azure/vm#az_vm_convert). Het volgende proces zet de virtuele machine met de naam `myVM`, inclusief de besturingssysteemschijf en alle gegevensschijven:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Start de VM's na de conversie naar beheerde schijven met [az vm start](/cli/azure/vm#az_vm_start). Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over opties voor opslag, [Azure beheerd schijven overzicht](../windows/managed-disks-overview.md).
