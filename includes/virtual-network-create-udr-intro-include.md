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
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175963"
---
Hoewel bij het gebruik van systeemroutes verkeer automatisch wordt gefaciliteerd voor uw implementatie, kunnen er gevallen zijn waarin u pakketten liever wilt routeren via een virtueel apparaat. U kunt hiertoe door de gebruiker gedefinieerde routes maken waarin u de volgende hop voor pakketstromen naar een specifiek subnet opgeeft, zodat de pakketten worden doorgestuurd naar uw virtuele apparaat. Daarvoor schakelt u Doorsturen via IP in voor de virtuele machine die u gebruikt als virtueel apparaat.

Enkele van de gevallen waar de virtuele apparaten kunnen worden gebruikt zijn:

* Bewaking van netwerkverkeer met een inbraakdetectiehandtekeningen detectiesysteem (id's)
* Verkeer met een firewall beheren

Ga voor meer informatie over UDR en IP-doorsturen naar [de gebruiker gedefinieerde Routes en doorsturen via IP](../articles/virtual-network/virtual-networks-udr-overview.md).

