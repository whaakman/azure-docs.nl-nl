---
title: Uw on-premises netwerk verbinden met Azure | Microsoft Docs
description: Uitleg en beschrijving van de verschillende methoden voor het verbinden met Microsoft-cloudservices zoals Azure, Office 365 en Dynamics CRM Online.
services: ''
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: ''
ms.assetid: 294d39ad-40e0-476a-a362-57911b1172b7
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: cherylmc
ms.openlocfilehash: 9361c0b2ee84d7459bd432f5a8ce13fc5fd85ef8
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "23851013"
---
# <a name="connecting-your-on-premises-network-to-azure"></a>Uw on-premises netwerk verbinden met Azure
Microsoft biedt verschillende soorten cloudservices. Terwijl u verbinding voor alle services via het openbare Internet maken kunt, kunt u ook verbinding maken met een van de services met behulp van een virtueel particulier netwerk (VPN)-tunnel via Internet of via een directe, particuliere verbinding met Microsoft. Dit artikel helpt u bepalen welke verbindingsoptie het beste voldoet aan uw behoeften op basis van de typen Microsoft-cloudservices die u verbruikt. De meeste organisaties maken gebruik van meerdere verbindingstypen die hieronder worden beschreven.

## <a name="connecting-over-the-public-internet"></a>Verbinding maken via het openbare Internet
Dit verbindingstype biedt toegang tot Microsoft-cloudservices rechtstreeks via Internet, zoals hieronder wordt weergegeven.

