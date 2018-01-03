---
title: 'Configureer geforceerde tunneling voor Azure Site-naar-Site-verbindingen: Resource Manager | Microsoft Docs'
description: Het omleiden of 'force' alle internetverkeer terug naar uw on-premises locatie.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2017
ms.author: cherylmc
ms.openlocfilehash: cc8a3e7f2a907b1eea4ecf39df2b291b0fb8b207
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Geforceerde tunneling met het implementatiemodel van Azure Resource Manager configureren

Geforceerde tunneling kunt u omleiding of 'force' alle internetverkeer terug naar uw on-premises locatie via een Site-naar-Site VPN-tunnel voor inspectie en controle. Dit is een kritieke beveiligingsvereiste voor de meeste bedrijven beleidsregels. Zonder geforceerde tunneling, Internet bestemd verkeer van uw virtuele machines in Azure altijd traverses van Azure netwerkinfrastructuur rechtstreeks uit met het Internet, zonder de optie kunt u controleren of het verkeer controleren. Niet-geautoriseerde toegang tot het Internet kan mogelijk leiden tot vrijgeven van informatie of andere typen schendingen van de beveiliging.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

In dit artikel begeleidt u bij het configureren geforceerde tunneling voor virtuele netwerken die zijn gemaakt met het implementatiemodel van Resource Manager. Geforceerde tunneling kan worden geconfigureerd met behulp van PowerShell, niet via de portal. Als u configureren voor het klassieke implementatiemodel een geforceerde tunneling wilt, selecteert u de klassieke artikel uit de volgende vervolgkeuzelijst:

> [!div class="op_single_selector"]
> * [PowerShell - Klassiek](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Over geforceerde tunneling

Het volgende diagram illustreert hoe geforceerde tunneling werkt. 

![Geforceerde tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

In het bovenstaande voorbeeld via het subnet wordt niet geforceerd Frontend tunnel. De werkbelastingen binnen het subnet Frontend kunnen blijven om te accepteren en rechtstreeks reageren op aanvragen van klanten via Internet. De middelste laag en back-end subnetten worden gedwongen via een tunnel. Uitgaande verbindingen van deze twee subnetten met het Internet worden gedwongen of omgeleid naar een on-premises site via een van de S2S VPN-tunnels.

Hiermee kunt u beperken en controleren van toegang tot Internet vanaf uw virtuele machines of cloudservices in Azure, terwijl u het inschakelen van uw service met meerdere lagen architectuur die is vereist. Als er geen internetverbinding werkbelastingen in uw virtuele netwerken, kunt ook u toepassen geforceerde tunneling naar de volledige virtuele netwerken.

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

Geforceerde tunneling in Azure wordt geconfigureerd via het virtuele netwerk zelfgedefinieerde routes. Intranetwebproxyserver verkeer omleidt naar een on-premises site wordt uitgedrukt als een standaardroute voor de Azure VPN-gateway. Zie voor meer informatie over de gebruiker gedefinieerde Routering en virtuele netwerken, [gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).

* Elk virtueel netwerksubnet heeft een ingebouwd systeem-routeringstabel. De routeringstabel van het systeem heeft de volgende drie groepen van routes:
  
  * **Lokale VNet routes:** rechtstreeks aan de doel-virtuele machines in hetzelfde virtuele netwerk.
  * **Lokale routes:** naar de Azure VPN-gateway.
  * **Standaardroute:** rechtstreeks met het Internet. Pakketten die bestemd zijn voor de persoonlijke IP-adressen niet wordt gedekt door de vorige twee routes worden verwijderd.
* Deze procedure gebruikt de gebruiker gedefinieerde routes (UDR) voor het maken van een routeringstabel een standaardroute toevoegen en vervolgens de routeringstabel wilt koppelen aan uw VNet subnetten met inschakelen van geforceerde tunneling op deze subnetten.
* Geforceerde tunneling moet worden gekoppeld aan een VNet met een op route gebaseerde VPN-gateway. U moet een "standaardsite instellen ' tussen de cross-premises lokale sites verbonden met het virtuele netwerk. De on-premises VPN-apparaat moet ook worden geconfigureerd met 0.0.0.0/0 als verkeer selectoren. 
* ExpressRoute geforceerde tunneling via dit mechanisme niet is geconfigureerd, maar in plaats daarvan wordt ingeschakeld door kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies. Zie voor meer informatie de [ExpressRoute-documentatie](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Configuratie-overzicht

De volgende procedure kunt u een resourcegroep en een VNet maken. Vervolgens moet u een VPN-gateway maken en geforceerde tunneling configureren. In deze procedure is het virtuele netwerk MultiTier-VNet drie subnetten: 'Frontend', 'Midtier' en 'Back-end' met vier cross-premises verbindingen: 'DefaultSiteHQ' en drie vertakkingen.

De procedurestappen de 'DefaultSiteHQ' als de standaard site-verbinding voor geforceerde tunneling instellen en configureren 'Midtier' en 'Back-end' subnetten gebruik van geforceerde tunneling.

## <a name="before"></a>Voordat u begint

Installeer de meest recente versie van de PowerShell-cmdlets van Azure Resource Manager. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets.

> [!IMPORTANT]
> De nieuwste versie van de PowerShell-cmdlets installeren is vereist. Anders krijgt u validatiefouten opgetreden bij het uitvoeren van enkele van de cmdlets.
>
>

### <a name="to-log-in"></a>Aanmelden

[!INCLUDE [To log in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Geforceerde tunneling configureren

> [!NOTE]
> Mogelijk ziet u waarschuwingen weergegeven met de tekst 'het objecttype van de uitvoer van deze cmdlet wordt gewijzigd in een toekomstige release'. Dit is verwacht gedrag en u kunt deze waarschuwingen negeren.
>
>


1. Maak een resourcegroep.

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. Een virtueel netwerk maken en geef de subnetten.

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. Maken van de lokale netwerkgateways.

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
5. De routetabel aan het Midtier- en back-end subnetten koppelen.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. De Gateway te maken met een standaard-site. Deze stap duurt enige tijd in beslag nemen, soms 45 minuten of langer, omdat u maken en configureren van de gateway.<br> De **- GatewayDefaultSite** de cmdlet-parameter waarmee de geforceerde routeringsconfiguratie voor gebruik, dus wees voorzichtig voor het configureren van deze instelling correct is. Als u fouten met betrekking tot de waarde GatewaySKU ValidateSet ziet, controleert u of u hebt geïnstalleerd het [meest recente versie van de PowerShell-cmdlets](#before). De nieuwste versie van de PowerShell-cmdlets bevat de nieuwe gevalideerde waarden voor de nieuwste Gateway-SKU's.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. De Site-naar-Site VPN-verbindingen tot stand brengen.

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
