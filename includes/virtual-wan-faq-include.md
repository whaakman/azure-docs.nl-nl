---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 03/18/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 931bc26e22db4bbf02a18d4824b9c846f1e66b18
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58190644"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Wat is het verschil tussen een gateway van een virtueel Azure-netwerk (VPN Gateway) en een Azure Virtual WAN-VPN-gateway?

Virtual WAN biedt grootschalige site-naar-site-connectiviteit en is ontworpen met het oog op doorvoer, schaalbaarheid en gebruiksgemak. ExpressRoute en punt-naar-siteconnectiviteit is momenteel in de preview-fase. CPE apparaten autoprovision vertakking en verbinding maken met Azure virtuele WAN. Deze apparaten zijn beschikbaar binnen een groeiend ecosysteem van SD-WAN- en VPN-partners. Zie de [lijst met partners van voorkeur](https://go.microsoft.com/fwlink/p/?linkid=2019615).

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

Ja, BGP wordt ondersteund. Wanneer u een VPN-site maakt, kunt u de BGP-parameters in deze opgeven. Dit wordt impliceren dat alle verbindingen gemaakt in Azure voor die site worden ingeschakeld voor BGP. Bovendien, moeten als u een VNet met een NVA had, en als dit VNet NVA is gekoppeld aan een virtueel WAN hub, om ervoor te zorgen dat de routes van een NVA-VNet op de juiste wijze, worden geadverteerd knooppunten die zijn gekoppeld aan VNet NVA BGP uitschakelen. Deze knooppunt VNets aan de virtuele hub VNet om ervoor te zorgen spoke VNet routes worden doorgegeven wordt ook verbinding maken met on-premises systemen.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kan ik verkeer beheren met behulp van de UDR in de virtuele hub?

Ja, u kunt verkeer naar een VNet sturen met behulp van de routetabel van de virtuele hub.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Is er informatie over licentieverlening of prijzen beschikbaar voor Virtual WAN?
 
Ja. Zie de [prijzenpagina](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="how-do-i-calculate-price-of-a-hub"></a>Hoe ik de prijs van een hub berekend?
 
U moet betalen voor de service in de hub. Bijvoorbeeld, 10 vertakkingen of on-premises apparaten die verbinding maken met Azure virtuele WAN zou kunnen betekenen dat verbinding te maken met VPN-eindpunten in de hub. Hiermee kunnen Stel dat dit VPN van 1 schaaleenheid = 500 Mbps, dit wordt in rekening gebracht op $0.361/ uur. Elke verbinding tot stand is gebracht met $ 0,08/uur. Voor 10-verbindingen, de totale kosten van service/uur zou zijn $0.361 + $. 8 / uur. Gegevens voor het verkeer verlaat azure in rekening gebracht. 

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>Hoe worden nieuwe partners die nog niet zijn vermeld in de startlijst van partners, opgenomen?

Stuur een e-mail naar azurevirtualwan@microsoft.com. Een ideale partner is iemand die een apparaat heeft dat kan worden ingericht voor een IKEv1 of IKEv2 IPSec-verbinding.

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>Wat gebeurt er als een apparaat ik gebruik is niet in de lijst met virtuele WAN-partner? Kan ik nog steeds gebruiken het verbinding maken met Azure virtuele WAN-VPN?

Ja, zolang het apparaat IPsec IKEv1 of IKEv2 ondersteunt. Virtuele WAN-partners automatiseren connectiviteit van het apparaat met Azure VPN-eindpunten. Dit betekent dat automatiseren stappen zoals 'vertakking gegevens uploaden', 'IPSec- en configuratie' en 'connectiviteit'. Omdat uw apparaat niet afkomstig van een virtuele WAN-partnerecosysteem is, moet u het zware werk aan handmatig houdend met de Azure-configuratie en het bijwerken van uw apparaat voor het instellen van IPSec-verbinding. 

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Is het mogelijk om een Azure Virtual WAN te maken met een Resource Manager-sjabloon?

Een eenvoudige configuratie van een virtueel WAN met een hub en een vpnsite kunnen worden gemaakt met een [Azure quickstart-sjabloon](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network). Virtual WAN is voornamelijk een service op basis van REST of Portal.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Is een vertakking-naar-vertakking-verbinding toegestaan in Virtual WAN?

Ja, een vertakking-naar-vertakking-verbinding is in Virtual WAN beschikbaar voor VPN en VPN naar ExpressRoute. Terwijl VPN site-naar-site GA is, zijn ExpressRoute en punt-maar-site momenteel in de preview-fase.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Loopt vertakking-naar-vertakking-verkeer door Azure Virtual WAN?

Ja.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Waarin verschilt Virtual WAN van de bestaande Azure Virtual Network-gateway?

VPN in een Virtual Network-gateway is beperkt tot 30 tunnels. U moet Virtual WAN gebruiken voor grootschalige VPN-verbindingen. Voor vertakkingen kunt u maximaal 1000 verbindingen van 2 Gbps maken in de hub. Dit geldt voor alle regio's behalve de regio West Central. Voor de regio West Central is 20 Gbps beschikbaar. In de toekomst gaan we 20 Gbps uitrollen in andere regio's. Een verbinding is een actief-actief-tunnel van het on-premises VPN-apparaat naar de virtuele hub. Per regio kan er één hub zijn. Dit betekent dat u meer dan 1000 vertakkingen kunt verbinden via de hubs.

### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>Hoe virtuele WAN ondersteunt SD-WAN-apparaten?

Virtuele WAN-partners automatiseren IPSec-verbinding met Azure VPN-eindpunten. Als de virtuele WAN-partner een SD-WAN-provider wordt geïmpliceerd dat de SD-WAN-controller automatiserings- en IPSec-verbinding met Azure VPN-eindpunten beheert. Als de SD-WAN een eigen eindpunt in plaats van de Azure VPN voor alle bedrijfseigen SD-WAN-functionaliteit vereist, kunt u de SD-WAN-eindpunt in een Azure VNet te implementeren en worden gecombineerd met Azure virtuele WAN.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Heeft Virtual WAN vanaf elke locatie ExpressRoute nodig?

Nee, Virtual WAN heeft niet vanaf elke locatie ExpressRoute nodig. Standaard wordt er gebruikgemaakt van IPSec-site-naar-site-verbindingen via internetkoppelingen van het apparaat naar een Azure Virtual WAN-hub. Uw sites kunnen worden verbonden met een providernetwerk met behulp van een ExpressRoute-circuit. Voor sites die zijn verbonden via ExpressRoute in Virtual Hub (in preview), kunnen de sites een verkeersstroom van vertakking naar vertakking hebben tussen VPN en ExpressRoute. 

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Is er een doorvoerlimiet in het netwerk bij gebruik van Azure Virtual WAN?

Het aantal vertakkingen is beperkt tot 1000 verbindingen per hub/regio tot een totaal van 2 G in de hub. De uitzondering is US - west-centraal, met een totaal van 20 Gbps. In de toekomst gaan we 20 Gbps uitrollen in andere regio's.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Kan het on-premises apparaat in Virtual WAN meerde ISP's parallel gebruiken of is er altijd sprake van één VPN-tunnel?

Een op virtuele WAN-VPN-verbinding is altijd een actief / actief-tunnel (voor tolerantie binnen dezelfde hub/regio) via een koppeling beschikbaar op de vertakking. Deze koppeling is mogelijk een ISP-koppeling op de on-premises-vertakking. Virtuele WAN biedt geen eventuele speciale logica voor het instellen van meerdere ISP parallel; failover beheren via Internet-provider op de vertakking is volledig een vertakking gericht netwerkbewerking. Uw favoriete SD-WAN-oplossing kunt u doen Padselectie op de vertakking.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Hoe wordt het verkeer naar de Azure-backbone geleid?

Het verkeer het patroon: ISP-vertakking apparaat > Microsoft edge -> -> Microsoft DC (hub VNet)-Microsoft edge -> > ISP-vertakking apparaat >

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Wat is er met dit model op elke locatie nodig? Alleen een internetverbinding?

Ja. Een verbinding met Internet en een fysiek apparaat die ondersteuning biedt voor IPsec, bij voorkeur van onze geïntegreerde [partners](https://go.microsoft.com/fwlink/p/?linkid=2019615). Optioneel kunt u de configuratie en verbinding met Azure vanaf het apparaat van uw voorkeur handmatig beheren.