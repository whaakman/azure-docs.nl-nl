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
ms.openlocfilehash: cff737b8c79c44494cb0151d6a6281550401b26e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
## <a name="scenario"></a>Scenario

Dit document gebruikt om te zien hoe u een VNet en subnetten, het volgende scenario:

![VNet-scenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

In dit scenario maakt u een VNet met de naam **TestVNet**, met een gereserveerde CIDR-blok van **192.168.0.0./16**. Het VNet bevat de volgende subnetten: 

* **FrontEnd**, met **192.168.1.0/24** als CIDR-blok.
* **BackEnd**, met **192.168.2.0/24** als CIDR-blok.

