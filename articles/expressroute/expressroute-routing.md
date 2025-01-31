---
title: 'Vereisten - ExpressRoute-routering: Azure | Microsoft Docs'
description: Deze pagina bevat gedetailleerde vereisten voor het configureren en beheren van routering voor ExpressRoute-circuits.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 6c475ab0a2e47cf654d1299a4c5638b34fb5e4b6
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508532"
---
# <a name="expressroute-routing-requirements"></a>Routeringsvereisten voor ExpressRoute
Als u ExpressRoute wilt gebruiken om verbinding te maken met Microsoft Cloud-services, moet u routering instellen en beheren. Sommige connectiviteitsproviders bieden het instellen en beheren van routering aan als een beheerde service. Neem contact op met uw connectiviteitsprovider om na te gaan of ze deze service leveren. Als dat niet het geval is, moet u voldoen aan de volgende vereisten:

Raadpleeg het artikel [Circuits and routing domains](expressroute-circuit-peerings.md) (Circuits en routeringsdomeinen) voor een beschrijving van de routeringssessies die moeten worden ingesteld om connectiviteit mogelijk te maken.

> [!NOTE]
> Microsoft biedt geen ondersteuning voor routerredundantieprotocollen (zoals HSRP, VRRP) voor de configuratie van hoge beschikbaarheid. We zijn afhankelijk van twee redundante BGP-sessies per peering voor maximale beschikbaarheid.
> 
> 

## <a name="ip-addresses-used-for-peerings"></a>IP-adressen die worden gebruikt voor peerings
U moet enkele blokken met IP-adressen reserveren om routering tussen uw netwerk en de MSEE-routers (Microsoft Enterprise Edge) te configureren. In deze sectie vindt u een lijst met vereisten en worden de regels beschreven met betrekking tot hoe u deze IP-adressen kunt verkrijgen en gebruiken.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP-adressen die worden gebruikt voor persoonlijke Azure-peering
U kunt privé IP-adressen of openbare IP-adressen gebruiken om de peerings te configureren. Het adresbereik dat wordt gebruikt voor het configureren van routes mag geen adresbereiken overlappen die worden gebruikt voor het maken van virtuele netwerken in Azure. 

