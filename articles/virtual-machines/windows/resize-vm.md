---
title: Grootte van een Windows-VM in Azure met PowerShell | Microsoft Docs
description: Het formaat van een virtuele Windows-computer in het Resource Manager-implementatiemodel, met Azure Powershell hebt gemaakt.
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 742efd1496de9ce76b1e5636297ef30f546bd108
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-windows-vm"></a>Een Windows VM vergroten of verkleinen
In dit artikel leest u hoe het formaat van een virtuele machine van Windows, in het Resource Manager-implementatiemodel met Azure Powershell hebt gemaakt.

Nadat u een virtuele machine (VM) gemaakt, u kunt de virtuele machine omhoog of omlaag schalen door het wijzigen van de VM-grootte. In sommige gevallen moet u eerst de VM ongedaan. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op de hardware-cluster waarop de virtuele machine wordt gehost.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Een Windows-VM niet in een beschikbaarheidsset vergroten of verkleinen
1. Lijst van de VM-grootten die beschikbaar zijn op de hardware-cluster waarop de virtuele machine wordt gehost. 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de virtuele machine aan. Als de gewenste grootte niet wordt weergegeven, gaat u naar stap 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Als de gewenste grootte niet wordt weergegeven, voer de volgende opdrachten toewijzing van de virtuele machine, het formaat en opnieuw opstarten van de virtuele machine.
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> Toewijzing van de virtuele machine versies dynamische IP-adressen toegewezen aan de virtuele machine. Het besturingssysteem en de gegevensschijven worden niet getroffen. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Een virtuele machine van Windows in een beschikbaarheidsset vergroten of verkleinen
Als de nieuwe grootte voor een virtuele machine in een beschikbaarheidsset niet beschikbaar is op de hardware-cluster momenteel de virtuele machine host is, moet alle VM's in de beschikbaarheidsset aan om de grootte van de virtuele machine te ongedaan. Ook is het mogelijk om bij te werken van de grootte van andere virtuele machines in de beschikbaarheidsset nadat er één virtuele machine is gewijzigd. Als u een virtuele machine in een beschikbaarheidsset, moet u de volgende stappen uitvoeren.

1. Lijst van de VM-grootten die beschikbaar zijn op de hardware-cluster waarop de virtuele machine wordt gehost.
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de virtuele machine aan. Als deze niet wordt weergegeven, gaat u naar stap 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Als de gewenste grootte niet wordt weergegeven, gaat u verder met de volgende stappen uit op alle VM's in de beschikbaarheidsset ongedaan vergroten of verkleinen van virtuele machines en het opnieuw.
4. Stop alle VM's in de beschikbaarheidsset.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. De grootte wijzigen en opnieuw opstarten van de virtuele machines in de beschikbaarheidsset.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a>Volgende stappen
* Voor extra schaalbaarheid, meerdere exemplaren van de VM uitvoeren en uitbreiden. Zie voor meer informatie [automatisch schalen van Windows-machines in een virtuele-Machineschaalset](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

