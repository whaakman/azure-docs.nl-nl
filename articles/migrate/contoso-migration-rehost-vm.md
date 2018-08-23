---
title: Een Contoso-app met migratie naar virtuele Azure-machines met Azure Site Recovery rehost | Microsoft Docs
description: Meer informatie over hoe een on-premises rehost-app met een lift-and-shift-migratie van on-premises machines naar Azure met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: 11859beb3d7bf0d0b0b801328c6570d274f1ea68
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42057513"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Migratie van Contoso: een app on-premises naar Azure VM's opnieuw hosten


In dit artikel ziet u hoe Contoso de on-premises SmartHotel-app in Azure, naamconflicten door te migreren van de VM's van de app naar Azure-VM's.


Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso n-premises resources naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en aanvullende artikelen na verloop van tijd toegevoegd.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Een app op Azure VM's en een beheerde SQL-exemplaar opnieuw hosten](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
Artikel 5: Een app op Azure VM's opnieuw hosten | Ziet u hoe Contoso de app SmartHotel virtuele machines migreren naar Azure-VM's, met behulp van de Site Recovery-service. | In dit artikel.
[Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar



In dit artikel worden de Contoso de twee lagen Windows gemigreerd. SmartHotel NET-app die worden uitgevoerd op virtuele VMware-machines naar Azure. Als u wilt dat deze app wilt gebruiken, wordt geleverd als open-source en u kunt dit ook downloaden via [github](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **Beperk risico**: de SmartHotel app is essentieel voor het bedrijf Contoso. Ze willen verplaatsen naar Azure met nul risico.
- **Uitbreiden**: Contoso niet wilt wijzigen van de app. Ze willen gewoon om ervoor te zorgen dat het stabiel is.


## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen worden gebruikt om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in VMware.  De app blijven als kritiek in de cloud als on-premises. 
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf, maar in de huidige vorm ze gewoon wilt verplaatsen veilig naar de cloud.
- Contoso wilt niet wijzigen van de ops-model voor deze app. Willen ze ermee in de cloud op dezelfde manier als nu.
- Contoso wilt niet wijzigen van alle app-functionaliteit. Alleen de locatie van de app wordt gewijzigd.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

Hier volgt de huidige omgeving

- De app is gelaagd over twee virtuele machines (**WEBVM** en **SQLVM**).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.

![Scenario-architectuur](./media/contoso-migration-rehost-vm/architecture.png) 

## <a name="migration-process"></a>Migratieproces

Contoso wordt de frontend van de app en database virtuele machines migreren naar Azure-VM's met behulp van Site Recovery:

- Als eerste stap, moeten ze voorbereiden en instellen van Azure-onderdelen voor Site Recovery en de on-premises VMware-infrastructuur voorbereiden.
- Ze al hebben hun [Azure-infrastructuur](contoso-migration-infrastructure.md) aanwezig is, zodat ze alleen wilt toevoegen van een aantal Azure-onderdelen specifiek voor Site Recovery.
- Met alles voorbereid, kunnen ze gaan repliceren van de virtuele machines.
-Nadat replicatie is ingeschakeld en werkt, zij de virtuele machine migreren met failover naar Azure.

![Migratieproces](./media/contoso-migration-rehost-vm/migraton-process.png) 



### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's, en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.


## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) nodig om uit te voeren in dit scenario.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al een abonnement hebt gemaakt tijdens de eerste artikelen in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor Site Recovery.
**On-premises servers** | On-premises vCenter-Servers moeten versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> ESXi-hosts moeten versie 5.5, 6.0 of 6.5 wordt uitgevoerd<br/><br/> Een of meer virtuele VMware-machines moet worden uitgevoerd op de ESXi-host.
**On-premises VM 's** | Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Azure voorbereiden voor Site Recovery**: maken van een Azure storage-account voor het opslaan van gerepliceerde gegevens, en een Recovery Services-kluis.
> * **Stap 2: On-premises VMware voorbereiden voor Site Recovery**: ze accounts voor VM-detectie- en agent-installatie voorbereiden en voorbereidingen voor het verbinding maken met virtuele Azure-machines na een failover.
> * **Stap 3:-Machines repliceren**: ze instellen van replicatie en beginnen met het repliceren van virtuele machines naar Azure storage.
> * **Stap 4: Migreer de virtuele machines met Site Recovery**: ze voert een testfailover uit om te controleren of alles goed werkt, en voer een volledige failover voor de virtuele machines migreren naar Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Azure voorbereiden voor de Site Recovery-service

Hier volgen de Azure-onderdelen die Contoso moet de virtuele machines migreren naar Azure:

- Een VNet waarin Azure-VM's geplaatst worden moeten nadat ze zijn gemaakt tijdens de failover.
- Azure storage-account voor het opslaan van gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Ze stelt deze als volgt:

1. Contoso instellen al een netwerk voor Site Recovery kan worden gebruikt wanneer ze [geïmplementeerd van de Azure-infrastructuur](contoso-migration-infrastructure.md)

    - De app SmartHotel is een productie-app en de virtuele machines worden gemigreerd naar de-Azure-productienetwerk (VNET-PROD-EUS2) in de primaire regio in de VS-Oost 2.
    - Beide VM's worden geplaatst in de resourcegroep ContosoRG, die wordt gebruikt voor productieresources.
    - De frontend van de app VM (WEBVM) worden gemigreerd naar het front-end-subnet (PROD-FE-EUS2), in het productienetwerk.
    - De virtuele machine (SQLVM) van de app-database worden gemigreerd naar de database-subnet (PROD-DB-EUS2), in het productienetwerk.

2. Contoso maakt een Azure storage-account (contosovmsacc20180528) in de primaire regio.
    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Ze gebruiken een account voor algemeen gebruik met de standard-opslag en LRS-replicatie. 

    ![Site Recovery-opslag](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Met de netwerk- en storage-account in plaats Contoso nu een Recovery Services-kluis (ContosoMigrationVault) gemaakt en plaatst deze in de resourcegroep ContosoFailoverRG in de primaire regio in de VS-Oost 2.

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

Contoso stelt als volgt u het account:

1. Een rol maken ze op de vCenter-niveau.
2. Ze toewijzen aan die rol op de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine.

- Site Recovery kan een automatische push-installatie van de Mobility-service kunt doen wanneer de VM-replicatie is ingeschakeld.
- Er is een account vereist, zodat Site Recovery toegang heeft tot de virtuele machines voor de push-installatie. U geeft dit account bij het instellen van replicatie.
- Het account kan worden domein of lokale met machtigingen om te installeren op de virtuele machines.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover wil Contoso verbinding maken met de Azure VM's. U doet dit door ze als volgt vóór de migratie:

1. Voor toegang via internet ze:

 - Schakelt u RDP in op de on-premises virtuele machine voordat de failover
 - Zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel.
 - Controleer of RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
 
2. Voor toegang via site-naar-site VPN, ze:

 - Schakelt u RDP in op de on-premises machine.
 - Toestaan dat RDP in de **Windows Firewall** -> **toegestane apps en functies**, voor **domein en privé** netwerken.
 - SAN-beleid van het besturingssysteem instellen op de on-premises virtuele machine om **OnlineAll**.

Wanneer ze een failover uitvoert moeten ze bovendien controleert u het volgende:

- Er mogen geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeert. Als er, zich ze niet aanmelden bij de virtuele machine totdat de update is voltooid.
- Na een failover, ze kunnen controleren **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven. Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd, en bekijk deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een rol voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) het maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Stap 3: De on-premises VM's repliceren

Voordat ze kunnen een migratie uitgevoerd naar Azure, moet Contoso instellen en inschakelen van replicatie.

### <a name="set-a-replication-goal"></a>Een replicatiedoel instellen

1. In de kluis onder de kluisnaam (ContosoVMVault) ze een replicatiedoel selecteren (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden** .
2. Ze opgeven dat de machines zich op locatie, die worden uitgevoerd op VMware, en naar Azure te repliceren zijn.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze bevestigen dat ze hebben de implementatieplanning uitgevoerd, door te selecteren **Ja, heb ik gedaan**. In dit scenario, Contoso slechts twee virtuele machines wilt migreren, en hoeft-implementatie plannen.


### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso nodig heeft om de bronomgeving te configureren. U doet dit door ze een OVF-sjabloon downloaden en gebruiken om te implementeren van de Site Recovery-configuratieserver als een maximaal beschikbaar, on-premises VMware-VM. Nadat de configuratieserver actief is, Registreer ze deze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.



Contoso voert de volgende stappen uit als volgt uit:

1. In de kluis downloaden ze de OVF-sjabloon van **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-vm/add-cs.png)

2. Ze Importeer de sjabloon in VMware om te maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3.  Wanneer ze de virtuele machine voor het eerst inschakelen, deze wordt opgestart in een Windows Server 2016-installatie-ervaring. Ze accepteer de gebruiksrechtovereenkomst en voer een administrator-wachtwoord.
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

7. Nadat de registratie is voltooid, wordt in de Azure-portal, Contoso dubbele controleert dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen, en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Nu Contoso Hiermee geeft u de replicatie-instellingen van het doel.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in de opgegeven doellocatie is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Contoso kan nu een replicatiebeleid maken.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maken van een beleid **ContosoMigrationPolicy**.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**. De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.

        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Het replicatiebeleid koppelen](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>Replicatie inschakelen voor WEBVM

Met alles op plek kunt Contoso inschakelen replicatie voor de virtuele machines. Ze start met WebVM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze geven aan dat ze willen VM's inschakelen, selecteert u de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Ze selecteren voor de doelinstellingen, met inbegrip van de resourcegroep en Azure-netwerk en het opslagaccount.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Contoso selecteert **WebVM** voor replicatie, controleert u het replicatiebeleid voor en replicatie inschakelen.

    - In dit stadium selecteert Contoso alleen WEBVM omdat VNet en subnet moeten worden geselecteerd en de Contoso de VM's van de app wordt geplaatst in verschillende subnetten.
    - Site Recovery installeert automatisch de Mobility-service op de virtuele machine wanneer replicatie is ingeschakeld.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
6. In **Essentials** in Azure portal, Contoso de structuur kunt bekijken voor de virtuele machines repliceren naar Azure.


### <a name="enable-replication-for-sqlvm"></a>Replicatie voor de SQLVM inschakelen

Contoso kan nu beginnen met replicatie van de machine SQLVM, met behulp van hetzelfde proces als hierboven.

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

Contoso voert een snelle testfailover uit en klikt u vervolgens een volledige failover naar de virtuele machines migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een test-failover kunt u ervoor zorgen dat alles werkt zoals verwacht. 

1. Contoso wordt een testfailover uitgevoerd naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 

    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.
    
3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Contoso wordt gecontroleerd of de virtuele machine de juiste grootte, verbonden met het juiste netwerk en wordt uitgevoerd. 
4. Nadat u hebt de testfailover gecontroleerd, ze opschonen van de failover, en noteer eventuele opmerkingen en sla. 

### <a name="create-and-customize-a-recovery-plan"></a>Maken en aanpassen van een herstelplan

 Contoso maakt nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, een plan voor herstel voor migratie. 

- Een herstelplan Hiermee geeft u de volgorde in met een failover optreedt en geeft aan hoe Azure-VM's wordt online worden gebracht in Azure.
- Omdat de app twee lagen is, aanpassen zij het herstelplan te gaan zodat de gegevens virtuele machine (SQLVM) wordt gestart voordat de front-end (WEBVM).

1. In **herstelplannen (Site Recovery)** > **+ herstelplan**, ze een plan maken en de virtuele machines aan toe te voegen.

    ![Plan voor herstel](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Na het maken van het abonnement, ze deze aanpassen (**herstelplannen** > **SmartHotelMigrationPlan** > **aanpassen**).
2.  Ze WEBVM uit verwijderen **groep 1: Start**.  Dit zorgt ervoor dat de eerste startactie is van invloed op SQLVM alleen.
3.  In **+ groep** > **toevoegen beveiligde items**, ze WEBVM toevoegen aan groep 2: Start.  De virtuele machines moeten zich in twee verschillende groepen.


### <a name="migrate-the-vms"></a>De virtuele machines migreren


Contoso kan nu een volledige failover voor de migratie uitvoeren.

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

Met de migratie is voltooid, worden de lagen van de app SmartHotel nu uitgevoerd op Azure Virtual machines.

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

- Voor het beheren van toegang, bekijk ze de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machines. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan naar de app deze kan bereiken.
- Ze ook rekening houden met de gegevens op de schijf met behulp van Azure Disk Encryption en Key Vault te beveiligen.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

Contoso gaat naar de back-up van de gegevens op de virtuele machines met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

1. Contoso heeft een bestaande licentie voor hun VM's en gebruik van Azure Hybrid Benefit.  Ze zullen de bestaande Azure-VM's, om te profiteren van deze prijzen converteren.
2. Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing waarmee u kunt gebruiken en beheren van Azure en andere cloudresources. [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management. 

## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso de SmartHotel-app in Azure door te migreren van de VM's van de app naar Azure-VM's met behulp van de Site Recovery-service. 


## <a name="next-steps"></a>Volgende stappen

In de [volgend artikel](contoso-migration-rehost-vm-sql-ag.md) in de serie leert u hoe Contoso naamconflicten de SmartHotel app front-end virtuele machine op een Azure-VM en de database migreert naar een SQL Server AlwaysOn-beschikbaarheidsgroep in Azure.

