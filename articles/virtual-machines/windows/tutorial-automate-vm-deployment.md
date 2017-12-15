---
title: Aanpassen van een Windows virtuele machine in Azure | Microsoft Docs
description: Informatie over het gebruik van de extensie voor aangepaste scripts en Sleutelkluis om aan te passen Windows virtuele machines in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 17a6c243aaf73fcd88261870fbdd9e8c936471b8
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a>Het aanpassen van een virtuele Windows-machine in Azure
Als u wilt configureren (virtuele machines) in een snelle en consistente manier, is een vorm van automatisering doorgaans gewenst. Een gemeenschappelijke aanpak voor het aanpassen van een virtuele machine van Windows is met [aangepast Script uitbreiding voor Windows](extensions-customscript.md). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De extensie voor aangepaste scripts gebruiken voor het installeren van IIS
> * Een virtuele machine die gebruikmaakt van de aangepaste Scriptextensie maken
> * Een actieve IIS-site weergeven nadat de uitbreiding is toegepast

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 


## <a name="custom-script-extension-overview"></a>Overzicht van de extensie aangepast script
De aangepaste Scriptextensie downloads en scripts worden uitgevoerd op Azure Virtual machines. Deze uitbreiding is nuttig voor post-implementatieconfiguratie, software-installatie of een andere configuratie / beheertaak. Scripts kunnen worden gedownload van Azure storage of GitHub, of naar de Azure portal op extensie uitvoeringstijd.

De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met de Azure CLI, PowerShell, Azure-portal of de REST-API van Azure virtuele Machine.

U kunt de aangepaste Scriptextensie gebruiken met Windows- en Linux-machines.


## <a name="create-virtual-machine"></a>Virtuele machine maken
Voordat u een virtuele machine maken kunt, maakt u een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupAutomate* in de *EastUS* locatie:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Stel dat een beheerder gebruikersnaam en wachtwoord voor de virtuele machines met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Nu kunt u de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld maakt de vereiste virtuele netwerkonderdelen, de configuratie van het besturingssysteem, en maakt vervolgens een virtuele machine met de naam *myVM*:

```azurepowershell-interactive
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine using the configuration
New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

Het duurt enkele minuten duren voordat de resources en de VM moet worden gemaakt.


## <a name="automate-iis-install"></a>IIS-installatie automatiseren
Gebruik [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) voor het installeren van de aangepaste Scriptextensie. De extensie wordt uitgevoerd `powershell Add-WindowsFeature Web-Server` voor het installeren van de IIS-webserver en updates van de *Default.htm* pagina voor het weergeven van de hostnaam van de virtuele machine:

```azurepowershell-interactive
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a>Test-website
Het openbare IP-adres van de load balancer met [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Het volgende voorbeeld verkrijgt het IP-adres voor *myPublicIP* eerder hebt gemaakt:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Vervolgens kunt u het openbare IP-adres in invoeren aan een webbrowser. De website wordt weergegeven, inclusief de hostnaam van de virtuele machine die de load balancer verkeer naar het volgende voorbeeld gedistribueerde:

![Actieve IIS-website](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt automatisch u de IIS-installatie op een virtuele machine. U hebt geleerd hoe u:

> [!div class="checklist"]
> * De extensie voor aangepaste scripts gebruiken voor het installeren van IIS
> * Een virtuele machine die gebruikmaakt van de aangepaste Scriptextensie maken
> * Een actieve IIS-site weergeven nadat de uitbreiding is toegepast

Ga naar de volgende zelfstudie voor informatie over het maken van aangepaste installatiekopieën van virtuele machine.

> [!div class="nextstepaction"]
> [Aangepaste VM-installatiekopieën maken](./tutorial-custom-images.md)
