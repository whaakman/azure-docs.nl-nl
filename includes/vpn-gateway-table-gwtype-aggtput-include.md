---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4cbbb64489acf23c1248e35269e1441dd2a6878e
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513863"
---
|**SKU**   | **S2S-/VNet-naar-VNet-<br>tunnels** | **P2S-<br>verbindingen** | **Benchmark cumulatieve<br>doorvoer** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Met maximaal 30*                         | Met maximaal 128\*\*             | 650 Mbps                    |
|**VpnGw2**| Met maximaal 30*                         | Met maximaal 128\*\*             | 1 Gbps                      |
|**VpnGw3**| Met maximaal 30*                         | Met maximaal 128\*\*             | 1,25 Gbps                   |
|**Basic** | Met maximaal 10                         | Met maximaal 128               | 100 Mbps                    | 

* (*) Gebruik [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md) als u meer dan 30 S2S VPN-tunnels nodig hebt.

* (**) Neem contact op met ondersteuning als er extra verbindingen nodig zijn. Dit is alleen van toepassing voor IKEv2; het aantal verbindingen voor SSTP kan niet worden verhoogd.

* De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. Het is geen gegarandeerde doorvoer vanwege de omstandigheden van internetverkeer en het gedrag van uw toepassing.

* Prijsinformatie vindt u op de pagina [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Gegevens over de SLA (Service Level Agreement) vindt u op de pagina [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* VpnGw1, VpnGw2 en VpnGw3 worden alleen ondersteund voor VPN-gateways die gebruikmaken van het Resource Manager-implementatiemodel.