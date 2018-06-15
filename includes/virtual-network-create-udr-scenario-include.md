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
ms.openlocfilehash: b91ae155761f6357e286f4742d57b97cf96d909a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31805153"
---
## <a name="scenario"></a>Scenario
Dit document gebruikt om beter te laten zien hoe udr's maken, het volgende scenario:

![BESCHRIJVING VAN AFBEELDING](./media/virtual-network-create-udr-scenario-include/figure1.png)

In dit scenario maakt u een UDR voor de *front-end-subnet* en een andere UDR voor de *Back-end subnet*als volgt: 

* **UDR FrontEnd**. De front-UDR wordt toegepast op de *FrontEnd* subnet, en bevatten één route:    
  * **RouteToBackend**. Deze route voor al het verkeer verzendt naar de back-end-subnet aan de **FW1** virtuele machine.
* **UDR-back-end**. De back-end-UDR wordt toegepast op de *back-end* subnet, en bevatten één route:    
  * **RouteToFrontend**. Deze route verzendt alle verkeer naar het front-end-subnet moet de **FW1** virtuele machine.

De combinatie van deze routes zorgt ervoor dat alle verkeer van één subnet naar een andere wordt doorgestuurd naar de **FW1** virtuele machine, die wordt gebruikt als een virtueel apparaat. U moet ook schakelt u doorsturen via IP voor de **FW1** VM, zodat het verkeer dat is bestemd voor andere virtuele machines kan ontvangen.

