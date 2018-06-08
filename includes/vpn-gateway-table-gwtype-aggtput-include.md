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
ms.openlocfilehash: bc42697f756ec75d9a8f2c20c99b28b2f7886ca1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34670272"
---
|**SKU**   | **S2S-/VNet-naar-VNet-<br>tunnels** | **P2S-<br>verbindingen** | **Benchmark cumulatieve<br>doorvoer** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Met maximaal 30                         | Met maximaal 128*              | 650 Mbps                    |
|**VpnGw2**| Met maximaal 30                         | Met maximaal 128*              | 1 Gbps                      |
|**VpnGw3**| Met maximaal 30                         | Met maximaal 128*              | 1,25 Gbps                   |
|**Basic** | Met maximaal 10                         | Met maximaal 128               | 100 Mbps                    | 

*Neem contact op met ondersteuning als er extra verbindingen nodig zijn
- De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. Het is geen gegarandeerde doorvoer vanwege de omstandigheden van internetverkeer en het gedrag van uw toepassing.

- Prijsinformatie vindt u op de pagina [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway).

- Gegevens over de SLA (Service Level Agreement) vindt u op de pagina [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

- VpnGw1, VpnGw2 en VpnGw3 worden alleen ondersteund voor VPN-gateways die gebruikmaken van het Resource Manager-implementatiemodel.