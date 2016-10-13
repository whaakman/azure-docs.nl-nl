<properties
    pageTitle="Wat is Site Recovery? | Microsoft Azure" 
    description="In dit artikel wordt een overzicht van de Azure Site Recovery-service geboden en uitgelegd hoe de service kan worden geïmplementeerd." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>


#  Wat is Site Recovery?

Welkom bij Azure Site Recovery! Begin met dit artikel om snel een beeld te krijgen van de Site Recovery-service en hoe deze kan bijdragen aan uw zakelijke continuïteit en noodherstelplan (BCDR).

## Overzicht

Organisaties hebben een BCDR-strategie nodig die bepaalt op welke wijze toepassingen, workloads en gegevens beschikbaar blijven tijdens geplande en ongeplande uitval, en ervoor zorgt dat deze zo snel mogelijk worden hersteld naar een normale manier van functioneren. In uw BCDR-strategie moeten bedrijfsgegevens veilig en herstelbaar blijven. Ook moet ervoor worden gezorgd dat workloads continu beschikbaar blijven wanneer zich een noodsituatie voordoet. 

Site Recovery is een Azure-service die bijdraagt aan uw BCDR-strategie door replicatie van fysieke on-premises servers en virtuele machines in de cloud (Azure) of naar een secundair datacenter te organiseren. Als er uitval optreedt op uw primaire locatie, schakelt u over naar de secundaire locatie om toepassingen en workloads beschikbaar te houden. U schakelt terug naar de primaire locatie wanneer deze weer normaal functioneert. Meer informatie vindt u in [Wat is Site Recovery?](site-recovery-overview.md)

