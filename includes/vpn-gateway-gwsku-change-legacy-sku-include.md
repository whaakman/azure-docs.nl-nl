---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/15/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4c232e1ce183c6935d625b5bc9987a4981865ae4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57908685"
---
Als u met het Resource Manager-implementatiemodel werkt, kunt u met de nieuwe gateway SKU's wijzigen. Wanneer u van een verouderde gateway-SKU naar een nieuwe SKU wijzigt, kunt u de bestaande VPN-gateway verwijderen en een nieuwe VPN-gateway maken.

Werkstroom:

1. Verwijder verbindingen van de gateway voor het virtuele netwerk.
2. Verwijder de oude VPN-gateway.
3. Maak de nieuwe VPN-gateway.
4. Werk uw on-premises VPN-apparaten bij met het nieuwe IP-adres van de VPN-gateway (voor site-naar-site-verbindingen).
5. Werk de waarde van het IP-adres voor de gateway bij voor alle lokale VNet-naar-VNet-netwerkgateways die verbinding met deze gateway maken.
6. Download nieuwe client-VPN-configuratiepakketten voor P2S-clients die verbinding maken met het virtuele netwerk via deze VPN-gateway.
7. Herstel de verbindingen met de gateway voor het virtuele netwerk.

Overwegingen:

* Als u wilt verplaatsen naar de nieuwe SKU's, moet uw VPN-gateway in het Resource Manager-implementatiemodel.
* Als u een klassieke VPN-gateway hebt, moet u verder met behulp van de oudere verouderde SKU's voor die gateway, maar u kunt wisselen tussen de verouderde SKU's. U wijzigen niet naar de nieuwe SKU's.
* U moet connectiviteit downtime wanneer u van een oude SKU naar een nieuwe SKU wijzigt.
* Wanneer een nieuwe gateway-SKU wijzigt, verandert het openbare IP-adres voor uw VPN-gateway. Dit gebeurt zelfs als u hetzelfde openbare IP-adresobject dat u eerder hebt gebruikt.