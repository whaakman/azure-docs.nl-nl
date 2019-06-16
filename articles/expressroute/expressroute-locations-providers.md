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
ms.date: 06/11/2019
ms.author: jaredr80
ms.openlocfilehash: f7006a8162aab87bc36f3dae034f10daa3f37ac1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67115456"
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
| **Europa** | 1 |Frankrijk - centraal, Frankrijk - zuid, Europa - noord, Europa - west, VK - west, VK - zuid |Amsterdam, Amsterdam2, Dublin, Frankfurt, Londen, London2, Marseille, Newport(Wales), Parijs, Zürich |
| **Noord-Amerika** | 1 |US - oost, US - west, US - oost 2, US - west 2, US - centraal, US - zuid-centraal, US - noord-centraal, US - west-centraal, Canada Centraal, Canada Oost |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silicon Valley, Silicon Valley2, Washington DC, Washington DC2, Montreal, Quebec City, Toronto |
| **Azië** | 2 |Azië - oost, Azië - zuidoost |Hong Kong SAR, Kuala Lumpur, Singapore, Singapore2, Taipei |
| **Australië** | 2 |Australië - zuidoost, Australië - oost |Melbourne, Perth, Sydney | 
| **India** | 2 |India - west, India - centraal, India - zuid |Chennai, Chennai2, Mumbai, Mumbai2 |
| **Japan** | 2 |Japan - west, Japan - oost |Osaka, Tokio |
| **Zuid-Korea** | 2 |Korea Centraal, Korea Zuid |Busan, Seoul|
| **VAE** | 3 | VAE-centraal, VAE-Noord | Dubai, Dubai2 |
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

**Lokale Azure-regio's** zijn die die [ExpressRoute lokale](expressroute-faqs.md) op elke locatie toegankelijk is. **n.v.t.** geeft aan dat lokale ExpressRoute niet beschikbaar op deze locatie is.


### <a name="production-azure"></a>Productie-Azure
| **Location** | **Eigenaar peeringlocatie** | **Lokale Azure-regio 's** | **Serviceproviders** |
| --- | --- | --- | --- |
| **Amsterdam** | Equinix | Europa -west | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdam2** | Interxion | Europa -west | DE-CIX, Interxion |
| **Atlanta** | Equinix | N.v.t. | Equinix, Megaport |
| **Busan** |LG CNS | Korea - zuid | LG CNS |
| **Canberra** | CDC | Australië - centraal | CDC |
| **Canberra2** | CDC | Australië - centraal 2| CDC |
| **Kaapstad** | Teraco | Zuid-Afrika (west) | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | India - zuid | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | India - zuid | Airtel |
| **Chicago** | Equinix | US - noord-centraal | Aryaka Networks, AT&T NetBond, Cologix, Comcast, Coresite, Equinix, InterCloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Dallas** | Equinix | N.v.t. | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, Telmex Uninet, Telia Carrier, Transtelco, Verizon, Zayo|
| **Denver** | CoreSite | US - west-centraal | CoreSite, Megaport, Zayo |
| **Dubai** | Etisalat VAE | VAE - noord | Etisalat VAE |
| **Dubai2** | du datamena | VAE - noord | du datamena, Orixcom |
| **Dublin** | Equinix | Europa - noord | Colt, eir, Equinix, Interxion, Megaport |
| **Frankfurt** | Interxion | N.v.t. | DE-CIX, Interxion |
| **Hongkong SAR** | Equinix | Azië - oost | Aryaka Networks, British Telecom, Chief Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Johannesburg** | Teraco | Zuid-Afrika (noord) | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Kuala Lumpur** | TIME dotCom | N.v.t. | TIME dotCom |
| **Las Vegas** | Switch | N.v.t. | CenturyLink Cloud Connect, Megaport |
| **Londen** | Equinix | Verenigd Koninkrijk Zuid | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | Telehouse | Verenigd Koninkrijk Zuid | IX Reach, Equinix |
| **Los Angeles** | CoreSite | N.v.t. | CoreSite, Equinix, Megaport, Neutrona Networks, NTT, Zayo |
| **Marseille** |Interxion | Frankrijk - zuid | Interxion, Jaguar Network |
| **Melbourne** | NextDC | Australië - zuidoost | AARNet, Devoli, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG Telecom |
| **Miami** | Equinix | N.v.t. | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | N.v.t. | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai** | Tata Communications | India - west | Globale CloudXchange (GCX), vertrouwen Jio, Sify, Tata communicatie, Verizon |
| **Mumbai2** | Airtel | India - west | Airtel, Sify, Vodafone Idea |
| **New York** | Equinix | N.v.t. | CenturyLink Cloud Connect, Coresite, Equinix, InterCloud, Megaport, Packet, Zayo |
| **Newport (Wales)** | Next Generation Data | Verenigd Koninkrijk West | British Telecom, Colt, Level 3 Communications, Next Generation Data |
| **Osaka** | Equinix | Japan - west | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Parijs** | Interxion | Frankrijk - centraal | Colt, Equinix, Intercloud, Interxion, Orange, Telia Carrier, Zayo |
| **Perth** | NextDC | N.v.t. | Megaport, NextDC |
| **Quebec** | 4Degrees | Canada - oost | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | US - zuid-centraal | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | Equinix | Brazilië - zuid | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | Equinix | US - west 2 | Aryaka Networks, Equinix, Level 3 Communications, Megaport, Telus, Zayo |
| **Seoul** | KINX | Korea - centraal | KINX, LG CNS, Sejong Telecom |
| **Silicon Valley** | Equinix | US - west | Aryaka netwerken, AT & T NetBond, Britse Telecom draaien Cloud Connect, Comcast, Coresite, Equinix, InterCloud, IX is bereikt, pakket, PacketFabric, Level 3 communicatie, Megaport, oranje, Sprint, Tata communicatie, Verizon, Zayo |
| **Silicon Valley2** | Coresite | US - west | Coresite | 
| **Singapore** | Equinix | Azië - zuidoost | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapore2** | Global Switch | Azië - zuidoost | Colt, Epsilon Global Communications, Megaport, SingTel |
| **Sydney** | Equinix | Australië - oost | AARNet, AT&T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT Communications, Optus, Orange, Telstra Corporation, TPG Telecom, Verizon |
| **Taipei** | Chief Telecom | N.v.t. | Chief Telecom, FarEasTone |
| **Tokio** | Equinix | Japan - oost | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT EAST, Orange, Softbank, Verizon |
| **Toronto** | Cologix | Canada - midden | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, IX Reach Megaport, Telus, Verizon, Zayo |
| **Washington DC** | Equinix | VS-Oost, VS-Oost 2 | Aryaka netwerken, AT & T NetBond, Britse Telecom, Cologix, Comcast, Coresite, Equinix, Internet2, InterCloud, Level 3 communicatie, Megaport, Neutrona netwerken, NTT communicatie, oranje, PacketFabric, Sprint, Tata communicatie, Telia provider, Verizon, Zayo |
| **Washington DC2** | Coresite | VS-Oost, VS-Oost 2 |Coresite | 
| **Zürich** | Interxion | N.v.t. | Interxion |

 **+** betekent binnenkort beschikbaar

