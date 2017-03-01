---
title: Hoe werkt Site Recovery? | Microsoft Docs
description: In dit artikel vindt u een overzicht van de architectuur van Site Recovery
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
ms.date: 02/21/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 080dce21c2c803fc05c945cdadb1edd55bd7fe1c
ms.openlocfilehash: 4993a873742db5ca2bd8c31eaab098beb0a0a030


---
# <a name="how-does-azure-site-recovery-work"></a>Hoe werkt Azure Site Recovery?

Lees dit artikel om inzicht te krijgen in de onderliggende architectuur van de Azure Site Recovery-service en de onderdelen daarvan.

Site Recovery is een Azure-service die bijdraagt aan uw BCDR-strategie door replicatie van fysieke on-premises servers en virtuele machines in de cloud (Azure) of naar een secundair datacenter te organiseren. Als er uitval optreedt op uw primaire locatie, schakelt u over naar de secundaire locatie om toepassingen en workloads beschikbaar te houden. U schakelt terug naar de primaire locatie wanneer deze weer normaal functioneert. Meer informatie vindt u in [Wat is Site Recovery?](site-recovery-overview.md)

Dit artikel beschrijft de implementatie in [Azure Portal](https://portal.azure.com). De [klassieke Azure-portal](https://manage.windowsazure.com/) kan worden gebruikt voor het onderhoud van de bestaande Site Recovery-kluizen. U kunt geen nieuwe kluizen maken.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen plaatsen.


## <a name="deployment-scenarios"></a>Implementatiescenario's

Site Recovery kan worden geïmplementeerd voor de organisatie van replicatie in een aantal scenario's:

- **Virtuele VMware-machines repliceren**: u kunt on-premises virtuele VMware-machines repliceren naar Azure of naar een secundair datacenter.
- **Fysieke machines repliceren**: u kunt fysieke computers (Windows of Linux) repliceren naar Azure of naar een secundair datacenter. Het proces voor het repliceren van fysieke computers is bijna hetzelfde als het proces voor het repliceren van virtuele VMware-machines.
- **Virtuele Hyper-V-machines repliceren**: u kunt virtuele Hyper-V-machines repliceren naar Azure of op een secundaire VMM-site. Als u ze wilt repliceren naar een secundaire site, moeten ze in VMM-clouds (System Center Virtual Machine Manager) worden beheerd.
- **VM’s migreren**: naast het repliceren (repliceren, failover en failback) van on-premises VM’s en fysieke servers naar Azure, kunt u ze ook migreren naar virtuele Azure-machines (repliceren, failover, geen failback). Dit is wat u kunt migreren:
    - Migreer workloads die worden uitgevoerd op on-premises virtuele Hyper-V-machines, virtuele VMware-machines en fysieke servers, zodat ze worden uitgevoerd op virtuele Azure-machines.
    - [Azure IaaS-VM's](site-recovery-migrate-azure-to-azure.md) migreren tussen Azure-regio's. Momenteel wordt in dit scenario alleen migratie ondersteund, dus geen failback.
    - Migreer [AWS Windows-exemplaren](site-recovery-migrate-aws-to-azure.md) naar Azure IaaS-VM's. Momenteel wordt in dit scenario alleen migratie ondersteund, dus geen failback.

Met Site Recovery kunt u toepassingen repliceren die op ondersteunde VM’s en fysieke servers worden uitgevoerd. Een volledig overzicht van de ondersteunde toepassingen vindt u in [Welke workloads kunt u met Azure Site Recovery beveiligen?](site-recovery-workload.md)

## <a name="replicate-vmware-vmsphysical-servers-to-azure"></a>Virtuele VMware-machines/fysieke servers repliceren naar Azure

### <a name="components"></a>Onderdelen

**Onderdeel** | **Details**
--- | ---
**Azure** | U hebt in Azure een Microsoft Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig.<br/><br/> Opslag en netwerk kunnen Resource Manager-accounts of klassieke accounts zijn.<br/><br/>  Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratieserver** | U kunt on-premises een configuratieserver instellen voor de coördinatie van communicatie tussen de on-premises site en Azure, en voor het beheren van de replicatie van gegevens.
**Processerver** | Standaard geïnstalleerd op de on-premises configuratieserver.<br/><br/> Fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens van beveiligde bronmachines, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag.<br/><br/> Hier wordt de push-installatie van de Mobility-service naar beveiligde computers afgehandeld en wordt automatische detectie van virtuele VMware-machines uitgevoerd.<br/><br/> Naarmate uw implementatie groeit, kunt u extra, afzonderlijk toegewezen processervers toevoegen om het toenemende replicatieverkeer af te handelen.
**Hoofddoelserver** | Standaard geïnstalleerd op de on-premises configuratieserver.<br/><br/> Hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld. Als de hoeveelheid failbackverkeer hoog is, kunt u een afzonderlijke hoofddoelserver voor failback implementeren.
**VMware-servers** | U kunt vCenter- en vSphere-servers toevoegen aan uw Recovery Services-kluis om virtuele VMware-machines repliceren.<br/><br/> Als u fysieke servers wilt repliceren, hebt u een on-premises VMware-infrastructuur voor failback nodig. U kunt geen failback uitvoeren naar een fysieke computer.
**Gerepliceerde machines** | De Mobility-service moet worden geïnstalleerd op elke computer die u wilt repliceren. Deze kan handmatig worden geïnstalleerd op elke computer, of met een push-installatie van de processerver.

**Afbeelding 1: onderdelen van VMware naar Azure**

![Onderdelen](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Replicatieproces

1. U stelt de implementatie, met inbegrip van de Azure-onderdelen, en een Recovery Services-kluis in. In de kluis geeft u de replicatiebron en het replicatiedoel op, stelt u de configuratieserver in, voegt u VMware-servers toe, maakt u een replicatiebeleid, implementeert u de Mobility-service, schakelt u de replicatie in en voert u een failovertest uit.
2.  Machines beginnen met de replicatie volgens het replicatiebeleid. Een initiële kopie van de gegevens wordt gerepliceerd naar Azure Storage.
4. De replicatie van deltaverschillen naar Azure begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
    - Replicerende machines communiceren met de configuratieserver op inkomende HTTPS-poort 443 voor replicatie.
    - Replicerende machines verzenden replicatiegegevens naar de processerver op inkomende HTTPS-poort 9443 (kan worden geconfigureerd).
    - De configuratieserver coördineert het replicatiebeheer met Azure via de uitgaande HTTPS-poort 443.
    - De processerver ontvangt gegevens van de bronmachines, optimaliseert en versleutelt deze gegevens en verzendt ze naar Azure Storage via de uitgaande poort 443.
    - Als u consistentie tussen meerdere VM's inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Meerdere VM’s worden gebruikt als u meerdere machines groepeert in de replicatiegroepen die crashconsistent en app-consistent herstelpunten delen bij failover. Dit is handig als machines dezelfde workload gebruiken en consistent moeten zijn.
5. Verkeer wordt via internet gerepliceerd naar de openbare eindpunten van Azure Storage. U kunt ook [openbare peering](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) van Azure ExpressRoute gebruiken. Het repliceren van verkeer via een site-naar-site-VPN van een on-premises site naar Azure wordt niet ondersteund.

**Afbeelding 2: replicatie van VMware naar Azure**

![Verbeterd](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt niet-geplande failovers uitvoeren vanaf on-premises virtuele VMware- machines en fysieke servers naar Azure. Geplande failover wordt niet ondersteund.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
3. Wanneer u een failover uitvoert, worden er replica-VM's gemaakt in Azure. U geeft een failover toegang tot de workload via de replica van de Azure-VM.
4. Als uw primaire on-premises site weer beschikbaar is, kunt u een failback uitvoeren. U stelt een infrastructuur voor failback in, start de replicatie van de machine vanaf de secundaire site naar de primaire site en voert een niet-geplande failover uit vanaf de secundaire site. Nadat u deze failover doorvoert, worden de gegevens teruggeplaatst op de on-premises site en moet u de replicatie naar Azure opnieuw inschakelen. [Meer informatie](site-recovery-failback-azure-to-vmware.md)

Er zijn enkele vereisten voor een failback:

- **Failback van fysiek-naar-fysiek wordt niet ondersteund**: dit betekent dat als u voor fysieke servers een failover naar Azure uitvoert en vervolgens een failback wilt uitvoeren, u een failback moet uitvoeren naar een virtuele VMware-machine. U kunt geen failback uitvoeren naar een fysieke server. U hebt een virtuele machine van Azure nodig om een failback naar uit te voeren, en als u de configuratieserver niet als een als een virtuele VMware-machine hebt geïmplementeerd, moet u een afzonderlijke hoofddoelserver instellen als een virtuele VMware-machine. Dit is nodig omdat de hoofddoelserver communiceert met en is gekoppeld aan VMware-opslag om de schijven te herstellen naar een virtuele VMware-machine.
- **Tijdelijke processerver in Azure**: als u na een failover een failback vanuit Azure wilt uitvoeren, moet u een virtuele Azure-machine hebben geconfigureerd als processerver, om de replicatie vanuit Azure af te handelen. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
- **VPN-verbinding**: voor de failback moet u een VPN-verbinding (of Azure ExpressRoute) hebben ingesteld van het Azure-netwerk naar de on-premises site.
- **Afzonderlijke on-premises hoofddoelserver**: op de on-premises hoofddoelserver wordt de failback afgehandeld. De hoofddoelserver wordt standaard op de beheerserver geïnstalleerd, maar als u een failback van grotere hoeveelheden verkeer uitvoert, kunt u voor dit doel beter een afzonderlijke on-premises hoofddoelserver instellen.
- **Failbackbeleid**: als u wilt repliceren naar de on-premises site, hebt u een failbackbeleid nodig. Dit wordt automatisch gemaakt wanneer u uw replicatiebeleid maakt.

**Afbeelding 3: failback van VMware/fysieke failback**

![Failback](./media/site-recovery-components/enhanced-failback.png)


## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>Virtuele VMware-machines/fysieke servers repliceren naar een secundaire site

### <a name="components"></a>Onderdelen

**Onderdeel** | **Details**
--- | ---
**Azure** | U implementeert dit scenario met InMage Scout. Voor het gebruik van InMage Scout hebt u een Azure-abonnement nodig.<br/><br/> Nadat u een Recovery Services-kluis hebt gemaakt, downloadt u InMage Scout en installeert u de nieuwste updates om de implementatie in te stellen.
**Processerver** | U stelt het processerveronderdeel in op uw primaire site om caching, compressie en optimalisatie van gegevens af te handelen.<br/><br/> Hier wordt ook de push-installatie van de Unified Agent afgehandeld naar computers die u wilt beveiligen.
**VMware ESX/ESXi- en vCenter-server** |  U hebt een VMware-infrastructuur nodig om virtuele VMware-machines te repliceren.
**VM's/fysieke servers** |  U installeert de Unified Agent op virtuele VMware-machines of fysieke servers met Windows of Linux die u wilt repliceren.<br/><br/> De agent fungeert als communicatieprovider tussen alle onderdelen.
**Configuratieserver** | Dit is het eerste onderdeel dat u installeert op de secundaire site om uw implementatie te beheren, te configureren en te bewaken met behulp van de beheerwebsite of de vContinuum-console.
**vContinuum-server** | Deze wordt geïnstalleerd op dezelfde locatie als de configuratieserver.<br/><br/> De vContinuum-server biedt een console voor het beheren en controleren van de beveiligde omgeving.
**Hoofddoelserver (secundaire site)** | De hoofddoelserver bevat gerepliceerde gegevens. De hoofddoelserver ontvangt gegevens van de processerver, maakt een replicamachine op de secundaire site en bevat de gegevensretentiepunten.<br/><br/> Het aantal hoofddoelservers dat u nodig hebt, is afhankelijk van het aantal machines dat u wilt beveiligen.<br/><br/> Als u failback wilt uitvoeren naar de primaire site, hebt u daar ook een hoofddoelserver nodig. De Unified Agent is geïnstalleerd op deze server.

### <a name="replication-process"></a>Replicatieproces

1. U stelt de onderdeelservers in op elke site (configuratie, proces, hoofddoel) en installeert de Unified Agent op de machines die u wilt repliceren.
2. Na de initiële replicatie verzendt de agent op elke machine deltareplicatiewijzigingen naar de processerver.
3. De processerver optimaliseert de gegevens en brengt deze over naar de hoofddoelserver op de secundaire site. De configuratieserver beheert het replicatieproces.

**Afbeelding 4: replicatie van VMware naar VMware**

![VMware naar VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="replicate-hyper-v-vms-to-azure"></a>Virtuele Hyper-V-machines repliceren naar Azure


### <a name="components"></a>Onderdelen

**Onderdeel** | **Details**
--- | ---

**Azure** | U hebt in Azure een Microsoft Azure-account, een Azure-opslagaccount en een Azure-netwerk nodig.<br/><br/> Opslag en netwerk kunnen Resource Manager-gebaseerd of klassieke accounts zijn.<br/><br/> Gerepliceerde gegevens worden opgeslagen in het opslagaccount. Azure-VM’s worden gemaakt met de gerepliceerde gegevens bij failover van uw on-premises site.<br/><br/> De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**VMM-server** | Als de Hyper-V-hosts zich in de VMM-clouds bevinden, hebt u logische netwerken en VM-netwerken nodig die zijn ingesteld om de netwerkkoppeling te configureren. Een netwerk met virtuele machines moet zijn gekoppeld aan een logisch netwerk dat verbinding heeft met de cloud.
**Hyper-V-hosts** | U hebt één of meer Hyper-V-hostservers nodig.
**Virtuele Hyper-V-machines** | u hebt een of meer virtuele machines nodig op de Hyper-V-hostserver. De provider die de Hyper-V-host uitvoert, coördineert en stuurt replicatie met de Site Recovery-service via internet. De agent handelt de gegevens voor gegevensreplicatie af via HTTPS 443. De communicatie vanuit zowel de Provider als de agent is beveiligd en versleuteld. De gerepliceerde gegevens in de Azure-opslag zijn eveneens versleuteld.


## <a name="replication-process"></a>Replicatieproces

1. U stelt de Azure-onderdelen in. Wij raden u aan om opslag- en netwerkaccounts in te stellen voordat u begint met de Site Recovery-implementatie.
2. U maakt een Replication Services-kluis voor Site Recovery en configureert de kluisinstellingen, waaronder:
    - Als u geen Hyper-V-hosts in een VMM-cloud beheert, moet u een Hyper-V-sitecontainer maken en hier Hyper-V-hosts aan toevoegen.
    - De replicatiebron en het replicatiedoel. Als uw Hyper-V-hosts in VMM worden beheerd, is de bron de VMM-cloud. Als dit niet het geval is, is de bron de Hyper-V-site.
    - Installatie van de Azure Site Recovery-provider en de Microsoft Azure Recovery Services-agent. Als u VMM hebt, wordt de Provider hierop geïnstalleerd en de agent op elke Hyper-V-host. Als u geen VMM hebt, worden de provider en de agent geïnstalleerd op elke host.
    - U maakt een replicatiebeleid voor de Hyper-V-site of de VMM-cloud. Het beleid wordt toegepast op alle VM’s die zich op hosts in de site of de cloud bevinden.
    - U schakelt replicatie voor virtuele Hyper-V-machines in. De initiële replicatie wordt uitgevoerd in overeenstemming met de instellingen van het replicatiebeleid.
4. Verschillen in de gegevens worden bijgehouden en de replicatie van deltaverschillen naar Azure begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een item worden opgeslagen in een .hrl-bestand.
5. U voert een testfailover uit om te controleren of alles werkt.

### <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande [failover](site-recovery-failover.md) uitvoeren van on-premises virtuele Hyper-V-machines naar Azure. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
4. Nadat u de failover hebt uitgevoerd, worden de gemaakte replica-VM’s in Azure weergegeven. U kunt een openbaar IP-adres toewijzen aan de VM, indien nodig.
5. U geeft de failover vervolgens toegang tot de workload via de replica van de Azure-VM.
6. Als uw primaire on-premises site weer beschikbaar is, kunt u een failback uitvoeren. U start een geplande failover van Azure naar de primaire site. Voor een geplande failover kunt u een failback naar de dezelfde VM of naar een alternatieve locatie selecteren en wijzigingen synchroniseren tussen Azure en on-premises, zodat er geen gegevens verloren gaan. Wanneer VM on-premises worden gemaakt, kunt u de failover doorvoeren.

**Afbeelding 5: replicatie van Hyper-V-site naar Azure**

![Onderdelen](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Afbeelding 6: replicatie van Hyper-V in VMM-clouds naar Azure**

![Onderdelen](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)



## <a name="replicate-hyper-v-vms-to-a-secondary-site"></a>Virtuele Hyper-V-machines repliceren naar een secundaire site

### <a name="components"></a>Onderdelen

**Onderdeel** | **Details**
--- | ---
**Azure-account** | U hebt een Microsoft Azure-account nodig.
**VMM-server** | Aanbevolen is een VMM-beheerserver op de primaire site en een op de secundaire site, met verbinding met het internet.<br/><br/> Elke server moet ten minste één VMM-privécloud hebben met de Hyper-V-profielset.<br/><br/> U installeert de Azure Site Recovery-provider op de VMM-server. De Provider coördineert en stuurt replicatie met de Site Recovery-service via internet. De communicatie tussen de Provider en Azure is beveiligd en versleuteld.
**Hyper-V-server** |  U hebt één of meer Hyper-V-hostservers nodig die zich bevinden in de primaire en secundaire VMM-clouds. Servers moeten verbinding met het internet hebben.<br/><br/> Gegevens worden tussen de primaire en secundaire Hyper-V-hostservers via het LAN of VPN gerepliceerd met behulp van Kerberos of verificatie via certificaat.  
**Bronmachines** | De bron-Hyper-V-hostserver moet ten minste één VM hebben die u wilt repliceren.

## <a name="replication-process"></a>Replicatieproces

1. U stelt het Azure-account in.
2. U maakt een Replication Services-kluis voor Site Recovery en configureert de kluisinstellingen, waaronder:

    - De replicatiebron en het replicatiedoel (primaire en secundaire sites).
    - Installatie van de Azure Site Recovery-provider en de Microsoft Azure Recovery Services-agent. De provider wordt geïnstalleerd op de VMM-servers en de agent op elke Hyper-V-host.
    - U maakt een replicatiebeleid voor de bron-VMM-cloud. Het beleid wordt toegepast op alle VM’s die zich op hosts in de cloud bevinden.
    - U schakelt replicatie voor virtuele Hyper-V-machines in. De initiële replicatie wordt uitgevoerd in overeenstemming met de instellingen van het replicatiebeleid.
4. Verschillen in de gegevens worden bijgehouden en de replicatie van deltaverschillen begint nadat de initiële replicatie is voltooid. Bijgehouden wijzigingen voor een item worden opgeslagen in een .hrl-bestand.
5. U voert een testfailover uit om te controleren of alles werkt.

**Afbeelding 7: replicatie van VMM naar VMM**

![On-premises naar on-premises](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Failover- en failbackproces

1. U kunt een geplande of niet-geplande [failover](site-recovery-failover.md) uitvoeren tussen on-premises sites. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.
2. U kunt een failover van één machine uitvoeren of [herstelplannen](site-recovery-create-recovery-plans.md) maken om de failover van meerdere virtuele machines te coördineren.
4. Als u een niet-geplande failover naar een secundaire site uitvoert wanneer de failovermachines in de secundaire locatie niet zijn ingeschakeld voor beveiliging of replicatie. Als u een geplande failover hebt uitgevoerd, zijn de machines op de secundaire locatie na de failover beveiligd.
5. U geeft de failover vervolgens toegang tot de werkbelasting via de replica-VM.
6. Als uw primaire site weer beschikbaar is, start u de omgekeerde replicatie van de secundaire site naar de primaire site. Omgekeerde replicatie geeft de virtuele machines een beveiligde status, maar het secundaire datacenter is nog steeds de actieve locatie.
7. Als u de primaire site weer de actieve locatie wilt maken, start u een geplande failover van de secundaire site naar de primaire site, gevolgd door nog een omgekeerde replicatie.


### <a name="hyper-v-replication-workflow"></a>Werkstroom van de Hyper-V-replicatie

**Werkstroomfase** | **Actie**
--- | ---
1. **Beveiliging inschakelen** | Nadat u de beveiliging voor een virtuele Hyper-V-machine hebt ingeschakeld, wordt de taak **Beveiliging inschakelen** gestart om te controleren of de computer aan de vereisten voldoet. De taak gebruikt twee methoden:<br/><br/> [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) voor het instellen van de replicatie met de instellingen die u hebt geconfigureerd.<br/><br/> [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) om een volledige VM-replicatie te initialiseren.
2. **Initiële replicatie** |  Er wordt een momentopname van de virtuele machine gemaakt en de virtuele harde schijven worden een voor een gerepliceerd tot ze allemaal zijn gekopieerd naar de secundaire locatie.<br/><br/> De tijd die nodig is om dit te voltooien, is afhankelijk van de grootte van de virtuele machine, van de netwerkbandbreedte en van de oorspronkelijke replicatiemethode.<br/><br/> Als er tijdens de eerste replicatie schijfwijzigingen optreden, worden deze door de Hyper-V Replica Replication Tracker bijgehouden als Hyper-V-replicatielogboeken (HRL-bestanden), die zich in dezelfde map bevinden als de schijven.<br/><br/> Elke schijf heeft een eigen HRL-bestand dat naar de secundaire opslag wordt verzonden.<br/><br/> De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd. Wanneer de eerste replicatie is voltooid, wordt de momentopname van de virtuele machine verwijderd, en worden de verschillen in het logboek gesynchroniseerd en samengevoegd.
3. **Beveiliging voltooien** | Wanneer de eerste replicatie is voltooid, worden de netwerkinstellingen en de overige instellingen na de replicatie zo geconfigureerd dat de virtuele machine is beveiligd. Hiervoor wordt de taak **Beveiliging voltooien** gebruikt.<br/><br/> Als u repliceert naar Azure, moet u mogelijk de instellingen voor de virtuele machine zo aanpassen dat deze gereed is voor failover.<br/><br/> Nu kunt u een failovertest uitvoeren om te controleren of alles werkt zoals het hoort.
4. **Replicatie** | Na de initiële replicatie start de deltasynchronisatie in overeenstemming met de replicatie-instellingen.<br/><br/> **Replicatiefout**: als de deltareplicatie mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt er een hersynchronisatie uitgevoerd. Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie. Met een hersynchronisatie wordt de hoeveelheid verzonden gegevens geminimaliseerd door controlesommen van de bron- en virtuele machines te berekenen en alleen de verschillen (delta) te verzenden. Na hersynchronisatie wordt de deltareplicatie hervat. Hersynchronisatie is standaard zo gepland dat deze automatisch buiten kantooruren wordt uitgevoerd, maar u kunt een virtuele machine ook handmatig opnieuw synchroniseren.<br/><br/> **Replicatiefout**: als er een replicatiefout optreedt, wordt de replicatie automatisch opnieuw geprobeerd. Als het een onherstelbare fout betreft, zoals een verificatie- of autorisatiefout, of als een replicamachine een ongeldige status heeft, wordt er geen nieuwe poging gedaan. Als het een herstelbare fout betreft, zoals een netwerkfout of weinig schijfruimte/geheugen, wordt er met steeds groter wordende intervallen (1, 2, 4, 8, 10 en vervolgens elke 30 minuten) een nieuwe poging gedaan.
5. **Geplande/niet-geplande failover** | U kunt naar behoefte geplande of niet-geplande failovers uitvoeren.<br/><br/> Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen.<br/><br/> Nadat de replica's van de virtuele machines zijn gemaakt, krijgen deze de status In behandeling. U moet ze doorvoeren om de failover te voltooien.<br/><br/> Wanneer de primaire site eenmaal wordt uitgevoerd, kunt u een failback naar de primaire site uitvoeren als deze beschikbaar is.


**Afbeelding 8: Hyper-V-werkstroom**

![werkstroom](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Volgende stappen

[Vereisten controleren](site-recovery-prereq.md)



<!--HONumber=Feb17_HO4-->


