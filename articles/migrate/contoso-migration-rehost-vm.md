---
title: Een Contoso-app met migratie naar virtuele Azure-machines met Azure Site Recovery rehost | Microsoft Docs
description: Meer informatie over hoe een on-premises rehost-app met een lift-and-shift-migratie van on-premises machines naar Azure met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 36a84618a998dd2e06f5563fb8706a1dc8400e59
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823348"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Migratie van Contoso: Een on-premises app opnieuw hosten op Azure-VM's


In dit artikel ziet u hoe Contoso de on-premises SmartHotel360-app in Azure, naamconflicten door te migreren van de VM's van de app naar Azure-VM's.


Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Aanvullende artikelen gaan we toevoegen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: On-premises resources voor migratie naar Azure evalueren](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
Artikel 5: Een app opnieuw hosten in Azure-VM's | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | In dit artikel
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep rehost](contoso-migration-rehost-vm-sql-ag.md) | Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[7-artikel: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso een lift-and-shift-migratie van de app van de osTicket Linux Azure-virtuele machines, met behulp van Azure Site Recovery is voltooid | Beschikbaar
[8-artikel: Een Linux-app op Azure VM's en Azure MySQL rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso de osTicket Linux app overzet naar virtuele Azure-machines met Azure Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[9-artikel: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso de SmartHotel360-app is gemigreerd naar een Azure-Web-App en de app-database migreert naar een Azure SQL Server-exemplaar met Database Migration Assistant | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar
[11-artikel: Herstructureren van TFS op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure. | Beschikbaar
[12-artikel: Een app op Azure-containers en Azure SQL Database opnieuw modelleren](contoso-migration-rearchitect-container-sql.md) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | Beschikbaar
[13-artikel: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar
[14-artikel: Schalen van een migratie naar Azure](contoso-migration-scale.md) | Na het proberen van migratie combinaties, bereidt Contoso worden uitgebreid naar een volledige migratie naar Azure. | Beschikbaar


In dit artikel worden de Contoso de twee lagen Windows gemigreerd. NET SmartHotel360-app die worden uitgevoerd op virtuele VMware-machines naar Azure. Als u wilt dat deze app wilt gebruiken, wordt geleverd als open-source en u kunt dit ook downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit, en er is als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **Beperk risico**: De SmartHotel360-app is essentieel voor het bedrijf Contoso. Het bedrijf wil de app naar Azure verplaatsen met nul risico.
- **Uitbreiden**: Contoso niet wilt wijzigen van de app, maar wil om ervoor te zorgen dat het stabiel is.


## <a name="migration-goals"></a>Migratiedoelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen worden gebruikt om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in VMware. De app blijven als kritiek in de cloud als on-premises.
- Contoso wil niet investeren in deze app. Het is belangrijk voor het bedrijf, maar in de huidige vorm Contoso gewoon wil deze veilig naar de cloud verplaatsen.
- Contoso wilt niet wijzigen van de ops-model voor deze app. Contoso wil communiceren met het in de cloud op dezelfde manier als nu.
- Contoso wilt niet wijzigen van alle app-functionaliteit. Alleen de locatie van de app wordt gewijzigd.


## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeert het migratieproces, met inbegrip van de Azure-services die Contoso voor de migratie wilt gebruiken.

### <a name="current-app"></a>Huidige app

- De app is gelaagd over twee virtuele machines (**WEBVM** en **SQLVM**).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).

### <a name="proposed-architecture"></a>Voorgestelde architectuur

- Omdat de app een productie-werkbelasting is, de virtuele machines in Azure-app bevindt zich in de resourcegroep productie ContosoRG.
- De VM's van de app worden gemigreerd naar de primaire Azure-regio (VS-Oost 2) en worden in de productienetwerk (VNET-PROD-EUS2).
- De front-end web virtuele machine bevindt zich in de front-end-subnet (PROD-FE-EUS2) in het productienetwerk.
- De virtuele machine van de database bevindt zich in de database-subnet (PROD-DB-EUS2) in het productienetwerk.
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.

![Scenario-architectuur](./media/contoso-migration-rehost-vm/architecture.png)

### <a name="database-considerations"></a>Database-overwegingen

Contoso heeft als onderdeel van het ontwerpproces, een vergelijking tussen Azure SQL Database en SQL Server. De volgende overwegingen geholpen ze om te bepalen aan de slag met SQL Server wordt uitgevoerd op een Azure IaaS-VM:

- Met behulp van een Azure-VM met SQL Server lijkt te zijn van een optimale oplossing als Contoso nodig heeft om aan te passen van het besturingssysteem of de database-server, of als het mogelijk wilt plaatsen en het uitvoeren van apps van derden op dezelfde virtuele machine.
- Met Software Assurance, kan in de toekomst Contoso uitwisselen bestaande licenties voor de kortingstarieven in een SQL Database Managed Instance met Azure Hybrid Benefit voor SQL Server. Dit kan tot 30% besparen op Managed Instance.



### <a name="solution-review"></a>Beoordelingen van oplossing

Contoso evalueert het ontwerp van de voorgestelde door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Pros** | Zowel de virtuele machines van app worden verplaatst naar Azure zonder wijzigingen, waardoor de migratie eenvoudig.<br/><br/> Omdat Contoso lift-and-shift voor zowel virtuele machines van de app gebruikt is, zijn geen speciale configuratie of de migratie van hulpprogramma's voor de database van de app nodig.<br/><br/> Contoso kan gebruikmaken van hun investering in Software Assurance, met behulp van Azure Hybrid Benefit.<br/><br/> Contoso, behouden die volledige controle over de virtuele machines in Azure-app.
**Nadelen** | WEBVM en SQLVM wordt Windows Server 2008 R2 uitgevoerd. Het besturingssysteem wordt ondersteund door Azure voor specifieke rollen (juli 2018). [Meer informatie](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).<br/><br/> De lagen web-apps en gegevens van de app blijft een single point of failover.</br><br/> SQLVM wordt uitgevoerd op SQL Server 2008 R2 die zich niet in het algemeen worden ondersteund. Het wordt echter ondersteund voor virtuele Azure-machines (juli 2018). [Meer informatie](https://support.microsoft.com/en-us/help/956893).<br/><br/> Contoso moet doorgaan met ondersteuning van de app als Azure-VM's in plaats van verplaatsen naar een beheerde service zoals Azure App Service en Azure SQL Database.



### <a name="migration-process"></a>Migratieproces

Contoso wordt de frontend van de app en database virtuele machines migreren naar Azure-VM's met Site Recovery:

- Als een eerste stap Contoso wordt voorbereid en stelt u de Azure-onderdelen voor Site Recovery- en bereidt de on-premises VMware-infrastructuur.
- Ze al hebben de [Azure-infrastructuur](contoso-migration-infrastructure.md) aanwezig is, dus Contoso NET moet een aantal Azure-onderdelen toevoegen voor Site Recovery.
- Met alles voorbereid, kun Contoso repliceren van de virtuele machines.
- Nadat replicatie is ingeschakeld en werkt, wordt Contoso de virtuele machine met failover naar Azure migreren.

![Migratieproces](./media/contoso-migration-rehost-vm/migraton-process.png)



### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's, en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht. Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.


## <a name="prerequisites"></a>Vereisten

Hier volgt wat Contoso moet worden uitgevoerd in dit scenario.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | Abonnementen Contoso gemaakt in een eerder artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md).
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor Site Recovery.
**On-premises servers** | On-premises vCenter-Servers moeten versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> ESXi-hosts moeten versie 5.5, 6.0 of 6.5 wordt uitgevoerd<br/><br/> Een of meer virtuele VMware-machines moet worden uitgevoerd op de ESXi-host.
**On-premises VM 's** | Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso-beheerders de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Azure voorbereiden voor Site Recovery**: Ze maken Azure storage-account voor het opslaan van gerepliceerde gegevens, en een Recovery Services-kluis.
> * **Stap 2: On-premises VMware voorbereiden voor Site Recovery**: Deze accounts voor VM-detectie- en agent-installatie voorbereiden en voorbereidingen voor het verbinding maken met virtuele Azure-machines na een failover.
> * **Stap 3: Virtuele machines repliceren**: Ze instellen van replicatie en beginnen met het repliceren van virtuele machines naar Azure storage.
> * **Stap 4: Migreren van de virtuele machines met Site Recovery**: Ze voert een testfailover uit om te controleren of dat alles goed werkt, en voer vervolgens een volledige failover voor de virtuele machines migreren naar Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Azure voorbereiden voor de Site Recovery-service

Hier volgen de Azure-onderdelen die Contoso moet de virtuele machines migreren naar Azure:

- Een VNet waarin Azure-VM's geplaatst worden moeten nadat ze zijn gemaakt tijdens de failover.
- Azure storage-account voor het opslaan van gerepliceerde gegevens.
- Een Recovery Services-kluis in Azure.

Ze stelt deze als volgt:

1. Instellen van een netwerk-Contoso al een netwerk dat voor Site Recovery kan worden ingesteld wanneer ze [geïmplementeerd van de Azure-infrastructuur](contoso-migration-infrastructure.md)

    - De SmartHotel360-app is een productie-app en de virtuele machines worden gemigreerd naar de-Azure-productienetwerk (VNET-PROD-EUS2) in de primaire regio in de VS-Oost 2.
    - Beide VM's worden geplaatst in de resourcegroep ContosoRG, die wordt gebruikt voor productieresources.
    - De frontend van de app VM (WEBVM) worden gemigreerd naar het front-end-subnet (PROD-FE-EUS2), in het productienetwerk.
    - De virtuele machine (SQLVM) van de app-database worden gemigreerd naar de database-subnet (PROD-DB-EUS2), in het productienetwerk.

2. Instellen van een storage-account-Contoso Hiermee maakt u een Azure storage-account (contosovmsacc20180528) in de primaire regio.
    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Ze gebruiken een account voor algemeen gebruik met de standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Een kluis met de netwerk- en storage-account maken in plaats, Contoso nu maakt u een Recovery Services-kluis (ContosoMigrationVault) en plaatst het in de resourcegroep ContosoFailoverRG in de primaire regio in de VS-Oost 2.

    ![Recovery Services-kluis](./media/contoso-migration-rehost-vm/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) instellen van Azure voor Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Stap 2: On-premises VMware voorbereiden voor Site Recovery

Hier volgt een uitleg Contoso on-premises wordt voorbereid:

- Een account op de vCenter-server of vSphere ESXi-host voor het automatiseren van VM-detectie.
- Een account waarmee de automatische installatie van de Mobility-service op de virtuele VMware-machines.
- On-premises VM-instellingen, zodat Contoso verbinding met de gerepliceerde Azure-VM's na een failover maken kan.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren.
- Coördineer de replicatie, failover en failback voor virtuele machines.
- Er is minimaal een alleen-lezen-account vereist. Het account moet kunnen uitvoeren van bewerkingen, zoals het maken en verwijderen van schijven en op virtuele machines in te schakelen.

Contoso-beheerders het account is als volgt instellen:

1. Een rol maken ze op de vCenter-niveau.
2. Ze toewijzen aan die rol op de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine.

- Site Recovery kan een automatische push-installatie van de Mobility-service kunt doen wanneer de VM-replicatie is ingeschakeld.
- Er is een account vereist, zodat Site Recovery toegang heeft tot de virtuele machines voor de push-installatie. U geeft dit account bij het instellen van replicatie.
- Het account kan worden domein of lokale met machtigingen om te installeren op de virtuele machines.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover wil Contoso verbinding maken met de Azure VM's. U doet dit door beheerders van Contoso als volgt vóór de migratie:

1. Voor toegang via internet ze:

 - Hiermee schakelt u RDP in op de on-premises virtuele machine voordat de failover.
 - Zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel.
 - Controleer of RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.

2. Voor toegang via site-naar-site VPN, ze:

 - Schakelt u RDP in op de on-premises machine.
 - Toestaan dat RDP in de **Windows Firewall** -> **toegestane apps en functies**, voor **domein en privé** netwerken.
 - SAN-beleid van het besturingssysteem instellen op de on-premises virtuele machine om **OnlineAll**.

Wanneer ze een failover uitvoert moeten ze bovendien controleert u het volgende:

- Er mogen geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeert. Als er, zich ze niet aanmelden bij de virtuele machine totdat de update is voltooid.
- Na een failover, ze kunnen controleren **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven. Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd, en bekijk deze [tips voor probleemoplossing](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een rol voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) het maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Stap 3: De on-premises VM's repliceren

Voordat de Contoso-beheerders kunnen een migratie naar Azure uitvoert, moeten ze instellen en inschakelen van replicatie.

### <a name="set-a-replication-goal"></a>Een replicatiedoel instellen

1. In de kluis onder de kluisnaam (ContosoVMVault) ze een replicatiedoel selecteren (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden** .
2. Ze opgeven dat de machines zich op locatie, die worden uitgevoerd op VMware, en naar Azure te repliceren zijn.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze bevestigen dat ze hebben de implementatieplanning uitgevoerd, door te selecteren **Ja, heb ik gedaan**. In dit scenario, Contoso slechts twee virtuele machines wilt migreren, en hoeft-implementatie plannen.


### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso-beheerders moeten de bronomgeving configureren. U doet dit door ze een OVF-sjabloon downloaden en gebruiken om te implementeren van de Site Recovery-configuratieserver als een maximaal beschikbaar, on-premises VMware-VM. Nadat de configuratieserver actief is, Registreer ze deze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.



Contoso-beheerders uitvoeren deze stappen als volgt:

1. In de kluis downloaden ze de OVF-sjabloon van **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-vm/add-cs.png)

2. Ze Importeer de sjabloon in VMware om te maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3. Wanneer ze de virtuele machine voor het eerst inschakelen, deze wordt opgestart in een Windows Server 2016-installatie-ervaring. Ze accepteer de gebruiksrechtovereenkomst en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. De Azure Site Recovery Configuration Tool wordt op de eerste aanmelding wordt standaard uitgevoerd.
5. In het hulpprogramma Geef ze een naam voor het registreren van de configuratieserver in de kluis.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, zich ze aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u ze de configuratieserver registreren.

    ![Configuratieserver registreren](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Ze zich aanmelden bij de machine opnieuw en de configuratiewizard voor Server Management wordt automatisch gestart.
9. In de wizard selecteert ze de NIC die replicatieverkeer moet ontvangen. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
10. Ze selecteert u het abonnement, resourcegroep en de kluis waarin de configuratieserver registreren.
        ![Kluis](./media/contoso-migration-rehost-vm/cswiz1.png)

10. Ze downloaden en installeren van de MySQL-Server en VMWare PowerCLI.
11. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort, en geef een beschrijvende naam voor de server in Azure.
12. Ze geven de account die ze hebben gemaakt voor automatische detectie en de referenties die worden gebruikt voor het automatisch installeren van de Mobility-Service. Voor Windows-machines moet het account lokale administrator-bevoegdheden op de virtuele machines.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Na registratie is voltooid, wordt in de Azure-portal, ze dubbele controleren dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren.
8. Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen, en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Beheerders van Contoso Geef nu de replicatie-instellingen van het doel.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in de opgegeven doellocatie is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Contoso-beheerders kunnen nu een replicatiebeleid maken.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maken van een beleid **ContosoMigrationPolicy**.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: De standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**. De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.

        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver.

    ![Het replicatiebeleid koppelen](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>Replicatie inschakelen voor WEBVM

Beheerders van Contoso kunt met alles in plaats nu inschakelen replicatie voor de virtuele machines. Ze start met WebVM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze geven aan dat ze willen VM's inschakelen, selecteert u de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Ze selecteren voor de doelinstellingen, met inbegrip van de resourcegroep en Azure-netwerk en het opslagaccount.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Ze selecteren **WebVM** controleren van het replicatiebeleid voor replicatie en replicatie inschakelen.

    - In dit stadium ze alleen selecteert WEBVM omdat VNet en subnet moeten worden geselecteerd en de VM's van de app wordt geplaatst in verschillende subnetten.
    - Site Recovery installeert automatisch de Mobility-service op de virtuele machine wanneer replicatie is ingeschakeld.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
6. In **Essentials** in Azure portal, zien ze de structuur voor de virtuele machines repliceren naar Azure.


### <a name="enable-replication-for-sqlvm"></a>Replicatie voor de SQLVM inschakelen

Contoso-beheerders kunnen nu beginnen met replicatie van de machine SQLVM, met behulp van hetzelfde proces als hierboven.

1. Deze selecteren instellingen voor gegevensbron.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Ze vervolgens opgeven de doelinstellingen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Ze selecteren SQLVM voor replicatie.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Ze toepassen van de hetzelfde replicatiebeleid dat is gebruikt voor WEBVM en replicatie inschakelen.

    ![Infrastructuurweergave](./media/contoso-migration-rehost-vm/essentials.png)

**Meer hulp nodig?**

- U kunt een overzicht van al deze stappen in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Vindt u meer informatie over [inschakelen van replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Stap 4: De virtuele machines migreren

Beheerders van Contoso uitvoeren een snelle testfailover uit en klikt u vervolgens een volledige failover naar de virtuele machines migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een test-failover kunt u ervoor zorgen dat alles werkt zoals verwacht.

1. Ze een testfailover uitvoeren naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt.
3. Test-failover wordt uitgevoerd:

    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Ze controleren of de virtuele machine de juiste grootte, verbonden met het juiste netwerk en wordt uitgevoerd.
4. Nadat u hebt de testfailover gecontroleerd, ze opschonen van de failover, en noteer eventuele opmerkingen en sla.

### <a name="create-and-customize-a-recovery-plan"></a>Maken en aanpassen van een herstelplan

 Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, maken de beheerders van Contoso een herstelplan voor migratie.

- Een herstelplan Hiermee geeft u de volgorde in met een failover optreedt en geeft aan hoe Azure-VM's wordt online worden gebracht in Azure.
- Omdat de app twee lagen is, aanpassen zij het herstelplan te gaan zodat de gegevens virtuele machine (SQLVM) wordt gestart voordat de front-end (WEBVM).

1. In **herstelplannen (Site Recovery)** > **+ herstelplan**, ze een plan maken en de virtuele machines aan toe te voegen.

    ![Herstelplan](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Na het maken van het abonnement, ze deze aanpassen (**herstelplannen** > **SmartHotelMigrationPlan** > **aanpassen**).
2.  Ze WEBVM uit verwijderen **groep 1: Start**. Dit zorgt ervoor dat de eerste startactie is van invloed op SQLVM alleen.
3.  In **+ groep** > **toevoegen beveiligde items**, ze WEBVM toevoegen aan groep 2: Start. De virtuele machines moeten zich in twee verschillende groepen.


### <a name="migrate-the-vms"></a>De virtuele machines migreren


Nu uitvoeren beheerders van Contoso een volledige failover om de migratie te voltooien.

1. Ze selecteert het herstelplan > **Failover**.
2. Ze failover naar de meest recente herstelpunt selecteren, en dat Site Recovery moet proberen de on-premises VM afsluiten voordat de failover wordt geactiveerd. Ze kunnen de voortgang van de failover volgen op de **taken** pagina.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)


3. Na de failover controleren ze of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

    ![Failover](./media/contoso-migration-rehost-vm/failover2.png)

3. Ze voltooien na de verificatie van de migratie voor elke virtuele machine. Deze replicatie voor de virtuele machine wordt gestopt en Site Recovery-facturering voor deze stopt.

    ![Failover](./media/contoso-migration-rehost-vm/failover3.png)

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) waarop een test-failover wordt uitgevoerd.
- [Informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) over het maken van een plan voor herstel.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Failover-overschakeling uitvoeren naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, worden de lagen van de app SmartHotel360 nu uitgevoerd op Azure Virtual machines.

Contoso moet nu deze opschonen stappen uitvoeren:

- Verwijder de WEBVM-machine uit de vCenter-inventaris.
- Verwijder de SQLVM-machine uit de vCenter-inventaris.
- WEBVM en SQLVM verwijderen uit lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locatie en IP-adressen voor de virtuele machines worden bijgewerkt.
- Alle resources die interactie met de virtuele machines hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de app nu wordt uitgevoerd, moet de Contoso nu volledig operationeel maken en deze te beveiligen in Azure.

### <a name="security"></a>Beveiliging

Het Contoso security team controleert de Azure-VM's, om te bepalen van alle beveiligingsproblemen met zich mee.

- Voor het beheren van toegang, controleert het team de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machines. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan naar de app deze kan bereiken.
- Het team is ook rekening houden met de beveiliging van de gegevens op de schijf met behulp van Azure Disk Encryption en Key Vault.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

## <a name="bcdr"></a>BCDR

Voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR) voert Contoso de volgende acties uit:

- Gegevens veilig te houden: Contoso back-ups van de gegevens op de virtuele machines met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Houd apps ingesteld en geactiveerd: Contoso repliceert de virtuele machines in Azure-app naar een secundaire regio met behulp van Site Recovery. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).



### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

1. Contoso heeft een bestaande licentie voor hun VM's en gebruik van Azure Hybrid Benefit. Contoso wordt geconverteerd en de bestaande Azure-VM's, om te profiteren van deze prijzen.
2. Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing waarmee u kunt gebruiken en beheren van Azure en andere cloudresources. [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.

## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso de SmartHotel360-app in Azure door te migreren van de VM's van de app naar Azure-VM's met behulp van de Site Recovery-service.


## <a name="next-steps"></a>Volgende stappen

In de [volgend artikel](contoso-migration-rehost-vm-sql-ag.md) in de serie leert u hoe Contoso naamconflicten de SmartHotel360-app front-end virtuele machine op een Azure-VM en de database migreert naar een SQL Server AlwaysOn-beschikbaarheidsgroep in Azure.

