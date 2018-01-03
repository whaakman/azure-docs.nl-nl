---
title: VMware naar Azure replicatie Azure Site Recovery-architectuur | Microsoft Docs
description: In dit artikel biedt een overzicht van de onderdelen en de architectuur die wordt gebruikt voor het lokale virtuele VMware-machines repliceren naar Azure met de Azure Site Recovery-service
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/19/2017
ms.author: raynew
ms.openlocfilehash: 1c991298d8f59c7f161b965541571b4c8ac3d8f9
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware naar Azure replicatie-architectuur

In dit artikel beschrijft de architectuur en processen die worden gebruikt wanneer u repliceren, failover en herstellen van virtuele VMware-machines (VM's) tussen een on-premises VMware-site en Azure, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die worden gebruikt voor VMware-replicatie naar Azure.  

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure storage-account en Azure-netwerk. | Gerepliceerde gegevens van de lokale virtuele machines wordt opgeslagen in het opslagaccount. Virtuele machines in Azure worden gemaakt met de gerepliceerde gegevens wanneer u een mislukken via on-premises naar Azure uitvoert. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratieserver** | Een enkel lokale VMware VM is geïmplementeerd als u wilt uitvoeren van alle van de on-premises onderdelen van Site Recovery. De virtuele machine wordt uitgevoerd voor de configuratieserver, de processerver en de hoofddoelserver. | De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
 **Processerver**:  | Samen met de configuratieserver standaard geïnstalleerd. | Fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag.<br/><br/> De Mobility-service de processerver ook geïnstalleerd op virtuele machines die u repliceren wilt, en voert u automatische detectie van virtuele machines op de on-premises VMware-servers.<br/><br/> Naarmate uw implementatie groeit, kunt u het voor afzonderlijke processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen.
 **Hoofddoelserver** | Samen met de configuratieserver standaard geïnstalleerd. | Hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.<br/><br/> U kunt een afzonderlijke hoofddoelserver voor failback toevoegen voor grote implementaties.
**VMware-servers** | Virtuele VMware-machines worden gehost op on-premises vSphere ESXi-servers. U wordt aangeraden een vCenter-server voor het beheren van de hosts. | Tijdens de implementatie van Site Recovery kunt u VMware-servers toevoegen aan de Recovery Services-kluis.
**Gerepliceerde machines** | De Mobility-service is geïnstalleerd op elke VMware VM die u wilt repliceren. | U wordt aangeraden toestaan van automatische installatie van de processerver. U kunt ook handmatig installeren van de service of gebruik de methode geautomatiseerde implementatie zoals System Center Configuration Manager. 

**VMware naar Azure-architectuur**

![Onderdelen](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replicatieproces

1. U instellen kunt de implementatie, met inbegrip van on-premises en Azure onderdelen. In de Recovery Services-kluis geeft u de replicatiebron en doel, instellen van de configuratieserver maken van een beleid voor wachtwoordreplicatie en replicatie inschakelen.
2. Machines repliceren in overeenstemming met het replicatiebeleid en een initiële kopie van de VM-gegevens worden gerepliceerd naar Azure-opslag.
3. Nadat de initiële replicatie is voltooid, begint de replicatie van wijzigingen naar Azure. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
    - Machines communiceren met de configuratieserver op poort 443 voor HTTPS binnenkomend verkeer voor replicatiebeheer.
    - Machines replicatiegegevens verzenden naar de processerver op poort HTTPS 9443 binnenkomende (kan worden aangepast).
    - De configuratieserver coördineert het replicatiebeheer met Azure via de uitgaande HTTPS-poort 443.
    - De processerver ontvangt gegevens van de bronmachines, optimaliseert en versleutelt deze gegevens en verzendt ze naar Azure Storage via de uitgaande poort 443.
    - Als u de consistentie tussen meerdere VM's inschakelt, wordt de machines in de replicatiegroep met elkaar communiceren via poort 20004. Meerdere VM’s worden gebruikt als u meerdere machines groepeert in de replicatiegroepen die crashconsistent en app-consistent herstelpunten delen bij failover. Dit is handig als machines dezelfde workload gebruiken en consistent moeten zijn.
4. Verkeer wordt via internet gerepliceerd naar de openbare eindpunten van Azure Storage. U kunt ook [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) van Azure ExpressRoute gebruiken. Het repliceren van verkeer via een site-naar-site-VPN van een on-premises site naar Azure wordt niet ondersteund.


**VMware naar Azure replicatieproces**

![Replicatieproces](./media/concepts-vmware-to-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

Nadat de replicatie is ingesteld en u hebt een disaster recovery Inzoomen (testfailover) om te controleren of dat alles werkt zoals verwacht uitgevoerd, kunt u een failover en failback uitvoeren als nodig is voor.

1. U kunt één machine failover of herstelplannen failover meerdere virtuele machines maken.
2. Wanneer u een failover uitvoert, worden virtuele Azure-machines in Azure-opslag van gerepliceerde gegevens gemaakt.
3. Na activering van de eerste failover doorvoeren u om te starten met het openen van de werkbelasting van de Azure VM.

Als uw primaire on-premises site weer beschikbaar is, kunt u een failback uitvoeren.
1. U moet voor het instellen van een failback-infrastructuur, met inbegrip van:
    - **Tijdelijke processerver in Azure**: als u wilt een failback van Azure, instellen van een virtuele machine van Azure om te fungeren als een processerver voor het afhandelen van replicatie van Azure. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
    - **VPN-verbinding**: als u wilt een failback uit, moet u een VPN-verbinding (of Azure ExpressRoute) van het netwerk van Azure naar de lokale site.
    - **Afzonderlijke hoofddoelserver**: de hoofddoelserver die is geïnstalleerd met de configuratieserver op de lokale VMware virtuele machine, wordt standaard de failback afgehandeld. Echter, als u back grote hoeveelheden verkeer mislukken wilt, moet u instellen een afzonderlijke on-premises hoofddoelserver voor dit doel.
    - **Failbackbeleid**: als u wilt repliceren naar de on-premises site, hebt u een failbackbeleid nodig. Dit is automatisch gemaakt tijdens het maken van uw replicatiebeleid voor van on-premises naar Azure.
2. Nadat de onderdelen aanwezig is zijn, de failback in drie fasen:
    - Stap 1: De Azure VM's opnieuw beveiligen zodat ze vanaf Azure repliceren terug naar de lokale VMware-machines.
    - Stap 2: Een failover naar de lokale site uitvoert.
    - Stap 3: Nadat werkbelastingen terug zijn uitgevallen, u weer inschakelen repliceren voor de lokale virtuele machines.

**VMware failback vanuit Azure**

![Failback](./media/concepts-vmware-to-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Volgende stappen

Ga als volgt [in deze zelfstudie](tutorial-vmware-to-azure.md) VMware naar Azure replicatie inschakelen.
