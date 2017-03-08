---
title: 'Locaties en connectiviteitsproviders: Azure ExpressRoute | Microsoft Docs'
description: Dit artikel bevat een gedetailleerd overzicht van de locaties waar services worden aangeboden en hoe u verbinding maakt met Azure-regio&quot;s. Gesorteerd op locatie.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 9b18af35dc0a70c7335fd54477a6d8f57048b685
ms.openlocfilehash: 5bf418af22746610e96712ed33fd389998e4fa46
ms.lasthandoff: 03/01/2017

---
# <a name="expressroute-partners-and-peering-locations"></a>Partners en peeringlocaties voor ExpressRoute

> [!div class="op_single_selector"]
> * [Locaties per provider](expressroute-locations.md)
> * [Providers per locatie](expressroute-locations-providers.md)


In de tabellen in dit artikel vindt u informatie over ExpressRoute-connectiviteitsproviders, geografische dekking van ExpressRoute, Microsoft Cloud-services die via ExpressRoute worden ondersteund en Expressroute SI's (System Integrator).

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>ExpressRoute-connectiviteitsproviders
ExpressRoute wordt ondersteund in alle Azure-regio's en -locaties. De volgende kaart bevat een lijst van Azure-regio's en ExpressRoute-locaties. ExpressRoute-locaties zijn locaties waarop Microsoft samenwerkt met verschillende serviceproviders.

![Locatiekaart][0]

U hebt toegang tot Azure-services in alle regio's binnen een geopolitieke regio als u bent verbonden met ten minste één ExpressRoute-locatie in die geopolitieke regio. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Azure-regio's naar ExpressRoute-locaties binnen een geopolitieke regio
In de volgende tabel vindt u een toewijzing van Azure-regio's aan ExpressRoute-locaties binnen een geopolitieke regio.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Noord-Amerika** |VS - oost, VS - west, VS - oost 2, VS - west 2, VS - midden, Zuid-centraal VS, Noord-centraal VS, West-centraal VS, Canada Centraal, Canada Oost |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal, Quebec, Toronto |
| **Zuid-Amerika** |Brazilië - zuid |Sao Paulo |
| **Europa** |Noord-Europa, West-Europa, Verenigd Koninkrijk - west, Verenigd Koninkrijk - zuid |Amsterdam, Dublin, Londen, Newport (Wales), Parijs |
| **Azië** |Oost-Azië, Zuidoost-Azië |Hongkong, Singapore |
| **Japan** |Japan - west, Japan - oost |Osaka, Tokio |
| **Australië** |Australië - zuidoost, Australië - oost |Melbourne, Sydney |
| **India** |India - west, India - midden, India - zuid |Chennai, Mumbai |
| **Zuid-Korea** |Korea Centraal, Korea Zuid |Busan, Seoul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regio's en geopolitieke grenzen voor nationale clouds
De volgende tabel bevat informatie over regio's en geopolitieke grenzen voor nationale clouds.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- | --- |
| **Cloud van de Amerikaanse overheid** |VS (overheid) - Iowa , VS (overheid) - Virginia, US DoD - centraal, US DoD - oost  |Chicago, Dallas, New York, Silicon Valley, Washington DC |
| **China** |China Noord, China Oost |Beijing, Shanghai |
| **Duitsland** |Duitsland Centraal, Duitsland Oost |Berlijn, Frankfurt |

Connectiviteit tussen de geopolitieke regio's wordt niet ondersteund op de standaard ExpressRoute-SKU. U moet de invoegtoepassing ExpressRoute Premium inschakelen voor ondersteuning van globale connectiviteit. Connectiviteit met nationale cloudomgevingen wordt niet ondersteund. U kunt met uw connectiviteitsprovider samenwerken als de noodzaak daartoe zich voordoet.

## <a name="a-namelocationsaconnectivity-provider-locations"></a><a name="locations"></a>Locaties van connectiviteitsproviders

