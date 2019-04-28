---
author: WenJason
ms.service: load-balancer
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/31/2018
ms.author: v-jay
ms.openlocfilehash: bf3aee30460e052db23cf9090f13e2af3b22628b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516999"
---
In dit scenario worden de volgende taken uitgevoerd:

* Een load balancer maken die netwerkverkeer ontvangt op poort 80 en taakverdelingsverkeer naar virtuele machines 'web1' en 'web2' verzenden
* NAT-regels voor toegang tot extern bureaublad maken/SSH voor virtuele machines achter de load balancer
* Statuscontroles maken

![Load balancer-scenario](./media/load-balancer-get-started-internet-scenario-include/scenario-classic.png)