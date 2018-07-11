---
title: VMware naar Azure-replicatie-architectuur in Azure Site Recovery | Microsoft Docs
description: In dit artikel biedt een overzicht van de onderdelen en architectuur die worden gebruikt bij het repliceren van on-premises VMware-machines naar Azure met Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 48adf61dc0f1796b820e1e14ca509d4618c6256b
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920563"
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware naar Azure-replicatie-architectuur

Dit artikel beschrijft de architectuur en processen die worden gebruikt bij het repliceren, failover en herstel van virtuele VMware-machines (VM's) tussen een on-premises VMware-site en Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die worden gebruikt voor replicatie van VMware naar Azure.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure Storage-account en Azure-netwerk. | Gerepliceerde gegevens van on-premises VM's worden opgeslagen in de storage-account. Azure VM's worden gemaakt met de gerepliceerde gegevens wanneer u een failover van on-premises naar Azure uitvoert. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratie van server-machine** | Een enkele on-premises computer. Het is raadzaam dat u uitvoert als een VMware-VM die kan worden geïmplementeerd vanaf een gedownloade OVF-sjabloon.<br/><br/> De machine wordt uitgevoerd van alle on-premises Site Recovery-onderdelen, waaronder de configuratieserver, processerver en hoofddoelserver. | **Configuratieserver**: coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.<br/><br/> **Processerver**: standaard geïnstalleerd op de configuratieserver. Deze ontvangt replicatiegegevens; Met caching, compressie en versleuteling, optimaliseert en verzendt dit naar Azure Storage. De processerver installeert ook Azure Site Recovery Mobility Service op virtuele machines die u repliceren wilt, en wordt automatische detectie van on-premises machines uitgevoerd. Naarmate uw implementatie groeit, kunt u extra, afzonderlijk processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen.<br/><br/> **Hoofddoelserver**: standaard geïnstalleerd op de configuratieserver. Replicatiegegevens worden verwerkt tijdens de failback vanuit Azure. U kunt een afzonderlijke hoofddoelserver voor failback toevoegen voor grote implementaties.
**VMware-servers** | Virtuele VMware-machines worden gehost op on-premises vSphere ESXi-servers. U wordt aangeraden een vCenter-server voor het beheren van de hosts. | Tijdens de implementatie van Site Recovery kunt u VMware-servers toevoegen aan de Recovery Services-kluis.
**Gerepliceerde machines** | Mobility-Service is geïnstalleerd op elke VMware-VM die u wilt repliceren. | Het is raadzaam dat u de automatische installatie van de processerver toestaan. U kunt ook de service handmatig installeren of gebruiken van een methode geautomatiseerde implementatie, zoals System Center Configuration Manager.

**VMware naar Azure-architectuur**

![Onderdelen](./media/vmware-azure-architecture/arch-enhanced.png)

## <a name="configuration-steps"></a>Configuratiestappen

De stappen voor het instellen van VMware naar Azure-noodherstel of de migratie zijn als volgt:

1. **Instellen van Azure-onderdelen**. U moet een Azure-account met de juiste machtigingen, Azure storage-account, een Azure-netwerk en een Recovery Services-kluis. [Meer informatie](tutorial-prepare-azure.md).
2. **Instellen van on-premises**. Deze omvatten het instellen van een account op de VMware-server zodat Site Recovery automatisch van virtuele machines repliceren detecteren kan, instellen van een account dat kan worden gebruikt voor het installeren van het onderdeel van de Mobility-service op virtuele machines die u wilt repliceren en verifiëren dat VMware-servers en virtuele machines te voldoen aan de vereisten. U kunt eventueel ook verbinding maken met deze Azure-VM's na failover voorbereiden. Site Recovery worden VM-gegevens gerepliceerd naar Azure storage-account en maakt u virtuele Azure-machines met behulp van de gegevens wanneer u een failover naar Azure uitvoert. [Meer informatie](vmware-azure-tutorial-prepare-on-premises.md).
3. **Instellen van replicatie**. U kiezen waar u naar wilt repliceren. U configureert de bronreplicatieomgeving door het instellen van een één on-premises virtuele VMware-machine (configuratieserver) met alle van de on-premises Site Recovery-onderdelen die u nodig hebt. Na de installatie kunt u de configuratie van server-machine registreren in de Recovery Services-kluis. Selecteer vervolgens de doelinstellingen. [Meer informatie](vmware-azure-tutorial.md).
4. **Maakt een replicatiebeleid**. U maakt een replicatiebeleid waarmee wordt aangegeven hoe replicatie moet gebeuren. 
    - **RPO-drempelwaarde**: deze bewakingsinstelling statussen als replicatie niet binnen de opgegeven tijd wordt uitgevoerd, een waarschuwing (en eventueel een e-mailbericht) is uitgegeven. Bijvoorbeeld, als u de RPO-drempelwaarde op 30 minuten instellen en een probleem wordt voorkomen replicatie gebeurt gedurende 30 minuten dat, wordt een gebeurtenis gegenereerd. Deze instelling heeft geen invloed op de replicatie. Replicatie is continue en herstelpunten worden gemaakt om de paar minuten
    - **Retentie**: herstelpunt bewaarperiode geeft aan hoe lang herstelpunten moet worden opgeslagen in Azure. Geef een waarde tussen 0 en 24 uur voor premium-opslag of omhoog tot 72 uur voor standard-opslag. U kunt failover uitvoeren naar de meest recente herstelpunt, of naar een opgeslagen als u de waarde die hoger is dan nul instelt. Na de bewaarperiode worden herstelpunten opgeschoond.
    - **Crash-consistente momentopnamen**: standaard, Site Recovery neemt crash-consistente momentopnamen en maakt herstelpunten met hen om de paar minuten. Een herstelpunt is een crash-consistent als alle onderdelen met elkaar verbonden gegevens schrijven-volgorde consistent zijn, zoals ze waren op het moment het herstelpunt is gemaakt. Om beter te begrijpen, stelt u de status van de gegevens op uw PC harde schijf zich na een stroomstoring of een soortgelijke gebeurtenis. Een crash-consistente herstelpunt is meestal voldoende als uw toepassing is ontworpen om te herstellen van een crash zonder gegevensinconsistenties.
    - **App-consistente momentopnamen**: als deze waarde niet nul is, wordt de Mobility-service die wordt uitgevoerd op de virtuele machine probeert om bestand system-consistente momentopnamen en herstelpunten te genereren. De eerste momentopname wordt gemaakt nadat de initiële replicatie is voltooid. Vervolgens worden momentopnamen gemaakt met de frequentie die u opgeeft. Een herstelpunt is toepassingsconsistente als, naast de order-write-consistente, actieve toepassingen al hun bewerkingen uitvoeren, en hun buffers naar schijf (toepassing stilleggen) leegmaken. App-consistente herstelpunten worden aanbevolen voor databasetoepassingen zoals SQL, Oracle en Exchange. Als een crash-consistente momentopname voldoende is, kunt u deze waarde ingesteld op 0.  
    - **Multi-VM-consistentie**: U kunt eventueel ook een replicatiegroep maken. Vervolgens, wanneer u replicatie inschakelt, u virtuele machines in die groep verzamelen kunt. Virtuele machines in een-replicatie repliceren groeperen en gedeelde crash-consistente en app-consistente herstelpunten bij een failover is uitgevoerd. Moet u deze optie zorgvuldig, omdat deze kan werkbelasting beïnvloeden als momentopnamen die nodig is tussen meerdere computers worden verzameld. Dit alleen doen als virtuele machines dezelfde workload en consistent moeten worden uitgevoerd en virtuele machines hebben vergelijkbare bussen. U kunt maximaal 8 virtuele machines toevoegen aan een groep. 
5. **Inschakelen van replicatie van virtuele machines**. Ten slotte, schakelt u replicatie voor uw on-premises VMware-machines. Als u een account voor het installeren van de Mobility-service hebt gemaakt en opgegeven dat Site Recovery een push-installatie moet doen, wordt de Mobility-service worden geïnstalleerd op elke virtuele machine waarvoor u replicatie inschakelt. [Meer informatie](vmware-azure-tutorial.md#enable-replication). Als u een replicatiegroep voor multi-VM-consistentie hebt gemaakt, kunt u virtuele machines toevoegen aan die groep.
6. **Testfailover**. Nadat alles ingesteld, kunt u een testfailover uitvoeren om te controleren dat virtuele machines een failover naar Azure zoals verwacht. [Meer informatie](tutorial-dr-drill-azure.md).
7. **Failover**. Als u de virtuele machines alleen naar Azure migreren bent - kunt u een volledige failover om dit te doen uitvoeren. Als u herstel na noodgevallen instelt, kunt u een volledige failover uitvoeren als u wilt. Voor volledige noodherstel, kunt na een failover naar Azure, u failover terug naar uw on-premises site als en wanneer deze beschikbaar is. [Meer informatie](vmware-azure-tutorial-failover-failback.md).

## <a name="replication-process"></a>Replicatieproces

1. Wanneer u replicatie voor een virtuele machine inschakelt, begint deze te repliceren in overeenstemming met het replicatiebeleid. 
2. Verkeer repliceert naar Azure storage-openbare eindpunten via internet. U kunt ook kunt u Azure ExpressRoute met [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Het repliceren van verkeer via een site-naar-site virtueel particulier netwerk (VPN) van een on-premises site naar Azure wordt niet ondersteund.
3. Een initiële kopie van de VM-gegevens wordt gerepliceerd naar Azure storage.
4. Nadat de initiële replicatie is voltooid, begint de replicatie van deltaverschillen naar Azure. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
5. Communicatie gebeurt er als volgt uit:

    - Virtuele machines communiceren met de on-premises configuratieserver op poort 443 voor HTTPS inkomende, voor het replicatiebeheer van.
    - De configuratieserver coördineert de replicatie met Azure via HTTPS-poort 443 uitgaande.
    - Virtuele machines verzenden replicatiegegevens naar de processerver (die wordt uitgevoerd op de server configuration) op HTTPS-poort 9443 inkomende. Deze poort kan worden gewijzigd.
    - De processerver ontvangt replicatiegegevens, optimaliseert en versleutelt deze en verzendt ze naar Azure storage via poort 443 uitgaande.


**VMware naar Azure replicatieproces**

![Replicatieproces](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

Nadat replicatie is ingesteld en u een Dr-herstelanalyse (testfailover) om te controleren of alles werkt zoals verwacht worden uitgevoerd, kunt u failover en failback uitvoeren als u wilt.

1. U mislukt voor één machine uitvoeren of plannen om meerdere virtuele machines op hetzelfde moment een failover van een herstelpunt maken. Het voordeel van een plan voor herstel in plaats van één machine failover zijn onder andere:
    - U kunt app-afhankelijkheden model door alle virtuele machines in de app in één herstelplan.
    - U kunt scripts, Azure-runbooks, toevoegen en onderbreken voor de handmatige acties.
2. Na de initiële failover wordt geactiveerd, wijst u het toe om toegang tot de workload via de Azure-VM.
3. Als uw primaire on-premises site weer beschikbaar is, kunt u voorbereiden voor failback. Als u wilt uitvoeren van een failback, moet u voor het instellen van een infrastructuur voor failback, met inbegrip van:

    * **Tijdelijke processerver in Azure**: failback van Azure, voor het instellen van een Azure-VM om te fungeren als een processerver voor het afhandelen van replicatie van Azure. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
    * **VPN-verbinding**: als u wilt uitvoeren van een failback, moet u een VPN-verbinding (of ExpressRoute) van het Azure-netwerk de on-premises site.
    * **Afzonderlijke hoofddoelserver**: standaard, de hoofddoelserver die is geïnstalleerd met de configuratieserver op de on-premises virtuele VMware-machine wordt de failback afgehandeld. Als u back grote hoeveelheden verkeer mislukt wilt, kunt u instellen van een afzonderlijke on-premises hoofddoelserver voor dit doel.
    * **Failbackbeleid**: als u wilt repliceren naar de on-premises site, hebt u een failbackbeleid nodig. Dit beleid is automatisch gemaakt tijdens het maken van uw beleid voor replicatie van on-premises naar Azure.
4. Nadat de onderdelen in plaats daarvan de failback in drie acties zijn:

    - Fase 1: De Azure VM's opnieuw beveiligen zodat ze van Azure worden gerepliceerd naar de on-premises VMware-machines.
    -  Fase 2: Een failover naar de on-premises site uitvoert.
    - Fase 3: Nadat workloads failback, u weer inschakelen replicatie voor de on-premises VM's.
    
 
**VMware-failback van Azure**

![Failback](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Volgende stappen

Ga als volgt [in deze zelfstudie](vmware-azure-tutorial.md) om in te schakelen van VMware naar Azure-replicatie.
