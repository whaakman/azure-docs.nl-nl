---
title: Hoe werkt VMware-replicatie naar Azure in Azure Site Recovery? | Microsoft Docs
description: "Dit artikel biedt een overzicht van de onderdelen en architectuur die worden gebruikt bij het repliceren van on-premises VMware-VM’s en fysieke servers naar Azure met de Azure Site Recovery-service"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-architecture
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 81f02c1277ae8a2c377ca0d6db67ec4211e9aa5e
ms.contentlocale: nl-nl
ms.lasthandoff: 06/28/2017

---



<a id="how-does-vmware-replication-to-azure-work-in-site-recovery" class="xliff"></a>

# Hoe werkt VMware-replicatie naar Azure in Site Recovery?

In dit artikel worden de onderdelen en processen beschreven die betrokken zijn bij het repliceren van on-premises virtuele VMware-machines en fysieke Windows- of Linux-servers naar Azure met de [Azure Site Recovery](site-recovery-overview.md)-service.

Wanneer u fysieke on-premises servers naar Azure wilt repliceren, gebruikt u dezelfde onderdelen en processen als bij VMware VM-replicatie. Houd echter de volgende verschillen in gedachten:

- U kunt als configuratieserver een fysieke server gebruiken in plaats van een virtuele VMware-machine.
- U hebt een on-premises VMware-infrastructuur nodig voor failback. U kunt geen failback uitvoeren naar een fysieke computer.

Eventuele opmerkingen kunt u onderaan dit artikel plaatsen of vragen stellen op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


<a id="architectural-components" class="xliff"></a>

## Architectuuronderdelen

Er zijn een aantal onderdelen die zijn betrokken bij het repliceren van virtuele VMware-machines en fysieke servers naar Azure.

**Onderdeel** | **Locatie** | **Details**
--- | --- | ---
**Azure** | U hebt in Azure een Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig. | Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratieserver** | Eén lokale beheerserver (VMWare VM) die alle voor de implementatie benodigde on-premises onderdelen uitvoert, met inbegrip van de configuratieserver, processerver, hoofddoelserver | Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
 **Processerver**:  | standaard geïnstalleerd op de configuratieserver. | Fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag.<br/><br/> De processerver verwerkt ook de push-installatie van de Mobility-service naar beveiligde computers en voert automatische detectie van virtuele VMware-machines uit.<br/><br/> Naarmate uw implementatie groeit, kunt u extra, afzonderlijk toegewezen processervers toevoegen om het toenemende replicatieverkeer af te handelen.
 **Hoofddoelserver** | Standaard geïnstalleerd op de on-premises configuratieserver. | Hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.<br/><br/> Als de hoeveelheid failbackverkeer hoog is, kunt u een afzonderlijke hoofddoelserver voor failback implementeren.
**VMware-servers** | Virtuele VMware-machines worden gehost op vSphere ESXi-servers. Het wordt aangeraden om een vCenter-server te gebruiken om de hosts te beheren. | U voegt VMware-servers toe aan uw Recovery Services-kluis.
**Gerepliceerde machines** | De Mobility-service wordt geïnstalleerd op elke virtuele VMware-machine die u wilt repliceren. Deze kan handmatig worden geïnstalleerd op elke computer, of met een push-installatie van de processerver.

Meer informatie over de implementatievereisten en de vereisten voor elk van deze onderdelen vindt u in de [ondersteuningsmatrix](site-recovery-support-matrix-to-azure.md).

**Afbeelding 1: onderdelen van VMware naar Azure**

![Onderdelen](./media/site-recovery-components/arch-enhanced.png)

<a id="replication-process" class="xliff"></a>

## Replicatieproces

