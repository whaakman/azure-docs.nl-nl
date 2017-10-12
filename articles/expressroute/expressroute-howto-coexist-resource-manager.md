---
title: 'ExpressRoute- en site-naar-site-VPN-verbindingen configureren die naast elkaar kunnen worden gebruikt: klassiek: Resource Manager: Azure | Microsoft Docs'
description: Dit artikel begeleidt u bij het configureren van ExpressRoute- en site-naar-site-VPN-verbindingen die naast elkaar kunnen worden gebruikt in het Resource Manager-implementatiemodel.
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.openlocfilehash: b29147a37f9a90fc80e16b350ac9b91daac1d7f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Gelijktijdige ExpressRoute- en site-to-site-verbindingen configureren
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell - Klassiek](expressroute-howto-coexist-classic.md)
> 
> 

Configuratie van gelijktijdige site-naar-site-VPN- en ExpressRoute-verbindingen heeft verschillende voordelen. U kunt een site-naar-site-VPN configureren als een beveiligd failoverpad voor ExressRoute of site-naar-site-VPN's gebruiken om verbinding te maken met sites die niet via ExpressRoute zijn verbonden. In dit artikel gaan we in op de stappen voor het configureren van beide scenario's. Dit artikel is van toepassing op het Resource Manager-implementatiemodel en maakt gebruik van PowerShell. Deze configuratie is niet beschikbaar in de Azure Portal.

> [!IMPORTANT]
> ExpressRoute-circuits moeten vooraf worden geconfigureerd voordat u de onderstaande instructies volgt. Zorg ervoor dat u de handleidingen hebt gevolgd [om een ExpressRoute-circuit te maken](expressroute-howto-circuit-arm.md) en [routering te configureren](expressroute-howto-routing-arm.md) voordat u verdergaat.
> 
> 

