---
title: Overzicht van Azure Virtual WAN | Microsoft Docs
description: Meer informatie over virtuele WAN geautomatiseerde schaalbare vertakking op vertakking-connectiviteit, beschikbare regio's en partners.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 03/20/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 6f2f002c5ff08e21741927d07a0facfd09ec0914
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295688"
---
# <a name="what-is-azure-virtual-wan"></a>Wat is Azure Virtual WAN?

Azure virtuele WAN is een netwerkservice, dat geoptimaliseerde en geautomatiseerde vertakking connectiviteit voor, en via Azure biedt. Azure-regio's fungeren als hubs die u kunt verbinding maken met uw vertakkingen. Zodra de filialen zijn verbonden, kunt u gebruikmaken van de Azure-backbone voor het maken van branch-naar-VNet- en vertakking op vertakking-connectiviteit. Zie voor een lijst met partners en de locaties die ondersteuning bieden voor virtuele WAN-VPN, de [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) artikel.

Azure virtuele WAN combineert veel Azure-cloud-connectiviteit-services zoals site-naar-site VPN (algemeen beschikbaar), ExpressRoute (Preview), punt-naar-site gebruiker VPN (Preview) in één operationele interface. Verbinding met Azure vnet's met behulp van virtueel netwerkverbindingen tot stand is gebracht.

![Virtual WAN-diagram](./media/virtual-wan-about/vwangraphic.png)

In dit artikel biedt een snel inzicht krijgt in de verbinding met het netwerk in Azure virtuele WAN. Virtual WAN biedt de volgende voordelen:

* **Geïntegreerde connectiviteitsoplossingen in de hub en spoke:** Site-naar-site-configuratie en connectiviteit tussen on-premises sites en een Azure-hub automatiseren.
* **Geautomatiseerde spoke-installatie en configuratie:** Verbinding maken met uw virtuele netwerken en werkbelastingen naar de Azure-hub naadloos.
* **Intuïtieve voor probleemoplossing:** U kunt de end-to-end-stroom binnen Azure weergeven en vervolgens deze informatie gebruiken om de vereiste actie ondernemen.

## <a name="resources"></a>Virtual WAN-resources

Voor het configureren van een end-to-end virtuele WAN, maakt u de volgende bronnen:

* **virtualWAN:** De resource virtualWAN vertegenwoordigt een virtuele-overlay van uw Azure-netwerk en is een verzameling van meerdere resources. Het bevat koppelingen naar al uw virtuele hubs die u wilt opnemen in het virtuele WAN. Virtual WAN-resources zijn van elkaar geïsoleerd en kunnen geen gemeenschappelijke hub bevatten. Virtuele hubs in Virtual WAN communiceren niet met elkaar. De eigenschap 'Vertakking-vertakking verkeer toestaan' kunt Sites verkeer tussen VPN-sites, evenals VPN met ExpressRoute (momenteel in Preview) ingeschakeld.

* **Hub:** Een virtuele hub is een Microsoft-beheerde virtueel netwerk. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. De hub is de kern van uw netwerk in een regio. Een Azure-regio mag maar één hub bevatten. Als u met de Azure-portal een hub maakt, wordt een VNet en een vpngateway voor de virtuele hub gemaakt.

  Een hubgateway is niet hetzelfde als een gateway voor het virtuele netwerk die u voor ExpressRoute en VPN Gateway gebruikt. Bijvoorbeeld, wanneer u virtuele WAN, maakt u geen een site-naar-site-verbinding van uw on-premises site rechtstreeks naar uw VNet. In plaats daarvan maakt u een site-naar-site-verbinding naar de hub. Het verkeer verloopt altijd via de hubgateway. Dit houdt in dat uw VNet's hun eigen gateway voor het virtuele netwerk niet nodig hebben. In Virtual WAN kunnen uw VNet's eenvoudig schalen via de virtuele hub en de gateway van de virtuele hub.

