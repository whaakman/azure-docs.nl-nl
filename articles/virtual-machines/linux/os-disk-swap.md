---
title: Wisseling OS-schijf voor een Azure-VM met CLI | Microsoft-Docs
description: De schijf van het besturingssysteem die wordt gebruikt door een virtuele machine van Azure met behulp van de CLI wijzigen.
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
ms.openlocfilehash: 1732b60ee135b765cdeea43f981bcef9575ff32c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>De OS-schijf die wordt gebruikt door een virtuele machine in Azure met behulp van de CLI wijzigen


Als u een bestaande virtuele machine hebt, maar u wilt dat de schijf voor een back-up schijf of een andere besturingssysteemschijf wisselen, kunt u de Azure CLI wisselen van de OS-schijven. U hoeft niet te verwijderen en opnieuw maken van de virtuele machine. U kunt zelfs een beheerde schijf gebruiken in een andere resourcegroep, zolang deze nog niet in gebruik.

De virtuele machine hoeft te zijn stopped\deallocated en vervolgens de resource-ID van de beheerde schijf kan worden vervangen door de bron-ID van een andere beheerde schijf. 

Zorg ervoor dat het VM-grootte en opslag type compatibel zijn met de schijf die u wilt koppelen. Bijvoorbeeld, als de schijf die u wilt gebruiken in Premium-opslag is, moet klikt u vervolgens de virtuele machine geschikt is voor Premium-opslag (zoals de grootte van een DS-serie).

Dit artikel is vereist voor Azure CLI versie 2.0.25 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


Gebruik [az Schijflijst](/cli/azure/disk#list) voor een lijst van de schijven in de resourcegroep.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Gebruik [az vm stoppen](/cli/azure/vm#stop) naar de virtuele machine voordat de schijven wisselen stop\deallocate.

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
   
Opnieuw opstarten van de virtuele machine met behulp [az vm start](/cli/azure/vm#start).

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Volgende stappen**

Zie het maken van een kopie van een schijf [momentopname maken van een schijf](snapshot-copy-managed-disk.md).