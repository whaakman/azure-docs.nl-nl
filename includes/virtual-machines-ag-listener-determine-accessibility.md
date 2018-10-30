---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8861396db6f6b680ddb55ce020e5579dc25b118e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226917"
---
Het is belangrijk om te profiteren van dat er twee manieren zijn om een listener voor de beschikbaarheidsgroep configureren in Azure. De manieren verschillen in het type van Azure load balancer die u gebruikt wanneer u de listener maakt. De volgende tabel worden de verschillen beschreven:

| Type load balancer | Implementatie | Gebruiken wanneer: |
| --- | --- | --- |
| **Externe** |Maakt gebruik van de *openbare virtuele IP-adres* van de cloudservice die als host fungeert voor de virtuele machines (VM's). |U moet toegang krijgen tot de listener van buiten het virtuele netwerk, met inbegrip van het Internet. |
| **Intern** |Maakt gebruik van een *interne load balancer* met een privé-adres voor de listener. |U kunt toegang tot de listener alleen uit binnen hetzelfde virtuele netwerk. Deze toegang bevat een site-naar-site VPN in hybride scenario's. |

> [!IMPORTANT]
> Voor een listener die gebruikmaakt van de cloudservice openbare VIP (externe load balancer), zo lang als de client, listener en -databases zich in dezelfde Azure-regio, worden er geen kosten voor uitgaand verkeer. Anders alle gegevens die zijn geretourneerd via de listener wordt beschouwd als uitgaand verkeer en wordt in rekening gebracht tegen normale gegevensoverdracht-tarieven. 
> 
> 

Een ILB kan worden geconfigureerd op virtuele netwerken met een regionaal bereik. Bestaande virtuele netwerken die zijn geconfigureerd voor een affiniteitsgroep een ILB niet gebruiken. Zie voor meer informatie, [overzicht van interne load balancer](../articles/load-balancer/load-balancer-internal-overview.md).

