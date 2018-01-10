---
title: Converteren van Azure storage van de schijven van standaard beheerd naar premium, en vice versa | Microsoft Docs
description: Het converteren van Azure schijven die worden beheerd van standaard naar premium en omgekeerd, met behulp van Azure PowerShell.
services: virtual-machines-windows
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 407cfe7d9eee4e226938f383c04bb359a17290fc
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Converteren van Azure storage van de schijven van standaard beheerd naar premium, en omgekeerd

Beheerde schijven biedt twee opties voor opslag: [Premium](premium-storage.md) (SSD-gebaseerde) en [standaard](standard-storage.md) (gebaseerd op harde schijf). Hiermee kunt u eenvoudig schakelen tussen de twee opties met minimale downtime op basis van uw prestatievereisten past. Deze mogelijkheid is niet beschikbaar voor niet-beheerde schijven. Maar u kunt eenvoudig [converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md) eenvoudig schakelen tussen de twee opties.

In dit artikel leest u hoe beheerde schijven van standard converteren naar premium, en vice versa met behulp van Azure PowerShell. Als u wilt installeren of upgraden, Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Voordat u begint

* De conversie vereist een herstart van de virtuele machine, zodat de migratie van de opslag van uw schijven tijdens een onderhoudsvenster vooraf bestaande plannen. 
* Als u niet-beheerde schijven eerst [converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md) in dit artikel gebruiken om over te schakelen tussen de twee opties voor opslag. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Standaard alle beheerde schijven van een virtuele machine converteren naar premium, en omgekeerd

In het volgende voorbeeld laten we zien hoe overschakelen van de schijven van een virtuele machine van standaard naar premium-opslag. U kunt beheerde premium-schijven, uw virtuele machine gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld wordt ook verandert in een grootte die ondersteuning biedt voor premium-opslag.

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Een beheerde schijf van standard converteren naar premium, en omgekeerd

Voor uw workload ontwikkelen en testen, is het raadzaam combinatie van standard en premium-schijven voor uw kosten hebben. U kunt dit doen door te upgraden naar de premium-opslag alleen op de schijven die betere prestaties zijn vereist. In het volgende voorbeeld laten we zien hoe overschakelen van één schijf van een virtuele machine van standaard naar premium-opslag, en vice versa. U kunt beheerde premium-schijven, uw virtuele machine gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld wordt ook verandert in een grootte die ondersteuning biedt voor premium-opslag.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get the ARM resource to get name and resource group of the VM
$vmResource = Get-AzureRmResource -ResourceId $disk.diskId

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Volgende stappen

Een alleen-lezen kopie van een virtuele machine uitvoeren met behulp van [momentopnamen](snapshot-copy-managed-disk.md).

