---
title: Azure PowerShell-Script steekproef - maken van een beheerde schijf van een VHD-bestand in een opslagaccount in dezelfde of verschillende abonnement | Microsoft Docs
description: Azure PowerShell-Script steekproef - maken van een beheerde schijf van een VHD-bestand in een opslagaccount in dezelfde of een ander abonnement
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
ms.openlocfilehash: 6d37fb1308ce0b866b42f961ada84d0869f25615
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>Maak een beheerde schijf van een VHD-bestand in een opslagaccount in dezelfde of verschillende abonnement met PowerShell

Dit script maakt een beheerde schijf van een VHD-bestand in een opslagaccount in dezelfde of een ander abonnement. Dit script gebruiken voor het importeren van een gespecialiseerde (geen gegeneraliseerde/Sysprep voorbereide) VHD naar beheerde OS-schijf voor het maken van een virtuele machine. Deze ook gebruiken voor het importeren van een VHD-gegevens naar beheerde gegevensschijf. 

Maak meerdere identieke beheerde schijven van een VHD-bestand in een kleine hoeveelheid tijd. Als u wilt maken beheerde schijven van een vhd-bestand, blob-momentopname van het vhd-bestand wordt gemaakt en wordt vervolgens gebruikt om beheerde schijven te maken. Slechts één blob momentopname kan worden gemaakt in een minuut dat ervoor zorgt het maken van schijffouten vanwege een beperking dat. Om te voorkomen dat deze beperking, maakt u een [beheerde momentopname van het vhd-bestand](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) en gebruik beheerd voor de beheerde momentopname maken van meerdere schijven vervolgens in korte tijd. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, wordt in deze zelfstudie vereist dat u Azure PowerShell-moduleversie 4.0 of hoger zijn. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure PowerShell installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het maken van een beheerde schijf vanaf een VHD in een ander abonnement. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Maakt de configuratie van de schijf die wordt gebruikt voor het maken van de schijf. Dit omvat opslagtype, locatie, resource-Id van het opslagaccount waarin de bovenliggende VHD is opgeslagen, VHD-URI van de bovenliggende VHD. |
| [Nieuwe AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Hiermee maakt u een schijf met de schijfconfiguratie, de naam van de en Resourcegroepnaam als parameters doorgegeven. |

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken door het koppelen van een beheerde schijf als besturingssysteemschijf](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra virtuele machine PowerShell-script kunnen worden gevonden in de [virtuele machine van Windows Azure-documentatie](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).