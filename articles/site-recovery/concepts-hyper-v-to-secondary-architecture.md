---
title: Architectuur voor Hyper-V-replicatie naar een secundaire site met Azure Site Recovery bekijken | Microsoft Docs
description: "Dit artikel biedt een overzicht van de architectuur voor het repliceren van on-premises Hyper-V-VM’s naar een secundaire System Center VMM-site met Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 26475782-a21a-408a-b089-35382d7e010e
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: a7a493097a4eaacc2c8d8449906b4a57eb411827
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

1. U kunt één machine failover of herstelplannen, indelen failover van meerdere machines maken.
2. U kunt een geplande of niet-geplande failover tussen on-premises sites uitvoeren. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
    - Als u een niet-geplande failover naar een secundaire site uitvoeren nadat de failover-machines in de secundaire locatie zijn niet beveiligd.
    - Als u een geplande failover hebt uitgevoerd, zijn de machines op de secundaire locatie na de failover beveiligd.
3. Nadat de initiële failover wordt uitgevoerd, wijst u het toe, voor het starten van de toegang tot de werkbelasting van de replica-VM.

Wanneer de primaire locatie weer beschikbaar is, kunt u failback uit.

1. U initiëren omgekeerde replicatie voor het starten van de secundaire site repliceren naar de primaire. Omgekeerde replicatie geeft de virtuele machines een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie.
2. Als u de primaire site weer de actieve locatie wilt maken, start u een geplande failover van de secundaire site naar de primaire site, gevolgd door nog een omgekeerde replicatie.



## <a name="next-steps"></a>Volgende stappen

Raadpleeg de ondersteuningsmatrix Volg de zelfstudie Hyper-V-replicatie tussen de VMM-clouds in te schakelen.
Voer een failover en failback.
