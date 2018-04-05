---
title: Een gegevensschijf koppelen aan een Windows-VM in Azure met behulp van PowerShell | Microsoft Docs
description: Klik hier voor meer informatie over het nieuwe of bestaande gegevensschijf koppelen aan een virtuele machine van Windows PowerShell gebruiken met het implementatiemodel van Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: cynthn
ms.openlocfilehash: 9733b17718aaa2f3e24d56fa15c7b84e2e5c72dd
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a>Een gegevensschijf koppelen aan een virtuele machine van Windows met behulp van PowerShell

In dit artikel leest u hoe nieuwe en bestaande schijven koppelen aan een virtuele Windows-machine met behulp van PowerShell. 

Voordat u dit doet, controleert u de volgende tips:
* De omvang van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie [grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Als u Premium-opslag, moet u een Premium-opslag ingeschakeld VM-grootte, zoals de DS-serie- of GS-serie virtuele machine. Zie voor meer informatie [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Een lege gegevensschijf toevoegen aan een virtuele machine

In dit voorbeeld laat zien hoe een lege gegevensschijf toevoegen aan een bestaande virtuele machine.

### <a name="using-managed-disks"></a>Beheerde schijven

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Gebruik van beheerde schijven in een Zone beschikbaarheid
Gebruik voor het maken van een schijf in een Zone beschikbaarheid [nieuw AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) met de `-Zone` parameter. Het volgende voorbeeld wordt een schijf in de zone *1*.


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Initialiseer de schijf

Nadat u een lege schijf toevoegt, moet u voor het initialiseren. U kunt aanmelden bij een virtuele machine en Gebruik Schijfbeheer om te initialiseren van de schijf. Als u WinRM en een certificaat op de virtuele machine ingeschakeld wanneer u het hebt gemaakt, kunt u extern PowerShell initialiseren van de schijf. U kunt ook een extensie voor aangepaste scripts gebruiken: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
Het scriptbestand kan bevatten dat lijkt op deze code voor het initialiseren van de schijven:

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

U kunt een bestaande beheerde schijf koppelen aan een virtuele machine als een gegevensschijf. 

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
