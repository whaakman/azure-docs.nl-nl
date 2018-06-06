---
title: Grootte van een Windows-VM in Azure met PowerShell | Microsoft Docs
description: Het formaat van een virtuele Windows-computer in het Resource Manager-implementatiemodel, met Azure Powershell hebt gemaakt.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 8854f694ce067aaa80a159430a9f48440bcdd95a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701867"
---
# <a name="resize-a-windows-vm"></a>Een Windows VM vergroten of verkleinen

Dit artikel ziet u een virtuele machine verplaatsen naar een andere [VM-grootte](sizes.md) met Azure Powershell.

Nadat u een virtuele machine (VM) gemaakt, u kunt de virtuele machine omhoog of omlaag schalen door het wijzigen van de VM-grootte. In sommige gevallen moet u eerst de VM ongedaan. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op de hardware-cluster waarop de virtuele machine wordt gehost.

Als uw VM Premium-opslag gebruikt, zorg ervoor dat u kiest een **s** versie van de grootte van de Premium-opslag ondersteuning krijgen. Bijvoorbeeld kiezen Standard_E4**s**_v3 in plaats van Standard_E4_v3.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Een Windows-VM niet in een beschikbaarheidsset vergroten of verkleinen

Sommige variabelen worden ingesteld. De waarden vervangt door uw eigen gegevens.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Lijst van de VM-grootten die beschikbaar zijn op de hardware-cluster waarop de virtuele machine wordt gehost. 
   
```powershell
Get-AzureRmVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de virtuele machine aan. Als de gewenste grootte niet wordt weergegeven, gaat u naar stap 3.
   
```powershell
$vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup
```

Als de gewenste grootte niet wordt weergegeven, voer de volgende opdrachten toewijzing van de virtuele machine, het formaat en opnieuw opstarten van de virtuele machine. Replease **<newVMsize>** met de gewenste grootte.
   
```powershell
Stop-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> Toewijzing van de virtuele machine versies dynamische IP-adressen toegewezen aan de virtuele machine. Het besturingssysteem en de gegevensschijven worden niet getroffen. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Een virtuele machine van Windows in een beschikbaarheidsset vergroten of verkleinen

Als de nieuwe grootte voor een virtuele machine in een beschikbaarheidsset niet beschikbaar is op de hardware-cluster momenteel de virtuele machine host is, moet alle VM's in de beschikbaarheidsset aan om de grootte van de virtuele machine te ongedaan. Ook is het mogelijk om bij te werken van de grootte van andere virtuele machines in de beschikbaarheidsset nadat er één virtuele machine is gewijzigd. Als u een virtuele machine in een beschikbaarheidsset, moet u de volgende stappen uitvoeren.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Lijst van de VM-grootten die beschikbaar zijn op de hardware-cluster waarop de virtuele machine wordt gehost. 
   
```powershell
Get-AzureRmVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de virtuele machine aan. Als deze niet wordt weergegeven, gaat u naar de volgende sectie.
   
```powershell
$vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup
```
    
Als de grootte die u wilt dat niet wordt weergegeven, gaat u verder met de volgende stappen uit op alle VM's in de beschikbaarheidsset ongedaan vergroten of verkleinen van virtuele machines en het opnieuw.

Stop alle VM's in de beschikbaarheidsset.
   
```powershell
$as = Get-AzureRmAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

De grootte wijzigen en opnieuw opstarten van de virtuele machines in de beschikbaarheidsset.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzureRmAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Volgende stappen

Voor extra schaalbaarheid, meerdere exemplaren van de VM uitvoeren en uitbreiden. Zie voor meer informatie [automatisch schalen van Windows-machines in een virtuele-Machineschaalset](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

