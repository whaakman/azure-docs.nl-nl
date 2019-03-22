---
title: Architectuur voor Hyper-V-noodherstel naar een secundaire on-premises site met Azure Site Recovery | Microsoft Docs
description: In dit artikel biedt een overzicht van de architectuur voor herstel na noodgevallen van on-premises Hyper-V-machines naar een secundaire System Center VMM-site met Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/19/2019
ms.author: raynew
ms.openlocfilehash: 553ecefc9f7588de0ee9056f2b2385f9a0ede50f
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316164"
---
# <a name="architecture---hyper-v-replication-to-a-secondary-site"></a>Architectuur - Hyper-V-replicatie naar een secundaire site

In dit artikel worden de onderdelen en processen beschreven die zijn betrokken bij het repliceren van on-premises Hyper-V-VM's (virtuele machines) in System Center VMM-clouds (Virtual Machine Manager) naar een secundaire VMM-site met behulp van de [Azure Site Recovery](site-recovery-overview.md)-service in Azure Portal.


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die voor Hyper-V-replicatie naar een secundaire site.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Azure-abonnement | U maakt een Recovery Services-kluis in het Azure-abonnement om de replicatie tussen VMM-locaties te organiseren en te beheren.
**VMM-server** | U hebt een primaire en secundaire VMM-locatie nodig. | Aanbevolen is een VMM-beheerserver op de primaire site en een op de secundaire site.
**Hyper-V-server** |  U hebt een of meer Hyper-V-hostservers nodig die zich bevinden in de primaire en secundaire VMM-clouds. | Gegevens worden tussen de primaire en secundaire Hyper-V-hostservers via het LAN of VPN gerepliceerd met behulp van Kerberos of verificatie via certificaat.  
**Virtuele Hyper-V-machines** | Op Hyper-V-hostserver. | De bronhostserver moet ten minste één VM hebben die u wilt repliceren.

**On-premises naar on-premises-architectuur**

![On-premises naar on-premises](./media/hyper-v-vmm-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Replicatieproces

1. Wanneer de eerste replicatie wordt geactiveerd, een [Hyper-V-VM-momentopname](https://technet.microsoft.com/library/dd560637.aspx) momentopname wordt gemaakt.
2. Virtuele harde schijven op de virtuele machine worden één voor één gerepliceerd, naar de secundaire locatie.
3. Als er tijdens de initiële replicatie wordt uitgevoerd, worden de wijzigingen in de Hyper-V Replica Replication Tracker bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze logboekbestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een bijbehorend .hrl-bestand dat wordt verzonden naar de secundaire locatie. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
4. Wanneer de initiële replicatie is voltooid, de VM-momentopname is verwijderd en begint de replicatie van verschillen.
5. Verschillen in het logboek worden gesynchroniseerd en samengevoegd op de bovenliggende schijf.


## <a name="failover-and-failback-process"></a>Failover- en failbackproces

- U kunt een enkele machine failover of plannen voor herstel, voor het indelen van failover van meerdere virtuele machines maken.
- U kunt een geplande of niet-geplande failover uitvoeren tussen on-premises sites. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
    - Als u een niet-geplande failover naar een secundaire site uitvoeren nadat de failovermachines in de secundaire locatie zijn niet beveiligd.
    - Als u een geplande failover hebt uitgevoerd, zijn de machines op de secundaire locatie na de failover beveiligd.
- Nadat de initiële failover wordt uitgevoerd, wijst u het toe, toegang tot de workload van de replica-VM.
- Als de primaire locatie weer beschikbaar is, kunt u failover terug.
    - U starten omgekeerde replicatie, om te beginnen met het repliceren van de secundaire site naar de primaire. Omgekeerde replicatie geeft de virtuele machines een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie.
    - Als u de primaire site weer de actieve locatie wilt maken, start u een geplande failover van de secundaire site naar de primaire site, gevolgd door nog een omgekeerde replicatie.



## <a name="next-steps"></a>Volgende stappen


Ga als volgt [in deze zelfstudie](hyper-v-vmm-disaster-recovery.md) Hyper-V-replicatie tussen VMM-clouds in te schakelen.
