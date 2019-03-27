---
title: Windows virtuele machine maken met vereenvoudigde cmdlet van New-AzVM in Azure Cloud Shell | Microsoft Docs
description: Leer snel hoe u Windows-machines maakt met de vereenvoudigde cmdlet New-AzVM in Azure Cloud Shell.
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
ms.openlocfilehash: 02f2bd78ca5656534b106c6f7f18c05165b4b9ff
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58444503"
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azvm-cmdlet-in-cloud-shell"></a>Een Windows-machine maken met de vereenvoudigde cmdlet New-AzVM in Cloud Shell 

De [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet een vereenvoudigde set parameters voor het maken van een nieuwe virtuele machine met behulp van PowerShell is toegevoegd. Dit onderwerp ziet u hoe u PowerShell in Azure Cloud Shell met de meest recente versie van de cmdlet New-AzureVM is vooraf geïnstalleerd, voor het maken van een nieuwe virtuele machine. We gebruiken een vereenvoudigde parameterset die automatisch alle benodigde bronnen met slimme standaardinstellingen worden gemaakt. 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]


## <a name="create-the-vm"></a>De virtuele machine maken

U kunt de cmdlet [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) gebruiken om een virtuele machine te maken met slimme standaardwaarden die gebruikmaken van de installatiekopie van het Windows Server 2016-datacenter vanuit de Azure Marketplace. U kunt New-AzVM met alleen de **-naam** parameter en wordt die waarde gebruiken voor alle resourcenamen van de. In dit voorbeeld wordt de parameter **-Name** ingesteld op *myVM*. 

Controleer of **PowerShell** in Cloud Shell is geselecteerd en typ:

```azurepowershell-interactive
New-AzVm -Name myVM
```

U wordt gevraagd om een gebruikersnaam en wachtwoord voor de virtuele machine te maken. Deze worden later in dit onderwerp gebruikt wanneer u verbinding maakt met de virtuele machine. Het wachtwoord moet 12 tot 123 tekens lang zijn en aan drie van de vier volgende complexiteitsvereisten voldoen: ten minste één kleine letter, één hoofdletter, één cijfer en één speciaal teken.

Het duurt een minuut om de virtuele machine en de bijbehorende bronnen te maken. Wanneer u klaar bent, ziet u alle resources die zijn gemaakt met behulp van de [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) cmdlet.

```azurepowershell-interactive
Get-AzResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

Wanneer de implementatie is voltooid, maakt u via een extern bureaublad verbinding met de virtuele machine.

Gebruik de [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) opdracht om het openbare IP-adres van de virtuele machine. Noteer dit IP-adres.

```azurepowershell-interactive
Get-AzPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Open een opdrachtprompt op uw lokale computer en gebruik de **mstsc** opdracht uit om te beginnen een extern-bureaubladsessie met de nieuwe virtuele machine. Vervang &lt;publicIPAddress&gt; door het IP-adres van de virtuele machine. Wanneer dit wordt gevraagd, voert u de gebruikersnaam en het wachtwoord in dat u bij het maken van de virtuele machine hebt opgegeven.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Geef verschillende resourcenamen

U kunt ook bieden meer beschrijvende namen voor de resources en nog steeds ze automatisch gemaakt. Hier volgt een voorbeeld waarin we hebben met de naam meerdere resources voor de nieuwe virtuele machine, met inbegrip van een nieuwe resourcegroep.

```azurepowershell-interactive
New-AzVm `
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

U kunt de opdracht [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myVM
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit onderwerp hebt u een eenvoudige virtuele machine geïmplementeerd met behulp van New-AzVM en vervolgens verbinding gemaakt met de virtuele machine via RDP. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)
