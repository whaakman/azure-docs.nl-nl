---
title: Voorbeeld van Azure PowerShell-script - Een VM maken door een beheerde schijf te koppelen als besturingssysteemschijf | Microsoft Docs
description: Voorbeeld van Azure PowerShell-script - Een VM maken door een beheerde schijf te koppelen als besturingssysteemschijf
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 519eccdd1d1b4628f41e12616d3e82014a2d301b
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "41918773"
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a>Een virtuele machine maken met behulp van een bestaande beheerde besturingssysteemschijf met PowerShell

Met dit script maakt u een virtuele machine door een bestaande beheerde schijf als besturingssysteemschijf te koppelen. Gebruik dit script in de volgende scenario's:
* Een virtuele machine maken van een bestaande beheerde besturingssysteemschijf die is gekopieerd van een beheerde schijf in een ander abonnement
* Een virtuele machine maken van een bestaande beheerde schijf die is gemaakt met behulp van een speciaal VHD-bestand 
* Een virtuele machine maken van een bestaande beheerde schijf die is gemaakt aan de hand van een momentopname 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het opvragen van de eigenschappen van de beheerde schijf, het koppelen van een beheerde schijf aan een nieuwe virtuele machine en het maken van een virtuele machine. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmDisk](/powershell/module/azurerm.compute/Get-AzureRmDisk) | Hiermee haalt u een schijfobject op dat is gebaseerd op de naam en de resourcegroep van een schijf. De eigenschap Id van het geretourneerde schijfobject wordt gebruikt om de schijf te koppelen aan een nieuwe VM |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Hiermee maakt u een VM-configuratie. Deze configuratie bevat informatie zoals de naam, het besturingssysteem en de beheerdersreferenties van de virtuele machine. De configuratie wordt gebruikt tijdens het maken van de virtuele machine. |
| [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) | Hiermee wordt een beheerde schijf met behulp van de eigenschap Id van de schijf gekoppeld als besturingssysteemschijf aan een nieuwe virtuele machine |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Hiermee maakt u een openbaar IP-adres. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Hiermee maakt u een netwerkinterface. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hiermee maakt u een virtuele machine. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Hiermee verwijdert u een resourcegroep en alle daarin opgenomen resources. |

Gebruik voor marketplace-installatiekopieën [Set AzureRmVMPlan](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmplan?view=azurermps-6.7.0) om de plangegevens in te stellen
```powershell
Set-AzureRmVMPlan -VM $VirtualMachine -Publisher $Publisher -Product $Product -Name $Bame
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

U kunt extra PowerShell-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor Windows-VM's](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
