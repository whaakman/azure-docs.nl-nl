---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 29b2ac1a5dc128028dbd40e683c1b45e6208d124
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Scenario

Dit document gebruikt om te zien hoe u een VNet en subnetten, het volgende scenario:

![VNet-scenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

In dit scenario maakt u een VNet met de naam **TestVNet**, met een gereserveerde CIDR-blok van **192.168.0.0./16**. Het VNet bevat de volgende subnetten: 

* **FrontEnd**, met **192.168.1.0/24** als CIDR-blok.
* **BackEnd**, met **192.168.2.0/24** als CIDR-blok.

