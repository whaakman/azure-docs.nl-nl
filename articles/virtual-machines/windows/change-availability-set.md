---
title: Wijzigen van een beschikbaarheidsset VMs | Microsoft Docs
description: Informatie over het wijzigen van de beschikbaarheidsset voor uw virtuele machines met behulp van Azure PowerShell en het Resource Manager-implementatiemodel.
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 44c90f90-bc9a-4260-a36f-5465e2a1ef94
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/15/2016
ms.author: cynthn
ms.openlocfilehash: 4ce76c35082775dd3fdabfb23d312c7653cb4e73
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="change-the-availability-set-for-a-windows-vm"></a>Wijzig de beschikbaarheidsset voor een virtuele machine van Windows
De volgende stappen beschrijven het wijzigen van de beschikbaarheidsset van een virtuele machine met Azure PowerShell. Een virtuele machine kan alleen worden toegevoegd aan een beschikbaarheidsset wanneer deze wordt gemaakt. Wijzigen van de beschikbaarheid instellen, moet u verwijderen en opnieuw maken van de virtuele machine. 

## <a name="change-the-availability-set-using-powershell"></a>Wijzig de beschikbaarheidsset met behulp van PowerShell
1. Vastleggen van de volgende belangrijke details van de virtuele machine worden gewijzigd.
   
    Naam van de virtuele machine
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
   
    VM-grootte
   
    ```powershell
    $vm.HardwareProfile.VmSize
    ```
   
    De primaire netwerkinterface netwerk en optionele netwerkinterfaces als deze bestaan op de virtuele machine
   
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```
   
    Profiel van de Besturingssysteemschijf
   
    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```
   
    Schijf profielen voor elke gegevensschijf 
   
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```
   
    VM-extensies geïnstalleerd 
   
    ```powershell
    $vm.Extensions
    ```
2. Verwijder de virtuele machine zonder een van de schijven of de netwerkinterfaces te verwijderen.
   
    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```
3. Maken van de beschikbaarheidsset als deze niet al bestaat
   
    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```
4. Maak de virtuele machine met behulp van de nieuwe beschikbaarheidsset op
   
    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>
   
    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]
   
    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 
   
    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 
5. Gegevensschijven en -extensies toevoegen. Zie voor meer informatie [gegevensschijf koppelen aan VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [uitbreidingen in het Resource Manager-sjablonen](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions). Gegevensschijven en -extensies kunnen worden toegevoegd aan de virtuele machine met PowerShell of Azure CLI.

## <a name="example-script"></a>Voorbeeldscript
Het volgende script geeft een voorbeeld van de vereiste gegevens verzamelen, worden verwijderd van de oorspronkelijke virtuele machine en vervolgens opnieuw te maken in een nieuwe beschikbaarheidsset.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkProfile.NetworkInterfaces) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic.Id
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Volgende stappen
Extra opslagruimte toevoegen aan uw virtuele machine met het toevoegen van een extra [gegevensschijf](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