* U moet een /29-subnet of twee /30-subnetten voor routeringsinterfaces reserveren.
* De subnetten voor routering kunnen privé of openbare IP-adressen zijn.
* De subnetten mogen geen conflicten opleveren met het bereik dat door de klant is gereserveerd voor gebruik in de Microsoft Cloud.
* Als er een /29-subnet wordt gebruikt, wordt dit verdeeld in twee /30-subnetten. 
  * Het eerste /30-subnet wordt gebruikt voor de primaire koppeling en het tweede /30-subnet voor de secundaire koppeling.
  * Voor beide /30-subnetten moet u het eerste IP-adres van het /30-subnet op de router gebruiken. Microsoft gebruikt het tweede IP-adres van het /30-subnet voor het instellen van een BGP-sessie.
  * Onze [beschikbaarheids-SLA](https://azure.microsoft.com/support/legal/sla/) is alleen geldig als beide BGP-sessies zijn ingesteld.  

#### <a name="example-for-private-peering"></a>Voorbeeld voor persoonlijke peering
Als u a.b.c.d/29 gebruikt om de peering in te stellen, wordt dit gesplitst in twee /30-subnetten. In het volgende voorbeeld ziet u hoe het subnet a.b.c.d/29 wordt gebruikt:

* a.b.c.d/29 wordt gesplitst in a.b.c.d/30 en a.b.c.d+4/30, en via de inrichting-API's doorgegeven aan Microsoft.
  * U gebruikt a.b.c.d+1 als de VRF-IP voor de primaire PE en Microsoft gebruikt a.b.c.d+2 als de VRF-IP voor de primaire MSEE.
  * U gebruikt a.b.c.d+5 als de VRF-IP voor de secundaire PE en Microsoft gebruikt a.b.c.d+6 als de VRF-IP voor de secundaire MSEE.

Stelt u zich een situatie voor waarin u 192.168.100.128/29 selecteert om persoonlijke peering in te stellen. 192.168.100.128/29 bevat adressen van 192.168.100.128 tot 192.168.100.135, waarbij:

* 192.168.100.128/30 wordt toegewezen aan link1, waarbij de provider 192.168.100.129 gebruikt en Microsoft 192.168.100.130 gebruikt.
* 192.168.100.132/30 wordt toegewezen aan link2, waarbij provider 192.168.100.133 gebruikt en Microsoft 192.168.100.134 gebruikt.

### <a name="ip-addresses-used-for-microsoft-peering"></a>IP-adressen die worden gebruikt voor Microsoft-peering
U moet voor het instellen van de BGP-sessies openbare IP-adressen gebruiken waarvan u eigenaar bent. Microsoft moet het eigenaarschap van de IP-adressen kunnen verifiëren via Routing Internet Registries en Internet Routing Registries.

* De IP-adressen die in de portal worden weergegeven voor geadverteerde openbare voorvoegsels voor Microsoft-Peering maken toegangsbeheerlijsten voor de Microsoft-corerouters die binnenkomend verkeer van deze IP-adressen toestaan. 
* U moet een uniek /29-subnet (IPv4) of /125-subnet (IPv6), of twee /30-subnetten (IPv4) of /126-subnetten (IPv6) gebruiken om de BGP-peering voor elke peering per ExpressRoute-circuit (als u er meer dan één hebt) in te stellen.
* Als er een /29-subnet wordt gebruikt, wordt dit verdeeld in twee /30-subnetten.
* Het eerste /30-subnet wordt gebruikt voor de primaire koppeling en het tweede /30-subnet voor de secundaire koppeling.
* Voor beide /30-subnetten moet u het eerste IP-adres van het /30-subnet op de router gebruiken. Microsoft gebruikt het tweede IP-adres van het /30-subnet voor het instellen van een BGP-sessie.
* Als er een /125-subnet wordt gebruikt, wordt dit verdeeld in twee /126-subnetten.
* Het eerste /126-subnet wordt gebruikt voor de primaire koppeling en het tweede /126-subnet voor de secundaire koppeling.
* Voor beide /126-subnetten moet u het eerste IP-adres van het /126-subnet op de router gebruiken. Microsoft gebruikt het tweede IP-adres van het /126-subnet voor het instellen van een BGP-sessie.
* Onze [beschikbaarheids-SLA](https://azure.microsoft.com/support/legal/sla/) is alleen geldig als beide BGP-sessies zijn ingesteld.

### <a name="ip-addresses-used-for-azure-public-peering"></a>IP-adressen die worden gebruikt voor openbare Azure-peering

> [!NOTE]
> Openbare Azure-peering is niet beschikbaar voor nieuwe circuits.
> 

U moet voor het instellen van de BGP-sessies openbare IP-adressen gebruiken waarvan u eigenaar bent. Microsoft moet het eigenaarschap van de IP-adressen kunnen verifiëren via Routing Internet Registries en Internet Routing Registries. 

* U moet een uniek /29-subnet of twee /30-subnetten gebruiken om de BGP-peering voor elke peering per ExpressRoute-circuit (als u er meer dan één hebt) in te stellen. 
* Als er een /29-subnet wordt gebruikt, wordt dit verdeeld in twee /30-subnetten. 
  * Het eerste /30-subnet wordt gebruikt voor de primaire koppeling en het tweede /30-subnet voor de secundaire koppeling.
  * Voor beide /30-subnetten moet u het eerste IP-adres van het /30-subnet op de router gebruiken. Microsoft gebruikt het tweede IP-adres van het /30-subnet voor het instellen van een BGP-sessie.
  * Onze [beschikbaarheids-SLA](https://azure.microsoft.com/support/legal/sla/) is alleen geldig als beide BGP-sessies zijn ingesteld.

## <a name="public-ip-address-requirement"></a>Vereiste openbaar IP-adres

### <a name="private-peering"></a>Persoonlijke peering
U kunt kiezen om openbare of persoonlijke IPv4-adressen te gebruiken voor persoonlijke peering. We bieden end-to-end-isolatie van uw verkeer, zodat het overlappen van adressen met andere klanten niet mogelijk is in het geval van persoonlijke peering. Deze adressen worden niet geadverteerd naar internet. 

### <a name="microsoft-peering"></a>Microsoft-peering
De Microsoft-peering kunt u verbinding maken met Microsoft-cloudservices. De lijst met services bevat Office 365-services, zoals Exchange Online, SharePoint Online, Skype voor Bedrijven en Dynamics 365. Microsoft ondersteunt bidirectionele connectiviteit op de Microsoft-peering. Verkeer dat is bestemd voor Microsoft Cloud-services, moet geldige openbare IPv4-adressen gebruiken voordat het het Microsoft-netwerk binnenkomt.

Controleer of uw IP-adres en AS-nummer in een van de volgende registers op uw naam zijn geregistreerd:

* [ARIN](https://www.arin.net/)
* [APNIC](https://www.apnic.net/)
* [AFRINIC](https://www.afrinic.net/)
* [LACNIC](https://www.lacnic.net/)
* [RIPENCC](https://www.ripe.net/)
* [RADB](https://www.radb.net/)
* [ALTDB](https://altdb.net/)

Als uw voorvoegsel en AS-nummer in voorgaande registers niet aan u zijn toegewezen, moet u een ondersteuningsaanvraag openen voor handmatige validatie van uw voorvoegsels en het ASN. De ondersteuning vraagt naar de vereiste documentatie, zoals een autorisatiebrief waaruit blijkt dat u de resources mag gebruiken.

Een persoonlijk AS-nummer is toegestaan met Microsoft-peering, maar moet ook handmatig worden gevalideerd. Bovendien verwijderen we persoonlijke AS-nummers in het AS-pad voor de ontvangen voorvoegsels. Hierdoor is het niet mogelijk om persoonlijke AS-nummers aan het AS-pad toe te voegen om [routering voor Microsoft-peering te beïnvloeden](expressroute-optimize-routing.md). 

> [!IMPORTANT]
> Niet dezelfde openbare IP-route met het openbare Internet en via ExpressRoute aangekondigd. Om het risico van onjuiste configuratie veroorzaakt asymmetrische routering, wordt sterk aangeraden de [NAT IP-adressen](expressroute-nat.md) geadverteerd aan Microsoft via ExpressRoute worden uit een bereik dat is niet geadverteerd naar internet helemaal. Als dit niet mogelijk om te realiseren, is het essentieel om te controleren of dat u een meer specifieke bereik aankondigen via ExpressRoute dan één op de verbinding met Internet. Naast de openbare route voor NAT, u kunt ook adverteren via ExpressRoute het openbare IP adressen die worden gebruikt door de servers in uw on-premises netwerk die met de eindpunten in Microsoft Office 365 communiceren. 
> 
> 

### <a name="public-peering-deprecated---not-available-for-new-circuits"></a>Openbare peering (afgeschaft - niet beschikbaar voor nieuwe circuits)
Met het pad voor openbare Azure-peering kunt u verbinding maken met alle services die via de openbare IP-adressen worden gehost in Azure. Deze lijst bevat services die worden vermeld in de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) en alle services die door ISV's worden gehost op Microsoft Azure. Connectiviteit met Microsoft Azure-services via openbare peering wordt altijd gestart vanuit uw netwerk naar het Microsoft-netwerk. U moet openbare IP-adressen gebruiken voor het verkeer dat bestemd is voor het Microsoft-netwerk.

> [!IMPORTANT]
> Alle Azure PaaS-services zijn toegankelijk via het Microsoft-peering.
>   

Een persoonlijke AS-nummer is toegestaan met openbare peering.

## <a name="dynamic-route-exchange"></a>Dynamische route-uitwisseling
Routeringsuitwisseling vindt plaats via het eBGP-protocol. EBGP-sessies worden tot stand gebracht tussen de MSEE's en uw routers. Verificatie van BGP-sessies is niet vereist. Indien nodig kan een MD5-hash worden geconfigureerd. Zie [Configure routing](how-to-routefilter-portal.md) (Routering configureren) en [Circuit provisioning workflows and circuit states](expressroute-workflows.md) (Werkstromen voor de inrichting van ExpressRoute-circuits en circuittoestanden) voor informatie over het configureren van BGP-sessies.

## <a name="autonomous-system-numbers"></a>Autonome systeemnummers
Microsoft gebruikt AS 12076 voor openbare Azure-peering, privé Azure-peering en Microsoft-peering. We hebben ASN's van 65515 tot 65520 gereserveerd voor intern gebruik. Zowel 16- als 32-bits AS-getallen worden ondersteund.

Er zijn geen vereisten met betrekking tot gegevensoverdrachtsymmetrie. De inkomende en uitgaande paden lopen mogelijk langs verschillende routerparen. Identieke routes moeten worden geadverteerd van beide zijden in meerdere circuit paren die van u. Route metrics hoeven niet identiek te zijn.

## <a name="route-aggregation-and-prefix-limits"></a>Limieten voor route-aggregatie en voorvoegsel
Wij ondersteunen maximaal 4000 voorvoegsels die aan ons zijn geadverteerd door middel van de persoonlijke Azure-peering. Dit aantal kan worden verhoogd tot 10.000 voorvoegsels als de Premium-invoegtoepassing voor ExpressRoute wordt ingeschakeld. We accepteren maximaal 200 voorvoegsels per BGP-sessie voor openbare Azure-peering en Microsoft-peering. 

De BGP-sessie wordt verwijderd als het aantal voorvoegsels de limiet overschrijdt. Standaardroutes worden alleen geaccepteerd op de persoonlijke peeringkoppeling. Provider moet standaardroute- en privé IP-adressen (RFC 1918) uit de paden voor openbare Azure- en Microsoft-peering filteren. 

## <a name="transit-routing-and-cross-region-routing"></a>Transitroutering en regio-overschrijdende routering
ExpressRoute kan niet worden geconfigureerd als transitrouter. Voor transitrouteringsservices bent u aangewezen op uw connectiviteitsprovider.

## <a name="advertising-default-routes"></a>Standaardroutes adverteren
Standaardroutes zijn alleen toegestaan voor persoonlijke Azure-peeringsessies. In dat geval wordt al het verkeer van de gekoppelde virtuele netwerken omgeleid naar uw netwerk. Wanneer standaardroutes worden geadverteerd naar persoonlijke peering, wordt het internetpad vanuit Azure geblokkeerd. Als u verkeer van en naar internet wilt routeren voor services die worden gehost in Azure, zult u gebruik moeten maken van uw bedrijfsfunctionaliteit. 

 Als u connectiviteit met andere Azure-services en infrastructuurservices wilt inschakelen, moet u een van de volgende voorzieningen treffen:

* Openbare Azure-peering is ingeschakeld om verkeer te routeren naar openbare eindpunten.
* U gebruikt door de gebruiker gedefinieerde routering om internetconnectiviteit toe te staan voor elk subnet dat internetconnectiviteit vereist.

> [!NOTE]
> Wanneer standaardroutes worden geadverteerd, wordt de activering van Windows- en andere VM-licenties verbroken. Volg [deze](https://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) instructies als u dit wilt omzeilen.
> 
> 

## <a name="bgp"></a>Ondersteuning voor BGP-community's
Deze sectie bevat een overzicht van hoe BGP-community's worden gebruikt met ExpressRoute. Microsoft adverteert routes in de paden voor openbare en Microsoft-peering waarbij de routes zijn gemarkeerd met de juiste communitywaarden. De reden hiervoor en meer informatie over communitywaarden worden hieronder beschreven. Microsoft erkent echter geen communitywaarden die zijn toegevoegd aan routes die worden geadverteerd aan Microsoft.

Als u op willekeurig welke locatie in een geopolitieke regio via ExpressRoute verbinding maakt met Microsoft, hebt u toegang tot alle Microsoft-cloudservices in alle regio's binnen de geopolitieke grenzen. 

Als u bijvoorbeeld via ExpressRoute bent verbonden met Microsoft in Amsterdam, hebt u toegang tot alle Microsoft Cloud-services die worden gehost in Europa - noord en Europa - west. 

Raadpleeg de pagina [ExpressRoute partners and peering locations](expressroute-locations.md) (Overzicht van ExpressRoute-partners en -peeringlocaties) voor een gedetailleerde lijst van de geopolitieke regio's, bijbehorende Azure-regio's en bijbehorende ExpressRoute-peeringlocaties.

U kunt meer dan één ExpressRoute-circuit per geopolitieke regio aanschaffen. Het hebben van meer verbindingen biedt aanzienlijke voordelen wat betreft hoge beschikbaarheid vanwege geografische redundantie. In gevallen waarin u meerdere ExpressRoute-circuits hebt, ontvangt u dezelfde set voorvoegsels geadverteerd vanuit Microsoft op de Microsoft-peering en openbare-peeringpad. Dat betekent dat er vanuit uw netwerk meerdere paden zijn naar Microsoft. Hierdoor kunnen er in uw netwerk suboptimale routeringsbeslissingen worden genomen. Dit kan leiden tot suboptimale connectiviteitservaringen met andere services. Op basis van de communitywaarden worden de juiste routeringsbeslissingen genomen voor [optimale routering naar gebruikers](expressroute-optimize-routing.md).

| **Microsoft Azure-regio** | **Regionale BGP-community** | **Opslag BGP-community** | **SQL-BGP-community 's** | **Cosmos DB BGP-community** |
| --- | --- | --- | --- | --- |
| **Noord-Amerika** | |
| US - oost | 12076:51004 | 12076:52004 | 12076:53004 | 12076:54004 |
| US - oost 2 | 12076:51005 | 12076:52005 | 12076:53005 | 12076:54005 |
| US - west | 12076:51006 | 12076:52006 | 12076:53006 | 12076:54006 |
| US - west 2 | 12076:51026 | 12076:52026 | 12076:53026 | 12076:54026 |
| US - west-centraal | 12076:51027 | 12076:52027 | 12076:53027 | 12076:54027 |
| US - noord-centraal | 12076:51007 | 12076:52007 | 12076:53007 | 12076:54007 |
| US - zuid-centraal | 12076:51008 | 12076:52008 | 12076:53008 | 12076:54008 |
| US - centraal | 12076:51009 | 12076:52009 | 12076:53009 | 12076:54009 |
| Canada - midden | 12076:51020 | 12076:52020 | 12076:53020 | 12076:54020 |
| Canada - oost | 12076:51021 | 12076:52021 | 12076:53021 | 12076:54021 |
| **Zuid-Amerika** | |
| Brazilië - zuid | 12076:51014 | 12076:52014 | 12076:53014 | 12076:54014 |
| **Europa** | |
| Europa - noord | 12076:51003 | 12076:52003 | 12076:53003 | 12076:54003 |
| Europa -west | 12076:51002 | 12076:52002 | 12076:53002 | 12076:54002 |
| Verenigd Koninkrijk Zuid | 12076:51024 | 12076:52024 | 12076:53024 | 12076:54024 |
| Verenigd Koninkrijk West | 12076:51025 | 12076:52025 | 12076:53025 | 12076:54025 |
| Frankrijk - centraal | 12076:51030 | 12076:52030 | 12076:53030 | 12076:54030 |
| Frankrijk - zuid | 12076:51031 | 12076:52031 | 12076:53031 | 12076:54031 |
| **Azië en Stille Oceaan** | |
| Azië - oost | 12076:51010 | 12076:52010 | 12076:53010 | 12076:54010 |
| Azië - zuidoost | 12076:51011 | 12076:52011 | 12076:53011 | 12076:54011 |
| **Japan** | |
| Japan - oost | 12076:51012 | 12076:52012 | 12076:53012 | 12076:54012 |
| Japan - west | 12076:51013 | 12076:52013 | 12076:53013 | 12076:54013 |
| **Australië** | |
| Australië - oost | 12076:51015 | 12076:52015 | 12076:53015 | 12076:54015 |
| Australië - zuidoost | 12076:51016 | 12076:52016 | 12076:53016 | 12076:54016 |
| **Australië - overheid** | |
| Australië - centraal | 12076:51032 | 12076:52032 | 12076:53032 | 12076:54032 |
| Australië - centraal 2 | 12076:51033 | 12076:52033 | 12076:53033 | 12076:54033 |
| **India** | |
| India - zuid | 12076:51019 | 12076:52019 | 12076:53019 | 12076:54019 |
| India - west | 12076:51018 | 12076:52018 | 12076:53018 | 12076:54018 |
| India - centraal | 12076:51017 | 12076:52017 | 12076:53017 | 12076:54017 |
| **Korea** | |
| Korea - zuid | 12076:51028 | 12076:52028 | 12076:53028 | 12076:54028 |
| Korea - centraal | 12076:51029 | 12076:52029 | 12076:53029 | 12076:54029 |
| **Zuid-Afrika**| |
| Zuid-Afrika (noord) | 12076:51034 | 12076:52034 | 12076:53034 | 12076:54034 |
| Zuid-Afrika (west) | 12076:51035 | 12076:52035 | 12076:53035 | 12076:54035 |
| **VAE**| |
| VAE - noord | 12076:51036 | 12076:52036 | 12076:53036 | 12076:54036 |
| VAE - centraal | 12076:51037 | 12076:52037 | 12076:53037 | 12076:54037 |


Alle routes die worden geadverteerd vanuit Microsoft, worden gemarkeerd met de juiste community-waarde. 

> [!IMPORTANT]
> Globale voorvoegsels worden gemarkeerd met een juiste community-waarde.
> 
> 

### <a name="service-to-bgp-community-value"></a>BGP-communitywaarde-service
Daarnaast worden voorvoegsels door Microsoft gemarkeerd op basis van de service waartoe ze behoren. Dit geldt alleen voor de Microsoft-peering. De volgende tabel bevat een toewijzing van services aan BGP-communitywaarden.

| **Service** | **BGP-communitywaarde** |
| --- | --- |
| Exchange Online | 12076:5010 |
| SharePoint Online | 12076:5020 |
| Skype voor Bedrijven Online | 12076:5030 |
| Dynamics 365 | 12076:5040 |
| Azure Global Services * | 12076:5050 |
| Andere online services van Office 365 | 12076:5100 |

\* Azure Global Services bevat alleen Azure DevOps op dit moment.


> [!NOTE]
> BGP-communitywaarden die u instelt op de routes die worden geadverteerd naar Microsoft, worden niet door Microsoft erkend.
> 
> 

### <a name="bgp-community-support-in-national-clouds"></a>Ondersteuning voor BGP-community's in nationale clouds

| **Nationale clouds Azure-regio**| **BGP-communitywaarde** |
| --- | --- |
| **Amerikaanse overheid** |  |
| VS (overheid) - Arizona | 12076:51106 |
| US Gov - Iowa | 12076:51109 |
| VS (overheid) - Virginia | 12076:51105 |
| VS (overheid) - Texas | 12076:51108 |
| US DoD - centraal | 12076:51209 |
| US DoD - oost | 12076:51205 |


| **Service in nationale clouds** | **BGP-communitywaarde** |
| --- | --- |
| **Amerikaanse overheid** |  |
| Exchange Online |12076:5110 |
| SharePoint Online |12076:5120 |
| Skype voor Bedrijven Online |12076:5130 |
| Dynamics 365 |12076:5140 |
| Andere online services van Office 365 |12076:5200 |

## <a name="next-steps"></a>Volgende stappen
* Configureer uw ExpressRoute-verbinding.
  
  * [Een circuit maken en wijzigen](expressroute-howto-circuit-arm.md)
  * [Een peeringconfiguratie maken en wijzigen](expressroute-howto-routing-arm.md)
  * [Een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)
