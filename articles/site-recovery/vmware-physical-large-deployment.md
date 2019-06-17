---
title: Herstel na noodgevallen naar Azure voor grote aantallen virtuele VMware-machines of fysieke servers met Azure Site Recovery instellen | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen naar Azure voor grote aantallen on-premises VMware-machines of fysieke servers met Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: raynew
ms.openlocfilehash: e96aafe61c0d8547ffca9e97bfd9e90c9529155f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237264"
---
# <a name="set-up-disaster-recovery-at-scale-for-vmware-vmsphysical-servers"></a>Herstel na noodgeval op schaal voor VMware-machines/fysieke servers instellen

In dit artikel wordt beschreven hoe u herstel na noodgevallen naar Azure voor grote aantallen (> 1000) on-premises VMware-machines of fysieke servers in uw productieomgeving instellen met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="define-your-bcdr-strategy"></a>Uw BCDR-strategie definiëren

Als onderdeel van uw zakelijke continuïteit en noodherstelplan (BCDR) definieert u herstelpuntdoelen (RPO's) en hersteltijddoelen (RTO's) voor uw zakelijke apps en workloads. RTO meet de duur van de tijd en service niveau waarin een business-app of een proces moet worden teruggezet en beschikbaar, om te voorkomen dat problemen van de bedrijfscontinuïteit te waarborgen.
- Site Recovery biedt continue replicatie voor VMware-machines en fysieke servers en een [SLA](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) voor RTO.
- Als u van plan voor grootschalige noodherstel voor VMware-VM's en het uitzoeken van de Azure-resources die u nodig hebt bent, kunt u een RTO-waarde die wordt gebruikt voor capaciteitsberekeningen.


## <a name="best-practices"></a>Aanbevolen procedures

Sommige algemene aanbevolen procedures voor grootschalige noodherstel. Deze aanbevolen procedures worden besproken in de volgende secties van het document in meer detail.

- **Doel-vereisten identificeren**: Een schatting van de behoeften van capaciteit en resources in Azure voordat u herstel na noodgevallen instelt.
- **Plannen voor Site Recovery-onderdelen**: Nagaan welke onderdelen van Site Recovery (configuratieserver, processervers) die u nodig hebt om te voldoen aan de capaciteit van de geschatte.
- **Instellen van een of meer scale-out processervers**: Gebruik niet de processerver die standaard wordt uitgevoerd op de configuratieserver. 
- **Uitvoeren van de meest recente updates**: De Site Recovery-team vrijgegeven nieuwe versies van Site Recovery-onderdelen op gezette tijden en moet u controleren of dat u nu de meest recente versies worden uitgevoerd. Om u te helpen bij die, bijhouden [wat is er nieuw](site-recovery-whats-new.md) voor updates en [inschakelen en het installeren van updates](service-updates-how-to.md) als ze beschikbaar zijn.
- **Proactief bewaken**: Als u herstel na noodgevallen ingesteld en geactiveerd, moet u de status en integriteit van gerepliceerde virtuele machines en infrastructuurresources proactief bewaken.
- **Analyses voor noodherstel**: U moet noodhersteloefeningen uitvoeren op gezette tijden. Dit geen invloed op uw productieomgeving, maar ervoor te zorgen dat de failover naar Azure, werken zoals verwacht wanneer dat nodig is.



## <a name="gather-capacity-planning-information"></a>Verzamelen van gegevens over capaciteitsplanning

Gegevens verzamelen over uw on-premises omgeving, om te beoordelen en maak een schatting van dat de capaciteit van uw doel (Azure) nodig.
- Voor VMware, uitvoeren van de Deployment Planner voor VMware-VM's om dit te doen.
- Verzamel de informatie handmatig voor fysieke servers.

### <a name="run-the-deployment-planner-for-vmware-vms"></a>De Implementatieplanner voor VMware-VM's uitvoeren

De Deployment Planner kunt u gegevens verzamelen over uw VMware on-premises omgeving.

