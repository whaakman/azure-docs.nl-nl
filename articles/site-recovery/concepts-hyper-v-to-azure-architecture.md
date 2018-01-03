---
title: Hyper-V Azure replicatie Azure Site Recovery-architectuur | Microsoft Docs
description: Dit artikel biedt een overzicht van de onderdelen en architectuur die worden gebruikt bij het repliceren van on-premises virtuele machines van Hyper-V naar Azure met de Azure Site Recovery-service.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/19/2017
ms.author: raynew
ms.openlocfilehash: aa27d75c5a1efe3971cabfe2b6a39433c772a40a
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="hyper-v-to-azure-replication-architecture"></a>Hyper-V Azure replicatie-architectuur


In dit artikel beschrijft de architectuur en processen die worden gebruikt wanneer u replicatie, failover en Hyper-V virtuele machines (VM's) tussen lokale Hyper-V-hosts en Azure herstellen met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.

Hyper-V-hosts kunnen eventueel worden beheerd in System Center Virtual Machine Manager (VMM) privéclouds.



## <a name="architectural-components---hyper-v-without-vmm"></a>Architectuur onderdelen - Hyper-V zonder VMM

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die voor Hyper-V-replicatie naar Azure wanneer u Hyper-V-hosts worden niet beheerd door VMM.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure storage-account en Azure-netwerk. | Gerepliceerde gegevens van de VM-workloads lokaal wordt opgeslagen in het opslagaccount. Virtuele machines in Azure worden gemaakt met de van de gerepliceerde Werkbelastinggegevens bij een storing van uw on-premises site.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Hyper-V** | Tijdens de implementatie van Site Recovery, u een Hyper-V-hosts en clusters verzamelen in Hyper-V-sites. U installeert de Azure Site Recovery Provider en de Recovery Services agent op elke Hyper-V-machine. | De provider coördineert de replicatie met Site Recovery via internet. De Recovery Services-agent handelt de gegevensreplicatie af.<br/><br/> De communicatie vanuit zowel de Provider als de agent is beveiligd en versleuteld. De gerepliceerde gegevens in de Azure-opslag zijn eveneens versleuteld.
**Virtuele Hyper-V-machines** | Een of meer virtuele machines waarop Hyper-V. | Er is niets moet expliciet worden geïnstalleerd op virtuele machines.


**Hyper-V aan Azure-architectuur (zonder VMM)**

![Architectuur](./media/concepts-hyper-v-to-azure-architecture/arch-onprem-azure-hypervsite.png)



## <a name="architectural-components---hyper-v-with-vmm"></a>Architectuur onderdelen - Hyper-V met VMM

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die voor Hyper-V-replicatie naar Azure wanneer Hyper-V-hosts in VMM-clouds worden beheerd.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure storage-account en Azure-netwerk. | Gerepliceerde gegevens van de VM-workloads lokaal wordt opgeslagen in het opslagaccount. Virtuele machines in Azure worden gemaakt met de gerepliceerde gegevens bij een storing van uw on-premises site.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**VMM-server** | De VMM-server heeft een of meer clouds met Hyper-V-hosts. | U installeert de Site Recovery Provider op de VMM-server naar de organisatie van replicatie met Site Recovery en registreer de server in de Recovery Services-kluis.
**Hyper-V-host** | Een of meer Hyper-V-hosts/clusters worden beheerd door VMM. |  U installeert de Recovery Services-agent op elk lid van de host of het cluster.
**Virtuele Hyper-V-machines** | Er worden een of meer VM's uitgevoerd op een Hyper-V-hostserver. | U hoeft niets expliciet te installeren op virtuele machines.
**Netwerken** | Er zijn logische en VM-netwerken ingesteld op de VMM-server. Het VM-netwerk moet worden gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud. | VM-netwerken worden toegewezen aan virtuele netwerken in Azure. Wanneer virtuele Azure-machines na een failover worden gemaakt, worden ze toegevoegd aan de Azure-netwerk dat toegewezen aan het VM-netwerk.

**Hyper-V aan Azure-architectuur (met VMM)**

![Onderdelen](./media/concepts-hyper-v-to-azure-architecture/arch-onprem-onprem-azure-vmm.png)



## <a name="replication-process"></a>Replicatieproces

![Hyper-V naar Azure replicatie](./media/concepts-hyper-v-to-azure-architecture/arch-hyperv-azure-workflow.png)

**Replicatie-en herstelproces**


### <a name="enable-protection"></a>Beveiliging inschakelen

1. Nadat u de beveiliging voor een Hyper-V-VM in Azure Portal of on-premises hebt ingeschakeld, start **Beveiliging inschakelen**.
2. Met deze taak wordt gecontroleerd of de machine voldoet aan de vereisten. Hierna wordt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aangeroepen om replicatie in te stellen op basis van de instellingen die u hebt geconfigureerd.
3. De taak start initiële replicatie door de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) aan te roepen om een volledige VM-replicatie te initialiseren en de virtuele schijven van de virtuele machine naar Azure te versturen.
4. U kunt de voortgang van de taak controleren op het tabblad **Taken**.      ![Takenlijst](media/concepts-hyper-v-to-azure-architecture/image1.png)![Inzoomen op beveiliging inschakelen](media/concepts-hyper-v-to-azure-architecture/image2.png)


