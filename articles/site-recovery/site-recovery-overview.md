---
title: Wat is Azure Site Recovery? | Microsoft Docs
description: "Dit artikel biedt een overzicht van de Azure Site Recovery-service en van de implementatiescenario’s."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: aa657c92f347f7529affee78ad1842e5e066b74d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-site-recovery"></a>Wat is Site Recovery?

Welkom bij de Azure Site Recovery-service! In dit artikel vindt u een kort overzicht van de service.

## <a name="business-continuity-and-disaster-recovery-bcdr-with-azure-recovery-services"></a>Bedrijfscontinuïteit en herstel na noodgevallen (BCDR) met Azure Recovery Services

Als organisatie moet u nagaan hoe u uw gegevens kunt beveiligen en apps/workloads kunt uitvoeren wanneer geplande en niet-geplande uitval optreedt.

Azure Recovery Services dragen bij aan uw BCDR-strategie:

- **Site Recovery-service**: met Site Recovery bent u verzekerd van bedrijfscontinuïteit door uw apps uit te voeren op virtuele machines en fysieke servers die beschikbaar zijn als een site uitvalt. Met Site Recovery repliceert u workloads die worden uitgevoerd op virtuele machines en fysieke servers, zodat deze beschikbaar blijven op een secundaire locatie als de primaire site niet beschikbaar is. Hiermee herstelt u werkbelastingen op de primaire site wanneer deze weer actief is.
- **Backup-service**: daarnaast kunt u met de [Azure Backup](https://docs.microsoft.com/azure/backup/)-service uw gegevens beveiligen en herstellen door hiervan een back-up te maken naar Azure.

Met Site Recovery kunt u replicatie beheren voor:

- Azure-VM's die worden gerepliceerd tussen Azure-regio's.
- On-premises virtuele machines en fysieke servers die worden gerepliceerd naar Azure of naar een secundaire site.


## <a name="what-does-site-recovery-provide"></a>Wat biedt Site Recovery?

**Functie** | **Details**
--- | ---
**Een eenvoudige BCDR-oplossing implementeren** | Met Site Recovery kunt u replicatie, failover en failback instellen en beheren vanaf één locatie in de Azure portal.
**Azure-VM's repliceren** | U kunt uw BCDR-strategie zo instellen dat Azure-VM's worden gerepliceerd tussen Azure-regio's.
**On-premises VM's offsite repliceren** | U kunt on-premises VM's en fysieke servers repliceren naar Azure of naar een secundaire on-premises locatie. Hierdoor kunt u een secundair datacenter goedkoper en eenvoudiger onderhouden.
**Workloads repliceren** | U kunt alle workloads repliceren die worden uitgevoerd op ondersteunde Azure-VM's, on-premises Hyper-V-VM's, VMware-VM's en fysieke Windows-/Linux-servers.
**Gegevens flexibel en veilig bewaren** | Met Site Recovery deelt u replicatie in zonder toepassingsgegevens te onderscheppen. Gerepliceerde gegevens worden opgeslagen in Azure Storage met alle bijbehorende flexibiliteit. Wanneer er een failover optreedt, worden er virtuele Azure-machines gemaakt met de gerepliceerde gegevens.
**Voldoen aan de RTO's en RPO's** | Zorg dat de beoogde hersteltijden (RTO) en beoogde herstelpunten (RPO) binnen de organisatielimieten blijven. Site Recovery biedt continue replicatie voor Azure-VM's en VMware-VM's en een lage replicatiefrequentie van slechts 30 seconden voor Hyper-V. U kunt beoogde hersteltijden (RTO) verder beperken door de integratie met [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/).
**Apps consistent houden bij failover** | U kunt herstelpunten configureren met toepassingsconsistente momentopnamen. Met toepassingsconsistente momentopnamen kunt u schijfgegevens, alle gegevens in het geheugen en alle lopende transacties vastleggen.
**Testen zonder onderbreking** | U kunt eenvoudig testfailovers uitvoeren ter ondersteuning van noodhersteloefeningen, zonder dat dit van invloed is op de doorlopende replicatie.
**Flexibele failovers uitvoeren** | U kunt bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies en bij onverwachte noodsituaties ongeplande failovers uitvoeren met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). U kunt weer terug naar uw primaire site wanneer deze weer beschikbaar is.
**Herstelplannen maken** | U kunt failovers en herstel van toepassingen met meerdere lagen op meerdere virtuele machines aanpassen en indelen met herstelplannen. U groepeert machines binnen plannen en voegt scripts en handmatige acties toe. Herstelplannen kunnen worden geïntegreerd met Azure Automation-runbooks.
**Integreren met bestaande BCDR-technologieën** | Met Site Recovery kunt u integreren met andere BCDR-technologieën. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Hierbij wordt ook systeemeigen ondersteuning geboden voor SQL Server AlwaysOn voor het beheren van failovers van beschikbaarheidsgroepen.
**Integreren met de Automation-bibliotheek** | Een uitgebreide Azure Automation-bibliotheek met toepassingsspecifieke scripts die klaar zijn voor gebruik en kunnen worden gedownload en geïntegreerd met Site Recovery.
**Netwerkinstellingen beheren** | Site Recovery kan worden geïntegreerd met Azure voor eenvoudig toepassingsnetwerkbeheer, waaronder het reserveren van IP-adressen, het configureren van load balancers en het integreren van Azure Traffic Manager voor het efficiënt schakelen tussen netwerken.


