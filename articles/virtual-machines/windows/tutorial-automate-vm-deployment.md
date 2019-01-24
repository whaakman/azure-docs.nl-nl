---
title: 'Zelfstudie: Toepassingen installeren op een virtuele Windows-machine in Azure | Microsoft Docs'
description: In deze zelfstudie leert u hoe u de aangepaste scriptextensie gebruikt om scripts uit te voeren scripts en toepassingen te implementeren op virtuele Windows-machines in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: zr-msft
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: 605649992cad988b2630034c7fdb8df1dc0fd5f5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435393"
---
# <a name="tutorial---deploy-applications-to-a-windows-virtual-machine-in-azure-with-the-custom-script-extension"></a>Zelfstudie: Toepassingen implementeren op een virtuele Windows-machine in Azure met de aangepaste scriptextensie

Als u virtuele machines (VM's) op een snelle en consistente manier wilt configureren, is een vorm van automatisering doorgaans gewenst. Een veelgebruikte manier om een virtuele Windows-machine aan te passen, is door een [aangepaste scriptextensie voor Windows](extensions-customscript.md) te gebruiken. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De aangepaste scriptextensie gebruiken om IIS te installeren
> * Een virtuele machine maken die de aangepaste scriptextensie gebruikt
> * Een actieve IIS-site weergeven nadat de extensie is toegepast

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.


## <a name="custom-script-extension-overview"></a>Overzicht aangepaste scriptextensie
Met de aangepaste scriptextensie kunnen scripts worden gedownload en uitgevoerd op virtuele machines in Azure. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies.

De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met Azure CLI, PowerShell, Azure Portal of de REST API van Azure Virtual Machine.

U kunt de aangepaste scriptextensie gebruiken met virtuele Windows- en Linux-machines.


## <a name="create-virtual-machine"></a>Virtuele machine maken
Stel eerst een beheerdersnaam en -wachtwoord in voor de virtuele machine met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

U kunt de virtuele machine nu maken met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt op de locatie *VS Oost*. Als deze niet al bestaan, worden de resourcegroep *myResourceGroupAutomate* en ondersteunende netwerkbronnen gemaakt. Om webverkeer mogelijk te maken, opent de cmdlet ook poort *80*.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Credential $cred
```

Het duurt enkele minuten voordat de bronnen en virtuele machine zijn gemaakt.


## <a name="automate-iis-install"></a>IIS-installatie automatiseren
Gebruik [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) om de aangepaste scriptextensie te installeren. De extensie voert `powershell Add-WindowsFeature Web-Server` uit om de IIS-webserver te installeren en werkt vervolgens de pagina *Default.htm* bij om de hostnaam van de VM weer te geven:

```azurepowershell-interactive
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroupAutomate" `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location "EastUS" `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
```


## <a name="test-web-site"></a>Website testen
Haal het openbare IP-adres van uw load balancer op met [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). In het volgende voorbeeld wordt het IP-adres opgehaald voor het eerder gemaakte *myPublicIPAddress*:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myPublicIPAddress" | select IpAddress
```

Vervolgens kunt u het openbare IP-adres invoeren in een webbrowser. De website wordt weergegeven met de hostnaam van de VM waarnaar de load balancer verkeer heeft gedistribueerd, zoals in het volgende voorbeeld:

![Actieve IIS-website](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u de IIS-installatie op een virtuele machine geautomatiseerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * De aangepaste scriptextensie gebruiken om IIS te installeren
> * Een virtuele machine maken die de aangepaste scriptextensie gebruikt
> * Een actieve IIS-site weergeven nadat de extensie is toegepast

Ga door naar de volgende zelfstudie voor informatie over het maken van aangepaste VM-installatiekopieën.

> [!div class="nextstepaction"]
> [Aangepaste VM-installatiekopieën maken](./tutorial-custom-images.md)
