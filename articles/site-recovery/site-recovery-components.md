<properties
    pageTitle="Hoe werkt Site Recovery? | Microsoft Azure"
    description="In dit artikel vindt u een overzicht van de architectuur van Site Recovery"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/12/2016"
    ms.author="raynew"/>

# Hoe werkt Azure Site Recovery?

Lees dit artikel om inzicht te krijgen in de onderliggende architectuur van de service Azure Site Recovery en de onderdelen daarvan. 

Eventuele opmerkingen of vragen kunt u onder aan dit artikel plaatsen of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## Overzicht

Organisaties hebben een BCDR-strategie nodig die bepaalt op welke wijze toepassingen, workloads en gegevens beschikbaar blijven tijdens geplande en ongeplande uitval, en ervoor zorgt dat deze zo snel mogelijk worden hersteld naar een normale manier van functioneren. In uw BCDR-strategie moeten bedrijfsgegevens veilig en herstelbaar blijven. Ook moet ervoor worden gezorgd dat workloads continu beschikbaar blijven wanneer zich een noodsituatie voordoet. 

Site Recovery is een Azure-service die bijdraagt aan uw BCDR-strategie door replicatie van fysieke on-premises servers en virtuele machines in de cloud (Azure) of naar een secundair datacenter te organiseren. Als er uitval optreedt op uw primaire locatie, schakelt u over naar de secundaire locatie om toepassingen en workloads beschikbaar te houden. U schakelt terug naar de primaire locatie wanneer deze weer normaal functioneert. Zie voor meer informatie [Wat is Site Recovery?](site-recovery-overview.md)

