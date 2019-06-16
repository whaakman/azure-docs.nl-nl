---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 9f7e2760ef8bf06a2e680dce90c323672ca9d491
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66416052"
---
* Voor de conversie moet de VM opnieuw worden opgestart. Plan daarom de migratie van uw VM's tijdens een reeds bestaand onderhoudsvenster. 

* De conversie kan niet ongedaan worden gemaakt. 

* Let erop dat alle gebruikers met de [Inzender voor virtuele machines](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol niet mogelijk de VM-grootte (indien deze vooraf conversie kunnen) wijzigen. Dit komt doordat virtuele machines met beheerde schijven moeten de gebruiker de Microsoft.Compute/disks/write-machtiging hebben voor de OS-schijven.

* Test de conversie. Migreer de test-VM voordat u de migratie in de productieomgeving uitvoert.

* Tijdens de conversie moet u de toewijzing van de VM ongedaan maken. De VM ontvangt een nieuw IP-adres wanneer deze na de conversie wordt opgestart. Indien vereist kunt u [een statisch IP-adres toewijzen](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) aan de VM.

* Bekijk de minimumversie van de Azure VM-agent vereist voor de ondersteuning van het conversieproces. Zie voor meer informatie over het controleren en bijwerken van de agentversie [minimaal versie-ondersteuning voor VM-agents in Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)