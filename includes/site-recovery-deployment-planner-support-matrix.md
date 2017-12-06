# <a name="support-matrix"></a>Ondersteuningsmatrix

| | **VMware naar Azure** |**Hyper-V naar Azure**|**Azure naar Azure**|**Hyper-V naar secundaire site**|**VMware naar secundaire site**
--|--|--|--|--|--
Ondersteunde scenario's |Ja|Ja|Nee|Ja*|Nee
Ondersteunde versie | vCenter 6.5, 6.0 of 5.5| WindowsServer 2016, Windows Server 2012 R2 | N.v.t. |WindowsServer 2016, Windows Server 2012 R2|N.v.t.
Ondersteunde configuraties|vCenter, ESXi| Hyper-V-cluster, Hyper-V-host|N.v.t.|Hyper-V-cluster, Hyper-V-host|N.v.t.|
Aantal servers dat kan profiling wordt gestart per exemplaar van de Planner met Azure Site Recovery-implementatie |Eén (virtuele machines die horen bij een vCenter-Server of een ESXi-server kunnen profiling wordt gestart op een tijdstip)|Meerdere (VM's op meerdere hosts of hostclusters kunnen tegelijk profiel zijn)| N.v.t. |Meerdere (VM's op meerdere hosts of hostclusters kunnen tegelijk profiel zijn)| N.v.t.

* Het hulpprogramma is voornamelijk bedoeld voor de Hyper-V Azure noodherstelscenario. Voor Hyper-V voor noodherstel van de secundaire site, kan deze alleen worden gebruikt om te begrijpen van de bron aan clientzijde aanbevelingen vereist netwerkbandbreedte, vrije opslagruimte nodig op elk van de Hyper-V-bronservers en de initiële replicatie batchverwerking cijfers en batch, zoals definities.  De Azure-aanbevelingen en de kosten van het rapport negeren. De bewerking ophalen doorvoer is ook niet van toepassing voor de Hyper-V op de secundaire site noodherstelscenario.
