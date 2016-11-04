---
title: Hoe werkt Site Recovery? | Microsoft Docs
description: In dit artikel vindt u een overzicht van de architectuur van Site Recovery
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: cfreeman
editor: ''

ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/13/2016
ms.author: raynew

---
# <a name="how-does-azure-site-recovery-work?"></a>Hoe werkt Azure Site Recovery?
Lees dit artikel om inzicht te krijgen in de architectuur van de service Azure Site Recovery en de onderdelen hiervan.

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.

## <a name="overview"></a>Overzicht
U hebt een BCDR-strategie (strategie voor bedrijfscontinuïteit en noodherstel) nodig om workloads en gegevens beschikbaar te houden tijdens geplande en ongeplande uitval en om ervoor te zorgen dat deze zo snel mogelijk worden hersteld naar een normale manier van functioneren. 

Site Recovery is een Azure-service die deel uitmaakt van uw BCDR-strategie. Er wordt een replicatie ingedeeld van on-premises fysieke servers en virtuele machines naar de cloud (Azure) of naar een secundair datacenter. Als er uitval optreedt op uw primaire locatie, schakelt u over naar de secundaire locatie om toepassingen en workloads beschikbaar te houden. U schakelt terug naar de primaire locatie wanneer deze weer normaal functioneert. Lees een overzicht in [Wat is Site Recovery?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery in de Azure-portal
Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken van en werken met resources: het Azure Resource Manager-model (ARM) en het klassieke model voor het beheren van services. Azure heeft ook twee portals: de [klassieke Azure Portal](https://manage.windowsazure.com/), die ondersteuning biedt voor het klassieke implementatiemodel, en de [Azure Portal](https://portal.azure.com), die ondersteuning biedt voor beide implementatiemodellen.

* Site Recovery is zowel beschikbaar in de klassieke portal als in de Azure-portal.
* In de klassieke Azure Portal kunt u Site Recovery ondersteunen met het klassieke model voor het beheren van services.
* In Azure Portal kunt u implementaties voor het klassieke model of het resourcemodel ondersteunen. 

De informatie in dit artikel is van toepassing op zowel klassieke als Azure-portal-implementaties. In de tabel ziet u een overzicht van de verschillen.

| **Repliceren** | **Azure Portal** | **Klassieke portal** |
| --- | --- | --- |
| **Replicatie van virtuele VMware-machines naar Azure** |Vereenvoudigd implementatieproces.<br/><br/> Failover-VM's naar klassieke opslag of Resource Manager-opslag.<br/><br/> Repliceren naar klassieke opslag of Resource Manager-opslag.<br/><br/> Gebruik een klassiek netwerk of Resource Manager-netwerk om na een failover verbinding te maken met de Azure-VM's.<br/><br/> Gebruik LRS- of GRS-opslag. |Failover naar alleen de klassieke opslag.<br/><br/> Gebruik alleen klassieke netwerken om na een failover verbinding te maken met VM's.<br/><br/> Gebruik GRS-opslag. |
| **Hyper-V VM replication (without VMM) to Azure** (Hyper-V VM-replicatie (zonder VMM) naar Azure) |Vereenvoudigd implementatieproces.<br/><br/> Failover-VM's naar klassieke opslag of Resource Manager-opslag.<br/><br/> Repliceren naar klassieke opslag of Resource Manager-opslag.<br/><br/> Gebruik een klassiek netwerk of Resource Manager-netwerk om na een failover verbinding te maken met de Azure-VM's. | |
| **Hyper-V VM replication (with VMM) to Azure** (Hyper-V VM-replicatie (met VMM) naar Azure) |Vereenvoudigd implementatieproces.<br/><br/> Failover-VM's naar klassieke opslag of Resource Manager-opslag.<br/><br/> Repliceren naar klassieke opslag of Resource Manager-opslag.<br/><br/> Gebruik een klassiek netwerk of Resource Manager-netwerk om na een failover verbinding te maken met de Azure-VM's.<br/><br/> Netwerktoewijzing moet worden ingesteld |Failover naar alleen de klassieke opslag.<br/><br/> Gebruik alleen klassieke netwerken om na een failover verbinding te maken met VM's. |
| **Hyper-V VM replication (with VMM) to secondary site** (Hyper-V VM-replicatie (met VMM) naar secundaire site) |Vereenvoudigd implementatieproces.<br/><br/> Failover-VM's naar klassieke opslag of Resource Manager-opslag.<br/><br/> Repliceren naar klassieke opslag of Resource Manager-opslag.<br/><br/> Gebruik een klassiek netwerk of Resource Manager-netwerk om na een failover verbinding te maken met de Azure-VM's.<br/><br/> Netwerktoewijzing moet worden ingesteld |Failover naar alleen de klassieke opslag.<br/><br/> Gebruik alleen klassieke netwerken om na een failover verbinding te maken met VM's.<br/><br/> U kunt de toewijzing van opslag instellen. <br/><br/> SAN-replicatie wordt niet ondersteund. |

## <a name="deployment-scenarios"></a>Implementatiescenario's
Site Recovery kan worden geïmplementeerd voor de organisatie van replicatie in een aantal scenario's:

* **Virtuele VMware-machines repliceren**: u kunt on-premises virtuele VMware-machines repliceren naar Azure-opslag of naar een secundair datacenter.
* **Fysieke machines repliceren**: u kunt fysieke computers waarop Windows of Linux wordt uitgevoerd, repliceren naar Azure-opslag of naar een secundair datacenter. Het proces voor het repliceren van fysieke computers is bijna hetzelfde als het proces voor het repliceren van virtuele VMware-machines.
* **Virtuele Hyper-V-machines repliceren**: als de Hyper-V-hosts worden beheerd in System Center VMM-clouds, kunt u de VM's repliceren naar Azure of naar een secundair datacenter. Als hosts niet worden beheerd met VMM, kunt u alleen repliceren naar Azure. U kunt virtuele Hyper-V-machines die niet door VMM worden beheerd, repliceren naar Azure-opslag.
* **Virtuele machines migreren**: met Site Recovery kunt u [virtuele Azure IaaS-machines migreren](site-recovery-migrate-azure-to-azure.md) tussen regio's of [AWS Windows-exemplaren migreren](site-recovery-migrate-aws-to-azure.md) naar virtuele Azure IaaS-machines. Volledige replicatie wordt momenteel niet ondersteund. U kunt repliceren voor migratie (failover), maar u kunt geen failback uitvoeren. 

Met Site Recovery kunt u de meeste toepassingen repliceren die op deze virtuele machines en fysieke servers worden uitgevoerd. Een volledig overzicht van de ondersteunde toepassingen vindt u in [Welke workloads kunt u met Azure Site Recovery beveiligen?](site-recovery-workload.md)

## <a name="replicate-vmware-virtual-machines-to-azure"></a>Virtuele VMware-machines repliceren naar Azure
![Verbeterd](./media/site-recovery-components/arch-enhanced.png)

| **Onderdeel** | **Details** |
| --- | --- |
| **Azure** |**Account**: u hebt een Azure-account nodig.<br/><br/> **Opslag**: u hebt een Azure-opslagaccount nodig om gerepliceerde gegevens op te slaan. U kunt een klassiek account of een Resource Manager-opslagaccount gebruiken. De account kan LRS of GRS zijn wanneer u in de Azure Portal implementeert. Gerepliceerde gegevens worden opgeslagen in Azure Storage en Azure VM's worden bij een failover ingezet.<br/><br/> **Netwerk**: u hebt een virtueel Azure-netwerk nodig waarmee de Azure-VM's verbinding maken wanneer ze na een failover worden gemaakt. In Azure Portal kunnen netwerken worden gemaakt in het klassieke model voor het beheren van services of in het Resource Manager-model. |
| **On-premises configuratieserver** |U hebt een on-premises Windows Server 2012 R2-machine nodig waarop de configuratieserver en andere onderdelen van Site Recovery worden uitgevoerd.<br/><br/> Dit moet een maximaal beschikbare virtuele VMware-machines zijn.<br/><br/> Onderdelen op de server zijn onder andere:<br/><br/> **Configuratieserver**: coördineert de communicatie tussen uw on-premises omgeving en Azure, en beheert gegevensreplicatie en herstel.<br/><br/> **Processerver**: fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens van beveiligde bronmachines, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag. Hier wordt ook de push-installatie van de Mobility-service naar beveiligde computers afgehandeld en wordt automatische detectie van virtuele VMware-machines uitgevoerd. Naarmate uw implementatie groeit, kunt u extra, afzonderlijk toegewezen processervers toevoegen om het toenemende replicatieverkeer af te handelen.<br/><br/> **Hoofddoelserver**: hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld. |
| **On-premises virtualisatieservers** |Een of meer vSphere-hosts. We raden u ook aan een vCenter-server te gebruiken om de hosts te beheren. |
| **VM's om te repliceren** |Op elke virtuele VMware-machine die u naar Azure wilt repliceren, moet de Mobility-service zijn geïnstalleerd. Deze service legt gegevens vast die naar de machine worden weggeschreven en stuurt ze door naar de processerver. Dit onderdeel kan handmatig worden geïnstalleerd, maar ook worden gepusht en vervolgens automatisch door de processerver worden geïnstalleerd wanneer u replicatie voor een machine inschakelt. |

* [Lees meer](site-recovery-vmware-to-azure.md#azure-prerequisites) over de implementatievereisten voor Azure Portal.
* [Lees meer](site-recovery-failback-azure-to-vmware.md) over failback in de Azure Portal.

## <a name="replicate-physical-servers-to-azure"></a>Fysieke servers repliceren naar Azure
![Verbeterd](./media/site-recovery-components/arch-enhanced.png)

| **Onderdeel** | **Details** |
| --- | --- |
| **Azure** |**Account**: u hebt een Azure-account nodig.<br/><br/> **Opslag**: u hebt een Azure-opslagaccount nodig om gerepliceerde gegevens op te slaan. U kunt een klassiek account of een Resource Manager-opslagaccount gebruiken. De account kan LRS of GRS zijn wanneer u in de Azure Portal implementeert. Gerepliceerde gegevens worden opgeslagen in Azure Storage en Azure VM's worden bij een failover ingezet.<br/><br/> **Netwerk**: u hebt een virtueel Azure-netwerk nodig waarmee de Azure-VM's verbinding maken wanneer ze na een failover worden gemaakt. In Azure Portal kunnen netwerken worden gemaakt in het klassieke model voor het beheren van services of in het Resource Manager-model. |
| **On-premises configuratieserver** |U hebt een on-premises Windows Server 2012 R2-machine nodig waarop de configuratieserver en andere onderdelen van Site Recovery worden uitgevoerd.<br/><br/> De server kan virtueel of fysiek zijn.<br/><br/> Onderdelen op de server zijn onder andere:<br/><br/> **Configuratieserver**: coördineert de communicatie tussen uw on-premises omgeving en Azure, en beheert gegevensreplicatie en herstel.<br/><br/> **Processerver**: fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens van beveiligde bronmachines, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag. Hier wordt ook de push-installatie van de Mobility-service naar beveiligde computers afgehandeld en wordt automatische detectie van virtuele VMware-machines uitgevoerd. Naarmate uw implementatie groeit, kunt u extra, afzonderlijk toegewezen processervers toevoegen om het toenemende replicatieverkeer af te handelen.<br/><br/> **Hoofddoelserver**: hier worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld. |
| **Machines om te repliceren** |Op elke fysieke Windows-/Linux-computer die u naar Azure wilt repliceren, moet de Mobility-service zijn geïnstalleerd. Deze service legt gegevens vast die naar de machine worden weggeschreven, en stuurt ze door naar de processerver. Dit onderdeel kan handmatig worden geïnstalleerd, maar ook worden gepusht en vervolgens automatisch door de processerver worden geïnstalleerd wanneer u replicatie voor een machine inschakelt. |
| **Failback** |Failback van fysiek-naar-fysiek wordt niet ondersteund. Dit betekent dat als u voor fysieke servers een failover naar Azure uitvoert en vervolgens een failback wilt uitvoeren, u een failback moet uitvoeren naar een virtuele VMware-machine. U kunt geen failback uitvoeren naar een fysieke server. U hebt een Azure-VM nodig om een failback naar uit te voeren. Als u de configuratieserver niet als een virtuele VMware-machine hebt geïmplementeerd, moet u een afzonderlijke hoofddoelserver instellen als een virtuele VMware-machine. Dit is nodig omdat de hoofddoelserver communiceert met en is gekoppeld aan VMware-opslag om de schijven te herstellen naar een virtuele VMware-machine. |

* [Lees meer](site-recovery-vmware-to-azure.md#azure-prerequisites) over de implementatievereisten voor Azure Portal.
* [Lees meer](site-recovery-failback-azure-to-vmware.md) over failback in de Azure Portal.

## <a name="replicate-hyper-v-vms-not-managed-by-vmm-to-azure"></a>Virtuele Hyper-V-machines die niet worden beheerd met VMM, repliceren naar Azure
![Hyper-V-site naar Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

| **Onderdeel** | **Details** |
| --- | --- |
| **Azure** |**Account**: u hebt een Azure-account nodig.<br/><br/> **Opslag**: u hebt een Azure-opslagaccount nodig om gerepliceerde gegevens op te slaan. U kunt een klassiek account of een Resource Manager-opslagaccount gebruiken. Het account moet GRS zijn. Gerepliceerde gegevens worden opgeslagen in Azure Storage en Azure VM's worden bij een failover ingezet.<br/><br/> **Netwerk**: u hebt een virtueel Azure-netwerk nodig waarmee de Azure-VM's verbinding maken wanneer ze na een failover worden gemaakt. |
| **Hosts/VM's van Hyper-V** |Een of meer Hyper-V-hosts waarop een of meer VM's worden uitgevoerd.<br/><br/> De Site Recovery Provider en Recovery Services-agent zijn tijdens de implementatie op elke host geïnstalleerd. |

* [Lees meer](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) over de implementatievereisten voor Azure Portal.
* [Lees meer](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) over de implementatievereisten voor de klassieke portal.

## <a name="replicate-hyper-v-vms-managed-by-vmm-to-azure"></a>Virtuele Hyper-V-machines die worden beheerd met VMM, repliceren naar Azure
![VMM naar Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

| **Onderdeel** | **Details** |
| --- | --- |
| **Azure** |**Account**: u hebt een Azure-account nodig.<br/><br/> **Opslag**: u hebt een Azure-opslagaccount nodig om gerepliceerde gegevens op te slaan. U kunt een klassiek account of een Resource Manager-opslagaccount gebruiken. Het account moet GRS zijn. Gerepliceerde gegevens worden opgeslagen in Azure Storage en Azure VM's worden bij een failover ingezet.<br/><br/> **Netwerk**: u hebt een virtueel Azure-netwerk nodig waarmee de Azure-VM's verbinding maken wanneer ze na een failover worden gemaakt. |
| **VMM-server** |U hebt een of meer on-premises VMM-servers nodig, met een of meer privéclouds. De Site Recovery Provider is tijdens de implementatie op elke server geïnstalleerd. |
| **Hosts/VM's van Hyper-V** |Een of meer Hyper-V-hosts waarop een of meer virtuele machines worden uitgevoerd.<br/><br/> De Recovery Services-agent is tijdens de implementatie op elke host geïnstalleerd. |

* [Lees meer](site-recovery-vmm-to-azure.md#azure-requirements) over de implementatievereisten voor Azure Portal.
* [Lees meer](site-recovery-vmm-to-azure-classic.md#before-you-start) over de implementatievereisten voor de klassieke portal.

## <a name="replicate-vmware-virtual-machines-or-physical-server-to-a-secondary-site"></a>Virtuele machines of fysieke servers van VMware repliceren naar een secundaire site
![VMware naar VMware](./media/site-recovery-components/vmware-to-vmware.png)

| **Onderdeel** | **Details** |
| --- | --- |
| **Azure** |**Account**: u implementeert dit scenario met InMage Scout. Voor het gebruik hiervan hebt u een Azure-account nodig.<br/><br/> Nadat u een Site Recovery-kluis hebt gemaakt, downloadt u InMage Scout en installeert u de nieuwste updates om de implementatie in te stellen. |
| **Primaire site** |**Processerver**: stel het processerveronderdeel in op uw primaire site om caching, compressie en optimalisatie van gegevens af te handelen. Hier wordt ook de push-installatie van de Unified Agent afgehandeld naar computers die u wilt beveiligen.<br/><br/> **VMware ESX/ESXi en vCenter**: u hebt een VMware EXS/ESXi -hypervisor nodig en optioneel ook een VMware vCenter-server.<br/><br/> **VM's** |
| **Secundaire site** |**Configuratieserver**: dit is het eerste onderdeel dat u installeert. De configuratieserver wordt op de secundaire site geïnstalleerd om uw implementatie te beheren, te configureren en te bewaken met behulp van de beheerwebsite of de vContinuum-console. Per implementatie is er maar één configuratieserver. Deze moet worden geïnstalleerd op een computer waarop Windows Server 2012 R2 wordt uitgevoerd.<br/><br/> **vContinuum-server (secundaire site)**: deze wordt geïnstalleerd op dezelfde locatie als de configuratieserver. De vContinuum-server biedt een console voor het beheren en controleren van de beveiligde omgeving. In een standaardinstallatie is de vContinuum-server de eerste hoofddoelserver. Hierop is de Unified Agent geïnstalleerd.<br/><br/> **Hoofddoelserver**: de hoofddoelserver bevat gerepliceerde gegevens. De hoofddoelserver ontvangt gegevens van de processerver, maakt een replicamachine op de secundaire site en bevat de gegevensretentiepunten. Het aantal hoofddoelservers dat u nodig hebt, is afhankelijk van het aantal machines dat u wilt beveiligen. Als u failback wilt uitvoeren naar de primaire site, hebt u daar ook een hoofddoelserver nodig. |

Als u virtuele VMware-machines of fysieke servers wilt repliceren naar een secundaire site, downloadt u InMage Scout. Dit is opgenomen in het Azure Site Recovery-abonnement. Het kan worden gedownload van Azure Portal of van de klassieke Azure Portal.

U stelt de onderdeelservers in op elke site (configuratie, proces, hoofddoel) en installeert de Unified Agent op de machines die u wilt repliceren. Na de initiële replicatie verzendt de agent op elke machine deltareplicatiewijzigingen naar de processerver. De processerver optimaliseert de gegevens en brengt deze over naar de hoofddoelserver op de secundaire site. De configuratieserver beheert het replicatieproces.

## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site"></a>Virtuele Hyper-V-machines die worden beheerd met VMM, repliceren naar een secundaire site
![On-premises naar on-premises](./media/site-recovery-components/arch-onprem-onprem.png)

| **Onderdeel** | **Details** |
| --- | --- |
| **Azure** |**Account**: u hebt een Azure-account nodig. |
| **VMM-server** |Aanbevolen is een VMM-beheerserver op de primaire site en een op de secundaire site. Elke server moet een of meer privéclouds hebben.<br/><br/> Tijdens de implementatie installeert u de Azure Site Recovery-provider op de VMM-server. |
| **Hosts/VM's van Hyper-V** |Een of meer Hyper-V-hosts die worden uitgevoerd in de VMM-clouds op de primaire en secundaire site<br/><br/> Elke host moet een of meer virtuele machines repliceren.<br/><br/>. De Recovery Services-agent is tijdens de implementatie op elke host geïnstalleerd. |

* [Lees meer](site-recovery-vmm-to-vmm.md#azure-prerequisites) over implementatievereisten in de Azure Portal.
* [Lees meer](site-recovery-vmm-to-vmm-classic.md#before-you-start) over implementatievereisten in de klassieke Azure Portal.

## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site-using-san-replication"></a>Virtuele Hyper-V-machines die worden beheerd met VMM, repliceren naar een secundaire site met behulp van SAN-replicatie
![SAN-replicatie](./media/site-recovery-components/arch-onprem-onprem-san.png)

U kunt virtuele Hyper-V-machines die worden beheerd in VMM-clouds, repliceren naar een secundaire site met behulp van SAN-replicatie via de klassieke portal. Dit scenario wordt momenteel niet ondersteund in Azure Portal.

| **Onderdeel** | **Details** |
| --- | --- |
| **Azure** |**Account**: u hebt een Azure-account nodig. |
| **VMM-server** |Aanbevolen is een VMM-beheerserver op de primaire site en een op de secundaire site. Elke server moet een of meer privéclouds hebben.<br/><br/> Tijdens de implementatie installeert u de Azure Site Recovery-provider op de VMM-server. |
| **SAN** |Een ondersteunde SAN-matrix beheerd door de primaire VMM-server.<br/><br/> De SAN moet een netwerkinfrastructuur delen met een andere SAN-matrix in de secundaire site. |
| **Hosts/VM's van Hyper-V** |Een of meer Hyper-V-hosts die worden uitgevoerd in de VMM-clouds op de primaire en secundaire site<br/><br/> Elke host moet een of meer virtuele machines repliceren.<br/><br/>. De Recovery Services-agent is tijdens de implementatie op elke host geïnstalleerd. |

In dit scenario installeert u tijdens de implementatie van Site Recovery de Azure Site Recovery Provider op de VMM-server. De Provider coördineert en stuurt replicatie met de Site Recovery-service via internet. Gegevens worden met synchrone SAN-replicatie tussen de primaire en de secundaire opslagmatrices gerepliceerd.

[Lees meer](site-recovery-vmm-san.md#before-you-start) over de implementatievereisten.

## <a name="hyper-v-protection-lifecycle"></a>Levenscyclus van Hyper-V-beveiliging
In deze werkstroom ziet u het proces voor de beveiliging, replicatie en failover van virtuele Hyper-V-machines.

1. **Beveiliging inschakelen**: u stelt de Site Recovery-kluis in, configureert replicatie-instellingen voor een VMM-cloud of Hyper-V-site en schakelt beveiliging voor virtuele machines in. Er wordt een taak met de naam **Beveiliging inschakelen** gestart. Deze kan worden bewaakt op het tabblad **Taken**. Met deze taak wordt gecontroleerd of de computer voldoet aan de vereisten en wordt de methode [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aangeroepen die replicatie naar Azure instelt op basis van de instellingen die u hebt geconfigureerd. Met de taak **Beveiliging inschakelen** wordt ook de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) aangeroepen om een volledige replicatie voor virtuele machines te initialiseren.
2. **Eerste replicatie**: er wordt een momentopname van de virtuele machine gemaakt en de virtuele harde schijven worden een voor een gerepliceerd tot ze allemaal zijn gekopieerd naar Azure of naar het secundaire datacenter. De tijd die nodig is om dit te voltooien, is afhankelijk van de grootte van de virtuele machine, van de netwerkbandbreedte en van de oorspronkelijke replicatiemethode. Als er tijdens de eerste replicatie schijfwijzigingen optreden, worden deze met de Hyper-V Replica-replicatietracker bijgehouden als Hyper-V-replicatielogboeken (.hrl), die zich in dezelfde map bevinden als de schijven. Elke schijf heeft een eigen HRL-bestand dat naar de secundaire opslag wordt verzonden. Houd er rekening mee dat de momentopname- en logboekbestanden schijfbronnnen in beslag nemen terwijl de eerste replicatie wordt uitgevoerd. Wanneer de eerste replicatie is voltooid, wordt de momentopname van de virtuele machine verwijderd, en worden de verschillen in het logboek gesynchroniseerd en samengevoegd.
3. **Beveiliging voltooien**: wanneer de eerste replicatie is voltooid, worden de netwerkinstellingen en de overige instellingen na de replicatie zo geconfigureerd dat de virtuele machine is beveiligd. Hiervoor wordt de taak **Beveiliging voltooien** gebruikt. Als u repliceert naar Azure, moet u mogelijk de instellingen voor de virtuele machine zo aanpassen dat deze gereed is voor failover. Nu kunt u een failovertest uitvoeren om te controleren of alles werkt zoals het hoort.
4. **Replicatie**: na de initiële replicatie start de deltasynchronisatie in overeenstemming met de replicatie-instellingen.
   * **Replicatiefout**: als de deltareplicatie mislukt en een volledige replicatie veel bandbreedte of tijd zou kosten, wordt er een hersynchronisatie uitgevoerd. Als de HRL-bestanden bijvoorbeeld 50% van de schijfgrootte in beslag nemen, wordt de virtuele machine gemarkeerd voor hersynchronisatie. Met een hersynchronisatie wordt de hoeveelheid verzonden gegevens geminimaliseerd door controlesommen van de bron- en virtuele machines te berekenen en alleen de verschillen (delta) te verzenden. Na hersynchronisatie wordt de deltareplicatie hervat. Hersynchronisatie is standaard zo gepland dat deze automatisch buiten kantooruren wordt uitgevoerd, maar u kunt een virtuele machine ook handmatig opnieuw synchroniseren.
   * **Replicatiefout**: als er een replicatiefout optreedt, wordt de replicatie automatisch opnieuw geprobeerd. Als het een onherstelbare fout betreft, zoals een verificatie- of autorisatiefout, of als een replicamachine een ongeldige status heeft, wordt er geen nieuwe poging gedaan. Als het een herstelbare fout betreft, zoals een netwerkfout of weinig schijfruimte/geheugen, wordt er met steeds groter wordende intervallen (1, 2, 4, 8, 10 en vervolgens elke 30 minuten) een nieuwe poging gedaan.
5. **Geplande/niet-geplande failovers**: u kunt naar behoefte geplande of niet-geplande failovers uitvoeren. Als u een geplande failover uitvoert, worden de virtuele bronmachines afgesloten om gegevensverlies te voorkomen. Nadat de replica’s van de virtuele machines zijn gemaakt, krijgen deze de status In behandeling. U moet ze doorvoeren om de failover te voltooien, tenzij u repliceert met SAN. In dat geval wordt er automatisch doorgevoerd. Wanneer de primaire site weer is hersteld, kan de failback worden uitgevoerd. Als u hebt gerepliceerd naar Azure, wordt er automatisch een omgekeerde replicatie uitgevoerd. In andere gevallen moet u de omgekeerde replicatie handmatig starten.

![werkstroom](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Volgende stappen
[Implementatie voorbereiden](site-recovery-best-practices.md)

<!--HONumber=Oct16_HO3-->


