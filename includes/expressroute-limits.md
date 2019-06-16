---
title: bestand opnemen
description: bestand opnemen
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 05/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9f7f9b1504533e0ec503e97c086c380da71671a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66299704"
---
| Resource | Standaard-/ maximumlimiet |
| --- | --- |
| ExpressRoute-circuits per abonnement |10 |
| ExpressRoute-circuits per regio per abonnement, met Azure Resource Manager |10 |
| Maximum aantal routes geadverteerd naar persoonlijke Azure-peering met ExpressRoute Standard |4,000 |
| Maximum aantal routes geadverteerd naar persoonlijke Azure-peering met ExpressRoute Premium-invoegonderdeel |10\.000 |
| Maximum aantal routes geadverteerd van persoonlijke Azure-peering van de VNet-adresruimte voor een ExpressRoute-verbinding |200 |
| Maximum aantal routes die zijn geadverteerd aan Microsoft-peering met ExpressRoute Standard |200 |
| Maximum aantal routes geadverteerd aan Microsoft-peering met ExpressRoute Premium-invoegonderdeel |200 |
| Maximum aantal ExpressRoute-circuits die zijn gekoppeld aan hetzelfde virtuele netwerk op dezelfde locatie bevinden peering |4 |
| Maximum aantal ExpressRoute-circuits die zijn gekoppeld aan hetzelfde virtuele netwerk op verschillende locaties voor peering |4 |
| Aantal virtuele netwerk-koppelingen per ExpressRoute-circuit toegestaan |Zie de [aantal virtuele netwerken per ExpressRoute-circuit](#vnetpercircuit) tabel.  |

#### <a name="vnetpercircuit"></a> Aantal virtuele netwerken per ExpressRoute-circuit
| **De grootte van het circuit** | **Aantal koppelingen tussen virtuele netwerken voor Standard** | **Aantal virtuele netwerkkoppelingen met Premium-invoegonderdeel** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |
| 40 Gbps* |10 |100 |
| 100 Gbps* |10 |100 |

\* Alleen Direct ExpressRoute