## Site Recovery in de Azure Portal

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken van en werken met resources: het Azure Resource Manager-model (ARM) en het klassieke model voor het beheer van services. Azure heeft ook twee portals: de [klassieke Azure Portal](https://manage.windowsazure.com/), die ondersteuning biedt voor het klassieke implementatiemodel, en de [Azure Portal](https://portal.azure.com), die ondersteuning biedt voor beide implementatiemodellen.

Site Recovery is zowel beschikbaar in de klassieke portal als in de Azure Portal. In de klassieke Azure Portal kunt u Site Recovery ondersteunen met het klassieke model voor het beheer van services. In de Azure Portal kunt u implementaties voor het klassieke model of het resourcemodel ondersteunen. [Lees meer informatie](site-recovery-overview.md#site-recovery-in-the-azure-portal) over het implementeren via de Azure Portal.

De informatie in dit artikel is van toepassing op zowel klassieke als Azure Portal-implementaties. Verschillen worden vermeld, indien van toepassing.

## Implementatiescenario's

Site Recovery kan worden geïmplementeerd voor de organisatie van replicatie in een aantal scenario's:

- **Virtuele VMware-machines repliceren**: u kunt on-premises virtuele VMware-machines repliceren naar Azure of naar een secundair datacenter.
- - **Fysieke machines repliceren**: u kunt fysieke computers waarop Windows of Linux wordt uitgevoerd, repliceren naar Azure of naar een secundair datacenter. Het proces voor het repliceren van fysieke computers is bijna hetzelfde als het proces voor het repliceren van virtuele VMware-machines
- **Virtuele Hyper-V-machines (zonder VMM) repliceren**: u kunt virtuele Hyper-V-machines die niet door VMM worden beheerd, repliceren naar Azure.
- **Virtuele Hyper-V-machines repliceren die worden beheerd in System Center VMM-clouds**: u kunt on-premises virtuele Hyper-V-machines die worden uitgevoerd op Hyper-V-hostservers in VMM-clouds repliceren naar Azure of naar een secundair datacenter. U kunt repliceren met behulp van standaard Hyper-V Replica of SAN-replicatie.
- **Virtuele machines migreren**: met Site Recovery kunt u [virtuele Azure IaaS-machines migreren](site-recovery-migrate-azure-to-azure.md) tussen regio's of [AWS Windows-exemplaren migreren](site-recovery-migrate-aws-to-azure.md) naar virtuele Azure IaaS-machines. Momenteel wordt alleen migratie ondersteund. Dat betekent dat er voor deze virtuele machines wel een failover is, maar geen failback.

Met Site Recovery kunt u de meeste toepassingen repliceren die op deze virtuele machines en fysieke servers worden uitgevoerd. Een volledig overzicht van de ondersteunde toepassingen vindt u in [Welke workloads kunt u met Azure Site Recovery beveiligen?](site-recovery-workload.md)


## Repliceren naar Azure: virtuele VMware-machines of fysieke Windows-/Linux-servers

Er zijn verschillende manieren om virtuele VMware-machines te repliceren met Site Recovery.

- **Met behulp van de Azure Portal**: bij het implementeren van Site Recovery in de Azure Portal kunt u voor VM's een failover uitvoeren naar klassieke Service Manager-opslag of naar Resource Manager. Het repliceren van virtuele VMware-machines in de Azure Portal brengt een aantal voordelen met zich mee, inclusief de mogelijkheid om te repliceren naar klassieke of Resource Manager-opslag in Azure. [Meer informatie](site-recovery-vmware-to-azure.md).
- **Met behulp van de klassieke portal**: u kunt Site Recovery in de klassieke portal implementeren met een verbeterde gebruikerservaring. Dit moet voor alle nieuwe implementaties in de klassieke portal worden gebruikt. In deze implementatie kunt u voor VM's alleen een failover uitvoeren naar klassieke opslag in Azure en niet naar Resource Manager-opslag. [Meer informatie](site-recovery-vmware-to-azure-classic.md). Er is ook een [verouderde ervaring](site-recovery-vmware-to-azure-classic-legacy.md) voor het instellen van de VMware-replicatie in de klassieke portal. Deze mag niet worden gebruikt voor nieuwe implementaties.  Als u al hebt geïmplementeerd met de verouderde ervaring, [lees dan meer informatie over migratie](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) naar de verbeterde implementatie.



De architectuurvereisten voor het implementeren van Site Recovery voor het repliceren van VMware VM's/fysieke servers in de Azure Portal of de klassieke Azure Portal (uitgebreid) zijn vergelijkbaar, met een paar verschillen:

- Als u in de Azure Portal implementeert, kunt u repliceren naar op Resource Manager gebaseerde opslag en de Resource Manager-netwerken gebruiken om verbinding te maken met de Azure VM's na een failover.
- Wanneer u in de Azure Portal implementeert, wordt zowel LRS- als GRS-opslag ondersteund. In de klassieke portal is GRS vereist.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de Azure Portal.


U hebt het volgende nodig:

- **Azure-account**: u hebt een Microsoft Azure-account nodig.
- **Azure-opslag**: u hebt een Azure-opslagaccount nodig om gerepliceerde gegevens op te slaan. U kunt een klassiek account of een Resource Manager-opslagaccount gebruiken. De account kan LRS of GRS zijn wanneer u in de Azure Portal implementeert. Gerepliceerde gegevens worden opgeslagen in Azure-opslag en bij een failover worden virtuele Azure-machines ingezet. 
- **Azure-netwerk**: u hebt een virtueel Azure-netwerk nodig waarmee de virtuele Azure-machines verbinding maken wanneer ze worden gemaakt na een failover. In de Azure Portal kunnen dit netwerken zijn die zijn gemaakt in het klassieke model voor het beheren van services of in het Resource Manager-model.
- **On-premises configuratieserver**: u hebt een on-premises Windows Server 2012 R2-machine nodig waarop de configuratieserver en andere onderdelen van Site Recovery worden uitgevoerd. Als u virtuele VMware-machines uitvoert, moet dit een maximaal beschikbare virtuele machine van VMware zijn. Als u fysieke servers wilt repliceren, kan de machine fysiek zijn. Deze Site Recovery-onderdelen worden geïnstalleerd op de machine:
    - **Configuratieserver**: coördineert de communicatie tussen uw on-premises omgeving en Azure, en beheert gegevensreplicatie en herstel.
    - **Processerver**: fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens van beveiligde bronmachines, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag. Hier wordt ook de push-installatie van de Mobility-service naar beveiligde computers afgehandeld en wordt automatische detectie van virtuele VMware-machines uitgevoerd. Naarmate uw implementatie groeit, kunt u extra, afzonderlijk toegewezen processervers toevoegen om het toenemende replicatieverkeer af te handelen.
    - **Hoofddoelserver**: hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld. 
- **Te repliceren virtuele VMWare-machines/fysieke servers**: op elke machine die u naar Azure wilt repliceren, moet de Mobility-service zijn geïnstalleerd. Deze service legt gegevens vast die naar de machine worden weggeschreven en stuurt ze door naar de processerver. Dit onderdeel kan handmatig worden geïnstalleerd, maar ook worden gepusht en vervolgens automatisch door de processerver worden geïnstalleerd wanneer u replicatie voor een machine inschakelt.
- **vSPhere-hosts/vCenter-server**: u hebt een of meer vSphere-hostservers nodig waarop virtuele VMware-machines worden uitgevoerd. We raden u aan een vCenter-server te implementeren voor het beheer van deze hosts.
- **Failback**: u hebt het volgende nodig:
    - **Failback van fysiek-naar-fysiek wordt niet ondersteund**: dit betekent dat als u voor fysieke servers een failover naar Azure uitvoert en vervolgens een failback wilt uitvoeren, u een failback moet uitvoeren naar een virtuele VMware-machine. U kunt geen failback uitvoeren naar een fysieke server. U hebt een virtuele machine van Azure nodig om een failback naar uit te voeren, en als u de configuratieserver niet als een als een virtuele VMware-machine hebt geïmplementeerd, moet u een afzonderlijke hoofddoelserver instellen als een virtuele VMware-machine. Dit is nodig omdat de hoofddoelserver communiceert met en is gekoppeld aan VMware-opslag om de schijven te herstellen naar een virtuele VMware-machine.
    - - **Tijdelijke processerver in Azure**: als u na een failover een failback vanuit Azure wilt uitvoeren, moet u een virtuele Azure-machine hebben geconfigureerd als processerver, om de replicatie vanuit Azure af te handelen. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.
    - **VPN-verbinding**: voor de failback moet u een VPN-verbinding (of Azure ExpressRoute) hebben ingesteld van het Azure-netwerk naar de on-premises site.
    - **Afzonderlijke on-premises hoofddoelserver**: op de on-premises hoofddoelserver wordt de failback afgehandeld. De hoofddoelserver wordt standaard op de beheerserver geïnstalleerd, maar als u een failback van grotere hoeveelheden verkeer uitvoert, kunt u voor dit doel beter een afzonderlijke on-premises hoofddoelserver instellen.

**Algemene architectuur**

![Verbeterd](./media/site-recovery-components/arch-enhanced.png)

**Implementatieonderdelen**

![Verbeterd](./media/site-recovery-components/arch-enhanced2.png)

**Failback**

![Failback in de verbeterde architectuur](./media/site-recovery-components/enhanced-failback.png)


- [Lees meer](site-recovery-vmware-to-azure.md#azure-prerequisites) over de implementatievereisten voor Azure Portal.
- [Lees meer](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) over de implementatievereisten voor de verbeterde architectuur in de klassieke portal.
- [Lees meer](site-recovery-failback-azure-to-vmware.md) over failback in de Azure Portal.
- [Lees meer](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) over failback in de klassieke portal.

## Repliceren naar Azure: Hyper-V VM’s niet beheerd door VMM

U kunt Hyper-V VM’s die niet worden beheerd door System Center VMM als volgt repliceren naar Azure met Site Recovery:

- **Met behulp van de Azure Portal**: bij het implementeren van Site Recovery in de Azure Portal kunt u voor VM's een failover uitvoeren naar klassieke opslag of naar Resource Manager. [Meer informatie](site-recovery-hyper-v-site-to-azure.md).
- **Met behulp van de klassieke portal**: u kunt Site Recovery in de klassieke portal implementeren. In deze implementatie kunt u voor VM's alleen een failover uitvoeren naar klassieke opslag in Azure en niet naar Resource Manager-opslag. [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md).

De architectuur voor beide implementaties is soortgelijk, met uitzondering van het volgende:

- Als u in de Azure Portal implementeert, kunt u repliceren naar Resource Manager-opslag en de Resource Manager-netwerken gebruiken om verbinding te maken met de Azure VM's na een failover.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de Azure Portal.

U hebt het volgende nodig:

- **Azure-account**: u hebt een Microsoft Azure-account nodig.
- **Azure-opslag**: u hebt een Azure-opslagaccount nodig om gerepliceerde gegevens op te slaan. In de Azure Portal kunt u een klassiek account of een Resource Manager-opslagaccount gebruiken. U kunt alleen een klassiek account gebruiken in de klassieke portal. Gerepliceerde gegevens worden opgeslagen in Azure-opslag en bij een failover worden virtuele Azure-machines ingezet.
- **Azure-netwerk**: u hebt een Azure-netwerk nodig waarmee de virtuele Azure-machines verbinding maken wanneer ze worden gemaakt na een failover. 
- **Hyper-v-host**: u hebt een of meer Windows Server 2012 R2 Hyper-V-hostservers nodig. Tijdens de implementatie van Site Recovery installeert u de 
- **Hyper-V VMs**: u hebt een of meer virtuele machines nodig op de Hyper-V-hostserver. Azure Site Recovery-provider en de Azure Recovery Services-agent op de Hyper-V-hosts tijdens de implementatie van Site Recovery. De Provider coördineert en stuurt replicatie met de Site Recovery-service via internet. De agent handelt de gegevens voor gegevensreplicatie af via HTTPS 443. De communicatie vanuit zowel de Provider als de agent is beveiligd en versleuteld. De gerepliceerde gegevens in de Azure-opslag zijn eveneens versleuteld.

**Algemene architectuur**

![Hyper-V-site naar Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Lees meer](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) over de implementatievereisten voor Azure Portal.
- [Lees meer](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) over de implementatievereisten voor de klassieke portal.



## Repliceren naar Azure: Hyper-V VM’s beheerd door VMM

U kunt als volgt virtuele Hyper-V-machines in VMM-clouds repliceren naar Azure met Site Recovery:

- **Met behulp van de Azure Portal**: bij het implementeren van Site Recovery in de Azure Portal kunt u voor VM's een failover uitvoeren naar klassieke opslag of naar Resource Manager. [Meer informatie](site-recovery-vmm-to-azure.md).
- **Met behulp van de klassieke portal**: u kunt Site Recovery in de klassieke portal implementeren. In deze implementatie kunt u voor VM's alleen een failover uitvoeren naar klassieke opslag in Azure en niet naar Resource Manager-opslag. [Meer informatie](site-recovery-vmm-to-azure-classic.md).

De architectuur voor beide implementaties is soortgelijk, met uitzondering van het volgende:

- Als u in de Azure Portal implementeert, kunt u repliceren naar op Resource Manager gebaseerde opslag en de Resource Manager-netwerken gebruiken om verbinding te maken met de Azure VM's na een failover.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de Azure Portal.


U hebt het volgende nodig:

- **Azure-account**: u hebt een Microsoft Azure-account nodig.
- **Azure-opslag**: u hebt een Azure-opslagaccount nodig om gerepliceerde gegevens op te slaan. In de Azure Portal kunt u een klassiek account of een Resource Manager-opslagaccount gebruiken. U kunt alleen een klassiek account gebruiken in de klassieke portal. Gerepliceerde gegevens worden opgeslagen in Azure-opslag en bij een failover worden virtuele Azure-machines ingezet.
- **Azure-netwerk**: u moet netwerktoewijzing instellen zodat virtuele Azure-machines als ze worden gemaakt na een failover zijn verbonden met het juiste netwerk. 
- **VMM-server**: u hebt een of meer on-premises VMM-servers met System Center 2012 R2 nodig die zijn ingesteld met een of meer privéclouds. Als u implementeert in de Azure Portal, moet u logische en VM-netwerken instellen, zodat u netwerktoewijzing kunt configureren. In de klassieke portal is dit optioneel.  Een netwerk met virtuele machines moet zijn gekoppeld aan een logisch netwerk dat verbinding heeft met de cloud.
- **Hyper-v-host**: u hebt een of meer Windows Server 2012 R2 Hyper-V-hostservers in de VMM-cloud nodig.
- **Hyper-V VMs**: u hebt een of meer virtuele machines nodig op de Hyper-V-hostserver.

**Algemene architectuur**

![VMM naar Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Lees meer](site-recovery-vmm-to-azure.md#azure-requirements) over de implementatievereisten voor Azure Portal.
- [Lees meer](site-recovery-vmm-to-azure-classic.md#before-you-start) over de implementatievereisten voor de klassieke portal.




## Repliceren naar een secundaire site: Virtuele VMware-machines of fysieke servers 

Als u virtuele VMware-machines of fysieke servers wilt repliceren naar een secundaire site, downloadt u InMage Scout, dat is opgenomen in het Azure Site Recovery-abonnement. Dit kan worden gedownload van de Azure Portal of van de klassieke Azure Portal. 

U stelt de onderdeelservers in op elke site (configuratie, proces, hoofddoel) en installeert de Unified Agent op de machines die u wilt repliceren. Na de initiële replicatie verzendt de agent op elke machine deltareplicatiewijzigingen naar de processerver. De processerver optimaliseert de gegevens en brengt deze over naar de hoofddoelserver op de secundaire site. De configuratieserver beheert het replicatieproces.

U hebt het volgende nodig:

**Azure-account**: u implementeert dit scenario met InMage Scout. Voor het gebruik van InMage Scout hebt u een Azure-abonnement nodig. Nadat u een Site Recovery-kluis hebt gemaakt, downloadt u InMage Scout en installeert u de nieuwste updates om de implementatie in te stellen.
**Processerver (primaire site)**: stel het processerveronderdeel in op uw primaire site om caching, compressie en optimalisatie van gegevens af te handelen. Hier wordt ook de push-installatie van de Unified Agent afgehandeld naar computers die u wilt beveiligen. 
**VMware ESX/ESXi- en vCenter-server (primaire site)**: als u virtuele VMware-machines beveiligt, hebt u een VMware EXS/ESXi-hypervisor en optioneel een VMware vCenter-server nodig om hypervisors te beheren.
- **Virtuele machines/fysieke servers (primaire site)**: op virtuele VMware-machines of fysieke servers met Windows of Linux die u wilt beveiligen, moet de Unified Agent zijn geïnstalleerd. De Unified Agent is ook geïnstalleerd op de computers die als hoofddoelserver fungeren. De agent fungeert als communicatieprovider tussen alle onderdelen. 
- - **Configuratieserver (secundaire site)**: dit is het eerste onderdeel dat u installeert. De configuratieserver wordt op de secundaire site geïnstalleerd om uw implementatie te beheren, te configureren en te bewaken met behulp van de beheerwebsite of de vContinuum-console. Per implementatie is er maar één configuratieserver. Deze moet worden geïnstalleerd op een computer waarop Windows Server 2012 R2 wordt uitgevoerd.
- **vContinuum-server (secundaire site)**: deze wordt geïnstalleerd op dezelfde locatie (secundaire site) als de configuratieserver. De vContinuum-server biedt een console voor het beheren en controleren van de beveiligde omgeving. In een standaardinstallatie is de vContinuum-server de eerste hoofddoelserver. Hierop is de Unified Agent geïnstalleerd.
- **Hoofddoelserver (secundaire site)**: de hoofddoelserver bevat gerepliceerde gegevens. De hoofddoelserver ontvangt gegevens van de processerver, maakt een replicamachine op de secundaire site en bevat de gegevensretentiepunten. Het aantal hoofddoelservers dat u nodig hebt, is afhankelijk van het aantal machines dat u wilt beveiligen. Als u failback wilt uitvoeren naar de primaire site, hebt u daar ook een hoofddoelserver nodig. 

**Algemene architectuur**

![VMware naar VMware](./media/site-recovery-components/vmware-to-vmware.png)


## Repliceren naar een secundaire site: Hyper-V VM’s beheerd door VMM


U kunt Hyper-V VM’s die worden beheerd door System Center VMM als volgt repliceren naar een secundair datacenter met Site Recovery:

- **Met behulp van de Azure Portal**: bij het implementeren van Site Recovery in de Azure Portal. [Meer informatie](site-recovery-hyper-v-site-to-azure.md).
- **Met behulp van de klassieke portal**: u kunt Site Recovery in de klassieke portal implementeren. [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md).

De architectuur voor beide implementaties is soortgelijk, met uitzondering van het volgende:

- Als u in de Azure Portal implementeert, moet u netwerktoewijzing instellen. In de klassieke portal is dit optioneel.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de Azure Portal.
- - Als u in de klassieke Azure Portal implementeert, is [opslagtoewijzing](site-recovery-storage-mapping.md) beschikbaar.

U hebt het volgende nodig:

- **Azure-account**: u hebt een Microsoft Azure-account nodig.
- **VMM-server**: we adviseren een VMM-server op de primaire site en één op de secundaire site, elk met minimaal één VMM-privécloud. Op de server moet minimaal System Center 2012 SP1 met de meest recente updates worden uitgevoerd. De server moet zijn verbonden met internet. Voor clouds moet het Hyper-V-mogelijkheidsprofiel zijn ingesteld. U installeert de Azure Site Recovery-provider op de VMM-server. De Provider coördineert en stuurt replicatie met de Site Recovery-service via internet. De communicatie tussen de Provider en Azure is beveiligd en versleuteld.
- **Hyper-V-server**: er moeten Hyper-V-hostservers aanwezig zijn in de primaire en secundaire VMM-clouds. Op de hostservers moet minimaal Windows Server 2012 met de nieuwste updates zijn geïnstalleerd. De servers moeten zijn verbonden met internet. Gegevens worden tussen de primaire en secundaire Hyper-V-hostservers via het LAN of VPN gerepliceerd met behulp van Kerberos of verificatie via certificaat.  
- **Beveiligde machines**: de bron-Hyper-V-hostserver moet ten minste één virtuele machine hebben die u wilt beveiligen.

**Algemene architectuur**

![On-premises naar on-premises](./media/site-recovery-components/arch-onprem-onprem.png)


- [Lees meer](site-recovery-vmm-to-vmm.md#azure-prerequisites) over implementatievereisten in de Azure Portal.
- - [Lees meer](site-recovery-vmm-to-vmm-classic.md#before-you-start) over implementatievereisten in de klassieke Azure Portal.




## Repliceren naar een secundaire site met SAN-replicatie: Hyper-V VM’s beheerd door VMM

U kunt Hyper-V VM's die worden beheerd in VMM-clouds repliceren naar een secundaire site met behulp van SAN-replicatie via de klassieke Azure Portal. Dit scenario wordt momenteel niet ondersteund in de nieuwe Azure Portal. 

In dit scenario installeert u tijdens de implementatie van Site Recovery de Azure Site Recovery Provider op de VMM-server. De Provider coördineert en stuurt replicatie met de Site Recovery-service via internet. Gegevens worden met synchrone SAN-replicatie tussen de primaire en de secundaire opslagmatrices gerepliceerd.

U hebt het volgende nodig:

**Azure-account**: u hebt een Azure-abonnement nodig
- **SAN-matrix**: een [ondersteunde SAN-matrix](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx), beheerd door de primaire VMM-server. De SAN deelt een netwerkinfrastructuur met een andere SAN-matrix in de secundaire site.
- **VMM-server**: we adviseren een VMM-server op de primaire site en één op de secundaire site, elk met minimaal één VMM-privécloud. Op de server moet minimaal System Center 2012 SP1 met de meest recente updates worden uitgevoerd. De server moet zijn verbonden met internet. Voor clouds moet het Hyper-V-mogelijkheidsprofiel zijn ingesteld.
- **Hyper-V-server**: Hyper-V-hostservers die zich bevinden in de primaire en secundaire VMM-clouds. Op de hostservers moet minimaal Windows Server 2012 met de nieuwste updates zijn geïnstalleerd. De servers moeten zijn verbonden met internet.
- **Beveiligde machines**: de bron-Hyper-V-hostserver moet ten minste één virtuele machine hebben die u wilt beveiligen.

**SAN-replicatiearchitectuur**

![SAN-replicatie](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Lees meer](site-recovery-vmm-san.md#before-you-start) over de implementatievereisten.
### On-premises



## Levenscyclus van Hyper-V-beveiliging

In deze werkstroom ziet u het proces voor de beveiliging, replicatie en failover van virtuele Hyper-V-machines. 

1. **Beveiliging inschakelen**: u stelt de Site Recovery-kluis in, configureert replicatie-instellingen voor een VMM-cloud of Hyper-V-site en schakelt beveiliging voor virtuele machines in. Er wordt een taak met de naam **Beveiliging inschakelen** gestart. Deze kan worden bewaakt op het tabblad **Taken**. Met deze taak wordt gecontroleerd of de computer voldoet aan de vereisten en wordt de methode [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aangeroepen die replicatie naar Azure instelt op basis van de instellingen die u hebt geconfigureerd. Met de taak **Beveiliging inschakelen** wordt ook de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) aangeroepen om een volledige replicatie voor virtuele machines te initialiseren.
2. **Eerste replicatie**: er wordt een momentopname van de virtuele machine gemaakt en de virtuele harde schijven worden een voor een gerepliceerd tot ze allemaal zijn gekopieerd naar Azure of het secundaire datacenter. De tijd die nodig is om dit te voltooien, is afhankelijk van de grootte van de virtuele machine, van de netwerkbandbreedte en van de oorspronkelijke replicatiemethode. Als er tijdens de eerste replicatie schijfwijzigingen optreden, worden deze door de Hyper-V Replica Replication Tracker bijgehouden als Hyper-V-replicatielogboeken (.hrl), die zich in dezelfde map bevinden als de schijven. Elke schijf heeft een eigen HRL-bestand dat naar de secundaire opslag wordt verzonden. Houd er rekening mee dat de momentopname- en logboekbestanden schijfbronnnen in beslag nemen terwijl de eerste replicatie wordt uitgevoerd. Wanneer de eerste replicatie is voltooid, wordt de momentopname van de virtuele machine verwijderd, en worden de verschillen in het logboek gesynchroniseerd en samengevoegd.
3. **Beveiliging voltooien**: wanneer de eerste replicatie is voltooid, worden de netwerkinstellingen en de overige instellingen na de replicatie zo geconfigureerd dat de virtuele machine is beveiligd. Hiervoor wordt de taak **Beveiliging voltooien** gebruikt. Als u repliceert naar Azure, moet u mogelijk de instellingen voor de virtuele machine zo aanpassen dat deze gereed is voor failover. Nu kunt u een failovertest uitvoeren om te controleren of alles werkt zoals het hoort.
4. **Replicatie**: na de initiële replicatie start de deltasynchronisatie in overeenstemming met de replicatie-instellingen. 
    - **Replicatiefout**: als de deltareplicatie mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt er een hersynchronisatie uitgevoerd. Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie. Met een hersynchronisatie wordt de hoeveelheid verzonden gegevens geminimaliseerd door controlesommen van de bron- en virtuele machines te berekenen en alleen de verschillen (delta) te verzenden. Na hersynchronisatie wordt de deltareplicatie hervat. Hersynchronisatie is standaard zo gepland dat deze automatisch buiten kantooruren wordt uitgevoerd, maar u kunt een virtuele machine ook handmatig opnieuw synchroniseren.
    - **Replicatiefout**: als er een replicatiefout optreedt, wordt de replicatie automatisch opnieuw geprobeerd. Als het een onherstelbare fout betreft, zoals een verificatie- of autorisatiefout, of als een replicamachine een ongeldige status heeft, wordt er geen nieuwe poging gedaan. Als het een herstelbare fout betreft, zoals een netwerkfout of weinig schijfruimte/geheugen, wordt er met steeds groter wordende intervallen (1, 2, 4, 8, 10 en vervolgens elke 30 minuten) een nieuwe poging gedaan.
4. **Geplande/niet-geplande failovers**: u kunt naar behoefte geplande of niet-geplande failovers uitvoeren. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen. Nadat de replica’s van de virtuele machines zijn gemaakt, krijgen deze de status In behandeling. U moet ze doorvoeren om de failover te voltooien, tenzij u repliceert met SAN. In dat geval wordt er automatisch doorgevoerd. Wanneer de primaire site weer is hersteld, kan de failback worden uitgevoerd. Als u hebt gerepliceerd naar Azure, wordt er automatisch een omgekeerde replicatie uitgevoerd. In andere gevallen moet u de omgekeerde replicatie handmatig starten.
 

![werkstroom](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## Volgende stappen

[Implementatie voorbereiden](site-recovery-best-practices.md)



<!--HONumber=ago16_HO4-->


