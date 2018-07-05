---
title: Windows virtuele machine maken met vereenvoudigde New-AzureRMVM-cmdlet in Azure Cloud Shell | Microsoft Docs
description: Leer snel hoe u Windows-machines maakt met de vereenvoudigde cmdlet New-AzureRMVM in Azure Cloud Shell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: ede8fab67c04eb7ce8d26280de2d1563b6cc8ad2
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435666"
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azurermvm-cmdlet-in-cloud-shell"></a>Een Windows-machine maken met de vereenvoudigde cmdlet New-AzureRMVM in Cloud Shell 

De [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) cmdlet een vereenvoudigde set parameters voor het maken van een nieuwe virtuele machine met behulp van PowerShell is toegevoegd. Dit onderwerp ziet u hoe u PowerShell in Azure Cloud Shell met de meest recente versie van de cmdlet New-AzureVM is vooraf geïnstalleerd, voor het maken van een nieuwe virtuele machine. We gebruiken een vereenvoudigde parameterset die automatisch alle benodigde bronnen met slimme standaardinstellingen worden gemaakt. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 5.1.1 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-the-vm"></a>De virtuele machine maken

U kunt de [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) cmdlet voor het maken van een virtuele machine met slimme standaardwaarden die gebruikmaken van de installatiekopie van het Windows Server 2016 Datacenter vanuit de Azure Marketplace. U kunt New-AzureRMVM met alleen de **-naam** parameter en wordt die waarde gebruiken voor alle resourcenamen van de. In dit voorbeeld wordt de parameter **-Name** ingesteld op *myVM*. 

Controleer of **PowerShell** in Cloud Shell is geselecteerd en typ:

```azurepowershell-interactive
New-AzureRMVm -Name myVM
```

U wordt gevraagd om een gebruikersnaam en wachtwoord voor de virtuele machine te maken. Deze worden later in dit onderwerp gebruikt wanneer u verbinding maakt met de virtuele machine. Het wachtwoord moet 12 tot 123 tekens lang zijn en aan drie van de vier volgende complexiteitsvereisten voldoen: ten minste één kleine letter, één hoofdletter, één cijfer en één speciaal teken.

Het duurt een minuut om de virtuele machine en de bijbehorende bronnen te maken. Wanneer u klaar bent, ziet u alle resources die zijn gemaakt met behulp van de [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) cmdlet.

```azurepowershell-interactive
Get-AzureRmResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u via een extern bureaublad verbinding met de virtuele machine.

Gebruik de opdracht [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van de virtuele machine te retourneren. Noteer dit IP-adres.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Open een opdrachtprompt op uw lokale computer en gebruik de **mstsc** opdracht uit om te beginnen een extern-bureaubladsessie met de nieuwe virtuele machine. Vervang &lt;publicIPAddress&gt; door het IP-adres van de virtuele machine. Wanneer dit wordt gevraagd, voert u de gebruikersnaam en het wachtwoord in dat u bij het maken van de virtuele machine hebt opgegeven.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Geef verschillende resourcenamen

U kunt ook bieden meer beschrijvende namen voor de resources en nog steeds ze automatisch gemaakt. Hier volgt een voorbeeld waarin we hebben met de naam meerdere resources voor de nieuwe virtuele machine, met inbegrip van een nieuwe resourcegroep.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVM
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit onderwerp hebt u een eenvoudige virtuele machine geïmplementeerd met behulp van New-AzVM en vervolgens verbinding gemaakt met de virtuele machine via RDP. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)
