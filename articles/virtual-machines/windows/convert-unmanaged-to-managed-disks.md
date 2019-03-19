---
title: Een Windows virtuele machine van niet-beheerde schijven converteren naar managed disks - Azure Managed Disks | Microsoft Docs
description: Het converteren van een Windows-VM van niet-beheerde schijven naar beheerde schijven met behulp van PowerShell in het Resource Manager-implementatiemodel
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
ms.date: 07/12/2018
ms.author: cynthn
ms.openlocfilehash: d848fdd23f459d7e95e85fe38f2272f4d67c32be
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58120785"
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Een Windows virtuele machine van niet-beheerde schijven converteren naar managed disks

Als u bestaande Windows virtuele machines (VM's) die gebruikmaken van niet-beheerde schijven hebt, kunt u de virtuele machines voor het gebruik van beheerde schijven via converteren de [Azure Managed Disks](managed-disks-overview.md) service. Dit proces converteert zowel schijf met het besturingssysteem en eventuele gekoppelde gegevensschijven.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="before-you-begin"></a>Voordat u begint


* Beoordeling [plannen voor de migratie naar Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

* Beoordeling [de veelgestelde vragen over het migreren naar Managed Disks](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Virtuele machines van één exemplaar converteren
In deze sectie wordt uitgelegd hoe u Azure-VM's van één exemplaar van niet-beheerde schijven converteren naar managed disks. (Als uw VM's zich in een beschikbaarheidsset, Zie de volgende sectie.) 

1. Wijs de virtuele machine met behulp van de [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) cmdlet. Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken `myVM` in de resourcegroep met de naam `myResourceGroup`: 

   ```azurepowershell-interactive
   $rgName = "myResourceGroup"
   $vmName = "myVM"
   Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
   ```

2. De virtuele machine converteren naar managed disks met behulp van de [ConvertTo-AzVMManagedDisk](https://docs.microsoft.com/powershell/module/az.compute/convertto-azvmmanageddisk) cmdlet. Het volgende proces converteert de vorige virtuele machine, met inbegrip van de schijf met het besturingssysteem en eventuele gegevensschijven en de virtuele Machine gestart:

   ```azurepowershell-interactive
   ConvertTo-AzVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
   ```



## <a name="convert-vms-in-an-availability-set"></a>Virtuele machines in een beschikbaarheidsset converteren

Als de virtuele machines die u converteren wilt naar beheerde schijven zijn in een beschikbaarheidsset, moet u eerst de beschikbaarheidsset om een beheerde beschikbaarheidsset te converteren.

1. Converteren van de beschikbaarheidsset met behulp van de [Update AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/update-azavailabilityset) cmdlet. Het volgende voorbeeld wordt de beschikbaarheidsset met de naam bijgewerkt `myAvailabilitySet` in de resourcegroep met de naam `myResourceGroup`:

   ```azurepowershell-interactive
   $rgName = 'myResourceGroup'
   $avSetName = 'myAvailabilitySet'

   $avSet = Get-AzAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
   Update-AzAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
   ```

   Als de regio waar uw beschikbaarheidsset zich bevindt alleen 2 beheerde foutdomeinen heeft, maar het aantal niet-beheerde foutdomeinen is 3, met deze opdracht toont een fout die lijkt op "het aantal opgegeven foutdomeinen 3 moet vallen binnen het bereik van 1 tot 2." Los de fout op door het foutdomein naar 2 en update bijwerken `Sku` naar `Aligned` als volgt:

   ```azurepowershell-interactive
   $avSet.PlatformFaultDomainCount = 2
   Update-AzAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
   ```

2. Toewijzing ongedaan maken en de virtuele machines in de beschikbaarheidsset converteren. Het volgende script maakt de toewijzingen elke VM ongedaan met behulp van de [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) cmdlet, converteert u deze met behulp van [ConvertTo-AzVMManagedDisk](https://docs.microsoft.com/powershell/module/az.compute/convertto-azvmmanageddisk), en start deze opnieuw automatisch elkaar van de conversie:

   ```azurepowershell-interactive
   $avSet = Get-AzAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

   foreach($vmInfo in $avSet.VirtualMachinesReferences)
   {
     $vm = Get-AzVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
   }
   ```


## <a name="troubleshooting"></a>Problemen oplossen

Als er een fout is opgetreden tijdens de conversie, of als een virtuele machine bevindt zich in een foutstatus vanwege problemen in een vorige conversie, de `ConvertTo-AzVMManagedDisk` cmdlet opnieuw uit. Een eenvoudige opnieuw proberen is gewoonlijk beter zicht de situatie.
Voordat u converteert, zorg ervoor dat alle VM-extensies zijn in de status 'Inrichten is voltooid' of de conversie mislukt met foutcode 409.


## <a name="convert-using-the-azure-portal"></a>Converteren met behulp van de Azure-portal

U kunt ook niet-beheerde schijven converteren naar managed disks met behulp van de Azure portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de virtuele machine uit de lijst met virtuele machines in de portal.
3. Selecteer in de blade voor de virtuele machine, **schijven** in het menu.
4. Aan de bovenkant van de **schijven** Selecteer **migreren naar managed disks**.
5. Als uw virtuele machine zich in een beschikbaarheidsset, worden er een waarschuwing weergegeven op de **migreren naar managed disks** blade die u moet eerst de beschikbaarheidsset converteren. De waarschuwing moet een koppeling die u kunt klikken op als u wilt converteren van de beschikbaarheidsset. Zodra de beschikbaarheidsset is geconverteerd, of als uw virtuele machine zich niet in een beschikbaarheidsset, klikt u op **migreren** om het proces van de migratie van uw schijven naar beheerde schijven te starten. 

De virtuele machine worden gestopt en opnieuw opgestart nadat de migratie is voltooid.

## <a name="next-steps"></a>Volgende stappen

[Standaard beheerde schijven converteren naar premium](convert-disk-storage.md)

Een alleen-lezen kopie van een virtuele machine maken met behulp van [momentopnamen](snapshot-copy-managed-disk.md).

