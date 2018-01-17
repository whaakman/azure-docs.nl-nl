---
title: Verbinding maken met uw on-premises netwerk naar Azure | Microsoft Docs
description: Legt uit en vergelijkt de verschillende methoden voor het verbinden met Microsoft cloud-services zoals Azure, Office 365 en Dynamics CRM Online.
services: 
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: 
ms.assetid: 294d39ad-40e0-476a-a362-57911b1172b7
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: cherylmc
ms.openlocfilehash: 37d83d3b6dea1763d85f2411816ba2fee4279100
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="connecting-your-on-premises-network-to-azure"></a>Verbinding maken met uw on-premises netwerk naar Azure
Microsoft biedt verschillende soorten cloudservices. Terwijl u verbinding met alle services die via het openbare Internet maken kunt, kunt u ook verbinding maken met enkele van de services met een virtueel particulier netwerk (VPN)-tunnel via Internet of via een directe, particuliere verbinding naar Microsoft. In dit artikel kunt u bepalen welke connectiviteitsoptie het beste voldoet aan uw behoeften op basis van de typen Microsoft-cloudservices die u wilt gebruiken. De meeste organisaties gebruikmaken van meerdere verbindingstypen die hieronder worden beschreven.

## <a name="connecting-over-the-public-internet"></a>Verbinding maken via het openbare Internet
Dit verbindingstype biedt toegang tot Microsoft cloud-services rechtstreeks via Internet, zoals hieronder wordt weergegeven.

