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
ms.openlocfilehash: d8091fdade9cd417af58755d8245c2fb091b86b3
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
De volgende tabel bevat de vereisten voor PolicyBased en RouteBased VPN-gateways. Deze tabel is van toepassing op de Resource Manager en de klassieke implementatiemodellen. Voor het klassieke model PolicyBased VPN-gateways zijn hetzelfde als statische gateways en op Route gebaseerde gateways zijn hetzelfde als dynamische gateways.

|  | **PolicyBased Basic VPN Gateway** | **Basic op route gebaseerd VPN-Gateway** | **RouteBased Standard VPN Gateway** | **RouteBased High Performance VPN-Gateway** |
| --- | --- | --- | --- | --- |
| **Site-naar-Site-connectiviteit (S2S)** |PolicyBased VPN-configuratie |RouteBased VPN-configuratie |RouteBased VPN-configuratie |RouteBased VPN-configuratie |
| **Punt-naar-site-verbinding (P2S**) |Niet ondersteund |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |Ondersteund (kan tegelijk bestaan met S2S) |
| **Verificatiemethode** |Vooraf gedeelde sleutel |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |Vooraf gedeelde sleutel voor S2S-connectiviteit, certificaten voor P2S-connectiviteit |
| **Maximumaantal S2S-verbindingen** |1 |10 |10 |30 |
| **Maximumaantal P2S-verbindingen** |Niet ondersteund |128 |128 |128 |
| **Ondersteuning voor actieve routering (BGP)** |Niet ondersteund |Niet ondersteund |Ondersteund |Ondersteund |