## Site Recovery in de Azure-portal

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken van en werken met resources: het Azure Resource Manager-model (ARM) en het klassieke model voor het beheren van services. Azure heeft ook twee portals: de [klassieke Azure Portal](https://manage.windowsazure.com/), die ondersteuning biedt voor het klassieke implementatiemodel, en de [Azure Portal](https://portal.azure.com), die ondersteuning biedt voor beide implementatiemodellen.

Site Recovery is zowel beschikbaar in de klassieke portal als in de Azure-portal. In de klassieke Azure-portal kunt u Site Recovery ondersteunen met het klassieke model voor het beheren van services. In de Azure-portal kunt u implementaties volgens het klassieke model of volgens Resource Manager ondersteunen. [Lees meer informatie](site-recovery-overview.md#site-recovery-in-the-azure-portal) over implementeren via de Azure-portal.

De informatie in dit artikel is van toepassing op zowel klassieke als Azure-portal-implementaties. Verschillen worden vermeld, indien van toepassing.


## Waarom u Site Recovery moet gebruiken 

Dit kan Site Recovery voor uw bedrijf betekenen:

- **Uw BCDR-strategie vereenvoudigen**: met Site Recovery is het eenvoudig om replicatie, failovers en herstel van meerdere zakelijke workloads en apps vanaf één locatie te beheren. Site Recovery regelt de replicatie en failovers, maar onderschept uw toepassingsgegevens niet en heeft er ook geen informatie over.
- **Flexibele replicatie bieden**: met behulp van Site Recovery kunt u workloads repliceren die worden uitgevoerd op virtuele Hyper-V-machines, virtuele VMware-machines en fysieke Windows-/Linux-servers. 
- **Eenvoudige failover en herstel**: Site Recovery biedt testfailovers ter ondersteuning van noodhersteloefeningen, zonder dat dat invloed heeft op uw productieomgeving. Bovendien kunt u bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies, en bij onverwachte noodsituaties ongeplande failovers met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). Na het uitvoeren van een failover kunt u weer een failback uitvoeren naar uw primaire sites. De herstelplannen van Site Recovery kunnen scripts en Azure Automation-werkmappen bevatten, zodat u failovers en het herstel van toepassingen met meerdere lagen naar behoefte kunt aanpassen. 
- **Secundair datacenter elimineren**: u kunt repliceren naar een secundaire on-premises site of naar Azure. Met Azure als bestemming voor herstel na noodgevallen worden de kosten en complexiteit voor het beheren van een secundaire site geëlimineerd. Gerepliceerde gegevens worden opgeslagen in Azure Storage, waarbij u profiteert van alle bijbehorende flexibiliteit.
- **Integreren met bestaande BCDR-technologieën**: Site Recovery werkt samen met BCDR-functies van andere toepassingen. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Hierbij wordt ook systeemeigen ondersteuning geboden voor SQL Server AlwaysOn voor het beheren van failovers van beschikbaarheidsgroepen. 

## Wat kan ik repliceren?

Nu volgt een samenvatting van wat u met Site Recovery kunt repliceren.

![On-premises naar on-premises](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLICEREN** | **REPLICEREN VANAF (ON-PREMISES)** | **REPLICEREN NAAR** | **ARTIKEL**
---|---|---|---
Virtuele VMware-machines | VMware-server | Azure | [Meer informatie](site-recovery-vmware-to-azure-classic.md)
Virtuele VMware-machines | VMware-server | Secundaire VMware-site | [Meer informatie](site-recovery-vmware-to-vmware.md) 
Virtuele Hyper-V-machines | Hyper-V-host in VMM-cloud | Azure | [Meer informatie](site-recovery-vmm-to-azure.md) 
Virtuele Hyper-V-machines | Hyper-V-host in VMM-cloud | Secundaire VMM-site | [Meer informatie](site-recovery-vmm-to-vmm.md)
Virtuele Hyper-V-machines | Hyper-V-host in VMM-cloud met SAN-opslag| Secundaire VMM-site met SAN-opslag | [Meer informatie](site-recovery-vmm-san.md)
Virtuele Hyper-V-machines | Hyper-V-host (geen VMM) | Azure | [Meer informatie](site-recovery-hyper-v-site-to-azure.md)
Fysieke Windows/Linux-servers | Fysieke server | Azure | [Meer informatie](site-recovery-vmware-to-azure-classic.md)
Workloads die worden uitgevoerd op fysieke Windows-/Linux-servers | Fysieke server | Secundair datacenter | [Meer informatie](site-recovery-vmware-to-vmware.md) 


## Welke workloads kan ik beveiligen?

Site Recovery kan helpen bij toepassingsgevoelige BCDR, zodat workloads en apps consistent blijven werken wanneer er uitval optreedt. Site Recovery biedt: 

- **Toepassingsconsistente momentopnamen**: replicatie met toepassingsconsistente momentopnamen voor apps met één of meer lagen.
- **Bijna-synchrone replicatie**: een replicatiefrequentie van 30 seconden voor Hyper-V en continue replicatie voor VMware.
- **Integratie met SQL Server AlwaysOn**: u kunt de failovers van beschikbaarheidsgroepen beheren in Site Recovery-herstelplannen. 
- **Flexibele herstelplannen**: u kunt herstelplannen maken en aanpassen met externe scripts, handmatige acties en Azure Automation-runbooks. Op die manier kunt u een volledige reeks toepassingen met één klik herstellen.
- **Automation-bibliotheek**: een uitgebreide Azure Automation-bibliotheek met toepassingsspecifieke scripts die klaar zijn voor gebruik, en kunnen worden gedownload en geïntegreerd met Site Recovery.
- **Eenvoudig netwerkbeheer**: dankzij geavanceerd netwerkbeheer in Site Recovery en Azure gelden er minder complexe vereisten voor het toepassingsnetwerk. Dit heeft onder andere invloed op het reserveren van IP-adressen, het configureren van load balancers en het integreren van Azure Traffic Manager voor het efficiënt schakelen tussen netwerken.


## Volgende stappen

- Meer informatie vindt u in [What workloads can Site Recovery protect?](site-recovery-workload.md) (Welke workloads kan Site Recovery beveiligen?).
- Zie [Hoe werkt Site Recovery?](site-recovery-components.md) voor meer informatie over de Site Recovery-architectuur
 



<!--HONumber=Oct16_HO1-->


