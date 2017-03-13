---
title: Hoe werkt Hyper-V-replicatie naar Azure in Site Recovery? | Microsoft Docs
description: Dit artikel geeft een overzicht van de werking van Hyper-V-replicatie in Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/05/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 9adf266c6a2ac00c3aaa34e2a29aefe34abe2871
ms.lasthandoff: 03/06/2017


---
# <a name="how-does-hyper-v-replication-to-azure-work"></a>Hoe werkt Hyper-V-replicatie naar Azure?

Lees dit artikel voor een beter begrip van de architectuur en werkstromen voor Hyper-V-replicatie naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md)-service.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen plaatsen.

U kunt het volgende repliceren naar Azure:
- **Hyper-V met VMM**: VM's op on-premises Hyper-V-hosts die in System Center Virtual Machine Manager-clouds (VMM) worden beheerd. Hosts kunnen worden uitgevoerd op elk [ondersteund besturingssysteem](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). U kunt virtuele Hyper-V-machines repliceren waarop gastbesturingssystemen worden uitgevoerd die [worden ondersteund door Hyper-V en Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).
- **Hyper-V zonder VMM**: on-premises VM's op Hyper-V-hosts die niet worden beheerd in VMM-clouds. Hosts kunnen elk van de [ondersteunde besturingssystemen](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) uitvoeren. U kunt virtuele Hyper-V-machines repliceren waarop gastbesturingssystemen worden uitgevoerd die [worden ondersteund door Hyper-V en Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="architectural-components"></a>Architectuuronderdelen

**Onderwerp** | **Onderdeel** | **Details**
--- | --- | ---
**Azure** | U hebt in Azure een Microsoft Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig. | Opslag en netwerk kunnen Resource Manager-gebaseerd of klassieke accounts zijn.<br/><br/> Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**VMM-server** | Hyper-V-hosts die zich in VMM-clouds bevinden | Als Hyper-V-hosts in VMM-clouds worden beheerd, kunt u de VMM-server registreren in de Recovery Services-kluis.<br/><br/> U installeert de Site Recovery-provider op de VMM-server om replicatie te beheren met Azure.<br/><br/> U hebt ingestelde logische en VM-netwerken nodig om netwerktoewijzing te configureren. Een netwerk met virtuele machines moet zijn gekoppeld aan een logisch netwerk dat verbinding heeft met de cloud.
**Hyper-V-host** | Hyper-V-servers kunnen worden geïmplementeerd met of zonder de VMM-server. | Als er geen VMM-server is, wordt de Site Recovery-provider geïnstalleerd op de host, zodat replicatie via internet kan worden beheerd met Site Recovery. Als er een VMM-server is, wordt de provider daarop geïnstalleerd en niet op de host.<br/><br/> De Recovery Services-agent wordt geïnstalleerd op de host om de gegevensreplicatie af te handelen.<br/><br/> De communicatie vanuit zowel de Provider als de agent is beveiligd en versleuteld. De gerepliceerde gegevens in de Azure-opslag zijn eveneens versleuteld.
**Virtuele Hyper-V-machines** | U hebt één of meer virtuele machines nodig op de Hyper-V-hostserver. | Er hoeft niets verplicht op virtuele machines te worden geïnstalleerd

## <a name="deployment-steps"></a>Implementatiestappen