### <a name="national-cloud-environments"></a>Nationale cloudomgevingen

### <a name="us-government-cloud"></a>Cloud van de Amerikaanse overheid
| **Location** | **Serviceproviders** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | AT&T NetBond, CenturyLink Cloud Connect, Megaport |
| **San Antonio** | CenturyLink Cloud Connect, Megaport |
| **Silicon Valley** | Equinix, Level 3 Communications, Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington DC** |AT&T NetBond, Equinix, Level 3 Communications, Megaport, Verizon |

### <a name="china"></a>China
| **Location** | **Serviceproviders** |
| --- | --- |
| **Beijing** |China Telecom |
| **Peking2** | China Telecom, GDS |
| **Shanghai** |China Telecom |
| **Shanghai2** | China Telecom, GDS |

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
| **Location** | **Exchange** | **Connectiviteitsproviders** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Telecity | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, MainOne, Nianet, Post, Proximus, TDC Erhverv, Telecom Italia Sparkle, Telia |
| **Atlanta** | Equinix| Castle kroon
| **Kaapstad** | Teraco | MTN |
| **Chicago** | Equinix | Kroon Castle, Spectrum Enterprise, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox bedrijven, kroon Castle, gegevens Foundry, Spectrum Enterprise, Transtelco |
| **Frankfurt** | Telecity | BICS, Cinia, Nianet, QSC AG |
| **Hamburg** | Equinix | Cinia |
| **Hongkong SAR** | Equinix | Chief, Macroview Telecom |
| **Johannesburg** | Teraco | MTN |
| **Londen** | BICS, Equinix, euNetworks, Telecity | Bezeq International Ltd., CoreAzure, Epsilon Telecommunications Limited, Exponential E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Kroon Castle, Spectrum Enterprise, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies, Inc. Cogeco Peer 1, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice, kroon Castle, Spectrum onderneming, Webair |
| **Parijs** | Equinix | Proximus |
| **Quebec** | Megaport | Fibrenoire |
| **Sao Paula** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Coresite, Equinix | Cox Business, Spectrum Enterprise, Windstream, X2nsat Inc. |
| **Singapore** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Cogeco Peer 1, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Cox Business, Crown Castle, Gtt Communications Inc, Epsilon Telecommunications Limited, Masergy, Windstream |

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
