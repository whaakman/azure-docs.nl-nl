---
title: Azure ExpressRoute-circuits en Routeringsdomeinen | Microsoft Docs
description: Deze pagina bevat een overzicht van ExpressRoute-circuits en de Routeringsdomeinen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: cherylmc
ms.openlocfilehash: c1fbc99702e077e600f01959224f3321e7543e59
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044396"
---
# <a name="expressroute-circuits-and-routing-domains"></a>ExpressRoute-circuits en Routeringsdomeinen
 U moet worden besteld een *ExpressRoute-circuit* om uw on-premises infrastructuur naar Microsoft via een connectiviteitsprovider verbinding te maken. De volgende afbeelding ziet u een logische representatie van connectiviteit tussen uw WAN en Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>ExpressRoute-circuits
Een *ExpressRoute-circuit* vertegenwoordigt een logische verbinding tussen uw on-premises infrastructuur en Microsoft cloud-services via een connectiviteitsprovider. U kunt meerdere ExpressRoute-circuits bestellen. Elk circuit kan in de dezelfde of verschillende regio's, en kan worden verbonden met uw locatie via verschillende connectiviteitsproviders. 

ExpressRoute-circuits toewijzen niet aan een fysieke entiteiten. Een circuit wordt uniek geïdentificeerd door een standaard die GPT als een servicesleutel (s-sleutel aangeroepen). De servicesleutel is de enige informatie die worden uitgewisseld tussen Microsoft en de connectiviteitsprovider. De s-sleutel is niet een geheim om veiligheidsredenen. Er is een 1:1-toewijzing tussen een ExpressRoute-circuit en de s-sleutel.

Een ExpressRoute-circuit kan maximaal drie onafhankelijke peerings hebben: Azure openbare, Azure privé- en Microsoft. Elke peering is een combinatie van een onafhankelijke BGP sessies van deze toch toe geconfigureerd voor hoge beschikbaarheid. Er is een 1: n (1 < = N < = 3) toewijzing tussen een ExpressRoute-circuit en routering van domeinen. Een ExpressRoute-circuit kan hebben een, twee of alle drie de peerings per ExpressRoute-circuit is ingeschakeld.

Elk circuit heeft een vaste bandbreedte (50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 10 Gbps) en is toegewezen aan een connectiviteitsprovider en een locatie. De bandbreedte die u selecteert, wordt gedeeld tussen alle peerings voor het circuit. 

### <a name="quotas-limits-and-limitations"></a>Quota's, limieten en beperkingen
Standaardquota en limieten gelden voor elk ExpressRoute-circuit. Raadpleeg de [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../azure-subscription-service-limits.md) pagina voor actuele informatie over quota.

## <a name="expressroute-routing-domains"></a>ExpressRoute-routeringsdomeinen
Een ExpressRoute-circuit heeft meerdere Routeringsdomeinen die ermee verbonden zijn: Azure openbare, Azure privé- en Microsoft. Elk van de Routeringsdomeinen identiek is geconfigureerd op twee routers (in de actief / actief- of delen van belasting configureren) voor hoge beschikbaarheid. Azure-services worden aangemerkt als *openbare Azure* en *Azure privé* om weer te geven van de IP-adressen van schema's.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="azure-private-peering"></a>Persoonlijke Azure-peering
Azure compute-services, namelijk virtuele machines (IaaS) en cloudservices (PaaS), die zijn geïmplementeerd in een virtueel netwerk kunnen worden verbonden via het domein voor persoonlijke peering. Het domein voor persoonlijke peering wordt beschouwd als een vertrouwde uitbreiding van uw Basisnetwerk in Microsoft Azure. U kunt bidirectionele connectiviteit tussen uw Basisnetwerk en de Azure-netwerken (VNets) instellen. Deze peering, kunt u verbinding maken met virtuele machines en services rechtstreeks op hun persoonlijke IP-adressen in de cloud.  

U kunt meer dan één virtueel netwerk verbinden met het domein voor persoonlijke peering. Controleer de [pagina met veelgestelde vragen](expressroute-faqs.md) voor meer informatie over limieten en beperkingen. U kunt de [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../azure-subscription-service-limits.md) pagina voor actuele informatie over limieten.  Raadpleeg de [Routing](expressroute-routing.md) pagina voor gedetailleerde informatie over de configuratie van de routering.

### <a name="microsoft-peering"></a>Microsoft-peering

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Er is een verbinding met Microsoft online services (Office 365, Dynamics 365 en Azure PaaS-services) via de Microsoft-peering. We inschakelen bidirectionele connectiviteit tussen uw WAN- en Microsoft cloud-services via de Microsoft-peering routering domein. U moet verbinding maken met Microsoft cloud-services alleen via het openbare IP-adressen die eigendom zijn van u of uw connectiviteitsprovider en u moet voldoen aan alle gedefinieerde regels. Zie voor meer informatie de [vereisten voor ExpressRoute](expressroute-prerequisites.md) pagina.

