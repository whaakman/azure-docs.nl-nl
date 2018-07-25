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
ms.openlocfilehash: fad9b990b6ff1021efdaf8aadeb1e19d8a55871d
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39138089"
---
|**SKU**   | **S2S-/VNet-naar-VNet-<br>tunnels** | **P2S-<br>verbindingen** | **Benchmark cumulatieve<br>doorvoer** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Met maximaal 30*                         | Met maximaal 128**             | 650 Mbps                    |
|**VpnGw2**| Met maximaal 30*                         | Met maximaal 128**             | 1 Gbps                      |
|**VpnGw3**| Met maximaal 30*                         | Met maximaal 128**             | 1,25 Gbps                   |
|**Basic** | Met maximaal 10                         | Met maximaal 128               | 100 Mbps                    | 

* (*) Gebruik [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md) als u meer dan 30 S2S VPN-tunnels nodig hebt.

* (**) Neem contact op met ondersteuning als er extra verbindingen nodig zijn.

* De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. Het is geen gegarandeerde doorvoer vanwege de omstandigheden van internetverkeer en het gedrag van uw toepassing.

* Prijsinformatie vindt u op de pagina [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Gegevens over de SLA (Service Level Agreement) vindt u op de pagina [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* VpnGw1, VpnGw2 en VpnGw3 worden alleen ondersteund voor VPN-gateways die gebruikmaken van het Resource Manager-implementatiemodel.