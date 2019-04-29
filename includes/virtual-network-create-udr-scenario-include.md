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
ms.openlocfilehash: 40b81904daabfdad7e45571d8ab86cf32cac8964
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743353"
---
## <a name="scenario"></a>Scenario
Dit document wordt u beter laten zien hoe u udr's maken, het volgende scenario:

![BESCHRIJVING VAN AFBEELDING](./media/virtual-network-create-udr-scenario-include/figure1.png)

In dit scenario maakt u een UDR voor het *Front-end-subnet* en een andere UDR voor het *Back-end-subnet*, als volgt: 

* **UDR-FrontEnd**. De front-end-UDR wordt toegepast op de *FrontEnd* subnet, en bevatten één route:    
  * **RouteToBackend**. Deze route verzendt alle verkeer naar de back-end-subnet aan de **FW1** virtuele machine.
* **UDR-BackEnd**. De UDR back-end wordt toegepast op de *back-end* subnet, en bevatten één route:    
  * **RouteToFrontend**. Deze route verzendt alle verkeer naar het front-end-subnet aan de **FW1** virtuele machine.

De combinatie van deze routes zorgt ervoor dat al het verkeer dat bestemd is van het ene subnet naar een andere wordt doorgestuurd naar de **FW1** virtuele machine, die wordt gebruikt als een virtueel apparaat. U moet ook in te schakelen doorsturen via IP voor de **FW1** VM, om te controleren of het verkeer dat bestemd is voor andere virtuele machines kan ontvangen.
