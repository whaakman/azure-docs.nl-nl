---
title: Architectuur voor Hyper-V-replicatie (met System Center VMM) naar Azure met Azure Site Recovery | Microsoft Docs
description: "Dit artikel biedt een overzicht van de onderdelen en architectuur die worden gebruikt bij het repliceren van on-premises Hyper-V-VM’s in VMM-clouds naar Azure met de Azure Site Recovery-service."
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
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: df4e227d02901153d3cfcfd4dfd4f11de180763a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="step-1-review-the-architecture"></a>Stap 1: de architectuur analyseren


In dit artikel worden de onderdelen en processen beschreven die worden gebruikt bij het repliceren van on-premises virtuele machines van Hyper-V in System Center VMM-clouds (Virtual Machine Manager) naar Azure met de [Azure Site Recovery](site-recovery-overview.md)-service.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen plaatsen.



## <a name="architectural-components"></a>Architectuuronderdelen

Er zijn verschillende onderdelen betrokken bij het repliceren van virtuele machines van Hyper-V in VMM-clouds naar Azure.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | U hebt in Azure een Microsoft Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig. | Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**VMM-server** | De VMM-server heeft een of meer clouds met Hyper-V-hosts. | Op de VMM-server installeert u de Site Recovery Provider voor de organisatie van de replicatie met Site Recovery, en registreert u de server in de Recovery Services-kluis.
**Hyper-V-host** | Een of meer Hyper-V-hosts/clusters worden beheerd door VMM. |  U installeert de Recovery Services-agent op elk lid van de host of het cluster.
**Virtuele Hyper-V-machines** | Er worden een of meer VM's uitgevoerd op een Hyper-V-hostserver. | U hoeft niets expliciet te installeren op virtuele machines.
**Netwerken** |Er zijn logische en VM-netwerken ingesteld op de VMM-server. Een netwerk met virtuele machines moet zijn gekoppeld aan een logisch netwerk dat verbinding heeft met de cloud. | VM-netwerken worden toegewezen aan virtuele netwerken in Azure, zodat de virtuele Azure-machines zich in een netwerk bevinden wanneer ze zijn gemaakt na een failover.

Meer informatie over de implementatievereisten en de vereisten voor elk van deze onderdelen vindt u in de [ondersteuningsmatrix](site-recovery-support-matrix-to-azure.md).


**Afbeelding 1: virtuele machines op Hyper-V-hosts in VMM-clouds repliceren naar Azure**