### <a name="initial-data-replication"></a>Eerste-gegevensreplicatie

1. Wanneer de eerste replicatie wordt geactiveerd, een [Hyper-V VM-momentopname](https://technet.microsoft.com/library/dd560637.aspx) momentopname wordt gemaakt.
2. Virtuele harde schijven op de virtuele machine zijn een voor een gerepliceerd, tot ze allemaal zijn gekopieerd naar Azure. Dit kan even duren, afhankelijk van de VM-grootte en de netwerkbandbreedte. [Meer informatie over hoe](https://support.microsoft.com/kb/3056159) netwerkbandbreedte verhogen.
3. Als er schijfwijzigingen optreden terwijl de eerste replicatie uitgevoerd wordt, de Hyper-V Replica Replication Tracker de wijzigingen worden bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze logboekbestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een hrl-bestand dat wordt verzonden naar de secundaire opslag. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
4. Als de initiële replicatie is voltooid, wordt de VM-momentopname verwijderd.
5. Verschillen in het logboek worden gesynchroniseerd en samengevoegd op de bovenliggende schijf.


### <a name="finalize-protection-process"></a>Beveiligingsproces voltooien

1. Nadat de initiële replicatie is voltooid, de **beveiliging op de virtuele machine voltooien** taak wordt uitgevoerd. Configureert het netwerk en andere instellingen na de replicatie, zodat de virtuele machine is beveiligd.
2. U kunt de VM-instellingen om ervoor te zorgen dat deze gereed voor failover is controleren in deze fase. U kunt een disaster recovery Inzoomen (testfailover) voor de virtuele machine, mislukt de bewerking dan verwacht controleren uitvoeren. 


## <a name="delta-replication"></a>Replicatie van verschillen

1. Na de initiële replicatie begint replicatie van verschillen, in overeenstemming met het replicatiebeleid.
2. Hyper-V Replica Replication Tracker bijgehouden wijzigingen op een virtuele harde schijf als .hrl-bestanden. Elke schijf die voor replicatie is geconfigureerd, heeft een bijbehorend .hrl-bestand.
3. Het logboek wordt verzonden naar de storage-account van de klant. Wanneer een logboek tijdens verzending naar Azure, worden de wijzigingen in de primaire schijf bijgehouden in een ander logboekbestand in dezelfde map.
4. U kunt de virtuele machine in de Azure-portal bewaken tijdens de eerste en delta-replicatie.

### <a name="resynchronization-process"></a>Hersynchronisatie-proces

1. Als de replicatie van verschillen mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt de virtuele machine gemarkeerd voor hersynchronisatie.
    - Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie.
    -  Standaard hersynchronisatie gepland laten uitvoeren buiten kantooruren.
1.  Hersynchronisatie wordt alleen de deltagegevens verzonden.
    - Hiermee minimaliseert u de hoeveelheid gegevens die zijn verzonden door controlesommen van de bron en doel-virtuele machines.
    - Het maakt gebruik van een vaste blok chunking-algoritme waar de bron en doel-bestanden zijn onderverdeeld in vaste segmenten.
    - Controlesommen voor elk segment worden gegenereerd. Deze worden vergeleken om te bepalen welke blokken van de bron moeten worden toegepast op het doel.
2. Na hersynchronisatie moet de normale replicatie van verschillen worden hervat.
3. Als u niet wilt wachten tot standaard hersynchronisatie buiten uren, kunt u een virtuele machine handmatig opnieuw synchroniseren. Bijvoorbeeld als er een storing optreedt. Om dit te doen, in de Azure portal, selecteert u de virtuele machine > **opnieuw synchroniseren**.

    ![Handmatig opnieuw synchroniseren](./media/concepts-hyper-v-to-azure-architecture/image4-site.png)


### <a name="retry-process"></a>Probeer proces

Als er een replicatiefout optreedt, wordt de replicatie automatisch opnieuw geprobeerd. Zoals beschreven in de tabel opnieuw ingedeeld.

**Categorie** | **Details**
--- | ---
**Niet-herstelbare fouten** | Er wordt geen nieuwe poging gedaan. De status van de virtuele machine is **Kritiek** en tussenkomst van de beheerder is vereist.<br/><br/> Voorbeelden van deze fouten zijn een verbroken VHD-keten een ongeldige status voor de replica virtuele machine, netwerkfouten voor verificatie, autorisatie-fouten, en de VM niet fouten gevonden (voor zelfstandige Hyper-V-servers.
**Herstelbare fouten** | Nieuwe pogingen worden elk replicatie-interval uitgevoerd met behulp van een exponentiële uitstelbewerking die het interval voor nieuwe pogingen vanaf het begin van de eerste poging met 1, 2, 4, 8 en 10 minuten verhoogt. Als een fout zich blijft voordoen, probeert u het om de 30 minuten opnieuw. Voorbeelden hiervan zijn netwerkfouten, weinig schijfruimte fouten en weinig geheugen beschikbaar.



## <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande failover uitvoeren van een lokale Hyper-V-machines naar Azure. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen. Een niet-geplande failover uitvoeren als uw primaire site niet worden geopend.
2. U kunt één machine failover of herstelplannen, indelen failover van meerdere machines maken.
3. U uitvoert een failover. Nadat de eerste fase van de failover is voltooid, moet u mogelijk zijn om te zien van de gemaakte replica virtuele machines in Azure. U kunt een openbaar IP-adres toewijzen aan de VM, indien nodig.
4. U vervolgens de failover voor het starten van de toegang tot de werkbelasting van de replica virtuele machine in Azure worden doorgevoerd.

Nadat uw on-premises infrastructuur opnieuw actief is, kunt u failback uit. Failback vindt plaats in drie fasen:

1. Ere van een geplande failover van Azure naar de lokale site:
    - **Uitvaltijd te minimaliseren**: als u deze optie gebruikt gegevens vóór de failover worden gesynchroniseerd met Site Recovery. Het gewijzigde gegevensblokken controleert en downloadt deze naar de lokale site, terwijl de virtuele machine van Azure houdt actief is en de uitvaltijd te minimaliseren. Wanneer u handmatig opgeven dat de failover moet worden voltooid, de Azure VM wordt afgesloten, een definitieve wijzigingen worden gekopieerd en de failover wordt gestart.
    - **Volledige download**: met deze optie worden de gegevens gesynchroniseerd tijdens failover. Deze optie downloadt de hele schijf. Het is sneller omdat er geen controlesommen worden berekend, maar er meer uitvaltijd is. Gebruik deze optie als u hebt is met de replica virtuele Azure-machines gedurende een bepaalde periode, of als de lokale virtuele machine is verwijderd.
    - **Virtuele machine maken**: kunt u terug naar dezelfde virtuele machine of naar een andere virtuele machine mislukt. U kunt opgeven dat Site Recovery de virtuele machine maken te als deze niet al bestaat.

2. Nadat de initiële synchronisatie is voltooid, selecteert u de failover voltooien. Nadat deze is voltooid, kunt u zich aanmelden op de lokale virtuele machine te controleren of dat alles werkt zoals verwacht. In de Azure portal ziet u dat de Azure VM's zijn gestopt.
3.  Voer vervolgens de failover voltooien en opnieuw openen van de werkbelasting van de lokale virtuele machine start.
4. Nadat de werkbelastingen terug zijn uitgevallen, schakelt u omgekeerde replicatie, zodat de lokale virtuele machines naar Azure opnieuw repliceren.



## <a name="next-steps"></a>Volgende stappen


Ga als volgt [in deze zelfstudie](tutorial-prepare-azure.md) aan de slag met Hyper-V naar Azure replicatie.


