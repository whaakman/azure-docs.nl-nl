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
ms.openlocfilehash: b3907882df09bfae1d6453fbffbd3e7562554f7c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159371"
---
* **PolicyBased:** PolicyBased VPN-verbindingen werden voorheen statische routeringsgateways genoemd in het klassieke implementatiemodel. Op beleid gebaseerde VPN-verbindingen versleutelen pakketten en sturen via IPsec-tunnels op basis van de IPsec-beleid dat is geconfigureerd met de combinaties van adresvoorvoegsels tussen uw on-premises netwerk en het Azure-VNet. Het beleid (of de verkeersselector) wordt gewoonlijk gedefinieerd als een toegangslijst in de configuratie van het VPN-apparaat. De waarde voor een PolicyBased VPN-type is *PolicyBased*. Wanneer u een PolicyBased VPN, houd rekening met de volgende beperkingen:
  
  * PolicyBased VPN-verbindingen kunnen **alleen** worden gebruikt voor de Basic gateway-SKU. Deze VPN-type is niet compatibel met andere gateway-SKU's.
  * U kunt slechts 1 tunnel hebben wanneer u een PolicyBased VPN.
  * U kunt alleen PolicyBased VPN-verbindingen gebruiken voor S2S-verbindingen en alleen voor bepaalde configuraties. De meeste VPN-Gateway-configuraties vereisen een RouteBased VPN.
* **RouteBased**: Op route gebaseerd VPN-verbindingen werden voorheen dynamische routeringsgateways genoemd in het klassieke implementatiemodel. Op route gebaseerd VPN's gebruiken 'routes' in de IP-doorstuurtabel of routeringstabel om pakketten rechtstreeks in de bijbehorende tunnelinterfaces. De tunnelinterfaces versleutelen of ontsleutelen de pakketten vervolgens naar en vanuit de tunnels. Het beleid (of de verkeersselector) voor RouteBased VPN-verbindingen zijn geconfigureerd als alles-naar-alles (of jokertekens). De waarde voor een RouteBased VPN-type is *RouteBased*.