1. U stelt de implementatie, met inbegrip van Azure-onderdelen, en een Recovery Services-kluis in. In de kluis geeft u de replicatiebron en het replicatiedoel op, stelt u de configuratieserver in, voegt u VMware-servers toe, maakt u een replicatiebeleid, implementeert u de Mobility-service, schakelt u de replicatie in en voert u een failovertest uit.
2.  Machines beginnen met de replicatie volgens het replicatiebeleid. Een initiële kopie van de gegevens wordt gerepliceerd naar Azure Storage.
4. De replicatie van deltaverschillen naar Azure begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
    - Replicerende machines communiceren met de configuratieserver op inkomende HTTPS-poort 443 voor replicatie.
    - Replicerende machines verzenden replicatiegegevens naar de processerver op inkomende HTTPS-poort 9443 (kan worden geconfigureerd).
    - De configuratieserver coördineert het replicatiebeheer met Azure via de uitgaande HTTPS-poort 443.
    - De processerver ontvangt gegevens van de bronmachines, optimaliseert en versleutelt deze gegevens en verzendt ze naar Azure Storage via de uitgaande poort 443.
    - Als u consistentie tussen meerdere VM's inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Meerdere VM’s worden gebruikt als u meerdere machines groepeert in de replicatiegroepen die crashconsistent en app-consistent herstelpunten delen bij failover. Dit is handig als machines dezelfde workload gebruiken en consistent moeten zijn.
5. Verkeer wordt via internet gerepliceerd naar de openbare eindpunten van Azure Storage. U kunt ook [openbare peering](../expressroute/expressroute-circuit-peerings.md#public-peering) van Azure ExpressRoute gebruiken. Het repliceren van verkeer via een site-naar-site-VPN van een on-premises site naar Azure wordt niet ondersteund.

**Afbeelding 2: replicatie van VMware naar Azure**

![Verbeterd](./media/site-recovery-components/v2a-architecture-henry.png)

<a id="failover-and-failback-process" class="xliff"></a>

## Failover- en failbackproces

1. Wanneer u hebt gecontroleerd of de testfailover is geslaagd, kunt u naar behoefte niet-geplande failovers naar Azure uitvoeren. Geplande failover wordt niet ondersteund.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om failovers van meerdere virtuele machines uit te voeren.
3. Wanneer u een failover uitvoert, worden er replica-VM's gemaakt in Azure. U geeft een failover toegang tot de workload via de replica van de Azure-VM.
4. Als uw primaire on-premises site weer beschikbaar is, kunt u een failback uitvoeren. U stelt een infrastructuur voor failback in, start de replicatie van de machine vanaf de secundaire site naar de primaire site en voert een niet-geplande failover uit vanaf de secundaire site. Nadat u deze failover doorvoert, worden de gegevens teruggeplaatst op de on-premises site en moet u de replicatie naar Azure opnieuw inschakelen. [Meer informatie](site-recovery-failback-azure-to-vmware.md)

Er zijn enkele vereisten voor een failback:


- **Tijdelijke processerver in Azure**: als u na een failover een failback vanuit Azure wilt uitvoeren, moet u een virtuele Azure-machine hebben geconfigureerd als processerver, om de replicatie vanuit Azure af te handelen. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
- **VPN-verbinding**: voor de failback moet u een VPN-verbinding (of Azure ExpressRoute) hebben ingesteld van het Azure-netwerk naar de on-premises site.
- **Afzonderlijke on-premises hoofddoelserver**: op de on-premises hoofddoelserver wordt de failback afgehandeld. De hoofddoelserver wordt standaard op de beheerserver geïnstalleerd, maar als u een failback van grotere hoeveelheden verkeer uitvoert, kunt u voor dit doel beter een afzonderlijke on-premises hoofddoelserver instellen.
- **Failbackbeleid**: als u wilt repliceren naar de on-premises site, hebt u een failbackbeleid nodig. Dit wordt automatisch gemaakt wanneer u uw replicatiebeleid maakt.
- **VMware-infrastructuur**: u moet een failback uitvoeren naar een lokale virtuele VMware-machine. Dit betekent dat u een on-premises VMware-infrastructuur nodig hebt, zelfs als u fysieke on-premises servers naar Azure repliceert.

**Afbeelding 3: failback van VMware/fysieke failback**

![Failback](./media/site-recovery-components/enhanced-failback.png)


<a id="next-steps" class="xliff"></a>

## Volgende stappen

De [ondersteuningsmatrix](site-recovery-support-matrix-to-azure.md) controleren

