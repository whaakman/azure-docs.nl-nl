---
title: Azure cross-netwerkverbinding | Microsoft Docs
description: Deze pagina wordt een toepassingsscenario voor cross-netwerkverbinding en de oplossing op basis van de Azure VPN-functies beschreven.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/03/2019
ms.author: rambala
ms.openlocfilehash: 4923c7f2048b7368af6314d5e2288216115bc3bc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59784736"
---
# <a name="cross-network-connectivity"></a>Onderlinge netwerkconnectiviteit

Fabrikam, Inc. is een grote fysieke aanwezigheid en Azure-implementatie in VS-Oost. Fabrikam is back-end-connectiviteit tussen de on-premises en Azure-implementaties via ExpressRoute. Contoso Ltd. heeft op dezelfde manier een aanwezigheid en een Azure-implementatie in VS-West. Contoso heeft een back-end-connectiviteit tussen de on-premises en Azure-implementaties via ExpressRoute.  

Fabrikam Inc. acquires Contoso Ltd. Na de fusie wil Fabrikam interconnect de netwerken. De volgende afbeelding ziet u het scenario:

 [![1]][1]

De onderbroken pijlen in het midden van de bovenstaande afbeelding geven aan de gewenste netwerk onderlinge verbindingen. Er zijn drie typen cross verbindingen nodig: Fabrikam en Contoso-VNets cross-verbinding van 1), 2) tussen regionale on-premises en vnet's verbindt cross (die is Fabrikam on-premises netwerk verbinden met Contoso VNet en Contoso on-premises netwerk verbinden met Fabrikam-VNet), en (3) Fabrikam en Contoso on-premises netwerk cross-verbinding maken. 

De volgende tabel ziet u de routetabel van de persoonlijke peering van het ExpressRoute van Contoso Ltd., voordat de fusie.

[![2]][2]

De volgende tabel ziet u de effectieve routes van een virtuele machine in de Contoso-abonnement, voordat de fusie. Per van de tabel is de virtuele machine op het VNet op de hoogte van de VNet-adresruimte en de on-premises-netwerk van Contoso, naast de standaardwaarden. 

[![4]][4]

De volgende tabel ziet u de routetabel van de persoonlijke peering van het ExpressRoute van Fabrikam Inc., voordat de fusie.

[![3]][3]

De volgende tabel ziet u de effectieve routes van een virtuele machine in het Fabrikam-abonnement voordat u de fusie. Per van de tabel is de virtuele machine op het VNet op de hoogte van de VNet-adresruimte en Fabrikam on-premises netwerk, naast de standaardwaarden.

[![5]][5]

In dit artikel gaat via Stapsgewijze instructies en informatie over het bereiken van de gewenste meerdere verbindingen met de volgende functies van Azure-netwerk:

* [Peering op virtueel netwerk][Virtual network peering] 
* [Virtueel netwerk ExpressRoute-verbinding][connection]
* [Globaal bereik][Global Reach] 

## <a name="cross-connecting-vnets"></a>Cross-VNets verbinden

Peering op virtueel netwerk (VNet-peering) biedt de meest optimale en de beste prestaties van het netwerk bij het verbinden van twee virtuele netwerken. VNet-peering ondersteunt twee Vnetten binnen de dezelfde Azure-regio (vaak genoemd VNet-peering) en in twee verschillende Azure-regio's (vaak genoemd wereldwijde VNet-peering). 

We gaan configureren globale VNet-peering tussen de VNets in Contoso en Fabrikam-Azure-abonnementen. Voor informatie over het maken van het virtuele netwerk-peering tussen twee virtuele netwerken kunt u vinden [maken van een virtueel netwerk-peering] [ Configure VNet peering] artikel.

De volgende afbeelding toont de netwerkarchitectuur na het configureren van wereldwijde VNet-peering.

[![6]][6]

De volgende tabel ziet de routes bekend zijn in het Contoso-abonnement VM. Let op het laatste item van de tabel. Dit item is het resultaat van tussen de virtuele netwerken met elkaar verbinden.

[![7]][7]

De volgende tabel ziet de routes bekend zijn in het abonnement Fabrikam VM. Let op het laatste item van de tabel. Dit item is het resultaat van tussen de virtuele netwerken met elkaar verbinden.

[![8]][8]

VNet-peering rechtstreeks koppelt twee virtuele netwerken (Zie er zijn geen volgende hop voor *VNetGlobalPeering* vermelding in de bovenstaande twee tabellen)

## <a name="cross-connecting-vnets-to-the-on-premises-networks"></a>Cross-VNets verbinden met de on-premises netwerken

We kunnen een ExpressRoute-circuit verbinding maken met meerdere virtuele netwerken. Zie [abonnement en Servicelimieten] [ Subscription limits] voor het maximum aantal virtuele netwerken die kunnen worden verbonden met een ExpressRoute-circuit. 

Laten we verbinden met Fabrikam ExpressRoute-circuit tot Contoso abonnement VNet en op dezelfde manier Contoso ExpressRoute-circuit Fabrikam abonnement VNet om in te schakelen tussen connectiviteit tussen virtuele netwerken en de on-premises netwerken. Voor een virtueel netwerk verbinding met een ExpressRoute-circuit in een ander abonnement, moeten we het maken en gebruiken van een autorisatie.  Zie het artikel: [Een virtueel netwerk verbinden met een ExpressRoute-circuit][Connect-ER-VNet].

De volgende afbeelding ziet u de netwerkarchitectuur na het configureren van de ExpressRoute cross-connectiviteit naar de virtuele netwerken.

[![9]][9]

