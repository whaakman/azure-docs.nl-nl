---
title: Netwerktoewijzing voor virtuele machine van Hyper-V-replicatie met Site Recovery plannen | Microsoft Docs
description: Stel netwerktoewijzing voor Hyper-V-replicatie voor virtuele machines van een on-premises datacenter naar Azure of naar een secundaire site.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: tysonn
ms.assetid: fcaa2f52-489d-4c1c-865f-9e78e000b351
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 91d6d0466789daa662162c60bc3c97ba6115e7eb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery"></a>Plan de netwerktoewijzing voor virtuele machine van Hyper-V-replicatie met Site Recovery



Dit artikel helpt u om te begrijpen en te plannen voor netwerk toewijzen tijdens de replicatie van Hyper-V-machines naar Azure of naar een secundaire site, met behulp van de [Azure Site Recovery-service](site-recovery-overview.md).

Na het lezen van dit artikel na de eventuele opmerkingen onder aan dit artikel of technische vragen over de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-to-azure"></a>Netwerktoewijzing voor replicatie naar Azure

Netwerktoewijzing wordt gebruikt wanneer u Hyper-V-machines (beheerd in VMM) repliceren naar Azure. Toewijzing toewijzingen tussen VM-netwerken op een bronserver met VMM-netwerk en Azure-netwerken zijn gericht. Toewijzing doet het volgende:

- **Netwerkverbinding**: zorgt ervoor dat de gerepliceerde virtuele machines van Azure met het toegewezen-netwerk verbonden bent. Alle machines die op hetzelfde netwerk failover kunnen verbinden met elkaar, zelfs als ze zich in verschillende herstelplannen failover.
- **Netwerkgateway**— als een netwerkgateway is ingesteld op de doel-Azure-netwerk, virtuele machines verbinding kunnen maken met andere on-premises virtuele machines.

Opmerking:

- U een bron-VMM VM-netwerk worden toegewezen aan een virtuele Azure-netwerk.
- Na een failover virtuele Azure-machines in de bron netwerk verbonden met het toegewezen virtuele netwerk.
- Nieuwe virtuele machines die zijn toegevoegd aan de bron-VM-netwerk zijn verbonden met het toegewezen Azure-netwerk wanneer er replicatie plaatsvindt.
- Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarin de virtuele bronmachine zich bevindt, wordt de gerepliceerde virtuele machine na een failover verbonden met dat doelsubnet.
- Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.


## <a name="network-mapping-for-replication-to-a-secondary-datacenter"></a>Netwerktoewijzing voor replicatie naar een secundair datacenter

Netwerktoewijzing wordt gebruikt bij het repliceren van Hyper-V-machines (beheerd in System Center Virtual Machine Manager (VMM)) naar een secundair datacenter. Koppelingen van Netwerktoewijzingen tussen VM-netwerken op een bronserver met VMM en VM-netwerken op een VMM-server. Toewijzing doet het volgende:

- **Netwerkverbinding**: virtuele machines verbinding maakt met een juiste netwerk na een failover. De replica-VM wordt verbonden met het netwerk dat toegewezen aan de bron-netwerk.
- **Optimale plaatsing**: optimaal plaatst de replica virtuele machines op Hyper-V-hostservers. Replica-VM's geplaatst op hosts die toegang heeft tot de toegewezen VM-netwerken.
- **Er is geen netwerktoewijzing**— als u geen netwerktoewijzing configureert, replica VMs niet verbonden met een VM-netwerken na een failover.

Opmerking:

- Netwerktoewijzing kan worden geconfigureerd tussen VM-netwerken op twee VMM-servers of op één VMM-server als de twee sites op dezelfde server worden beheerd.
- Wanneer toewijzing correct is geconfigureerd replicatie is ingeschakeld, wordt een virtuele machine op de primaire locatie worden verbonden met een netwerk en de replica op de doellocatie wordt verbonden met de toegewezen netwerkpad.
-
- Als netwerken hebt wanneer u een VM-netwerk van het doel bij netwerktoewijzing selecteert in VMM correct is ingesteld, wordt de VMM-bron-clouds die gebruikmaken van de bron-VM-netwerk weergegeven, samen met de beschikbare doelservers VM-netwerken op de doel-clouds die worden gebruikt voor beveiliging .
- Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten dezelfde naam heeft als het subnet waarop de virtuele bronmachine zich bevindt, klikt u vervolgens de replica virtuele machine verbonden met dat Doelsubnet na een failover. Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.



