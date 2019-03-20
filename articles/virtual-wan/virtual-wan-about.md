---
title: Overzicht van Azure Virtual WAN | Microsoft Docs
description: Meer informatie over virtuele WAN geautomatiseerde schaalbare vertakking op vertakking-connectiviteit, beschikbare regio's en partners.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 03/19/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 5c6e69e05eaa036e140d7275b4e66930a3e5be7a
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225294"
---
# <a name="what-is-azure-virtual-wan"></a>Wat is Azure Virtual WAN?

Azure virtuele WAN is een netwerkservice, dat geoptimaliseerde en geautomatiseerde vertakking connectiviteit voor, en via Azure biedt. Azure-regio's fungeren als hubs die u kunt verbinding maken met uw vertakkingen. Zodra de filialen zijn verbonden, kunt u gebruikmaken van de Azure-backbone voor het maken van branch-naar-VNet- en vertakking op vertakking-connectiviteit.

Azure virtuele WAN combineert veel Azure-cloud-connectiviteit-services zoals Site-naar-Site VPN (algemeen beschikbaar), ExpressRoute (Preview), punt-naar-Site gebruiker VPN (Preview) in één operationele interface. Verbinding met Azure vnet's tot stand is gebracht met behulp van de virtuele netwerkverbindingen.

![Virtual WAN-diagram](./media/virtual-wan-about/vwangraphic.png)

In dit artikel biedt een snel inzicht krijgt in de verbinding met het netwerk in Azure virtuele WAN. Virtual WAN biedt de volgende voordelen:

* **Geïntegreerde connectiviteitsoplossingen in de hub en spoke:** Site-naar-Site-configuratie en connectiviteit tussen on-premises sites en een Azure-hub automatiseren.
* **Geautomatiseerde spoke-installatie en configuratie:** Verbinding maken met uw virtuele netwerken en werkbelastingen naar de Azure-hub naadloos.
* **Intuïtieve voor probleemoplossing:** U kunt zien van de end-to-end-stroom binnen Azure en deze informatie gebruiken om de vereiste actie ondernemen.

## <a name="partner-region"></a>Partners en -locaties

Zie voor meer informatie de [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) artikel.

### <a name="partner"></a>Partners

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

### <a name="locations"></a>locaties

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="s2s"></a>Site-naar-site-verbindingen

![Virtual WAN-diagram](./media/virtual-wan-about/virtualwan.png)

Als u een verbinding tussen de verschillende locaties wilt maken met Virtual WAN, kunt u dat doen via een [Virtual WAN-partner](virtual-wan-locations-partners.md) of de verbinding handmatig tot stand brengen.

### <a name="s2spartner"></a>Partner-werkstroom

Wanneer u met een virtueel WAN-partner werkt, wordt de werkstroom is:

1. De controller (VPN/SD-WAN) voor filiaalapparaten wordt geverifieerd voor het exporteren van locatiegegevens naar Azure met behulp van een [Azure-service-principal](../active-directory/develop/howto-create-service-principal-portal.md).
2. De controller (VPN/SD-WAN) voor filiaalapparaten ontvangt de Azure-verbindingsconfiguratie en werkt het lokale apparaat bij. Op deze manier wordt het downloaden van de configuratie, het bewerken en het bijwerken van het on-premises VPN-apparaat geautomatiseerd.
3. Wanneer het apparaat eenmaal de juiste Azure-configuratie heeft, wordt een verbinding tussen de locaties (twee actieve tunnels) en Azure WAN tot stand gebracht. Azure ondersteunt zowel IKEv1 als IKEv2. BGP is optioneel.

Als u niet wilt gebruiken van een partner, kunt u de verbinding handmatig configureren, Zie [maken van een Site-naar-Site-verbinding met virtuele WAN](virtual-wan-site-to-site-portal.md).

## <a name="p2s"></a>Punt-naar-site-verbindingen (preview)

Met een point-to-site-verbinding (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele hub. Een P2S-verbinding wordt tot stand gebracht door deze te starten vanaf de clientcomputer. Deze oplossing is handig voor telewerkers die verbinding willen maken vanaf een externe locatie, zoals thuis of een conferentie. P2S-VPN is ook een uitstekende oplossing in plaats van een S2S-VPN wanneer u maar een paar clients hebt die verbinding moeten maken.

Raadpleeg [Een site-naar-site-verbinding maken met Virtual WAN](virtual-wan-point-to-site-portal.md) als u de verbinding handmatig tot stand wilt brengen.

## <a name="er"></a>ExpressRoute-verbindingen (preview)

Raadpleeg [Een ExpressRoute-verbinding maken met Virtual WAN](virtual-wan-expressroute-portal.md) als u de verbinding handmatig tot stand wilt brengen.

## <a name="resources"></a>Virtual WAN-resources

Als u een end-to-end virtuele WAN wilt configureren, maakt u de volgende resources:

* **virtualWAN:** De resource virtualWAN vertegenwoordigt een virtuele-overlay van uw Azure-netwerk en is een verzameling van meerdere resources. Het bevat koppelingen naar al uw virtuele hubs die u wilt opnemen in het virtuele WAN. Virtual WAN-resources zijn van elkaar geïsoleerd en kunnen geen gemeenschappelijke hub bevatten. Virtuele hubs in Virtual WAN communiceren niet met elkaar. De eigenschap 'verkeer tussen filialen toestaan' maakt verkeer mogelijk tussen VPN-sites, evenals tussen VPN-sites en sites waarop ExpressRoute is ingeschakeld. Houd er rekening mee dat ExpressRoute in Azure Virtual WAN momenteel alleen als preview beschikbaar is.

* **Site:** De sitebron bekend als vpnsite vertegenwoordigt uw on-premises VPN-apparaat en de bijbehorende instellingen. Wanneer u met een Virtual WAN-partner werkt, beschikt u over een geïntegreerde oplossing die deze gegevens automatisch naar Azure exporteert.

* **Hub:** Een virtuele hub is een Microsoft-beheerde virtueel netwerk. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. De hub is de kern van uw netwerk in een regio. Een Azure-regio mag maar één hub bevatten. Als u met de Azure-portal een hub maakt, wordt een VNet en een vpngateway voor de virtuele hub gemaakt.

  Een hubgateway is niet hetzelfde als een gateway voor het virtuele netwerk die u voor ExpressRoute en VPN Gateway gebruikt. Wanneer u Virtual WAN gebruikt, maakt u bijvoorbeeld geen interlokale verbinding vanaf uw on-premises locatie rechtstreeks naar uw VNet. In plaats daarvan maakt u een interlokale verbinding naar de hub. Het verkeer verloopt altijd via de hubgateway. Dit houdt in dat uw VNet's hun eigen gateway voor het virtuele netwerk niet nodig hebben. In Virtual WAN kunnen uw VNet's eenvoudig schalen via de virtuele hub en de gateway van de virtuele hub. 

* **Hub virtuele netwerkverbinding:** De verbindingsbron van de Hub virtueel netwerk wordt gebruikt om de hub naadloos verbinding maken met het virtuele netwerk. Op dit moment kunt u alleen verbinding maken met virtuele netwerken die zich in dezelfde hubregio bevinden.

* **Routetabel hub:**  U kunt maken van een virtuele hub-route en de route van toepassing op de virtuele hub-routetabel. U kunt meerdere routes toepassen op de routetabel van de virtuele hub.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen

Weergave de [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) pagina voor meer informatie over onze virtuele WAN-partners en -locaties.
