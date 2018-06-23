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
ms.openlocfilehash: 2c1a4a1931bc2e38b0bee5f90518b01fdf4767a1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "30196785"
---
Als u met het Resource Manager-implementatiemodel werkt, kunt u met de nieuwe gateway-SKU's. Wanneer u van een verouderde SKU-gateway op een nieuwe SKU overschakelt, kunt u de bestaande VPN-gateway verwijdert en maak een nieuwe VPN-gateway.

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
* Als u een klassieke VPN-gateway hebt, moet u verder met de oudere verouderde SKU's voor deze gateway, maar u kunt de grootte tussen de verouderde SKU's. U kunt de nieuwe SKU's niet wijzigen.
* Hebt u connectiviteit uitvaltijd wanneer u van een verouderde SKU op een nieuwe SKU overschakelt.
* Wanneer u naar een nieuwe gateway SKU, verandert het openbare IP-adres voor uw VPN-gateway. Dit gebeurt zelfs als u hetzelfde openbare IP-adresobject dat u eerder hebt gebruikt.