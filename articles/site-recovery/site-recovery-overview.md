---
title: Wat is Site Recovery? | Microsoft Docs
description: Dit artikel biedt een overzicht van de Azure Site Recovery-service en van de implementatiescenario’s.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: cfreeman
editor: ''

ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/13/2016
ms.author: raynew

---
# <a name="what-is-site-recovery?"></a>Wat is Site Recovery?
Welkom bij Azure Site Recovery! Dit artikel biedt een snel overzicht van de Site Recovery-service en legt uit hoe deze service bijdraagt aan uw bedrijf.

Uw organisatie heeft een BCDR-strategie (strategie voor bedrijfscontinuïteit en noodherstel) nodig die workloads en gegevens veilig en beschikbaar houdt tijdens geplande en ongeplande uitval en ervoor zorgt dat deze zo snel mogelijk worden hersteld naar een normale manier van functioneren. Site Recovery is een Azure-service die deel uitmaakt van deze strategie.

Site Recovery stuurt de replicatie van workloads die on-premises worden uitgevoerd op fysieke servers en virtuele machines. U kunt servers en VM’s van een primair gegevenscentrum repliceren naar de cloud (Azure) of naar een secundair datacenter. Als er uitval optreedt op de primaire site, schakelt u over naar de secundaire locatie om toepassingen en workloads toegankelijk en beschikbaar te houden. U schakelt terug naar de primaire locatie wanneer deze weer normaal functioneert.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery in de Azure-portal
Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken van en werken met resources. Het Azure Resource Manager-model en het klassieke servicebeheermodel. Azure heeft ook twee portals: de [klassieke Azure Portal](https://manage.windowsazure.com/), die ondersteuning biedt voor het klassieke implementatiemodel en [Azure Portal](https://portal.azure.com) die ondersteuning biedt voor zowel het klassieke als het Resource Manager-model.

* Site Recovery is zowel beschikbaar in de klassieke portal als in de Azure-portal.
* In de klassieke Azure Portal kunt u Site Recovery ondersteunen met het klassieke model voor het beheren van services.
* In Azure Portal kunt u implementaties volgens het klassieke model of volgens Resource Manager ondersteunen. 

De informatie in dit artikel is van toepassing op zowel klassieke als Azure-portal-implementaties. Verschillen worden vermeld, indien van toepassing.

## <a name="why-deploy-site-recovery?"></a>Waarom Site Recovery implementeren?
Dit kan Site Recovery voor uw bedrijf betekenen:

* **BCDR vereenvoudigen**: u kunt replicatie, failover en herstel van meerdere workloads op één locatie in Azure Portal verwerken. Site Recovery regelt de replicatie en failovers, maar onderschept uw toepassingsgegevens niet en heeft er ook geen informatie over.
* **Flexibele replicatie bieden**: met behulp van Site Recovery kunt u workloads repliceren die worden uitgevoerd op ondersteunde Hyper-V-VM’s, VMware-VM’s en fysieke Windows-/Linux-servers.
* **Eenvoudige replicatietests uitvoeren**: Site Recovery biedt testfailovers ter ondersteuning van noodhersteloefeningen, zonder dat dit invloed heeft op uw productieomgeving.
* **Failover en herstel**: u kunt bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies, en bij onverwachte noodsituaties ongeplande failovers met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). Na het uitvoeren van een failover kunt u weer een failback uitvoeren naar uw primaire sites. De herstelplannen van Site Recovery kunnen scripts en Azure Automation-werkmappen bevatten, zodat u failovers en het herstel van toepassingen met meerdere lagen naar behoefte kunt aanpassen.
* **Een secundair datacenter verwijderen**: u kunt workloads repliceren naar Azure in plaats van naar een secundaire site. Dit elimineert de kosten en complexiteit van het onderhoud van een secundair datacenter. Gerepliceerde gegevens worden opgeslagen in Azure Storage met alle bijbehorende flexibiliteit. VM’s worden gemaakt met de gerepliceerde gegevens wanneer een failover optreedt.
* **Integreren met bestaande BCDR-technologieën**: Site Recovery integreert met andere BCDR-functies. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Hierbij wordt ook systeemeigen ondersteuning geboden voor SQL Server AlwaysOn voor het beheren van failovers van beschikbaarheidsgroepen.

## <a name="what-can-i-replicate?"></a>Wat kan ik repliceren?
Nu volgt een samenvatting van wat u met Site Recovery kunt repliceren.

![On-premises naar on-premises](./media/site-recovery-overview/asr-overview-graphic.png)

| **REPLICEREN** | **REPLICEREN NAAR** |
| --- | --- |
| Workloads die on-premises worden uitgevoerd op virtuele VMware-machines |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md) |
| Workloads die on-premises worden uitgevoerd op Hyper-V virtuele machines die worden beheerd in VMM-clouds |[Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Secundaire site](site-recovery-vmm-to-vmm.md) |
| Workloads die on-premises worden uitgevoerd op Hyper-V virtuele machines die worden beheerd in VMM-clouds, met SANopslag |[Secundaire site](site-recovery-vmm-san.md) |
| Workloads die on-premises worden uitgevoerd op Hyper-V virtuele machines, zonder VMM |[Azure](site-recovery-hyper-v-site-to-azure.md) |
| Workloads die on-premises worden uitgevoerd op fysieke Windows-/Linux-servers |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md) |

## <a name="what-workloads-can-i-protect?"></a>Welke workloads kan ik beveiligen?
Site Recovery maakt toepassingsgevoelige BCDR mogelijk zodat workloads en apps consistent blijven werken wanneer er uitval optreedt. Site Recovery biedt:

* **Toepassingsconsistente momentopnamen**: machines repliceren met toepassingsconsistente momentopnamen voor apps met één of meer lagen. Naast het vastleggen van de schijfgegevens, leggen toepassingsconsistente momentopnamen alle gegevens in het geheugen en alle lopende transacties vast.
* **Bijna-synchrone replicatie**: Site Recovery biedt een lage replicatiefrequentie van 30 seconden voor Hyper-V en continue replicatie voor VMware.
* **Flexibele herstelplannen**: u kunt herstelplannen maken en aanpassen met externe scripts en handmatige acties. Met de integratie met Azure Automation-runbooks kunt u een volledige toepassingsstack met één klik herstellen.
* **Integratie met SQL Server AlwaysOn**: u kunt de failovers van beschikbaarheidsgroepen beheren in Site Recovery-herstelplannen.
* **Automation-bibliotheek**: een uitgebreide Azure Automation-bibliotheek met toepassingsspecifieke scripts die klaar zijn voor gebruik, en kunnen worden gedownload en geïntegreerd met Site Recovery.
* **Eenvoudig netwerkbeheer**: dankzij geavanceerd netwerkbeheer in Site Recovery en Azure gelden er minder complexe vereisten voor het toepassingsnetwerk. Dit heeft onder andere invloed op het reserveren van IP-adressen, het configureren van load balancers en het integreren van Azure Traffic Manager voor het efficiënt schakelen tussen netwerken.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie vindt u in [What workloads can Site Recovery protect?](site-recovery-workload.md) (Welke workloads kan Site Recovery beveiligen?).
* Zie [Hoe werkt Site Recovery?](site-recovery-components.md) voor meer informatie over de Site Recovery-architectuur

<!--HONumber=Oct16_HO3-->


