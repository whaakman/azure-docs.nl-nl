---
title: 'Geforceerde tunneling voor Azure Site-naar-Site-verbindingen configureren: Resource Manager | Microsoft Docs'
description: Het omleiden of 'afdwingen' alle internetverkeer terug naar uw on-premises locatie.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2018
ms.author: cherylmc
ms.openlocfilehash: 21004c29f1baf0346cd83d8483ff1862a98fc845
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55506461"
---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Geforceerde tunneling met het implementatiemodel van Azure Resource Manager configureren

Geforceerde tunneling kunt u omleiding of 'force' alle internetverkeer terug naar uw on-premises locatie via een Site-naar-Site VPN-tunnel voor inspectie en controle. Dit is een essentiële vereiste voor de meeste bedrijven beleid. Zonder geforceerde tunneling, Internet bestemde verkeer van uw virtuele machines in Azure altijd traverses van Azure network-infrastructuur rechtstreeks uit met het Internet, zonder de optie waarmee u kunt controleren of het verkeer controleren. Niet-geautoriseerde toegang tot Internet kan mogelijk leiden tot vrijgeven van informatie of andere typen schendingen van de beveiliging.

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

In dit artikel begeleidt u bij het configureren geforceerde tunneling voor virtuele netwerken die zijn gemaakt met het Resource Manager-implementatiemodel. Geforceerde tunneling kan worden geconfigureerd met behulp van PowerShell, niet via de portal. Als u configureren met geforceerde tunnels zijn voor het klassieke implementatiemodel wilt, selecteert u klassieke artikel in de volgende vervolgkeuzelijst:

> [!div class="op_single_selector"]
> * [PowerShell - Klassiek](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Informatie over geforceerde tunneling

Het volgende diagram illustreert hoe geforceerde tunneling werkt. 

![Geforceerde tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

In het bovenstaande voorbeeld worden de front-end subnet niet gedwongen tunnel. De werkbelastingen in het subnet Frontend kunnen blijven om te accepteren en rechtstreeks reageren op aanvragen van klanten via Internet. De middelste laag en back-end-subnetten zijn geforceerde tunnels. Uitgaande verbindingen vanuit deze twee subnetten met het Internet wordt afgedwongen of keert u terug naar een on-premises site via een van de S2S VPN-tunnels.

Hiermee kunt u om te beperken en controleren van toegang tot het Internet van uw virtuele machines of cloudservices in Azure, terwijl u verdergaat met het inschakelen van uw service met meerdere lagen architectuur die is vereist. Als er geen internetverbinding werkbelastingen in uw virtuele netwerken zijn, kunt ook u aanvragen geforceerde tunneling naar de volledige virtuele netwerken.

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

Geforceerde tunneling in Azure is via het virtuele netwerk zelfgedefinieerde routes geconfigureerd. Verkeer omleiden naar een on-premises site wordt uitgedrukt als een standaard-Route naar de Azure VPN-gateway. Zie voor meer informatie over de gebruiker gedefinieerde Routering en virtuele netwerken, [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).

* Elk virtueel netwerksubnet heeft een ingebouwde, systeem-routeringstabel. De routeringstabel van het systeem heeft de volgende drie groepen van routes:
  
  * **Lokale VNet routes:** Rechtstreeks naar de bestemming VM's in hetzelfde virtuele netwerk.
  * **On-premises routes:** Met Azure VPN-gateway.
  * **Standaard-route:** Rechtstreeks met Internet. Pakketten dat is bestemd voor het particuliere IP-adressen niet wordt gedekt door de vorige twee routes worden verwijderd.
* Deze procedure maakt gebruik van gebruiker gedefinieerde routes (UDR) om het maken van een routeringstabel voor het toevoegen van een standaardroute en koppel vervolgens de routeringstabel voor de subnetten van uw VNet om in te schakelen geforceerde tunneling op deze subnetten.
* Geforceerde tunneling moet worden gekoppeld aan een VNet met een op route gebaseerde VPN-gateway. U moet een 'standaard-site"tussen de cross-premises lokale sites die zijn verbonden met het virtuele netwerk instellen. De on-premises VPN-apparaat moet ook worden geconfigureerd met 0.0.0.0/0 als verkeerkiezers. 
* ExpressRoute geforceerde tunneling is niet geconfigureerd via dit mechanisme, maar in plaats daarvan wordt ingeschakeld door kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies. Zie voor meer informatie de [documentatie voor ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Configuratie-overzicht

De volgende procedure kunt u een resourcegroep en een VNet maken. Vervolgens maakt een VPN-gateway en geforceerde tunneling configureren. In deze procedure is het virtuele netwerk MultiTier-VNet drie subnetten: 'Frontend', 'Midtier' en 'Back-end', met vier cross-premises verbindingen: 'DefaultSiteHQ' en drie vertakkingen.

De procedurestappen de 'DefaultSiteHQ' als de standaard-site-verbinding voor geforceerde tunneling instellen en configureren van de 'Midtier' en 'back-end-subnetten gebruik van geforceerde tunneling.

## <a name="before"></a>Voordat u begint

Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets.

> [!IMPORTANT]
> Installeer de nieuwste versie van de PowerShell-cmdlets is vereist. Anders ontvangt u mogelijk validatiefouten bij het uitvoeren van enkele van de cmdlets.
>
>

### <a name="to-log-in"></a>Om aan te melden

[!INCLUDE [To log in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Geforceerde tunneling configureren

> [!NOTE]
> Mogelijk ziet u dat 'het objecttype van de uitvoer van deze cmdlet wordt gewijzigd in een toekomstige release' waarschuwingen. Dit is normaal en u deze waarschuwingen kan negeren.
>
>


1. Maak een resourcegroep.

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. Maak een virtueel netwerk en subnetten opgeven.

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. Maak de lokale netwerkgateways.

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. De routetabel en route-regel maken.

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. De routetabel aan het Midtier- en back-end-subnetten koppelen.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. De virtuele netwerkgateway maakt. Deze stap neemt enige tijd in beslag en soms 45 minuten of langer, omdat u het maken en configureren van de gateway. Als u fouten met betrekking tot de waarde GatewaySKU ValidateSet ziet, controleert u of dat u hebt geïnstalleerd het [meest recente versie van de PowerShell-cmdlets](#before). De nieuwste versie van de PowerShell-cmdlets bevat de nieuwe gevalideerde waarden voor de meest recente Gateway-SKU's.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $false
  ```
7. Een standaard-site toewijzen aan de virtuele netwerkgateway. De **- GatewayDefaultSite** is de cmdlet-parameter waarmee de configuratie van de geforceerde routering om te werken,. Let daarom goed deze instelling niet te configureren. 

  ```powershell
  $LocalGateway = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling"
  $VirtualGateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkGatewayDefaultSite -GatewayDefaultSite $LocalGateway -VirtualNetworkGateway $VirtualGateway
  ```
8. De Site-naar-Site VPN-verbindingen tot stand brengen.

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```
