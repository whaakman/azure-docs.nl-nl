---
title: Routeren van netwerkverkeer Azure PowerShell | Microsoft Docs
description: In dit artikel leert u hoe u netwerkverkeer te routeren met een routetabel met behulp van PowerShell.
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
ms.openlocfilehash: ad09d41b004fe2b8a4090dce16a7a70f9c57b1f3
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56651496"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Netwerkverkeer routeren met een routetabel met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure routeert standaard automatisch verkeer tussen alle subnetten in een virtueel netwerk. U kunt uw eigen routes maken om de standaardroutering van Azure te overschrijven. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld verkeer tussen subnetten wilt routeren via een NVA (virtueel netwerkapparaat). In dit artikel leert u het volgende:

* Een routetabel maken
* Een route maken
* Een virtueel netwerk met meerdere subnetten maken
* Een routetabel aan een subnet koppelen
* Een NVA maken voor het routeren van verkeer
* Virtuele machines (VM's) implementeren in verschillende subnetten
* Verkeer van het ene subnet naar het andere leiden via een NVA

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-route-table"></a>Een routetabel maken

Voordat u een routetabel maken kunt, maakt u een resourcegroep met [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* voor alle resources die in dit artikel is gemaakt.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Maak een routetabel met [New-AzRouteTable](/powershell/module/az.network/new-azroutetable). Het volgende voorbeeld wordt een routetabel met de naam *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Een route maken

Een route maken door op te halen van de route-table-object met [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable), een route maakt met [toevoegen AzRouteConfig](/powershell/module/az.network/add-azrouteconfig), noteert u de configuratie van de route aan de routetabel met [ Set-AzRouteTable](/powershell/module/az.network/set-azroutetable).

```azurepowershell-interactive
Get-AzRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Voordat u een routetabel aan een subnet kunt koppelen, moet u een virtueel netwerk en subnet maken. Maak een virtueel netwerk met [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork* met het adresvoorvoegsel *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Drie subnetten gemaakt met behulp van drie subnetconfiguraties met [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Het volgende voorbeeld maakt u drie subnetconfiguraties voor *openbare*, *persoonlijke*, en *DMZ* subnetten:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

De subnetconfiguraties schrijven naar het virtuele netwerk met [Set AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork), waarmee de subnetten wordt gemaakt in het virtuele netwerk:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

Koppelt de *myRouteTablePublic* routetabel aan de *openbare* subnet met de [Set AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) en schrijf de subnetconfiguratie naar de virtueel netwerk met [Set AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork).

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzVirtualNetwork
```

## <a name="create-an-nva"></a>Een NVA maken

Een NVA is een VM die een netwerkfunctie uitvoert, zoals routering, firewall of WAN-optimalisatie.

Voordat u een virtuele machine maakt, moet u een netwerkinterface maken.

### <a name="create-a-network-interface"></a>Een netwerkinterface maken

Voordat u een netwerkinterface, die u hebt om op te halen van het virtuele netwerk-Id met [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork), klikt u vervolgens de Id van het subnet met [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig). Maken van een netwerkinterface met [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) in de *DMZ* subnet met doorsturen via IP is ingeschakeld:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>Een virtuele machine maken

Een virtuele machine maken en koppelen van een bestaande netwerkinterface toe, moet u eerst een VM-configuratie met maken [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig). De configuratie omvat de netwerkinterface in de vorige stap hebt gemaakt. Wanneer u hierom wordt gevraagd een gebruikersnaam en wachtwoord, selecteert u de gebruikersnaam en wachtwoord die u wilt aanmelden bij de virtuele machine met.

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzVMNetworkInterface -Id $nic.Id
```

De virtuele machine maken met de VM-configuratie met [New-AzVM](/powershell/module/az.compute/new-azvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmNva*.

```azurepowershell-interactive
$vmNva = New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak twee VM's in het virtuele netwerk, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via het virtuele netwerkapparaat in een latere stap.

Maken van een virtuele machine in de *openbare* subnet met de [New-AzVM](/powershell/module/az.compute/new-azvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmPublic* in de *openbare* subnet van de *myVirtualNetwork* virtueel netwerk.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

Maken van een virtuele machine in de *persoonlijke* subnet.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

Het maken van de virtuele machine duurt een paar minuten. Niet verder met de volgende stap voordat de virtuele machine wordt gemaakt en Azure uitvoer geretourneerd naar PowerShell.

## <a name="route-traffic-through-an-nva"></a>Verkeer routeren via een NVA

Gebruik [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) om terug te keren van het openbare IP-adres van de *myVmPrivate* VM. Het volgende voorbeeld wordt het openbare IP-adres van de *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Gebruik de volgende opdracht om te maken van een extern-bureaubladsessie met de *myVmPrivate* VM vanaf uw lokale computer. Vervang `<publicIpAddress>` door het IP-adres dat is geretourneerd met de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd.

Voer de gebruikersnaam en het wachtwoord in die u bij het maken van de VM hebt opgegeven (mogelijk moet u **Meer opties** en **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de virtuele machine) en selecteer **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.

In een latere stap wordt de `tracert.exe` opdracht wordt gebruikt voor het testen van de routering. Tracert maakt gebruik van het Internet Control Message Protocol (ICMP), die door de Windows Firewall wordt geweigerd. Schakel ICMP via de Windows-firewall in met de volgende opdracht vanuit PowerShell op de VM *myVmPrivate*:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

Hoewel route traceren wordt gebruikt voor het testen van de routering in dit artikel, wordt het ICMP via de Windows Firewall voor productie-implementaties toe te staan niet aanbevolen.

U doorsturen via IP in Azure voor de netwerkinterface van de virtuele machine in doorsturen via IP inschakelen ingeschakeld. In de VM moet het besturingssysteem of een toepassing die wordt uitgevoerd op de virtuele machine, ook netwerkverkeer kunnen doorsturen. Schakel doorsturen via IP in het besturingssysteem van de *myVmNva*.

Vanaf een opdrachtprompt op de *myVmPrivate* VM, extern bureaublad naar de *myVmNva*:

``` 
mstsc /v:myvmnva
```

Om Doorsturen via IP binnen het besturingssysteem in te schakelen, voert u de volgende opdracht in PowerShell in vanaf de VM *myVmNva* :

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```

Start *myVmNva* opnieuw op. Hierdoor wordt ook de extern-bureaubladsessie verbroken.

Maak terwijl u nog verbinding hebt met de VM *myVmPrivate* een extern-bureaubladsessie met de VM *myVmPublic* nadat de VM *myVmNva* opnieuw is opgestart:

``` 
mstsc /v:myVmPublic
```

Schakel ICMP via de Windows-firewall in met de volgende opdracht vanuit PowerShell op de VM *myVmPublic*:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Om de routering van netwerkverkeer naar de VM *myVmPrivate* van de VM *myVmPublic* te testen, voert u op de VM *myVmPublic* de volgende opdracht van PowerShell uit:

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

U ziet dat de eerste hop 10.0.2.4 is, het privé IP-adres van het NVA. De tweede hop is 10.0.1.4, het privé- IP-adres van de VM *myVmPrivate*. Door de route die is toegevoegd aan de routetabel *myRouteTablePublic* en gekoppeld aan het *Openbare* subnet leidt Azure het verkeer via het NVA in plaats van rechtstreeks naar het *Privé*-subnet.

Sluit de externe bureaubladsessie met de VM *myVmPublic*. U houdt nog verbinding met de VM *myVmPrivate*.

Om de routering van netwerkverkeer naar de VM *myVmPublic* van de VM *myVmPrivate* VM te testen, voert u op de VM *myVmPrivate* de volgende opdracht uit vanaf een opdrachtprompt:

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

U ziet dat verkeer rechtstreeks vanuit *myVmPrivate* naar *myVmPublic* wordt geleid. Standaard routeert Azure verkeer rechtstreeks tussen subnetten.

Sluit de externe bureaubladsessie naar de VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, gebruikt u [Remove-AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle resources die deze bevat te verwijderen.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een routetabel gemaakt en die aan een subnet gekoppeld. U hebt gemaakt met een eenvoudig virtueel netwerkapparaat dat verkeer van een openbaar subnet naar een privé-subnet gerouteerd. Implementeren van tal van vooraf geconfigureerde virtuele netwerkapparaten die netwerkfuncties zoals firewall- en WAN-optimalisatie van uitvoeren de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Zie [Routeringoverzicht](virtual-networks-udr-overview.md) en [Routetabel beheren](manage-route-table.md) voor meer informatie over routeren.

Hoewel u veel Azure-resources binnen een virtueel netwerk kunt implementeren, kunnen resources voor sommige Azure PaaS-diensten niet in een virtueel netwerk worden geïmplementeerd. U kunt de toegang tot de resources van sommige Azure PaaS-diensten echter nog steeds beperken tot alleen verkeer vanaf een subnet van een virtueel netwerk. Voor meer informatie Zie [netwerktoegang tot PaaS-resources beperken](tutorial-restrict-network-access-to-resources-powershell.md).