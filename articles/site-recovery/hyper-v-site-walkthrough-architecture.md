---
title: Architectuur voor Hyper-V-replicatie (zonder System Center VMM) naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel biedt een overzicht van de onderdelen en architectuur die worden gebruikt bij het repliceren van on-premises virtuele machines van Hyper-V naar Azure met de Azure Site Recovery-service.
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
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: d57cbc5b205cfb020070d567097f3bb648ce5300
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-azure"></a>Stap 1: De architectuur voor Hyper-V-replicatie naar Azure nader bekijken


In dit artikel worden de onderdelen en processen beschreven die worden gebruikt bij het repliceren van on-premises virtuele machines van Hyper-V (die niet worden beheerd door System Center VMM) naar Azure met de [Azure Site Recovery](site-recovery-overview.md)-service.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen plaatsen.



## <a name="architectural-components"></a>Architectuuronderdelen

Er zijn verschillende onderdelen betrokken bij het repliceren van virtuele machines van Hyper-V naar Azure.

**Onderdeel** | **Locatie** | **Details**
--- | --- | ---
**Azure** | U hebt in Azure een Microsoft Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig. | Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Hyper-V** | Hosts en clusters van Hyper-V worden verzameld in Hyper-V-sites. Azure Site Recovery Provider en de Recovery Services-agent worden op elke Hyper-V-host geïnstalleerd. | De provider coördineert de replicatie met Site Recovery via internet. De Recovery Services-agent handelt de gegevensreplicatie af.<br/><br/> De communicatie vanuit zowel de Provider als de agent is beveiligd en versleuteld. De gerepliceerde gegevens in de Azure-opslag zijn eveneens versleuteld.
**Virtuele Hyper-V-machines** | U hebt een of meer VM's nodig die worden uitgevoerd op een Hyper-V-hostserver. | U hoeft niets expliciet te installeren op virtuele machines.

Meer informatie over de implementatievereisten en de vereisten voor elk van deze onderdelen vindt u in de [ondersteuningsmatrix](site-recovery-support-matrix-to-azure.md).

**Afbeelding 1: replicatie van Hyper-V-site naar Azure**

![Onderdelen](./media/hyper-v-site-walkthrough-architecture/arch-onprem-azure-hypervsite.png)


## <a name="replication-process"></a>Replicatieproces

**Afbeelding 2: replicatie- en herstelproces voor Hyper-V-replicatie naar Azure**

![werkstroom](./media/hyper-v-site-walkthrough-architecture/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Beveiliging inschakelen

1. Nadat u de beveiliging voor een Hyper-V-VM in Azure Portal of on-premises hebt ingeschakeld, start **Beveiliging inschakelen**.
2. Met deze taak wordt gecontroleerd of de machine voldoet aan de vereisten. Hierna wordt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aangeroepen om replicatie in te stellen op basis van de instellingen die u hebt geconfigureerd.
3. De taak start initiële replicatie door de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) aan te roepen om een volledige VM-replicatie te initialiseren en de virtuele schijven van de virtuele machine naar Azure te versturen.
4. U kunt de voortgang van de taak controleren op het tabblad **Taken**.
 
### <a name="replicate-the-initial-data"></a>De initiële gegevens repliceren

1. Er wordt een [Hyper-V VM-momentopname](https://technet.microsoft.com/library/dd560637.aspx) gemaakt wanneer de initiële replicatie wordt geactiveerd.
2. Virtuele harde schijven worden één voor één gerepliceerd totdat ze allemaal naar Azure zijn gekopieerd. Het kan even duren, afhankelijk van de VM-grootte en de netwerkbandbreedte. Zie [How to manage on-premises to Azure protection network bandwidth usage](https://support.microsoft.com/kb/3056159) (On-premises beheren aan de hand van de door Azure-beveiliging gebruikte netwerkbandbreedte) om uw netwerkgebruik te optimaliseren.
3. Als er tijdens de eerste replicatie schijfwijzigingen optreden, worden deze met de Hyper-V Replica-replicatietracker bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze bestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een eigen HRL-bestand dat naar de secundaire opslag wordt verzonden.
4. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
5. Als de initiële replicatie is voltooid, wordt de VM-momentopname verwijderd. Verschillen in het logboek worden gesynchroniseerd en samengevoegd op de bovenliggende schijf.


### <a name="finalize-protection"></a>Beveiliging voltooien

1. Wanneer de eerste replicatie is voltooid, configureert de taak **Beveiliging van de virtuele machine voltooien** de netwerkinstellingen en overige instellingen na de replicatie zodanig dat de virtuele machine is beveiligd.
2. Als u repliceert naar Azure, moet u mogelijk de instellingen voor de virtuele machine zo aanpassen dat deze gereed is voor failover. Nu kunt u een failovertest uitvoeren om te controleren of alles werkt zoals het hoort.

### <a name="replicate-the-delta"></a>De delta repliceren

1. Na de initiële replicatie start de deltasynchronisatie in overeenstemming met de replicatie-instellingen.
2. De Hyper-V Replica-replicatietracker houdt de wijzigingen op een virtuele harde schijf bij in .hrl-bestanden. Elke schijf die voor replicatie is geconfigureerd, heeft een bijbehorend .hrl-bestand. Dit logboek wordt naar het opslagaccount van de klant verzonden nadat de initiële replicatie is voltooid. Wanneer een logboek naar Azure wordt verzonden, worden de wijzigingen in de primaire schijf bijgehouden in een ander logboekbestand in dezelfde map.
3. Tijdens de initiële replicatie en replicatie van verschillen, kunt u de virtuele machine in de VM-weergave bewaken. [Meer informatie](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Replicatie synchroniseren

1. Als de replicatie van verschillen mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt de virtuele machine gemarkeerd voor hersynchronisatie. Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie.
2.  Met een hersynchronisatie wordt de hoeveelheid verzonden gegevens geminimaliseerd door controlesommen van de bron- en doel-VM's te berekenen en alleen de verschilgegevens te verzenden. Hersynchronisatie maakt gebruik van een vaste-blokalgoritme voor verdelen in segmenten, waarbij bron- en doelbestanden in vaste segmenten worden verdeeld. Voor elk segment worden controlesommen gegenereerd en vervolgens vergeleken om te bepalen welke blokken van de bron op het doel moeten worden toegepast.
3. Na hersynchronisatie moet de normale replicatie van verschillen worden hervat. Hersynchronisatie is standaard zo gepland dat deze automatisch buiten kantooruren wordt uitgevoerd, maar u kunt een virtuele machine ook handmatig opnieuw synchroniseren. U kunt de hersynchronisatie bijvoorbeeld hervatten als er een netwerkstoring of een andere storing optreedt. Selecteer hiervoor de virtuele machine in de portal > **Opnieuw synchroniseren**.

    ![Handmatig opnieuw synchroniseren](./media/hyper-v-site-walkthrough-architecture/image4.png)


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

Ga naar [Stap 2: De implementatievereisten controleren](hyper-v-site-walkthrough-prerequisites.md)
