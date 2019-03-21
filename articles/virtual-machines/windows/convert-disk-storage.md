---
title: Converteren van Azure managed disks-opslag van Standard naar Premium- of Premium naar standaard | Microsoft Docs
description: Het converteren van Azure beheerde schijven van Standard naar Premium- of Premium naar standaard met behulp van Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: f97140ffeed9115a0308215ea082baee611501fb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58009887"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Het opslagtype van een beheerde schijf bijwerken

Er zijn vier opties voor Azure beheerde schijven: Azure Ultra Disk-opslag, Premium SSD, Standard-SSD en standaard harde schijven. U kunt schakelen tussen deze soorten opslag op basis van de prestatiebehoeften van uw met weinig downtime. Deze functionaliteit wordt niet ondersteund voor niet-beheerde schijven. Maar u kunt eenvoudig [niet-beheerde schijven converteren naar een beheerde schijf](convert-unmanaged-to-managed-disks.md) te kunnen schakelen tussen schijftypen.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prerequisites"></a>Vereisten

* Omdat de conversie opnieuw moet opstarten van de virtuele machine (VM), moet u plannen dat de migratie van uw disk-opslag tijdens een reeds bestaande onderhoudsperiode.
* Als de schijf niet wordt beheerd, eerst is [converteren naar een beheerde schijf](convert-unmanaged-to-managed-disks.md) , zodat u tussen opslagopties schakelen kunt.

## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Overschakelen van alle beheerde schijven van een virtuele machine tussen Premium en Standard

In dit voorbeeld laat zien hoe alle schijven van een virtuele machine converteren van Standard naar Premium storage of van Premium naar Standard-opslag. Voor het gebruik van Premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor Premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor premium-opslag:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from Standard to Premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to Premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzDiskUpdateConfig –AccountType $storageType
        Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Overschakelen van beheerde schijven van de afzonderlijke naast standaard en Premium

Voor uw workload ontwikkelen en testen, kunt u een combinatie van standaard en Premium-schijven om uw kosten te verlagen. U kunt alleen de schijven die betere prestaties nodig een upgrade uitvoert. In dit voorbeeld laat zien hoe een enkele VM-schijf converteren van Standard naar Premium storage of van Premium naar Standard-opslag. Voor het gebruik van Premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor Premium-opslag. In dit voorbeeld laat ook zien hoe overschakelen naar een grootte die ondersteuning biedt voor Premium storage:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-managed-disks-from-standard-to-premium-in-the-azure-portal"></a>Beheerde schijven converteren van Standard naar Premium in de Azure-portal

Volg deze stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de virtuele machine uit de lijst met **virtuele machines** in de portal.
3. Als de virtuele machine is niet gestopt, selecteert u **stoppen** aan de bovenkant van de virtuele machine **overzicht** deelvenster en wacht tot de virtuele machine te stoppen.
3. Selecteer in het deelvenster voor de virtuele machine, **schijven** in het menu.
4. Selecteer de schijf die u wilt converteren.
5. Selecteer **configuratie** in het menu.
6. Wijziging de **accounttype** van **Standard HDD** naar **Premium SSD**.
7. Klik op **opslaan**, en sluit het deelvenster van de schijf.

Conversie van de schijf is onmiddellijk. U kunt uw virtuele machine opnieuw opstarten na de conversie.

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Overschakelen van beheerde schijven tussen Standard HDD- en Standard-SSD 

In dit voorbeeld laat zien hoe een enkele VM-schijf converteren van Standard-SSD in plaats van Standard HDD of Standard-SSD op standaard harde schijven:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Volgende stappen

Een alleen-lezen kopie van een virtuele machine maken met behulp van een [momentopname](snapshot-copy-managed-disk.md).
