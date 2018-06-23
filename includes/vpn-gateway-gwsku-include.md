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
ms.openlocfilehash: 05dc8ae48a9164e4f7118d378ab0eb7c30a4249e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "30196798"
---
Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Selecteer de SKU die aan uw vereisten voldoet op basis van de typen werkbelasting, doorvoer, functies en SLA's.

###  <a name="benchmark"></a>Gateway-SKU's per tunnel, verbinding en doorvoer

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>Gateway-SKU's door functieset

De nieuwe VPN-gateway SKU's stroomlijnen de functiesets aangeboden op de gateways:

| **SKU**| **Functies**|
| ---    | ---         |
|**Basic** (*)   | **Route gebaseerde VPN-**: 10 tunnels met P2S; er zijn geen RADIUS-verificatie voor P2S; er is geen IKEv2 voor P2S<br>**Op beleid gebaseerd VPN**: (IKEv1): 1 tunnel; geen P2S|
| **VpnGw1, VpnGw2 en VpnGw3** | **Route gebaseerde VPN-**: maximaal 30 tunnels (*), P2S, BGP, actieve, aangepaste IPsec/IKE-beleid, ExpressRoute/VPN co-existentie |
|        |             |

( * ) U kunt "PolicyBasedTrafficSelectors" configureren om een op route gebaseerde VPN-gateway (VpnGw1, VpnGw2, VpnGw3) te verbinden met meerdere on-premises, op beleid gebaseerde firewallapparaten. Raadpleeg [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (VPN-gateways verbinden met meerdere on-premises,op beleid gebaseerde VPN-apparaten met behulp van PowerShell) voor meer informatie.

(**) De basis-SKU wordt beschouwd als een verouderde SKU. De basis-SKU heeft bepaalde beperkingen. U kan niet de grootte van een gateway die gebruikmaakt van een basis-SKU op een van de nieuwe gateway-SKU's, moet u in plaats daarvan wijzigen in een nieuwe SKU, waarbij verwijderen en opnieuw maken van uw VPN-gateway.

###  <a name="workloads"></a>Gateway-SKU's - tegenover voor productie. werkbelastingen voor ontwikkelen en testen

Vanwege de verschillen in de sla en functiesets raden we aan de volgende SKU's voor productie versus dev-test:

| **Workload**                       | **SKU's**               |
| ---                                | ---                    |
| **Productie, kritieke werkbelastingen** | VpnGw1, VpnGw2, VpnGw3 |
| **Ontwikkelen en testen of conceptontwerpen**   | Basic (*)                 |
|                                    |                        |

(**) De basis-SKU wordt beschouwd als een verouderde SKU en beperkingen van de functie heeft. Controleer of de functie die u nodig hebt voordat u de basis-SKU gebruiken wordt ondersteund.

Als u de oude SKU's (verouderd) gebruikt, wordt de productie-SKU-aanbevelingen zijn standaard- en HighPerformance. Zie voor informatie en instructies voor het oude SKU's [Gateway-SKU's (verouderd)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).