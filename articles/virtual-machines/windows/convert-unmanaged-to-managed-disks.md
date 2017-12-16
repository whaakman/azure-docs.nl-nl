---
title: Een virtuele Windows-machine van niet-beheerde schijven converteren naar beheerde schijven - beheerde Azure-schijven | Microsoft Docs
description: Een virtuele Windows-machine converteren van niet-beheerde schijven op schijven die worden beheerd met behulp van PowerShell in het Resource Manager-implementatiemodel
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: cynthn
ms.openlocfilehash: 5f6376bb9e7f172df317b2ec857025bf37793799
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Een virtuele Windows-machine van niet-beheerde schijven converteren naar beheerde schijven

Als u een bestaande Windows virtuele machines (VM's) die gebruikmaken van niet-beheerde schijven hebt, kunt u de virtuele machines voor het gebruik van beheerde schijven via converteren de [Azure beheerd schijven](managed-disks-overview.md) service. Dit proces converteert zowel schijf met het besturingssysteem en eventuele aangesloten gegevensschijven.

In dit artikel leest u hoe het converteren van virtuele machines met behulp van Azure PowerShell. Als u wilt installeren of upgraden, Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Voordat u begint


* Bekijk [plannen voor de migratie naar schijven beheerd](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).
* Bekijk [de veelgestelde vragen over de migratie naar schijven beheerd](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Single instance VMs converteren
Deze sectie wordt uitgelegd hoe single instance Azure VM's van niet-beheerde schijven converteren naar beheerde schijven. (Als uw virtuele machines zich in een beschikbaarheidsset, Zie de volgende sectie.) 

1. Toewijzing van de virtuele machine met behulp van de [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet. Het volgende voorbeeld de virtuele machine met de naam deallocates `myVM` in de resourcegroep met de naam `myResourceGroup`: 

  ```azurepowershell-interactive
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. De virtuele machine converteren naar beheerde schijven met behulp van de [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet. De volgende procedure wordt de vorige VM, inclusief de besturingssysteemschijf en alle gegevensschijven geconverteerd:

  ```azurepowershell-interactive
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. Start de virtuele machine na de conversie naar beheerde schijven met behulp van [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm). Het volgende voorbeeld wordt opnieuw opgestart met de vorige VM:

  ```azurepowershell-interactive
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a>Converteren van virtuele machines in een beschikbaarheidsset

Als de virtuele machines die u converteren wilt naar beheerde schijven zich bevinden in een beschikbaarheidsset, moet u eerst de beschikbaarheidsset aan een beheerde beschikbaarheidsset converteren.

1. Converteren van de beschikbaarheidsset met behulp van de [Update AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet. Het volgende voorbeeld de beschikbaarheid van de set met de naam bijgewerkt `myAvailabilitySet` in de resourcegroep met de naam `myResourceGroup`:

  ```azurepowershell-interactive
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Als de regio waar uw beschikbaarheidsset zich bevindt slechts 2 beheerde foutdomeinen heeft, maar het aantal niet-beheerde foutdomeinen 3, wordt deze opdracht geeft u een fout vergelijkbaar met 'het opgegeven aantal foutdomeinen 3 moet vallen binnen het bereik van 1 tot 2'. Los de fout op door het foutdomein naar 2 en update bijwerken `Sku` naar `Aligned` als volgt:

  ```azurepowershell-interactive
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Toewijzing ongedaan maken en omzetten van de virtuele machines in de beschikbaarheidsset. Het volgende script deallocates elke virtuele machine met behulp van de [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converteert u deze met behulp van [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), en start deze opnieuw met behulp van [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):

  ```azurepowershell-interactive
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name
  }
  ```


## <a name="troubleshooting"></a>Problemen oplossen

Als er een fout optreedt tijdens de conversie, of als een virtuele machine bevindt zich in een foutstatus vanwege problemen in een vorige conversie, voer de `ConvertTo-AzureRmVMManagedDisk` cmdlet opnieuw. Een eenvoudige opnieuw blokkering meestal opgeheven de situatie.
Voordat u converteert, Controleer of alle VM-extensies in de status 'Inrichten is voltooid' of de conversie mislukt met foutcode 409.


## <a name="next-steps"></a>Volgende stappen

[Standaard beheerde schijven worden geconverteerd naar premium](convert-disk-storage.md)

Een alleen-lezen kopie van een virtuele machine uitvoeren met behulp van [momentopnamen](snapshot-copy-managed-disk.md).

