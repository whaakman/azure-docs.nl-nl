---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/19/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 98ea4d78a473123708be6e371587252acad6ffcd
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554742"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Wat is het verschil tussen een gateway van een virtueel Azure-netwerk (VPN Gateway) en een Azure Virtual WAN-VPN-gateway?

Virtual WAN biedt grootschalige site-naar-site-connectiviteit en is ontworpen met het oog op doorvoer, schaalbaarheid en gebruiksgemak. ExpressRoute en punt-naar-siteconnectiviteit is momenteel in de preview-fase. CPE-branchapparaten worden automatisch ingericht en verbonden met Azure Virtual WAN. Deze apparaten zijn beschikbaar binnen een groeiend ecosysteem van SD-WAN- en VPN-partners. Zie de [lijst met partners van voorkeur](https://go.microsoft.com/fwlink/p/?linkid=2019615).

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Welke apparaatproviders (virtuele WAN-partners) worden bij de introductie ondersteund? 

Veel partners bieden momenteel ondersteuning voor de volledig geautomatiseerde Virtual WAN-ervaring. Zie [Virtual WAN-partners](https://go.microsoft.com/fwlink/p/?linkid=2019615) voor meer informatie. 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>Wat zijn de stappen voor automatisering voor Virtual WAN-partners?

Zie [Automatisering voor Virtual WAN-partners](../articles/virtual-wan/virtual-wan-configure-automation-providers.md) voor meer informatie.

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Moet ik een apparaat van een voorkeurspartner gebruiken?

Nee. U kunt ieder VPN-apparaat gebruiken dat voldoet aan de Azure-vereisten voor IKEv2/IKEv1 IPSec-ondersteuning.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Hoe automatiseren Virtual WAN-partners connectiviteit met Azure Virtual WAN?

Softwarematige oplossingen voor netwerkconnectiviteit beheren hun vertakkingsapparaten doorgaans met behulp van een controller of een knooppunt voor apparaatinrichting. De controller kan Azure API’s gebruiken om de connectiviteit met Azure Virtual WAN te automatiseren. Zie Automatisering voor Virtual WAN-partners voor meer informatie.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Veranderen er bij het gebruik van Virtual WAN bestaande functies voor connectiviteit?   

Er zijn geen wijzigingen in bestaande connectiviteitsfuncties van Azure.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Zijn er nieuwe Resource Manager-resources beschikbaar voor Virtual WAN?
  
Ja, met Virtual WAN worden nieuwe Resource Manager-resources geïntroduceert. Zie voor meer informatie het [Overzicht](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>Hoeveel VPN-apparaten kunnen verbinding maken met één hub?

Per virtuele hub worden maximaal 1000 verbindingen ondersteund. Elke verbinding bestaat uit twee tunnels die zich in een actief/actief-configuratie bevinden. De tunnels eindigen in een Azure Virtual Hub-VPN-gateway.

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>Kan het on-premises VPN-apparaat verbinding maken met meerdere hubs?

Ja. In het begin loopt de verkeersstroom van het on-premises apparaat naar de dichtstbijzijnde Microsoft-edge en vervolgens naar de virtuele hub.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Wordt wereldwijde VNET-peering met Azure Virtual WAN ondersteund? 

 Nee.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Kunnen VNets op knooppunten die zijn verbonden met een virtuele hub met elkaar communiceren?

Ja. Ster-VNets kunnen rechtstreeks communiceren via Virtual Network-peering. We ondersteunen echter geen VNets die transitief communiceren via de hub. Zie voor meer informatie [Peering op virtueel netwerk](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Kan ik mijn favoriete virtuele netwerkapparaat implementeren en gebruiken (in een NVA-VNet) met Azure Virtual WAN?

Ja, u kunt uw favoriete virtuele netwerkapparaat (NVA)-VNet verbinden met de Azure Virtual WAN. Verbind eerst het NVA-VNet naar de hub met een Hub Virtual Network-verbinding. Maak vervolgens een virtuele-hubroute met een volgende hop die naar het virtuele apparaat wijst. U kunt meerdere routes toepassen op de routetabel van de virtuele hub. Eventuele spaken die verbonden zijn met het NVA-VNet moeten aanvullend worden verbonden met de virtuele hub om ervoor te zorgen dat de VNet-spaakroutes naar on-premises systemen worden doorgevoerd.

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Kan een NVA-VNet een virtuele netwerkgateway hebben?

Nee. Het NVA-VNet kan geen virtuele netwerkgateway hebben als deze is verbonden met de virtuele hub. 

### <a name="is-there-support-for-bgp"></a>Is er ondersteuning voor BGP?

Ja, BGP wordt ondersteund. Om ervoor te zorgen dat de routes van een NVA-VNet op de juiste wijze worden geadverteerd, moet BGP op knooppunten worden uitgeschakeld als deze zijn verbonden met een NVA-VNet dat op zijn beurt is verbonden met een virtuele hub. Verbind vervolgens de spaak-VNets met de virtuele hub om ervoor te zorgen dat spaak-VNet-routes naar on-premises systemen worden doorgevoerd.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kan ik verkeer beheren met behulp van de UDR in de virtuele hub?

Ja, u kunt verkeer naar een VNet sturen met behulp van de routetabel van de virtuele hub.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Is er informatie over licentieverlening of prijzen beschikbaar voor Virtual WAN?
 
Ja. Zie de [prijzenpagina](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>Hoe worden nieuwe partners die nog niet zijn vermeld in de startlijst van partners, opgenomen?

Stuur een e-mail naar azurevirtualwan@microsoft.com. Een ideale partner is iemand die een apparaat heeft dat kan worden ingericht voor een IKEv1 of IKEv2 IPSec-verbinding.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Is het mogelijk om een Azure Virtual WAN te maken met een Resource Manager-sjabloon?

Een eenvoudige configuratie van één Virtual WAN met één hub en één vpnsite kunnen met een [Azure Quick Start-sjabloon](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network) worden gemaakt. Virtual WAN is voornamelijk een service op basis van REST of Portal.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Is een vertakking-naar-vertakking-verbinding toegestaan in Virtual WAN?

Ja, een vertakking-naar-vertakking-verbinding is in Virtual WAN beschikbaar voor VPN en VPN naar ExpressRoute. Terwijl VPN site-naar-site GA is, zijn ExpressRoute en punt-maar-site momenteel in de preview-fase.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Loopt vertakking-naar-vertakking-verkeer door Azure Virtual WAN?

Ja.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Waarin verschilt Virtual WAN van de bestaande Azure Virtual Network-gateway?

VPN in een Virtual Network-gateway is beperkt tot 30 tunnels. U moet Virtual WAN gebruiken voor grootschalige VPN-verbindingen. Voor vertakkingen kunt u maximaal 1000 verbindingen van 2 Gbps maken in de hub. Dit geldt voor alle regio's behalve de regio West Central. Voor de regio West Central is 20 Gbps beschikbaar. In de toekomst gaan we 20 Gbps uitrollen in andere regio's. Een verbinding is een actief-actief-tunnel van het on-premises VPN-apparaat naar de virtuele hub. Per regio kan er één hub zijn. Dit betekent dat u meer dan 1000 vertakkingen kunt verbinden via de hubs.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Heeft Virtual WAN vanaf elke locatie ExpressRoute nodig?

Nee, Virtual WAN heeft niet vanaf elke locatie ExpressRoute nodig. Standaard wordt er gebruikgemaakt van IPSec-site-naar-site-verbindingen via internetkoppelingen van het apparaat naar een Azure Virtual WAN-hub. Uw sites kunnen worden verbonden met een providernetwerk met behulp van een ExpressRoute-circuit. Voor sites die zijn verbonden via ExpressRoute in Virtual Hub (in preview), kunnen de sites een verkeersstroom van vertakking naar vertakking hebben tussen VPN en ExpressRoute. 

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Is er een doorvoerlimiet in het netwerk bij gebruik van Azure Virtual WAN?

Het aantal vertakkingen is beperkt tot 1000 verbindingen per hub/regio tot een totaal van 2 G in de hub. De uitzondering is US - west-centraal, met een totaal van 20 Gbps. In de toekomst gaan we 20 Gbps uitrollen in andere regio's.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Kan het on-premises apparaat in Virtual WAN meerde ISP's parallel gebruiken of is er altijd sprake van één VPN-tunnel?

Ja, u kunt active-active-tunnels gebruiken (2 tunnels = 1 Azure Virtual WAN-verbinding) vanaf één vertakking, afhankelijk van het apparaat in de vertakking.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Hoe wordt het verkeer naar de Azure-backbone geleid?

Het verkeer volgt het volgende patroon: vertakkingsapparaat->ISP->Microsoft Edge->Microsoft DC->Microsoft Edge->ISP->vertakkingsapparaat

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Wat is er met dit model op elke locatie nodig? Alleen een internetverbinding?

Ja. Een internetverbinding en een fysiek apparaat, bij voorkeur van onze geïntegreerde [partners](https://go.microsoft.com/fwlink/p/?linkid=2019615). Optioneel kunt u de configuratie en verbinding met Azure vanaf het apparaat van uw voorkeur handmatig beheren.
