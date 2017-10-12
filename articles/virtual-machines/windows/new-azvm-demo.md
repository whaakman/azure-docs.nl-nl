---
title: Virtuele Windows-machines maken met de cmdlet New-AzVM in de Azure Cloud Shell | Microsoft Docs
description: Leer snel hoe u virtuele Windows-machines maakt met de cmdlet New-AzVM in Azure Cloud Shell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/21/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 3be46c8c02ad136edb1936fbb39560d479b27277
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-new-azvm-preview-in-cloud-shell"></a>Een virtuele Windows-machine maken met de cmdlet New-AzVM (preview) in de Cloud Shell 

De cmdlet New-AzVM (preview) is een eenvoudigere manier om een nieuwe virtuele machine te maken met behulp van PowerShell. In deze handleiding wordt uitgelegd hoe u PowerShell gebruikt in Azure Cloud Shell waarin vooraf de cmdlet New-AzVM is geïnstalleerd, voor het maken van een nieuwe virtuele Azure-machine waarop Windows Server 2016 wordt uitgevoerd. Wanneer de implementatie is voltooid, wordt er met behulp van RDP verbinding gemaakt met de server.  

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

## <a name="create-the-vm"></a>De virtuele machine maken

U kunt de cmdlet **New-AzVM** gebruiken om een virtuele machine te maken met slimme standaardwaarden die gebruikmaken van de installatiekopie van het Windows Server 2016-datacenter vanuit de Azure Marketplace. U kunt New-AzVM ook afzonderlijk gebruiken. De cmdlet gebruikt dan standaardwaarden voor de resourcenamen. In dit voorbeeld wordt de parameter **-Name** ingesteld op *myVM*. De cmdlet maakt de vereiste resources met *myVM* als het voorvoegsel voor de resourcenaam. 

Controleer of **PowerShell** in Cloud Shell is geselecteerd en typ:

```azurepowershell-interactive
New-AzVm -Name myVM
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

Open op uw lokale computer een opdrachtprompt en gebruik de opdracht **mstsc** om een extern-bureaubladsessie te starten met de nieuwe virtuele machine. Vervang &lt;publicIPAddress&gt; door het IP-adres van de virtuele machine. Wanneer dit wordt gevraagd, voert u de gebruikersnaam en het wachtwoord in dat u bij het maken van de virtuele machine hebt opgegeven.

```
mstsc /v:<publicIpAddress>
```

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit onderwerp hebt u een eenvoudige virtuele machine geïmplementeerd met behulp van New-AzVM en vervolgens verbinding gemaakt met de virtuele machine via RDP. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)
