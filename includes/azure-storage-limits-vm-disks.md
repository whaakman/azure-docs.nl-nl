---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 2936fd318f08c74675f7e8b382c861f4a28319fc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261595"
---
U kunt een aantal gegevensschijven koppelen aan een virtuele machine van Azure. Op basis van de schaalbaarheids- en prestatiedoelen voor gegevensschijven van een virtuele machine, kunt u het aantal en type van de schijf die u nodig hebt om te voldoen aan uw prestaties en capaciteitsvereisten bepalen.

> [!IMPORTANT]
> Voor optimale prestaties, het aantal intensief gebruikte schijven die zijn gekoppeld aan de virtuele machine om te voorkomen dat mogelijk beperking te beperken. Als alle gekoppelde schijven worden niet op hetzelfde moment maximaal hebt gebruikt, kan de virtuele machine een groter aantal schijven ondersteunen.

**Beheerde schijven voor Azure:**

De volgende tabel ziet u de standaard- en maximumlimieten van het aantal resources per regio per abonnement

> | Resource | Standaardlimiet  | Maximumaantal |
> | --- | --- | --- |
> | Standaard beheerde schijven | 25,000 | 50,000 |
> | Managed Disks - Standard - SSD | 25,000 | 50,000 |
> | Beheerde Premium-schijven | 25,000 | 50,000 |
> | Standard_LRS momentopnamen | 25,000 | 50,000 |
> | Standard_ZRS momentopnamen | 25,000 | 50,000 |
> | Beheerde installatiekopie | 25,000 | 50,000 |

* **Voor Standard storage-accounts:** Een standaardopslagaccount heeft een maximale totale aanvraagsnelheid van 20.000 IOP's. De totale IOPS over alle van de schijven van uw virtuele machines in een Standard storage-account mag niet groter zijn dan deze limiet.
  
    U kunt het aantal intensief gebruikte schijven dat wordt ondersteund door een enkele Standard storage-account op basis van de aanvraag-frequentielimiet ongeveer berekenen. Bijvoorbeeld, voor een Basic-laag virtuele machine is het maximum aantal intensief gebruikte schijven dat ongeveer 66 20.000/300 IOP's per schijf. Het maximale aantal intensief gebruikte schijven dat voor een virtuele machine in de standaardlaag is ongeveer 40, dit is 20.000/500 IOP's per schijf. 

* **Voor Premium storage-accounts:** Premium storage-account heeft een maximale totale doorvoersnelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.

