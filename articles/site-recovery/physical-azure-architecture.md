---
title: Architectuur voor noodherstel van de fysieke server naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en architectuur die worden gebruikt tijdens het herstel na noodgevallen van on-premises fysieke servers naar Azure met de Azure Site Recovery-service.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 9476713bdca185fd84289fca3cf7aa304ad3f9fb
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311421"
---
# <a name="physical-server-to-azure-disaster-recovery-architecture"></a>Fysieke server naar Azure disaster recovery-architectuur

Dit artikel beschrijft de architectuur en processen die worden gebruikt bij het repliceren, failover en fysieke Windows- en Linux-servers tussen een on-premises site en Azure, herstellen met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die worden gebruikt voor fysieke servers repliceren naar Azure.  

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure storage-account en Azure-netwerk. | Gerepliceerde gegevens van on-premises VM's worden opgeslagen in de storage-account. Azure-VM's worden gemaakt met de gerepliceerde gegevens wanneer u een failover van on-premises naar Azure uitvoert. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratieserver** | Een enkele on-premises fysieke machine of VMware-VM is geïmplementeerd om uit te voeren alle van de on-premises Site Recovery-onderdelen. De virtuele machine wordt uitgevoerd voor de configuratieserver, processerver en hoofddoelserver. | De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
 **Processerver**:  | Standaard samen met de configuratieserver is geïnstalleerd. | Fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag.<br/><br/> De processerver installeert ook de Mobility-service op servers die u wilt repliceren.<br/><br/> Naarmate uw implementatie groeit, kunt u extra, afzonderlijk processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen.
 **Hoofddoelserver** | Standaard samen met de configuratieserver is geïnstalleerd. | Hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.<br/><br/> U kunt een afzonderlijke hoofddoelserver voor failback toevoegen voor grote implementaties.
**Gerepliceerde servers** | De Mobility-service is geïnstalleerd op elke server die u wilt repliceren. | U wordt aangeraden dat u automatische installatie van de processerver toestaan. U kunt ook de service handmatig installeren of gebruiken van een methode geautomatiseerde implementatie, zoals System Center Configuration Manager.

**Fysiek naar Azure-architectuur**

![Onderdelen](./media/physical-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replicatieproces

1. Instellen van de implementatie, met inbegrip van on-premises en Azure-onderdelen. In de Recovery Services-kluis geeft u de replicatiebron en het doel, instellen van de configuratieserver maken van beleid voor replicatie en replicatie inschakelen.
2. Machines repliceren in overeenstemming met het replicatiebeleid en een initiële kopie van de server-gegevens wordt gerepliceerd naar Azure storage.
3. Nadat de initiële replicatie is voltooid, begint de replicatie van deltaverschillen naar Azure. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
    - Machines communiceren met de configuratieserver op poort 443 voor HTTPS inkomende, voor het replicatiebeheer van.
    - Machines verzenden replicatiegegevens naar de processerver op HTTPS-poort 9443 Inkomend (kan worden gewijzigd).
    - De configuratieserver coördineert het replicatiebeheer met Azure via de uitgaande HTTPS-poort 443.
    - De processerver ontvangt gegevens van de bronmachines, optimaliseert en versleutelt deze gegevens en verzendt ze naar Azure Storage via de uitgaande poort 443.
    - Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Meerdere VM’s worden gebruikt als u meerdere machines groepeert in de replicatiegroepen die crashconsistent en app-consistent herstelpunten delen bij failover. Dit is handig als machines dezelfde workload gebruiken en consistent moeten zijn.
4. Verkeer wordt via internet gerepliceerd naar de openbare eindpunten van Azure Storage. U kunt ook [openbare peering](../expressroute/expressroute-circuit-peerings.md#publicpeering) van Azure ExpressRoute gebruiken. Het repliceren van verkeer via een site-naar-site-VPN van een on-premises site naar Azure wordt niet ondersteund.


**Fysiek naar Azure replicatieproces**

![Replicatieproces](./media/physical-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

Nadat replicatie is ingesteld en hebt u een Dr-herstelanalyse (testfailover) om te controleren of dat alles werkt zoals verwacht uitgevoerd, kunt u failover en failback uitvoeren als u wilt. Houd rekening met het volgende:

- Geplande failover wordt niet ondersteund.
- U moet een failback naar een on-premises virtuele VMware-machine. Dit betekent dat u een on-premises VMware-infrastructuur nodig hebt, zelfs wanneer u on-premises fysieke servers naar Azure repliceren.
- U een enkele machine, of het maken van plannen voor herstel, op meerdere machines samen een failover.
- Wanneer u een failover uitvoert, worden de Azure-VM's gemaakt van gerepliceerde gegevens in Azure storage.
- Na de initiële failover wordt geactiveerd, wijst u het toe om toegang tot de workload via de Azure-VM.
- Als uw primaire on-premises site weer beschikbaar is, kunt u een failback uitvoeren.
- U moet voor het instellen van een infrastructuur voor failback, met inbegrip van:
    - **Tijdelijke processerver in Azure**: Failback van Azure, voor het instellen van een Azure-VM om te fungeren als een processerver, voor het afhandelen van replicatie van Azure. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
    - **VPN-verbinding**: Als u wilt uitvoeren van een failback, moet u een VPN-verbinding (of Azure ExpressRoute) van het Azure-netwerk de on-premises site.
    - **Afzonderlijke hoofddoelserver**: De hoofddoelserver die is geïnstalleerd met de configuratieserver op de on-premises virtuele VMware-machine, wordt standaard de failback afgehandeld. Echter, als u back grote hoeveelheden verkeer mislukken wilt, u moet instellen een afzonderlijke on-premises hoofddoelserver voor dit doel.
    - **Failbackbeleid**: Als u wilt repliceren naar uw on-premises site, moet u een beleid voor failback. Dit is automatisch gemaakt tijdens het maken van uw beleid voor replicatie van on-premises naar Azure.
    - **VMware-infrastructuur**: U moet een VMware-infrastructuur voor failback. U kunt geen failback uitvoeren naar een fysieke server.
- Nadat de onderdelen in plaats daarvan failback vindt plaats in drie fasen zijn:
    - Fase 1: De Azure VM's opnieuw beveiligen zodat ze van Azure worden gerepliceerd naar de on-premises VMware-machines.
    - Fase 2: Een failover uitvoeren naar de on-premises site.
    - Fase 3: Nadat workloads failback, maar u opnieuw inschakelen replicatie.

**VMware-failback van Azure**

![Failback](./media/physical-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Volgende stappen

Ga als volgt [in deze zelfstudie](physical-azure-disaster-recovery.md) fysieke server naar Azure-replicatie inschakelen.
