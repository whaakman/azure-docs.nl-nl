---
title: Een momentopname van een VHD maken in Azure | Microsoft Docs
description: Informatie over het maken van een kopie van een Azure-VM om te gebruiken als een back up of voor het oplossen van problemen.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: dea8547905cb558cb0be7dc23f89099773e84ff0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58074775"
---
# <a name="create-a-snapshot"></a>Een momentopname maken

Een momentopname is een volledige, alleen-lezen kopie van een virtuele harde schijf (VHD). U kunt een momentopname van een besturingssysteem of de gegevensschijf VHD te gebruiken als een back-up of problemen met virtuele machine (VM).

Als u de momentopname gebruiken wilt voor het maken van een nieuwe virtuele machine, wordt u aangeraden dat u de juiste wijze de virtuele machine afgesloten voor het maken van een momentopname, om te wissen van alle processen die uitgevoerd worden.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **een resource maken**, en zoek en selecteer **momentopname**.
3. In de **momentopname** venster **maken**. De **maken momentopname** venster wordt weergegeven.
4. Voer een **naam** voor de momentopname.
5. Selecteer een bestaande [resourcegroep](../../azure-resource-manager/resource-group-overview.md#resource-groups) of Voer de naam van een nieuwe. 
6. Selecteer de **Locatie** van een Azure-datacenter.  
7. Voor **bronschijf**, selecteert u de beheerde schijf met de momentopname.
8. Selecteer de **accounttype** te gebruiken voor het opslaan van de momentopname. Selecteer **Standard_HDD**, tenzij u de momentopname moet worden opgeslagen op een schijf met hoge prestaties.
9. Selecteer **Maken**.

## <a name="use-powershell"></a>PowerShell gebruiken

De volgende stappen laten zien hoe u de VHD-schijf kopiëren, de configuratie van de momentopnamen maken en een momentopname van de schijf met behulp van de [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot) cmdlet. 

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

1. Sommige parameters instellen: 

   ```azurepowershell-interactive
   $resourceGroupName = 'myResourceGroup' 
   $location = 'eastus' 
   $vmName = 'myVM'
   $snapshotName = 'mySnapshot'  
   ```

2. Haal de virtuele machine:

   ```azurepowershell-interactive
   $vm = get-azvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
   ```

3. Maak de momentopname-configuratie. In dit voorbeeld wordt de momentopname van de besturingssysteemschijf is:

   ```azurepowershell-interactive
   $snapshot =  New-AzSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
   ```
   
   > [!NOTE]
   > Als u wilt voor het opslaan van de momentopname in de zone-robuuste opslag, maakt u dit in een regio die ondersteuning biedt voor [beschikbaarheidszones](../../availability-zones/az-overview.md) en bevatten de `-SkuName Standard_ZRS` parameter.   
   
4. De momentopname:

   ```azurepowershell-interactive
   New-AzSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
   ```


## <a name="next-steps"></a>Volgende stappen

Een virtuele machine maken van een momentopname van een beheerde schijf maken op basis van een momentopname en vervolgens de nieuwe beheerde schijf als de besturingssysteemschijf te koppelen. Zie voor meer informatie het voorbeeld in [maken van een virtuele machine van een momentopname met PowerShell](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json).
