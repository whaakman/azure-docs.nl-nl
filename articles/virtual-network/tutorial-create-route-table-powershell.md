---
title: Doorsturen van netwerkverkeer Azure PowerShell | Microsoft Docs
description: Informatie over het netwerkverkeer omgeleid met een routetabel met behulp van PowerShell in dit artikel.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f6f3bd2a9683daf5f523cc5cfe43e568fb508694
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Doorsturen van netwerkverkeer met een routetabel met behulp van PowerShell

Azure automatisch routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt uw eigen routes voor het overschrijven van Azure maken standaardroutering. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld het routeren van verkeer tussen subnetten via een netwerk (NVA) voor virtueel apparaat. In dit artikel leert u hoe:

* Een routetabel maken
* Een route maken
* Een virtueel netwerk maken met meerdere subnetten
* Een routetabel aan een subnet koppelen
* Maken van een NVA waarmee verkeer
* Virtuele machines (VM) implementeren in verschillende subnetten
* Verkeer leiden uit één subnet naar een andere door middel van een NVA

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="create-a-route-table"></a>Een routetabel maken

Voordat u een routetabel maken kunt, maakt u een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* voor alle resources in dit artikel is gemaakt. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Maken van een routetabel met [nieuw AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable). Het volgende voorbeeld wordt een routetabel met de naam *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzureRmRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Een route maken

Een route maken door op te halen van het object van de tabel route met [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), maakt u een route met [toevoegen AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig), noteert u de configuratie van de route aan de routetabel met [Set AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable). 

```azurepowershell-interactive
Get-AzureRmRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzureRmRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzureRmRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Voordat u een routetabel aan een subnet koppelt kunt, hebt u een virtueel netwerk en subnet maken. Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork* met het adresvoorvoegsel *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Drie subnetten gemaakt met behulp van drie subnetconfiguraties met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld wordt drie subnetconfiguraties voor *openbare*, *persoonlijke*, en *DMZ* subnetten:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

De subnetconfiguraties schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), waarvan de subnetten maakt in het virtuele netwerk:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Koppel de *myRouteTablePublic* routetabel aan de *openbare* subnet met [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) en schrijf vervolgens de subnetconfiguratie van de virtueel netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

## <a name="create-an-nva"></a>Maken van een NVA

Een NVA is een virtuele machine die een netwerkfunctie, zoals routering, gebruik of WAN-optimalisatie uitvoert.

Voordat u een virtuele machine maakt, maakt u een netwerkinterface.

### <a name="create-a-network-interface"></a>Een netwerkinterface maken

Voordat u een netwerkinterface maakt, dat u hebt voor het ophalen van het virtuele netwerk-Id met [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), klikt u vervolgens de Id van het subnet met [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Maken van een netwerkinterface met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) in de *DMZ* subnet met doorsturen via IP ingeschakeld:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzureRmVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>Een virtuele machine maken

Voor het maken van een virtuele machine en een bestaande netwerkinterface gekoppeld, moet u eerst een VM-configuratie met maken [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). De configuratie omvat de netwerkinterface die in de vorige stap hebt gemaakt. Als u wordt gevraagd om een gebruikersnaam en wachtwoord, selecteert u de gebruikersnaam en wachtwoord die u wilt zich aanmelden bij de virtuele machine met. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzureRmVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id
```

De virtuele machine maken met de VM-configuratie met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via het virtuele netwerk-apparaat in een later stadium. 

Maak een VM in de *openbare* subnet met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmPublic* in de *openbare* subnet van de *myVirtualNetwork* virtueel netwerk. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

Maak een VM in de *persoonlijke* subnet.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

De virtuele machine duurt een paar minuten maken. Niet verder gaan met de volgende stap voordat de virtuele machine wordt gemaakt en Azure PowerShell uitvoer weer.

## <a name="route-traffic-through-an-nva"></a>-Routeverkeer via een NVA

Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van de *myVmPrivate* VM. Het volgende voorbeeld wordt het openbare IP-adres van de *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een sessie voor extern bureaublad met de *myVmPrivate* VM van de lokale computer. Vervang `<publicIpAddress>` geretourneerd met het IP-adres van de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Open het gedownloade RDP-bestand. Als u wordt gevraagd, selecteert u **Connect**.

Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven), Selecteer vervolgens **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding. 

