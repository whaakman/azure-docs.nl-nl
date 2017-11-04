---
title: "Azure PowerShell-Script voorbeeld - momentopname van de Export/kopiëren als VHD naar een opslagaccount in andere regio | Microsoft Docs"
description: "Azure PowerShell-Script voorbeeld - momentopname van de Export/kopiëren als VHD naar een opslagaccount in dezelfde andere regio"
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
ms.openlocfilehash: be21a891121df1d645b430d87b572cde6c945d61
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a>Export/kopiëren beheerde momentopnamen als VHD naar een opslagaccount in andere regio met PowerShell

Dit script wordt een momentopname van een beheerde exporteert naar een opslagaccount in andere regio. Het genereert eerst de SAS-URI van de momentopname en vervolgens gebruikt om deze te kopiëren naar een opslagaccount in andere regio. Dit script gebruiken voor het onderhouden van back-up van uw beheerde schijven in andere regio voor herstel na noodgevallen.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt na de opdrachten voor het genereren van SAS-URI voor een momentopname van een beheerde en kopieert de momentopname naar een opslagaccount met SAS-URI. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Verleen AzureRmSnapshotAccess](/powershell/module/azurerm.compute/New-AzureRmDisk) | Genereert SAS-URI voor een momentopname die wordt gebruikt om deze te kopiëren naar een opslagaccount. |
| [Nieuwe AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Maakt een context die storage-account met behulp van de accountnaam en de sleutel. Deze context kan worden gebruikt voor het lezen/schrijven bewerkingen uitvoeren op het opslagaccount. |
| [Start AzureStorageBlobCopy](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | De onderliggende VHD van een momentopname kopieert naar een opslagaccount |

## <a name="next-steps"></a>Volgende stappen

[Een beheerde schijf vanaf een VHD maken](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Een virtuele machine maken van een beheerde schijf](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra virtuele machine PowerShell-script kunnen worden gevonden in de [virtuele machine van Windows Azure-documentatie](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).