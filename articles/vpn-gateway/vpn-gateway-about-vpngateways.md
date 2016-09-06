<properties 
   pageTitle="Informatie over VPN-gateway| Microsoft Azure"
   description="Meer informatie over VPN Gateway-verbindingen voor Azure Virtual Networks."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# Informatie over VPN-gateway


VPN Gateway is een verzameling resources die worden gebruikt voor het verzenden van netwerkverkeer tussen virtuele netwerken en on-premises locaties. Gateways worden gebruikt voor site-naar-site-, punt-naar-site- en ExpressRoute-verbindingen. VPN-gateway wordt ook gebruikt om verkeer tussen meerdere virtuele netwerken in Azure (VNet-naar-VNet) te verzenden. 

Als u een verbinding wilt maken, voegt u een virtuele netwerkgateway toe aan een VNet en configureert u aanvullende VPN Gateway-resources en de bijbehorende instellingen. Elk virtueel netwerk kan maar één virtuele netwerkgateway per type gateway hebben. U kunt voor een virtueel netwerk bijvoorbeeld één gateway gebruiken die -GatewayType Vpn gebruikt en één die -GatewayType ExpressRoute gebruikt.

Zie [Gatewayvereisten](vpn-gateway-about-vpn-gateway-settings.md#requirements) voor meer informatie over vereisten voor gateways. Zie [About VPN Gateway settings](vpn-gateway-about-vpn-gateway-settings.md#aggthroughput) (Informatie over instellingen voor VPN Gateway) voor geschatte geaggregeerde doorvoer. Zie [Prijzen voor VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) voor prijzen. Zie [Netwerklimieten](../articles/azure-subscription-service-limits.md#networking-limits) voor abonnementen en servicelimieten.

Wanneer u VPN Gateway configureert, zijn de instructies die u moet volgen afhankelijk van het implementatiemodel waarmee u het virtuele netwerk hebt gemaakt. Als u bijvoorbeeld uw VNet hebt gemaakt met het klassieke implementatiemodel, gebruikt u de richtlijnen en instructies voor het klassieke implementatiemodel om de VPN-gateway te maken en de instellingen te configureren. Zie [Understanding Resource Manager and classic deployment models](../resource-manager-deployment-model.md) (Het Resource Manager-implementatie en klassieke implementatie begrijpen) voor meer informatie.

De volgende secties bevatten tabellen met de volgende informatie voor de configuratie:

- beschikbaar implementatiemodel
- beschikbare configuratiehulpprogramma's
- rechtstreekse koppelingen naar een artikel, indien beschikbaar


Gebruik de diagrammen en beschrijvingen als hulp bij het selecteren van de juiste configuratietopologie voor uw vereisten. De diagrammen tonen de belangrijkste basistopologieën, maar het is mogelijk om met de diagrammen als richtlijn complexere configuraties te bouwen. Elke configuratie is afhankelijk van de VPN Gateway-instellingen die u selecteert.

### Instellingen voor VPN Gateway configureren

Omdat VPN Gateway een verzameling resources is, kunt u bepaalde resources configureren met één hulpprogramma en vervolgens naar een ander hulpprogramma gaan om de instellingen van een andere resource te configureren. Op dit moment is het niet mogelijk om elke VPN Gateway-resource-instelling in de Azure-portal te configureren. De instructies in de artikelen voor elke configuratie geven aan of een specifiek hulpprogramma nodig is. Als u met het klassieke implementatiemodel werkt, wilt u nu misschien in de klassieke portal werken of PowerShell gebruiken. Zie [About VPN Gateway settings](vpn-gateway-about-vpn-gateway-settings.md) (Informatie over instellingen voor VPN Gateway) voor meer informatie over de afzonderlijke beschikbare instellingen.



## Site-naar-site en multi-site

### Site-naar-site

Een site-naar-site-verbinding (S2S) is een verbinding via een VPN-tunnel met IPsec/IKE (IKEv1 of IKEv2). Voor dit type verbinding moet er een VPN-apparaat on-premises aanwezig zijn waaraan een openbaar IP-adres is toegewezen en dat zich niet achter een NAT bevindt. S2S-verbindingen kunnen worden gebruikt voor cross-premises en hybride configuraties.   

![S2S-verbinding](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")


### Multi-site

U kunt een VPN-verbinding tussen uw VNet en meerdere on-premises netwerken maken en configureren. Als u met meerdere verbindingen werkt, moet u een op een route gebaseerd VPN-type (dynamische gateway voor klassieke VNets) gebruiken. Omdat een VNet maar één virtuele netwerkgateway kan hebben, delen alle verbindingen via de gateway de beschikbare bandbreedte. Dit type configuratie wordt vaak een multi-site-verbinding genoemd.
 

![Multi-site-verbinding](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### Implementatiemodellen en -methoden

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## VNet-naar-VNet

Het verbinden van een virtueel netwerk met een ander virtueel netwerk (VNet-naar-VNet) lijkt op het verbinden van een VNet met een on-premises locatie. Voor beide connectiviteitstypen wordt een Azure VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden. U kunt zelfs VNet-naar-VNet-communicatie met multi-site-configuraties combineren. Zo kunt u netwerktopologieën maken waarin cross-premises connectiviteit is gecombineerd met connectiviteit tussen virtuele netwerken.

U kunt de volgende VNets verbinden:

- in dezelfde of verschillende regio's
- in dezelfde of verschillende abonnementen 
- in de dezelfde of verschillende implementatiemodellen



![VNet-naar-VNet-verbinding](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")



### Verbindingen tussen implementatiemodellen

Azure heeft momenteel twee implementatiemodellen: klassiek en Resource Manager. Als u eerder enige tijd met Azure hebt gewerkt, hebt u waarschijnlijk Azure-VM's en -rolinstanties in een klassiek VNet. De nieuwere VM's en rolinstanties werken mogelijk in een VNet dat is gemaakt in Resource Manager. U kunt de VNets verbinden zodat de resources in het ene VNet direct met de resources in het andere kunnen communiceren.


### Implementatiemodellen en -methoden

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

### VNet-peering

Zolang de configuratie van het virtuele netwerk voldoet aan bepaalde vereisten, kunt u VNet-peering gebruiken om uw verbinding te maken. Bij VNet-peering wordt geen virtuele netwerkgateway gebruikt. [VNet-peering](../virtual-network/virtual-network-peering-overview.md) bevindt zich momenteel in de previewfase.



## Punt-naar-site

Met een punt-naar-site (P2S)-configuratie kunt u vanuit een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtueel netwerk. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Voor P2S-verbindingen hebt u geen VPN-apparaat of openbaar IP-adres nodig. U kunt de VPN-verbinding maken door deze vanaf de clientcomputer te starten. Dit is een uitstekende oplossing als u uw VNet vanaf een externe locatie, zoals vanaf thuis of een conferentie, wilt verbinden, of wanneer u slechts enkele clients hebt die verbinding moeten maken met een VNet. 


![Punt-naar-site-verbinding](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### Implementatiemodellen en -methoden

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Zie [Technical Overview](../expressroute/expressroute-introduction.md) (Technisch overzicht) voor meer informatie over ExpressRoute.


## Site-naar-site- en ExpressRoute-verbindingen naast elkaar

ExpressRoute is een directe, exclusieve verbinding van uw WAN (niet via het openbare internet) met Microsoft-services, waaronder Azure. Site-naar-site-VPN-verkeer verplaatst zich versleuteld via het openbare internet. De mogelijkheid om site-naar-site-VPN- en ExpressRoute-verbindingen voor hetzelfde virtuele netwerk te configureren, heeft verschillende voordelen.

U kunt een site-naar-site-VPN configureren als een beveiligd failoverpad voor ExpressRoute of site-naar-site-VPN’s gebruiken om verbinding te maken met sites die geen deel uitmaken van uw netwerk, maar zijn verbonden via ExpressRoute. Deze configuratie vereist twee gateways voor hetzelfde virtuele netwerk, één met -GatewayType Vpn en de andere met -GatewayType ExpressRoute.


![Naast elkaar bestaande verbinding](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### Implementatiemodellen en -methoden

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Volgende stappen

Voor meer informatie over VPN-gateways raadpleegt u [Veelgestelde vragen over VPN-gateways](vpn-gateway-vpn-faq.md)

Verbind uw on-premises locatie met een VNet. Zie [Een site-naar-site-verbinding maken](vpn-gateway-howto-site-to-site-resource-manager-portal.md).





 



<!--HONumber=ago16_HO5-->


