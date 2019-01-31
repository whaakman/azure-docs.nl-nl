---
title: Een gegevensschijf koppelen aan een Windows-VM in Azure met behulp van PowerShell | Microsoft Docs
description: Hoe u een nieuwe of bestaande gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell met het Resource Manager-implementatiemodel.
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
ms.date: 10/16/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: abfaf76743f42a3fc4a13878d528c755feeef42a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55458564"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>Een gegevensschijf koppelen aan een Windows-VM met PowerShell

In dit artikel wordt beschreven hoe u zowel nieuwe als bestaande schijven koppelt aan een virtuele machine van Windows met behulp van PowerShell. 

Bekijk eerst de volgende tips:
* De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie, [grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Voor het gebruik van Premium-opslag, moet u een Premium-opslag ingeschakeld VM-type, zoals de DS-reeks of GS-serie virtuele machine. Zie voor meer informatie, [Premium Storage: Opslag met hoge prestaties voor workloads van virtuele Azure-machines](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, in deze zelfstudie moduleversie 6.0.0 van Azure PowerShell vereist of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook om uit te voeren `Connect-AzureRmAccount` voor het maken van een verbinding met Azure.


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Een lege gegevensschijf toevoegen aan een virtuele machine

In dit voorbeeld laat zien hoe een lege gegevensschijf toevoegen aan een bestaande virtuele machine.

### <a name="using-managed-disks"></a>Beheerde schijven

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Beheerde schijven gebruiken in een Beschikbaarheidszone
Gebruik voor het maken van een schijf in een Beschikbaarheidszone [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) met de `-Zone` parameter. Het volgende voorbeeld wordt een schijf in de zone *1*.


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Initialiseer de schijf

Nadat u een lege schijf toevoegt, moet u voor het initialiseren. Voor het initialiseren van de schijf, kunt u zich aanmelden bij een virtuele machine en Gebruik Schijfbeheer. Als u ingeschakeld [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) en een certificaat op de virtuele machine hebt gemaakt, kunt u externe PowerShell gebruiken voor het initialiseren van de schijf. U kunt ook een aangepaste scriptextensie gebruiken: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
Het scriptbestand kan code voor het initialiseren van de schijven, bijvoorbeeld bevatten:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Een bestaande gegevensschijf koppelen aan een virtuele machine

U kunt een bestaande beheerde schijf koppelen aan een virtuele machine als gegevensschijf. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Volgende stappen

Maak een [momentopname](snapshot-copy-managed-disk.md).
