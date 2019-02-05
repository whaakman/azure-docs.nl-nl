---
title: Opnieuw een Contoso-app hosten door te migreren naar Azure-VM's en SQL Server AlwaysOn-beschikbaarheidsgroep | Microsoft Docs
description: Informatie over hoe Contoso een on-premises app rehost door te migreren naar Azure-VM's en SQL Server AlwaysOn-beschikbaarheidsgroep
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 84a2ef60c9f6aa4270c3fc4f83327070b298b8b2
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697184"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-azure-vms-and-sql-server-alwayson-availability-group"></a>Migratie van Contoso: Een on-premises-app op Azure VM's en SQL Server AlwaysOn-beschikbaarheidsgroep rehost

In dit artikel ziet u hoe Contoso naamconflicten voor de SmartHotel360-app in Azure. De frontend van de app VM migreert Contoso naar een Azure-VM en de app-database naar een Azure SQL Server VM, die worden uitgevoerd in een Windows Server-failovercluster met SQL Server AlwaysOn-beschikbaarheidsgroep gGroups.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Aanvullende artikelen gaan we toevoegen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: On-premises resources voor migratie naar Azure evalueren](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar   
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep rehost | Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | In dit artikel
[7-artikel: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso een lift-and-shift-migratie van de app van de osTicket Linux Azure-virtuele machines, met behulp van Azure Site Recovery is voltooid | Beschikbaar
[8-artikel: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Contoso de osTicket Linux app overzet naar virtuele Azure-machines met Azure Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[9-artikel: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso de SmartHotel360-app is gemigreerd naar een Azure-Web-App en de app-database migreert naar een Azure SQL Server-exemplaar met Database Migration Assistant | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar 
[11-artikel: Herstructureren van TFS op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure. | Beschikbaar
[12-artikel: Een app op Azure-containers en Azure SQL Database opnieuw modelleren](contoso-migration-rearchitect-container-sql.md) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | Beschikbaar
[13-artikel: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar
[14-artikel: Schalen van een migratie naar Azure](contoso-migration-scale.md) | Na het proberen van migratie combinaties, bereidt Contoso worden uitgebreid naar een volledige migratie naar Azure. | Beschikbaar


In dit artikel worden de twee lagen SmartHotel360 .NET Windows-app die worden uitgevoerd op virtuele VMware-machines naar Azure gemigreerd door Contoso. Als u wilt deze app wilt gebruiken, wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-team leiderschap heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit, en er is als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen stroomlijnen voor ontwikkelaars en gebruikers.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**:  Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie.  IT kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: Wanneer het bedrijf is groeit, moet: Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde opgeven.

## <a name="migration-goals"></a>Migratiedoelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen zijn gebruikt voor het bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in VMWare.  De app blijven als kritiek in de cloud als on-premises.
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf, maar in de huidige vorm Contoso wilt veilig naar de cloud verplaatsen.
- De on-premises database voor de app heeft problemen met de beschikbaarheid. Contoso wil implementeren in Azure als een cluster met hoge beschikbaarheid, met failover wordt toegepast.
- Contoso wil een upgrade van hun huidige SQL Server 2008 R2-platform, naar SQL Server 2017.
- Contoso-niet wilt gebruiken van een Azure SQL Database voor deze app en er wordt gezocht naar alternatieven.


## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag hun doelstellingen en vereisten, Contoso ontwerpen en beoordelingen van een implementatieoplossing, en identificeert het migratieproces, met inbegrip van de Azure-services die worden gebruikt voor de migratie.

### <a name="current-architecture"></a>Huidige architectuur

- De app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).


### <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- Contoso wordt de frontend van de app WEBVM migreren naar een Azure IaaS-VM.
    - De frontend van de virtuele machine in Azure worden geïmplementeerd in de resourcegroep voor ContosoRG (gebruikt voor productieresources).
    -  Deze bevindt zich in de-Azure-productienetwerk (VNET-PROD-EUS2) in de primaire regio in de VS-Oost 2.
- De app-database worden, gemigreerd naar een Azure SQL Server-VM.
    - Deze bevindt zich in het Contoso van het netwerk van Azure database (PROD-DB-EUS2) in de primaire regio in de VS-Oost 2.
    - Deze wordt geplaatst in een Windows Server failovercluster met twee knooppunten, die gebruikmaakt van SQL Server Always On Availability Groups.
    - In Azure worden de twee SQL Server-VM-knooppunten in het cluster worden geïmplementeerd in de resourcegroep ContosoRG.
    - De VM-knooppunten bevindt zich in de van Azure-productienetwerk (VNET-PROD-EUS2) in de primaire regio in de VS-Oost 2.
    - Virtuele machines worden uitgevoerd Windows Server 2016 Enterprise Edition van SQL Server 2017. Contoso geen licenties voor dit besturingssysteem, zodat een installatiekopie wordt gebruikt in de Azure Marketplace die de licentie als de kosten voor de Azure EA-toezegging biedt.
    - Naast de unieke namen voor beide VM's dezelfde instellingen gebruikt.
- Contoso implementeert een interne load balancer die luistert naar verkeer op het cluster, en stuurt deze naar de juiste clusterknooppunt.
    - De interne load balancer wordt geïmplementeerd in de ContosoNetworkingRG (gebruikt voor netwerkresources).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.

![Scenario-architectuur](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="database-considerations"></a>Database-overwegingen

Contoso heeft als onderdeel van het ontwerpproces, een vergelijking tussen Azure SQL Database en SQL Server. De volgende overwegingen geholpen ze om te bepalen aan de slag met een Azure IaaS-VM met SQL Server:

 - Met behulp van een Azure-VM met SQL Server lijkt te zijn van een optimale oplossing als Contoso nodig heeft om aan te passen van het besturingssysteem of de database-server, of als het mogelijk wilt plaatsen en het uitvoeren van apps van derden op dezelfde virtuele machine.
 - Met behulp van de Data Migration Assistant, kan Contoso eenvoudig beoordelen en migreren naar een Azure SQL-Database.
 

### <a name="solution-review"></a>Beoordelingen van oplossing

Contoso evalueert het voorgestelde ontwerp door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Pros** | WEBVM wordt verplaatst naar Azure zonder wijzigingen, waardoor de migratie eenvoudig.<br/><br/> De SQL Server-laag wordt uitgevoerd op SQL Server 2017 en Windows Server 2016. De huidige Windows Server 2008 R2-besturingssysteem wordt hierdoor en uitvoeren van SQL Server 2017 biedt ondersteuning voor de technische vereisten en de doelstellingen van Contoso. IT is 100% compatibel tijdens het verplaatsen van SQL Server 2008 R2.<br/><br/> Contoso kan gebruikmaken van hun investering in Software Assurance, met behulp van Azure Hybrid Benefit.<br/><br/> Een SQL Server-implementatie in Azure met hoge beschikbaarheid biedt fouttolerantie, zodat de gegevenslaag van de app niet langer een single point of failover is.
**Nadelen** | WEBVM wordt Windows Server 2008 R2 uitgevoerd. Het besturingssysteem wordt ondersteund door Azure voor specifieke rollen (juli 2018). [Meer informatie](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).<br/><br/> De weblaag van de app blijft een single point of failover.</br><br/> Contoso moet doorgaan met ondersteuning van de weblaag als een Azure-VM in plaats van verplaatsen naar een beheerde service zoals Azure App Service.<br/><br/> Met de oplossing voor gekozen moet Contoso doorgaan met twee SQL Server-VM's beheren in plaats van verplaatsen naar een beheerd platform, zoals Azure SQL Database Managed Instance. Contoso kan bovendien met Software Assurance wisselen hun bestaande licenties voor gereduceerde tarieven voor Azure SQL Database Managed Instance.


### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA lokaal op de on-premises SQL Server-computer wordt uitgevoerd en de database via een VPN-site-naar-site naar Azure migreert. | DMA is een gratis, downloadbare hulpprogramma.
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Site Recovery coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.

 

## <a name="migration-process"></a>Migratieproces

Beheerders van Contoso, wordt de VM's van de app migreren naar Azure.

- Zij u de front-end VM migreren naar virtuele Azure-machine met behulp van Site Recovery:
    - Als eerste stap, moeten ze voorbereiden en instellen van Azure-onderdelen en voorbereiden van de on-premises VMware-infrastructuur.
    - Met alles voorbereid, kunnen ze gaan repliceren van de virtuele machine.
    - Nadat replicatie is ingeschakeld en werkt, zij de virtuele machine migreren met failover naar Azure.
- Zij u de database migreren naar een SQL Server-cluster in Azure met behulp van de Data Migration Assistant (DMA).
    - Als eerste stap hebt voor het inrichten van SQL Server-VM's in Azure, instellen van het cluster en een interne load balancer, en ze AlwaysOn-beschikbaarheidsgroepen configureren.
    - Dit is voldaan, kunnen ze migreren van de database
- Na de migratie schakelt ze AlwaysOn-beveiliging voor de database.

![Migratieproces](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Vereisten

Hier volgt wat Contoso nodig heeft voor dit scenario te laten doen.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | Contoso is al een abonnement gemaakt in een eerdere artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor Site Recovery.
**Site recovery (on-premises)** | De on-premises vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.<br/><br/> Virtuele machines die u wilt repliceren, moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).



## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Voorbereiden van een cluster**: Maak een cluster voor het implementeren van twee knooppunten van de SQL Server-VM in Azure.
> * **Stap 2: Implementeren en instellen van het cluster**: Bereid een Azure SQL Server-cluster.  Databases worden gemigreerd in deze vooraf gemaakte cluster.
> * **Stap 3: De load balancer implementeren**: Een load balancer voor het verdelen van verkeer naar de SQL Server-knooppunten implementeren.
> * **Stap 4: Azure voorbereiden voor Site Recovery**: Maak een Azure storage-account voor het opslaan van gerepliceerde gegevens, en een Recovery Services-kluis. 
> * **Stap 5: On-premises VMware voorbereiden voor Site Recovery**: Accounts voor VM-detectie- en agent-installatie voorbereiden. On-premises machines voorbereiden, zodat gebruikers verbinding met Azure-VM's na de m maken kunnen; migratie.
> * **Stap 6: Virtuele machines repliceren**: Schakel replicatie van virtuele machines naar Azure.
> * **Stap 7: DMA installeren**: Download en installeer de Database Migration Assistant.
> * **Stap 7: Migreren van de database met DMA**: De database migreren naar Azure.
> * **Stap 9: Beveiliging van de database**: Maak een AlwaysOn-beschikbaarheidsgroep voor het cluster.
> * **Stap 10: De web-app VM migreren**: Voer een testfailover uit om te controleren of alles goed werkt. Voer een volledige failover naar Azure. 


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>Stap 1: Voorbereiden van een cluster met de groep op SQL Server AlwaysOn-beschikbaarheid

Contoso-beheerders instellen van het cluster als volgt:

1. Ze maken twee SQL Server-VM's met behulp van SQL Server 2017 Enterprise Windows Server 2016-installatiekopie in de Azure Marketplace. 

    ![SQL VM-SKU](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. In de **maken van virtuele machine Wizard** > **basisbeginselen**, ze configureren:

    - Namen voor de virtuele machines: **SQLAOG1** en **SQLAOG2**.
    - Aangezien machines bedrijfskritische, inschakelen ze SSD voor de VM-schijftype.
    - Deze opgeven machine-referenties.
    - Implementatie van de virtuele machines in de primaire VS-Oost 2-regio's, in de resourcegroep ContosoRG.

3. In **grootte**, ze beginnen met D2s_V3 SKU voor beide VM's. Ze u later naar te behoefte schalen.
4. In **instellingen**, ze de volgende handelingen uit:

    - Omdat deze VM's essentiële databases voor de app zijn, ze beheerde schijven gebruiken.
    - Deze plaatst u de machines in de productienetwerk van de VS-Oost 2 primaire regio (**VNET-PROD-EUS2**), in de database-subnet (**PROD-DB-EUS2**).
    - Ze maken een nieuwe beschikbaarheidsset: **SQLAOGAVSET**, met twee foutdomeinen en vijf updatedomeinen.

      ![SQL-VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. In **SQL Server-instellingen**, ze SQL-verbinding met het virtuele netwerk (persoonlijk), de limiet voor de standaardpoort 1433. Voor de verificatie ze dezelfde referenties gebruiken als ze op locatie (**contosoadmin**).

    ![SQL-VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**Meer hulp nodig?**

- [Hulp bij het](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) inrichten van een SQL Server-VM.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) virtuele machines configureren voor verschillende SQL Server-SKU's.

## <a name="step-2-deploy-and-set-up-the-cluster"></a>Stap 2: Implementeren en instellen van het cluster

Hier ziet hoe Contoso beheerders instellen van het cluster:

1. Zij instellen een Azure storage-account om te fungeren als de cloud-witness.
2. Ze toevoegen de SQL Server-VM's aan de Active Directory-domein in de Contoso-on-premises datacentrum.
3. Ze maken van het cluster in Azure.
4. De cloud-witness configureren.
5. Ten slotte kunnen SQL Always On-beschikbaarheidsgroepen.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Instellen van een storage-account als cloud-witness

Als u een cloud-witness instelt, moet Contoso een Azure Storage-account dat de blob-bestand dat is gebruikt voor het cluster arbitrage kan bevatten. Hetzelfde opslagaccount kan worden gebruikt voor het instellen van cloud-witness voor meerdere clusters. 

Beheerders van Contoso maken als volgt een opslagaccount:

1. Ze een herkenbare naam voor de account opgeven (**contosocloudwitness**).
2. Ze implementeren een algemene veelzijdige rekening met LRS.
3. Ze plaatst u het account in een derde regio - Zuid-centraal VS. Deze plaatst u het buiten de primaire en secundaire regio zodat deze beschikbaar is bij een regionale storing blijft.
4. Ze plaats deze in de resourcegroep waarin de infrastructuurresources - **ContosoInfraRG**.

    ![Cloud-witness](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Wanneer ze maken het opslagaccount, de primaire en secundaire toegangssleutel voor zijn gegenereerd. Moeten ze de primaire toegangssleutel voor het maken van de cloud-witness. De sleutel wordt weergegeven onder de naam van het opslagaccount > **toegangssleutels**.

    ![Toegangssleutel](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>SQL Server-VM's toevoegen aan de Contoso-domein

1. SQLAOG1 en SQLAOG2 van Contoso worden toegevoegd aan het domein contoso.com.
2. Klik vervolgens op elke virtuele machine installatie van de functie Windows Failover Clustering en hulpprogramma's.

### <a name="set-up-the-cluster"></a>Instellen van het cluster

Voordat u het cluster instelt, momentopname beheerders van Contoso een van de schijf met het besturingssysteem op elke computer.

![momentopname](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

1. Voer vervolgens, ze een script dat ze hebt geplaatst om toepassingsgatewayresource te maken van het Windows-failovercluster.

    ![Cluster maken](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

2. Nadat ze het cluster hebt gemaakt, controleren ze of de virtuele machines worden weergegeven als clusterknooppunten.

     ![Cluster maken](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>De cloud-witness configureren

1. Beheerders van Contoso configureren met behulp van de cloud-witness de **Quorum-configuratiewizard** in Failoverclusterbeheer.
2. Ze selecteren in de wizard het maken van een cloud-witness met de storage-account.
3. Nadat de cloud-witness is geconfigureerd, wordt in weergegeven in de module Failoverclusterbeheer.

    ![Cloud-witness](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

### <a name="enable-sql-server-always-on-availability-groups"></a>SQL Server Always On-beschikbaarheidsgroepen inschakelen

Contoso-beheerders kunnen nu altijd op inschakelen:

1. In SQL Server Configuration Manager ze inschakelen **AlwaysOn Availability Groups** voor de **SQL Server (MSSQLSERVER)** service.

    ![Schakel AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Ze start de service voor de wijzigingen worden doorgevoerd.

Contoso kan met AlwaysOn inschakelen instellen van de AlwaysOn-beschikbaarheidsgroep die worden beveiligd door de SmartHotel360-database.

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) witness en het instellen van een storage-account voor de cloud.
- [Instructies](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) voor het instellen van een cluster en het maken van een beschikbaarheidsgroep.

## <a name="step-3-deploy-the-azure-load-balancer"></a>Stap 3: De Azure Load Balancer implementeren

Beheerders van Contoso wil implementeren van een interne load balancer die bevindt zich voor de clusterknooppunten. De load balancer luistert naar verkeer, en stuurt deze naar het juiste knooppunt.

![Taakverdeling](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Ze maken als volgt de load balancer:

1. In Azure portal > **netwerken** > **Load Balancer**, ze een nieuwe interne load balancer instellen: **ILB-PROD-DB-EUS2-SQLAOG**.
2. Deze plaatst u de load balancer in het productienetwerk **VNET-PROD-EUS2**, in het subnet van de database **PROD-DB-EUS2**.
3. Ze wijzen een statisch IP-adres: 10.245.40.100.
4. Als een VPN-element, ze de load balancer in de netwerkresourcegroep implementeren **ContosoNetworkingRG**.

    ![Taakverdeling](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

Nadat de interne load balancer is geïmplementeerd, moeten ze instellen. Ze maken van een back-endadresgroep, een statustest instellen en configureren van een regel voor taakverdeling.

### <a name="add-a-backend-pool"></a>Een back-endgroep toevoegen

Voor het distribueren van verkeer naar de virtuele machines in het cluster, Contoso-beheerders instellen van een back-end-adresgroep met de IP-adressen van de NIC's voor virtuele machines die worden ontvangen van netwerkverkeer van de load balancer.

1. In de load balancer-instellingen in de portal, worden in Contoso een back-endgroep toevoegen: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Deze koppelen pool aan de beschikbaarheidsset SQLAOGAVSET. De virtuele machines in de set (**SQLAOG1** en **SQLAOG2**) aan de groep zijn toegevoegd.

    ![Back-endpool](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Een statustest maken

Beheerders van Contoso maken een statustest zodat de load balancer de status van de app kunt bewaken. De test wordt dynamisch toegevoegd of verwijderd van virtuele machines uit de load balancer-rotatie, op basis van hoe ze op statuscontroles reageren.

Ze maken de test als volgt: 

1. In de load balancer-instellingen in de portal, wordt een statustest gemaakt door Contoso: **SQLAlwaysOnEndPointProbe**.
2. Ze de test om te controleren van virtuele machines op TCP-poort 59999 ingesteld.
3. Ze een interval van vijf seconden tussen tests en een drempelwaarde van 2 hebt ingesteld. Als twee tests is mislukt, de virtuele machine wordt worden als slecht beschouwd.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Configureer de load balancer voor het ontvangen van verkeer


Nu Contoso beheerders een load balancer-regel instellen om te bepalen hoe verkeer wordt gedistribueerd naar de virtuele machines.

- Het front-end-IP-adres zorgt voor inkomend verkeer.
- De back-end-IP-adresgroep ontvangt het verkeer.

Ze Maak de regel als volgt:

1. In de load balancer-instellingen in de portal toevoegen ze een nieuwe regel voor taakverdeling: **SQLAlwaysOnEndPointListener**.
2. Ze een front-end-listener voor het ontvangen van binnenkomende SQL-client-verkeer op TCP 1433 ingesteld.
3. Deze opgeven met de back-endpool naar welk verkeer wordt gerouteerd, en de poort waarop de virtuele machines luisteren naar verkeer.
4. Ze mogelijk zwevend IP (direct server return). Dit is altijd vereist voor de SQL AlwaysOn.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**Meer hulp nodig?**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) van Azure Load Balancer.
- [Meer informatie over](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) het maken van een load balancer.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>Stap 4: Azure voorbereiden voor de Site Recovery-service

Hier volgen de Azure-onderdelen die Contoso moet Site Recovery implementeren:

- Een VNet waarin virtuele machines geplaatst worden moeten nadat ze tijdens de failover maakt.
- Azure storage-account voor het opslaan van gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Deze wordt als volgt instellen door beheerders van Contoso:

1.  Contoso is al gemaakt/subnet van een netwerk voor Site Recovery kan worden gebruikt wanneer ze [geïmplementeerd van de Azure-infrastructuur](contoso-migration-rehost-vm-sql-ag.md).

    - De SmartHotel360-app is een productie-app en WEBVM worden gemigreerd naar de-Azure-productienetwerk (VNET-PROD-EUS2) in de primaire regio in de VS-Oost 2.
    - WEBVM wordt geplaatst in de resourcegroep ContosoRG, die wordt gebruikt voor productieresources, en in de productie-subnet (PROD-FE-EUS2).

2. Beheerders van Contoso maken Azure storage-account (contosovmsacc20180528) in de primaire regio.

    - Ze gebruiken een account voor algemeen gebruik met de standard-opslag en LRS-replicatie.
    - Het account moet zich in dezelfde regio bevinden als de kluis.

      ![Site Recovery-opslag](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Met de netwerk- en storage-account in plaats ze nu een Recovery Services-kluis maken (**ContosoMigrationVault**), en plaats deze in de **ContosoFailoverRG** resourcegroep in de primaire regio in de VS-Oost 2 .

    ![Recovery Services-kluis](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) instellen van Azure voor Site Recovery.


## <a name="step-5-prepare-on-premises-vmware-for-site-recovery"></a>Stap 5: On-premises VMware voorbereiden voor Site Recovery

Dit is wat beheerders Contoso on-premises voorbereiden:

- Een account op de vCenter-server of vSphere ESXi-host voor het automatiseren van VM-detectie.
- Een account waarmee de automatische installatie van de Mobility-service op VMware-VM's die u wilt repliceren.
- On-premises VM-instellingen, zodat Contoso verbinding met de gerepliceerde Azure-virtuele machine na een failover maken kan.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. 
- Replicatie, failover en failback orkestreren.
- Er is minimaal een alleen-lezen-account vereist. U moet een account die bewerkingen zoals het maken en verwijderen van schijven en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso-beheerders het account is als volgt instellen:

1. Een rol maken ze op de vCenter-niveau.
2. Deze vervolgens toewijzen die rol de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer replicatie is ingeschakeld voor de virtuele machine.
- U moet een account met Site Recovery gebruiken kunt voor toegang tot de virtuele machine voor de push-installatie. U geeft dit account bij het instellen van replicatie in de Azure-console.
- Het account kan worden domein of lokale met machtigingen om te installeren op de virtuele machine.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover wil Contoso kunnen verbinding maken met virtuele Azure-machines. U doet dit door beheerders van Contoso als volgt vóór de migratie:

1. Voor toegang via internet ze:

   - Schakelt u RDP in op de on-premises virtuele machine voordat de failover
   - Zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel.
   - Controleer of RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
 
2. Voor toegang via site-naar-site VPN, ze:

   - Schakelt u RDP in op de on-premises machine.
   - Toestaan dat RDP in de **Windows Firewall** -> **toegestane apps en functies**, voor **domein en privé** netwerken.
   - SAN-beleid van het besturingssysteem instellen op de on-premises virtuele machine om **OnlineAll**.

Wanneer ze een failover uitvoert moeten ze bovendien controleert u het volgende:

- Er mogen geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeert. Als er, zich gebruikers niet aanmelden bij de virtuele machine totdat de update is voltooid.
- Na een failover, ze kunnen controleren **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven. Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd, en bekijk deze [tips voor probleemoplossing](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een rol voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) het maken van een account voor push-installatie van de Mobility-service.


## <a name="step-6-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Stap 6: De on-premises VM's repliceren naar Azure met Site Recovery

Voordat ze een migratie naar Azure uitvoert kunnen, moet de Contoso-beheerders instellen en inschakelen van replicatie.

### <a name="set-a-replication-goal"></a>Een replicatiedoel instellen

1. In de kluis onder de kluisnaam (ContosoVMVault) ze een replicatiedoel selecteren (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden** .
2. Ze opgeven dat de machines zich op locatie, die worden uitgevoerd op VMware, en naar Azure te repliceren zijn.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze nodig hebben om te bevestigen dat ze hebben de implementatieplanning uitgevoerd, door te selecteren **Ja, heb ik gedaan**. In dit scenario Contoso zijn alleen migreren van een virtuele machine en plannen van de implementatie niet nodig.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Beheerders van Contoso moet configureren de bronomgeving. U doet dit door ze een OVF-sjabloon downloaden en gebruiken om te implementeren van de Site Recovery-configuratieserver als een maximaal beschikbaar, on-premises VMware-VM. Nadat de configuratieserver actief is, Registreer ze deze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso-beheerders uitvoeren deze stappen als volgt:


1. In de kluis downloaden ze de OVF-sjabloon van **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Ze Importeer de sjabloon in VMware om te maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. Wanneer ze de virtuele machine voor het eerst inschakelen, deze wordt opgestart in een Windows Server 2016-installatie-ervaring. Ze accepteer de gebruiksrechtovereenkomst en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. De Azure Site Recovery Configuration Tool wordt op de eerste aanmelding wordt standaard uitgevoerd.
5. In het hulpprogramma Geef ze een naam te gebruiken voor het registreren van de configuratieserver in de kluis.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, zich ze aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.

    ![Configuratieserver registreren](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Ze zich aanmelden bij de machine opnieuw en de configuratiewizard voor Server Management wordt automatisch gestart.
9. In de wizard selecteert ze de NIC die replicatieverkeer moet ontvangen. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
10. Ze selecteert u het abonnement, resourcegroep en kluis waarin de configuratieserver registreren.
        ![Kluis](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. Ze vervolgens downloaden en MySQL-Server en VMWare PowerCLI installeren. 
11. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort, en geef een beschrijvende naam voor de vCenter-server.
12. Ze geven de account die ze hebben gemaakt voor automatische detectie en de referenties die worden gebruikt voor het automatisch installeren van de Mobility-Service. Voor Windows-machines moet het account lokale administrator-bevoegdheden op de virtuele machines.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Na registratie is voltooid, wordt in de Azure-portal, ze dubbele controleren dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen, en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Nu opgeven beheerders van Contoso doel replicatie-instellingen.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Contoso-beheerders kunnen nu een replicatiebeleid maken.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maken van een beleid **ContosoMigrationPolicy**.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: De standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**. De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Het replicatiebeleid koppelen](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Replicatie inschakelen

Contoso-beheerders kunnen nu starten WebVM repliceren.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze geven aan dat ze willen VM's inschakelen, selecteert u de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Ze geven nu de doelinstellingen, met inbegrip van de resourcegroep en VNet en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Ze selecteert de WebVM voor replicatie, controleert het replicatiebeleid en replicatie inschakelen. Site Recovery installeert de Mobility-Service op de virtuele machine wanneer replicatie is ingeschakeld.
 
    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
5. In **Essentials** in Azure portal, zien ze de structuur voor de virtuele machines repliceren naar Azure.

    ![Infrastructuurweergave](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**Meer hulp nodig?**

- U kunt een overzicht van al deze stappen in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Vindt u meer informatie over [inschakelen van replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-7-install-the-database-migration-assistant-dma"></a>Stap 7: De Database Migration Assistant (DMA) installeren

Beheerders van Contoso de SmartHotel360-database wilt migreren naar Azure-VM **SQLAOG1** met behulp van de DMA. Ze stelt DMA als volgt in:

1. Ze het hulpprogramma downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) naar de on-premises SQL Server-VM (**SQLVM**).
2. Ze uitvoeren setup (DownloadMigrationAssistant.msi) op de virtuele machine.
3. Op de **voltooien** pagina, selecteren ze **Start Microsoft Data Migration Assistant** voordat de wizard is voltooid.

## <a name="step-8-migrate-the-database-with-dma"></a>Stap 8: Migreren van de database met DMA

1. In de DMA ze uitvoeren van een nieuwe migratie - **SmartHotel**.
2. Ze selecteert de **doelservertype** als **SQL Server op Azure Virtual Machines**. 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. In de migratie, die ze toevoegen **SQLVM** als de bronserver en **SQLAOG1** als het doel. Deze opgeven referenties voor elke machine.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Ze een lokale share voor de database en configuratie-informatie te maken. Het moet toegankelijk zijn met schrijftoegang voor de SQL-serviceaccount op SQLVM en SQLAOG1.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso selecteert de aanmeldingen die moeten worden gemigreerd en de migratie start. Nadat deze is voltooid, ziet u DMA de migratie als geslaagd.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Ze controleren of de database wordt uitgevoerd op **SQLAOG1**.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

DMS maakt verbinding met de on-premises SQL Server-VM via een site-naar-site VPN-verbinding tussen de Contoso-datacenter en Azure, en worden vervolgens de database.

## <a name="step-7-protect-the-database-with-alwayson"></a>Stap 7: Beveiliging van de database met AlwaysOn

Met de app-database op **SQLAOG1**, Contoso-beheerders kunnen nu beveiligen met behulp van AlwaysOn-beschikbaarheidsgroepen. Ze configureren met behulp van SQL Management Studio AlwaysOn en wijs een listener met behulp van Windows-clustering. 

### <a name="create-an-alwayson-availability-group"></a>Een AlwaysOn-beschikbaarheidsgroep maken

1. In SQL Management Studio, ze met de rechtermuisknop op **altijd op hoge beschikbaarheid** om te beginnen de **Wizard Nieuwe beschikbaarheidsgroep**.
2. In **beveiligingsopties opgeven**, naam van de beschikbaarheidsgroep **SHAOG**. In **Databases selecteren**, ze de SmartHotel360-database selecteren.

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. In **replica's opgeven**, ze de twee SQL-knooppunten toevoegen als beschikbare replica's en configureer deze voor automatische failover met synchrone doorvoer.

     ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Configureren van een listener voor de groep (**SHAOG**) en poort. Het IP-adres van de interne load balancer wordt toegevoegd als een statisch IP-adres (10.245.40.100).

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. In **Select Data Synchronization**, ze inschakelen automatische seeding. Met deze optie maakt SQL Server automatisch de secundaire replica's voor elke database in de groep, zodat Contoso niet hoeven te handmatig een back-up en herstel deze. Na de validatie van wordt de beschikbaarheidsgroep gemaakt.

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso is een probleem bij het maken van de groep. Ze zijn niet gebruikmaakt van Active Directory Windows geïntegreerde beveiliging en moeten dus verlenen van machtigingen aan de SQL-aanmelding voor het maken van de functies van het Windows Failover Cluster.

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. Nadat de groep is gemaakt, kunt Contoso wordt het in SQL Management Studio.

### <a name="configure-a-listener-on-the-cluster"></a>Een listener op het cluster configureren

Als laatste stap in het instellen van de SQL-implementatie, beheerders van Contoso configureren van de interne load balancer als de listener op het cluster en de listener online brengt. Ze een script gebruiken om dit te doen.

![Cluster-listener](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>De configuratie controleren

Alles wat u instelt, heeft Contoso nu een functionele beschikbaarheidsgroep in Azure die gebruikmaakt van de gemigreerde database. Beheerders wordt dit controleren door verbinding te maken met de interne load balancer in SQL Management Studio.

![ILB-verbinding](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**Meer hulp nodig?**
- Meer informatie over het maken van een [beschikbaarheidsgroep](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) en [listener](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- Handmatig [het cluster ingesteld voor het gebruik van de IP-adres van de load balancer](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [Meer informatie](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) over het maken en met behulp van SAS.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>Stap 8: Migreren van de virtuele machine met Site Recovery

Beheerders van Contoso uitvoeren van een snelle failover testen en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitvoeren om te controleren of alles werkt zoals verwacht vóór de migratie. 

1. Ze een testfailover uitvoeren naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 

    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Zij controleren dat de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, ze opschonen van de failover, en noteer eventuele opmerkingen en sla. 

### <a name="run-a-failover"></a>Een failover uitvoeren

1. Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, Contoso-beheerders maken van een herstelplan voor migratie en WEBVM toevoegen aan het plan.

     ![Herstelplan](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Ze een failover worden uitgevoerd op het plan. Ze het meest recente herstelpunt selecteren en opgeven dat Site Recovery proberen moet de on-premises VM afsluiten voordat de failover wordt geactiveerd.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Na de failover controleren ze of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

    ![Herstelplan](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Nadat u hebt de virtuele machine in Azure gecontroleerd, voltooien ze van de migratie voor het voltooien van het migratieproces, replicatie voor de virtuele machine stoppen en Site Recovery-facturering voor de virtuele machine stoppen.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

Beheerders van Contoso bijwerken als de laatste stap in het migratieproces, de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde database die wordt uitgevoerd op de listener SHAOG. Deze configuratie wordt gewijzigd op de WEBVM nu uitgevoerd in Azure.  Deze configuratie bevindt zich in het bestand web.config van de ASP-toepassing. 

1. Zoek het bestand op C:\inetpub\SmartHotelWeb\web.config.  Wijzig de naam van de server in overeenstemming met de FQDN-naam van de AOG: shaog.contoso.com.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Na het bijwerken van het bestand en op te slaan, ze IIS opnieuw starten op WEBVM. Ze doen dit met behulp van de IISRESET /RESTART vanaf een opdrachtprompt.
2. Nadat IIS opnieuw is gestart, wordt de database die wordt uitgevoerd op de SQL-MI nu gebruik van de toepassing.


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) waarop een test-failover wordt uitgevoerd. 
- [Informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) over het maken van een plan voor herstel.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Failover-overschakeling uitvoeren naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Na de migratie, de SmartHotel360-app wordt uitgevoerd op een Azure VM en de SmartHotel360-database bevindt zich in de Azure SQL-cluster.

Contoso moet nu deze opschonen stappen uitvoeren:  

- De on-premises VM's van de vCenter-voorraad verwijderen.
- Verwijder de virtuele machines uit lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locaties en IP-adressen voor VM's worden bijgewerkt.
- Alle resources die interactie met de uit bedrijf genomen VM's hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.
- Voeg dat de twee nieuwe VM's (SQLAOG1 en SQLAOG2) moeten worden toegevoegd aan de productie bewaken.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

Het Contoso security team controleert de Azure VM's WEBVM, SQLAOG1 en SQLAOG2 om beveiligingsproblemen te bepalen. 

- Het team controleert de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machine om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan voor de toepassing kan worden doorgegeven.
- Het team rekening gehouden met de gegevens op de schijf met behulp van Azure Disk Encryption en Key Vault te beveiligen.
- Het team moet evalueren transparante gegevensversleuteling (TDE) en vervolgens op de SmartHotel360-database die wordt uitgevoerd op de nieuwe SQL AOG in te schakelen. [Meer informatie](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms) over procedures voor beveiliging voor virtuele machines.


## <a name="bcdr"></a>BCDR

 Voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR) voert Contoso de volgende acties uit:
 - Gegevens veilig te houden: Contoso back-ups van de gegevens op WEBVM, SQLAOG1 en SQLAOG2 VM's met behulp van de Azure Backup-service. [Meer informatie].
(https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Contoso wordt ook meer informatie over het Azure Storage gebruiken voor het back-up van SQL Server rechtstreeks naar de blob-opslag. [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-use-storage-sql-server-backup-restore).
- Houd apps ingesteld en geactiveerd: Contoso repliceert de virtuele machines in Azure-app naar een secundaire regio met behulp van Site Recovery. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).


### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

1. Contoso heeft een bestaande licentie voor hun WEBVM en gebruik van Azure Hybrid Benefit.  Contoso wordt geconverteerd en de bestaande Azure-VM's om te profiteren van deze prijzen.
2. Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management. 

## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso de SmartHotel360-app in Azure door het app-front-end virtuele machine migreren naar Azure met behulp van de Site Recovery-service. Contoso-gemigreerd van de app-database naar een SQL Server-cluster dat is ingericht in Azure en beveiligd in een SQL Server AlwaysOn-beschikbaarheidsgroep.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks, we laten zien hoe Contoso rehost voor hun servicedesk osTicket-app die wordt uitgevoerd op Linux en geïmplementeerd met een MySQL-database.


