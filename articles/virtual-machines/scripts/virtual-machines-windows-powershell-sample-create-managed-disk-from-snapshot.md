---
title: 'Azure PowerShell-voorbeeldscript: een beheerde schijf maken op basis van een momentopname | Microsoft Docs'
description: 'Azure PowerShell-voorbeeldscript: een beheerde schijf maken op basis van een momentopname'
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
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 253193c00371aae9450b6001895d8a870af2513a
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730103"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Een beheerde schijf maken met PowerShell op basis van een momentopname

Met dit script maakt u een beheerde schijf op basis van een momentopname. Gebruik het script om een virtuele machine te herstellen op basis van momentopnamen van het besturingssysteem en gegevensschijven. Maak beheerde OS- en gegevensschijven op basis van de respectieve momentopnamen en maak vervolgens een nieuwe virtuele machine door beheerde schijven te koppelen. U kunt ook gegevensschijven van een bestaande virtuele machine herstellen door het koppelen van gegevensschijven die zijn gemaakt op basis van momentopnamen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten om een beheerde schijf te maken op basis van een momentopname. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/Get-AzSnapshot) | Hiermee worden de eigenschappen van de momentopname opgehaald.  |
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | Hiermee wordt de schijfconfiguratie gemaakt die voor het maken van de schijf wordt gebruikt. Het bevat de resource-id van momentopname, de locatie (die dezelfde is als de locatie van de bovenliggende momentopname) en het opslagtype.  |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Hiermee maakt u een schijf die de schijfconfiguratie, de schijfnaam en de naam van de resourcegroep als parameters gebruikt. |


## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken op basis van een beheerde schijf](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

U kunt extra PowerShell-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor Windows-VM's](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
