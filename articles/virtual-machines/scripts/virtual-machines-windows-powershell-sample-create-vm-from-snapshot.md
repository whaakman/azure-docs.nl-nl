---
title: 'Azure PowerShell-Script voorbeeld: een virtuele machine maken vanuit een momentopname | Microsoft Docs'
description: 'Azure PowerShell-Script voorbeeld: een virtuele machine maken vanuit een momentopname'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 63d108bbfd0f58f8a40bf1c7c8649e3a1f7ed288
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a>Een virtuele machine maken vanuit een momentopname met PowerShell

Dit script maakt een virtuele machine van een momentopname van een besturingssysteemschijf. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt de volgende opdrachten voor het ophalen van de eigenschappen van de momentopname, maakt u een beheerde schijf vanuit een momentopname en een virtuele machine maken. Elk item in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/get-azurermsnapshot) | Hiermee haalt u een momentopname met de naam van de momentopname. |
| [Nieuwe AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) | De configuratie van een schijf maakt. Deze configuratie wordt gebruikt met een proces voor het maken van de schijf. |
| [Nieuwe AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) | Hiermee maakt u een beheerde schijf. |
| [Nieuwe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Maakt een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties VM. De configuratie wordt gebruikt tijdens het maken van VM. |
| [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) | De beheerde schijf als besturingssysteemschijf is gekoppeld aan de virtuele machine |
| [Nieuwe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres. |
| [Nieuwe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Maakt een netwerkinterface. |
| [Nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hiermee maakt u een virtuele machine. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle resources binnen. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra virtuele machine PowerShell-script kunnen worden gevonden in de [virtuele machine van Windows Azure-documentatie](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).