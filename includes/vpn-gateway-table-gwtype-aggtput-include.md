---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 555a8e3e92dc1d12cb7c6d6e06d2511f15a2c862
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52973116"
---
|**SKU**   | **S2S-/VNet-naar-VNet-<br>tunnels** | **P2S<br> SSTP-verbindingen** | **P2S<br> IKEv2-verbindingen** | **Benchmark cumulatieve<br>doorvoer** | **BGP** |
|---       | ---        | ---       | ---            | ---       | --- |
|**Basic** | Met maximaal 10    | Met maximaal 128  | Niet ondersteund  | 100 Mbps  | Niet ondersteund|
|**VpnGw1**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 250       | 650 Mbps  | Ondersteund |
|**VpnGw2**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 500       | 1 Gbps    | Ondersteund |
|**VpnGw3**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 1000      | 1,25 Gbps | Ondersteund |


(\*) Gebruik [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md) als u meer dan 30 S2S VPN-tunnels nodig hebt.

* De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. De Benchmark cumulatieve doorvoer voor een VPN-gateway is gebaseerd op de combinatie S2S + P2S. **Als u veel P2S-verbindingen gebruikt, kan dit in verband met doorvoerbeperkingen een negatieve invloed hebben op S2S-verbindingen.** In verband met omstandigheden in het internetverkeer en het gedrag van uw toepassing geeft de Benchmark cumulatieve doorvoer geen gegarandeerde doorvoer aan.

* Er gelden afzonderlijke verbindingslimieten. U kunt bijvoorbeeld 128 SSTP-verbindingen en ook 250 IKEv2-verbindingen hebben in een SKU van het type VpnGw1.

* Prijsinformatie vindt u op de pagina [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Gegevens over de SLA (Service Level Agreement) vindt u op de pagina [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* VpnGw1, VpnGw2 en VpnGw3 worden alleen ondersteund voor VPN-gateways die gebruikmaken van het Resource Manager-implementatiemodel.

* De Basic-SKU wordt beschouwd als verouderd. Deze SKU kent bepaalde functiebeperkingen. Zo kunt u een gateway die gebruikmaakt van een Basic-SKU, niet overzetten naar een van de nieuwe gateway-SKU's. In plaats daarvan moet u een nieuwe SKU gebruiken, wat betekent dat u uw VPN-gateway eerst moet verwijderen en vervolgens opnieuw moet maken. Controleer of de functie die u nodig hebt, wordt ondersteund voordat u kiest voor de Basic-SKU.
