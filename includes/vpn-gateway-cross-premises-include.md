---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fa9c27457b1da4d233aaea2a6621af9f5d01149d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
|  | **Point-to-Site** | **Site-to-Site** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Door Azure ondersteunde Services** |Cloudservices en virtuele machines |Cloudservices en virtuele machines |[Lijst met Services](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Gebruikelijke bandbreedten** |Doorgaans < 100 Mbps aggregaat |Doorgaans < 1 Gbps aggregaat |50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps |
| **Ondersteunde protocollen** |Secure Sockets Tunneling Protocol (SSTP) |IPsec |Directe verbinding via VLAN's, NSP’s VPN-technologieën (MPLS, VPLS,...) |
| **Routering** |Op route gebaseerd (dynamisch) |We bieden ondersteuning voor PolicyBased (statische routering) en op route gebaseerd (dynamische routering VPN) |BGP |
| **Verbindingstolerantie** |actief-passief |actief-passief of actief / actief |actief-actief |
| **Typische gebruiksscenario 's** |Maken van een prototype, dev/testen/labscenario's voor cloudservices en virtuele machines |Dev/testen/labscenario's en kleinschalige productie-workloads voor cloudservices en virtuele machines |Toegang tot alle Azure-services (gevalideerde lijst), workloads op bedrijfsniveau en bedrijfskritieke taken, back-up, Big Data, Azure als een DR-site |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Prijzen** |[Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Prijzen](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Technische documentatie** |[VPN Gateway-documentatie](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN Gateway-documentatie](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Documentatie voor ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) |
| **Veelgestelde vragen** |[Veelgestelde vragen VPN Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Veelgestelde vragen VPN Gateway](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[Veelgestelde vragen over ExpressRoute](../articles/expressroute/expressroute-faqs.md) |
