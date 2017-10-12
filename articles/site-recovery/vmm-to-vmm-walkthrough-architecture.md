---
title: Architectuur voor Hyper-V-replicatie naar een secundaire site met Azure Site Recovery bekijken | Microsoft Docs
description: "Dit artikel biedt een overzicht van de architectuur voor het repliceren van on-premises Hyper-V-VM’s naar een secundaire System Center VMM-site met Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b78cd0d5a5395873afaddc8856004775f447e8ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-a-secondary-site"></a>Stap 1: de architectuur voor Hyper-V-replicatie naar een secundaire site bekijken

In dit artikel worden de onderdelen en processen beschreven die zijn betrokken bij het repliceren van on-premises Hyper-V-VM's (virtuele machines) in System Center VMM-clouds (Virtual Machine Manager) naar een secundaire VMM-site met behulp van de [Azure Site Recovery](site-recovery-overview.md)-service in Azure Portal.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen plaatsen.



## <a name="architectural-components"></a>Architectuuronderdelen

Dit is wat u nodig hebt om Hyper V-VM's te repliceren naar een secundaire VMM-site.

**Onderdeel** | **Locatie** | **Details**
--- | --- | ---
**Azure** | Abonnement in Azure. | U maakt een Recovery Services-kluis in het Azure-abonnement om de replicatie tussen VMM-locaties te organiseren en te beheren.
**VMM-server** | U hebt een primaire en secundaire VMM-locatie nodig. | Het wordt aanbevolen om één VMM-beheerserver te gebruiken op de primaire site en één op de secundaire site 
**Hyper-V-server** |  U hebt een of meer Hyper-V-hostservers nodig die zich bevinden in de primaire en secundaire VMM-clouds. | Gegevens worden tussen de primaire en secundaire Hyper-V-hostservers via het LAN of VPN gerepliceerd met behulp van Kerberos of verificatie via certificaat.  
**Virtuele Hyper-V-machines** | Op Hyper-V-hostserver. | De bronhostserver moet ten minste één VM hebben die u wilt repliceren.

## <a name="replication-process"></a>Replicatieproces

1. U stelt het Azure-account in, maakt een Recovery Services-kluis en geeft op wat u wilt repliceren.
2. U configureert de instellingen van de bron- en doelreplicatie. Dit omvat het installeren van Azure Site Recovery Provider op VMM-servers en van de Microsoft Azure Recovery Services-agent op elke Hyper-V-host.
3. U maakt een replicatiebeleid voor de bron-VMM-cloud. Het beleid wordt toegepast op alle VM’s die zich op hosts in de cloud bevinden.
4. U schakelt replicatie in voor elke VMM. Er vindt een initiële replicatie van een VM plaats in overeenstemming met de instellingen die u kiest.
5. Na de initiële replicatie begint de replicatie van deltawijzigingen. Bijgehouden wijzigingen voor een item worden opgeslagen in een .hrl-bestand.


![On-premises naar on-premises](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande [failover](site-recovery-failover.md) uitvoeren tussen on-premises sites. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
4. Als u een niet-geplande failover naar een secundaire site uitvoert wanneer de failovermachines in de secundaire locatie niet zijn ingeschakeld voor beveiliging of replicatie. Als u een geplande failover hebt uitgevoerd, zijn de machines op de secundaire locatie na de failover beveiligd.
5. U geeft de failover vervolgens toegang tot de werkbelasting via de replica-VM.
6. Als uw primaire site weer beschikbaar is, start u de omgekeerde replicatie van de secundaire site naar de primaire site. Omgekeerde replicatie geeft de virtuele machines een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie.
7. Als u de primaire site weer de actieve locatie wilt maken, start u een geplande failover van de secundaire site naar de primaire site, gevolgd door nog een omgekeerde replicatie.



## <a name="next-steps"></a>Volgende stappen

Ga naar [Stap 2: de vereisten en beperkingen controleren](vmm-to-vmm-walkthrough-prerequisites.md).
