---
title: 'Azure PowerShell-Script voorbeeld: een virtuele machine maken door het koppelen van een beheerde schijf als besturingssysteemschijf | Microsoft Docs'
description: 'Azure PowerShell-Script voorbeeld: een virtuele machine maken door het koppelen van een beheerde schijf als besturingssysteemschijf'
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
ms.openlocfilehash: ec532811e94647c8a04b9faf9474f6749969f83e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a>Maak een virtuele machine met behulp van een bestaande beheerde OS-schijf met PowerShell

Dit script maakt een virtuele machine door het koppelen van een bestaande beheerde schijf als de besturingssysteemschijf. Gebruik dit script in de voorgaande scenario's:
* Een virtuele machine maken van een bestaande beheerde OS schijf die is opgehaald uit een beheerde schijf in een ander abonnement
* Een virtuele machine maken van een bestaande beheerde schijf dat is gemaakt vanaf een speciale VHD-bestand 
* Een virtuele machine maken van een bestaande beheerde OS schijf die is gemaakt vanuit een momentopname 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt de volgende opdrachten voor het ophalen van beheerde schijfeigenschappen, een beheerde schijf koppelen met een nieuwe virtuele machine en een virtuele machine maken. Elk item in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmDisk](/powershell/module/azurerm.compute/Get-AzureRmDisk) | Schijfobject op basis van de naam en de resourcegroep van een schijf opgehaald. Id-eigenschap van het object geretourneerde schijf wordt gebruikt om de schijf koppelen aan een nieuwe virtuele machine |
| [Nieuwe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Maakt een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties VM. De configuratie wordt gebruikt tijdens het maken van VM. |
| [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) | Een beheerde schijf met behulp van de eigenschap Id van de schijf als de besturingssysteemschijf voor een nieuwe virtuele machine gekoppeld |
| [Nieuwe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres. |
| [Nieuwe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Maakt een netwerkinterface. |
| [Nieuwe-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Maak een virtuele machine. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle resources binnen. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra virtuele machine PowerShell-script kunnen worden gevonden in de [virtuele machine van Windows Azure-documentatie](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).