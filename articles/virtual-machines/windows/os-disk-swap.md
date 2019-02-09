---
title: Wisselen van OS-schijf voor een Azure-VM met PowerShell | Microsoft-Docs
description: Wijzig de besturingssysteemschijf die worden gebruikt door een virtuele machine van Azure met behulp van PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: 73aab0750d97981d6684d04415683435bbd28797
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55980411"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>Wijzigen van de besturingssysteemschijf die worden gebruikt door een Azure-VM met behulp van PowerShell

Als u een bestaande virtuele machine hebt, maar u wilt de schijf voor een back-up schijf of de schijf met een ander besturingssysteem wisselen, kunt u Azure PowerShell om te vervangen door de OS-schijven. U hoeft niet te verwijderen en opnieuw maken van de virtuele machine. U kunt zelfs een beheerde schijf gebruiken in een andere resourcegroep, zolang deze niet al wordt gebruikt.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

De virtuele machine hoeft te worden stopped\deallocated en vervolgens de resource-ID van de beheerde schijf kan worden vervangen door de resource-ID van een andere beheerde schijf.

Zorg ervoor dat de VM-grootte en de opslag-type zijn compatibel met de schijf die u wilt koppelen. Bijvoorbeeld, als de schijf die u wilt gebruiken in Premium-opslag, moet klikt u vervolgens de virtuele machine geschikt voor Premium-opslag (zoals de grootte van een DS-serie). 

Haal een lijst van schijven in een resource-groep met [Get-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk)

```azurepowershell-interactive
Get-AzDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
Wanneer u de naam van de schijf die u wilt gebruiken, stelt u dat als de besturingssysteemschijf voor de virtuele machine. In dit voorbeeld stop\deallocates de virtuele machine met de naam *myVM* en wijst de schijf met de naam *newDisk* als een schijf met het nieuwe besturingssysteem. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**Volgende stappen**

Zie voor het maken van een kopie van een schijf, [momentopname maken van een schijf](snapshot-copy-managed-disk.md).