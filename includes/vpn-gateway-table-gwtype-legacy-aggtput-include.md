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
ms.openlocfilehash: 6efec75884857d93f2e128104136bf59a1114594
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197171"
---
In de volgende tabel ziet u de gatewaytypen en de geschatte geaggregeerde doorvoer per gateway-SKU. Deze tabel is van toepassing op de resourcemanager en de klassieke implementatiemodellen. 

De prijzen zijn afhankelijk van de gateway-SKU's. Zie [Prijzen van VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) voor meer informatie.

De gateway-SKU UltraPerformance staat niet in deze tabel. Meer informatie over de SKU UltraPerformance vindt u in het document [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

|  | **Doorvoer VPN-gateway (1)** | **Max. IPsec-tunnels VPN-gateway (2)** | **Doorvoer ExpressRoute-gateway** | **VPN-gateway en ExpressRoute bestaan tegelijk** |
| --- | --- | --- | --- | --- |
| **Basic SKU (3)(5)(6)** |100 Mbps |10 |500 Mbps (6) |Nee |
| **Standaard SKU (4)(5)** |100 Mbps |10 |1000 Mbps |Ja |
| **High Performance SKU (4)** |200 Mbps |30 |2000 Mbps |Ja |


(1) De VPN-doorvoer is een ruwe schatting op basis van metingen tussen VNET's in dezelfde Azure-regio. Het is geen gegarandeerde doorvoer voor cross-premises verbindingen via internet. Het is een meting van de maximaal mogelijke doorvoer.

(2) Voor het aantal tunnels raadpleegt u de VPN op basis van route. Een op beleid gebaseerde VPN biedt alleen ondersteuning voor één site-naar-site VPN-tunnel.

(3) BGP wordt niet ondersteund voor de Basis-SKU.

(4) Op beleid gebaseerde VPN-verbindingen worden niet ondersteund voor deze SKU. Ze worden alleen ondersteund voor de Basis-SKU.

(5) Actief-actief S2S VPN-gatewayverbindingen worden niet ondersteund voor deze SKU. Actief-actief wordt alleen ondersteund op de HighPerformance SKU.

(6) het basis-SKU is voor gebruik met ExpressRoute afgeschaft.
