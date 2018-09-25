---
title: Wisselen van OS-schijf voor een Azure-VM met behulp van CLI | Microsoft-Docs
description: Wijzig de besturingssysteemschijf die worden gebruikt door een Azure-machine met de CLI.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: 983c2e6d03735ba26f7660fc07dcf1a05ef88189
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960393"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>Wijzigen van de besturingssysteemschijf die worden gebruikt door een Azure-VM met behulp van de CLI


Als u een bestaande virtuele machine hebt, maar u wilt de schijf voor een back-up schijf of de schijf met een ander besturingssysteem wisselen, kunt u de Azure CLI de OS-schijven wisselen. U hoeft niet te verwijderen en opnieuw maken van de virtuele machine. U kunt zelfs een beheerde schijf gebruiken in een andere resourcegroep, zolang deze niet al wordt gebruikt.

De virtuele machine hoeft te worden stopped\deallocated en vervolgens de resource-ID van de beheerde schijf kan worden vervangen door de resource-ID van een andere beheerde schijf. 

Zorg ervoor dat de VM-grootte en de opslag-type zijn compatibel met de schijf die u wilt koppelen. Bijvoorbeeld, als de schijf die u wilt gebruiken in Premium-opslag, moet klikt u vervolgens de virtuele machine geschikt voor Premium-opslag (zoals de grootte van een DS-serie).

In dit artikel gebruikmaken van Azure CLI versie 2.0.25 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 


Gebruik [az Schijflijst](/cli/azure/disk#list) voor een lijst van de schijven in de resourcegroep.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Gebruik [az vm stop](/cli/azure/vm#stop) aan de virtuele machine voordat u de schijven wisselen stop\deallocate.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Gebruik [az vm update](/cli/azure/vm#az-vm-update) met de volledige resource-ID van de nieuwe schijf voor de `--osdisk` parameter 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
Start opnieuw op de virtuele machine met behulp van [az vm start](/cli/azure/vm#start).

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Volgende stappen**

Zie voor het maken van een kopie van een schijf, [momentopname maken van een schijf](snapshot-copy-managed-disk.md).