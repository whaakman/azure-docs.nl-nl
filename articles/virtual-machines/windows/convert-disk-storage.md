---
title: Converteren van Azure managed disks-opslag van standard naar premium, en vice versa | Microsoft Docs
description: Het converteren van Azure beheerde schijven van standard naar premium, en vice versa, met behulp van Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: ramankum
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 7748cf41bb97e0136d7b619debcb60d460df5d8b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39596576"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Converteren van Azure managed disks-opslag van standard naar premium, en vice versa

Managed Disks biedt drie opties voor opslag: [Premium SSD](../windows/premium-storage.md), standaard SSD(Preview) en [Standard HDD](../windows/standard-storage.md). Hiermee kunt u gemakkelijk schakelen tussen de opties met minimale downtime op basis van uw prestatiebehoeften. Dit wordt niet ondersteund voor niet-beheerde schijven. Maar u kunt eenvoudig [converteren naar managed disks](convert-unmanaged-to-managed-disks.md) eenvoudig schakelen tussen de schijftypen.

In dit artikel wordt beschreven hoe u beheerde schijven converteren van standard naar premium, en vice versa met behulp van Azure PowerShell. Als u wilt installeren of upgraden, raadpleegt [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Voordat u begint

* De conversie moet opnieuw worden opgestart van de virtuele machine, dus plan de migratie van uw opslag schijven tijdens een reeds bestaande onderhoudsperiode. 
* Als u niet-beheerde schijven, eerst [converteren naar managed disks](convert-unmanaged-to-managed-disks.md) in dit artikel gebruiken om over te schakelen tussen de opties voor opslag. 
* Dit artikel gebruikmaken van de Azure PowerShell-moduleversie 6.0.0 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). U moet ook `Connect-AzureRmAccount` uitvoeren om een verbinding met Azure tot stand te brengen.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Alle beheerde schijven van een virtuele machine converteren van standard naar premium, en vice versa

Het volgende voorbeeld ziet hoe u wilt overschakelen van alle schijven van een virtuele machine van standaard naar premium storage. Voor het gebruik van premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor premium-opslag.

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'

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
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Een beheerde schijf converteren van standard naar premium, en vice versa

Voor uw workload ontwikkelen en testen, kunt u de combinatie van standard en premium-schijven om uw kosten lager zijn. U kunt dit kunt uitvoeren door te upgraden naar premium-opslag, alleen de schijven die betere prestaties nodig hebben. Het volgende voorbeeld laat zien hoe om over te schakelen van één schijf van een virtuele machine van standard naar premium storage, en vice versa. Voor het gebruik van premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor premium-opslag.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

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

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Een beheerde schijf converteren van standard HDD naar standard-SSD, en vice versa

Het volgende voorbeeld laat zien hoe om over te schakelen van één schijf van een virtuele machine van de standaard harde schijf naar de standard-SSD, en vice versa.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Volgende stappen

Een alleen-lezen kopie van een virtuele machine maken met behulp van [momentopnamen](snapshot-copy-managed-disk.md).

