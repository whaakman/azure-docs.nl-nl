---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: aa701ada917811382351fee9469a5cfa9a7599b8
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279575"
---
Een virtuele Azure-machine biedt ondersteuning voor het koppelen van meerdere gegevensschijven. Dit artikel wordt beschreven schaalbaarheids- en prestatiedoelen voor gegevensschijven van een virtuele machine. Gebruik deze doelen om te bepalen het aantal en type van de schijf die u nodig hebt om te voldoen aan de vereisten van uw prestaties en capaciteit. 

> [!IMPORTANT]
> Voor optimale prestaties, het aantal intensief gebruikte schijven die zijn gekoppeld aan de virtuele machine om te voorkomen dat mogelijk beperking te beperken. Als alle gekoppelde schijven maximaal niet op hetzelfde moment worden gebruikt, kan de virtuele machine een groter aantal schijven ondersteunen.

* **Voor Azure Managed Disks:** 

> | Resource | Standaardlimiet | Maximumaantal |
> | --- | --- | --- |
> | Standard Managed Disks | 10.000 | 50,000 |
> | Standard SSD Managed Disks | 10.000 | 50,000 |
> | Premium Managed Disks | 10.000 | 50,000 |
> | Standard_LRS momentopnamen | 10.000 | 50,000 |
> | Standard_ZRS momentopnamen | 10.000 | 50,000 |
> | Beheerde installatiekopie | 10.000 | 50,000 |

* **Voor standaardopslagaccounts:** een standaardopslagaccount heeft een maximale totale aanvraagsnelheid van 20.000 IOP's. Het totale aantal IOP's van alle schijven van een virtuele machine in een standaardopslagaccount mag niet groter zijn dan deze limiet.
  
    U kunt het aantal intensief gebruikte schijven dat binnen één standaardopslagaccount wordt ondersteund, ruwweg berekenen op basis van de limiet voor het aantal aanvragen. Bijvoorbeeld, voor een VM in Basic-laag, het maximum aantal intensief gebruikte schijven dat ongeveer 66 (20.000/300 IOP's per schijf) en voor een VM uit de Standard-laag is, is dit ongeveer 40 (20.000/500 IOP's per schijf). 

* **Voor Premium-opslagaccounts:** Premium-opslagaccounts bieden een maximale totale doorvoersnelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.

