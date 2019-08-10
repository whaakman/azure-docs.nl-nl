---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 08/06/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8a4bbe92cc1b34801abffa1e905d873d4382025a
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912346"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Wat is het verschil tussen een gateway van een virtueel Azure-netwerk (VPN Gateway) en een Azure Virtual WAN-VPN-gateway?

Virtual WAN biedt grootschalige site-naar-site-connectiviteit en is ontworpen met het oog op doorvoer, schaalbaarheid en gebruiksgemak. ExpressRoute voor Virtual WAN Connectivity is momenteel beschikbaar als preview-versie. CPE Branch-apparaten automatisch inrichten en verbinding maken met een virtueel WAN van Azure. Deze apparaten zijn beschikbaar binnen een groeiend ecosysteem van SD-WAN- en VPN-partners. Zie de [lijst met partners van voorkeur](https://go.microsoft.com/fwlink/p/?linkid=2019615).

### <a name="what-is-a-branch-connection-to-azure-virtual-wan"></a>Wat is een vertakkings verbinding met Azure Virtual WAN?

Een verbinding van een vertakkings apparaat in een virtueel WAN van Azure, bestaande uit twee actieve/actieve IPsec-tunnels.

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

Maxi maal 1.000 verbindingen worden ondersteund per virtuele hub. Elke verbinding bestaat uit twee tunnels die zich in een actief/actief-configuratie bevinden. De tunnels eindigen in een Azure Virtual Hub-VPN-gateway.

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

Ja, BGP wordt ondersteund. Wanneer u een VPN-site maakt, kunt u hier de BGP-para meters opgeven. Dit betekent dat alle verbindingen die zijn gemaakt in azure voor die site worden ingeschakeld voor BGP. Bovendien, als u een VNet met een NVA had en dit NVA VNet is gekoppeld aan een virtuele WAN-hub, om ervoor te zorgen dat routes van een NVA VNet op de juiste wijze worden geadverteerd, moeten spokes die zijn gekoppeld aan NVA VNet het BGP uitschakelen. Verbind deze spoke-VNets ook met het virtuele hub VNet om ervoor te zorgen dat spoke VNet-routes worden door gegeven aan on-premises systemen.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kan ik verkeer beheren met behulp van de UDR in de virtuele hub?

Ja, u kunt verkeer naar een VNet sturen met behulp van de routetabel van de virtuele hub.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Is er informatie over licentieverlening of prijzen beschikbaar voor Virtual WAN?
 
Ja. Zie de [prijzenpagina](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="how-do-i-calculate-price-of-a-hub"></a>De prijs van een hub Hoe kan ik berekenen?
 
U betaalt voor de service in de hub. Bijvoorbeeld: 10 filialen of on-premises apparaten die verbinding moeten maken met Azure Virtual WAN, betekent dat er verbinding wordt gemaakt met VPN-eind punten in de hub. Dit is een VPN van 1 schaal eenheid = 500 Mbps. dit wordt in rekening gebracht op $0.361/uur. Elke verbinding wordt in rekening gebracht op $0.08/uur. Voor 10 verbindingen is de totale kosten van de service/HR $0,361 + $. 8/ afdeling. Gegevens kosten voor verkeer waardoor Azure van toepassing is. 

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>Hoe worden nieuwe partners die nog niet zijn vermeld in de startlijst van partners, opgenomen?

Stuur een e-mail naar azurevirtualwan@microsoft.com. Een ideale partner is iemand die een apparaat heeft dat kan worden ingericht voor een IKEv1 of IKEv2 IPSec-verbinding.

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>Wat gebeurt er als een apparaat dat ik gebruik niet voor komt in de lijst met virtuele WAN-partners? Kan ik deze nog steeds gebruiken om verbinding te maken met Azure Virtual WAN VPN?

Ja als het apparaat IPsec IKEv1 of IKEv2 ondersteunt. Virtuele WAN-partners automatiseren de connectiviteit van het apparaat tot Azure VPN-eind punten. Dit impliceert de automatiserings stappen zoals ' vertakkings gegevens uploaden ', ' IPsec en configuratie ' en ' connectiviteit '. Omdat uw apparaat niet afkomstig is van een virtueel WAN-partner ecosysteem, moet u het zware werk van de Azure-configuratie hand matig opheffen en uw apparaat bijwerken om IPsec-verbindingen in te stellen. 

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Is het mogelijk om een Azure Virtual WAN te maken met een Resource Manager-sjabloon?

Een eenvoudige configuratie van één virtueel WAN met één hub en één vpnsite kan worden gemaakt met behulp van een [Azure Quick](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network)start-sjabloon. Virtual WAN is voornamelijk een service op basis van REST of Portal.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Is een vertakking-naar-vertakking-verbinding toegestaan in Virtual WAN?

Ja, een vertakking-naar-vertakking-verbinding is in Virtual WAN beschikbaar voor VPN en VPN naar ExpressRoute. Terwijl VPN-site-naar-site GA is, is ExpressRoute momenteel beschikbaar als preview-versie.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Gaat vertakkings-naar-vertakking verkeer via Azure Virtual WAN?

Ja.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Waarin verschilt Virtual WAN van de bestaande Azure Virtual Network-gateway?

VPN in een Virtual Network-gateway is beperkt tot 30 tunnels. U moet Virtual WAN gebruiken voor grootschalige VPN-verbindingen. U kunt Maxi maal 1.000 vertakkings verbindingen maken met 20 Gbps in de hub voor alle regio's. Een verbinding is een actief-actief-tunnel van het on-premises VPN-apparaat naar de virtuele hub. U kunt één hub per regio hebben, wat betekent dat u meer dan 1.000 vertakkingen in hubs kunt verbinden.

### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>Hoe wordt virtuele WAN ondersteunt SD-WAN-apparaten?

Virtuele WAN-partners automatiseren IPsec-verbinding met Azure VPN-eind punten. Als de virtuele WAN-partner een SD-WAN-provider is, is het geïmpliceerd dat de SD-WAN-controller de automatiserings-en IPsec-verbinding beheert met Azure VPN-eind punten. Als het SD-WAN-apparaat een eigen eind punt in plaats van Azure VPN nodig heeft voor een eigen SD-WAN-functionaliteit, kunt u het SD-WAN-eind punt in een Azure-VNet implementeren en samen met Azure Virtual WAN bestaan.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Heeft Virtual WAN vanaf elke locatie ExpressRoute nodig?

Nee, Virtual WAN heeft niet vanaf elke locatie ExpressRoute nodig. Standaard wordt er gebruikgemaakt van IPSec-site-naar-site-verbindingen via internetkoppelingen van het apparaat naar een Azure Virtual WAN-hub. Uw sites kunnen worden verbonden met een providernetwerk met behulp van een ExpressRoute-circuit. Voor sites die zijn verbonden via ExpressRoute in Virtual Hub (in preview), kunnen de sites een verkeersstroom van vertakking naar vertakking hebben tussen VPN en ExpressRoute. 

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Is er een doorvoerlimiet in het netwerk bij gebruik van Azure Virtual WAN?

Het aantal vertakkingen is beperkt tot 1000 verbindingen per hub/regio en in totaal 20 Gbps in de hub.

### <a name="i-dont-see-the-20-gbps-setting-for-the-virtual-hub-in-the-portal-how-do-i-configure-that"></a>De instelling van 20 Gbps wordt niet weer geven voor de virtuele hub in de portal. Dat Hoe kan ik configureren?

Op dit moment kunt u de gateway schaal eenheden voor 20 Gbps configureren met behulp van de cmdlet [Update-AzVpnGateway](https://docs.microsoft.com/powershell/module/az.network/update-azvpngateway) . Deze instelling bevindt zich in het schema dat beschikbaar is in de portal.

### <a name="how-many-vpn-connections-does-a-virtual-wan-hub-support"></a>Hoeveel VPN-verbindingen ondersteunt een virtuele WAN-hub?

Een virtuele WAN-hub van Azure kan Maxi maal 1.000 S2S-verbindingen en 10.000 P2S-verbindingen tegelijk ondersteunen.

### <a name="what-is-the-total-vpn-throughput-of-a-vpn-tunnel-and-a-connection"></a>Wat is de totale VPN-door Voer van een VPN-tunnel en een verbinding?

De totale VPN-door Voer van een hub is Maxi maal 20 Gbps op basis van de gekozen schaal eenheid. De door Voer wordt gedeeld door alle bestaande verbindingen.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Kan het on-premises apparaat in Virtual WAN meerde ISP's parallel gebruiken of is er altijd sprake van één VPN-tunnel?

Een verbinding die wordt weer in virtuele WAN VPN is altijd een actief/actief-tunnel (voor tolerantie binnen dezelfde hub/regio) met behulp van een koppeling die beschikbaar is op de vertakking. Deze koppeling kan een ISP-koppeling zijn op de on-premises vertakking. Virtuele WAN biedt geen speciale logica voor het parallel instellen van meerdere ISP'S. het beheren van failover in de hele ISP in de vertakking is volledig een vertakkings gerichte netwerk bewerking. U kunt uw favoriete SD-WAN-oplossing gebruiken om het pad naar de vertakking te selecteren.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Hoe wordt het verkeer naar de Azure-backbone geleid?

Het verkeer volgt het patroon: vertakking apparaat-> ISP-> micro soft Edge-> micro soft DC (hub VNet)-> micro soft Edge-> ISP->-vertakkings apparaat

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Wat is er met dit model op elke locatie nodig? Alleen een internetverbinding?

Ja. Een Internet verbinding en een fysiek apparaat die IPsec ondersteunt, bij voor keur van onze geïntegreerde [partners](https://go.microsoft.com/fwlink/p/?linkid=2019615). Optioneel kunt u de configuratie en verbinding met Azure vanaf het apparaat van uw voorkeur handmatig beheren.
