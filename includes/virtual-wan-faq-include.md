---
title: bestand opnemen
description: bestand opnemen
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/10/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5cb1360731eeabe4963330210ba6fe090f0e088a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008823"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Wat is het verschil tussen een gateway van een virtueel Azure-netwerk (VPN Gateway) en een Azure Virtual WAN-VPN-gateway?

Virtual WAN biedt grootschalige site-naar-site-connectiviteit en is ontworpen met het oog op doorvoer, schaalbaarheid en gebruiksgemak. CPE-branchapparaten worden automatisch ingericht en verbonden met Azure Virtual WAN. Deze apparaten zijn beschikbaar binnen een groeiend ecosysteem van SD-WAN- en VPN-partners.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Welke apparaatproviders (virtuele WAN-partners) worden bij de introductie ondersteund? 

Op dit moment bieden Citrix en Riverbed ondersteuning voor de volledig geautomatiseerde Virtual WAN-ervaring. In de komende maanden zullen er meer partners worden toegevoegd, zoals Nokia Nuage, Palo Alto en Checkpoint. Zie [Virtual WAN-partners](https://aka.ms/virtualwan) voor meer informatie.

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Moet ik een apparaat van een voorkeurspartner gebruiken?

Nee. U kunt ieder VPN-apparaat gebruiken dat voldoet aan de Preview-vereisten voor IKEv2 IPSec-ondersteuning.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Hoe automatiseren Virtual WAN-partners connectiviteit met Azure Virtual WAN?

Softwarematige oplossingen voor netwerkconnectiviteit beheren hun vertakkingsapparaten doorgaans met behulp van een controller of een knooppunt voor apparaatinrichting. De controller kan Azure API’s gebruiken om de connectiviteit met Azure Virtual WAN te automatiseren. Raadpleeg voor meer informatie over hoe dit werkt [Virtual WAN partner automation](../articles/virtual-wan/virtual-wan-configure-automation-providers.md) (Automatisering voor Virtual WAN-partners).

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Veranderen er bij het gebruik van Virtual WAN bestaande functies voor connectiviteit?   

Er zijn geen wijzigingen in bestaande connectiviteitsfuncties van Azure.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Zijn er nieuwe Resource Manager-resources beschikbaar voor Virtual WAN?
  
Ja, met Virtual WAN worden nieuwe Resource Manager-resources geïntroduceert. Zie voor meer informatie het [Overzicht](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="are-there-any-special-requirements-to-join-the-preview"></a>Zijn er speciale vereisten voor deelname aan de Preview-versie? 

Voordat u een Azure Virtual WAN kunt configureren, dient u eerst uw abonnement voor de Preview-versie te registreren. Als u zich wilt inschrijven, stuurt u een e-mail met uw abonnements-id naar <azurevirtualwan@microsoft.com>. U ontvangt een e-mailbevestiging zodra uw abonnement is geregistreerd. Totdat u een bevestiging heeft ontvangen dat uw abonnement is geregistreerd, kunt u niet in de portal werken met Virtual WAN.

Overwegingen:

* De Preview-versie is beperkt tot enkel openbare Azure-regio’s.
* Per virtuele hub worden maximaal 100 verbindingen ondersteund. Elke verbinding bestaat uit 2 tunnels die zich in een actief/actief-configuratie bevinden. De tunnels eindigen in een Azure Virtual Hub-VPN-gateway.
* Overweeg deze Preview te gebruiken indien:
  * u minder dan 1 Gbps geaggregeerde bandbreedte wilt implementeren per virtuele hub;
  * u een VPN-apparaat hebt dat ondersteuning biedt voor op route gebaseerde configuratie en IKEv2 IPSec-connectiviteit;
  * u kennis wilt maken met SD-WAN en vertakkingsapparaten van de introductiepartners (Riverbed en Citrix);<br>of<br>u vertakking-naar-vertakking- en vertakking-naar-Azure-verbindingen wilt opzetten die gebruikmaken van het configuratiebeheer van uw on-premises-apparaat. (Zelf configureren)

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Wordt wereldwijde VNET-peering met Azure Virtual WAN ondersteund? 

 Nee.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Kunnen VNets op knooppunten die zijn verbonden met een virtuele hub met elkaar communiceren?

Ja. U kunt VNets op verschillende knooppunten die verbonden zijn met een virtuele hub met elkaar peeren. Zie voor meer informatie [Peering op virtueel netwerk](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Kan ik mijn favoriete virtuele netwerkapparaat implementeren en gebruiken (in een NVA-VNet) met Azure Virtual WAN?

Ja, u kunt het VNet van uw favoriete virtuele netwerkapparaat (Network Virtual Appliance, NVA) koppelen aan Azure Virtual WAN, maar hiervoor zijn wel routeringsmogelijkheden in de hub vereist, die pas bij de algemene introductie beschikbaar zijn. Alle knooppunten die zijn verbonden met het NVA-VNet moeten bovendien zijn verbonden met de virtuele hub. 

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Kan een NVA-VNet een virtuele netwerkgateway hebben?

Nee. Het NVA-VNet kan geen virtuele netwerkgateway hebben als deze is verbonden met de virtuele hub. 

### <a name="is-there-support-for-bgp"></a>Is er ondersteuning voor BGP?

Ja, BGP wordt ondersteund. Om ervoor te zorgen dat de routes van een NVA-VNet op de juiste wijze worden geadverteerd, moet BGP op knooppunten worden uitgeschakeld als deze zijn verbonden met een NVA-VNet, die op zijn beurt is verbonden met een virtuele hub. Ook moeten de VNets op het knooppunt worden verbonden met de virtuele hub.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kan ik verkeer beheren met behulp van de UDR in de virtuele hub?

UDR en routeringsfunctionaliteit zijn beschikbaar wanneer Virtual WAN algemeen beschikbaar wordt gemaakt.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Is er informatie over licentieverlening of prijzen beschikbaar voor Virtual WAN?
 
Er worden geen extra kosten berekend. De huidige [tarieven voor Azure VPN-verkeer en uitgaand verkeer](https://azure.microsoft.com/pricing/details/vpn-gateway/) blijven van kracht tijdens de Preview.