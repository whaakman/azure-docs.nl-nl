---
title: Overzicht van BGP en Azure VPN-gateways | Microsoft Docs
description: Dit artikel bevat een overzicht van BGP met Azure VPN-gateways.
services: vpn-gateway
author: yushwang
manager: rossort
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: 91e9fe1eb6b3df0b64d05f2b1e300403a9e01db9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60762262"
---
# <a name="about-bgp-with-azure-vpn-gateway"></a>Over BGP met Azure VPN-Gateway
Dit artikel bevat een overzicht van BGP (Border Gateway Protocol)-ondersteuning in Azure VPN-Gateway.

BGP is het standaardprotocol voor routering dat doorgaans op internet wordt gebruikt voor het uitwisselen van routerings- en bereikbaarheidsgegevens tussen twee of meer netwerken. Wanneer BGP wordt gebruikt in de context van Azure Virtual Networks, maakt BGP mogelijk dat Azure VPN Gateways en uw on-premises VPN-apparaten, zogenaamde BGP-peers of neighbors, 'routes' kunnen uitwisselen die beide gateways informeren over de beschikbaarheid en bereikbaarheid voor deze voorvoegsels zodat ze via de juiste gateways of routers communiceren. Met BGP kan ook transitroutering tussen meerdere netwerken worden ingeschakeld door routes die een BGP-gateway leert van één BGP te propageren naar alle andere BGP-peers. 

## <a name="why"></a>Waarom BGP gebruiken?
BGP is een optionele functie die u kunt gebruiken met op Azure-routes gebaseerde VPN-gateways. Controleer voordat u de functie inschakelt of uw on-premises VPN-apparaten BGP ondersteunen. U kunt Azure VPN-gateways en uw on-premises VPN-apparaten zonder BGP blijven gebruiken. Het is het equivalent van het gebruik van statische routes (zonder BGP) *versus* het gebruik van dynamische routering met BGP tussen uw netwerken en Azure.

Het gebruik van BGP heeft een aantal voordelen en nieuwe mogelijkheden:

### <a name="prefix"></a>Ondersteuning voor automatische en flexibele voorvoegselupdates
Met BGP hoeft u alleen via de IPSec S2S VPN-tunnel een minimaal voorvoegsel te declareren op een specifieke BGP-peer. Dit hoeft niet groter te zijn dan een hostvoorvoegsel (/32) van het IP-adres van de BGP-peer van uw on-premises VPN-apparaat. U bepaalt zelf welke on-premises netwerkvoorvoegsels u wilt adverteren naar Azure opdat uw Azure Virtual Network toegang heeft.

U kunt ook grotere voorvoegsels die bijvoorbeeld enkele van uw VNet-adresvoorvoegsels, zoals een grote privé IP-adresruimte (bijvoorbeeld 10.0.0.0/8) bevatten adverteren. Houd er rekening mee dat de voorvoegsels kunnen niet identiek zijn met een van uw VNet-voorvoegsels. Routes die identiek zijn aan uw VNet-voorvoegsels worden geweigerd.

### <a name="multitunnel"></a>Ondersteuning van meerdere tunnels tussen een VNet en een on-premises site met automatische failover op basis van BGP
U kunt meerdere verbindingen maken tussen uw Azure VNet en uw on-premises VPN-apparaten op dezelfde locatie. Deze mogelijkheid biedt meerdere tunnels (paden) tussen de twee netwerken in een actief/actief-configuratie. Als een van de tunnels wordt verbroken, wordt de bijbehorende routes ingetrokken via BGP en wordt het verkeer automatisch verplaatst naar de resterende tunnels.

In het volgende diagram ziet u een eenvoudig voorbeeld van deze configuratie voor hoge beschikbaarheid:

![Meerdere actieve paden](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="transitrouting"></a>Ondersteuning van transitroutering tussen uw on-premises netwerken en meerdere Azure VNets
BGP maakt mogelijk dat meerdere gateways voorvoegsels van verschillende netwerken leren en propageren, ongeacht of ze direct of indirect zijn verbonden. Op die manier is transitroutering met Azure VPN-gateways mogelijk tussen uw on-premises sites of tussen meerdere Azure Virtual Networks.

In de volgende diagram ziet u een voorbeeld van een multihop-topologie met meerdere paden, waarmee verkeer tussen de twee on-premises netwerken kan worden doorgevoerd via Azure VPN-gateways binnen de Microsoft-netwerken:

![Multihop-transit](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="faq"></a>VEELGESTELDE VRAGEN OVER BGP
[!INCLUDE [vpn-gateway-faq-bgp-include](../../includes/vpn-gateway-faq-bgp-include.md)]

## <a name="next-steps"></a>Volgende stappen
Zie [Aan de slag met BGP op Azure VPN-gateways](vpn-gateway-bgp-resource-manager-ps.md) voor stappen om BGP te configureren voor uw on-premises verbindingen en VNet-naar-VNet-verbindingen.

