---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 127ade3b37380c50461c5b648265b431a2f4467e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227109"
---
Een *aangepaste* virtuele machine is een virtuele machine die u maakt met een **aanbevolen app** uit **Marketplace**. Een dergelijke app doet veel werk voor u. U kunt echter nog steeds enkele configuratiekeuzes maken, zoals:

* De virtuele machine verbinden met een virtueel netwerk.
* De virtuele-machine-agent van Azure en de extensies van de virtuele Azure-machine installeren, zoals voor antimalware.
* De virtuele machine toevoegen aan bestaande cloudservices.
* De virtuele machine toevoegen aan een bestaand opslagaccount.
* De virtuele machine toevoegen aan een beschikbaarheidsset.

<!--
> [!IMPORTANT]
> If you want your virtual machine to use a virtual network so you can connect to it directly by host name or set up cross-premises connections, make sure that you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network overview](../articles/virtual-network/virtual-networks-overview.md).
>
>
 -->

> [!IMPORTANT]
> Als u wilt dat uw virtuele machine gebruikmaakt van een virtueel netwerk, zorgt u ervoor dat u het virtuele netwerk opgeeft tijdens het maken van de virtuele machine.

> * Twee voordelen die u hebt wanneer u een virtueel netwerk gebruikt, zijn dat u rechtstreeks verbinding maakt met de virtuele machine en dat u verbindingen tussen locaties kunt maken.

> * Een virtuele machine kan worden geconfigureerd om alleen verbinding te maken met een virtueel netwerk wanneer u de virtuele machine maakt. Zie [Overzicht van Azure Virtual Network](../articles/virtual-network/virtual-networks-overview.md) voor meer informatie over virtuele netwerken.
>
>

## <a name="to-create-the-virtual-machine"></a>De virtuele machine maken
