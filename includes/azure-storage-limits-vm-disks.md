Een virtuele Azure-machine biedt ondersteuning voor het koppelen van meerdere gegevensschijven. Voor optimale prestaties moet u het aantal intensief gebruikte schijven dat u aan de virtuele machine koppelt, echter zo laag mogelijk houden. Zo voorkomt u dat u tegen beperkingen aanloopt. Als de diverse schijven niet op hetzelfde moment maximaal worden gebruikt, kan het opslagaccount een groter aantal schijven ondersteunen.

* **Voor Azure Managed Disks:** het maximale aantal Managed Disks-schijven is afhankelijk van de regio van uw abonnement. De standaardlimiet is 2000. Neem contact op met de ondersteuning van Azure als u de limiet wilt verhogen.

    De limiet voor Managed Disks geldt ook voor beheerde momentopnamen en installatiekopieën.

* **Voor standaardopslagaccounts:** een standaardopslagaccount heeft een maximale totale aanvraagsnelheid van 20.000 IOP's. Het totale aantal IOP's van alle schijven van een virtuele machine in een standaardopslagaccount mag niet groter zijn dan deze limiet.
  
    U kunt het aantal intensief gebruikte schijven dat binnen één standaardopslagaccount wordt ondersteund, ruwweg berekenen op basis van de limiet voor het aantal aanvragen. Het maximale aantal intensief gebruikte schijven voor een virtuele machine in de categorie Basic is bijvoorbeeld ongeveer 66 (20.000/300 IOP's per schijf). Voor een virtuele machine in de categorie Standard is dit ongeveer 40 (20.000/500 IOP's per schijf). Zie de tabel hieronder. 
* **Voor Premium-opslagaccounts:** Premium-opslagaccounts bieden een maximale totale doorvoersnelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.



<!--HONumber=Feb17_HO2-->