![Internet](./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Deze verbinding is doorgaans het eerste type gebruikt voor het verbinden met Microsoft-cloudservices. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Benefits** | **Overwegingen** |
| --- | --- |
| Vereist geen wijziging in uw on-premises netwerk, zolang alle apparaten van clients hebben onbeperkte toegang tot alle IP-adressen en poorten op het Internet. |Al het verkeer wordt vaak versleuteld via HTTPS, kan deze worden onderschept onderweg omdat deze het openbare Internet passeert. |
| Kan verbinding maken met alle Microsoft-cloudservices blootgesteld aan het openbare Internet. |Onvoorspelbare latentie omdat de verbinding over het Internet wordt verzonden. |
| Gebruikt uw bestaande internetverbinding. | |
| Beheer van apparaten die verbinding vereist. | |

Deze verbinding heeft geen netwerkverbinding of de kosten van bandbreedte, omdat u uw bestaande internetverbinding gebruiken.

## <a name="connecting-with-a-point-to-site-connection"></a>Verbinding maken met een punt-naar-site-verbinding
Dit verbindingstype biedt toegang tot sommige Microsoft-cloudservices via een tunnel Secure Socket Tunneling Protocol (SSTP) toegestaan via Internet, zoals hieronder wordt weergegeven.

![P2S](./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "punt-naar-site-verbinding")

De verbinding wordt gemaakt via uw bestaande internetverbinding, maar gebruik van een Azure VPN-Gateway vereist. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Benefits** | **Overwegingen** |
| --- | --- |
| Vereist geen wijziging in uw on-premises netwerk, zolang alle apparaten van clients hebben onbeperkte toegang tot alle IP-adressen en poorten op het Internet. |Al het verkeer is versleuteld met IPSec, kan deze worden onderschept onderweg omdat deze het openbare Internet passeert. |
| Gebruikt uw bestaande internetverbinding. |Onvoorspelbare latentie omdat de verbinding over het Internet wordt verzonden. |
| De doorvoer maximaal 200 Mb/s per gateway. |Maken en beheren van afzonderlijke verbindingen tussen elk apparaat in uw on-premises netwerk en elk apparaat verbinding moet maken met elke-gateway vereist. |
| Kan verbinding maken met Azure-services die kunnen worden verbonden met een virtuele Azure-netwerken (VNet) worden gebruikt zoals Azure Virtual Machines en Azure Cloud Services. |Minimale doorlopende beheer van een Azure VPN-Gateway vereist. |
| Verbinding maken met Microsoft Office 365 of Dynamics CRM Online kan niet worden gebruikt. | |
| Verbinding maken met Azure-services die niet worden verbonden met een VNet kan niet worden gebruikt. | |

Meer informatie over de [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) service, de [prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway), en uitgaande gegevensoverdracht [prijzen](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Verbinding maken met een site-naar-site-verbinding
Dit verbindingstype biedt toegang tot sommige Microsoft-cloudservices via een IPSec-tunnel via Internet, zoals hieronder wordt weergegeven.

![S2S](./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Site-naar-site-verbinding")

De verbinding wordt gemaakt via uw bestaande internetverbinding, maar gebruik van een Azure VPN-Gateway met de bijbehorende prijzen en uitgaande gegevensoverdracht prijzen vereist. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Benefits** | **Overwegingen** |
| --- | --- |
| Alle apparaten in uw on-premises netwerk kunnen communiceren met de Azure-services verbonden met een VNet is niet nodig om afzonderlijke verbindingen voor elk apparaat te configureren. |Al het verkeer is versleuteld met IPSec, kan deze worden onderschept onderweg omdat deze het openbare Internet passeert. |
| Gebruikt uw bestaande internetverbinding. |Onvoorspelbare latentie omdat de verbinding over het Internet wordt verzonden. |
| Verbinding maken met Azure-services die kunnen worden verbonden met een VNet, zoals virtuele Machines en Cloud Services kan worden gebruikt. |Moet configureren en beheren van een gevalideerde VPN-apparaat * lokale. |
| De doorvoer maximaal 200 Mb/s per gateway. |Minimale doorlopende beheer van een Azure VPN-Gateway vereist. |
| Uitgaand verkeer gestart vanaf cloud virtuele machines via de on-premises netwerk voor controle en logboekregistratie met behulp van de gebruiker gedefinieerde routes of het Border Gateway Protocol (BGP) kunt afdwingen **. |Verbinding maken met Microsoft Office 365 of Dynamics CRM Online kan niet worden gebruikt. |
| Verbinding maken met Azure-services die niet worden verbonden met een VNet kan niet worden gebruikt. | |
| Als u gebruikmaakt van services die verbindingen naar on-premises apparaten starten en uw beveiligingsbeleid dit is vereist, moet u mogelijk een firewall tussen de on-premises netwerk en Azure. | |

* * Een lijst weergeven met [VPN-apparaten gevalideerd](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable).
* ** Meer informatie over het gebruik van [gebruiker gedefinieerde routes](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) of [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) om af te dwingen routering van Azure VNets naar een on-premises-apparaat.

## <a name="connecting-with-a-dedicated-private-connection"></a>Verbinding maken met een speciale persoonlijke verbinding
Dit verbindingstype biedt toegang tot alle Microsoft cloud-services via een speciale persoonlijke verbinding naar Microsoft die niet voor het Internet passeren biedt, zoals hieronder wordt weergegeven.

![ER](./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute-verbinding")

De verbinding vereist gebruik van de service ExpressRoute en een verbinding met een connectiviteitsprovider. De volgende tabel bevat de voor- en nadelen van dit verbindingstype.

| **Benefits** | **Overwegingen** |
| --- | --- |
| Verkeer kan niet worden onderschept tijdens de overdracht via het openbare Internet aangezien wordt gebruikt voor een specifieke verbinding via een serviceprovider. |On-premises router beheer vereist. |
| Bandbreedte maximaal 10 Gb/s per ExpressRoute-circuit en de doorvoer maximaal 2 Gb/s naar elke gateway. |Een speciale verbinding vereist met een connectiviteitsprovider. |
| Voorspelbare latentie omdat het een specifieke verbinding met Microsoft die niet via Internet verloopt. |Mogelijk minimale doorlopende beheer van een of meer Azure VPN-Gateways (als de verbinding met het maken van het circuit met vnet's). |
| Vereist geen gecodeerde communicatie, maar u het verkeer versleutelen kunt indien gewenst. |Als u cloudservices die verbindingen naar on-premises apparaten starten, moet u mogelijk een firewall tussen de on-premises netwerk en Azure. |
| Kunnen rechtstreeks verbinding maken met alle Microsoft cloud-services, met enkele uitzonderingen *. |Netwerkadresomzetting (NAT) vereist van het lokale IP-adressen voor het invoeren van de Microsoft-datacenters voor services die niet worden verbonden met een VNet.* * |
| Kunt afdwingen dat uitgaand verkeer vanaf cloud virtuele machines via de on-premises netwerk voor controle en logboekregistratie met behulp van BGP gestart. | |

* * Weergeven een [lijst met services](../expressroute/expressroute-faqs.md#supported-services) die met ExpressRoute kan niet worden gebruikt. Uw Azure-abonnement moet worden goedgekeurd voor verbinding met Office 365.  Zie de [Azure ExpressRoute voor Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) artikel voor meer informatie.
* ** Meer informatie over ExpressRoute [NAT](../expressroute/expressroute-nat.md) vereisten.

Meer informatie over [ExpressRoute](../expressroute/expressroute-introduction.md), de bijbehorende [prijzen](https://azure.microsoft.com/pricing/details/expressroute), en [connectiviteitsproviders](../expressroute/expressroute-locations.md#locations).

## <a name="additional-considerations"></a>Aanvullende overwegingen
* Enkele van de bovenstaande opties hebben verschillende maximale beperkingen die ze voor VNet-verbindingen, gatewayverbindingen en andere criteria kunnen ondersteunen. Het raadzaam dat u de Azure controleren [networking limieten](../azure-subscription-service-limits.md#networking-limits) om te begrijpen als deze van invloed op de connectiviteit-typen die u wilt gebruiken.
* Als u van plan bent om een gateway van een site-naar-site VPN-verbinding met hetzelfde VNet als een ExpressRoute-gateway, moet u vertrouwd raken met belangrijke beperkingen eerst. Zie de [configureren van ExpressRoute- en Site-naar-Site naast elkaar bestaande verbindingen](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) artikel voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
De onderstaande resources wordt uitgelegd hoe de verbindingstypen die in dit artikel te implementeren.

* [Een punt-naar-site-verbinding implementeren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
* [Een site-naar-site-verbinding implementeren](guidance-hybrid-network-vpn.md)
* [Een speciale persoonlijke verbinding implementeren](guidance-hybrid-network-expressroute.md)
* [Een speciale persoonlijke verbinding met een site-naar-site-verbinding voor hoge beschikbaarheid implementeren](guidance-hybrid-network-expressroute-vpn-failover.md)
