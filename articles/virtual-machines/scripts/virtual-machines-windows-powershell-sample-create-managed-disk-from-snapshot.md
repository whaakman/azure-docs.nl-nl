---
title: 'Azure PowerShell-Script voorbeeld: een beheerde schijven maken vanuit een momentopname | Microsoft Docs'
description: 'Azure PowerShell-Script voorbeeld: een beheerde schijven maken vanuit een momentopname'
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 39ddb49771a55af55949b75623c2d4755b625208
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Een beheerde schijf maken vanuit een momentopname met PowerShell

Dit script maakt een beheerde schijf van een momentopname. Gebruik dit voor een virtuele machine herstellen van momentopnamen van het besturingssysteem en gegevensschijven. OS maken en gegevens van de respectieve momentopnamen schijven die worden beheerd en maak vervolgens een nieuwe virtuele machine door het koppelen van beheerde schijven. U kunt ook de gegevensschijven van een bestaande virtuele machine herstellen door het koppelen van gegevensschijven gemaakt op basis van momentopnamen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, wordt in deze zelfstudie vereist dat u Azure PowerShell-moduleversie 4.0 of hoger zijn. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure PowerShell installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het maken van een beheerde schijf vanuit een momentopname. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | Eigenschappen van de momentopname opgehaald.  |
| [Nieuwe AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Maakt de configuratie van de schijf die wordt gebruikt voor het maken van de schijf. Dit omvat de resource-Id van de momentopname van de bovenliggende, de locatie die is hetzelfde als de locatie van momentopname van de bovenliggende en een opslagtype.  |
| [Nieuwe AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Hiermee maakt u een schijf met de schijfconfiguratie, de naam van de en Resourcegroepnaam als parameters doorgegeven. |


## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken van een beheerde schijf](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra virtuele machine PowerShell-script kunnen worden gevonden in de [virtuele machine van Windows Azure-documentatie](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).