![Internet](./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Deze verbinding is meestal het eerste dat wordt gebruikt voor het verbinden met Microsoft-cloudservices. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Voordelen** | **Overwegingen** |
| --- | --- |
| Vereist geen wijzigingen aan uw on-premises netwerk, zolang alle clientapparaten hebben onbeperkte toegang tot alle IP-adressen en poorten op het Internet. |Al het verkeer wordt vaak versleuteld via HTTPS, kan deze worden onderschept onderweg zijn omdat deze het openbare Internet passeert. |
| Kan verbinding maken met alle Microsoft cloud-services toegang heeft tot het openbare Internet. |Onvoorspelbare latentie omdat de verbinding gaat via Internet. |
| Maakt gebruik van uw bestaande internetverbinding. | |
| Beheer van apparaten connectiviteit is geen vereist. | |

Deze verbinding heeft geen connectiviteit of de bandbreedtekosten, omdat u uw bestaande internetverbinding gebruiken.

## <a name="connecting-with-a-point-to-site-connection"></a>Verbinding maken met een punt-naar-site-verbinding
Dit verbindingstype biedt toegang tot sommige Microsoft-cloudservices via een Secure Socket Tunneling Protocol (SSTP)-tunnel via Internet, zoals hieronder wordt weergegeven.

![P2S](./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "punt-naar-site-verbinding")

De verbinding wordt gemaakt via uw bestaande internetverbinding, maar het gebruik van een Azure VPN-Gateway is vereist. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Voordelen** | **Overwegingen** |
| --- | --- |
| Vereist geen wijzigingen aan uw on-premises netwerk, zolang alle clientapparaten hebben onbeperkte toegang tot alle IP-adressen en poorten op het Internet. |Al het verkeer wordt versleuteld met behulp van IPSec, kan deze worden onderschept onderweg zijn omdat deze het openbare Internet passeert. |
| Maakt gebruik van uw bestaande internetverbinding. |Onvoorspelbare latentie omdat de verbinding gaat via Internet. |
| De doorvoer van maximaal 200 Mb/s per gateway. |Het maken en beheren van afzonderlijke verbindingen tussen elk apparaat op uw on-premises netwerk en elk apparaat verbinding maken moet met elke-gateway vereist. |
| Kan worden gebruikt om verbinding maken met Azure-services die kunnen worden verbonden met een virtuele Azure-netwerken (VNet), zoals Azure Virtual Machines en Azure Cloud Services. |Minimale doorlopende beheer van een Azure VPN-Gateway vereist. |
| Kan niet worden gebruikt om verbinding maken met Microsoft Office 365 of Dynamics CRM Online. | |
| Verbinding maken met Azure-services die niet worden verbonden met een VNet kan niet worden gebruikt. | |

Meer informatie over de [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) -service, de [prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway), en uitgaande gegevensoverdracht [prijzen](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Verbinding maken met een site-naar-site-verbinding
Dit verbindingstype biedt toegang tot sommige Microsoft-cloudservices via een IPSec-tunnel via Internet, zoals hieronder wordt weergegeven.

![S2S](./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Site-naar-site-verbinding")

De verbinding wordt gemaakt via uw bestaande internetverbinding, maar het gebruik van een Azure VPN-Gateway met de bijbehorende prijzen en de prijzen voor uitgaande gegevensoverdracht is vereist. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Voordelen** | **Overwegingen** |
| --- | --- |
| Alle apparaten in uw on-premises netwerk kunnen communiceren met Azure-services die zijn verbonden met een VNet, zodat er is niet nodig om afzonderlijke verbindingen voor elk apparaat te configureren. |Al het verkeer wordt versleuteld met behulp van IPSec, kan deze worden onderschept onderweg zijn omdat deze het openbare Internet passeert. |
| Maakt gebruik van uw bestaande internetverbinding. |Onvoorspelbare latentie omdat de verbinding gaat via Internet. |
| Verbinding maken met Azure-services die kunnen worden verbonden met een VNet, zoals virtuele Machines en Cloud Services kan worden gebruikt. |Moet configureren en beheren van een gevalideerde VPN-apparaat * on-premises. |
| De doorvoer van maximaal 200 Mb/s per gateway. |Minimale doorlopende beheer van een Azure VPN-Gateway vereist. |
| Kunt afdwingen dat uitgaand verkeer afkomstig zijn van cloud virtuele machines via de on-premises netwerk voor controle en logboekregistratie met behulp van de gebruiker gedefinieerde routes of Border Gateway Protocol (BGP) **. |Kan niet worden gebruikt om verbinding maken met Microsoft Office 365 of Dynamics CRM Online. |
| Verbinding maken met Azure-services die niet worden verbonden met een VNet kan niet worden gebruikt. | |
| Als u services die verbindingen naar on-premises apparaten starten en uw beveiligingsbeleid dit nodig is, moet u mogelijk een firewall tussen de on-premises netwerk en Azure. | |

* * Bekijk een lijst van [VPN-apparaten gevalideerd](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable).
* ** Meer informatie over het gebruik van [gebruiker gedefinieerde routes](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) of [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) om af te dwingen routering van Azure vnet's naar een on-premises-apparaat.

## <a name="connecting-with-a-dedicated-private-connection"></a>Verbinding maken met een speciale persoonlijke verbinding
Dit verbindingstype biedt toegang tot alle Microsoft-cloudservices via een speciale persoonlijke verbinding naar Microsoft loopt niet via het Internet, zoals hieronder wordt weergegeven.

![ER](./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute-verbinding")

De verbinding vereist gebruik van de ExpressRoute-service en een verbinding met een connectiviteitsprovider. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Voordelen** | **Overwegingen** |
| --- | --- |
| Verkeer kan niet worden onderschept tijdens de overdracht via het openbare Internet, omdat een speciale verbinding via een serviceprovider van de wordt gebruikt. |On-premises router beheer vereist. |
| Bandbreedte tot 10 Gb/s per ExpressRoute-circuit en de doorvoer maximaal 2 Gb/s voor elke gateway. |Vereist een specifieke verbinding met een connectiviteitsprovider. |
| Voorspelbare latentie omdat het een specifieke verbinding met Microsoft die loopt niet via Internet. |Mogelijk minimale doorlopende beheer van een of meer Azure VPN-Gateways (als de verbinding met het maken van het circuit met vnet's). |
| Vereist niet versleutelde communicatie, maar u het verkeer versleutelen kunt indien gewenst. |Als u cloud-services die verbindingen naar on-premises apparaten starten, moet u mogelijk een firewall tussen de on-premises netwerk en Azure. |
| Rechtstreeks verbinding maken met alle Microsoft cloudservices, met enkele uitzonderingen *. |Netwerkadresomzetting (NAT) vereist van on-premises IP-adressen opgeven van de datacentra van Microsoft voor services die niet worden verbonden met een VNet.* * |
| Kunt afdwingen dat uitgaand verkeer afkomstig zijn van cloud virtuele machines via de on-premises netwerk voor controle en logboekregistratie met behulp van BGP. | |

* * Weer een [lijst met services](../expressroute/expressroute-faqs.md#supported-services) die niet worden gebruikt met ExpressRoute. Uw Azure-abonnement moet worden goedgekeurd als u wilt verbinding maken met Office 365.  Zie de [Azure ExpressRoute voor Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) artikel voor meer informatie.
* ** Meer informatie over ExpressRoute [NAT](../expressroute/expressroute-nat.md) vereisten.

Meer informatie over [ExpressRoute](../expressroute/expressroute-introduction.md), de bijbehorende [prijzen](https://azure.microsoft.com/pricing/details/expressroute), en [connectiviteitsproviders](../expressroute/expressroute-locations.md#locations).

## <a name="additional-considerations"></a>Aanvullende overwegingen
* Enkele van de bovenstaande opties hebben verschillende maximumlimieten die ze voor VNet-verbindingen, verbindingen en andere criteria ondersteunen kunnen. Het wordt aanbevolen dat u de Azure controleren [netwerklimieten](../azure-subscription-service-limits.md#networking-limits) om te begrijpen als deze van invloed op de verbindingstypen die u wilt gebruiken.
* Als u een gateway vanuit een site-naar-site VPN-verbinding verbinding maken met hetzelfde VNet als een ExpressRoute-gateway wilt, zorg ervoor dat u met belangrijke beperkingen eerst. Zie de [configureren van ExpressRoute en Site-naar-Site-verbindingen](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) artikel voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
De onderstaande bronnen wordt uitgelegd hoe u de verbindingstypen in dit artikel besproken implementeert.

* [Een punt-naar-site-verbinding implementeren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
* [Implementeren van een site-naar-site-verbinding](guidance-hybrid-network-vpn.md)
* [Een speciale persoonlijke verbinding implementeren](guidance-hybrid-network-expressroute.md)
* [Een speciale persoonlijke verbinding met een site-naar-site-verbinding voor hoge beschikbaarheid implementeren](guidance-hybrid-network-expressroute-vpn-failover.md)
