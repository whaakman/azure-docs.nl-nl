---
title: Azure ExpressRoute-circuits en Routeringsdomeinen | Microsoft Docs
description: Deze pagina bevat een overzicht van ExpressRoute-circuits en de Routeringsdomeinen.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6f0c5d8e-cc60-4a04-8641-2c211bda93d9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2017
ms.author: ganesr,cherylmc
ms.openlocfilehash: c8f3c0e87a052b327e9949acd3e7db1d28c1eb46
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="expressroute-circuits-and-routing-domains"></a>ExpressRoute-circuits en Routeringsdomeinen
 U moet bestellen een *ExpressRoute-circuit* uw on-premises infrastructuur verbinding te maken met Microsoft via een connectiviteitsprovider. De volgende afbeelding ziet een logische representatie van de verbinding tussen uw WAN en Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>ExpressRoute-circuits
Een *ExpressRoute-circuit* vertegenwoordigt een logische verbinding tussen uw on-premises infrastructuur en Microsoft cloud-services via een connectiviteitsprovider. U kunt meerdere ExpressRoute-circuits bestellen. Elk circuit kan zich in dezelfde of verschillende regio's en kan worden verbonden met uw lokale via verschillende connectiviteitsproviders. 

ExpressRoute-circuits niet aan een fysieke entiteiten zijn toegewezen. Een circuit wordt uniek geïdentificeerd door een standaard die GUID worden aangeroepen als een servicesleutel (s-sleutel). De servicesleutel is de enige informatie uitgewisseld tussen Microsoft en de connectiviteitsprovider. De s-sleutel is niet een geheim om beveiligingsredenen. Er is een 1:1-toewijzing tussen een ExpressRoute-circuit en de s-sleutel.

Een ExpressRoute-circuit kan maximaal drie onafhankelijke peerings hebben: Azure openbare, Azure privé- en Microsoft. Elke peering is een combinatie van BGP onafhankelijke sessies van deze toch toe geconfigureerd voor maximale beschikbaarheid. Er is een 1: n (1 < = N < = 3) toewijzing tussen een ExpressRoute-circuit en routering van domeinen. Een ExpressRoute-circuit kan hebben een, twee of alle drie de peerings per ExpressRoute-circuit is ingeschakeld.

Elk circuit heeft een vaste bandbreedte (50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 10 Gbps) en is toegewezen aan een connectiviteitsprovider en een locatie. De bandbreedte die u selecteert worden verdeeld over de peerings voor het circuit. 

