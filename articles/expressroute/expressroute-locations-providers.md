<properties
   pageTitle="ExpressRoute-locaties | Microsoft Azure"
   description="Dit artikel bevat een gedetailleerd overzicht van de locaties waar services worden aangeboden en hoe u verbinding maakt met Azure-regio's."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/20/2016"
   ms.author="cherylmc" />


# Partners en peeringlocaties voor ExpressRoute

In de tabellen in dit artikel vindt u informatie over ExpressRoute-connectiviteitsproviders, geografische dekking van ExpressRoute, Microsoft Cloud-services die via ExpressRoute worden ondersteund en Expressroute SI's (System Integrator).

## <a name="partners"></a>ExpressRoute-connectiviteitsproviders

ExpressRoute wordt ondersteund in alle Azure-regio's en -locaties. De volgende kaart bevat een lijst van Azure-regio's en ExpressRoute-locaties. ExpressRoute-locaties zijn locaties waarop Microsoft samenwerkt met verschillende serviceproviders.

![Locatiekaart][0]

U hebt toegang tot Azure-services in alle regio's binnen een geopolitieke regio als u bent verbonden met ten minste één ExpressRoute-locatie in die geopolitieke regio. In de volgende tabel vindt u een toewijzing van Azure-regio's aan ExpressRoute-locaties binnen een geopolitieke regio.

|**Geopolitieke regio**|**Azure-regio's**|**ExpressRoute-locaties**|
|---|---|---|
|**Noord-Amerika**|VS - oost, VS - west, VS - oost 2, VS - midden, Zuid-centraal VS, Noord-centraal VS, Canada Centraal, Canada Oost|Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal+, Quebec City+, Toronto|
|**Zuid-Amerika**|Brazilië - zuid|Sao Paulo|
|**Europa**|Noord-Europa, West-Europa, Verenigd Koninkrijk - west, Verenigd Koninkrijk - zuid|Amsterdam, Dublin, Londen, Newport(Wales)+, Parijs|
|**Azië**|Oost-Azië, Zuidoost-Azië|Hongkong, Singapore|
|**Japan**|Japan - west, Japan - oost|Osaka, Tokio|
|**Australië**|Australië - zuidoost, Australië - oost|Melbourne, Sydney|
|**India**|India - west, India - midden, India - zuid|Chennai, Mumbai|



De volgende tabel bevat informatie over regio's en geopolitieke grenzen voor nationale clouds.

|**Geopolitieke regio**|**Azure-regio's**|**ExpressRoute-locaties**|
|---|---|---|---|
|**Cloud van de Amerikaanse overheid**|VS (overheid) - Iowa, VS (overheid) - Virginia|Chicago, Dallas, New York, Washington DC|
|**China**|China Noord, China Oost|Beijing, Shanghai|
|**Duitsland**|Duitsland Centraal, Duitsland Oost|Berlijn+, Frankfurt|


Connectiviteit tussen de geopolitieke regio's wordt niet ondersteund op de standaard ExpressRoute-SKU. U moet de invoegtoepassing ExpressRoute Premium inschakelen voor ondersteuning van globale connectiviteit. Connectiviteit met nationale cloudomgevingen wordt niet ondersteund. U kunt met uw connectiviteitsprovider samenwerken als de noodzaak daartoe zich voordoet.


## Locaties van connectiviteitsproviders

> [AZURE.SELECTOR]
[Locaties per provider](expressroute-locations.md#connectivity-provider-locations)
[Providers per locatie](expressroute-locations-providers.md#connectivity-provider-locations)

### Productie-Azure
| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT+, InterCloud, Internet Solutions - Cloud Connect, Interxion, Level 3 Communications, Orange, Tata Communications, TeleCity Group, Telenor, Verizon |
| **Atlanta** | Equinix |
| **Chennai** | Tata Communications |
| **Chicago** | AT&T NetBond, Comcast, Equinix, Level 3 Communications, Zayo Group |
| **Dallas** | AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport |
| **Dublin** | Colt, Telecity Group |
| **Hongkong** | British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Londen** | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc+, Level 3 Communications, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telenor, Verizon, Vodafone |
| **Las Vegas** | Level 3 Communications+, Megaport
| **Los Angeles** | CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** | Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo Group |
| **Montreal** | Cologix+ |
| **Mumbai** | Tata Communications |
| **Osaka** | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank |
| **Parijs** | Interxion |
| **Sao Paulo** | Equinix, Telefonica |
| **Seattle** | Equinix, Level 3 Communications, Megaport |
| **Silicon Valley** | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Equinix, Level 3 Communications, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapore** | Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Megaport, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** | AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokio** | Aryaka Networks, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** | Cologix, Equinix, Zayo Group |
| **Washington DC** | Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Group |

 **+** betekent binnenkort beschikbaar

### Nationale cloudomgevingen

#### Cloud van de Amerikaanse overheid

| **Locatie**  |**Serviceproviders** |
|---------------|--------------------|
| **Chicago** | AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |  Equinix, Verizon+ |
| **New York** | Equinix, Level 3 Communications+, Verizon |
| **Washington DC** | AT&T NetBond, Equinix, Level 3 Communications, Verizon |

#### China

| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Peking** | China Telecom |
| **Shanghai** |  China Telecom |
Zie [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/) voor meer informatie.

#### Duitsland

| **Locatie**  | **Serviceproviders** |
|---------------|-----------------------|
| **Berlijn** | Colt+, e-shelter+ |
| **Frankfurt** | Colt, Equinix+, Interxion |

## <a name="nonpartners"></a>Connectiviteit via niet vermelde serviceproviders

Als uw connectiviteitsprovider niet wordt vermeld in de vorige secties, kunt u alsnog verbinding maken.

- Neem contact op met uw connectiviteitsprovider om na te gaan of ze zijn verbonden met een van de exchange-punten in de bovenstaande tabel. Via de volgende koppelingen vindt u meer informatie over services die door de exchange-providers worden verstrekt. Verschillende connectiviteitsproviders zijn al verbonden met Ethernet-exchange-punten.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Vraag uw connectiviteitsprovider om uw netwerk uit te breiden tot de gewenste peeringlocatie.
    - Vergewis u ervan dat de connectiviteitsprovider uw connectiviteit uitbreidt op een maximaal beschikbare manier, zodat er geen storingspunten zijn.
- Vraag een ExpressRoute-circuit aan met het exchange-punt wanneer uw connectiviteitsprovider verbinding maakt met Microsoft.
    - Volg de stappen in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Een ExpressRoute-circuit maken) om connectiviteit in te stellen.

|**Locatie**|**Exchange**|**Connectiveitsproviders**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Seattle** | Equinix | Alaska Communications |
| **Silicon Valley** | Equinix | XO Communications |
| **Singapore** | Equinix | 1CLOUDSTAR |
| **Washington DC** | Equinix | Lightower |

## ExpressRoute-SI's

Het inschakelen van particuliere connectiviteit conform uw specifieke behoeften kan lastig zijn, al naargelang de schaal van uw netwerk. U kunt alle SI's uit de volgende tabel gebruiken om u te helpen met de voorbereidingen voor ExpressRoute.

|**Continent**|**System integrators**|
|-------------|---------------------|
| **Azië** | Avanade Inc., OneAs1a|
| **Europa** | Avanade Inc., Dotnet Solutions|
| **VS** | Avanade Inc., Equinix Professional Services, Perficient, Project Leadership|

## Volgende stappen

- Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
- Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatiekaart"



<!--HONumber=Sep16_HO3-->


