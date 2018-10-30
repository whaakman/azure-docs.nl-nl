---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: bf26af7fa4b1b31514fb82c5e28a85154b2e274a
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226624"
---
* Voor de conversie moet de VM opnieuw worden opgestart. Plan daarom de migratie van uw VM's tijdens een reeds bestaand onderhoudsvenster. 

* De conversie kan niet ongedaan worden gemaakt. 

* Let erop dat alle gebruikers met de [Inzender voor virtuele machines](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol niet mogelijk de VM-grootte (indien deze vooraf conversie kunnen) wijzigen. Dit komt doordat virtuele machines met beheerde schijven moeten de gebruiker de Microsoft.Compute/disks/write-machtiging hebben voor de OS-schijven.

* Test de conversie. Migreer de test-VM voordat u de migratie in de productieomgeving uitvoert.

* Tijdens de conversie moet u de toewijzing van de VM ongedaan maken. De VM ontvangt een nieuw IP-adres wanneer deze na de conversie wordt opgestart. Indien vereist kunt u [een statisch IP-adres toewijzen](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) aan de VM.

* De oorspronkelijke VHD's en het opslagaccount die vóór de conversie werden gebruikt door de VM worden niet verwijderd. Hiervoor worden nog altijd kosten in rekening gebracht. Als u wilt voorkomen dat er kosten worden doorberekend voor de artefacten, verwijdert u de oorspronkelijke VHD-blobs nadat u hebt gecontroleerd of de conversie is voltooid.

* Bekijk de minimumversie van de Azure VM-agent vereist voor de ondersteuning van het conversieproces. Zie voor meer informatie over het controleren en bijwerken van de agentversie [minimaal versie-ondersteuning voor VM-agents in Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)