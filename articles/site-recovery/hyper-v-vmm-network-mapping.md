---
title: Over netwerktoewijzing voor replicatie van Hyper-V-VM (met VMM) naar Azure met Site Recovery | Microsoft Docs
description: Beschrijft hoe u voor het instellen van netwerktoewijzing voor replicatie van Hyper-V-machines worden beheerd in VMM-clouds, met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 35a2fe2373ccbf24c73cd63c096145da19dc6c4c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921252"
---
# <a name="prepare-network-mapping-for-hyper-v-vm-replication-to-azure"></a>Netwerktoewijzing voorbereiden voor replicatie van virtuele Hyper-V-machines naar Azure


Dit artikel helpt u om te begrijpen en voorbereiden op netwerktoewijzing wanneer u Hyper-V-machines in System Center Virtual Machine Manager (VMM)-clouds naar Azure of naar een secundaire site repliceren met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="prepare-network-mapping-for-replication-to-azure"></a>Netwerktoewijzing voorbereiden voor replicatie naar Azure

Als u repliceert naar Azure, koppelingen van Netwerktoewijzingen tussen VM-netwerken op een bron-VMM-server en virtuele netwerken van Azure als doel. Toewijzing doet het volgende:
    -  **Netwerkverbinding**, zorgt u ervoor dat de gerepliceerde Azure-VM's zijn verbonden met het toegewezen netwerk. Alle machines die failover wordt uitgevoerd in hetzelfde netwerk kunnen verbinding maken met elkaar worden verbonden, zelfs als ze in verschillende herstelplan een failover.
    - **Netwerkgateway**: als een netwerkgateway is ingesteld op de doel-Azure-netwerk, virtuele machines worden verbonden met andere on-premises virtuele machines.

Netwerktoewijzing werkt als volgt:

- U een bron-VMM VM-netwerk worden toegewezen aan een Azure-netwerk.
- Na een failover van virtuele Azure-machines in de bron netwerk verbonden met het toegewezen virtuele netwerk.
- Nieuwe virtuele machines die zijn toegevoegd aan de bron-VM-netwerk zijn verbonden met het toegewezen Azure-netwerk wanneer replicatie plaatsvindt.
- Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarin de virtuele bronmachine zich bevindt, wordt de gerepliceerde virtuele machine na een failover verbonden met dat doelsubnet.
- Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.

## <a name="prepare-network-mapping-for-replication-to-a-secondary-site"></a>Netwerktoewijzing voorbereiden voor replicatie naar een secundaire site

Als u naar een secundaire site repliceert, wijst de netwerktoewijzing tussen VM-netwerken op een bron-VMM-server en de VM-netwerken op een VMM-server. Toewijzing doet het volgende:

- **Netwerkverbinding**: VM's het juiste netwerk na een failover verbinding maakt. De replica-VM verbonden met het netwerk dat toegewezen aan het Bronnetwerk.
- **Optimale VM-plaatsing**: optimaal plaatst de replica virtuele machines op Hyper-V-hostservers. Replica-VM's worden geplaatst op hosts die toegang hebben tot de toegewezen VM-netwerken.
- **Er is geen netwerktoewijzing**— als u geen netwerktoewijzing configureert, replica-VM's niet verbonden met een VM-netwerken na een failover.

Netwerktoewijzing werkt als volgt:

- Netwerktoewijzing kan worden geconfigureerd tussen VM-netwerken op twee VMM-servers of op een enkele VMM-server als twee sites worden beheerd door dezelfde server.
- Wanneer toewijzing correct is geconfigureerd en replicatie is ingeschakeld, wordt een virtuele machine op de primaire locatie worden verbonden met een netwerk, en de replica op de doellocatie worden verbonden met het toegewezen netwerk.
- Wanneer u een doel-VM-netwerk tijdens netwerktoewijzing in Site Recovery selecteert, wordt de VMM-bron-clouds die gebruikmaken van de bron-VM-netwerk weergegeven, samen met de beschikbare doelservers VM-netwerken op de doel-clouds die worden gebruikt voor beveiliging.
- Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarop de virtuele bronmachine zich bevindt, wordt klikt u vervolgens de replica-VM verbonden met dat Doelsubnet na een failover. Als er geen Doelsubnet met een overeenkomende naam, de virtuele machine verbonden met het eerste subnet in het netwerk.

