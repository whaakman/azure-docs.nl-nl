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
ms.date: 10/30/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 64d085bff08d9a824204851d32504fac3e79024c


---
# <a name="what-is-site-recovery"></a>Wat is Site Recovery?
Welkom bij de Azure Site Recovery-service! In dit artikel vindt u een kort overzicht van Site Recovery.

Uw organisatie heeft een BCDR-strategie (strategie voor bedrijfscontinuïteit en noodherstel) nodig die apps en gegevens veilig en beschikbaar houdt tijdens geplande en ongeplande uitval. Daarnaast moet er een strategie zijn om de normale manier van functioneren zo snel mogelijk te herstellen na uitval.

Site Recovery draagt bij aan uw BCDR-strategie door de replicatie van on-premises virtuele machines en fysieke servers te orkestreren. U kunt servers en VM's van een primair on-premises datacenter repliceren naar de cloud (Azure) of naar een secundair datacenter.

Als er uitval optreedt op de primaire site, schakelt u over naar de secundaire locatie om workloads toegankelijk en beschikbaar te houden. U schakelt terug naar de primaire locatie wanneer deze weer normaal functioneert.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery in de Azure-portal
Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken van en werken met resources. Het Azure Resource Manager-model en het klassieke servicebeheermodel. Azure heeft ook twee portals. De [klassieke Azure-portal](https://manage.windowsazure.com/) en [Azure Portal](https://portal.azure.com).

* Site Recovery is zowel beschikbaar in de klassieke portal als in Azure Portal.
* In de klassieke Azure Portal kunt u Site Recovery ondersteunen met het klassieke model voor het beheren van services.
* In Azure Portal kunt u implementaties volgens het klassieke model of volgens Resource Manager ondersteunen.

De informatie in dit artikel is van toepassing op zowel klassieke als Azure-portal-implementaties. Verschillen worden vermeld, indien van toepassing.

## <a name="why-deploy-site-recovery"></a>Waarom Site Recovery implementeren?
Dit kan Site Recovery voor uw bedrijf betekenen:

* **BCDR vereenvoudigen**: u kunt replicatie, failover en herstel van meerdere workloads op één locatie in Azure Portal verwerken. Site Recovery deelt replicatie en failover in zonder toepassingsgegevens te onderscheppen.
* **Flexibele replicatie bieden**: u kunt workloads repliceren die worden uitgevoerd op ondersteunde Hyper-V-VM's, VMware-VM's en fysieke Windows-/Linux-servers.
* **Een secundair datacenter verwijderen**: u kunt workloads repliceren naar Azure in plaats van naar een secundaire site. Dit elimineert de kosten en complexiteit van het onderhoud van een secundair datacenter. Gerepliceerde gegevens worden opgeslagen in Azure Storage met alle bijbehorende flexibiliteit. Wanneer een failover optreedt, worden Azure VM's gemaakt met de gerepliceerde gegevens.
* **Eenvoudige replicatietests uitvoeren**: u kunt testfailovers uitvoeren ter ondersteuning van noodhersteloefeningen, zonder dat dit invloed heeft op uw productieomgeving.
* **Failover en herstel**: u kunt bij verwachte uitval geplande failovers uitvoeren zonder gegevensverlies, en bij onverwachte noodsituaties ongeplande failovers uitvoeren met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie). U kunt weer terug naar uw primaire site wanneer deze opnieuw beschikbaar is.
* **Meerdere VM-failovers**: u kunt herstelplannen configureren die ook scripts en Azure Automation-runbooks omvatten. Met herstelplannen kunt u failover en herstel modelleren en aanpassen voor toepassingen met meerdere lagen die worden verdeeld over meerdere virtuele machines.
* **Integreren met bestaande BCDR-technologieën**: Site Recovery kan worden geïntegreerd met andere BCDR-technologieën. U kunt Site Recovery bijvoorbeeld gebruiken ter bescherming van de SQL Server-back-end van zakelijke workloads. Hierbij wordt ook systeemeigen ondersteuning geboden voor SQL Server AlwaysOn voor het beheren van failovers van beschikbaarheidsgroepen.

## <a name="what-can-i-replicate"></a>Wat kan ik repliceren?
Nu volgt een samenvatting van wat u met Site Recovery kunt repliceren.

![On-premises naar on-premises](./media/site-recovery-overview/asr-overview-graphic.png)

| **REPLICEREN** | **REPLICEREN NAAR** |
| --- | --- |
| On-premises VMware VM's |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md) |
| On-premises Hyper-V VM's die worden beheerd in VMM-clouds |[Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Secundaire site](site-recovery-vmm-to-vmm.md) |
| On-premises Hyper-V VM's die worden beheerd in VMM-clouds, met SAN-opslag |[Secundaire site](site-recovery-vmm-san.md) |
| On-premises Hyper-V VM's, zonder VMM |[Azure](site-recovery-hyper-v-site-to-azure.md) |
| On-premises fysieke Windows/Linux-servers |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Secundaire site](site-recovery-vmware-to-vmware.md) |

## <a name="how-does-site-recovery-protect-workloads"></a>Hoe worden workloads beveiligd door Site Recovery?
Site Recovery maakt toepassingsgevoelige replicatie mogelijk, zodat workloads en apps consistent blijven werken wanneer er uitval optreedt.

* **Toepassingsconsistente momentopnamen**: machines repliceren met toepassingsconsistente momentopnamen voor apps met één of meer lagen. Met toepassingsconsistente momentopnamen worden niet alleen schijfgegevens vastgelegd, maar ook alle gegevens in het geheugen en alle lopende transacties.
* **Bijna-synchrone replicatie**: Site Recovery biedt een lage replicatiefrequentie van 30 seconden voor Hyper-V en continue replicatie voor VMware.
* **Flexibele herstelplannen**: u kunt herstelplannen maken en aanpassen met externe scripts en handmatige acties. Met de integratie met Azure Automation-runbooks kunt u een volledige toepassingsstack met één klik herstellen.
* **Integratie met SQL Server AlwaysOn**: u kunt de failovers van beschikbaarheidsgroepen beheren met behulp van herstelplannen.
* **Automation-bibliotheek**: een uitgebreide Azure Automation-bibliotheek met toepassingsspecifieke scripts die klaar zijn voor gebruik, en kunnen worden gedownload en geïntegreerd met Site Recovery.
* **Eenvoudig netwerkbeheer**: dankzij geavanceerd netwerkbeheer in Site Recovery en Azure gelden er minder complexe vereisten voor het toepassingsnetwerk. Dit heeft onder andere invloed op het reserveren van IP-adressen, het configureren van load balancers en het integreren van Azure Traffic Manager voor het efficiënt schakelen tussen netwerken.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie vindt u in [What workloads can Site Recovery protect?](site-recovery-workload.md) (Welke workloads kan Site Recovery beveiligen?).
* Zie [Hoe werkt Site Recovery?](site-recovery-components.md) voor meer informatie over de Site Recovery-architectuur




<!---HONumber=Nov16_HO2-->


