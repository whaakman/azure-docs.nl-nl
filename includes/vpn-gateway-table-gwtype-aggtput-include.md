---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/22/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8fb9e9ea0e126509697b4874bf1e5e0b6a380e7f
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66425745"
---
|**SKU**   | **S2S-/VNet-naar-VNet-<br>tunnels** | **P2S<br> SSTP-verbindingen** | **P2S<br> IKEv2/OpenVPN verbindingen** | **Benchmark cumulatieve<br>doorvoer** | **BGP** | **Zone-redundant** |
|---       | ---        | ---       | ---            | ---       | --- | --- |
|**Basic** | Met maximaal 10    | Met maximaal 128  | Niet ondersteund  | 100 Mbps  | Niet ondersteund| Nee |
|**VpnGw1**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 250       | 650 Mbps  | Ondersteund | Nee |
|**VpnGw2**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 500       | 1 Gbps    | Ondersteund | Nee |
|**VpnGw3**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 1000      | 1,25 Gbps | Ondersteund | Nee |
|**VpnGw1AZ**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 250       | 650 Mbps  | Ondersteund | Ja |
|**VpnGw2AZ**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 500       | 1 Gbps    | Ondersteund | Ja |
|**VpnGw3AZ**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 1000      | 1,25 Gbps | Ondersteund | Ja |


(\*) Gebruik [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md) als u meer dan 30 S2S VPN-tunnels nodig hebt.

* De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. De Benchmark cumulatieve doorvoer voor een VPN-gateway is gebaseerd op de combinatie S2S + P2S. **Als u veel P2S-verbindingen gebruikt, kan dit in verband met doorvoerbeperkingen een negatieve invloed hebben op S2S-verbindingen.** In verband met omstandigheden in het internetverkeer en het gedrag van uw toepassing geeft de Benchmark cumulatieve doorvoer geen gegarandeerde doorvoer aan.

* Er gelden afzonderlijke verbindingslimieten. U kunt bijvoorbeeld 128 SSTP-verbindingen en ook 250 IKEv2-verbindingen hebben in een SKU van het type VpnGw1.

* Prijsinformatie vindt u op de pagina [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Gegevens over de SLA (Service Level Agreement) vindt u op de pagina [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* VpnGw1, VpnGw2 en VpnGw3 worden alleen ondersteund voor VPN-gateways die gebruikmaken van het Resource Manager-implementatiemodel.

* De Basic-SKU wordt beschouwd als verouderd. Deze SKU kent bepaalde functiebeperkingen. Zo kunt u een gateway die gebruikmaakt van een Basic-SKU, niet overzetten naar een van de nieuwe gateway-SKU's. In plaats daarvan moet u een nieuwe SKU gebruiken, wat betekent dat u uw VPN-gateway eerst moet verwijderen en vervolgens opnieuw moet maken. Controleer of de functie die u nodig hebt, wordt ondersteund voordat u kiest voor de Basic-SKU.