### <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld ter illustratie van dit mechanisme. U gaat nu een organisatie met twee locaties in New York en Chicago.

**Locatie** | **VMM-server** | **VM-netwerken** | **Toegewezen aan**
---|---|---|---
New York | VMM-NewYork| VMNetwork1 NewYork | Toegewezen aan VMNetwork1 Chicago
 |  | VMNetwork2 NewYork | Niet toegewezen
Chicago | VMM-Chicago| VMNetwork1 Chicago | Toegewezen aan VMNetwork1 NewYork
 | | VMNetwork1 Chicago | Niet toegewezen

In dit voorbeeld:

- Wanneer een replica virtuele machine is gemaakt voor elke virtuele machine die is verbonden met VMNetwork1 NewYork, wordt het verbonden zijn met VMNetwork1 Chicago.
- Wanneer een replica virtuele machine wordt gemaakt voor VMNetwork2 NewYork of VMNetwork2 Chicago, wordt deze niet verbonden met een netwerk.

Hier volgt hoe VMM-clouds worden ingesteld in ons voorbeeldorganisatie en de logische netwerken die zijn gekoppeld aan clouds.

#### <a name="cloud-protection-settings"></a>Instellingen voor cloudbeveiliging

**Beveiligde cloud** | **Beveiligen van de cloud** | **Logisch netwerk (Den Haag)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>N.v.t.</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>N.v.t.</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Logische schijf als VM-netwerkinstellingen

**Locatie** | **Logisch netwerk** | **Bijbehorende VM-netwerk**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

#### <a name="target-network-settings"></a>Doel-netwerkinstellingen

Op basis van deze instellingen wanneer u het doel VM-netwerk selecteert, ziet de volgende tabel u de opties die beschikbaar zijn.

**Selecteren** | **Beveiligde cloud** | **Beveiligen van de cloud** | **Doelnetwerk beschikbaar**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Niet beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar


Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten dezelfde naam heeft als het subnet waarop de virtuele bronmachine zich bevindt, klikt u vervolgens de replica virtuele machine verbonden met dat Doelsubnet na een failover. Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.


#### <a name="failback-behavior"></a>Failback gedrag

Als u wilt zien wat er gebeurt in het geval van een failback (omgekeerde replicatie), gaan we ervan uit dat VMNetwork1 NewYork is toegewezen aan VMNetwork1-Chicago, met de volgende instellingen.


**Virtuele machine** | **Verbonden met het VM-netwerk**
---|---
VM1 | VMNetwork1-netwerk
VM2 (replica van VM1) | VMNetwork1 Chicago

Met deze instellingen gaan we bekijken wat er gebeurt in een aantal mogelijke scenario's.

**Scenario** | **Resultaat**
---|---
Er is geen wijziging in de netwerkeigenschappen van VM-2 na een failover. | VM-1 blijft verbonden met het Bronnetwerk.
Netwerkeigenschappen van VM-2 worden gewijzigd na een failover en is verbroken. | VM-1 wordt verbroken.
Netwerkeigenschappen van VM-2 zijn gewijzigd na een failover en is verbonden met VMNetwork2 Chicago. | Als VMNetwork2 Chicago niet is toegewezen, kunt u VM-1 wordt verbroken.
Netwerktoewijzing van VMNetwork1 Chicago wordt gewijzigd. | VM-1 wordt verbonden met het netwerk is nu toegewezen aan VMNetwork1 Chicago.



## <a name="next-steps"></a>Volgende stappen

Meer informatie over [planning van de netwerkinfrastructuur](site-recovery-network-design.md).