![Onderdelen](./media/vmm-to-azure-walkthrough-architecture/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a>Replicatieproces

**Afbeelding 2: replicatie- en herstelproces voor Hyper-V-replicatie naar Azure**

![werkstroom](./media/vmm-to-azure-walkthrough-architecture/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Beveiliging inschakelen

1. Nadat u de beveiliging voor een Hyper-V-VM in Azure Portal of on-premises hebt ingeschakeld, start **Beveiliging inschakelen**.
2. Met deze taak wordt gecontroleerd of de machine voldoet aan de vereisten. Hierna wordt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aangeroepen om replicatie in te stellen op basis van de instellingen die u hebt geconfigureerd.
3. De taak start initiële replicatie door de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) aan te roepen om een volledige VM-replicatie te initialiseren en de virtuele schijven van de virtuele machine naar Azure te versturen.
4. U kunt de voortgang van de taak controleren op het tabblad **Taken**.      ![Takenlijst](media/vmm-to-azure-walkthrough-architecture/image1.png)![Inzoomen op beveiliging inschakelen](media/vmm-to-azure-walkthrough-architecture/image2.png)

### <a name="replicate-the-initial-data"></a>De initiële gegevens repliceren

1. Er wordt een [Hyper-V VM-momentopname](https://technet.microsoft.com/library/dd560637.aspx) gemaakt wanneer de initiële replicatie wordt geactiveerd.
2. Virtuele harde schijven worden één voor één gerepliceerd totdat ze allemaal naar Azure zijn gekopieerd. Het kan even duren, afhankelijk van de VM-grootte en de netwerkbandbreedte. Zie [How to manage on-premises to Azure protection network bandwidth usage](https://support.microsoft.com/kb/3056159) (On-premises beheren aan de hand van de door Azure-beveiliging gebruikte netwerkbandbreedte) om uw netwerkgebruik te optimaliseren.
3. Als er tijdens de eerste replicatie schijfwijzigingen optreden, worden deze met de Hyper-V Replica-replicatietracker bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze bestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een eigen HRL-bestand dat naar de secundaire opslag wordt verzonden.
4. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
5. Als de initiële replicatie is voltooid, wordt de VM-momentopname verwijderd. Verschillen in het logboek worden gesynchroniseerd en samengevoegd op de bovenliggende schijf.


### <a name="finalize-protection"></a>Beveiliging voltooien

1. Wanneer de eerste replicatie is voltooid, configureert de taak **Beveiliging van de virtuele machine voltooien** de netwerkinstellingen en overige instellingen na de replicatie zodanig dat de virtuele machine is beveiligd.
    ![Taak Beveiliging voltooien](media/vmm-to-azure-walkthrough-architecture/image3.png)
2. Als u repliceert naar Azure, moet u mogelijk de instellingen voor de virtuele machine zo aanpassen dat deze gereed is voor failover. Nu kunt u een failovertest uitvoeren om te controleren of alles werkt zoals het hoort.

### <a name="replicate-the-delta"></a>De delta repliceren

1. Na de initiële replicatie start de deltasynchronisatie in overeenstemming met de replicatie-instellingen.
2. De Hyper-V Replica-replicatietracker houdt de wijzigingen op een virtuele harde schijf bij in .hrl-bestanden. Elke schijf die voor replicatie is geconfigureerd, heeft een bijbehorend .hrl-bestand. Dit logboek wordt naar het opslagaccount van de klant verzonden nadat de initiële replicatie is voltooid. Wanneer een logboek naar Azure wordt verzonden, worden de wijzigingen in de primaire schijf bijgehouden in een ander logboekbestand in dezelfde map.
3. Tijdens de initiële replicatie en replicatie van verschillen, kunt u de virtuele machine in de VM-weergave bewaken. [Meer informatie](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Replicatie synchroniseren

1. Als de replicatie van verschillen mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt de virtuele machine gemarkeerd voor hersynchronisatie. Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie.
2.  Met een hersynchronisatie wordt de hoeveelheid verzonden gegevens geminimaliseerd door controlesommen van de bron- en doel-VM's te berekenen en alleen de verschilgegevens te verzenden. Hersynchronisatie maakt gebruik van een vaste-blokalgoritme voor verdelen in segmenten, waarbij bron- en doelbestanden in vaste segmenten worden verdeeld. Voor elk segment worden controlesommen gegenereerd en vervolgens vergeleken om te bepalen welke blokken van de bron op het doel moeten worden toegepast.
3. Na hersynchronisatie moet de normale replicatie van verschillen worden hervat. Hersynchronisatie is standaard zo gepland dat deze automatisch buiten kantooruren wordt uitgevoerd, maar u kunt een virtuele machine ook handmatig opnieuw synchroniseren. U kunt de hersynchronisatie bijvoorbeeld hervatten als er een netwerkstoring of een andere storing optreedt. Selecteer hiervoor de virtuele machine in de portal > **Opnieuw synchroniseren**.

    ![Handmatig opnieuw synchroniseren](media/vmm-to-azure-walkthrough-architecture/image4.png)


### <a name="retry-logic"></a>Logica voor opnieuw proberen

Als er een replicatiefout optreedt, wordt de replicatie automatisch opnieuw geprobeerd. Deze logica kan in twee categorieën worden ingedeeld:

**Categorie** | **Details**
--- | ---
**Niet-herstelbare fouten** | Er wordt geen nieuwe poging gedaan. De status van de virtuele machine is **Kritiek** en tussenkomst van de beheerder is vereist. Enkele voorbeelden van deze fouten zijn: verbroken VHD-keten; ongeldige status voor de replica-VM; netwerkverificatiefouten: autorisatiefouten; fouten in verband met een niet gevonden virtuele machine (voor zelfstandige Hyper-V-servers)
**Herstelbare fouten** | Nieuwe pogingen worden elk replicatie-interval uitgevoerd met behulp van een exponentiële uitstelbewerking die het interval voor nieuwe pogingen vanaf het begin van de eerste poging met 1, 2, 4, 8 en 10 minuten verhoogt. Als een fout zich blijft voordoen, probeert u het om de 30 minuten opnieuw. Voorbeelden zijn onder meer: netwerkfouten; fouten in verband met weinig schijfruimte; weinig geheugenruimte |



## <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande [failover](site-recovery-failover.md) uitvoeren van on-premises virtuele Hyper-V-machines naar Azure. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
4. Nadat u de failover hebt uitgevoerd, worden de gemaakte replica-VM’s in Azure weergegeven. U kunt een openbaar IP-adres toewijzen aan de VM, indien nodig.
5. U geeft de failover vervolgens toegang tot de workload via de replica van de Azure-VM.
6. Als uw primaire on-premises site weer beschikbaar is, kunt u een [failback](site-recovery-failback-from-azure-to-hyper-v.md) uitvoeren. U start een geplande failover van Azure naar de primaire site. Voor een geplande failover kunt u een failback naar de dezelfde VM of naar een alternatieve locatie selecteren en wijzigingen synchroniseren tussen Azure en on-premises, zodat er geen gegevens verloren gaan. Wanneer VM on-premises worden gemaakt, kunt u de failover doorvoeren.




## <a name="next-steps"></a>Volgende stappen

Ga naar [Stap 2: De implementatievereisten controleren](vmm-to-azure-walkthrough-prerequisites.md)
