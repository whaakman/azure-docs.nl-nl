---
title: Doorsturen van netwerkverkeer - PowerShell | Microsoft Docs
description: Informatie over het netwerkverkeer omgeleid met een routetabel met behulp van PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f91b143c75a82aa6760796770b3ae4d0e4ec53dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Doorsturen van netwerkverkeer met een routetabel met behulp van PowerShell

Azure automatisch routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt uw eigen routes voor het overschrijven van Azure maken standaardroutering. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld het routeren van verkeer tussen subnetten door een firewall. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een routetabel maken
> * Een route maken
> * Een routetabel aan het subnet van een virtueel netwerk koppelen
> * Test routering
> * Routering oplossen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="create-a-route-table"></a>Een routetabel maken

Azure routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. Zie voor meer informatie over Azure standaardroutes, [systeemroutes](virtual-networks-udr-overview.md). Als u wilt overschrijven Azure standaard routering, een routetabel met routes maken en koppelen van de routetabel een subnet van het virtuele netwerk.

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

Een routetabel bevat nul of meer routes. Een route maken door op te halen van het object van de tabel route met [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), maakt u een route met [toevoegen AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig), noteert u de configuratie van de route aan de routetabel met [Set AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable). 

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

De route wordt al het verkeer dat is bestemd voor het adresvoorvoegsel 10.0.1.0/24 via een virtueel netwerk-apparaat met het IP-adres 10.0.2.4 doorverwezen. De virtuele netwerkapparaat en het subnet met het opgegeven adres-voorvoegsel worden in latere stappen gemaakt. De route overschrijft van Azure-standaard routering, die verkeer tussen subnetten rechtstreeks gerouteerd. Elke route geeft een volgend hoptype. Het volgende hoptype geïnstrueerd Azure hoe het verkeer te routeren. In dit voorbeeld wordt het volgende hoptype is *VirtualAppliance*. Zie voor meer informatie over alle beschikbare hoptypen in de volgende in Azure, en wanneer u ze gebruikt, [volgende hop typen](virtual-networks-udr-overview.md#custom-routes).

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

De adresvoorvoegsels moet binnen het adresvoorvoegsel gedefinieerd voor het virtuele netwerk. De subnet-adresvoorvoegsels overlappen met elkaar niet.

De subnetconfiguraties schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), waarvan de subnetten maakt in het virtuele netwerk:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

U kunt een routetabel met nul of meer subnetten koppelen. Een subnet kan nul of één routetabel gekoppeld hebben. Uitgaand verkeer van een subnet wordt doorgestuurd op basis van Azure standaardroutes en eventuele aangepaste routes die u hebt toegevoegd aan een routetabel die u aan een subnet koppelen. Koppel de *myRouteTablePublic* routetabel aan de *openbare* subnet met [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) en schrijf vervolgens de subnetconfiguratie van de virtueel netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

Voordat u implementeert routetabellen voor gebruik in productieomgevingen, wordt aanbevolen dat u zorgvuldig vertrouwd raken met [routering in Azure](virtual-networks-udr-overview.md) en [Azure limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Test routering

Als u wilt testen routering, maakt u een virtuele machine die als het virtuele netwerk-apparaat fungeert dat de route die u in de vorige stap hebt gemaakt door middel van routes. Na het maken van het virtuele netwerk-apparaat, implementeert u een virtuele machine in de *openbare* en *persoonlijke* subnetten. U moet vervolgens doorsturen verkeer van de *openbare* subnet moet de *persoonlijke* subnet via het netwerk virtuele apparaat.

### <a name="create-a-network-virtual-appliance"></a>Maken van een virtueel netwerkapparaat

Een virtuele machine met een netwerktoepassing wordt vaak een virtueel netwerkapparaat genoemd. Virtuele netwerkapparaten meestal ontvangen netwerkverkeer, uitvoeren van een bepaalde actie vervolgens doorsturen of netwerkverkeer op basis van logica die zijn geconfigureerd in de netwerktoepassing verwijderen. 

#### <a name="create-a-network-interface"></a>Een netwerkinterface maken

In de vorige stap, moet u een route die een virtueel netwerkapparaat als het volgende hoptype opgegeven gemaakt. Een virtuele machine met een netwerktoepassing wordt vaak een virtueel netwerkapparaat genoemd. In productieomgevingen is het netwerk virtuele apparaat die u implementeert vaak een vooraf geconfigureerde virtuele machine. Verschillende virtuele netwerkapparaten zijn beschikbaar via de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). In dit artikel wordt wordt een eenvoudige virtuele machine gemaakt.

