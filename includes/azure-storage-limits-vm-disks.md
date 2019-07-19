---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: ef9c46c0b55f97b8931f16d751f1b1e6099e6c9d
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68328601"
---
U kunt een aantal gegevens schijven koppelen aan een virtuele machine van Azure. Op basis van de schaalbaarheids-en prestatie doelen voor de gegevens schijven van een virtuele machine kunt u het aantal en type schijf bepalen dat u nodig hebt om te voldoen aan de prestatie-en capaciteits vereisten.

> [!IMPORTANT]
> Voor optimale prestaties kunt u het aantal zeer gebruikte schijven dat is gekoppeld aan de virtuele machine beperken om mogelijke beperking te voor komen. Als alle aangesloten schijven niet op hetzelfde moment worden gebruikt, kan de virtuele machine een groter aantal schijven ondersteunen.

**Voor Azure Managed disks:**

In de volgende tabel ziet u de standaard-en maximum limieten van het aantal resources per regio per abonnement

> | Resource | Standaardlimiet  | Maximumaantal |
> | --- | --- | --- |
> | Standard Managed disks | 50,000 | 50,000 |
> | Beheerde schijven Standard-SSD | 50,000 | 50,000 |
> | Premium Managed disks | 50,000 | 50,000 |
> | Standard_LRS-moment opnamen | 50,000 | 50,000 |
> | Standard_ZRS-moment opnamen | 50,000 | 50,000 |
> | Beheerde installatie kopie | 50,000 | 50,000 |

* **Voor standaard opslag accounts:** Een standaard opslag account heeft een maximale totale aanvraag frequentie van 20.000 IOPS. Het totale aantal IOPS voor alle virtuele-machine schijven in een Standard-opslag account mag deze limiet niet overschrijden.
  
    U kunt het aantal zeer gebruikte schijven dat wordt ondersteund door één standaard opslag account, berekenen op basis van de frequentie limiet van de aanvraag. Het maximum aantal zeer gebruikte schijven voor een virtuele machine in de laag is bijvoorbeeld ongeveer 66, dat wil zeggen 20000/300 IOPS per schijf. Het maximum aantal zeer gebruikte schijven voor een virtuele machine van de standaardlaag is ongeveer 40, dat wil zeggen 20000/500 IOPS per schijf. 

* **Voor Premium Storage-accounts:** Een Premium-opslag account heeft een maximale totale doorvoer snelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.

