---
title: Een momentopname van een VHD maken in Azure | Microsoft Docs
description: Informatie over het maken van een kopie van een virtuele machine in Azure wilt gebruiken als een back up of voor het oplossen van problemen.
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: 10b5eb0062e4a029b0f233ee8af17d590d59c8d4
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-snapshot"></a>Een momentopname maken

Een momentopname van een VHD voor back-up of om op te lossen VM uitgeeft OS- of -schijf. Een momentopname is een volledig, alleen-lezen kopie van een VHD. 

## <a name="use-azure-portal-to-take-a-snapshot"></a>Azure portal gebruiken om een momentopname 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Beginnen in de linkerbovenhoek, klikt u op **nieuw** en zoek naar **momentopname**.
3. Klik op de blade momentopname **maken**.
4. Voer een **naam** voor de momentopname.
5. Selecteer een bestaande [Resourcegroep](../../azure-resource-manager/resource-group-overview.md#resource-groups) of typ de gewenste naam voor een nieuwe resourcegroep. 
6. Selecteer een Azure-datacenter locatie.  
7. Voor **bronschijf**, selecteert u de schijf worden beheerd met de momentopname.
8. Selecteer de **accounttype** moet worden gebruikt voor het opslaan van de momentopname. Het is raadzaam **Standard_LRS** tenzij u deze opgeslagen op een hoog presterende schijf nodig.
9. Klik op **Create**.

## <a name="use-powershell-to-take-a-snapshot"></a>PowerShell gebruiken om een momentopname
De volgende stappen ziet u hoe u de VHD-schijf moet worden gekopieerd, maken de momentopnameconfiguraties en een momentopname van de schijf met behulp van de [nieuw AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet. 

Zorg ervoor dat u de nieuwste versie van de AzureRM.Compute PowerShell-module geïnstalleerd hebt. Voer de volgende opdracht om deze te installeren.

```
Install-Module AzureRM.Compute -MinimumVersion 2.6.0
```
Zie voor meer informatie [Azure PowerShell Versioning](/powershell/azure/overview).


1. Sommige parameters instellen. 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$dataDiskName = 'myDisk' 
$snapshotName = 'mySnapshot'  
```

2. Ophalen van de VHD-schijf moet worden gekopieerd.

 ```azurepowershell-interactive
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Maken van de momentopnameconfiguraties. 

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. De momentopname.

 ```azurepowershell-interactive
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Als u van plan bent de momentopname gebruiken om te maken van een schijf beheerd en koppelt u dit een virtuele machine die moet worden hoge prestaties, gebruikt u de parameter `-AccountType Premium_LRS` met de opdracht New-AzureRmSnapshot. De parameter wordt de momentopname gemaakt zodat deze wordt opgeslagen als een schijf Premium beheerd. Premium-schijven beheerd zijn duurder dan de standaard. Daarom moet u dat wat u moet Premium voordat u deze parameter.

## <a name="next-steps"></a>Volgende stappen

Een virtuele machine maken vanuit een momentopname maken van een beheerde schijf vanuit een momentopname en vervolgens de nieuwe beheerde schijf als de OS-schijf koppelen. Zie voor meer informatie de [een virtuele machine maken vanuit een momentopname](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) voorbeeld.