De volgende tabel geeft de connectiviteitslocaties en de serviceproviders voor elke locatie weer. Als u serviceproviders en de locaties waarvoor zij deze service kunnen bieden, wilt bekijken, raadpleegt u [Locaties per serviceprovider](expressroute-locations.md#locations). 


### <a name="production-azure"></a>Productie-Azure
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Amsterdam** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet Solutions - Cloud Connect, Interxion, KPN, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telenor, Verizon |
| **Atlanta** |Equinix |
| **Busan** |LG CNS+, Sejong Telecom |
| **Chennai** |Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Equinix, Level 3 Communications, Zayo Group |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport |
| **Dublin** |Colt, Telecity Group |
| **Hongkong** |British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Londen** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telenor, Verizon, Vodafone |
| **Las Vegas** |Level 3 Communications+, Megaport |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** |AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** |Coresite, Equinix, Megaport, Zayo Group |
| **Newport (Wales)** |Next Generation Data |
| **Montreal** |Cologix |
| **Mumbai** |Tata Communications |
| **Osaka** |Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank |
| **Parijs** |Interxion, Equinix+ |
| **Quebec** | Megaport |
| **Sao Paulo** |Equinix, Telefonica |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Seoul** |KINX, Sejong Telecom |
| **Silicon Valley** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Console, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapore** |Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokio** |Aryaka Networks, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** |Bell Canada, Cologix, Equinix, Megaport, Zayo Group |
| **Washington DC** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Verizon, Zayo Group |

 **+** betekent binnenkort beschikbaar

### <a name="national-cloud-environments"></a>Nationale cloudomgevingen

### <a name="us-government-cloud"></a>Cloud van de Amerikaanse overheid
| **Locatie** | **Serviceproviders** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, Level 3 Communications+, Verizon |
| **Silicon Valley** | Equinix |
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
| **Berlijn** |Colt+, e-shelter, Megaport+ |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Connectiviteit via niet vermelde serviceproviders
Als uw connectiviteitsprovider niet wordt vermeld in de vorige secties, kunt u alsnog verbinding maken.

* Neem contact op met uw connectiviteitsprovider om na te gaan of ze zijn verbonden met een van de exchange-punten in de bovenstaande tabel. Via de volgende koppelingen vindt u meer informatie over services die door de exchange-providers worden verstrekt. Verschillende connectiviteitsproviders zijn al verbonden met Ethernet-exchange-punten.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Vraag uw connectiviteitsprovider om uw netwerk uit te breiden tot de gewenste peeringlocatie.
  * Vergewis u ervan dat de connectiviteitsprovider uw connectiviteit uitbreidt op een maximaal beschikbare manier, zodat er geen storingspunten zijn.
* Vraag een ExpressRoute-circuit aan met het exchange-punt wanneer uw connectiviteitsprovider verbinding maakt met Microsoft.
  * Volg de stappen in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Een ExpressRoute-circuit maken) om connectiviteit in te stellen.

| **Locatie** | **Exchange** | **Connectiviteitsproviders** |
| --- | --- | --- |
| **Amsterdam** | Equinix | Eurofiber |
| **Londen** | Equinix | Exponential E, HSO |
| **New York** |Equinix |Lightower |
| **Seattle** |Equinix |Alaska Communications |
| **Silicon Valley** |Equinix |XO Communications |
| **Singapore** |Equinix |1CLOUDSTAR |
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation |
| **Washington DC** |Equinix | Lightower, Masergy |

## <a name="expressroute-system-integrators"></a>ExpressRoute-SI's
Het inschakelen van particuliere connectiviteit conform uw specifieke behoeften kan lastig zijn, al naargelang de schaal van uw netwerk. U kunt alle SI's uit de volgende tabel gebruiken om u te helpen met de voorbereidingen voor ExpressRoute.

| **Continent** | **Systeemintegratie** |
| --- | --- |
| **Azië** |Avanade Inc., OneAs1a |
| **Australië** | IT Consultancy |
| **Europa** |Avanade Inc., Dotnet Solutions, MSG Services, Nelite |
| **Noord-Amerika** |Avanade Inc., Equinix Professional Services, Perficient, Project Leadership |
| **Zuid-Amerika** |Avanade Inc. |
## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatiekaart"

