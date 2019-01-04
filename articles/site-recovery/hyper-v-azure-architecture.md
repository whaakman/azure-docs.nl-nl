---
title: Hyper-V naar Azure disaster recovery-architectuur in Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en architectuur die worden gebruikt bij het implementeren van herstel na noodgevallen voor on-premises Hyper-V-machines (zonder VMM) naar Azure met de Azure Site Recovery-service.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: ae71fb9f509c39e871f4d1dfb29626be47bea4b9
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790976"
---
# <a name="hyper-v-to-azure-disaster-recovery-architecture"></a>Hyper-V naar Azure disaster recovery-architectuur


Dit artikel beschrijft de architectuur en processen die worden gebruikt bij het repliceren, failover en herstellen van Hyper-V virtuele machines (VM's) tussen on-premises Hyper-V-hosts en Azure, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.

Hyper-V-hosts kunnen desgewenst worden beheerd in persoonlijke clouds van System Center Virtual Machine Manager (VMM).



## <a name="architectural-components---hyper-v-without-vmm"></a>Architectuuronderdelen - Hyper-V zonder VMM

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die voor Hyper-V-replicatie naar Azure, als Hyper-V-hosts niet worden beheerd door VMM.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure storage-account en Azure-netwerk. | Gerepliceerde gegevens van on-premises VM-workloads worden opgeslagen in de storage-account. Azure-VM's worden gemaakt met de van gerepliceerde Werkbelastinggegevens wanneer failover van uw on-premises site wordt uitgevoerd.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Hyper-V** | Tijdens de implementatie van Site Recovery deelt u de Hyper-V-hosts en clusters in Hyper-V-sites. U kunt de Azure Site Recovery Provider en Recovery Services-agent installeren op elke zelfstandige Hyper-V-host of op elke Hyper-V-clusterknooppunt. | De provider coördineert de replicatie met Site Recovery via internet. De Recovery Services-agent handelt de gegevensreplicatie af.<br/><br/> De communicatie vanuit zowel de Provider als de agent is beveiligd en versleuteld. De gerepliceerde gegevens in de Azure-opslag zijn eveneens versleuteld.
**Virtuele Hyper-V-machines** | Een of meer virtuele machines die worden uitgevoerd op Hyper-V. | Niets expliciet worden geïnstalleerd op virtuele machines.


**Hyper-V naar Azure-architectuur (zonder VMM)**

![Architectuur](./media/hyper-v-azure-architecture/arch-onprem-azure-hypervsite.png)



## <a name="architectural-components---hyper-v-with-vmm"></a>Architectuuronderdelen - Hyper-V met VMM

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die voor Hyper-V-replicatie naar Azure, worden gebruikt wanneer Hyper-V-hosts worden beheerd in VMM-clouds.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure storage-account en Azure-netwerk. | Gerepliceerde gegevens van on-premises VM-workloads worden opgeslagen in de storage-account. Azure-VM's worden gemaakt met de gerepliceerde gegevens wanneer failover van uw on-premises site wordt uitgevoerd.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**VMM-server** | De VMM-server heeft een of meer clouds met Hyper-V-hosts. | U installeert de Site Recovery Provider op de VMM-server om replicatie met Site Recovery en de server registreren in de Recovery Services-kluis.
**Hyper-V-host** | Een of meer Hyper-V-hosts/clusters worden beheerd door VMM. |  U installeert de Recovery Services-agent op elke Hyper-V-host of cluster-knooppunt.
**Virtuele Hyper-V-machines** | Er worden een of meer VM's uitgevoerd op een Hyper-V-hostserver. | U hoeft niets expliciet te installeren op virtuele machines.
**Netwerken** | Er zijn logische en VM-netwerken ingesteld op de VMM-server. Het VM-netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud. | VM-netwerken worden toegewezen aan virtuele netwerken van Azure. Wanneer Azure-VM's zijn gemaakt na een failover, worden ze toegevoegd aan het Azure-netwerk dat toegewezen aan het VM-netwerk.

**Hyper-V naar Azure-architectuur (met VMM)**

![Onderdelen](./media/hyper-v-azure-architecture/arch-onprem-onprem-azure-vmm.png)



## <a name="replication-process"></a>Replicatieproces

![Hyper-V naar Azure-replicatie](./media/hyper-v-azure-architecture/arch-hyperv-azure-workflow.png)

**Replicatie en herstelproces**


### <a name="enable-protection"></a>Beveiliging inschakelen

1. Nadat u de beveiliging voor een Hyper-V-VM in Azure Portal of on-premises hebt ingeschakeld, start **Beveiliging inschakelen**.
2. Met deze taak wordt gecontroleerd of de machine voldoet aan de vereisten. Hierna wordt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aangeroepen om replicatie in te stellen op basis van de instellingen die u hebt geconfigureerd.
3. De taak start initiële replicatie door de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) aan te roepen om een volledige VM-replicatie te initialiseren en de virtuele schijven van de virtuele machine naar Azure te versturen.
4. U kunt de voortgang van de taak controleren op het tabblad **Taken**.      ![Takenlijst](media/hyper-v-azure-architecture/image1.png) ![Inzoomen op beveiliging inschakelen](media/hyper-v-azure-architecture/image2.png)


