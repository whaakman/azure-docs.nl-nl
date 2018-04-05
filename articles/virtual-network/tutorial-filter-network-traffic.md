---
title: Filteren van netwerkverkeer - Azure PowerShell | Microsoft Docs
description: In dit artikel leert u hoe voor het filteren van netwerkverkeer naar een subnet, met een netwerkbeveiligingsgroep met behulp van PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 53406150cfc2ec4229ecd9cf3356ad03d60f8e7e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="filter-network-traffic-with-a-network-security-group-using-powershell"></a>Filteren van netwerkverkeer met een netwerkbeveiligingsgroep met behulp van PowerShell

U kunt het netwerkverkeer naar binnenkomende en uitgaande van een virtueel netwerksubnet filteren met een netwerkbeveiligingsgroep. Netwerkbeveiligingsgroepen bevatten beveiligingsregels voor verbindingen die filteren van netwerkverkeer door IP-adres, poort en protocol. Beveiligingsregels worden toegepast op resources geïmplementeerd in een subnet. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een netwerkbeveiliging groep en beveiliging regels maken
> * Een virtueel netwerk maken en koppelen van een netwerkbeveiligingsgroep aan een subnet
> * Virtuele machines (VM) implementeren in een subnet
> * De verkeersfilters van de test

Als u liever, kunt u voltooien in dit artikel met behulp van de [Azure CLI](tutorial-filter-network-traffic-cli.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell moduleversie 5.4.1 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Een netwerkbeveiligingsgroep bevat beveiligingsregels voor verbindingen. Beveiligingsregels Geef een bron en bestemming. Bronnen en bestemmingen kunnen beveiligingsgroepen van toepassing zijn.

### <a name="create-application-security-groups"></a>Beveiligingsgroepen toepassing maken

Eerst maakt u een resourcegroep voor alle resources die zijn gemaakt in dit artikel met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld maakt u een resourcegroep in de *eastus* locatie: 


```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Maak de beveiligingsgroep van een toepassing met [nieuw AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup). De beveiligingsgroep van een toepassing kunt u groepsservers met vergelijkbaar poortfiltering vereisten. Het volgende voorbeeld maakt twee beveiligingsgroepen van toepassing.

```azurepowershell-interactive
$webAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgWebServers `
  -Location eastus

$mgmtAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgMgmtServers `
  -Location eastus
```

### <a name="create-security-rules"></a>Beveiligingsregels voor verbindingen maken

Maken van een regel met [nieuw AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Het volgende voorbeeld wordt een regel waarmee verkeer inkomend verkeer van internet naar de *myWebServers* toepassing beveiligingsgroep poorten 80 en 443:

```azurepowershell-interactive
$webRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-Web-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $webAsg.id `
  -DestinationPortRange 80,443

The following example creates a rule that allows traffic inbound from the internet to the *myMgmtServers* application security group over port 3389:

$mgmtRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-RDP-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 110 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $mgmtAsg.id `
  -DestinationPortRange 3389
```

In dit artikel RDP (poort 3389) wordt blootgesteld aan internet voor de *myAsgMgmtServers* VM. Voor productieomgevingen, in plaats van het blootstellen van poort 3389 voor internet, wordt aanbevolen dat u verbinding maken met Azure-resources die u wilt beheren met behulp van een [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [persoonlijke](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) netwerkverbinding.

### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maken van een netwerkbeveiligingsgroep met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNsg*: 

```powershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myNsg `
  -SecurityRules $webRule,$mgmtRule
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtuele naam *myVirtualNetwork*:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Maken van een subnetconfiguratie met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig), en vervolgens de configuratie van het subnet te schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). Het volgende voorbeeld wordt een subnet met de naam *mySubnet* met het virtuele netwerk en gekoppeld de *myNsg* netwerkbeveiligingsgroep aan:

```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -VirtualNetwork $virtualNetwork `
  -AddressPrefix "10.0.2.0/24" `
  -NetworkSecurityGroup $nsg
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Voordat u de virtuele machines, ophalen van het object van het virtuele netwerk met het subnet met [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell-interactive
$virtualNetwork = Get-AzureRmVirtualNetwork `
 -Name myVirtualNetwork `
 -Resourcegroupname myResourceGroup
```
Een openbaar IP-adres maken voor elke virtuele machine met [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

$publicIpWeb = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic ` - ResourceGroupName myResourceGroup `
  -Location eastus ` -naam myVmWeb

$publicIpMgmt = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic ` - ResourceGroupName myResourceGroup `
  -Location eastus ` -naam myVmMgmt


Maak twee netwerkinterfaces met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface), en een openbare IP-adres toewijzen aan de netwerkinterface. Het volgende voorbeeld wordt een netwerkinterface, gekoppeld de *myVmWeb* openbaar IP-adres, en maakt het een lid van de *myAsgWebServers* toepassing beveiligingsgroep:

```powershell-interactive
$webNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $webAsg.Id `
  -PublicIpAddressId $publicIpWeb.Id
```

Het volgende voorbeeld wordt een netwerkinterface, gekoppeld de *myVmMgmt* openbaar IP-adres, en maakt het een lid van de *myAsgMgmtServers* toepassing beveiligingsgroep:

```powershell-interactive
$mgmtNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $mgmtAsg.Id `
  -PublicIpAddressId $publicIpMgmt.Id
```

Twee virtuele machines maken in het virtuele netwerk, zodat u kunt controleren of een latere stap van het filteren van verkeer. 

Maak een VM-configuratie met [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), maakt u de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine die als een webserver fungeert. De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap: 

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$webVmConfig = New-AzureRmVMConfig `
  -VMName myVmWeb `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmWeb `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $webNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $webVmConfig `
  -AsJob
```

Maak een VM moeten fungeren als een beheerserver:

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create the web server virtual machine configuration and virtual machine.
$mgmtVmConfig = New-AzureRmVMConfig `
  -VMName myVmMgmt `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmMgmt `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $mgmtNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $mgmtVmConfig
```

De virtuele machine duurt een paar minuten maken. Niet verder gaan met de volgende stap voordat Azure is gemaakt van de virtuele machine.

## <a name="test-traffic-filters"></a>De verkeersfilters van de test

Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van een virtuele machine. Het volgende voorbeeld wordt het openbare IP-adres van de *myVmMgmt* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een sessie voor extern bureaublad met de *myVmMgmt* VM van de lokale computer. Vervang `<publicIpAddress>` geretourneerd met het IP-adres van de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Open het gedownloade RDP-bestand. Als u wordt gevraagd, selecteert u **Connect**.

Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven), Selecteer vervolgens **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding. 
   
De verbinding slaagt, omdat poort 3389 is toegestaan inkomende van internet naar de *myAsgMgmtServers* toepassing beveiligingsgroep die de netwerkinterface is gekoppeld aan de *myVmMgmt* VM bevindt zich in.

Gebruik de volgende opdracht voor het maken van een verbinding met extern bureaublad naar de *myVmWeb* VM, uit de *myVmMgmt* VM, met de volgende opdracht, vanuit PowerShell:

``` 
mstsc /v:myvmWeb
```

De verbinding slaagt omdat een standaardbeveiligingsregel binnen elke netwerkbeveiligingsgroep verkeer via alle poorten tussen alle IP-adressen binnen een virtueel netwerk is toegestaan. U kan niet naar een extern bureaublad-verbinding maken de *myVmWeb* VM vanaf het internet omdat de beveiliging voor regel de *myAsgWebServers* mag geen poort 3389 inkomend van internet.

Gebruik de volgende opdracht voor het installeren van Microsoft IIS op de *myVmWeb* VM vanuit PowerShell:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Nadat de installatie van IIS voltooid is, verbreekt u de *myVmWeb* VM, waarbij u in de *myVmMgmt* VM-verbinding met extern bureaublad. Weergeven van het IIS-welkomstscherm, open een internetbrowser en blader naar http://myVmWeb.

Verbreek de verbinding tussen de *myVmMgmt* VM.

Voer op de computer de volgende opdracht vanuit PowerShell voor het ophalen van het openbare IP-adres van de *myVmWeb* server:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Om te bevestigen dat u toegang hebt tot de *myVmWeb* webserver buiten Azure, opent u een internet-browser op uw computer en blader naar `http://<public-ip-address-from-previous-step>`. De verbinding slaagt, omdat poort 80 is toegestaan inkomende van internet naar de *myAsgWebServers* toepassing beveiligingsgroep die de netwerkinterface is gekoppeld aan de *myVmWeb* VM bevindt zich in.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources die deze bevat:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een netwerkbeveiligingsgroep gemaakt en die aan een virtueel netwerksubnet zijn gekoppeld. Zie voor meer informatie over netwerkbeveiligingsgroepen, [netwerk groep beveiligingsoverzicht](security-overview.md) en [beheren van een netwerkbeveiligingsgroep](virtual-network-manage-nsg-arm-ps.md).

Azure routes verkeer tussen subnetten standaard. In plaats daarvan kunt u verkeer leiden tussen subnetten door middel van een virtuele machine, fungeren als een firewall, bijvoorbeeld. Voor meer informatie over het maken van een routetabel, Ga naar het volgende artikel.

> [!div class="nextstepaction"]
> [Een routetabel maken](./tutorial-create-route-table-portal.md)