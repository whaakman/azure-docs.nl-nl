---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: 09b4f94db3464943a8367bfb3ca89f9a88446193
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553710"
---
U kunt een aantal gegevensschijven koppelen aan een virtuele machine van Azure. Op basis van de schaalbaarheids- en prestatiedoelen voor gegevensschijven van een virtuele machine, kunt u het aantal en type van de schijf die u nodig hebt om te voldoen aan uw prestaties en capaciteitsvereisten bepalen.

> [!IMPORTANT]
> Voor optimale prestaties, het aantal intensief gebruikte schijven die zijn gekoppeld aan de virtuele machine om te voorkomen dat mogelijk beperking te beperken. Als alle gekoppelde schijven worden niet op hetzelfde moment maximaal hebt gebruikt, kan de virtuele machine een groter aantal schijven ondersteunen.

* **Beheerde schijven voor Azure:** 

> | Resource | Standaardlimiet | Maximumaantal |
> | --- | --- | --- |
> | Standaard beheerde schijven | 10.000 | 50,000 |
> | Managed Disks - Standard - SSD | 10.000 | 50,000 |
> | Beheerde Premium-schijven | 10.000 | 50,000 |
> | Standard_LRS momentopnamen | 10.000 | 50,000 |
> | Standard_ZRS momentopnamen | 10.000 | 50,000 |
> | Beheerde installatiekopie | 10.000 | 50,000 |

* **Voor Standard storage-accounts:** Een standaardopslagaccount heeft een maximale totale aanvraagsnelheid van 20.000 IOP's. De totale IOPS over alle van de schijven van uw virtuele machines in een Standard storage-account mag niet groter zijn dan deze limiet.
  
    U kunt het aantal intensief gebruikte schijven dat wordt ondersteund door een enkele Standard storage-account op basis van de aanvraag-frequentielimiet ongeveer berekenen. Bijvoorbeeld, voor een Basic-laag virtuele machine is het maximum aantal intensief gebruikte schijven dat ongeveer 66 20.000/300 IOP's per schijf. Het maximale aantal intensief gebruikte schijven dat voor een virtuele machine in de standaardlaag is ongeveer 40, dit is 20.000/500 IOP's per schijf. 

* **Voor Premium storage-accounts:** Premium storage-account heeft een maximale totale doorvoersnelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.