In een latere stap, de opdracht tracert.exe gebruikt voor het testen van routering. Tracert maakt gebruik van het Internet Control Message Protocol (ICMP), die via de Windows Firewall is geweigerd. ICMP inschakelen via de Windows firewall met de volgende opdracht vanuit PowerShell op de *myVmPrivate* VM:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

Hoewel de opdracht Traceroute wordt gebruikt voor het testen van routering in dit artikel, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.

U doorsturen via IP in Azure voor de netwerkinterface van de VM in ingeschakeld [Enable IP fowarding](#enable-ip-forwarding). In de VM moet het besturingssysteem of een toepassing die wordt uitgevoerd vanuit de virtuele machine, ook kunnen voor het doorsturen van netwerkverkeer. Inschakelen van doorsturen via IP in het besturingssysteem van de *myVmNva*.

Vanaf een opdrachtprompt op de *myVmPrivate* VM, extern bureaublad naar de *myVmNva*:

``` 
mstsc /v:myvmnva
```
    
Voer zodat doorsturen via IP in het besturingssysteem de volgende opdracht in PowerShell uit de *myVmNva* VM:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Start opnieuw op de *myVmNva* VM, die ook ontkoppeld van de extern bureaublad-sessiehost.

Tijdens het nog wordt verbonden met de *myVmPrivate* VM, een extern-bureaubladsessie met de *myVmPublic* VM na het *myVmNva* VM opnieuw wordt opgestart:

``` 
mstsc /v:myVmPublic
```
    
ICMP inschakelen via de Windows firewall met de volgende opdracht vanuit PowerShell op de *myVmPublic* VM:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Voor het testen van routering van netwerkverkeer naar de *myVmPrivate* VM van de *myVmPublic* virtuele machine, voer de volgende opdracht vanuit PowerShell op de *myVmPublic* VM:

```
tracert myVmPrivate
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:
    
```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```
      
U ziet dat de eerste hop 10.0.2.4 die de NVA privé IP-adres is. De tweede hop is 10.0.1.4, de persoonlijke IP-adres van de *myVmPrivate* VM. De route toegevoegd aan de *myRouteTablePublic* routetabel en die is gekoppeld aan de *openbare* subnet veroorzaakt Azure voor het routeren van het verkeer via de NVA in plaats van rechtstreeks naar de *persoonlijke* subnet.

Sluit de extern-bureaubladsessie naar de *myVmPublic* VM, zodat u nog steeds is verbonden met de *myVmPrivate* VM.

Voor het testen van routering van netwerkverkeer naar de *myVmPublic* VM van de *myVmPrivate* virtuele machine, voer de volgende opdracht vanaf een opdrachtprompt op de *myVmPrivate* VM:

```
tracert myVmPublic
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
   
Trace complete.
```

U kunt zien dat verkeer wordt gerouteerd rechtstreeks vanuit de *myVmPrivate* VM naar de *myVmPublic* VM. Standaard Azure routes verkeer rechtstreeks tussen subnetten.

Sluit de extern-bureaubladsessie naar de *myVmPrivate* VM.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources bevat.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een routetabel gemaakt en gekoppeld aan een subnet. U hebt gemaakt met een eenvoudig virtueel netwerkapparaat dat verkeer van een openbare subnet naar een persoonlijke subnet gerouteerd. Implementeren van tal van vooraf geconfigureerde virtuele netwerkapparaten die netwerkfuncties zoals firewall en WAN-optimalisatie van uitvoeren de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Zie voor meer informatie over routering, [Routering-overzicht](virtual-networks-udr-overview.md) en [beheren een routetabel](manage-route-table.md).

Terwijl u veel Azure-resources binnen een virtueel netwerk implementeren kunt, kunnen geen resources voor sommige Azure PaaS-services worden geïmplementeerd in een virtueel netwerk. U kunt nog steeds toegang beperken tot de bronnen van sommige services Azure PaaS verkeer alleen via een virtueel netwerksubnet al. Voor meer informatie Zie [beperken van toegang tot het netwerk voor PaaS-resources](tutorial-restrict-network-access-to-resources-powershell.md).