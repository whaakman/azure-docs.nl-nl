---
title: Windows virtuele machine maken met vereenvoudigde New-AzureRMVM cmdlet in de Azure-Cloud-Shell | Microsoft Docs
description: Snel informatie over Windows virtuele machines maken met de vereenvoudigde cmdlet New-AzureRMVM in de Azure-Cloud-Shell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 94eb6232cf59d502a9d70545785c3788398f4d27
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azurermvm-cmdlet-in-cloud-shell"></a>Een Windows-machine maken met de vereenvoudigde cmdlet New-AzureRMVM in de Cloud-Shell 

De [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) cmdlet een vereenvoudigde set parameters voor het maken van een nieuwe virtuele machine met behulp van PowerShell is toegevoegd. Dit onderwerp leest u hoe u PowerShell gebruikt in de Azure-Cloud-Shell met de meest recente versie van de cmdlet New-AzureVM is vooraf geïnstalleerd, voor het maken van een nieuwe virtuele machine. We gebruiken een vereenvoudigde parameterset die automatisch de benodigde resources met behulp van slimme standaardwaarden worden gemaakt. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, in deze zelfstudie vereist de Azure PowerShell moduleversie 5.1.1 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-the-vm"></a>De virtuele machine maken

U kunt de [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) cmdlet voor het maken van een virtuele machine met smart standaardwaarden met behulp van de installatiekopie van het Windows Server 2016 Datacenter vanuit Azure Marketplace. U kunt New-AzureRMVM met alleen de **-naam** parameter en wordt die waarde gebruiken voor alle resourcenamen van de. In dit voorbeeld wordt de parameter **-Name** ingesteld op *myVM*. 

Controleer of **PowerShell** in Cloud Shell is geselecteerd en typ:

```azurepowershell-interactive
New-AzureRMVm -Name myVM
```

U wordt gevraagd om een gebruikersnaam en wachtwoord voor de virtuele machine te maken. Deze worden later in dit onderwerp gebruikt wanneer u verbinding maakt met de virtuele machine. Het wachtwoord moet 12 tot 123 tekens lang zijn en aan drie van de vier volgende complexiteitsvereisten voldoen: ten minste één kleine letter, één hoofdletter, één cijfer en één speciaal teken.

Het duurt een minuut om de virtuele machine en de bijbehorende bronnen te maken. Wanneer u klaar bent, kunt u met de cmdlet [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) alle resources zien die zijn gemaakt.

```azurepowershell-interactive
Find-AzureRmResource `
    -ResourceGroupNameEquals myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u via een extern bureaublad verbinding met de virtuele machine.

Gebruik de opdracht [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van de virtuele machine te retourneren. Noteer dit IP-adres.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Op uw lokale computer een opdrachtprompt openen en gebruiken de **mstsc** opdracht een extern bureaublad-sessiehost starten met de nieuwe virtuele machine. Vervang &lt;publicIPAddress&gt; door het IP-adres van de virtuele machine. Wanneer dit wordt gevraagd, voert u de gebruikersnaam en het wachtwoord in dat u bij het maken van de virtuele machine hebt opgegeven.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Geef verschillende Bronnamen

U kunt ook beschrijvende naam voor de bronnen te bieden en nog steeds ze automatisch gemaakt. Hier volgt een voorbeeld waarin we hebben met de naam meerdere resources voor de nieuwe virtuele machine, met inbegrip van een nieuwe resourcegroep.

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
