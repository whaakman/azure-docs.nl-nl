Een virtuele Azure-machine biedt ondersteuning voor het koppelen van meerdere gegevensschijven. Dit artikel wordt beschreven schaalbaarheids- en prestatiedoelen voor gegevensschijven van een virtuele machine. Gebruik deze doelen bij het bepalen van het aantal en type van de schijf die u nodig hebt om te voldoen aan de vereisten van uw prestaties en capaciteit. 

> [!IMPORTANT]
> Voor optimale prestaties, het aantal maximaal gebruikte schijven zijn gekoppeld aan de virtuele machine om te voorkomen dat mogelijk bandbreedtebeperking te beperken. Als u alle gekoppelde schijven maximaal niet tegelijkertijd worden gebruikt, kan een groter aantal schijven ondersteunen op de virtuele machine.

* **Voor Azure schijven die worden beheerd:** 

> | Resource | Standaardlimiet | Maximumaantal |
> | --- | --- | --- |
> | Standard Managed Disks | 10.000 | 50,000 |
> | Standard SSD Managed Disks | 10.000 | 50,000 |
> | Premium Managed Disks | 10.000 | 50,000 |
> | Standard_LRS momentopnamen | 10.000 | 50,000 |
> | Standard_ZRS momentopnamen | 10.000 | 50,000 |
> | Premium_LRS momentopnamen | 10.000 | 50,000 |
> | Begeleide afbeelding | 10.000 | 50,000 |

* **Voor standaardopslagaccounts:** een standaardopslagaccount heeft een maximale totale aanvraagsnelheid van 20.000 IOP's. Het totale aantal IOP's van alle schijven van een virtuele machine in een standaardopslagaccount mag niet groter zijn dan deze limiet.
  
    U kunt het aantal intensief gebruikte schijven dat binnen één standaardopslagaccount wordt ondersteund, ruwweg berekenen op basis van de limiet voor het aantal aanvragen. Voor een basis-laag VM, het maximum aantal maximaal gebruikte schijven over 66 (20.000/300 IOP's per schijf) en voor een Standard-laag VM is, is het bijvoorbeeld ongeveer 40 (20.000/500 IOP's per schijf). 

* **Voor Premium-opslagaccounts:** Premium-opslagaccounts bieden een maximale totale doorvoersnelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.