De volgende tabel ziet u de routetabel van de persoonlijke peering van het ExpressRoute van Contoso Ltd., na meerdere virtuele netwerken verbinden met de on-premises netwerken via ExpressRoute. Zie dat de routetabel bevat routes die behoren tot de virtuele netwerken.

[![10]][10]

De volgende tabel ziet u de routetabel van de persoonlijke peering van het ExpressRoute van Fabrikam Inc., na meerdere virtuele netwerken verbinden met de on-premises netwerken via ExpressRoute. Zie dat de routetabel bevat routes die behoren tot de virtuele netwerken.

[![11]][11]

De volgende tabel ziet de routes bekend zijn in het Contoso-abonnement VM. Let op de *gateway van virtueel netwerk* vermeldingen van de tabel. De virtuele machine ziet routes voor zowel de on-premises netwerken.

[![12]][12]

De volgende tabel ziet de routes bekend zijn in het abonnement Fabrikam VM. Let op de *gateway van virtueel netwerk* vermeldingen van de tabel. De virtuele machine ziet routes voor zowel de on-premises netwerken.

[![13]][13]

>[!NOTE]
>In een knooppunt de Fabrikam en/of Contoso-abonnementen die u kunt ook hebben VNets naar de respectieve hub VNet (een hub en spoke-ontwerp wordt niet weergegeven in de architectuurdiagrammen in dit artikel). De cross-verbindingen tussen de hub VNet-gateways met ExpressRoute kunnen ook communicatie tussen Oost en West-hubs en knooppunten.
>

## <a name="cross-connecting-on-premises-networks"></a>Cross-verbinding maakt on-premises netwerken

Globaal bereik ExpressRoute biedt connectiviteit tussen on-premises-netwerken die zijn verbonden met andere ExpressRoute-circuits. We gaan configureren globaal bereik tussen Contoso en Fabrikam ExpressRoute-circuits. Omdat de ExpressRoute-circuits zich in verschillende abonnementen bevinden, moet het maken en gebruiken van een autorisatie. Zie [ExpressRoute globaal bereik configureren] [ Configure Global Reach] artikel voor stapsgewijze instructies.

De volgende afbeelding toont de netwerkarchitectuur na het configureren van globaal bereik.

[![14]][14]

De volgende tabel ziet u de routetabel van de persoonlijke peering van het ExpressRoute van Contoso Ltd., na het configureren van globaal bereik. Zie dat de routetabel bevat routes die behoren tot zowel de on-premises netwerken. 

[![15]][15]

De volgende tabel ziet u de routetabel van de persoonlijke peering van het ExpressRoute van Fabrikam Inc., na het configureren van globaal bereik. Zie dat de routetabel bevat routes die behoren tot zowel de on-premises netwerken.

[![16]][16]

## <a name="next-steps"></a>Volgende stappen

Zie [Veelgestelde vragen over virtuele netwerken][VNet-FAQ]voor verdere vragen over VNet-peering en VNet-peering. Zie [Veelgestelde vragen over ExpressRoute] [ ER-FAQ] voor verdere vragen over ExpressRoute en virtuele netwerkverbinding.

Wereldwijd bereik wordt geïmplementeerd op basis van per land. Als u wilt zien als globaal bereik is beschikbaar in de landen die u wilt, Zie [ExpressRoute globaal bereik][Global Reach].

<!--Image References-->
[1]: ./media/cross-network-connectivity/premergerscenario.png "scenario van de toepassing"
[2]: ./media/cross-network-connectivity/contosoexr-rt-premerger.png "Contoso ExpressRoute routetabel vóór de fusie"
[3]: ./media/cross-network-connectivity/fabrikamexr-rt-premerger.png "Fabrikam ExpressRoute routetabel vóór de fusie"
[4]: ./media/cross-network-connectivity/contosovm-routes-premerger.png "Contoso VM routes vóór de fusie"
[5]: ./media/cross-network-connectivity/fabrikamvm-routes-premerger.png "Fabrikam VM routes vóór de fusie"
[6]: ./media/cross-network-connectivity/vnet-peering.png "de architectuur na de VNet-peering"
[7]: ./media/cross-network-connectivity/contosovm-routes-peering.png "Contoso VM routes na de VNet-peering"
[8]: ./media/cross-network-connectivity/fabrikamvm-routes-peering.png "Fabrikam VM routes na de VNet-peering"
[9]: ./media/cross-network-connectivity/exr-x-connect.png "de architectuur na expressroutes waaraan cross-verbinding"
[10]: ./media/cross-network-connectivity/contosoexr-rt-xconnect.png "Contoso ExpressRoute routetabel na meerdere verbindende ExR en VNets"
[11]: ./media/cross-network-connectivity/fabrikamexr-rt-xconnect.png "Fabrikam ExpressRoute routetabel na meerdere verbindende ExR en VNets"
[12]: ./media/cross-network-connectivity/contosovm-routes-xconnect.png "routes Contoso VM na het maken van verbinding ExR en VNets tussen"
[13]: ./media/cross-network-connectivity/fabrikamvm-routes-xconnect.png "routes Fabrikam VM na het maken van verbinding ExR en VNets tussen"
[14]: ./media/cross-network-connectivity/globalreach.png "de architectuur na het configureren van globaal bereik"
[15]: ./media/cross-network-connectivity/contosoexr-rt-gr.png "Contoso ExpressRoute routetabel na globaal bereik"
[16]: ./media/cross-network-connectivity/fabrikamexr-rt-gr.png "Fabrikam ExpressRoute route table after Global Reach"

<!--Link References-->
[Virtual network peering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview
[connection]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure VNet peering]: https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[Subscription limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits
[Connect-ER-VNet]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[ER-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[VNet-FAQ]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq