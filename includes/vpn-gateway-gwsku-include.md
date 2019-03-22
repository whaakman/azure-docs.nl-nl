---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/20/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b1a9d93d9fccf02ba1517e429625150736e539e9
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305266"
---
Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Selecteer de SKU die aan uw vereisten voldoet op basis van de typen werkbelasting, doorvoer, functies en SLA's. Zie voor de virtuele netwerkgateway-SKU's in Azure-Beschikbaarheidszones, [Azure Availability Zones Gateway-SKU's](../articles/vpn-gateway/about-zone-redundant-vnet-gateways.md).

###  <a name="benchmark"></a>Gateway-SKU's per tunnel, verbinding en doorvoer

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>Gateway-SKU's per reeks functies

De nieuwe VPN-gateway SKU's stroomlijnen de functiesets die worden aangeboden op de gateways:

| **SKU**| **Functies**|
| ---    | ---         |
|**Basic** (*)   | **Op route gebaseerde VPN**: 10 tunnels voor S2S/verbindingen; Er zijn geen RADIUS-verificatie voor P2S; Er is geen IKEv2 voor P2S<br>**Op beleid gebaseerde VPN**: (IKEv1): 1/S2S-verbinding tunnel; Er is geen P2S|
| **VpnGw1, VpnGw2 en VpnGw3** | **Op route gebaseerde VPN**: maximaal 30 tunnels (*), P2S, BGP, actief-actief, aangepast IPsec/IKE-beleid, ExpressRoute/VPN co-existentie |
|        |             |

( * ) U kunt "PolicyBasedTrafficSelectors" configureren om een op route gebaseerde VPN-gateway (VpnGw1, VpnGw2, VpnGw3) te verbinden met meerdere on-premises, op beleid gebaseerde firewallapparaten. Raadpleeg [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (VPN-gateways verbinden met meerdere on-premises,op beleid gebaseerde VPN-apparaten met behulp van PowerShell) voor meer informatie.

(\*\*) De basis-SKU wordt beschouwd als een verouderde SKU. Deze SKU kent bepaalde functiebeperkingen. Zo kunt u een gateway die gebruikmaakt van een Basic-SKU, niet overzetten naar een van de nieuwe gateway-SKU's. In plaats daarvan moet u een nieuwe SKU gebruiken, wat betekent dat u uw VPN-gateway eerst moet verwijderen en vervolgens opnieuw moet maken.

###  <a name="workloads"></a>Gateway-SKU's - productie vs. werkbelastingen voor ontwikkelen en testen

Vanwege de verschillen in sla's en functiesets raden we aan de volgende SKU's voor productie versus ontwikkelen en testen:

| **Workload**                       | **SKU's**               |
| ---                                | ---                    |
| **Productie, kritieke werkbelastingen** | VpnGw1, VpnGw2, VpnGw3 |
| **Ontwikkelen en testen of conceptontwerpen**   | Basic (**)                 |
|                                    |                        |

(\*\*) De basis-SKU wordt beschouwd als een verouderde SKU en beperkingen van de functie heeft. Controleer of de functie die u nodig hebt, wordt ondersteund voordat u kiest voor de Basic-SKU.

Als u de oude SKU's (oud) gebruikt, zijn de aanbevelingen voor de productie-SKU Standard en HighPerformance. Zie voor informatie en instructies voor het oude SKU's, [Gateway-SKU's (verouderd)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).
