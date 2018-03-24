---
title: VMware naar Azure replicatie Azure Site Recovery-architectuur | Microsoft Docs
description: In dit artikel biedt een overzicht van de onderdelen en gebruikt wanneer de lokale virtuele VMware-machines repliceren naar Azure met Azure Site Recovery-architectuur
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/19/2018
ms.author: raynew
ms.openlocfilehash: c1aa89f14edab7d0e560c20d6bc48480aff1631f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware naar Azure replicatie-architectuur

In dit artikel beschrijft de architectuur en processen die worden gebruikt wanneer u replicatie, failover en herstellen van virtuele VMware-machines (VM's) tussen een on-premises VMware-site en Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die worden gebruikt voor VMware-replicatie naar Azure.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure Storage-account en Azure-netwerk. | Gerepliceerde gegevens van de lokale virtuele machines wordt opgeslagen in het opslagaccount. Virtuele machines in Azure worden gemaakt met de gerepliceerde gegevens wanneer u een failover van on-premises naar Azure uitvoert. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratie-servercomputer** | Een één on-premises machine. Het is raadzaam dat u uitvoert als een VMware-VM die kan worden geïmplementeerd vanaf een gedownloade OVF-sjabloon.<br/><br/> De machine wordt uitgevoerd voor alle on-premises Site Recovery onderdelen, waaronder de configuratieserver, de processerver en de hoofddoelserver. | **Configuratieserver**: coördineert de communicatie tussen de on-premises en Azure en beheert gegevensreplicatie.<br/><br/> **Processerver**: standaard geïnstalleerd op de configuratieserver. Het ontvangt replicatiegegevens; Deze optimaliseert met caching, compressie en codering; en verzendt het naar Azure Storage. Azure Site Recovery Mobility-Service de processerver ook geïnstalleerd op virtuele machines die u repliceren wilt, en de werking van automatische detectie van lokale computers. Naarmate uw implementatie groeit, kunt u het voor afzonderlijke processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen.<br/><br/> **Hoofddoelserver**: standaard geïnstalleerd op de configuratieserver. Replicatiegegevens verwerkt tijdens de failback vanuit Azure. U kunt een afzonderlijke hoofddoelserver voor failback toevoegen voor grote implementaties.
**VMware-servers** | Virtuele VMware-machines worden gehost op on-premises vSphere ESXi-servers. U wordt aangeraden een vCenter-server voor het beheren van de hosts. | Tijdens de implementatie van Site Recovery kunt u VMware-servers toevoegen aan de Recovery Services-kluis.
**Gerepliceerde machines** | Mobility-Service is geïnstalleerd op elke VMware VM die u wilt repliceren. | We raden u aan automatische installatie van de processerver. U kunt ook de service handmatig te installeren of gebruiken van een methode geautomatiseerde implementatie, zoals System Center Configuration Manager.

**VMware naar Azure-architectuur**

![Onderdelen](./media/vmware-azure-architecture/arch-enhanced.png)

## <a name="configuration-steps"></a>Configuratiestappen

De stappen voor het instellen van VMware naar Azure noodherstel of de migratie zijn als volgt:

1. **Instellen van Azure onderdelen**. U moet een Azure-account met de juiste machtigingen, een Azure storage-account, een Azure-netwerk en een Recovery Services-kluis. [Meer informatie](tutorial-prepare-azure.md).
2. **Instellen van on-premises**. Het gaat hierbij instellen van een account op de VMware-server zodat de Site Recovery kan automatisch detecteren voor virtuele machines worden gerepliceerd, instellen van een account dat kan worden gebruikt voor het installeren van het onderdeel van de Mobility-service op virtuele machines die u wilt repliceren en verifiëren die VMware-servers en virtuele machines vereisten voldoen. U kunt eventueel ook verbinding maken met deze virtuele Azure-machines na een failover voorbereiden. Site Recovery VM-gegevens worden gerepliceerd naar Azure storage-account en maakt Azure virtuele machines met behulp van de gegevens wanneer u een failover naar Azure uitvoert. [Meer informatie](vmware-azure-tutorial-prepare-on-premises.md).
3. **De replicatie instellen**. Kies van waar u naar wilt repliceren. U configureert de bronomgeving replicatie met behulp van een VMware één on-premises VM (configuratieserver) waarop alle van de on-premises Site Recovery-onderdelen die u nodig hebt. Na de installatie kunt u de configuratie van server-machine in de Recovery Services-kluis registreren. Selecteer vervolgens de doelinstellingen. [Meer informatie](vmware-azure-tutorial.md).
4. **Maken van een beleid voor wachtwoordreplicatie**. U maakt een beleid voor wachtwoordreplicatie waarmee wordt aangegeven hoe replicatie moet gebeuren. 
    - **Drempelwaarde voor RPO**: deze bewakingsinstelling statussen als replicatie niet binnen de opgegeven tijd wordt uitgevoerd, een waarschuwing (en eventueel een e-mailbericht) is uitgegeven. Als u de RPO-drempel op 30 minuten instellen en replicatie gebeurt gedurende 30 minuten wordt verhinderd door een probleem, wordt bijvoorbeeld een gebeurtenis gegenereerd. Deze instelling heeft geen invloed op de replicatie. Replicatie is continue en herstelpunten worden gemaakt om de paar minuten
    - **Bewaartermijn**: herstelpunt bewaarbeleid geeft aan hoe lang herstelpunten moet worden opgeslagen in Azure. Geef een waarde tussen 0 en 24 uur voor premium-opslag of omhoog tot 72 uur voor standard-opslag. U kunt de failover naar de meest recente herstelpunt of naar een opgeslagen als u de waarde die hoger is dan nul. Na de bewaarperiode worden herstelpunten opgeschoond.
    - **Crashconsistent momentopnamen**: standaard Site Recovery crashconsistent momentopnamen worden gemaakt en worden er herstelpunten aangemaakt met hen elke paar minuten. Een herstelpunt crashconsistent als alle onderdelen met elkaar verbonden gegevens schrijven volgorde consistent is, zoals ze waren op het moment het herstelpunt is gemaakt. Om beter te begrijpen, stelt u de status van de gegevens op de harde schijf van uw PC zich na een stroomstoring of een soortgelijke gebeurtenis. Er is een crashconsistent herstelpunt meestal voldoende als uw toepassing is ontworpen voor het herstellen van een crash zonder eventuele gegevensinconsistenties op.
    - **Toepassingsconsistente momentopnamen**: als deze waarde niet nul is, de Mobility-service uitgevoerd op de virtuele machine probeert om file system-toepassingsconsistente momentopnamen en herstelpunten te genereren. De eerste momentopname wordt gemaakt nadat de initiële replicatie is voltooid. Vervolgens worden momentopnamen gemaakt met de frequentie die u opgeeft. Een herstelpunt is toepassingsconsistente als naast schrijven-volgorde wordt consistente, actieve toepassingen Voer alle hun activiteiten en hun buffers naar schijf (toepassing stilleggen) leegmaken. Toepassingsconsistente herstelpunten worden aanbevolen voor databasetoepassingen zoals SQL, Oracle en Exchange. Als een momentopname van een crashconsistent voldoende is, kunt u deze waarde ingesteld op 0.  
    - **Consistentie tussen meerdere VM's**: U kunt eventueel ook een replicatiegroep maken. Wanneer u replicatie inschakelt, kunt u virtuele machines vervolgens verzamelen in die groep. Virtuele machines in een replicatie repliceren groeperen en crashconsistent en toepassingsconsistente herstelpunten wanneer failover hebt gedeeld. Moet u deze optie zorgvuldig, omdat dit kan invloed hebben op prestaties van de workload als momentopnamen worden verzameld over meerdere machines nodig. Dit alleen doen als virtuele machines dezelfde werkbelasting en consistent worden uitgevoerd en virtuele machines hebben vergelijkbare bussen. U kunt maximaal 8 virtuele machines toevoegen aan een groep. 
5. **VM-replicatie inschakelen**. Ten slotte inschakelen u replicatie voor uw lokale VMware-machines. Als u een account voor het installeren van de Mobility-service hebt gemaakt en de opgegeven Site Recovery doet een push-installatie, wordt de Mobility-service worden geïnstalleerd op elke virtuele machine waarvoor u replicatie inschakelen. [Meer informatie](vmware-azure-tutorial.md#enable-replication). Als u een replicatiegroep voor consistentie tussen meerdere VM's hebt gemaakt, kunt u virtuele machines toevoegen aan die groep.
6. **Failover testen**. Nadat u alles ingesteld, kunt u een testfailover doen om te controleren of virtuele machines een failover naar Azure zoals verwacht. [Meer informatie](tutorial-dr-drill-azure.md).
7. **Failover**. Als u de virtuele machines alleen naar Azure migreren bent - kunt u een volledige failover kunt u doen uitvoeren. Als u herstel na noodgevallen instelt, kunt u een volledige failover uitvoeren als nodig is voor. Voor volledige noodherstel, kunt na een failover naar Azure, u niet terug naar uw on-premises site als en wanneer deze beschikbaar is. [Meer informatie](vmware-azure-tutorial-failover-failback.md).

## <a name="replication-process"></a>Replicatieproces

1. Wanneer u replicatie voor een virtuele machine inschakelt, begint deze te repliceren in overeenstemming met het replicatiebeleid. 
2. Verkeer wordt gerepliceerd naar Azure storage openbare eindpunten via internet. Ook kunt u Azure ExpressRoute met [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Verkeer via een site-naar-site virtueel particulier netwerk (VPN) van een on-premises site repliceren naar Azure wordt niet ondersteund.
3. Een initiële kopie van de VM-gegevens worden gerepliceerd naar Azure-opslag.
4. Nadat de initiële replicatie is voltooid, begint de replicatie van wijzigingen naar Azure. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
5. Communicatie gebeurt er als volgt uit:

    - Virtuele machines communiceren met de lokale configuratie-server op poort 443 voor HTTPS binnenkomend verkeer voor replicatiebeheer.
    - De configuratieserver stuurt replicatie met Azure via poort 443 voor HTTPS uitgaand.
    - Virtuele machines verzenden voor binnenkomende gegevens van replicatie naar de processerver op 9443 HTTPS-poort (uitgevoerd op de servercomputer van de configuratie). Deze poort kan worden gewijzigd.
    - De processerver replicatiegegevens ontvangt, optimaliseert versleutelt en verzendt het naar Azure storage via poort 443 uitgaand.


**VMware naar Azure replicatieproces**

![Replicatieproces](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

Nadat de replicatie is ingesteld en u een disaster recovery Inzoomen (testfailover) om te controleren of alles werkt zoals verwacht worden uitgevoerd, kunt u een failover en failback uitvoeren als nodig is voor.

1. U mislukken voor een enkele computer uitvoeren of maakt u een herstelpunt plannen om meerdere virtuele machines op hetzelfde moment een failover. Het voordeel van een plan voor herstel in plaats van één machine failover zijn onder andere:
    - U kunt app-afhankelijkheden model door de virtuele machines in de app in één herstelplan.
    - U kunt scripts, Azure-runbooks, toevoegen en onderbreken voor handmatige acties.
2. Na activering van de eerste failover doorvoeren u om te starten met het openen van de werkbelasting van de Azure VM.
3. Als uw primaire on-premises site weer beschikbaar is, kunt u voorbereiden voor failback. Als u wilt een failback uit, moet u voor het instellen van een failback-infrastructuur, met inbegrip van:

    * **Tijdelijke processerver in Azure**: als u wilt een failback van Azure, instellen van een virtuele machine van Azure om te fungeren als een processerver voor het afhandelen van replicatie van Azure. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
    * **VPN-verbinding**: als u wilt een failback uit, moet u een VPN-verbinding (of ExpressRoute) van het netwerk van Azure naar de lokale site.
    * **Afzonderlijke hoofddoelserver**: standaard wordt de hoofddoelserver die is geïnstalleerd met de configuratieserver op de virtuele machine van de lokale VMware de failback afgehandeld. Als u back grote hoeveelheden verkeer mislukken wilt, een afzonderlijke on-premises hoofddoelserver server instellen voor dit doel.
    * **Failbackbeleid**: als u wilt repliceren naar de on-premises site, hebt u een failbackbeleid nodig. Dit beleid is automatisch gemaakt tijdens het maken van uw replicatiebeleid voor van on-premises naar Azure.
4. Nadat de onderdelen aanwezig is zijn, de failback in drie acties:

    - Stap 1: De Azure VM's opnieuw beveiligen zodat ze vanaf Azure repliceren terug naar de lokale VMware-machines.
    -  Stap 2: Een failover naar de lokale site uitvoert.
    - Stap 3: Nadat werkbelastingen terug zijn uitgevallen, u u replicatie voor de lokale virtuele machines.
    
 
**VMware failback vanuit Azure**

![Failback](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Volgende stappen

Ga als volgt [in deze zelfstudie](vmware-azure-tutorial.md) VMware naar Azure replicatie inschakelen.
