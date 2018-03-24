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
ms.openlocfilehash: 5de227e5de5ef9b41f6e0f64db86b7195259f7d6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
* **PolicyBased:** PolicyBased VPN-verbindingen werden voorheen statische routeringsgateways genoemd in het klassieke implementatiemodel. Op beleid gebaseerde VPN-verbindingen versleutelen pakketten en sturen via IPsec-tunnels op basis van het IPSec-beleid dat is geconfigureerd met de combinaties van adresvoorvoegsels tussen uw on-premises netwerk en het Azure VNet. Het beleid (of de verkeersselector) wordt gewoonlijk gedefinieerd als een toegangslijst in de configuratie van het VPN-apparaat. De waarde voor een PolicyBased VPN-type is *PolicyBased*. Wanneer u een PolicyBased VPN, houd er rekening mee houden de volgende beperkingen:
  
  * PolicyBased VPN-verbindingen kunnen **alleen** worden gebruikt op de standaard gateway-SKU. Deze VPN-type is niet compatibel met andere gateway-SKU's.
  * U kunt slechts 1 tunnel hebben wanneer u een PolicyBased VPN.
  * U kunt alleen PolicyBased VPN's gebruiken voor S2S-verbindingen en alleen voor bepaalde configuraties. De meeste configuraties van de VPN-Gateway moet een RouteBased VPN.
* **RouteBased**: op route gebaseerd VPN-verbindingen werden voorheen dynamische routeringsgateways genoemd in het klassieke implementatiemodel. Op route gebaseerd VPN-verbindingen gebruiken 'routes' in de IP-doorstuurtabel of routeringstabel om pakketten direct naar de bijbehorende tunnelinterfaces. De tunnelinterfaces versleutelen of ontsleutelen de pakketten vervolgens naar en vanuit de tunnels. Het beleid (of de verkeersselector) voor op route gebaseerd VPN-verbindingen zijn geconfigureerd als alles-naar-alles (of jokertekens). De waarde voor een RouteBased VPN-type is *RouteBased*.
