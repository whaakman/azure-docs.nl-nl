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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/28/2019
ms.author: pareshmu
ms.openlocfilehash: 8a137919bfd3cf330586c3a4d5c9df18916e41e6
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629790"
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

| **Geopolitieke regio** | **Zone** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- | --- |
| **Australië - overheid** | 1 | Australië Centraal, Australië Centraal 2 |Canberra, Canberra2 |
| **Europa** | 1 |Frankrijk - centraal, Frankrijk - zuid, Europa - noord, Europa - west, VK - west, VK - zuid |Amsterdam, Amsterdam2, Dublin, Londen, London2, Marseille, Newport(Wales), Parijs, Zürich |
| **Noord-Amerika** | 1 |US - oost, US - west, US - oost 2, US - west 2, US - centraal, US - zuid-centraal, US - noord-centraal, US - west-centraal, Canada Centraal, Canada Oost |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silicon Valley, Washington DC, Washington DC2, Montreal, Quebec City, Toronto |
| **Azië** | 2 |Azië - oost, Azië - zuidoost |Hongkong, Kuala Lumpur, Singapore, Singapore2, Taipei |
| **Australië** | 2 |Australië - zuidoost, Australië - oost |Melbourne, Perth, Sydney | 
| **India** | 2 |India - west, India - centraal, India - zuid |Chennai, Chennai2, Mumbai, Mumbai2 |
| **Japan** | 2 |Japan - west, Japan - oost |Osaka, Tokio |
| **Zuid-Korea** | 2 |Korea Centraal, Korea Zuid |Busan, Seoul|
| **Zuid-Afrika** | 3 |Zuid-Afrika West, Zuid-Afrika (Noord) |Kaapstad, Johannesburg |
| **Zuid-Amerika** | 3 |Brazilië - zuid |Sao Paulo |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regio's en geopolitieke grenzen voor nationale clouds
De volgende tabel bevat informatie over regio's en geopolitieke grenzen voor nationale clouds.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Cloud van de Amerikaanse overheid** |US Gov - AZ, US Gov - Iowa, US Gov - TX, US Gov - Virginia, US DoD - centraal, US DoD - oost  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silicon Valley, Washington DC |
| **China - oost** |China - oost, China - oost2 |Shanghai, Shanghai2 |
| **China - noord** |China - noord, China - noord2 |Beijing, Beijing2 |
| **Duitsland** |Duitsland Centraal, Duitsland Oost |Berlijn, Frankfurt |

Connectiviteit tussen de geopolitieke regio's wordt niet ondersteund op de standaard ExpressRoute-SKU. U moet de invoegtoepassing ExpressRoute Premium inschakelen voor ondersteuning van globale connectiviteit. Connectiviteit met nationale cloudomgevingen wordt niet ondersteund. U kunt met uw connectiviteitsprovider samenwerken als de noodzaak daartoe zich voordoet.

## <a name="locations"></a>Locaties van connectiviteitsproviders

