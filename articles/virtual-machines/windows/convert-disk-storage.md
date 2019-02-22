---
title: Converteren van Azure managed disks-opslag van standard naar premium, en vice versa | Microsoft Docs
description: Het converteren van Azure beheerde schijven van standard naar premium, en vice versa, met behulp van Azure PowerShell.
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
ms.date: 10/04/2018
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: 14b6559420fad22cfc2294817cbefd23a6c47a70
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650101"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Het opslagtype van een beheerde schijf bijwerken

Azure managed disks biedt vier type opslagopties: Ultra Solid-State drives (SSD), Premium SSD, Standard-SSD en standaard harde schijven (HDD). U kunt een beheerde schijf tussen opslagtypen met minimale downtime, op basis van uw prestatiebehoeften overschakelen. Schakelen tussen opslagtypen wordt niet ondersteund voor een niet-beheerde schijf. u kunt echter eenvoudig [niet-beheerde schijven converteren naar een beheerde schijf](convert-unmanaged-to-managed-disks.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prerequisites"></a>Vereisten

* Omdat de conversie opnieuw moet opstarten van de virtuele machine (VM), moet u plannen dat de migratie van uw opslag schijven tijdens een reeds bestaande onderhoudsperiode. 
* Als u een niet-beheerde schijf eerst [converteren naar een beheerde schijf](convert-unmanaged-to-managed-disks.md) waarmee u kunt het schakelen tussen de opslagtypen. 
* De voorbeelden in dit artikel moet de Azure PowerShell-moduleversie 6.0.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Voer [Connect AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) voor het maken van een verbinding met Azure.

* De voorbeelden in dit artikel moet de Azure PowerShell-moduleversie 6.0.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Voer [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) voor het maken van een verbinding met Azure.

## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium"></a>Alle beheerde schijven van een virtuele machine converteren van standard naar premium

Het volgende voorbeeld ziet hoe u wilt overschakelen van alle schijven van een virtuele machine van standaard naar premium storage. Voor het gebruik van premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld verandert ook in een grootte die ondersteuning biedt voor premium-opslag:

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
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
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

## <a name="convert-a-managed-disk-from-standard-to-premium"></a>Een beheerde schijf converteren van standard naar premium

Voor uw workload ontwikkelen en testen, kunt u een combinatie van standard en premium-schijven om uw kosten te beperken. Upgraden naar premium-opslag alleen deze schijven die betere prestaties nodig hebben om dit te doen. Het volgende voorbeeld laat zien hoe om over te schakelen van één schijf van een virtuele machine van standard naar premium storage, en vice versa. Voor het gebruik van premium-beheerde schijven, de virtuele machine moet gebruiken een [VM-grootte](sizes.md) die ondersteuning biedt voor premium-opslag. In dit voorbeeld laat ook zien hoe overschakelen naar een grootte die ondersteuning biedt voor premium-opslag:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-managed-disks-from-standard-to-premium-in-azure-portal"></a>Beheerde schijven converteren van standard naar premium in Azure portal

U kunt een beheerde schijf converteren van standard naar premium in de Azure-portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de virtuele machine uit de lijst met **virtuele machines** in de portal.
3. Als de virtuele machine niet gestopt is, klikt u op **stoppen** boven aan de blade overzicht van de virtuele machine en wacht tot de virtuele machine te stoppen.
3. Selecteer in de blade voor de virtuele machine, **schijven** in het menu.
4. Selecteer de schijf die u wilt converteren.
5. Selecteer **configuratie** in het menu.
6. Wijziging de **accounttype** van **Standard HDD** naar **Premium SSD**.
7. Klik op **opslaan** en sluit de blade van de schijf.

De update van het schijftype is effectief onmiddellijk. U kunt uw virtuele machine opnieuw opstarten na de conversie.

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd"></a>Een beheerde schijf converteren van standard HDD naar standard-SSD

Het volgende voorbeeld laat zien hoe om over te schakelen van één schijf van een virtuele machine van de standaard harde schijf naar de standard-SSD, en vice versa:

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

