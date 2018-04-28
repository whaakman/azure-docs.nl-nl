---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 226dfd9add69e8d89a030b858c819691d7b20627
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
Hoewel bij het gebruik van systeemroutes verkeer automatisch wordt gefaciliteerd voor uw implementatie, kunnen er gevallen zijn waarin u pakketten liever wilt routeren via een virtueel apparaat. U kunt hiertoe door de gebruiker gedefinieerde routes maken waarin u de volgende hop voor pakketstromen naar een specifiek subnet opgeeft, zodat de pakketten worden doorgestuurd naar uw virtuele apparaat. Daarvoor schakelt u Doorsturen via IP in voor de virtuele machine die u gebruikt als virtueel apparaat.

Enkele van de gevallen waar de virtuele apparaten kunnen worden gebruikt:

* Bewaking van verkeer met een detectiesysteem inbraakdetectie (id's)
* Verkeer met een firewall beheren

Bezoek voor meer informatie over het doorsturen van UDR en IP- [gebruiker gedefinieerde Routes en doorsturen via IP](../articles/virtual-network/virtual-networks-udr-overview.md).

