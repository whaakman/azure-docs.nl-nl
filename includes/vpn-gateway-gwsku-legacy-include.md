---
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: cherylmc
ms.openlocfilehash: f2ee442d0d6fecf34449ad28f058615a1274bbea
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269684"
---
De verouderde (oude) VPN-gateway-SKU's zijn:

* Basic
* Standard
* HighPerformance

VPN Gateway maakt geen gebruik van de gateway-SKU UltraPerformance. Meer informatie over de SKU UltraPerformance vindt u in het document [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Als u werkt met de verouderde SKU's, kunt u het volgende overwegen:

* Als u een PolicyBased VPN-type gebruikt, moet u Basis-SKU gebruiken. PolicyBased VPN-verbindingen (voorheen statische routering) worden niet ondersteund op andere SKU's.
* BGP wordt niet ondersteund op de basis-SKU.
* ExpressRoute-VPN-Gateway-configuraties die naast elkaar bestaan, worden niet ondersteund in de basis-SKU.
* Actief-actief S2S VPN-gatewayverbindingen kunnen alleen worden geconfigureerd op de HighPerformance SKU.