Zie de [pagina met veelgestelde vragen](expressroute-faqs.md) voor meer informatie over de services die worden ondersteund, kosten en informatie over de configuratie. Zie de [ExpressRoute-locaties](expressroute-locations.md) pagina voor meer informatie over de lijst van connectiviteitsproviders bieden ondersteuning voor Microsoft-peering.

### <a name="azure-public-peering"></a>Openbare Azure-peering (afgeschaft voor nieuwe circuits)

> [!IMPORTANT]
> Openbare Azure-peering is niet beschikbaar voor nieuwe circuits.  
>   

Services zoals Azure Storage, SQL-databases en Websites worden aangeboden op openbare IP-adressen. U kunt privé verbinding maken met services die worden gehost op openbare IP-adressen, met inbegrip van VIP's van uw cloudservices, via de openbare peering routeringsdomein. U kunt het openbare peering domein verbinden met uw Perimeternetwerk en verbinding maken met alle Azure-services op hun openbare IP-adressen van uw WAN zonder verbinding maken via internet. 

Connectiviteit wordt altijd gestart vanuit uw WAN met Microsoft Azure-services. Microsoft Azure-services worden niet verbindingen in uw netwerk via de deze routeringsdomein kunnen initiëren. Zodra de openbare peering is ingeschakeld, kunt u verbinding met alle Azure-services. We kunt niet u selectief kiezen services waarvoor we routes te adverteren.

U kunt aangepaste routefilters definiëren in uw netwerk te gebruiken alleen de routes die u nodig hebt. Raadpleeg de [Routing](expressroute-routing.md) pagina voor gedetailleerde informatie over de configuratie van de routering. 

Zie voor meer informatie over de services die worden ondersteund via de openbare peering routeringsdomein, de [Veelgestelde vragen over](expressroute-faqs.md).

## <a name="routing-domain-comparison"></a>Vergelijking van de routering domein
De volgende tabel vergelijkt de drie Routeringsdomeinen:

|  | **Persoonlijke Peering** | **Microsoft-Peering** |  **Openbare Peering** (afgeschaft voor circuits) |
| --- | --- | --- | --- |
| **Max. # voorvoegsels per peering ondersteund** |4000 standaard 10.000 met ExpressRoute Premium |200 |200 |
| **IP-adresbereiken ondersteund** |Een geldig IP-adres binnen uw WAN. |Openbare IP-adressen die eigendom zijn door u of uw connectiviteitsprovider. |Openbare IP-adressen die eigendom zijn door u of uw connectiviteitsprovider. |
| **Aantal vereisten** |Persoonlijke en openbare AS-nummers. U moet de eigenaar bent van de openbare AS-nummer als u ervoor kiest een te gebruiken. |Persoonlijke en openbare AS-nummers. U moet echter bewijzen eigendom van openbare IP-adressen. |Persoonlijke en openbare AS-nummers. U moet echter bewijzen eigendom van openbare IP-adressen. |
| **IP-protocollen die worden ondersteund**| IPv4 |  IPv4, IPv6 | IPv4 |
| **Routering Interface IP-adressen** |RFC1918 en openbare IP-adressen |Openbare IP-adressen in routeringsregisters voor u geregistreerd. |Openbare IP-adressen in routeringsregisters voor u geregistreerd. |
| **MD5-Hash-ondersteuning** |Ja |Ja |Ja |

U kunt een of meer van de Routeringsdomeinen inschakelen als onderdeel van uw ExpressRoute-circuit. U kunt kiezen om alle Routeringsdomeinen plaats op de dezelfde VPN-verbinding als u wilt combineren in één routeringsdomein. U kunt ze ook op verschillende Routeringsdomeinen, vergelijkbaar met het diagram plaatsen. De aanbevolen configuratie is dat privépeering rechtstreeks met het Basisnetwerk verbonden is en de openbare en Microsoft peeringkoppelingen zijn verbonden met uw DMZ.

Als u dat alle drie peering sessies wilt, moet u drie paren van BGP-sessies (een paar voor elk type peering) hebben. De BGP-sessie-paren bieden een maximaal beschikbare koppeling. Als u verbinding via layer 2-connectiviteitsproviders maakt, bent u verantwoordelijk voor het configureren en beheren van routering. U kunt meer informatie aan de hand van de [werkstromen](expressroute-workflows.md) voor het instellen van ExpressRoute.

## <a name="expressroute-health"></a>ExpressRoute-status
ExpressRoute-circuits mogen worden bewaakt voor beschikbaarheid, verbinding met vnet's en het gebruik van bandbreedte gebruik [Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM).

NPM controleert de status van persoonlijke Azure-peering en Microsoft-peering.  Bekijk onze [boeken](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* Zoek een serviceprovider Zie [ExpressRoute-providers en -locaties service](expressroute-locations.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).
* Configureer uw ExpressRoute-verbinding.
  * [Create and manage ExpressRoute circuits](expressroute-howto-circuit-portal-resource-manager.md) (ExpressRoute-circuits maken en beheren)
  * [Routering (peering) configureren voor ExpressRoute-circuits](expressroute-howto-routing-portal-resource-manager.md)

