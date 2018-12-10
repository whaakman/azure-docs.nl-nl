---
title: Overzicht van Azure Virtual WAN | Microsoft Docs
description: Lees meer over geautomatiseerde schaalbare verbindingen tussen filialen via Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 09/25/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: c2edb821eb8bd9a5da7a6cce81269e7d3f611722
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52869888"
---
# <a name="what-is-azure-virtual-wan"></a>Wat is Azure Virtual WAN?

Azure Virtual WAN is een netwerkservice die via Azure voor een geoptimaliseerde en geautomatiseerde verbinding tussen filialen zorgt. Met Virtual WAN kunt u apparaten in filialen verbinden en configureren, zodat deze met Azure kunnen communiceren. U kunt dit handmatig doen of met behulp van apparaten van voorkeurspartners via een Virtual WAN-partner. Zie het artikel over [voorkeurspartners](https://go.microsoft.com/fwlink/p/?linkid=2019615) voor meer informatie. Met apparaten van voorkeurspartners geniet u de voordelen van gebruiksgemak, eenvoudige verbindingen en configuratiebeheer. Het geïntegreerde dashboard van Azure WAN biedt u directe inzichten voor het oplossen van problemen waardoor u tijd bespaart, en u kunt er op eenvoudige wijze grootschalige verbindingsmogelijkheden mee bekijken.

![Virtual WAN-diagram](./media/virtual-wan-about/virtualwan.png)

Met dit artikel krijgt u snel inzicht in de netwerkverbindingen van de workloads die u via Azure en buiten Azure uitvoert. Virtual WAN biedt de volgende voordelen:

* **Geïntegreerde verbindingsoplossingen in de hub en spoke:** automatiseer de interlokale configuratie en verbindingen tussen on-premises locaties en een Azure-hub.
* **Geautomatiseerde installatie en configuratie van spokes:** zorg voor een naadloze verbinding van uw virtuele netwerken en workloads met de Azure-hub.
* **Intuïtieve probleemoplossing:** u beschikt over een overzicht van de end-to-end stroom in Azure en kunt aan de hand van deze informatie de vereiste acties ondernemen.

## <a name="s2s"></a>Site-naar-site-verbindingen

Als u een verbinding tussen de verschillende locaties wilt maken met Virtual WAN, kunt u dat doen via een [Virtual WAN-partner](virtual-wan-locations-partners.md) of de verbinding handmatig tot stand brengen.

### <a name="s2spartner"></a>Werken met een Virtual WAN-partner

Wanneer u met een Virtual WAN-partner werkt, is dit het proces:

1. De controller (VPN/SD-WAN) voor filiaalapparaten wordt geverifieerd voor het exporteren van locatiegegevens naar Azure met behulp van een [Azure-service-principal](../active-directory/develop/howto-create-service-principal-portal.md).
2. De controller (VPN/SD-WAN) voor filiaalapparaten ontvangt de Azure-verbindingsconfiguratie en werkt het lokale apparaat bij. Op deze manier wordt het downloaden van de configuratie, het bewerken en het bijwerken van het on-premises VPN-apparaat geautomatiseerd.
3. Wanneer het apparaat eenmaal de juiste Azure-configuratie heeft, wordt een verbinding tussen de locaties (twee actieve tunnels) en Azure WAN tot stand gebracht. Azure ondersteunt zowel IKEv1 als IKEv2. BGP is optioneel.


Als u geen voorkeurspartner wilt gebruiken, kunt u de verbinding handmatig configureren. Zie [Een site-naar-site-verbinding maken met Virtual WAN](virtual-wan-site-to-site-portal.md).

## <a name="p2s"></a>Punt-naar-site-verbindingen (preview)

Met een point-to-site-verbinding (P2S) kunt u vanaf een afzonderlijke clientcomputer een beveiligde verbinding maken met uw virtuele hub. Een P2S-verbinding wordt tot stand gebracht door deze te starten vanaf de clientcomputer. Deze oplossing is handig voor telewerkers die verbinding willen maken vanaf een externe locatie, zoals thuis of een conferentie. P2S-VPN is ook een uitstekende oplossing in plaats van een S2S-VPN wanneer u maar een paar clients hebt die verbinding moeten maken.

Raadpleeg [Een site-naar-site-verbinding maken met Virtual WAN](virtual-wan-point-to-site-portal.md) als u de verbinding handmatig tot stand wilt brengen.

## <a name="er"></a>ExpressRoute-verbindingen (preview)

Raadpleeg [Een ExpressRoute-verbinding maken met Virtual WAN](virtual-wan-expressroute-portal.md) als u de verbinding handmatig tot stand wilt brengen.


## <a name="resources"></a>Virtual WAN-resources

Als u een end-to-end virtuele WAN wilt configureren, maakt u de volgende resources:

* **virtualWAN:** de virtualWAN-resource staat voor een virtuele overlay van uw Azure-netwerk en is een verzameling van meerdere resources. Het bevat koppelingen naar al uw virtuele hubs die u wilt opnemen in het virtuele WAN. Virtual WAN-resources zijn van elkaar geïsoleerd en kunnen geen gemeenschappelijke hub bevatten. Virtuele hubs in Virtual WAN communiceren niet met elkaar. De eigenschap 'verkeer tussen filialen toestaan' maakt verkeer mogelijk tussen VPN-sites, evenals tussen VPN-sites en sites waarop ExpressRoute is ingeschakeld. Houd er rekening mee dat ExpressRoute in Azure Virtual WAN momenteel alleen als preview beschikbaar is.

* **Locatie:** de locatieresource, ook wel vpnsite genoemd, staat voor uw on-premises VPN-apparaat en de instellingen daarvan. Wanneer u met een Virtual WAN-partner werkt, beschikt u over een geïntegreerde oplossing die deze gegevens automatisch naar Azure exporteert.

* **Hub:** een virtuele hub is een virtueel netwerk dat door Microsoft wordt beheerd. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. De hub is de kern van uw netwerk in een regio. Een Azure-regio mag maar één hub bevatten. Als u met de Azure-portal een hub maakt, wordt een VNet en een vpngateway voor de virtuele hub gemaakt.

  Een hubgateway is niet hetzelfde als een gateway voor het virtuele netwerk die u voor ExpressRoute en VPN Gateway gebruikt. Wanneer u Virtual WAN gebruikt, maakt u bijvoorbeeld geen interlokale verbinding vanaf uw on-premises locatie rechtstreeks naar uw VNet. In plaats daarvan maakt u een interlokale verbinding naar de hub. Het verkeer verloopt altijd via de hubgateway. Dit houdt in dat uw VNet's hun eigen gateway voor het virtuele netwerk niet nodig hebben. In Virtual WAN kunnen uw VNet's eenvoudig schalen via de virtuele hub en de gateway van de virtuele hub. 

* **Hub-verbinding met het virtuele netwerk:** de resource van de hub-verbinding met het virtuele netwerk wordt gebruikt om de hub naadloos met het virtuele netwerk te verbinden. Op dit moment kunt u alleen verbinding maken met virtuele netwerken die zich in dezelfde hubregio bevinden.

* **De tabel met de hubroute:** u kunt een virtuele-hubroute maken en de route toepassen op de routetabel van de virtuele hub. U kunt meerdere routes toepassen op de routetabel van de virtuele hub.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]


## <a name="next-steps"></a>Volgende stappen

Bekijk de pagina [Virtual WAN partners and locations](virtual-wan-locations-partners.md) (Virtual WAN-partners en -locaties).