### <a name="initial-data-replication"></a>Eerste gegevensreplicatie

1. Wanneer de eerste replicatie wordt geactiveerd, een [Hyper-V-VM-momentopname](https://technet.microsoft.com/library/dd560637.aspx) momentopname wordt gemaakt.
2. Virtuele harde schijven op de virtuele machine worden één voor één gerepliceerd, tot ze allemaal zijn gekopieerd naar Azure. Dit kan even duren, afhankelijk van de VM-grootte en de netwerkbandbreedte. [Informatie over hoe](https://support.microsoft.com/kb/3056159) te verhogen van de netwerkbandbreedte.
3. Als er tijdens de initiële replicatie wordt uitgevoerd, worden de wijzigingen in de Hyper-V Replica Replication Tracker bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze logboekbestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een bijbehorend .hrl-bestand dat wordt verzonden naar een secundaire opslag. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
4. Als de initiële replicatie is voltooid, wordt de VM-momentopname verwijderd.
5. Verschillen in het logboek worden gesynchroniseerd en samengevoegd op de bovenliggende schijf.


### <a name="finalize-protection-process"></a>Beveiliging voltooien

1. Nadat de initiële replicatie is voltooid, de **beveiliging op de virtuele machine voltooien** taak wordt uitgevoerd. Het netwerk en de overige instellingen na de replicatie zo geconfigureerd dat de virtuele machine is beveiligd.
2. In deze fase kunt u de instellingen van de virtuele machine om ervoor te zorgen dat deze gereed voor failover is controleren. U kunt een Dr-herstelanalyse (testfailover) voor de virtuele machine, om te controleren of deze niet meer dan zoals verwacht kan uitvoeren. 


## <a name="delta-replication"></a>Replicatie van verschillen

1. Na de initiële replicatie begint de replicatie van verschillen, in overeenstemming met het replicatiebeleid.
2. De Hyper-V Replica-Replicatietracker houdt wijzigingen in een virtuele harde schijf als .hrl-bestanden. Elke schijf die voor replicatie is geconfigureerd, heeft een bijbehorend .hrl-bestand.
3. Het logboek wordt verzonden naar het storage-account van de klant. Wanneer een logboek onderweg naar Azure is, worden de wijzigingen in de primaire schijf bijgehouden in een ander logboekbestand in dezelfde map.
4. Tijdens de eerste en delta-replicatie, kunt u de virtuele machine in Azure portal controleren.

### <a name="resynchronization-process"></a>De hersynchronisatie-proces

1. Als de replicatie van verschillen mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt de virtuele machine gemarkeerd voor hersynchronisatie.
    - Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie.
    -  Standaard is de hersynchronisatie is gepland voor uitvoering automatisch buiten kantooruren.
1.  Opnieuw synchroniseren verzendt alleen deltagegevens.
    - Wordt de hoeveelheid gegevens die door controlesommen van de bron en doel-VM's worden verzonden.
    - Het maakt gebruik van een vaste-blokalgoritme waarbij bron-en doelbestanden in vaste segmenten worden onderverdeeld.
    - Controlesommen voor elk segment worden gegenereerd. Deze worden vergeleken om te bepalen welke blokken van de bron moeten worden toegepast op het doel.
2. Na hersynchronisatie moet de normale replicatie van verschillen worden hervat.
3. Als u niet wilt wachten tot de hersynchronisatie is standaard buiten uur, kunt u een virtuele machine handmatig opnieuw synchroniseren. Bijvoorbeeld als er een storing optreedt. Om dit te doen, in de Azure-portal, selecteer de virtuele machine > **synchroniseren**.

    ![Handmatig opnieuw synchroniseren](./media/hyper-v-azure-architecture/image4-site.png)


### <a name="retry-process"></a>Probeer het opnieuw verwerken

Als er een replicatiefout optreedt, wordt de replicatie automatisch opnieuw geprobeerd. Probeer het opnieuw wordt ingedeeld, zoals beschreven in de tabel.

**Categorie** | **Details**
--- | ---
**Niet-herstelbare fouten** | Er wordt geen nieuwe poging gedaan. De status van de virtuele machine is **Kritiek** en tussenkomst van de beheerder is vereist.<br/><br/> Voorbeelden van deze fouten zijn onder meer een verbroken VHD-keten, een ongeldige status voor de replica-VM, netwerkfouten voor verificatie, autorisatie-fouten, en fouten in virtuele machine niet worden gevonden (voor zelfstandige Hyper-V-servers.
**Herstelbare fouten** | Nieuwe pogingen worden elk replicatie-interval uitgevoerd met behulp van een exponentiële uitstelbewerking die het interval voor nieuwe pogingen vanaf het begin van de eerste poging met 1, 2, 4, 8 en 10 minuten verhoogt. Als een fout zich blijft voordoen, probeert u het om de 30 minuten opnieuw. Voorbeelden hiervan zijn netwerkfouten, fouten weinig schijfruimte en weinig geheugen beschikbaar.



## <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande failover van on-premises Hyper-V-machines naar Azure uitvoert. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen. Een niet-geplande failover uitvoeren als uw primaire site kan niet worden geopend.
2. U kunt een enkele machine failover of plannen voor herstel, voor het indelen van failover van meerdere virtuele machines maken.
3. U moet een failover uitvoeren. Nadat de eerste fase van de failover is voltooid, zou het mogelijk om te zien van de gemaakte replica-VM's in Azure. U kunt een openbaar IP-adres toewijzen aan de VM, indien nodig.
4. Vervolgens geeft u de failover, toegang tot de workload via de replica virtuele machine van Azure.

Nadat uw on-premises infrastructuur opnieuw actief is, kunt u failover terug. Failback vindt plaats in drie fasen:

1. Een vliegende start een geplande failover van Azure naar de on-premises site:
    - **Downtime minimaliseren**: Als u deze optie gebruikt met Site Recovery, worden gegevens voordat de failover wordt gesynchroniseerd. Het gewijzigde gegevensblokken controleert en downloadt deze naar de on-premises site, terwijl de virtuele machine van Azure houdt actief is, leiden tot minimale downtime. Wanneer u handmatig opgeven dat de failover moet worden voltooid, de Azure-VM wordt afgesloten, een definitieve wijzigingen worden gekopieerd en de failover wordt gestart.
    - **Volledige download**: Met deze optie gegevens gesynchroniseerd tijdens de failover. Deze optie wordt de hele schijf gedownload. Het is sneller omdat er geen controlesommen worden berekend, maar er meer downtime is. Gebruik deze optie als u hebt zijn met de replica virtuele Azure-machines gedurende een bepaalde periode, of als de on-premises VM is verwijderd.
    - **Virtuele machine maken**: U kunt selecteren voor failover naar dezelfde virtuele machine of een andere virtuele machine. U kunt opgeven dat Site Recovery de virtuele machine maken te als deze nog niet bestaat.

2. Nadat de initiële synchronisatie is voltooid, selecteert u om de failover te voltooien. Nadat deze is voltooid, kunt u zich aanmelden op de on-premises-VM om te controleren of dat alles werkt zoals verwacht. In de Azure-portal, kunt u zien dat de Azure VM's zijn gestopt.
3.  Vervolgens geeft u de failover voor het voltooien van en toegang tot de workload via de on-premises VM opnieuw.
4. Nadat de werkbelastingen terug zijn mislukt, schakelt u omgekeerde replicatie, zodat de on-premises VM's naar Azure opnieuw repliceren.



## <a name="next-steps"></a>Volgende stappen


Ga als volgt [in deze zelfstudie](tutorial-prepare-azure.md) aan de slag met Hyper-V naar Azure-replicatie.


