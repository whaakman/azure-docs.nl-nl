---
title: Wisseling OS-schijf voor een virtuele machine in Azure met PowerShell | Microsoft-Docs
description: De schijf van het besturingssysteem die wordt gebruikt door een virtuele machine van Azure met behulp van PowerShell wijzigen.
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
ms.openlocfilehash: caa8fe2088995e3d30c9b808f639b9280e3a74be
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32196194"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>Wijzigen van de schijf met het besturingssysteem die wordt gebruikt door een virtuele machine in Azure met behulp van PowerShell

Als u een bestaande virtuele machine hebt, maar u wilt dat de schijf voor een back-up schijf of een andere besturingssysteemschijf wisselen, kunt u Azure PowerShell de OS-schijven wisselen. U hoeft niet te verwijderen en opnieuw maken van de virtuele machine. U kunt zelfs een beheerde schijf gebruiken in een andere resourcegroep, zolang deze nog niet in gebruik.

De virtuele machine hoeft te zijn stopped\deallocated en vervolgens de resource-ID van de beheerde schijf kan worden vervangen door de bron-ID van een andere beheerde schijf.

Zorg ervoor dat het VM-grootte en opslag type compatibel zijn met de schijf die u wilt koppelen. Bijvoorbeeld, als de schijf die u wilt gebruiken in Premium-opslag is, moet klikt u vervolgens de virtuele machine geschikt is voor Premium-opslag (zoals de grootte van een DS-serie). 

Een lijst met schijven in een resource-groep met [Get-AzureRmDisk](/powershell/module/azurerm.compute/get-azurermdisk)

```azurepowershell-interactive
Get-AzureRmDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
Wanneer u de naam van de schijf die u wilt gebruiken hebt, stelt u die als de besturingssysteemschijf voor de virtuele machine. In dit voorbeeld stop\deallocates de virtuele machine met de naam *myVM* en wijst u de schijf met de naam *newDisk* als de schijf met het nieuwe besturingssysteem. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzureRmDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzureRmVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**Volgende stappen**

Zie het maken van een kopie van een schijf [momentopname maken van een schijf](snapshot-copy-managed-disk.md).