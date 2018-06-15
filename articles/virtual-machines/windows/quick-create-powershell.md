---
title: 'Snelstart: Een virtuele Windows-machine maken met Azure PowerShell | Microsoft Docs'
description: In deze snelstart leert u hoe u Azure PowerShell gebruikt om een virtuele Windows-machine te maken
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 74b4a593e46fe7f4650306d90a65e287a6d48206
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187171"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-azure-with-powershell"></a>Snelstart: Een virtuele Linux-machine maken in Azure met PowerShell

De Azure PowerShell-module wordt gebruikt voor het maken en beheren van Azure-resources vanaf de PowerShell-opdrachtregel of in scripts. In deze snelstart wordt beschreven hoe u de Azure PowerShell-module gebruikt voor het implementeren van een virtuele machine (VM) in Azure waarop Windows Server 2016 wordt uitgevoerd. Om uw virtuele machine in actie te zien, voert u een externe bureaubladsessie uit voor de virtuele machine uit en installeert u de IIS-webserver.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een VM met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Geef de namen op van elke resource. De cmdlet [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) maakt de resources vervolgens (als ze nog niet bestaan).

Wanneer u hierom wordt gevraagd, geeft u een gebruikersnaam en wachtwoord op dat moet worden gebruikt als de aanmeldingsreferenties voor de VM:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389
```

## <a name="connect-to-virtual-machine"></a>Verbinding maken met de virtuele machine

Nadat de implementatie is voltooid, opent u een externe bureaubladsessie met de virtuele machine. Om uw virtuele machine in actie te zien, wordt vervolgens de IIS-webserver geïnstalleerd.

Gebruik de cmdlet [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van de virtuele machine te bekijken:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select "IpAddress"
```

Gebruik de volgende opdracht om een sessie met een extern bureaublad te starten vanaf uw lokale computer. Vervang het IP-adres door het openbare IP-adres van de virtuele machine. Wanneer u hierom wordt gevraagd, voert u de referenties in die zijn gebruikt toen de VM is gemaakt:

```powershell
mstsc /v:publicIpAddress
```

## <a name="install-web-server"></a>Webserver installeren

Als u uw virtuele machine in actie wilt zien, installeert u de IIS-webserver. Open een PowerShell-prompt op de virtuele machine en voer de volgende opdracht uit:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Wanneer u klaar bent, sluit u de externe-bureaubladverbinding met de virtuele machine.

## <a name="view-the-web-server-in-action"></a>De webserver in actie zien

Nu IIS is geïnstalleerd en poort 80 op de virtuele machine is geopend voor toegang vanaf internet, kunt u een webbrowser van uw keuze gebruiken om de standaardwelkomstpagina van IIS weer te geven. Gebruik het openbare IP-adres van uw virtuele machine dat is verkregen in een vorige stap. In het volgende voorbeeld ziet u de IIS-standaardwebsite:

![Standaardsite van IIS](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de cmdlet [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige virtuele machine geïmplementeerd, een netwerkpoort geopend voor internetverkeer en een eenvoudige webserver geïnstalleerd. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](./tutorial-manage-vm.md)