* **Hub virtuele netwerkverbinding:** De verbindingsbron van de Hub virtueel netwerk wordt gebruikt om de hub naadloos verbinding maken met het virtuele netwerk. Op dit moment kunt u alleen verbinding maken met virtuele netwerken die zich in dezelfde hubregio bevinden.

* **Routetabel hub:**  U kunt maken van een virtuele hub-route en de route van toepassing op de virtuele hub-routetabel. U kunt meerdere routes toepassen op de routetabel van de virtuele hub.

**Aanvullende virtuele WAN-bronnen**

  * **Site:** Deze bron wordt gebruikt voor alleen-site-naar-site-verbindingen. De sitebron is **vpnsite**. Staat uw on-premises VPN-apparaat en de bijbehorende instellingen. Wanneer u met een Virtual WAN-partner werkt, beschikt u over een geïntegreerde oplossing die deze gegevens automatisch naar Azure exporteert.

## <a name="connectivity"></a>Connectiviteit

Virtuele WAN kunt drie soorten connectiviteit: site-naar-site, punt-naar-site (Preview) en ExpressRoute (Preview).

### <a name="s2s"></a>Site-naar-site VPN-verbindingen

![Virtual WAN-diagram](./media/virtual-wan-about/virtualwan.png)

Wanneer u een virtuele WAN-site-naar-site-verbinding maakt, kunt u werken met een partner beschikbaar. Als u niet wilt gebruiken van een partner, kunt u de verbinding handmatig configureren. Zie voor meer informatie, [maken van een site-naar-site-verbinding met virtuele WAN](virtual-wan-site-to-site-portal.md).

#### <a name="s2spartner"></a>Virtuele WAN-partner-werkstroom

Wanneer u met een virtueel WAN-partner werkt, wordt de werkstroom is:

1. De controller (VPN/SD-WAN) voor filiaalapparaten wordt geverifieerd voor het exporteren van locatiegegevens naar Azure met behulp van een [Azure-service-principal](../active-directory/develop/howto-create-service-principal-portal.md).
2. De controller (VPN/SD-WAN) voor filiaalapparaten ontvangt de Azure-verbindingsconfiguratie en werkt het lokale apparaat bij. Op deze manier wordt het downloaden van de configuratie, het bewerken en het bijwerken van het on-premises VPN-apparaat geautomatiseerd.
3. Wanneer het apparaat eenmaal de juiste Azure-configuratie heeft, wordt een verbinding tussen de locaties (twee actieve tunnels) en Azure WAN tot stand gebracht. Azure ondersteunt zowel IKEv1 als IKEv2. BGP is optioneel.

#### <a name="partners"></a>Partners voor site-naar-site virtueel WAN-verbindingen

Zie voor een lijst van de beschikbare partners en locaties, de [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) artikel.

### <a name="p2s"></a>Punt-naar-site VPN-verbindingen (Preview)

Met een point-to-site-verbinding (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele hub. Een P2S-verbinding wordt tot stand gebracht door deze te starten vanaf de clientcomputer. Deze oplossing is handig voor telewerkers die verbinding willen maken vanaf een externe locatie, zoals thuis of een conferentie. P2S-VPN is ook een uitstekende oplossing in plaats van een S2S-VPN wanneer u maar een paar clients hebt die verbinding moeten maken.

De verbinding wilt maken, Zie [maken van een punt-naar-site-verbinding met virtuele WAN](virtual-wan-point-to-site-portal.md).

### <a name="er"></a>ExpressRoute-verbindingen (preview)

Met ExpressRoute kunt u verbinding maken met on-premises netwerk naar Azure via een persoonlijke verbinding. De verbinding wilt maken, Zie [maken van een ExpressRoute-verbinding met virtuele WAN](virtual-wan-expressroute-portal.md).

## <a name="locations"></a>locaties

Locatie-informatie, Zie de [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) artikel.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen

[Maken van een site-naar-site-verbinding met virtuele WAN](virtual-wan-site-to-site-portal.md)
