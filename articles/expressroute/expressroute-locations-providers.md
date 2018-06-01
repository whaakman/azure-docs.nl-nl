---
title: 'Locaties en connectiviteitsproviders: Azure ExpressRoute | Microsoft Docs'
description: Dit artikel bevat een gedetailleerd overzicht van de locaties waar services worden aangeboden en hoe u verbinding maakt met Azure-regio's. Gesorteerd op locatie.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/15/2018
ms.author: pareshmu
ms.openlocfilehash: 6a85676525bc17f62866c7828e03d5334b2c579c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34200908"
---
# <a name="expressroute-partners-and-peering-locations"></a>Partners en peeringlocaties voor ExpressRoute

> [!div class="op_single_selector"]
> * [Locaties per provider](expressroute-locations.md)
> * [Providers per locatie](expressroute-locations-providers.md)


In de tabellen in dit artikel vindt u informatie over ExpressRoute-connectiviteitsproviders, geografische dekking van ExpressRoute, Microsoft Cloud-services die via ExpressRoute worden ondersteund en Expressroute SI's (System Integrator).

## <a name="partners"></a>ExpressRoute-connectiviteitsproviders
ExpressRoute wordt ondersteund in alle Azure-regio's en -locaties. De volgende kaart bevat een lijst van Azure-regio's en ExpressRoute-locaties. ExpressRoute-locaties zijn locaties waarop Microsoft samenwerkt met verschillende serviceproviders.

![Locatiekaart][0]

U hebt toegang tot Azure-services in alle regio's binnen een geopolitieke regio als u bent verbonden met ten minste één ExpressRoute-locatie in die geopolitieke regio. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Azure-regio's naar ExpressRoute-locaties binnen een geopolitieke regio
In de volgende tabel vindt u een toewijzing van Azure-regio's aan ExpressRoute-locaties binnen een geopolitieke regio.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Noord-Amerika** |VS - oost, VS - west, VS - oost 2, VS - west 2, VS - midden, Zuid-centraal VS, Noord-centraal VS, West-centraal VS, Canada Centraal, Canada Oost |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silicon Valley, Washington DC, Montreal, Quebec City, Toronto |
| **Zuid-Amerika** |Brazilië - zuid |Sao Paulo |
| **Europa** |Frankrijk - centraal, Frankrijk - zuid, Noord-Europa, West-Europa, VK - west, VK - zuid |Amsterdam, Dublin, Londen, Newport (Wales), Parijs |
| **Azië** |Oost-Azië, Zuidoost-Azië |Hongkong, Singapore, Singapore2 |
| **Japan** |Japan - west, Japan - oost |Osaka, Tokio |
| **Australië** |Australië - zuidoost, Australië - oost |Melbourne, Sydney |
| **Australië - overheid** | Australië Centraal, Australië Centraal 2 |Canberra, Canberra2 | 
| **India** |India - west, India - midden, India - zuid |Chennai, Mumbai |
| **Zuid-Korea** |Korea Centraal, Korea Zuid |Busan, Seoul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regio's en geopolitieke grenzen voor nationale clouds
De volgende tabel bevat informatie over regio's en geopolitieke grenzen voor nationale clouds.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Cloud van de Amerikaanse overheid** |VS (overheid) - Iowa , VS (overheid) - Virginia, US DoD - centraal, US DoD - oost  |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **China** |China Noord, China Oost |Beijing, Shanghai |
| **Duitsland** |Duitsland Centraal, Duitsland Oost |Berlijn, Frankfurt |

Connectiviteit tussen de geopolitieke regio's wordt niet ondersteund op de standaard ExpressRoute-SKU. U moet de invoegtoepassing ExpressRoute Premium inschakelen voor ondersteuning van globale connectiviteit. Connectiviteit met nationale cloudomgevingen wordt niet ondersteund. U kunt met uw connectiviteitsprovider samenwerken als de noodzaak daartoe zich voordoet.

## <a name="locations"></a>Locaties van connectiviteitsproviders

