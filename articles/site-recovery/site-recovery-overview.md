---
title: Wat is Site Recovery? | Microsoft Docs
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
ms.date: 03/14/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: d8e4e4bb7dd1e40d8c561adba04b8346fcb2127d
ms.lasthandoff: 03/15/2017


---
# <a name="what-is-site-recovery"></a>Wat is Site Recovery?

Welkom bij de Azure Site Recovery-service! In dit artikel vindt u een kort overzicht van de service.

Storingen worden veroorzaakt door natuurlijke gebeurtenissen en operationele fouten. Uw organisatie heeft een BCDR-strategie (strategie voor bedrijfscontinuïteit en noodherstel) nodig die apps en gegevens veilig en beschikbaar houdt tijdens geplande en ongeplande uitval. Daarnaast moet er een strategie zijn om de normale manier van functioneren na uitval zo snel mogelijk te herstellen.

Azure Recovery Services dragen bij aan uw BCDR-strategie. De [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/)-service slaat uw gegevens veilig en herstelbaar op. Gebruik Site Recovery voor replicatie, failover en herstel van workloads, zodat ze beschikbaar blijven in geval van storing.

## <a name="what-does-site-recovery-provide"></a>Wat biedt Site Recovery?

- **Herstel na noodgevallen in de cloud**: U kunt de workloads die op virtuele machines en fysieke servers worden uitgevoerd, naar Azure repliceren, in plaats van naar een secundaire site. Dit elimineert de kosten en complexiteit van het onderhoud van een secundair datacenter.
- **Flexibele replicatie voor hybride omgevingen**: U kunt alle workloads repliceren die worden uitgevoerd op ondersteunde on-premises Hyper-V-VM's, VMware-VM's en fysieke Windows-/Linux-servers.
- **Migratie**: U kunt Site Recovery gebruiken om on-premises AWS-exemplaren naar virtuele Azure-machines te migreren of om virtuele Azure-machines tussen verschillende Azure-regio's te migreren.
- **Vereenvoudigde BCDR**: U kunt in Azure Portal replicatie vanaf één locatie implementeren.  U kunt eenvoudige failovers en failback van een of meer machines uitvoeren.
- **Veerkracht**: Site Recovery deelt replicatie en failover in zonder toepassingsgegevens te onderscheppen.
Gerepliceerde gegevens worden opgeslagen in Azure Storage met alle bijbehorende flexibiliteit. Wanneer er een failover optreedt, worden er virtuele Azure-machines gemaakt met de gerepliceerde gegevens.
- **Replicatieresultaat**: Site Recovery biedt een lage replicatiefrequentie van slechts 30 seconden voor Hyper-V en continue replicatie voor VMware. U kunt drempelwaarden voor het beoogde herstelpunt (RPO) instellen om te bepalen hoe vaak er gegevensherstelpunten moeten worden gemaakt. Bovendien kunt u de beoogde hersteltijd (RTO) beperken met het automatische herstelproces van Site Recovery en integratie met [Azure Traffic Manager](https://azure.microsoft.com/en-us/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/)
- **Toepassingsconsistentie**: Machines repliceren met toepassingsconsistente momentopnamen. Met toepassingsconsistente momentopnamen worden niet alleen schijfgegevens vastgelegd, maar ook alle gegevens in het geheugen en alle lopende transacties.
- **Testen zonder onderbreking**: U kunt testfailovers uitvoeren ter ondersteuning van noodhersteloefeningen, zonder dat dit invloed heeft op uw productieomgeving.
- **Flexibele failover en herstel**: U kunt bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies, en bij onverwachte noodsituaties ongeplande failovers uitvoeren met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). U kunt weer terug naar uw primaire site wanneer deze weer beschikbaar is.
- **Aangepaste herstelplannen**: met herstelplannen kunt u failover en herstel instellen en aanpassen voor toepassingen met meerdere lagen die zijn verdeeld over meerdere virtuele machines. U rangschikt groepen binnen plannen en voegt scripts en handmatige acties toe. Herstelplannen kunnen worden geïntegreerd met Azure Automation-runbooks.
- **Multi-tier apps**: U kunt herstelplannen maken voor opeenvolgende failovers en herstel van multi-tiered apps. U kunt machines in verschillende lagen (bijvoorbeeld database, web, app) binnen een herstelplan groeperen en de wijze van failover en opstarten aanpassen.
* **Integratie met bestaande BCDR-technologieën**: Site Recovery kan worden geïntegreerd met andere BCDR-technologieën. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Hierbij wordt ook systeemeigen ondersteuning geboden voor SQL Server AlwaysOn voor het beheren van failovers van beschikbaarheidsgroepen.
* **Integratie met de Automation-bibliotheek**: De uitgebreide Azure Automation-bibliotheek biedt toepassingsspecifieke scripts die klaar zijn voor gebruik en kunnen worden gedownload en geïntegreerd met Site Recovery.
* **Eenvoudig netwerkbeheer**: Dankzij geavanceerd netwerkbeheer in Site Recovery en Azure gelden er minder complexe vereisten voor het toepassingsnetwerk. Dit heeft onder andere invloed op het reserveren van IP-adressen, het configureren van load balancers en het integreren van Azure Traffic Manager voor het efficiënt schakelen tussen netwerken.


## <a name="whats-supported"></a>Wat wordt er ondersteund?

**Ondersteund** | **Details**
--- | ---
**Welke regio's worden ondersteund voor Site Recovery?** | [Ondersteunde regio's](https://azure.microsoft.com/en-us/regions/services/) |
**Wat kan ik repliceren?** | On-premises VMware-VM's, Hyper-V-VM's, fysieke Windows- en Linux-servers.
**Welke besturingssystemen zijn vereist voor gerepliceerde machines?** | [Ondersteunde besturingssystemen](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) voor VMware-VM's<br/><br/> Voor Hyper-V-VM's worden alle [gastbesturingssystemen](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) ondersteund die door Azure en Hyper-V worden ondersteund.<br/><br/> [Besturingssystemen](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) voor fysieke servers
**Waar kan ik naar repliceren?** | Naar Azure Storage of naar een secundair datacenter<br/><br/> Voor Hyper-V kunnen alleen virtuele machines op Hyper-V-hosts die in System Center VMM-clouds worden beheerd, naar een secundair datacenter repliceren.
**Welke VMware-servers/-hosts moet ik gebruiken?** | De VMware-VM's die u wilt repliceren, kunnen worden beheerd door [ondersteunde vSphere-hosts/vCenter-servers](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)
**Welke workloads kan ik repliceren?** | U kunt iedere werkload repliceren die wordt uitgevoerd op een ondersteunde replicatiemachine. Daarnaast heeft het Site Recovery-team specifieke tests uitgevoerd voor een [aantal apps](site-recovery-workload.md#workload-summary).


## <a name="which-azure-portal"></a>Welke Azure-portal?

* Site Recovery kan zowel in de nieuwere [Azure Portal](https://portal.azure.com) als in de [klassieke Azure-portal](https://manage.windowsazure.com/)worden geïmplementeerd.
* In de klassieke Azure-portal kunt u Site Recovery ondersteunen met het klassieke model voor het beheren van services.
* In Azure Portal kunt u implementaties volgens het klassieke model of volgens het nieuwere [Resource Manager-implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md) ondersteunen.
- De klassieke portal moet alleen worden gebruikt voor het onderhouden van de bestaande Site Recovery-implementaties. U kunt via de klassieke portal geen nieuwe kluizen maken.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [workloadondersteuning](site-recovery-workload.md)
* Meer informatie over [Site Recovery-architectuur en -onderdelen](site-recovery-components.md)

