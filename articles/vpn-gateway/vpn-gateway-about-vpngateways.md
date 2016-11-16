---
title: Informatie over VPN-gateway| Microsoft Docs
description: Meer informatie over VPN Gateway-verbindingen voor Azure Virtual Networks.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 909320f7f898a10ff58c50d276bbe9b2b2a92b48


---
# <a name="about-vpn-gateway"></a>Informatie over VPN-gateway
Een virtuele netwerkgateway wordt gebruikt voor het verzenden van netwerkverkeer tussen virtuele netwerken van Azure en on-premises locaties, evenals tussen virtuele netwerken in Azure (VNet-naar-VNet). Wanneer u een VPN-gateway configureert, moet u een virtuele netwerkgateway en een virtuele netwerkgatewayverbinding maken en configureren.

Wanneer u een resource van een virtuele netwerkgateway maakt in het implementatiemodel van Resource Manager, kunt u verschillende instellingen opgeven. Een van de vereiste instellingen is '-GatewayType'. Er zijn twee types virtuele netwerkgateway’s: VPN en ExpressRoute. 

Wanneer netwerkverkeer wordt verzonden via een specifieke privéverbinding, gebruikt u het gatewaytype 'ExpressRoute'. Dit wordt ook wel een ExpressRoute-gateway genoemd. Wanneer netwerkverkeer versleuteld wordt verzonden via een openbare verbinding, gebruikt u het gatewaytype 'VPN'. Dit wordt ook wel een VPN-gateway genoemd. Site-naar-site-, punt-naar-site- en VNet-naar-VNet-verbindingen gebruiken allemaal een VPN-gateway.

Elk virtueel netwerk kan maar één virtuele netwerkgateway per type gateway hebben. U kunt voor een virtueel netwerk bijvoorbeeld één gateway gebruiken die -GatewayType ExpressRoute gebruikt en één die -GatewayType Vpn gebruikt. Dit artikel richt zich voornamelijk op VPN Gateway. Zie [Technical Overview](../expressroute/expressroute-introduction.md) (Technisch overzicht) voor meer informatie over ExpressRoute.

