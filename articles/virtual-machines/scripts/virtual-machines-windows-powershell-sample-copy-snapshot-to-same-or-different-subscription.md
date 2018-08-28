---
title: 'Azure PowerShell-voorbeeldscript: een momentopname van een beheerde schijf kopiëren (verplaatsen) naar hetzelfde of een ander abonnement | Microsoft Docs'
description: 'Azure PowerShell-voorbeeldscript: een momentopname van een beheerde schijf kopiëren (verplaatsen) naar hetzelfde of een ander abonnement'
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 2905747b6ece93f018bd6ef67fae1b23ec9c9d5d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "41918093"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>Met PowerShell een momentopname van een beheerde schijf kopiëren naar hetzelfde of een ander abonnement

Met dit script maakt u een kopie van een momentopname in hetzelfde abonnement of in een ander abonnement. Gebruik dit script voor het verplaatsen van een momentopname naar een ander abonnement om een kopie van de gegevens te bewaren. Het opslaan van momentopnamen in verschillende abonnementen voorkomt dat u momentopnamen kwijtraakt die per ongeluk worden verwijderd uit uw hoofdabonnement. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een momentopname te maken in het doelabonnement met behulp van de id van de bronmomentopname. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmSnapshotConfig](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | Hiermee maakt u de configuratie voor momentopnamen die wordt gebruikt voor het maken van een momentopname. De opdracht bevat de resource-id van de bovenliggende momentopname en de locatie, die dezelfde is als de locatie van de bovenliggende momentopname.  |
| [New-AzureRmSnapshot](/powershell/module/azurerm.compute/New-AzureRmDisk) | Hiermee maakt u een momentopname aan de hand van de configuratie voor momentopnamen, de naam voor de momentopname en de naam van de resourcegroep die als parameters zijn doorgegeven. |


## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken van een momentopname](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

U kunt extra PowerShell-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor Windows-VM's](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).