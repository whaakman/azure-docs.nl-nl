---
title: Overzicht van Azure Virtual WAN | Microsoft Docs
description: Meer informatie over de geautomatiseerde schaal bare vertakking-to-Branch connectiviteit van Virtual WAN, beschik bare regio's en partners.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 07/22/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: f1576e963f9c25821b5e3f57907662e3d86df4e0
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68406347"
---
# <a name="what-is-azure-virtual-wan"></a>Wat is Azure Virtual WAN?

Azure Virtual WAN is een netwerk service die zorgt voor geoptimaliseerde en geautomatiseerde vertakkings connectiviteit met, en via Azure. Azure-regio's fungeren als hubs die u kunt gebruiken om uw filialen te koppelen aan. U kunt de Azure-backbone gebruiken om ook vertakkingen te verbinden en te profiteren van de connectiviteit van Branch-to-VNet. Er is een lijst met partners die connectiviteits automatisering ondersteunen met Azure Virtual WAN VPN. Zie het artikel [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) voor meer informatie.

Met Azure Virtual WAN worden veel Azure-Cloud verbindings Services, zoals site-naar-site-VPN en ExpressRoute, in één operationele interface gecombineerd. Connectiviteit met Azure VNets wordt tot stand gebracht met behulp van virtuele netwerk verbindingen.

ExpressRoute voor Virtual WAN is momenteel beschikbaar als preview-versie.

![Virtual WAN-diagram](./media/virtual-wan-about/virtualwan1.png)

In dit artikel vindt u een kort overzicht van de netwerk verbinding in azure Virtual WAN. Virtual WAN biedt de volgende voordelen:

* **Geïntegreerde connectiviteits oplossingen in hub en spoke:** Automatiseer site-naar-site-configuratie en connectiviteit tussen on-premises sites en een Azure-hub.
* **Automatische spoke-installatie en-configuratie:** Verbind uw virtuele netwerken en werk belastingen naadloos met de Azure-hub.
* **Intuïtieve problemen oplossen:** U kunt de end-to-end-stroom in azure bekijken en vervolgens deze informatie gebruiken om de vereiste acties uit te voeren.

## <a name="resources"></a>Virtual WAN-resources

Als u een end-to-end virtuele WAN wilt configureren, maakt u de volgende resources:

* **virtualWAN:** De virtualWAN-resource vertegenwoordigt een virtuele overlay van uw Azure-netwerk en is een verzameling van meerdere resources. Het bevat koppelingen naar al uw virtuele hubs die u wilt opnemen in het virtuele WAN. Virtual WAN-resources zijn van elkaar geïsoleerd en kunnen geen gemeenschappelijke hub bevatten. Virtuele hubs in Virtual WAN communiceren niet met elkaar. Met de eigenschap ' vertakking naar vertakkings verkeer toestaan ' kunnen verkeer tussen VPN-sites en VPN naar ExpressRoute (momenteel in Preview) ingeschakelde sites worden.

* **Hub:** Een virtuele hub is een virtueel netwerk dat door micro soft wordt beheerd. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. De hub is de kern van uw netwerk in een regio. Een Azure-regio mag maar één hub bevatten. Als u met de Azure-portal een hub maakt, wordt een VNet en een vpngateway voor de virtuele hub gemaakt.

  Een hubgateway is niet hetzelfde als een gateway voor het virtuele netwerk die u voor ExpressRoute en VPN Gateway gebruikt. Wanneer u bijvoorbeeld virtueel WAN gebruikt, maakt u geen site-naar-site-verbinding vanaf uw on-premises site rechtstreeks naar uw VNet. In plaats daarvan maakt u een site-naar-site-verbinding met de hub. Het verkeer verloopt altijd via de hubgateway. Dit houdt in dat uw VNet's hun eigen gateway voor het virtuele netwerk niet nodig hebben. In Virtual WAN kunnen uw VNet's eenvoudig schalen via de virtuele hub en de gateway van de virtuele hub.

* **Virtuele hub-netwerk verbinding:** De netwerk verbindings bron van de hub wordt gebruikt om de hub naadloos met uw virtuele netwerk te verbinden. Op dit moment kunt u alleen verbinding maken met virtuele netwerken die zich in dezelfde hubregio bevinden.

* **Route tabel van de hub:**  U kunt een virtuele hub-route maken en de route Toep assen op de route tabel van de virtuele hub. U kunt meerdere routes toepassen op de routetabel van de virtuele hub.

**Aanvullende virtuele WAN-resources**

  * **Site** Deze bron wordt alleen gebruikt voor site-naar-site-verbindingen. De site resource is **vpnsite**. Het staat voor uw on-premises VPN-apparaat en de bijbehorende instellingen. Wanneer u met een Virtual WAN-partner werkt, beschikt u over een geïntegreerde oplossing die deze gegevens automatisch naar Azure exporteert.

## <a name="connectivity"></a>Mogelijkheden

Met Virtual WAN kunt u twee typen verbindingen toestaan: Site-naar-site-en ExpressRoute (preview-versie).

### <a name="s2s"></a>Site-naar-site-VPN-verbindingen

![Virtual WAN-diagram](./media/virtual-wan-about/virtualwan.png)

Wanneer u een virtuele WAN-site-naar-site-verbinding maakt, kunt u werken met een beschik bare partner. Als u geen partner wilt gebruiken, kunt u de verbinding hand matig configureren. Zie [een site-naar-site-verbinding maken met behulp van Virtual WAN](virtual-wan-site-to-site-portal.md)voor meer informatie.

#### <a name="s2spartner"></a>Virtuele WAN-partner werk stroom

Wanneer u met een virtuele WAN-partner werkt, is de werk stroom:

1. De controller (VPN/SD-WAN) voor filiaalapparaten wordt geverifieerd voor het exporteren van locatiegegevens naar Azure met behulp van een [Azure-service-principal](../active-directory/develop/howto-create-service-principal-portal.md).
2. De controller (VPN/SD-WAN) voor filiaalapparaten ontvangt de Azure-verbindingsconfiguratie en werkt het lokale apparaat bij. Op deze manier wordt het downloaden van de configuratie, het bewerken en het bijwerken van het on-premises VPN-apparaat geautomatiseerd.
3. Wanneer het apparaat eenmaal de juiste Azure-configuratie heeft, wordt een verbinding tussen de locaties (twee actieve tunnels) en Azure WAN tot stand gebracht. Azure ondersteunt zowel IKEv1 als IKEv2. BGP is optioneel.

#### <a name="partners"></a>Partners voor site-naar-site virtuele WAN-verbindingen

Zie het artikel [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) voor een lijst met de beschik bare partners en locaties.


### <a name="er"></a>ExpressRoute-verbindingen (preview)

Met ExpressRoute kunt u een on-premises netwerk verbinden met Azure via een particuliere verbinding. Zie [een ExpressRoute-verbinding maken met behulp van Virtual WAN](virtual-wan-expressroute-portal.md)als u de verbinding wilt maken.

## <a name="locations"></a>Maplocaties

Zie het artikel [virtuele WAN-partners en locaties](virtual-wan-locations-partners.md) voor informatie over de locatie.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Volgende stappen

[Een site-naar-site-verbinding maken met behulp van Virtual WAN](virtual-wan-site-to-site-portal.md)