## <a name="pricing"></a>Prijzen
[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

## <a name="gateway-skus"></a>Gateway-SKU's
[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Zie [Gateway-SKU's](vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor meer informatie over gateway-SKU's voor VPN Gateway.

### <a name="estimated-aggregate-throughput-by-sku"></a>Geschatte geaggregeerde doorvoer per SKU
[!INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="configuring-a-vpn-gateway"></a>Een VPN-gateway configureren
Wanneer u een VPN-gateway configureert, zijn de instructies die u moet volgen afhankelijk van het implementatiemodel waarmee u het virtuele netwerk hebt gemaakt. Als u bijvoorbeeld uw VNet hebt gemaakt met het klassieke implementatiemodel, gebruikt u de richtlijnen en instructies voor het klassieke implementatiemodel om de VPN-gateway te maken en de instellingen te configureren. Zie [Het Resource Manager-implementatiemodel en het klassieke implementatiemodel begrijpen](../resource-manager-deployment-model.md) voor meer informatie over de implementatiemodellen.

Een VPN-gatewayverbinding is afhankelijk van meerdere resources die zijn geconfigureerd met specifieke instellingen. De meeste resources kunnen afzonderlijk worden geconfigureerd, hoewel ze in sommige gevallen in een bepaalde volgorde moeten worden geconfigureerd. U kunt beginnen met het maken en configureren van resources met een configuratiehulpprogramma, zoals Azure Portal. U kunt vervolgens later besluit om over te schakelen naar een ander hulpprogramma, zoals PowerShell, om aanvullende resources te configureren, of om indien nodig bestaande bronnen te wijzigen. Op dit moment is het niet mogelijk om elke resource en resource-instelling in Azure Portal te configureren. De instructies in de artikelen voor elke verbindingstopologie geven aan of een specifiek confihuratiehulpprogramma nodig is. Zie voor meer informatie over afzonderlijke resources en de instellingen voor VPN Gateway [Over VPN Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).

De volgende gedeelten bevatten tabellen met de volgende informatie:

* beschikbaar implementatiemodel
* beschikbare configuratiehulpprogramma's
* rechtstreekse koppelingen naar een artikel, indien beschikbaar

Gebruik de diagrammen en beschrijvingen als hulp bij het selecteren van de juiste verbindingstopologie voor uw vereisten. De diagrammen tonen de belangrijkste basistopologieën, maar het is mogelijk om met de diagrammen als richtlijn complexere configuraties te bouwen.

## <a name="sitetosite-and-multisite"></a>Site-naar-site en multi-site
### <a name="sitetosite"></a>Site-naar-site
Een site-naar-site-VPN-gatewayverbinding (S2S) is een verbinding via een VPN-tunnel met IPsec/IKE (IKEv1 of IKEv2). Voor dit type verbinding moet er een VPN-apparaat on-premises aanwezig zijn waaraan een openbaar IP-adres is toegewezen en dat zich niet achter een NAT bevindt. S2S-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.   

![S2S-verbinding](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")

### <a name="multisite"></a>Multi-site
U kunt een VPN-gatewayverbinding tussen uw VNet en meerdere on-premises netwerken maken en configureren. Als u met meerdere verbindingen werkt, moet u een op een route gebaseerd VPN-type (dynamische gateway voor klassieke VNets) gebruiken. Omdat een VNet maar één VPN-gateway kan hebben, delen alle verbindingen via de gateway de beschikbare bandbreedte. Dit wordt vaak een multi-site-verbinding genoemd.

![Multi-site-verbinding](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### <a name="deployment-models-and-methods-for-sitetosite-and-multisite"></a>Implementatiemodellen en -methoden voor site-naar-site en multi-site
[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="vnettovnet"></a>VNet-naar-VNet
Het verbinden van een virtueel netwerk met een ander virtueel netwerk (VNet-naar-VNet) lijkt op het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. U kunt zelfs VNet-naar-VNet-communicatie met multi-site-verbindingsconfiguraties combineren. Zo kunt u netwerktopologieën maken waarin cross-premises connectiviteit is gecombineerd met connectiviteit tussen virtuele netwerken.

U kunt de volgende VNets verbinden:

* in dezelfde of verschillende regio's
* in dezelfde of verschillende abonnementen 
* in dezelfde of verschillende implementatiemodellen

![VNet-naar-VNet-verbinding](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")

#### <a name="connections-between-deployment-models"></a>Verbindingen tussen implementatiemodellen
Azure heeft momenteel twee implementatiemodellen: klassiek en Resource Manager. Als u al langer met Azure hebt gewerkt, hebt u waarschijnlijk Azure-VM's en -rolinstanties in een klassiek VNet. De nieuwere VM's en rolinstanties werken mogelijk in een VNet dat is gemaakt in Resource Manager. U kunt de VNets verbinden zodat de resources in het ene VNet direct met de resources in het andere kunnen communiceren.

#### <a name="vnet-peering"></a>VNet-peering
Zolang het virtuele netwerk voldoet aan bepaalde vereisten, kunt u VNet-peering gebruiken om uw verbinding te maken. Bij VNet-peering wordt geen virtuele netwerkgateway gebruikt. Zie het artikel [VNet-peering](../virtual-network/virtual-network-peering-overview.md) voor meer informatie.

### <a name="deployment-models-and-methods-for-vnettovnet"></a>Implementatiemodellen en -methoden voor VNet-naar-VNet
[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="pointtosite"></a>Punt-naar-site
Met een punt-naar-site (P2S)-VPN-gatewayverbinding kunt u vanuit een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Voor P2S-verbindingen hebt u geen VPN-apparaat of openbaar IP-adres nodig. U kunt de VPN-verbinding maken door deze vanaf de clientcomputer te starten. Dit is een uitstekende oplossing als u uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, wilt verbinden, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een VNet. P2S-verbindingen kunnen worden gebruikt in combinatie met S2S-verbindingen via dezelfde VPN-gateway, mits alle configuratievereisten voor beide verbindingen compatibel zijn.

![Punt-naar-site-verbinding](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### <a name="deployment-models-and-methods-for-pointtosite"></a>Implementatiemodellen en -methoden voor punt-naar-site
[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="expressroute"></a>ExpressRoute
[!INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

In een ExpressRoute-verbinding wordt een virtuele netwerkgateway geconfigureerd met het gatewaytype 'ExpressRoute' in plaats van 'VPN'. Zie [Technical Overview](../expressroute/expressroute-introduction.md) (Technisch overzicht) voor meer informatie over ExpressRoute.

## <a name="sitetosite-and-expressroute-coexisting-connections"></a>Site-naar-site- en ExpressRoute-verbindingen naast elkaar
ExpressRoute is een directe, exclusieve verbinding van uw WAN (niet via het openbare internet) met Microsoft-services, waaronder Azure. Site-naar-site-VPN-verkeer verplaatst zich versleuteld via het openbare internet. De mogelijkheid om site-naar-site-VPN- en ExpressRoute-verbindingen voor hetzelfde virtuele netwerk te configureren, heeft verschillende voordelen.

U kunt een site-naar-site-VPN configureren als een beveiligd failoverpad voor ExpressRoute of site-naar-site-VPN’s gebruiken om verbinding te maken met sites die geen deel uitmaken van uw netwerk, maar zijn verbonden via ExpressRoute. Deze configuratie vereist twee gateways voor hetzelfde virtuele netwerk, één met -GatewayType VPN en de andere met -GatewayType ExpressRoute.

![Naast elkaar bestaande verbinding](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Implementatiemodellen en -methoden voor S2S en ExpressRoute
[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="next-steps"></a>Volgende stappen
De VPN-gatewayconfiguratie plannen. Zie [VPN-gateway plannen en ontwerpen](vpn-gateway-plan-design.md) en [Uw on-premises netwerk verbinden met Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).




<!--HONumber=Nov16_HO2-->