De volgende tabel geeft de connectiviteitslocaties en de serviceproviders voor elke locatie weer. Als u serviceproviders en de locaties waarvoor zij deze service kunnen bieden, wilt bekijken, raadpleegt u [Locaties per serviceprovider](expressroute-locations.md#locations). 


### <a name="production-azure"></a>Productie-Azure
| **Locatie** | **Eigenaar peeringlocatie** | **Serviceproviders** |
| --- | --- | --- |
| **Amsterdam** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdam2** | Interxion | DE-CIX, Interxion |
| **Atlanta** | Equinix | Equinix, Megaport |
| **Busan** |LG CNS | LG CNS |
| **Canberra** | CDC | CDC |
| **Canberra2** | CDC | CDC |
| **Kaapstad** | Teraco | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Airtel |
| **Chicago** | Equinix | Aryaka Networks, AT&T NetBond, Cologix, Comcast, Coresite, Equinix, InterCloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Dallas** | Equinix | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, Telmex Uninet, Telia Carrier, Verizon, Zayo|
| **Denver** | CoreSite | CoreSite, Megaport, Zayo |
| **Dublin** | Equinix | Colt, eir, Equinix, Interxion, Megaport |
| **Hongkong SAR** | Equinix | Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Johannesburg** | Teraco | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Kuala Lumpur** | TIME dotCom | TIME dotCom |
| **Las Vegas** | Switch | CenturyLink Cloud Connect, Megaport |
| **Londen** | Equinix | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | Telehouse | Equinix
| **Los Angeles** | CoreSite | CoreSite, Equinix, Megaport, Neutrona Networks, NTT, Zayo |
| **Marseille** |Interxion | Interxion |
| **Melbourne** | NextDC | AARNet, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation |
| **Miami** | Equinix | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai** | Tata Communications | Global CloudXchange (GCX), Sify, Tata Communications, Vodafone Idea |
| **Mumbai2** | Airtel | Airtel, Sify, Vodafone Idea |
| **New York** | Equinix | CenturyLink Cloud Connect, Coresite, Equinix, InterCloud, Megaport, Zayo |
| **Newport (Wales)** | Next Generation Data | British Telecom, Level 3 Communications, Next Generation Data |
| **Osaka** | Equinix | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Parijs** | Interxion | Colt, Equinix, Intercloud, Interxion, Orange, Zayo |
| **Perth** | NextDC | Megaport, NextDC |
| **Quebec** | 4Degrees | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | Equinix | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | Equinix | Aryaka Networks, Equinix, Level 3 Communications, Megaport, Zayo |
| **Seoul** | KINX | KINX, LG CNS, Sejong Telecom |
| **Silicon Valley** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Comcast, Coresite, Equinix, InterCloud, IXReach, PacketFabric, Level 3 Communications, Megaport, Orange, Sprint, Tata Communications, Verizon, Zayo |
| **Singapore** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapore2** | Global Switch | Colt, Epsilon Global Communications, Megaport, SingTel |
| **Sydney** | Equinix | AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, NTT Communications, Optus, Orange, Telstra Corporation, Verizon |
| **Taipei** | Chief Telecom | Chief Telecom |
| **Tokio** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT EAST, Orange, Softbank, Verizon |
| **Toronto** | Cologix | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, Megaport, Telus, Zayo |
| **Washington DC** | Equinix | Aryaka Networks, AT&T NetBond, British Telecom, Cologix, Comcast, Coresite, Equinix, Internet2, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, PacketFabric, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |
| **Washington DC2** | Coresite | Coresite | 
| **Zürich** | Interxion | Interxion |

 **+** betekent binnenkort beschikbaar

### <a name="national-cloud-environments"></a>Nationale cloudomgevingen

### <a name="us-government-cloud"></a>Cloud van de Amerikaanse overheid
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | AT&T NetBond, CenturyLink Cloud Connect |
| **San Antonio** | Megaport |
| **Silicon Valley** | Equinix, Level 3 Communications, Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington DC** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>China
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Beijing** |China Telecom |
| **Peking2** | GDS |
| **Shanghai** |China Telecom |
| **Shanghai2** | GDS |

Zie [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/) voor meer informatie.

### <a name="germany"></a>Duitsland
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Berlijn** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Connectiviteit via Exchange-providers
Als uw connectiviteitsprovider niet wordt vermeld in de vorige secties, kunt u alsnog verbinding maken.

* Neem contact op met uw connectiviteitsprovider om na te gaan of ze zijn verbonden met een van de exchange-punten in de bovenstaande tabel. Via de volgende koppelingen vindt u meer informatie over services die door de exchange-providers worden verstrekt. Verschillende connectiviteitsproviders zijn al verbonden met Ethernet-exchange-punten.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  
* Vraag uw connectiviteitsprovider om uw netwerk uit te breiden tot de gewenste peeringlocatie.
  * Vergewis u ervan dat de connectiviteitsprovider uw connectiviteit uitbreidt op een maximaal beschikbare manier, zodat er geen storingspunten zijn.
* Vraag een ExpressRoute-circuit aan met het exchange-punt wanneer uw connectiviteitsprovider verbinding maakt met Microsoft.
  * Volg de stappen in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Een ExpressRoute-circuit maken) om connectiviteit in te stellen.

## <a name="c1partners"></a>Connectiviteit via additionele serviceproviders
| **Locatie** | **Exchange** | **Connectiviteitsproviders** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Telecity | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, MainOne, Nianet, Post, Proximus, TDC Erhverv, Telecom Italia Sparkle, Telia |
| **Kaapstad** | Teraco | MTN |
| **Chicago** | Equinix | Lightower, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Data Foundry, Transtelco |
| **Frankfurt** | Telecity | BICS, Cinia, Nianet, QSC AG |
| **Hamburg** | Equinix | Cinia |
| **Hongkong SAR** | Equinix | Chief, Macroview Telecom |
| **Johannesburg** | Teraco | MTN |
| **Londen** | BICS, Equinix, euNetworks, Telecity | Bezeq International Ltd., CoreAzure, Epsilon Telecommunications Limited, Exponential E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies, Inc. Cogeco Peer 1, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice Business, Lightower, Webair |
| **Parijs** | Equinix | Proximus |
| **Quebec** | Megaport | Fibrenoire |
| **Sao Paula** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Equinix | Cox Business, Windstream |
| **Singapore** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies, Inc. Cogeco Peer 1, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Cox Business, Gtt Communications Inc, Epsilon Telecommunications Limited, Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute-SI's
Het inschakelen van particuliere connectiviteit conform uw specifieke behoeften kan lastig zijn, al naargelang de schaal van uw netwerk. U kunt alle SI's uit de volgende tabel gebruiken om u te helpen met de voorbereidingen voor ExpressRoute.

| **Continent** | **Systeemintegratie** |
| --- | --- |
| **Azië** |Avanade Inc., OneAs1a |
| **Australië** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Noord-Amerika** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient, Presidio |
| **Zuid-Amerika** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatiekaart"