## <a name="limits-and-limitations"></a>Limieten en beperkingen
* **Transitroutering wordt niet ondersteund.** U kunt niet (via Azure) routeren tussen uw lokale netwerk dat is verbonden via site-naar-site-VPN en uw lokale netwerk dat is verbonden via ExpressRoute.
* **Basic SKU-gateway wordt niet ondersteund.** U moet een niet-basic SKU-gateway gebruiken voor zowel de [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) als de [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Alleen een op route gebaseerde VPN-gateway wordt ondersteund.** U moet een op route gebaseerde [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) gebruiken.
* **Statische route moet worden geconfigureerd voor de VPN-gateway.** Als uw lokale netwerk is verbonden met ExpressRoute en een site-naar-site-VPN, moet u in uw lokale netwerk een statische route hebben geconfigureerd voor het routeren van de site-naar-site-VPN-verbinding met het openbare internet.
* **ExpressRoute-gateway moet eerst worden geconfigureerd en gekoppeld aan een circuit.** U moet eerst de ExpressRoute-gateway maken en deze koppelen aan een circuit voordat u de site-naar-site-VPN-gateway kunt toevoegen.

## <a name="configuration-designs"></a>Configuratie-ontwerpen
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Een site-naar-site-VPN configureren als een failoverpad voor ExpressRoute
U kunt een site-naar-site-VPN-verbinding configureren als een back-up voor ExpressRoute. Dit geldt alleen voor virtuele netwerken die zijn gekoppeld aan het pad voor persoonlijke Azure-peering. Er is geen op VPN gebaseerde failoveroplossing voor services die toegankelijk zijn via openbare Azure- en Microsoft-peerings. Het ExpressRoute-circuit is altijd de primaire koppeling. Gegevens worden alleen via het site-naar-site-VPN-pad geleid als het ExpressRoute-circuit niet beschikbaar is.

> [!NOTE]
> Hoewel een ExpressRoute-circuit de voorkeur heeft boven site-naar-site-VPN wanneer beide routes hetzelfde zijn, gebruikt Azure de langste voorvoegselovereenkomst om de route naar de bestemming van het pakket te kiezen.
> 
> 

![Naast elkaar gebruiken](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>Een site-naar-site-VPN configureren om verbinding te maken met sites die niet zijn verbonden via ExpressRoute
U kunt uw netwerk zodanig configureren dat sommige sites rechtstreeks verbinding maken met Azure via site-naar-site-VPN en sommige sites verbinding maken via ExpressRoute. 

![Naast elkaar gebruiken](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> U kunt een virtueel netwerk niet configureren als transitrouter.
> 
> 

## <a name="selecting-the-steps-to-use"></a>De stappen selecteren die u gaat gebruiken
Er zijn twee verschillende procedures waaruit u kunt kiezen. Welke configuratieprocedure u selecteert, is afhankelijk van het gegeven of u een nieuw virtueel netwerk wilt maken of een bestaand virtueel netwerk hebt waarmee u verbinding wilt maken.

* Ik heb geen VNet en moet er een maken.
  
    Als u nog geen virtueel netwerk hebt, begeleidt deze procedure u bij het maken van een nieuw virtueel netwerk met behulp van de Resource Manager-implementatie, en bij het maken van nieuwe ExpressRoute- en site-naar-site-VPN-verbindingen. Volg voor het configureren van een virtueel netwerk de stappen in [Een nieuw virtueel netwerk en naast elkaar bestaande verbindingen maken](#new).
* Ik heb al een VNet van het Resource Manager-implementatiemodel.
  
    Mogelijk hebt u al een virtueel netwerk met een bestaande site-naar-site-VPN-verbinding of ExpressRoute-verbinding. Het gedeelte [Naast elkaar bestaande verbindingen configureren voor een bestaand VNet](#add) begeleidt u bij het verwijderen van de gateway en vervolgens bij het maken van nieuwe ExpressRoute- en site-naar-site-VPN-verbindingen. De stappen voor het maken van de nieuwe verbindingen moeten in een specifieke volgorde worden uitgevoerd. Gebruik geen instructies uit andere artikelen om uw gateways en verbindingen te maken.
  
    In deze procedure moet u uw gateway verwijderen en vervolgens nieuwe gateways configureren om verbindingen te maken die naast elkaar kunnen worden gebruikt. U moet tijdens het verwijderen en opnieuw maken van uw gateway en verbindingen rekening houden met uitvaltijd voor uw cross-premises verbindingen. U hoeft uw virtuele machines of services echter niet te migreren naar een nieuw virtueel netwerk. Terwijl u uw gateway configureert, kunnen uw virtuele machines en services nog steeds communiceren via de load balancer, mits ze hiervoor zijn geconfigureerd.

## <a name="new"></a>Een nieuw virtueel netwerk en naast elkaar bestaande verbindingen maken
Deze procedure begeleidt u bij het maken van een VNet en van site-naar-site- en ExpressRoute-verbindingen die naast elkaar kunnen worden gebruikt.

1. Installeer de meest recente versie van de Azure PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de cmdlets. De cmdlets die u voor deze configuratie gebruikt, kunnen enigszins afwijken van de cmdlets waarmee u bekend bent. Zorg ervoor dat u de cmdlets gebruikt die in deze instructies worden vermeld.
2. Meld u aan bij uw account en stel de omgeving in.

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Maak een virtueel netwerk met een gatewaysubnet. Zie [Azure Virtual Network-configuratie](../virtual-network/virtual-networks-create-vnet-arm-ps.md) voor meer informatie over de configuratie van het virtuele netwerk.
   
   > [!IMPORTANT]
   > Het gatewaysubnet moet /27 of een korter voorvoegsel (zoals /26 of /25) zijn.
   > 
   > 
   
    Maak een nieuw VNet.

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    Voeg subnetten toe.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Sla de VNet-configuratie op.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="gw"></a>Maak een ExpressRoute-gateway. Zie [ExpressRoute-gatewayconfiguratie](expressroute-howto-add-gateway-resource-manager.md) voor meer informatie over de configuratie van de ExpressRoute-gateway. De GatewaySKU moet *Standard*, *HighPerformance* of *UltraPerformance* zijn.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. Koppel de ExpressRoute-gateway aan het ExpressRoute-circuit. Nadat deze stap is voltooid, wordt de verbinding tussen uw on-premises netwerk en Azure tot stand gebracht via ExpressRoute. Zie [VNets koppelen aan ExpressRoute](expressroute-howto-linkvnet-arm.md) voor meer informatie over de koppelingsbewerking.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <a name="vpngw"></a>Maak vervolgens uw site-naar-site-VPN-gateway. Zie [Een VNet met een site-naar-site-verbinding configureren](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) voor meer informatie over de configuratie van de VPN-gateway. De GatewaySKU moet *Standard*, *HighPerformance* of *UltraPerformance* zijn. Het VpnType moet *RouteBased* zijn.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    Azure VPN-gateway biedt ondersteuning voor BGP-routeringsprotocol. U kunt voor dit virtuele netwerk een ASN (AS-nummer) opgeven door in de volgende opdracht de schakeloptie -Asn toe te voegen. Als u deze parameter niet opgeeft, is het AS-nummer standaard 65515.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    U kunt het BGP peering IP- en het AS-nummer die Azure voor de VPN-gateway gebruikt, vinden in $azureVpn.BgpSettings.BgpPeeringAddress en $azureVpn.BgpSettings.Asn. Zie [BGP configureren](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) voor Azure VPN-gateway voor meer informatie.
7. Maak een lokaal exemplaar van de VPN-gateway van uw site. Deze opdracht wordt niet gebruikt om uw on-premises VPN-gateway te configureren. Met deze opdracht kunt u de instellingen voor de lokale gateway opgeven, zoals het openbare IP-adres en de on-premises-adresruimte, zodat de Azure VPN-gateway er verbinding mee kan maken.
   
    Als uw lokale VPN-apparaat alleen statische routering ondersteunt, kunt u de statische routes op de volgende manier configureren:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Als uw lokale VPN-apparaat de BGP ondersteunt en u dynamische ondersteuning wilt inschakelen, moet u het BGP peering-IP en het AS-nummer die uw lokale VPN-apparaat gebruikt, kennen.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for the BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. Configureer het lokale VPN-apparaat om verbinding te maken met de nieuwe Azure VPN-gateway. Zie [VPN-apparaatconfiguratie](../vpn-gateway/vpn-gateway-about-vpn-devices.md) voor meer informatie over het configureren van een VPN-apparaat. 
9. Koppel de site-naar-site-VPN-gateway in Azure aan de lokale gateway.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <a name="add"></a>Naast elkaar bestaande verbindingen configureren voor een bestaand VNet
Als u een bestaand virtueel netwerk hebt, controleert u de grootte van het gatewaysubnet. Als het gatewaysubnet /28 of /29 is, moet u eerst de gateway van het virtuele netwerk verwijderen en het gatewaysubnet vergroten. In de stappen in dit gedeelte wordt beschreven hoe u dat doet.

Als het gatewaysubnet /27 of groter is en het virtuele netwerk is verbonden via ExpressRoute, kunt u onderstaande stappen overslaan en doorgaan met [Stap 6 - Een site-naar-site-VPN-gateway maken](#vpngw) in het vorige gedeelte. 

> [!NOTE]
> Als u de bestaande gateway verwijdert terwijl u bezig bent met deze configuratie, wordt de verbinding tussen uw lokale site en uw virtuele netwerk verbroken. 
> 
> 

1. U moet de meest recente versie van de Azure PowerShell-cmdlets installeren. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over het installeren van cmdlets. De cmdlets die u voor deze configuratie gebruikt, kunnen enigszins afwijken van de cmdlets waarmee u bekend bent. Zorg ervoor dat u de cmdlets gebruikt die in deze instructies worden vermeld. 
2. Verwijder de bestaande ExpressRoute- of site-naar-site-VPN-gateway.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Verwijder het gatewaysubnet.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. Voeg een gatewaysubnet toe van /27 of hoger.
   
   > [!NOTE]
   > Als u in uw virtuele netwerk niet voldoende IP-adressen over hebt om het gatewaysubnet te vergroten, moet u meer IP-adresruimte toevoegen.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Sla de VNet-configuratie op.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. U hebt op dit moment een VNet zonder gateways. Als u nieuwe gateways wilt maken en uw verbindingen wilt voltooien, kunt u doorgaan met [Stap 4 - Een ExpressRoute-gateway maken](#gw). Deze stap vindt u in de voorgaande reeks stappen.

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a>Punt-naar-site-configuratie toevoegen aan de VPN-gateway
Volg onderstaande stappen om punt-naar-site-configuratie toe te voegen aan uw VPN-gateway in een installatie waarbij ze naast elkaar kunnen worden gebruikt.

1. Voeg een VPN-clientadresgroep toe.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. Upload het VPN-basiscertificaat voor uw VPN-gateway naar Azure. In dit voorbeeld wordt ervan uitgegaan dat het basiscertificaat is opgeslagen op de lokale computer waarop de volgende PowerShell-cmdlets worden uitgevoerd.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

Zie [Een punt-naar-site-verbinding configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) voor meer informatie over punt-naar-site-VPN.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
