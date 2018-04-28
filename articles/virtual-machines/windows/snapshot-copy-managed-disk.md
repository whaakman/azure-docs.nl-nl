---
title: Een momentopname van een VHD maken in Azure | Microsoft Docs
description: Informatie over het maken van een kopie van een virtuele machine in Azure wilt gebruiken als een back up of voor het oplossen van problemen.
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: cynthn
ms.openlocfilehash: d7315d3fb7fc156beb85271d0e5aa19ec6baa7a9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-snapshot"></a>Een momentopname maken

Een momentopname van een VHD voor back-up of om op te lossen VM uitgeeft OS- of -schijf. Een momentopname is een volledig, alleen-lezen kopie van een VHD. 

## <a name="use-azure-portal-to-take-a-snapshot"></a>Azure portal gebruiken om een momentopname 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Beginnen in de linkerbovenhoek, klikt u op **maken van een resource** en zoek naar **momentopname**.
3. Klik op de blade momentopname **maken**.
4. Voer een **naam** voor de momentopname.
5. Selecteer een bestaande [Resourcegroep](../../azure-resource-manager/resource-group-overview.md#resource-groups) of typ de gewenste naam voor een nieuwe resourcegroep. 
6. Selecteer een Azure-datacenter locatie.  
7. Voor **bronschijf**, selecteert u de schijf worden beheerd met de momentopname.
8. Selecteer de **accounttype** moet worden gebruikt voor het opslaan van de momentopname. Het is raadzaam **Standard_LRS** tenzij u deze opgeslagen op een hoog presterende schijf nodig.
9. Klik op **Create**.

## <a name="use-powershell-to-take-a-snapshot"></a>PowerShell gebruiken om een momentopname

De volgende stappen ziet u hoe u de VHD-schijf moet worden gekopieerd, maken de momentopnameconfiguraties en een momentopname van de schijf met behulp van de [nieuw AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet. 

Voordat u begint, zorg ervoor dat u de nieuwste versie van de AzureRM.Compute PowerShell-module hebt. Dit artikel is vereist voor de AzureRM moduleversie 5.7.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

Sommige parameters instellen. 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  
```

De virtuele machine worden opgehaald.

 ```azurepowershell-interactive
$vm = get-azurermvm `
   -ResourceGroupName $resourceGroupName `
   -Name $vmName
```

De configuratie van de momentopname maken. In dit voorbeeld gaan we momentopname van de besturingssysteemschijf.

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig `
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
   -Location $location `
   -CreateOption copy
```
   
> [!NOTE]
> Als u wilt voor het opslaan van uw momentopname in de zone robuuste opslag, moet u deze maken in een regio die ondersteuning biedt voor [beschikbaarheid zones](../../availability-zones/az-overview.md) en bevatten de `-SkuName Standard_ZRS` parameter.   

   
De momentopname.

```azurepowershell-interactive
New-AzureRmSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```




## <a name="next-steps"></a>Volgende stappen

Een virtuele machine maken vanuit een momentopname maken van een beheerde schijf vanuit een momentopname en vervolgens de nieuwe beheerde schijf als de OS-schijf koppelen. Zie voor meer informatie de [een virtuele machine maken vanuit een momentopname](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) voorbeeld.