## <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld ter illustratie van dit mechanisme. We gaan een organisatie met twee locaties in New York en Chicago.

**Locatie** | **VMM-server** | **VM-netwerken** | **Toegewezen aan**
---|---|---|---
New York | VMM-NewYork| VMNetwork1 NewYork | Toegewezen aan VMNetwork1 Chicago
 |  | VMNetwork2 NewYork | Niet toegewezen
Chicago | VMM-Chicago| VMNetwork1-Chicago | Toegewezen aan VMNetwork1 NewYork
 | | VMNetwork2-Chicago | Niet toegewezen

In dit voorbeeld:

- Wanneer een replica die virtuele machine is gemaakt voor elke virtuele machine die verbonden met VMNetwork1 NewYork, wordt het verbonden zijn met VMNetwork1 Chicago.
- Wanneer een replica die virtuele machine is gemaakt voor VMNetwork2 NewYork of VMNetwork2 Chicago, kunt u deze niet verbonden met een netwerk.

Hier ziet hoe de VMM-clouds zijn ingesteld in ons van de voorbeeldorganisatie en de logische netwerken die zijn gekoppeld aan de clouds.

### <a name="cloud-protection-settings"></a>Instellingen voor cloud-beveiliging

**Beveiligde cloud** | **Cloud beveiligen** | **Logisch netwerk (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>N.v.t.</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>N.v.t.</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Instellingen voor logische en VM-netwerk

**Locatie** | **Logisch netwerk** | **Gekoppelde VM-netwerk**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

### <a name="target-network-settings"></a>Instellingen van het doelnetwerk

Op basis van deze instellingen wanneer u het doel-VM-netwerk selecteert, ziet de volgende tabel u de opties die beschikbaar zijn.

**Selecteren** | **Beveiligde cloud** | **Cloud beveiligen** | **Doelnetwerk beschikbaar**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Niet beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar


Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarop de virtuele bronmachine zich bevindt, klikt u vervolgens de replica virtuele machine verbonden met dat Doelsubnet na een failover. Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.


### <a name="failback-behavior"></a>Gedrag voor failback

Als u wilt zien wat er gebeurt in het geval van een failback (omgekeerde replicatie), gaan we ervan uit dat VMNetwork1 NewYork is toegewezen aan VMNetwork1-Chicago, met de volgende instellingen.


**VM** | **Verbonden met de VM-netwerk**
---|---
VM1 | VMNetwork1-netwerk
VM2 (replica van VM1) | VMNetwork1-Chicago

Met deze instellingen, laten we bekijken wat er gebeurt in een aantal mogelijke scenario's.

**Scenario** | **Resultaat**
---|---
Er is geen wijziging in de netwerkeigenschappen van de VM-2 na een failover. | VM-1, blijft gekoppeld aan het Bronnetwerk.
Netwerkeigenschappen van VM-2 zijn gewijzigd na een failover en niet is verbonden. | VM-1 wordt verbroken.
Netwerkeigenschappen van VM-2 worden gewijzigd na een failover en is verbonden met VMNetwork2 Chicago. | Als VMNetwork2 Chicago niet is toegewezen, kunt u VM-1 wordt verbroken.
Netwerktoewijzing van VMNetwork1 Chicago wordt gewijzigd. | VM-1 worden, verbonden met het netwerk dat is nu toegewezen aan VMNetwork1 Chicago.



## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over](hyper-v-vmm-networking.md) IP-adressering na een failover naar een secundaire VMM-site.
- [Meer informatie over](concepts-on-premises-to-azure-networking.md) IP-adressering na een failover naar Azure.