Een virtuele machine heeft een of meer netwerkinterfaces gekoppeld, waarmee de virtuele machine om te communiceren met het netwerk. Voor een netwerkinterface om te kunnen doorsturen van netwerkverkeer dat is verzonden, en dat niet bestemd is voor eigen IP-adres, moet doorsturen via IP worden ingeschakeld voor de netwerkinterface. Voordat u een netwerkinterface maakt, dat u hebt voor het ophalen van het virtuele netwerk-Id met [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), klikt u vervolgens de Id van het subnet met [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Maken van een netwerkinterface met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) in de *DMZ* subnet met doorsturen via IP ingeschakeld:

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

#### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Voor het maken van een virtuele machine en een bestaande netwerkinterface gekoppeld, moet u eerst een virtuele-machineconfiguratie met maken [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). De configuratie omvat de netwerkinterface die in de vorige stap hebt gemaakt. Als u wordt gevraagd om een gebruikersnaam en wachtwoord, selecteert u de gebruikersnaam en wachtwoord die u wilt zich aanmelden bij de virtuele machine met. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine configuration.
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

Maak de virtuele machine met behulp van de virtuele-machineconfiguratie met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap. Wanneer u wordt gevraagd, typt u de gebruikersnaam en wachtwoord die u wilt aanmelden bij de virtuele machine met. In productieomgevingen is het netwerk virtuele apparaat die u implementeert vaak een vooraf geconfigureerde virtuele machine. Verschillende virtuele netwerkapparaten zijn beschikbaar via de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Azure toegewezen 10.0.2.4 als het privé IP-adres van de virtuele machine omdat 10.0.2.4 is het eerste beschikbare IP-adres in de *DMZ* subnet van *myVirtualNetwork*.

### <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via het virtuele netwerk-apparaat in een later stadium. 

Maken van een virtuele machine in de *openbare* subnet met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmWeb* in de *openbare* subnet van de *myVirtualNetwork* virtueel netwerk. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmWeb" `
  -AsJob
```

Azure 10.0.0.4 toegewezen als de persoonlijke IP-adres van de virtuele machine omdat 10.0.1.4 het eerste beschikbare IP-adres in de *openbare* subnet van *myVirtualNetwork*.

Maken van een virtuele machine in de *persoonlijke* subnet.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmMgmt"
```

De virtuele machine duurt een paar minuten maken. Azure toegewezen 10.0.1.4 als het privé IP-adres van de virtuele machine omdat 10.0.1.4 is het eerste beschikbare IP-adres in de *persoonlijke* subnet van *myVirtualNetwork*. 

Niet verder gaan met de volgende stap voordat de virtuele machine wordt gemaakt en Azure PowerShell uitvoer weer.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>-Routeverkeer via een virtueel netwerkapparaat

Gebruik [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) ICMP inschakelen in de *myVmWeb* en *myVmMgmt* virtuele machines via de Windows firewall voor het gebruik van tracert testen communicatie tussen de virtuele machines in een latere stap:

```powershell-interactive
Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmWeb `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS

Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmMgmt `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS
```

De vorige opdrachten kunnen enkele minuten duren. Ga niet verder met de volgende stap totdat de opdrachten voltooit en u uitvoer geretourneerd naar PowerShell. Hoewel tracert wordt gebruikt voor het testen van routering in dit artikel, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.

U verbinding met het openbare IP-adres van een virtuele machine vanaf het Internet. Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van een virtuele machine. Het volgende voorbeeld wordt het openbare IP-adres van de *myVmMgmt* virtuele machine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Gebruik de volgende opdracht voor het maken van een sessie voor extern bureaublad met de *myVmMgmt* virtuele machine van de lokale computer. Vervang `<publicIpAddress>` geretourneerd met het IP-adres van de vorige opdracht.

```
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt, naar de computer gedownload en geopend. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, om op te geven de referenties die u hebt ingevoerd wanneer u de virtuele machine gemaakt), en selecteer vervolgens **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding. 

U doorsturen via IP in Azure voor de netwerkinterface van de virtuele machine in ingeschakeld [Enable IP fowarding](#enable-ip-forwarding). Vanuit de virtuele machine moet het besturingssysteem of een toepassing die wordt uitgevoerd op de virtuele machine ook kunnen voor het doorsturen van netwerkverkeer. Wanneer u een virtueel netwerkapparaat in een productieomgeving implementeert, het toestel doorgaans filters, Logboeken of een andere functie uitgevoerd voordat het doorsturen van verkeer. In dit artikel, het besturingssysteem gewoon alle verkeer wordt doorgestuurd die het ontvangt. Inschakelen van doorsturen via IP in het besturingssysteem van de *myVmNva*:

Vanaf een opdrachtprompt op de *myVmMgmt* virtuele machine, extern bureaublad naar de *myVmNva* virtuele machine:

``` 
mstsc /v:myVmNva
```
    
Om in te schakelen doorsturen via IP in het besturingssysteem van de *myVmNva* virtuele machine, voer de volgende opdracht in PowerShell op de *myVmNva* virtuele machine:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Start opnieuw op de *myVmNva* virtuele machine, die ook de extern bureaublad-sessiehost, zodat u binnen de extern bureaublad-sessiehost u hebt geopend naar verbreekt de *myVmMgmt* virtuele machine.

Na de *myVmNva* virtuele machine opnieuw is opgestart, gebruik de volgende opdracht voor het testen van routering voor netwerkverkeer naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine.

```
tracert myvmweb
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```
Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
  
Trace complete.
```

