---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/18/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d059cab5668eef8d4dafc1442ca9749a7dcf8c9d
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39162511"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Wat is het verschil tussen een gateway van een virtueel Azure-netwerk (VPN Gateway) en een Azure Virtual WAN-VPN-gateway?

Virtual WAN biedt grootschalige site-naar-site-connectiviteit en is ontworpen met het oog op doorvoer, schaalbaarheid en gebruiksgemak. CPE-branchapparaten worden automatisch ingericht en verbonden met Azure Virtual WAN. Deze apparaten zijn beschikbaar binnen een groeiend ecosysteem van SD-WAN- en VPN-partners.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Welke apparaatproviders (virtuele WAN-partners) worden bij de introductie ondersteund? 

Op dit moment bieden Citrix en Riverbed ondersteuning voor de volledig geautomatiseerde Virtual WAN-ervaring. Zie [Virtual WAN-partners](https://aka.ms/virtualwan) voor meer informatie.

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Moet ik een apparaat van een voorkeurspartner gebruiken?

Nee. U kunt ieder VPN-apparaat gebruiken dat voldoet aan de Preview-vereisten voor IKEv2 IPSec-ondersteuning.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Hoe automatiseren Virtual WAN-partners connectiviteit met Azure Virtual WAN?

Softwarematige oplossingen voor netwerkconnectiviteit beheren hun vertakkingsapparaten doorgaans met behulp van een controller of een knooppunt voor apparaatinrichting. De controller kan Azure API’s gebruiken om de connectiviteit met Azure Virtual WAN te automatiseren. Raadpleeg [Virtual WAN partner automation](../articles/virtual-wan/virtual-wan-configure-automation-providers.md) (Automatisering voor Virtual WAN-partners) voor meer informatie.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Veranderen er bij het gebruik van Virtual WAN bestaande functies voor connectiviteit?   

Er zijn geen wijzigingen in bestaande connectiviteitsfuncties van Azure.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Zijn er nieuwe Resource Manager-resources beschikbaar voor Virtual WAN?
  
Ja, met Virtual WAN worden nieuwe Resource Manager-resources geïntroduceert. Zie voor meer informatie het [Overzicht](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="are-there-any-special-requirements-to-join-the-preview"></a>Zijn er speciale vereisten voor deelname aan de Preview-versie? 

Voordat u een Azure Virtual WAN kunt configureren, dient u eerst uw abonnement voor de Preview-versie te registreren. Als u zich wilt inschrijven, stuurt u een e-mail met uw abonnements-id naar <azurevirtualwan@microsoft.com>. U ontvangt een e-mailbevestiging zodra uw abonnement is geregistreerd. Totdat u een bevestiging heeft ontvangen dat uw abonnement is geregistreerd, kunt u niet in de portal werken met Virtual WAN.

Overwegingen:

* De Preview-versie is beperkt tot enkel openbare Azure-regio’s.
* Per virtuele hub worden maximaal 100 verbindingen ondersteund. Elke verbinding bestaat uit twee tunnels die zich in een actief/actief-configuratie bevinden. De tunnels eindigen in een Azure Virtual Hub-VPN-gateway.
* Overweeg deze Preview te gebruiken indien:
  * u minder dan 1 Gbps geaggregeerde bandbreedte wilt implementeren per virtuele hub;
  * u een VPN-apparaat hebt dat ondersteuning biedt voor op route gebaseerde configuratie en IKEv2 IPSec-connectiviteit;
  * u kennis wilt maken met SD-WAN en vertakkingsapparaten van de introductiepartners (Riverbed en Citrix);<br>of<br>u vertakking-naar-vertakking- en vertakking-naar-Azure-verbindingen wilt opzetten die gebruikmaken van het configuratiebeheer van uw on-premises-apparaat. (Zelf configureren)

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Wordt wereldwijde VNET-peering met Azure Virtual WAN ondersteund? 

 Nee.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Kunnen VNets op knooppunten die zijn verbonden met een virtuele hub met elkaar communiceren?

Ja. U kunt VNets op verschillende knooppunten die verbonden zijn met een virtuele hub met elkaar peeren. Zie voor meer informatie [Peering op virtueel netwerk](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Kan ik mijn favoriete virtuele netwerkapparaat implementeren en gebruiken (in een NVA-VNet) met Azure Virtual WAN?

Ja, u kunt het VNet van uw favoriete virtuele netwerkapparaat (Network Virtual Appliance, NVA) koppelen aan Azure Virtual WAN, maar hiervoor zijn wel routeringsmogelijkheden in de hub vereist, die we wel gepland hebben, maar nog niet beschikbaar zijn. Alle knooppunten die zijn verbonden met het NVA-VNet moeten bovendien zijn verbonden met de virtuele hub. 

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Kan een NVA-VNet een virtuele netwerkgateway hebben?

Nee. Het NVA-VNet kan geen virtuele netwerkgateway hebben als deze is verbonden met de virtuele hub. 

### <a name="is-there-support-for-bgp"></a>Is er ondersteuning voor BGP?

Ja, BGP wordt ondersteund. Om ervoor te zorgen dat de routes van een NVA-VNet op de juiste wijze worden geadverteerd, moet BGP op knooppunten worden uitgeschakeld als deze zijn verbonden met een NVA-VNet die op zijn beurt is verbonden met een virtuele hub. Ook moeten de VNets op het knooppunt worden verbonden met de virtuele hub.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kan ik verkeer beheren met behulp van de UDR in de virtuele hub?

Dit staat op onze planning. Kom regelmatig terug voor het laatste nieuws!

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Is er informatie over licentieverlening of prijzen beschikbaar voor Virtual WAN?
 
Tijdens een preview worden er geen extra kosten berekend. De huidige [tarieven voor Azure VPN-verkeer en uitgaand verkeer](https://azure.microsoft.com/pricing/details/vpn-gateway/) blijven van kracht tijdens de Preview.

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>Hoe worden nieuwe partners die nog niet zijn vermeld in de startlijst van partners, opgenomen?

Stuur een e-mail naar azurevirtualwan@microsoft.com. Een ideale partner is iemand die een apparaat heeft dat kan worden ingericht voor een IKEv2 IPSec-verbinding.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Is het mogelijk om een Azure Virtual WAN te maken met een Resource Manager-sjabloon?

Daar werken we aan. Op dit moment werkt de service met REST en Portal.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Is een vertakking-naar-vertakking-verbinding toegestaan in Virtual WAN?

Ja, een vertakking-naar-vertakking-verbinding is beschikbaar in Virtual WAN.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Loopt vertakking-naar-vertakking-verkeer door Azure Virtual WAN?

Ja.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Waarin verschilt Virtual WAN van de bestaande Azure Virtual Network-gateway?

VPN in een Virtual Network-gateway is beperkt tot 30 tunnels. U moet Virtual WAN gebruiken voor grootschalige VPN-verbindingen. In de openbare preview is dit beperkt tot 100 vertakkingsverbindingen met 1 Gbps in de hub.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Heeft Virtual WAN vanaf elke locatie ExpressRoute nodig?

Nee, Virtual WAN heeft niet vanaf elke locatie ExpressRoute nodig. Standaard wordt er gebruikgemaakt van IPSec-site-naar-site-verbindingen via internetkoppelingen van het apparaat naar een Azure Virtual WAN-hub.

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Is er een doorvoerlimiet in het netwerk bij gebruik van Azure Virtual WAN?

In de openbare preview is het aantal vertakkingen beperkt tot 100 verbindingen per hub/regio en tot een totaal van 1 G in de hub.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Kan het on-premises apparaat in Virtual WAN meerde ISP's parallel gebruiken of is er altijd sprake van één VPN-tunnel?

Ja, u kunt active-active-tunnels gebruiken (2 tunnels = 1 Azure Virtual WAN-verbinding) vanaf één vertakking, afhankelijk van het apparaat in de vertakking.

### <a name="how-is-traffic-is-routed-on-the-azure-backbone"></a>Hoe wordt het verkeer naar de Azure-backbone geleid?

Het verkeer volgt het volgende patroon: vertakkingsapparaat->ISP->Microsoft Edge->Microsoft DC->Microsoft Edge->ISP->vertakkingsapparaat.

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Wat is er met dit model op elke locatie nodig? Alleen een internetverbinding?

Ja. Een internetverbinding en een fysiek apparaat, bij voorkeur van onze geïntegreerde partners. Tenzij u handmatig de configuratie en verbinding met Azure vanaf het apparaat van uw voorkeur wilt beheren.