---
title: Hyper-V-replicatie met de architectuur van de secundaire site in de Azure Site Recovery | Microsoft Docs
description: "Dit artikel biedt een overzicht van de architectuur voor het repliceren van on-premises Hyper-V-VM’s naar een secundaire System Center VMM-site met Azure Site Recovery."
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/19/2017
ms.author: raynew
ms.openlocfilehash: 3380d189518f811ca6cf628608a253e5d93b2730
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="hyper-v-replication-to-a-secondary-site"></a>Hyper-V-replicatie naar een secundaire site

In dit artikel worden de onderdelen en processen beschreven die zijn betrokken bij het repliceren van on-premises Hyper-V-VM's (virtuele machines) in System Center VMM-clouds (Virtual Machine Manager) naar een secundaire VMM-site met behulp van de [Azure Site Recovery](site-recovery-overview.md)-service in Azure Portal.


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen voor Hyper-V-replicatie naar een secundaire site gebruikt.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Azure-abonnement | U maakt een Recovery Services-kluis in het Azure-abonnement om de replicatie tussen VMM-locaties te organiseren en te beheren.
**VMM-server** | U hebt een primaire en secundaire VMM-locatie nodig. | Aanbevolen is een VMM-beheerserver op de primaire site en een op de secundaire site.
**Hyper-V-server** |  U hebt een of meer Hyper-V-hostservers nodig die zich bevinden in de primaire en secundaire VMM-clouds. | Gegevens worden tussen de primaire en secundaire Hyper-V-hostservers via het LAN of VPN gerepliceerd met behulp van Kerberos of verificatie via certificaat.  
**Virtuele Hyper-V-machines** | Op Hyper-V-hostserver. | De bronhostserver moet ten minste één VM hebben die u wilt repliceren.

**On-premises naar lokale-architectuur**

![On-premises naar on-premises](./media/concepts-hyper-v-to-secondary-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Replicatieproces

1. Wanneer de eerste replicatie wordt geactiveerd, een [Hyper-V VM-momentopname](https://technet.microsoft.com/library/dd560637.aspx) momentopname wordt gemaakt.
2. Virtuele harde schijven op de virtuele machine zijn een voor een gerepliceerd, naar de secundaire locatie.
3. Als er schijfwijzigingen optreden terwijl de eerste replicatie uitgevoerd wordt, 
4. Als de initiële replicatie is voltooid, begint de replicatie van verschillen. De wijzigingen de Hyper-V Replica Replication Tracker bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze logboekbestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een hrl-bestand dat naar de secundaire locatie wordt verzonden. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
5. Verschillen in het logboek worden gesynchroniseerd en samengevoegd op de bovenliggende schijf.
6. 

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

- U kunt één machine failover of herstelplannen, indelen failover van meerdere machines maken.
- U kunt een geplande of niet-geplande failover tussen on-premises sites uitvoeren. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
    - Als u een niet-geplande failover naar een secundaire site uitvoeren nadat de failover-machines in de secundaire locatie zijn niet beveiligd.
    - Als u een geplande failover hebt uitgevoerd, zijn de machines op de secundaire locatie na de failover beveiligd.
- Nadat de initiële failover wordt uitgevoerd, wijst u het toe, voor het starten van de toegang tot de werkbelasting van de replica-VM.
- Wanneer de primaire locatie weer beschikbaar is, kunt u failback uit.
    - U initiëren omgekeerde replicatie voor het starten van de secundaire site repliceren naar de primaire. Omgekeerde replicatie geeft de virtuele machines een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie.
    - Als u de primaire site weer de actieve locatie wilt maken, start u een geplande failover van de secundaire site naar de primaire site, gevolgd door nog een omgekeerde replicatie.



## <a name="next-steps"></a>Volgende stappen


Ga als volgt [in deze zelfstudie](tutorial-vmm-to-vmm.md) Hyper-V-replicatie tussen de VMM-clouds in te schakelen.