- De Implementatieplanner uitvoeren tijdens een periode die typisch gegevensverloop voor uw virtuele machines vertegenwoordigt. Hiermee wordt meer nauwkeurige schattingen en aanbevelingen gegenereerd.
- Het is raadzaam dat u de Deployment Planner op de server configureren, uitvoeren omdat de Planner berekent de doorvoer van de server waarop deze wordt uitgevoerd. [Meer informatie](site-recovery-vmware-deployment-planner-run.md#get-throughput) over het meten van doorvoer.
- Als u geen nog een configuratieserver instellen:
    - [Bekijk een overzicht](vmware-physical-azure-config-process-server-overview.md) van Site Recovery-onderdelen.
    - [Instellen van een configuratieserver](vmware-azure-deploy-configuration-server.md), om de Deployment Planner erop worden uitgevoerd.

Voer vervolgens de Planner als volgt uit:

1. [Meer informatie over](site-recovery-deployment-planner.md) de Deployment Planner. U kunt de meest recente versie downloaden vanuit de portal of [rechtstreeks downloaden](https://aka.ms/asr-deployment-planner).
2. Controleer de [vereisten](site-recovery-deployment-planner.md#prerequisites) en [meest recente updates](site-recovery-deployment-planner-history.md) voor de Implementatieplanner en [downloaden en uitpakken](site-recovery-deployment-planner.md#download-and-extract-the-deployment-planner-tool) het hulpprogramma.
3. [De Implementatieplanner uitvoeren](site-recovery-vmware-deployment-planner-run.md) op de configuratieserver.
4. [Een rapport genereren](site-recovery-vmware-deployment-planner-run.md#generate-report) om samen te vatten schattingen en aanbevelingen.
5. Analyseer de [rapport aanbevelingen](site-recovery-vmware-deployment-planner-analyze-report.md) en [kosten schattingen](site-recovery-vmware-deployment-planner-cost-estimation.md).

>[!NOTE]
> Standaard wordt met het hulpprogramma is geconfigureerd voor het profiel en wordt rapport gegenereerd voor maximaal 1000 virtuele machines. U kunt deze limiet wijzigen door de sleutelwaarde maxvmssupported in het bestand ASRDeploymentPlanner.exe.config te verhogen.

## <a name="plan-target-azure-requirements-and-capacity"></a>Doel (Azure)-vereisten en capaciteit plannen

Met behulp van de verzamelde schattingen en aanbevelingen, kunt u plannen voor de doelresources en capaciteit. Als u de Deployment Planner voor VMware-VM's uitgevoerd, kunt u een aantal van de [rapport aanbevelingen](site-recovery-vmware-deployment-planner-analyze-report.md#recommendations) om u te helpen.

- **Compatibele virtuele machines**: Dit nummer gebruiken om het aantal virtuele machines die gereed voor herstel na noodgevallen naar Azure zijn te identificeren. Aanbevelingen over de bandbreedte van het netwerk en Azure-kerngeheugens zijn gebaseerd op dit nummer.
- **Vereiste netwerkbandbreedte**: Houd er rekening mee de bandbreedte die u nodig hebt voor replicatie van verschillen van compatibele virtuele machines. 
    - Wanneer u de Planner uitvoeren geeft u de gewenste RPO in minuten. De aanbevelingen ziet u de bandbreedte die nodig zijn om te voldoen aan deze RPO 100% en 90% van de tijd. 
    - De aanbevelingen voor netwerkbandbreedte rekening mee dat de bandbreedte die nodig zijn voor het totale aantal configuratieservers en processervers aanbevolen in de Planner.
- **Azure-kerngeheugens vereist**: Opmerking het aantal kernen u in de doel-Azure-regio moet, gebaseerd op het aantal compatibele virtuele machines. Als u geen onvoldoende cores, na een failover Site Recovery niet mogelijk om te maken van de vereiste Azure-VM's.
- **Aanbevolen VM-batchgrootte**: De aanbevolen batchgrootte is gebaseerd op de mogelijkheid om een geslaagde initiële replicatie voor de batch binnen 72 uur standaard aan de wensen van een RPO van 100%. De uurwaarde kan worden gewijzigd.

U kunt deze aanbevelingen gebruiken om te plannen voor Azure-resources, netwerkbandbreedte en batchverwerking van de virtuele machine.

## <a name="plan-azure-subscriptions-and-quotas"></a>Azure-abonnementen en quota's plannen

We willen ervoor zorgen dat de beschikbare quota's in het doelabonnement voldoende zijn zijn voor de failover af te handelen.

**Taak** | **Details** | **Actie**
--- | --- | ---
**Selectievakje kernen** | Als cores in het beschikbare quotum niet gelijk zijn aan of groter zijn dan het aantal totale doel op het moment van failover, mislukt de failover. | Voor virtuele VMware-machines, Controleer dat u hebt onvoldoende cores in het doelabonnement om te voldoen aan de aanbevelingen van Deployment Planner core.<br/><br/> Controleer of Azure-kerngeheugens, voldoen aan uw handmatige schattingen voor fysieke servers.<br/><br/> Om te controleren of de quota's in Azure portal > **abonnement**, klikt u op **gebruik + quota**.<br/><br/> [Meer informatie](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) over het verhogen van quota.
**Controleer de failover-limieten** | Het aantal failovers kan niet groter zijn dan de limieten van Site Recovery-failover. |  Als failover de grenzen overschrijdt, kunt u abonnementen, toevoegen en fungeren als failover voor meerdere abonnementen of verhoog het quotum voor een abonnement. 


### <a name="failover-limits"></a>Failover-limieten

De limieten geeft het aantal failovers die worden ondersteund door siteherstel binnen een uur, uitgaande van de drie schijven per machine.

Wat betekenen voldoet? Voor het starten van een virtuele machine van Azure, Azure vereist dat sommige stuurprogramma's in de toestand bij het opstarten start, en services zoals DHCP moet worden ingesteld op automatisch starten.
- Computers die voldoen hebt al deze instellingen in plaats.
- Voor machines waarop Windows wordt uitgevoerd, kunt u proactief controleren op naleving en zodat ze voldoen aan het beleid zo nodig. [Meer informatie](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010).
- Linux-machines worden alleen weer in overeenstemming is op het moment van failover.

**Machine voldoet aan de Azure?** | **Azure VM-limieten (failover voor beheerde schijf)**
--- | --- 
Ja | 2000
Nee | 1000

- Limieten wordt ervan uitgegaan dat minimale dat andere taken worden uitgevoerd in de doelregio voor het abonnement.
- Sommige Azure-regio's zijn kleiner en er iets lagere limieten.

## <a name="plan-infrastructure-and-vm-connectivity"></a>Infrastructuur- en VM-connectiviteit plannen

Na een failover naar Azure moet u uw workloads om te werken als on-premises, en waarmee gebruikers toegang krijgen tot workloads die worden uitgevoerd op de Azure VM's.

- [Meer informatie](site-recovery-active-directory.md#test-failover-considerations) over failover van uw on-premises Active Directory of DNS-infrastructuur naar Azure.
- [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) over het maken van verbinding maken met virtuele Azure-machines na een failover.



## <a name="plan-for-source-capacity-and-requirements"></a>Plan voor de broncomputer en vereisten

Het is belangrijk dat u hebt voldoende configuratieservers en processervers dat scale-out moet voldoen aan de vereisten voor de opslagcapaciteit. Als u uw grootschalige implementatie begint, kunt u beginnen met een configuratie voor één server en een enkele uitbreidbare processerver. Als u de voorgeschreven limiet hebt bereikt, moet u extra servers toevoegen.

>[!NOTE]
> Voor virtuele VMware-machines kunt de Deployment Planner u enkele aanbevelingen over de configuratie en de proces-servers die u nodig hebt. U wordt aangeraden dat u de tabellen die zijn opgenomen in de volgende procedures, in plaats van de Deployment Planner-aanbeveling te volgen. 


## <a name="set-up-a-configuration-server"></a>Een configuratieserver instellen
 
Capaciteit van configuratie-server wordt beïnvloed door het aantal machines die worden gerepliceerd, en niet door gegevens verloop tarief. Als u wilt weten of u aanvullende configuratie-servers nodig hebt, gebruikt u deze gedefinieerde VM-limieten.

**CPU** | **Geheugen** | **Cacheschijf** | **Limiet voor computer gerepliceerd**
 --- | --- | --- | ---
8 vcpu 's<br> 2-sockets * 4 kernen @ 2,5 Ghz | 16 GB | 600 TB | 550 machines<br> Wordt ervan uitgegaan dat elke machine drie schijven van 100 GB elk heeft.

- Deze limieten zijn gebaseerd op een configuratieserver instellen met behulp van een OVF-sjabloon.
- De limieten wordt ervan uitgegaan dat u niet de processerver die standaard wordt uitgevoerd op de configuratieserver gebruikt.

Als u wilt toevoegen van een nieuwe configuratieserver, volgt u deze instructies:

- [Instellen van een configuratieserver](vmware-azure-deploy-configuration-server.md) voor herstel na noodgevallen VMware-VM met behulp van een OVF-sjabloon.
- [Instellen van een configuratieserver](physical-azure-set-up-source.md) handmatig voor fysieke servers, of voor VMware-implementaties die een OVF-sjabloon niet kunnen gebruiken.

Bij het instellen van een configuratieserver, houd er rekening mee dat:

- Bij het instellen van een configuratieserver, is het belangrijk dat u rekening houden met het abonnement en de kluis waarin deze zich bevindt, omdat deze al dan niet mogen worden gewijzigd na de installatie. Als u wijzigen van de kluis wilt, hebt u loskoppelen van de configuratieserver in de kluis en deze opnieuw te registreren. Hierdoor wordt voorkomen dat de replicatie van virtuele machines in de kluis.
- Als u wilt voor het instellen van een configuratieserver met meerdere netwerkadapters, moet u dit doen tijdens het instellen van. U kunt dit niet doen na het registreren van de configuratieserver in de kluis.

## <a name="set-up-a-process-server"></a>Een processerver instellen

Proces-servercapaciteit wordt beïnvloed door gegevenstarieven verloop, en niet door het aantal machines die zijn ingeschakeld voor replicatie.

- Voor grote implementaties moet u altijd ten minste één uitbreidbare processerver hebben.
- Als u wilt weten of u extra servers nodig hebt, gebruik de volgende tabel.
- We raden u aan een server met de hoogste specificaties. 


**CPU** | **Geheugen** | **Cacheschijf** | **Tarief verloop**
 --- | --- | --- | --- 
12 vcpu 's<br> 2-sockets * 6 kernen @ 2,5 Ghz | 24 GB | 1 GB | Tot 2 TB per dag

Stel de processerver als volgt in:

1. Controleer de [vereisten](vmware-azure-set-up-process-server-scale.md#prerequisites).
2. Installeren van de server in de [portal](vmware-azure-set-up-process-server-scale.md#install-from-the-ui), of vanuit de [vanaf de opdrachtregel](vmware-azure-set-up-process-server-scale.md#install-from-the-command-line).
3. Gerepliceerde machines voor het gebruik van de nieuwe server configureren. Als u al machines die worden gerepliceerd hebt:
    - U kunt [verplaatsen](vmware-azure-manage-process-server.md#switch-an-entire-workload-to-another-process-server) de serverwerkbelasting van een hele proces naar de nieuwe processerver.
    - U kunt ook [verplaatsen](vmware-azure-manage-process-server.md#move-vms-to-balance-the-process-server-load) specifieke virtuele machines naar de nieuwe processerver.



## <a name="enable-large-scale-replication"></a>Grootschalige replicatie inschakelen

Na het plannen van capaciteit en de vereiste onderdelen en de infrastructuur implementeren, schakelt u replicatie voor grote aantallen virtuele machines.

1. Machines voor het sorteren in batches. U schakelt replicatie voor virtuele machines binnen een batch- en gaat u naar de volgende batch.

    - Voor virtuele VMware-machines, kunt u de [aanbevolen VM-batchgrootte](site-recovery-vmware-deployment-planner-analyze-report.md#recommended-vm-batch-size-for-initial-replication) in de Deployment Planner-rapport.
    - Voor fysieke computers raden wij dat u identificeren op basis van machines waarvoor een vergelijkbare grootte en de hoeveelheid gegevens en op de beschikbare netwerkdoorvoer batches. Het doel is het batch-machines die waarschijnlijk de initiële replicatie in rond de dezelfde hoeveelheid tijd voltooien.
    
2. Als het verloop van de schijf voor een virtuele machine is hoog of overschrijdt de grenzen in implementatie thePlanner, kunt u niet-kritieke bestanden die u niet nodig hebt voor het repliceren van de machine uit (zoals log dumpbestanden voor foutopsporing of tijdelijke bestanden) verplaatsen. Voor virtuele VMware-machines, kunt u deze bestanden verplaatsen naar een afzonderlijke schijf, en vervolgens [uitsluiten van die schijf](vmware-azure-exclude-disk.md) van replicatie.
3. Voordat u replicatie inschakelt, moet u controleren die voldoen aan machines [replicatievereisten](vmware-physical-azure-support-matrix.md#replicated-machines).
4. Configureren van een replicatiebeleid voor [virtuele VMware-machines](vmware-azure-set-up-replication.md#create-a-policy) of [fysieke servers](physical-azure-disaster-recovery.md#create-a-replication-policy).
5. Replicatie inschakelen voor [virtuele VMware-machines](vmware-azure-enable-replication.md) of [fysieke servers](physical-azure-disaster-recovery.md#enable-replication). Deze begint de initiële replicatie voor de geselecteerde computers.

## <a name="monitor-your-deployment"></a>Uw implementatie bewaken

Nadat u een vliegende start replicatie voor de eerste batch VM's, start u de bewaking van uw implementatie als volgt:  

1. Een beheerder van het herstel na noodgevallen voor het bewaken van de status van gerepliceerde machines toewijzen.
2. [Gebeurtenissen controleren](site-recovery-monitor-and-troubleshoot.md) voor gerepliceerde items en de infrastructuur.
3. [Controleer de status](vmware-physical-azure-monitor-process-server.md) van uw scale-out processervers.
4. Zich registreren om op te halen [e-mailmeldingen](https://docs.microsoft.com/azure/site-recovery/site-recovery-monitor-and-troubleshoot#subscribe-to-email-notifications) voor gebeurtenissen, bewaking vergemakkelijken.
5. Normaal voeren [noodhersteloefeningen](site-recovery-test-failover-to-azure.md), om ervoor te zorgen dat alles werkt zoals verwacht.


## <a name="plan-for-large-scale-failovers"></a>Plan voor grootschalige failovers

In een gebeurtenis van na noodgevallen moet u mogelijk failover van een groot aantal machines/workloads naar Azure. Bereid als volgt voor dit type gebeurtenis.

U kunt voorbereiden vooraf voor failover als volgt:

- [Voorbereiden van uw infrastructuur en virtuele machines](#plan-infrastructure-and-vm-connectivity) zodat uw workloads na een failover beschikbaar en zodat gebruikers toegang hebben tot de Azure VM's.
- Houd er rekening mee de [failover limieten](#failover-limits) eerder in dit document. Zorg ervoor dat uw failovers valt binnen deze limieten.
- Voer regelmatig [noodhersteloefeningen](site-recovery-test-failover-to-azure.md). Zoomt u in de help voor:
    - Hiaten in uw implementatie voordat de failover.
    - Maak een schatting van de RTO end-to-end voor uw apps.
    - Maak een schatting van de RPO end-to-end voor uw workloads.
    - IP-adres adresbereik veroorzaakt een conflict identificeren.
    - Als u oefeningen uitvoert, wordt aangeraden dat u geen productienetwerken om de oefeningen gebruiken, Vermijd het gebruik van de dezelfde namen van subnetten in productie en testen netwerken en testfailovers na elke inzoomen opschonen.

Als u wilt een grootschalige failover uitvoert, raden we het volgende:

1. Herstelplannen maken voor failover van werkbelastingen.
    - Elk plan voor herstel kunt failover van maximaal 50 machines activeren.
    - [Meer informatie](recovery-plan-overview.md) over plannen voor herstel.
2. Azure Automation runbook scripts toevoegen aan herstelplannen, om een handmatige taken op Azure te automatiseren. Typische taken omvatten het configureren van load balancers, bijwerken DNS enzovoort. [Meer informatie](site-recovery-runbook-automation.md)
2. Bereid voordat de failover, Windows-machines zodat deze aan de Azure-omgeving voldoen. [Failover-limieten](#plan-azure-subscriptions-and-quotas) hoger voor machines die in overeenstemming zijn. [Meer informatie](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010) over runbooks.
4.  Failover met activeren de [Start AzRecoveryServicesAsrPlannedFailoverJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/start-azrecoveryservicesasrplannedfailoverjob?view=azps-2.0.0&viewFallbackFrom=azps-1.1.0) PowerShell-cmdlet, samen met een plan voor herstel.



## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Monitor voor Site Recovery](site-recovery-monitor-and-troubleshoot.md)