U kunt zien dat verkeer wordt gerouteerd rechtstreeks vanuit de *myVmMgmt* virtuele machine naar de *myVmWeb* virtuele machine. Azure standaardroutes verkeer leiden tussen de subnetten.

Gebruik de volgende opdracht met extern bureaublad naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine:

``` 
mstsc /v:myVmWeb
```

Voor het testen van routering voor netwerkverkeer naar de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine, typt u de volgende opdracht uit vanaf de opdrachtprompt:

```
tracert myvmmgmt
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```
Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```

U ziet dat de eerste hop 10.0.2.4 die de virtuele netwerkapparaat van privé IP-adres is. De tweede hop is 10.0.1.4, de persoonlijke IP-adres van de *myVmMgmt* virtuele machine. De route toegevoegd aan de *myRouteTablePublic* routetabel en die is gekoppeld aan de *openbare* subnet veroorzaakt Azure voor het routeren van het verkeer via de NVA in plaats van rechtstreeks naar de *persoonlijke* subnet.

Sluit de extern bureaublad-sessies tot zowel de *myVmWeb* en *myVmMgmt* virtuele machines.

## <a name="troubleshoot-routing"></a>Routering oplossen

Zoals u in de vorige stappen hebt geleerd, geldt Azure standaardroutes die u desgewenst met uw eigen routes overschrijven kunt. Verkeer kan soms niet zoals verwacht dat het worden gerouteerd. Gebruik [nieuw AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) een netwerk-Watcher in de regio EastUS inschakelen als u nog geen netwerk-Watcher in deze regio:

```azurepowershell-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region:
$nw = New-AzureRmNetworkWatcher `
 -Location eastus `
 -Name myNetworkWatcher_eastus `
 -ResourceGroupName myResourceGroup
```

Gebruik [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) om te bepalen hoe verkeer wordt gerouteerd tussen twee virtuele machines. Bijvoorbeeld de volgende opdracht test voor verkeersroutering van de *myVmWeb* (10.0.0.4) virtuele machine naar de *myVmMgmt* (10.0.1.4) virtuele machine:

```azurepowershell-interactive
$vmWeb = Get-AzureRmVM `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup

Get-AzureRmNetworkWatcherNextHop `
  -DestinationIPAddress 10.0.1.4 `
  -NetworkWatcherName myNetworkWatcher_eastus `
  -ResourceGroupName myResourceGroup `
  -SourceIPAddress 10.0.0.4 `
  -TargetVirtualMachineId $vmWeb.Id
```
De volgende uitvoer wordt geretourneerd na een korte wachten:

```azurepowershell
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

De uitvoer wordt gemeld dat de volgende hop-IP-adres voor verkeer van *myVmWeb* naar *myVmMgmt* 10.0.2.4 wordt (de *myVmNva* virtuele machine), het volgende hoptype is  *VirtualAppliance*, en dat de routetabel zorgt ervoor dat de routering *myRouteTablePublic*.

De effectieve routes voor elke netwerkinterface zijn een combinatie van Azure standaardroutes en alle routes die u definieert. Voor een overzicht van alle routes effectieve voor een netwerkinterface in een virtuele machine met [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). Bijvoorbeeld, om weer te geven van de effectieve routes voor de *myVmWeb* netwerkinterface in de *myVmWeb* virtuele machine, voer de volgende opdracht:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Alle standaardroutes en de route die u in de vorige stap hebt toegevoegd, worden geretourneerd.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources bevat.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een routetabel gemaakt en gekoppeld aan een subnet. U hebt gemaakt met een virtueel netwerkapparaat dat verkeer van een openbare subnet naar een persoonlijke subnet gerouteerd. Terwijl u veel Azure-resources binnen een virtueel netwerk implementeren kunt, kunnen geen resources voor sommige Azure PaaS-services worden geïmplementeerd in een virtueel netwerk. U kunt nog steeds toegang beperken tot de bronnen van sommige services Azure PaaS verkeer alleen via een virtueel netwerksubnet al. Ga naar het volgende artikel voor informatie over het netwerktoegang tot Azure PaaS-resources te beperken.

> [!div class="nextstepaction"]
> [Beperken van toegang tot het netwerk voor PaaS-resources](virtual-network-service-endpoints-configure.md#azure-powershell)