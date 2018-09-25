---
title: Een virtuele Linux-machine in Azure van niet-beheerde schijven converteren naar managed disks - Azure Managed Disks | Microsoft Docs
description: Een Linux-VM van niet-beheerde schijven converteren naar beheerde schijven met behulp van Azure CLI in het Resource Manager-implementatiemodel
services: virtual-machines-linux
documentationcenter: ''
author: roygara
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/15/2017
ms.author: rogarana
ms.openlocfilehash: 62ac70134c1bdf9d178d59723ff73561049a4bbf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993067"
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Een virtuele Linux-machine van niet-beheerde schijven converteren naar managed disks

Als u bestaande Linux virtuele machines (VM's) die gebruikmaken van niet-beheerde schijven hebt, kunt u de virtuele machines gebruiken converteren [Azure Managed Disks](../linux/managed-disks-overview.md). Dit proces converteert zowel schijf met het besturingssysteem en eventuele gekoppelde gegevensschijven.

In dit artikel wordt beschreven hoe u virtuele machines converteren met behulp van de Azure CLI. Als u wilt installeren of upgraden, raadpleegt [Azure CLI installeren](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Voordat u begint
* Beoordeling [de veelgestelde vragen over het migreren naar Managed Disks](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Virtuele machines van één exemplaar converteren
In deze sectie wordt uitgelegd hoe u Azure-VM's van één exemplaar van niet-beheerde schijven converteren naar managed disks. (Als uw VM's zich in een beschikbaarheidsset, Zie de volgende sectie.) Dit proces kunt u de virtuele machines van premium (SSD), niet-beheerde schijven naar premium-beheerde schijven, of van standaard (HDD) niet-beheerde schijven converteren naar standard managed disks.

1. Wijs de virtuele machine met behulp van [az vm deallocate](/cli/azure/vm#az_vm_deallocate). Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken `myVM` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. De virtuele machine converteren naar managed disks met behulp van [az vm converteren](/cli/azure/vm#az_vm_convert). Het volgende proces zet de virtuele machine met de naam `myVM`, met inbegrip van de schijf met het besturingssysteem en eventuele gegevensschijven:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Start de virtuele machine na de conversie naar beheerde schijven met behulp van [az vm start](/cli/azure/vm#az_vm_start). Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Virtuele machines in een beschikbaarheidsset converteren

Als de virtuele machines die u converteren wilt naar beheerde schijven zijn in een beschikbaarheidsset, moet u eerst de beschikbaarheidsset om een beheerde beschikbaarheidsset te converteren.

De toewijzing van alle virtuele machines in de beschikbaarheidsset moeten ongedaan worden gemaakt voordat u de beschikbaarheidsset converteren. Wilt u alle virtuele machines converteren naar beheerde schijven na de beschikbaarheidsset zelf is geconverteerd naar een beheerde beschikbaarheidsset. Vervolgens start u de virtuele machines en verder uitgevoerd die normaal werken.

1. Lijst met alle virtuele machines in een beschikbaarheidsset met behulp van [az vm availability-set list](/cli/azure/vm/availability-set#az_vm_availability_set_list). Het volgende voorbeeld worden alle virtuele machines in de beschikbaarheidsset met de naam `myAvailabilitySet` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Toewijzing van alle virtuele machines met behulp van [az vm deallocate](/cli/azure/vm#az_vm_deallocate). Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken `myVM` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Converteren van de beschikbaarheidsset met behulp van [az vm availability-set convert](/cli/azure/vm/availability-set#az_vm_availability_set_convert). Het volgende voorbeeld wordt de beschikbaarheidsset met de naam geconverteerd `myAvailabilitySet` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Alle virtuele machines converteren naar managed disks met behulp van [az vm converteren](/cli/azure/vm#az_vm_convert). Het volgende proces zet de virtuele machine met de naam `myVM`, met inbegrip van de schijf met het besturingssysteem en eventuele gegevensschijven:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Alle virtuele machines starten na de conversie naar beheerde schijven met behulp van [az vm start](/cli/azure/vm#az_vm_start). Het volgende voorbeeld wordt de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over opslagopties [overzicht Azure Managed Disks](../windows/managed-disks-overview.md).
