Een virtuele Azure-machine biedt ondersteuning voor het koppelen van meerdere gegevensschijven. Voor optimale prestaties moet u het aantal intensief gebruikte schijven dat u aan de virtuele machine koppelt, echter zo laag mogelijk houden. Zo voorkomt u dat u tegen beperkingen aanloopt. Als de diverse schijven niet op hetzelfde moment maximaal worden gebruikt, kan het opslagaccount een groter aantal schijven ondersteunen.

* **Voor Azure Managed schijven:** limiet voor het aantal beheerde schijven is regionaal en ook afhankelijk van het opslagtype. De standaardinstelling en ook het maximum aantal is 10.000 per abonnement, per regio en per opslagtype. Bijvoorbeeld, kunt u maximaal 10.000 standaard beheerd schijven en ook 10.000 premium schijven in een abonnement en in een regio die worden beheerd. 

    De limiet voor Managed Disks geldt ook voor beheerde momentopnamen en installatiekopieën.

* **Voor standaardopslagaccounts:** een standaardopslagaccount heeft een maximale totale aanvraagsnelheid van 20.000 IOP's. Het totale aantal IOP's van alle schijven van een virtuele machine in een standaardopslagaccount mag niet groter zijn dan deze limiet.
  
    U kunt het aantal intensief gebruikte schijven dat binnen één standaardopslagaccount wordt ondersteund, ruwweg berekenen op basis van de limiet voor het aantal aanvragen. Het maximale aantal intensief gebruikte schijven voor een virtuele machine in de categorie Basic is bijvoorbeeld ongeveer 66 (20.000/300 IOP's per schijf). Voor een virtuele machine in de categorie Standard is dit ongeveer 40 (20.000/500 IOP's per schijf). Zie de tabel hieronder. 
* **Voor Premium-opslagaccounts:** Premium-opslagaccounts bieden een maximale totale doorvoersnelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.