## <a name="what-can-i-replicate"></a>Wat kan ik repliceren?

**Ondersteund** | **Details**
--- | ---
**Wat kan ik repliceren?** | Azure-VM's tussen Azure-regio's (in preview)<br/><br/>  On-premises VMware-VM's, Hyper-V-VM's, fysieke servers (Windows en Linux) naar Azure<br/> On-premises VMware-VM's, Hyper-V-VM's, fysieke servers naar een secundaire site. Voor Hyper-V-VM's wordt replicatie naar een secundaire site alleen ondersteund als Hyper-V-hosts worden beheerd door System Center VMM.
**Welke regio's worden ondersteund voor Site Recovery?** | [Ondersteunde regio's](https://azure.microsoft.com/regions/services/) |
**Welke besturingssystemen zijn vereist voor gerepliceerde machines?** | [Vereisten voor Azure-VM's](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)<br></br>[Vereisten voor VMware-VM's](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> Voor Hyper-V-VM's worden alle [gastbesturingssystemen](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) ondersteund die door Azure en Hyper-V worden ondersteund.<br/><br/> [Vereisten voor fysieke servers](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Welke VMware-servers/-hosts moet ik gebruiken?** | VMware-VM's kunnen zich bevinden op [ondersteunde vSphere-hosts/vCenter-servers](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)
**Welke workloads kan ik repliceren?** | U kunt iedere werkload repliceren die wordt uitgevoerd op een ondersteunde replicatiemachine. Daarnaast heeft het Site Recovery-team specifieke tests uitgevoerd voor een [aantal apps](site-recovery-workload.md#workload-summary).


## <a name="azure-portal-considerations"></a>Overwegingen voor Azure Portal

* Site Recovery kan worden geïmplementeerd in [Azure Portal](https://portal.azure.com).
* In de klassieke Azure Portal kunt u Site Recovery beheren met het klassieke model voor het beheren van services.
- De klassieke portal moet alleen worden gebruikt voor het onderhouden van de bestaande Site Recovery-implementaties. U kunt via de klassieke portal geen nieuwe kluizen maken.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [workloadondersteuning](site-recovery-workload.md)
* Aan de slag met [Azure VM-replicatie tussen regio's](site-recovery-azure-to-azure.md), [VMware-replicatie naar Azure](vmware-walkthrough-overview.md) of [Hyper-V-replicatie naar Azure](hyper-v-site-walkthrough-overview.md).
