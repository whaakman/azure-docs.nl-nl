---
title: bestand opnemen
description: bestand opnemen
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 03/19/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 551b1d4924c354eb6fd501c1b82ed306a7ea4822
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305291"
---
### <a name="what-is-expressroute-global-reach"></a>Wat is ExpressRoute globaal bereik?

Globaal bereik ExpressRoute is een Azure-service die uw on-premises netwerken via de ExpressRoute-service via een mondiaal netwerk van Microsoft verbindt. Bijvoorbeeld, hebt u een particulier datacenter in Californië verbonden met ExpressRoute in Silicon Valley en een ander particulier datacenter in Texas verbonden met ExpressRoute in Dallas, met ExpressRoute globaal bereik, u kunt uw persoonlijke datacenters met elkaar verbinden data center verkeer wordt via de twee ExpressRoute-verbindingen en de cross passeren via de netwerk-backbone van Microsoft.

### <a name="how-do-i-enable-or-disable-expressroute-global-reach"></a>Hoe ik in- of uitschakelen ExpressRoute globaal bereik?

U inschakelen ExpressRoute globaal bereik door het met elkaar verbinden van uw ExpressRoute-circuits. U kunt de functie uitschakelen door de circuits. Zie de [configuratie](../articles/expressroute/expressroute-howto-set-global-reach.md).

### <a name="do-i-need-expressroute-premium-for-expressroute-global-reach"></a>Moet ik ExpressRoute Premium voor ExpressRoute globaal bereik?

Als uw ExpressRoute-circuits in dezelfde geopolitieke regio zijn, hoeft u geen ExpressRoute Premium ze met elkaar verbinden. Als twee ExpressRoute-circuits in andere geopolitieke regio's zijn, moet u ExpressRoute Premium voor beide circuits om in te schakelen connectiviteit ertussen. 

### <a name="how-will-i-be-charged-for-expressroute-global-reach"></a>Hoe moet ik betalen voor ExpressRoute globaal bereik?

Met ExpressRoute kunt connectiviteit vanuit uw on-premises netwerk naar Microsoft-cloudservices. ExpressRoute globaal bereik zorgt voor connectiviteit tussen uw eigen on-premises netwerken via uw bestaande ExpressRoute-circuits, gebruik te maken van het wereldwijde netwerk van Microsoft. ExpressRoute globaal bereik wordt apart in rekening gebracht van de bestaande ExpressRoute-service. Er zijn de kosten van een invoegtoepassing voor het inschakelen van deze functie op elk ExpressRoute-circuit. Verkeer tussen uw on-premises netwerken ingeschakeld door ExpressRoute globaal bereik wordt gefactureerd voor een snelheid van uitgangsgebeurtenissen bij de bron en de snelheid van een inkomend verkeer op de bestemming. De tarieven zijn gebaseerd op de zone waarop de circuits zich bevinden. Zie <pricing page>

### <a name="where-is-expressroute-global-reach-supported"></a>Waar wordt globaal bereik van ExpressRoute ondersteund?

Globaal bereik van ExpressRoute wordt ondersteund in [landen of locaties selecteren](../articles/expressroute/expressroute-global-reach.md). De ExpressRoute-circuits moeten worden gemaakt op de peering locaties in die landen of plaatsen.

### <a name="i-have-more-than-two-on-premises-networks-each-connected-to-an-expressroute-circuit-can-i-enable-expressroute-global-reach-to-connect-all-of-my-on-premises-networks-together"></a>Ik heb meer dan twee on-premises netwerken, elk verbonden met een ExpressRoute-circuit. Kan ik inschakelen globaal bereik van ExpressRoute al mijn on-premises netwerken met elkaar verbinden?

Ja, u kunt wijzigen, zolang de circuits in de ondersteunde landen zijn. U moet twee ExpressRoute-circuits tegelijk verbinding maken. Voor het maken van een volledig geïntegreerde netwerk, moet u alle circuit paren opsommen en herhaalt u de configuratie. 

### <a name="can-i-enable-expressroute-global-reach-between-two-expressroute-circuits-at-the-same-peering-location"></a>Kan ik ExpressRoute globaal bereik inschakelen tussen de twee ExpressRoute-circuits op dezelfde locatie peering?

Nee. De twee circuits moeten verschillende peeringlocaties. Als een metro in een ondersteunde land/regio meer dan één ExpressRoute-peering locatie heeft, kunt u de ExpressRoute-circuits die zijn gemaakt op verschillende locaties van peering in die in metro elkaar verbinden. 

### <a name="if-expressroute-global-reach-is-enabled-between-circuit-x-and-circuit-y-and-between-circuit-y-and-circuit-z-will-my-on-premises-networks-connected-to-circuit-x-and-circuit-z-talk-to-each-other-via-microsofts-network"></a>Als u ExpressRoute globaal bereik tussen circuit X en Y-circuit, en tussen circuit Y en Z-circuit is ingeschakeld, wordt mijn on-premises-netwerken die zijn verbonden met circuit X en het circuit Z met elkaar communiceren via het netwerk van Microsoft?

Nee. Om in te schakelen connectiviteit tussen twee van uw on-premises netwerken, moet u de bijbehorende ExpressRoute-circuits expliciet verbinden. In het bovenstaande voorbeeld moet u verbinding maken met circuit X en Z-circuit. 

### <a name="what-is-the-network-throughput-i-can-expect-between-my-on-premises-networks-after-i-enable-expressroute-global-reach"></a>Wat is de doorvoer van het netwerk die ik tussen mijn on-premises netwerken verwachten kan nadat ik ExpressRoute globaal bereik hebt ingeschakeld?

De netwerkdoorvoer tussen uw on-premises netwerken, ingeschakeld door ExpressRoute globaal bereik, wordt beperkt door de kleinste hoeveelheid van de twee ExpressRoute-circuits. Premises-to-Azure-verkeer en premises-to-premises delen hetzelfde circuit en zijn onderworpen aan de dezelfde Bandbreedtelimiet. 

### <a name="with-expressroute-global-reach-what-are-the-limits-on-the-number-of-routes-i-can-advertise-and-the-number-of-routes-i-will-receive"></a>Met ExpressRoute globaal bereik, wat zijn de limieten voor het aantal routes dat ik kunt adverteren en het aantal routes dat ik ontvang?

Het aantal routes dat u naar Microsoft voor persoonlijke Azure-peering adverteren kunt blijft 4000 op een standaardcircuit of 10000 op een Premium-circuit. Het aantal routes dat u van Microsoft op persoonlijke Azure-peering ontvangt wordt de som van de routes van uw Azure-netwerken en de routes van uw on-premises-netwerken die zijn verbonden via ExpressRoute globaal bereik. Zorg ervoor dat u een limiet voor de bijbehorende maximale voorvoegsel instelt op uw on-premises router. 

### <a name="what-is-the-sla-for-expressroute-global-reach"></a>Wat is de SLA voor ExpressRoute globaal bereik?

Globaal bereik ExpressRoute biedt dezelfde [beschikbaarheids-SLA](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/) als de gewone ExpressRoute-service.