De volgende tabel geeft de connectiviteitslocaties en de serviceproviders voor elke locatie weer. Als u serviceproviders en de locaties waarvoor zij deze service kunnen bieden, wilt bekijken, raadpleegt u [Locaties per serviceprovider](expressroute-locations.md#locations). 


### <a name="production-azure"></a>Productie-Azure
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Amsterdam** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet Solutions - Cloud Connect, Interxion, KPN, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Verizon, Zayo |
| **Atlanta** |Equinix |
| **Busan** |LG CNS |
| **Canberra** | CDC |
| **Canberra2** | CDC |
| **Chennai** | Airtel+, Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Coresite, Equinix, PacketFabric, Level 3 Communications, Megaport, Verizon, Zayo |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport, Telmex Uninet+, Verizon, Zayo|
| **Denver** |CoreSite, Megaport |
| **Dublin** |Colt, eir, Interxion, Megaport, Telecity Group |
| **Hongkong** |Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Las Vegas** |CenturyLink Cloud Connect, Megaport |
| **Londen** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo |
| **Melbourne** |AARNet, Equinix, Megaport, NEXTDC, Optus+, Telstra Corporation |
| **Miami** |C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** |Bell Canada, Cologix, Telus, Zayo |
| **Mumbai** |Airtel+, Global CloudXchange (GCX), Sify, Tata Communications |
| **New York** |CenturyLink Cloud Connect, Coresite, Equinix, Megaport, Zayo |
| **Newport (Wales)** |Level 3 Communications, Next Generation Data |
| **Osaka** |Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Parijs** |Colt, Intercloud, Interxion, Equinix, Orange |
| **Quebec** | Bell Canada, Megaport |
| **San Antonio** |CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** |Ascenty Data Centers+, Equinix, Level 3 Communications, Neutrona Networks, Telefonica, UOLDIVEO |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Seoul** |KINX, LG CNS, Sejong Telecom |
| **Silicon Valley** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Comcast, Coresite, Equinix, IXReach, PacketFabric, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapore** |Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Singapore2** |Megaport, SingTel |
| **Sydney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, NTT Communications, Optus, Orange, Telstra Corporation, Verizon |
| **Tokio** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT EAST, Softbank, Verizon |
| **Toronto** |AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Console, Equinix, Megaport, Telus, Zayo |
| **Washington DC** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Coresite, Equinix, Internet2, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, PacketFabric, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |

 **+** betekent binnenkort beschikbaar

### <a name="national-cloud-environments"></a>Nationale cloudomgevingen

### <a name="us-government-cloud"></a>Cloud van de Amerikaanse overheid
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, Level 3 Communications+, Verizon |
| **Silicon Valley** | Equinix, Level 3 Communications |
| **Seattle** | Equinix |
| **Washington DC** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>China
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Beijing** |China Telecom |
| **Shanghai** |China Telecom |

Zie [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/) voor meer informatie.

### <a name="germany"></a>Duitsland
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Berlijn** |Colt+, e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Connectiviteit via Exchange-providers
Als uw connectiviteitsprovider niet wordt vermeld in de vorige secties, kunt u alsnog verbinding maken.

* Neem contact op met uw connectiviteitsprovider om na te gaan of ze zijn verbonden met een van de exchange-punten in de bovenstaande tabel. Via de volgende koppelingen vindt u meer informatie over services die door de exchange-providers worden verstrekt. Verschillende connectiviteitsproviders zijn al verbonden met Ethernet-exchange-punten.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [IX Reach](https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Vraag uw connectiviteitsprovider om uw netwerk uit te breiden tot de gewenste peeringlocatie.
  * Vergewis u ervan dat de connectiviteitsprovider uw connectiviteit uitbreidt op een maximaal beschikbare manier, zodat er geen storingspunten zijn.
* Vraag een ExpressRoute-circuit aan met het exchange-punt wanneer uw connectiviteitsprovider verbinding maakt met Microsoft.
  * Volg de stappen in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Een ExpressRoute-circuit maken) om connectiviteit in te stellen.

## <a name="c1partners"></a>Connectiviteit via additionele serviceproviders
| **Locatie** | **Exchange** | **Connectiviteitsproviders** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Telecity | BICS, Eurofiber, Fastweb S.p.A, MainOne, Nianet, Post, Proximus, Telia |
| **Chicago** | Equinix | Lightower, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Data Foundry, Transtelco |
| **Frankfurt** | Telecity | BICS, Nianet, QSC AG |
| **Hongkong** | Equinix | Macroview Telecom |
| **Londen** | Equinix, euNetworks, Telecity | Bezeq International Ltd., Epsilon, Exponential E, HSO, NexGen Networks, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies. Inc, Cogeco Peer 1, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice Business, Lightower, Webair |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Equinix | Cox Business, Windstream |
| **Singapore** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies. Inc, Cogeco Peer 1, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Gtt Communications Inc, Epsilon, Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute-SI's
Het inschakelen van particuliere connectiviteit conform uw specifieke behoeften kan lastig zijn, al naargelang de schaal van uw netwerk. U kunt alle SI's uit de volgende tabel gebruiken om u te helpen met de voorbereidingen voor ExpressRoute.

| **Continent** | **Systeemintegratie** |
| --- | --- |
| **Azië** |Avanade Inc., OneAs1a |
| **Australië** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Noord-Amerika** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient, Presidio |
| **Zuid-Amerika** |Avanade Inc. |
## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatiekaart"