1. **Azure**: u stelt de Azure-onderdelen in. Wij raden u aan om opslag- en netwerkaccounts in te stellen voordat u begint met de Site Recovery-implementatie.
2. **Kluis**: u maakt een Recovery Services-kluis voor Site Recovery en configureert de kluisinstellingen, met inbegrip van het configureren van de bron- en doelinstellingen, het instellen van een replicatiebeleid en het inschakelen van replicatie.
3. **Bron en doel**:
    - **Hyper-V-hosts in VMM-clouds**: als onderdeel van het opgeven van de broninstellingen downloadt en installeert u de Azure Site Recovery-provider op de VMM-server en de Azure Recovery Services-agent op elke Hyper-V-host. De bron is de VMM-server. Het doel is Azure.
    - Hyper-V-hosts zonder VMM: wanneer u instellingen voor de bron opgeeft, downloadt en installeert u de provider en agent op elke Hyper-V-host. Tijdens de implementatie verzamelt u de hosts op een Hyper-V-site en geeft u vervolgens deze site op als bron. Het doel is Azure.

    ![Hyper-V-/VMM-replicatie naar Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)
    ![Replicatie van de Hyper-V-site naar Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Replicatiebeleid**: u maakt een replicatiebeleid voor de Hyper-V-site of de VMM-cloud. Het beleid wordt toegepast op alle VM’s die zich op hosts in de site of de cloud bevinden.
5. **Replicatie inschakelen**: u schakelt replicatie voor virtuele Hyper-V-machines in. De initiële replicatie wordt uitgevoerd in overeenstemming met de instellingen van het replicatiebeleid. Verschillen in de gegevens worden bijgehouden en de replicatie van deltaverschillen naar Azure begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een item worden opgeslagen in een .hrl-bestand.
6. **Testfailover**: u voert een testfailover uit om te controleren of alles werkt.

Meer informatie over implementatie:
- [Aan de slag met replicatie van Hyper-V-VM's naar Azure (met VMM)](site-recovery-vmm-to-azure.md)
- [Aan de slag met replicatie van Hyper-V-VM's naar Azure (zonder VMM)](site-recovery-hyper-v-site-to-azure.md)

## <a name="hyper-v-replication-workflow"></a>Werkstroom van de Hyper-V-replicatie

### <a name="enable-protection"></a>Beveiliging inschakelen

1. Nadat u de beveiliging voor een Hyper-V-VM in Azure Portal of on-premises hebt ingeschakeld, start **Beveiliging inschakelen**.
2. Met deze taak wordt gecontroleerd of de machine voldoet aan de vereisten. Hierna wordt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aangeroepen om replicatie in te stellen op basis van de instellingen die u hebt geconfigureerd.
3. De taak start initiële replicatie door de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) aan te roepen om een volledige VM-replicatie te initialiseren en de virtuele schijven van de virtuele machine naar Azure te versturen.
4. U kunt de voortgang van de taak controleren op het tabblad **Taken**.
        ![Takenlijst](media/site-recovery-hyper-v-azure-architecture/image1.png)
        ![Inzoomen op beveiliging inschakelen](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>Initiële replicatie

1. Er wordt een [Hyper-V VM-momentopname](https://technet.microsoft.com/library/dd560637.aspx) gemaakt wanneer de initiële replicatie wordt geactiveerd.
2. Virtuele harde schijven worden één voor één gerepliceerd totdat ze allemaal naar Azure zijn gekopieerd. Het kan even duren, afhankelijk van de VM-grootte en de netwerkbandbreedte. Zie [How to manage on-premises to Azure protection network bandwidth usage](https://support.microsoft.com/kb/3056159) (On-premises beheren aan de hand van de door Azure-beveiliging gebruikte netwerkbandbreedte) om uw netwerkgebruik te optimaliseren.
3. Als er tijdens de eerste replicatie schijfwijzigingen optreden, worden deze met de Hyper-V Replica-replicatietracker bijgehouden als Hyper-V-replicatielogboeken (.hrl). Deze bestanden bevinden zich in dezelfde map als de schijven. Elke schijf heeft een eigen HRL-bestand dat naar de secundaire opslag wordt verzonden.
4. De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
5. Als de initiële replicatie is voltooid, wordt de VM-momentopname verwijderd. Verschillen in het logboek worden gesynchroniseerd en samengevoegd op de bovenliggende schijf.


### <a name="finalize-protection"></a>Beveiliging voltooien

1. Wanneer de eerste replicatie is voltooid, configureert de taak **Beveiliging van de virtuele machine voltooien** de netwerkinstellingen en overige instellingen na de replicatie zodanig dat de virtuele machine is beveiligd.
    ![Taak Beveiliging voltooien](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Als u repliceert naar Azure, moet u mogelijk de instellingen voor de virtuele machine zo aanpassen dat deze gereed is voor failover. Nu kunt u een failovertest uitvoeren om te controleren of alles werkt zoals het hoort.

### <a name="delta-replication"></a>Replicatie van verschillen

1. Na de initiële replicatie start de deltasynchronisatie in overeenstemming met de replicatie-instellingen.
2. De Hyper-V Replica-replicatietracker houdt de wijzigingen op een virtuele harde schijf bij in .hrl-bestanden. Elke schijf die voor replicatie is geconfigureerd, heeft een bijbehorend .hrl-bestand. Dit logboek wordt naar het opslagaccount van de klant verzonden nadat de initiële replicatie is voltooid. Wanneer een logboek naar Azure wordt verzonden, worden de wijzigingen in de primaire schijf bijgehouden in een ander logboekbestand in dezelfde map.
3. Tijdens de initiële replicatie en replicatie van verschillen, kunt u de virtuele machine in de VM-weergave bewaken. [Meer informatie](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Replicatiesynchronisatie

1. Als de replicatie van verschillen mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt de virtuele machine gemarkeerd voor hersynchronisatie. Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie.
2.  Met een hersynchronisatie wordt de hoeveelheid verzonden gegevens geminimaliseerd door controlesommen van de bron- en doel-VM's te berekenen en alleen de verschilgegevens te verzenden. Hersynchronisatie maakt gebruik van een vaste-blokalgoritme voor verdelen in segmenten, waarbij bron- en doelbestanden in vaste segmenten worden verdeeld. Voor elk segment worden controlesommen gegenereerd en vervolgens vergeleken om te bepalen welke blokken van de bron op het doel moeten worden toegepast.
3. Na hersynchronisatie moet de normale replicatie van verschillen worden hervat. Hersynchronisatie is standaard zo gepland dat deze automatisch buiten kantooruren wordt uitgevoerd, maar u kunt een virtuele machine ook handmatig opnieuw synchroniseren. U kunt de hersynchronisatie bijvoorbeeld hervatten als er een netwerkstoring of een andere storing optreedt. Selecteer hiervoor de virtuele machine in de portal > **Opnieuw synchroniseren**.

    ![Handmatig opnieuw synchroniseren](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Nieuwe pogingen

Als er een replicatiefout optreedt, wordt de replicatie automatisch opnieuw geprobeerd. Deze logica kan in twee categorieën worden ingedeeld:

**Categorie** | **Details**
--- | ---
**Niet-herstelbare fouten** | Er wordt geen nieuwe poging gedaan. De status van de virtuele machine is **Kritiek** en tussenkomst van de beheerder is vereist. Enkele voorbeelden van deze fouten zijn: verbroken VHD-keten; ongeldige status voor de replica-VM; netwerkverificatiefouten: autorisatiefouten; fouten in verband met een niet gevonden virtuele machine (voor zelfstandige Hyper-V-servers)
**Herstelbare fouten** | Nieuwe pogingen worden elk replicatie-interval uitgevoerd met behulp van een exponentiële uitstelbewerking die het interval voor nieuwe pogingen vanaf het begin van de eerste poging met 1, 2, 4, 8 en 10 minuten verhoogt. Als een fout zich blijft voordoen, probeert u het om de 30 minuten opnieuw. Voorbeelden zijn onder meer: netwerkfouten; fouten in verband met weinig schijfruimte; weinig geheugenruimte |

## <a name="protection-and-recovery-lifecycle"></a>Levenscyclus beveiliging en herstel

![werkstroom](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Volgende stappen

- [Vereisten voor implementatie controleren](site-recovery-prereq.md)
- Problemen oplossen met:
    - [Beveiliging controleren en beveiligingsproblemen oplossen](site-recovery-monitoring-and-troubleshooting.md)
    - [Hulp van Microsoft-ondersteuning](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support)
    - [Veelvoorkomende fouten en oplossingen](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions)

