---
title: 'Zelfstudie: Toepassingen installeren op een virtuele Windows-machine in Azure | Microsoft Docs'
description: In deze zelfstudie leert u hoe u de aangepaste scriptextensie gebruikt om scripts uit te voeren scripts en toepassingen te implementeren op virtuele Windows-machines in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/29/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 72489c6d0b03166fcb2a5f1ed39f7b8d5408ff24
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708170"
---
# <a name="tutorial---deploy-applications-to-a-windows-virtual-machine-in-azure-with-the-custom-script-extension"></a>Zelfstudie: Toepassingen implementeren op een virtuele Windows-machine in Azure met de aangepaste scriptextensie

Om virtuele machines (VM's) snel en consistent te configureren, kunt u de [Aangepaste scriptextensie voor Windows](extensions-customscript.md) gebruiken. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De aangepaste scriptextensie gebruiken om IIS te installeren
> * Een virtuele machine maken die de aangepaste scriptextensie gebruikt
> * Een actieve IIS-site weergeven nadat de extensie is toegepast

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. 

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/powershell](https://shell.azure.com/powershell) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

## <a name="custom-script-extension-overview"></a>Overzicht aangepaste scriptextensie
Met de aangepaste scriptextensie kunnen scripts worden gedownload en uitgevoerd op virtuele machines in Azure. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies.

De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met Azure CLI, PowerShell, Azure Portal of de REST API van Azure Virtual Machine.

U kunt de aangepaste scriptextensie gebruiken met virtuele Windows- en Linux-machines.


## <a name="create-virtual-machine"></a>Virtuele machine maken
Stel naam en wachtwoord van de beheerder in voor de VM met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

U kunt de virtuele machine nu maken met [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt op de locatie *VS Oost*. Als deze niet al bestaan, worden de resourcegroep *myResourceGroupAutomate* en ondersteunende netwerkbronnen gemaakt. Om webverkeer mogelijk te maken, opent de cmdlet ook poort *80*.

```azurepowershell-interactive
New-AzVm `
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
Gebruik [Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) om de aangepaste scriptextensie te installeren. De extensie voert `powershell Add-WindowsFeature Web-Server` uit om de IIS-webserver te installeren en werkt vervolgens de pagina *Default.htm* bij om de hostnaam van de VM weer te geven:

```azurepowershell-interactive
Set-AzVMExtension -ResourceGroupName "myResourceGroupAutomate" `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location "EastUS" `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
```


## <a name="test-web-site"></a>Website testen
Haal het openbare IP-adres van uw load balancer op met [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress). In het volgende voorbeeld wordt het IP-adres opgehaald voor het eerder gemaakte *myPublicIPAddress*:

```azurepowershell-interactive
Get-AzPublicIPAddress `
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
