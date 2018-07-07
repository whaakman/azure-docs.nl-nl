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
ms.openlocfilehash: 605533f25b36a92a660301d28aa63cb2ecdd44f4
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37909975"
---
Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Selecteer de SKU die aan uw vereisten voldoet op basis van de typen werkbelasting, doorvoer, functies en SLA's.

###  <a name="benchmark"></a>Gateway-SKU's per tunnel, verbinding en doorvoer

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>Gateway-SKU's per reeks functies

De nieuwe VPN-gateway SKU's stroomlijnen de functiesets die worden aangeboden op de gateways:

| **SKU**| **Functies**|
| ---    | ---         |
|**Basic** (*)   | **Op route gebaseerde VPN**: 10 tunnels met P2S; geen RADIUS-verificatie voor P2S; geen IKEv2 voor P2S<br>**Op beleid gebaseerd VPN**: (IKEv1): 1 tunnel; geen P2S|
| **VpnGw1, VpnGw2 en VpnGw3** | **Op route gebaseerde VPN**: maximaal 30 tunnels (*), P2S, BGP, actief-actief, aangepast IPsec/IKE-beleid, ExpressRoute/VPN co-existentie |
|        |             |

( * ) U kunt "PolicyBasedTrafficSelectors" configureren om een op route gebaseerde VPN-gateway (VpnGw1, VpnGw2, VpnGw3) te verbinden met meerdere on-premises, op beleid gebaseerde firewallapparaten. Raadpleeg [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (VPN-gateways verbinden met meerdere on-premises,op beleid gebaseerde VPN-apparaten met behulp van PowerShell) voor meer informatie.

(\*\*) De basis-SKU wordt beschouwd als een verouderde SKU. De basis-SKU heeft bepaalde beperkingen van de functie. U kunt geen formaat van een gateway die gebruikmaakt van een basis-SKU naar een van de nieuwe gateway-SKU's, moet u in plaats daarvan naar een nieuwe SKU, verwijderen en opnieuw maken van uw VPN-gateway te wijzigen.

###  <a name="workloads"></a>Gateway-SKU's - productie vs. werkbelastingen voor ontwikkelen en testen

Vanwege de verschillen in sla's en functiesets raden we aan de volgende SKU's voor productie versus ontwikkelen en testen:

| **Workload**                       | **SKU's**               |
| ---                                | ---                    |
| **Productie, kritieke werkbelastingen** | VpnGw1, VpnGw2, VpnGw3 |
| **Ontwikkelen en testen of conceptontwerpen**   | Basic (*)                 |
|                                    |                        |

(\*\*) De basis-SKU wordt beschouwd als een verouderde SKU en beperkingen van de functie heeft. Controleer of dat de functie die u nodig hebt voordat u de basis-SKU wordt ondersteund.

Als u de oude SKU's (oud) gebruikt, zijn de aanbevelingen voor de productie-SKU Standard en HighPerformance. Zie voor informatie en instructies voor het oude SKU's, [Gateway-SKU's (verouderd)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).
