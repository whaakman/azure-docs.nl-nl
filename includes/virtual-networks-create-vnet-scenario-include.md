---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 06/11/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 2b1f9990985951a4e6ef260954968c0e1466c298
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60596978"
---
## <a name="scenario"></a>Scenario

Ter illustratie van het maken van een VNet en subnetten, in dit document worden de volgende scenario:

![VNet-scenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

In dit scenario maakt u een VNet met de naam **TestVNet**, met een gereserveerde CIDR-blok van **192.168.0.0./16**. Het VNet bevat de volgende subnetten: 

* **FrontEnd**, met **192.168.1.0/24** als CIDR-blok.
* **BackEnd**, met **192.168.2.0/24** als CIDR-blok.