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
ms.openlocfilehash: 5e7e365a73c9ba9cfd91d4f93759e7af55a0ad48
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
De volgende tabel bevat de vereisten voor PolicyBased en RouteBased VPN-gateways. Deze tabel is van toepassing op de Resource Manager en de klassieke implementatiemodellen. Voor het klassieke model PolicyBased VPN-gateways zijn hetzelfde als statische gateways en op Route gebaseerde gateways zijn hetzelfde als dynamische gateways.

|  | **PolicyBased Basic VPN-Gateway** | **Basic op route gebaseerd VPN-Gateway** | **Standaard op route gebaseerd VPN-Gateway** | **RouteBased High Performance VPN-Gateway** |
| --- | --- | --- | --- | --- |
| **Site-naar-Site-connectiviteit (S2S)** |PolicyBased VPN-configuratie |RouteBased VPN-configuratie |RouteBased VPN-configuratie |RouteBased VPN-configuratie |
| **Punt-naar-site-verbinding (P2S**) |Niet ondersteund |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |
| **Verificatiemethode** |Vooraf gedeelde sleutel |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |
| **Maximumaantal S2S-verbindingen** |1 |10 |10 |30 |
| **Maximumaantal P2S-verbindingen** |Niet ondersteund |128 |128 |128 |
| **Ondersteuning voor actieve routering (BGP)** (*) |Niet ondersteund |Niet ondersteund |Ondersteund |Ondersteund |

  (*) BGP wordt niet ondersteund voor het klassieke implementatiemodel.