### <a name="quotas-limits-and-limitations"></a>Quota's, limieten en beperkingen
Standaardquota en limieten gelden voor elk ExpressRoute-circuit. Raadpleeg de [Azure-abonnement en Service-limieten, quota's en beperkingen](../azure-subscription-service-limits.md) pagina voor actuele informatie over quota.

## <a name="expressroute-routing-domains"></a>ExpressRoute-routeringsdomeinen
Een ExpressRoute-circuit heeft meerdere Routeringsdomeinen gekoppeld: Azure openbare, Azure privé- en Microsoft. Elk van de Routeringsdomeinen identiek is geconfigureerd op een combinatie van routers (in de actieve of loadsharing configuration) voor hoge beschikbaarheid. Azure-services zijn aangemerkt als *openbare Azure* en *Azure privé* vertegenwoordigt het IP-adresschema's.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="azure-private-peering"></a>Azure persoonlijke peering
Azure compute-services, namelijk virtuele machines (IaaS) en cloudservices (PaaS), die zijn geïmplementeerd in een virtueel netwerk kunnen worden verbonden via het persoonlijke peering domein. Het domein voor persoonlijke peering wordt beschouwd als een vertrouwde uitbreiding van uw Basisnetwerk in Microsoft Azure. U kunt instellen wanneer er bidirectionele connectiviteit tussen uw Basisnetwerk en de virtuele netwerken van Azure (vnet's). Deze peering, kunt u verbinding maken met virtuele machines en cloudservices rechtstreeks op hun persoonlijke IP-adressen.  

U kunt meer dan één virtueel netwerk verbinden met het domein voor persoonlijke peering. Controleer de [pagina met veelgestelde vragen](expressroute-faqs.md) voor meer informatie over limieten en beperkingen. U kunt de [Azure-abonnement en Service-limieten, quota's en beperkingen](../azure-subscription-service-limits.md) pagina voor actuele informatie over limieten.  Raadpleeg de [routering](expressroute-routing.md) pagina voor gedetailleerde informatie over de configuratie van de routering.

### <a name="azure-public-peering"></a>Openbare Azure-peering

> [!IMPORTANT]
> Alle Azure PaaS-services zijn ook toegankelijk via Microsoft-peering. We raden u aan Microsoft-peering in te stellen en via Microsoft-peering verbinding te maken met Azure PaaS-services.  
>   


Services zoals Azure Storage, SQL-databases en Websites worden aangeboden op het openbare IP-adressen. U kunt privé verbinding maken met services die worden gehost op openbare IP-adressen, met inbegrip van VIP's van uw cloud-services via openbare peering routeringsdomein. U kunt verbinding maken met het openbare peering domein uw Perimeternetwerk en verbinding maken met alle Azure-services op hun openbare IP-adressen van uw WAN zonder verbinding maken via internet. 

Connectiviteit wordt altijd gestart vanuit uw WAN met Microsoft Azure-services. Microsoft Azure-services kan niet worden verbindingen in uw netwerk via deze routeringsdomein te initiëren. Zodra openbare peering is ingeschakeld, kunt u verbinding met alle Azure-services. We kunt niet u services waarvoor adverteren we de routes naar selectief te kiezen.

U kunt aangepaste routefilters definiëren in uw netwerk alleen de routes die u moet gebruiken. Raadpleeg de [routering](expressroute-routing.md) pagina voor gedetailleerde informatie over de configuratie van de routering. 

Zie de [pagina met veelgestelde vragen](expressroute-faqs.md) voor meer informatie over services die worden ondersteund via openbare peering routeringsdomein. 

### <a name="microsoft-peering"></a>Microsoft-peering
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Er is een verbinding met alle andere Microsoft online services (Office 365, Dynamics 365 en Azure PaaS-services) via de Microsoft-peering. We inschakelen bidirectionele connectiviteit tussen uw WAN- en Microsoft cloud-services via het Microsoft peering routeringsdomein. U moet verbinding maken met Microsoft cloud-services alleen via het openbare IP-adressen die eigendom zijn van u of uw connectiviteitsprovider en moet u voldoen aan alle gedefinieerde regels. Zie de [vereisten voor ExpressRoute](expressroute-prerequisites.md) pagina voor meer informatie.

Zie de [pagina met veelgestelde vragen](expressroute-faqs.md) voor meer informatie over services die worden ondersteund, kosten en configuratie-informatie. Zie de [ExpressRoute-locaties](expressroute-locations.md) pagina voor informatie over de lijst van connectiviteitsproviders met Microsoft ondersteuning.

## <a name="routing-domain-comparison"></a>Vergelijking van routering domein
De volgende tabel vergelijkt de drie Routeringsdomeinen:

|  | **Persoonlijke Peering** | **Openbare Peering** | **Microsoft-Peering*** |
| --- | --- | --- | --- |
| **Max. # voorvoegsels per peering ondersteund** |4000 standaard 10.000 met ExpressRoute Premium |200 |200 |
| **IP-adresbereiken ondersteund** |Een geldig IP-adres binnen uw WAN. |Openbare IP-adressen die eigendom zijn van door u of uw connectiviteitsprovider. |Openbare IP-adressen die eigendom zijn van door u of uw connectiviteitsprovider. |
| **Als een aantal vereisten** |Persoonlijke en openbare AS-nummers. U moet eigenaar van de openbare AS-nummer als u ervoor kiest een te gebruiken. |Persoonlijke en openbare AS-nummers. U moet echter bewijzen dat eigendom van het openbare IP-adressen. |Persoonlijke en openbare AS-nummers. U moet echter bewijzen dat eigendom van het openbare IP-adressen. |
| **IP-protocollen die worden ondersteund**| IPv4 | IPv4 | IPv4, IPv6 |
| **Routering Interface IP-adressen** |RFC1918 en openbare IP-adressen |Openbare IP-adressen voor u geregistreerd in routeringsregisters. |Openbare IP-adressen voor u geregistreerd in routeringsregisters. |
| **MD5-Hash-ondersteuning** |Ja |Ja |Ja |

(*) De laag Premium-invoegtoepassing SKU is vereist

U kunt een of meer van de Routeringsdomeinen inschakelen als onderdeel van uw ExpressRoute-circuit. U kunt alle Routeringsdomeinen plaatsen op de dezelfde VPN-verbinding als u wilt samenvoegen tot één routeringsdomein. U kunt ze ook op andere Routeringsdomeinen, vergelijkbaar met het diagram plaatsen. De aanbevolen configuratie is dat privépeering rechtstreeks is verbonden met het Basisnetwerk en de openbare en Microsoft-peering koppelingen zijn verbonden met uw Perimeternetwerk.

Als u ervoor kiest om alle drie peeringsessies, moet u drie paren van BGP-sessies (één paar voor elk type peering) hebben. De BGP-sessie paren geeft u een maximaal beschikbare koppeling. Als u verbinding via layer 2-connectiviteitsproviders maakt, bent u verantwoordelijk voor het configureren en beheren van routering. U kunt meer informatie aan de hand van de [werkstromen](expressroute-workflows.md) voor het instellen van ExpressRoute.

## <a name="next-steps"></a>Volgende stappen
* Zoek een serviceprovider Zie [ExpressRoute-providers en -locaties service](expressroute-locations.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).
* Configureer uw ExpressRoute-verbinding.
  * [Create and manage ExpressRoute circuits](expressroute-howto-circuit-portal-resource-manager.md) (ExpressRoute-circuits maken en beheren)
  * [Routering (peering) configureren voor ExpressRoute-circuits](expressroute-howto-routing-portal-resource-manager.md)

