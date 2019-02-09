---
title: Grootte van een Windows-VM in Azure met PowerShell | Microsoft Docs
description: Het formaat van een Windows-machine hebt gemaakt in het Resource Manager-implementatiemodel, met behulp van Azure Powershell.
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
ms.openlocfilehash: 0c942056e95812dfbbe6e3b1e8963799088273fb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981193"
---
# <a name="resize-a-windows-vm"></a>Formaat van een Windows VM

Dit artikel leest u hoe u een virtuele machine verplaatsen naar een andere [VM-grootte](sizes.md) met behulp van Azure Powershell.

Nadat u een virtuele machine (VM) maakt, u kunt de virtuele machine omhoog of omlaag schalen door het veranderen van de VM-grootte. In sommige gevallen moet u eerst de virtuele machine toewijzing. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op de hardware-cluster dat momenteel als host voor de virtuele machine fungeert.

Als uw virtuele machine maakt gebruik van Premium Storage, zorgt u ervoor dat u kiest een **s** versie van de grootte voor Premium Storage-ondersteuning. Bijvoorbeeld, kiest u Standard_E4**s**_v3 in plaats van Standard_E4_v3.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Formaat van een Windows-VM niet in een beschikbaarheidsset

Sommige variabelen instellen. Vervang de waarden door uw eigen gegevens.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Lijst met de VM-grootten die beschikbaar zijn op de hardware-cluster waar de virtuele machine wordt gehost. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de virtuele machine aan. Als de gewenste grootte niet wordt vermeld, gaat u naar stap 3.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

Als de gewenste grootte niet wordt weergegeven, voer de volgende opdrachten toewijzing van de virtuele machine, wijzig de grootte ervan, en start de VM opnieuw. Vervang **<newVMsize>** met de gewenste grootte.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> De toewijzing van de virtuele machine ongedaan maken een dynamische IP-adressen toegewezen aan de virtuele machine worden vrijgegeven. Het besturingssysteem en gegevensschijven worden niet beïnvloed. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Formaat van een virtuele Windows-machine in een beschikbaarheidsset

Als de nieuwe grootte voor een virtuele machine in een beschikbaarheidsset niet beschikbaar op de hardware-cluster op dit moment de virtuele machine die als host fungeert is, klikt u vervolgens moet alle virtuele machines in de beschikbaarheidsset ongedaan worden gemaakt om de grootte van de virtuele machine te. Ook is het mogelijk om bij te werken van de grootte van andere virtuele machines in de beschikbaarheidsset nadat een virtuele machine is gewijzigd. Als u wilt het formaat van een virtuele machine in een beschikbaarheidsset, moet u de volgende stappen uitvoeren.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Lijst met de VM-grootten die beschikbaar zijn op de hardware-cluster waar de virtuele machine wordt gehost. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de virtuele machine aan. Als deze niet wordt vermeld, gaat u naar de volgende sectie.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
Als de gewenste grootte niet wordt vermeld, gaat u verder met de volgende stappen voor toewijzing ongedaan maken van alle virtuele machines in de beschikbaarheidsset, vergroten of verkleinen van virtuele machines en start deze opnieuw.

Stop alle virtuele machines in de beschikbaarheidsset.
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Het formaat en de virtuele machines opnieuw opstarten in de beschikbaarheidsset.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Volgende stappen

Voor extra schaalbaarheid, uitvoeren van meerdere VM-exemplaren en de schaal vergroten. Zie voor meer informatie, [Windows-machines automatisch schalen in een virtuele-Machineschaalset](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

