---
title: Opnieuw een Contoso-app hosten door te migreren naar Azure-VM's en SQL Server AlwaysOn-beschikbaarheidsgroep | Microsoft Docs
description: Informatie over hoe Contoso een on-premises app rehost door te migreren naar Azure-VM's en SQL Server AlwaysOn-beschikbaarheidsgroep
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: raynew
ms.openlocfilehash: 0cfb583f9d16039249aaffe18f71039e91dc3705
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359203"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-azure-vms-and-sql-server-alwayson-availability-group"></a>Migratie van Contoso: een on-premises-app op Azure VM's en SQL Server AlwaysOn-beschikbaarheidsgroep Rehost

In dit artikel ziet u hoe Contoso naamconflicten voor de SmartHotel-app in Azure. Zij migreren de app front-end virtuele machine naar een Azure-VM en de app-database naar een Azure SQL Server VM, die worden uitgevoerd in een Windows Server-failovercluster met SQL Server AlwaysOn-beschikbaarheidsgroep gGroups.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en aanvullende artikelen na verloop van tijd toegevoegd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
[Artikel 3: On-premises resources evalueren](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Een app op Azure VM's en een beheerde SQL-exemplaar opnieuw hosten](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de app SmartHotel uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso de SmartHotel-app met behulp van Site Recovery alleen VM's migreren. | Beschikbaar
Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | In dit artikel
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app naar Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso de SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar



In dit artikel is voor Contoso de app Windows .NET SmartHotel met twee lagen die worden uitgevoerd op virtuele VMware-machines naar Azure migreren. Als u wilt deze app wilt gebruiken, wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-team leiderschap heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen stroomlijnen voor ontwikkelaars en gebruikers.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**: Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie.  Het kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: wanneer het bedrijf met succes groeit, Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde moet opgeven.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen zijn gebruikt voor het bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in VMWare.  De app blijven als kritiek in de cloud als on-premises.
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf, maar in de huidige vorm ze gewoon wilt verplaatsen veilig naar de cloud.
- De on-premises database voor de app heeft problemen met de beschikbaarheid. Ze willen zien dat dit als een cluster met hoge beschikbaarheid, met failover-functionaliteit zijn geïmplementeerd in Azure.
- Contoso wil een upgrade van hun huidige SQL Server 2008 R2-platform, naar SQL Server 2017.
- Contoso-niet wilt gebruiken van een Azure SQL Database voor deze app en er wordt gezocht naar alternatieven.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- De app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.

![Scenario-architectuur](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Management-Service](https://docs.microsoft.com/azure/dms/dms-overview) | Contoso worden gemigreerd van de gegevenslaag van de app met behulp van DMS. DMS verbinding maken met de SQLVM on-premises computer via een site-naar-site-VPN en migreren van DMS kunnen naadloze migratie van meerdere databasebronnen naar Azure-gegevensplatforms, met minimale downtime. | Meer informatie over [ondersteunde regio's](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) voor DMS en get [prijsinformatie](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Contoso wordt Site Recovery gebruiken voor een lift-and-shift-migratie van de app front-end virtuele machine. Site Recovery coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.

 

## <a name="migration-process"></a>Migratieproces

- Contoso wordt de app-VM's migreren naar Azure.
- Zij u de front-end VM migreren naar virtuele Azure-machine met behulp van Site Recovery:
    - Als eerste stap, moeten ze voorbereiden en instellen van Azure-onderdelen en voorbereiden van de on-premises VMware-infrastructuur.
    - Met alles voorbereid, kunnen ze gaan repliceren van de virtuele machine.
    - Nadat replicatie is ingeschakeld en werkt, zij de virtuele machine migreren met failover naar Azure.
- Zij u de database migreren naar een SQL Server-cluster in Azure met behulp van de Data Migration Service (DMS).
    - Als eerste stap hebt voor het inrichten van SQL Server-VM's in Azure, instellen van het cluster en een interne load balancer, en ze AlwaysOn-beschikbaarheidsgroepen configureren.
    - Dit is voldaan, kunnen ze migreren van de database
- Na de migratie schakelt ze AlwaysOn-beveiliging voor de database.

![Migratieproces](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) nodig om uit te voeren in dit scenario:

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al een abonnement hebt gemaakt wanneer u de evaluatie uitgevoerd in de eerste artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor Site Recovery.
**Site recovery (on-premises)** | Uw on-premises vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.<br/><br/> Virtuele machines die u wilt repliceren, moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).
**DMS** | Voor DMS moet u een [compatibel on-premises VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> U moet de on-premises VPN-apparaat configureren. Er moet een extern gericht openbaar IPv4-adres en het adres kan zich niet achter een NAT-apparaat.<br/><br/> Zorg ervoor dat u toegang hebt tot uw on-premises SQL Server-database.<br/><br/> De Windows Firewall moet toegang hebben tot de bron-database-engine. [Meer informatie](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Als er een firewall voor uw databasemachine, kunt u regels voor toegang tot de database, en bestanden via SMB-poort 445 toevoegen.<br/><br/> De referenties waarmee verbinding maken met de bron-SQL Server en beheerd exemplaar als doel, moeten lid zijn van de serverrol sysadmin.<br/><br/> U moet een netwerk delen in uw on-premises database die DMS gebruiken kunt om back-up van de brondatabase.<br/><br/> Zorg ervoor dat het serviceaccount waarop de SQL Server-bronexemplaar wordt uitgevoerd schrijfbevoegdheden op de netwerkshare heeft.<br/><br/> Maak een notitie van een Windows-gebruiker (en wachtwoord) die het volledige beheer op de netwerkshare heeft. Azure Database Migration Service imiteert de referenties van deze gebruiker voor het uploaden van back-upbestanden naar de Azure storage-container.<br/><br/> Het installatieproces van SQL Server Express wordt het TCP/IP-protocol ingesteld op **uitgeschakelde** standaard. Zorg ervoor dat deze ingeschakeld.


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: De SQL Server-VM's maken in Azure**: voor hoge beschikbaarheid, Contoso wilt implementeren van een geclusterde database in Azure. Deze implementeren twee SQL Server-VM's en een interne load balancer van Azure.
> * **Stap 2: Het cluster implementeren**: voorbereiden van na de implementatie van de SQL Server-VM's, een Azure SQL Server-cluster.  Zij u hun database in deze vooraf gemaakte cluster migreren.
> * **Stap 3: Voorbereiden DMS**: om voor te bereiden DMS ze zich registreren op de provider van de migratie van de Database, een DMS-exemplaar en een project maken. Ze instellen van een shared access signature (SAS) Uniform Resource Identifier (URI). DMS maakt gebruik van de SA-URI voor toegang tot de opslagaccountcontainer waarop SQL Server back-up maken van bestanden voor de service worden geüpload.
> * **Stap 4: Azure voorbereiden voor Site Recovery**: maken van een Azure storage-account voor het opslaan van gerepliceerde gegevens, en een Recovery Services-kluis.
> * **Stap 5: On-premises VMware voorbereiden voor Site Recovery**: ze accounts voorbereiden op VM-detectie- en agent-installatie, en on-premises machines voorbereiden, zodat ze verbinding met virtuele Azure-machines na een failover maken kunnen.
> * **Stap 6:-Machines repliceren**: ze replicatie-instellingen configureren en inschakelen van replicatie van virtuele machines.
> * **Stap 7: Migreren van de database met DMS**: migreren van de database.
> * **Stap 8: Migreer de virtuele machines met Site Recovery**: ze eerst een testfailover uit om te controleren of alles goed werkt, gevolgd door een volledige failover voor het migreren van de virtuele machine worden uitgevoerd.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>Stap 1: Bereid een cluster met de groep op SQL Server AlwaysOn-beschikbaarheid

Contoso wil plan vooruit door het implementeren van de database in een cluster in Azure. Dit cluster kan vervolgens worden gebruikt voor andere databases. 

- De SQL Server-VM's worden geïmplementeerd in de resourcegroep voor ContosoRG (gebruikt voor productieresources).
- Naast de unieke namen voor beide VM's dezelfde instellingen gebruikt.
- De interne load balancer wordt geïmplementeerd in de ContosoNetworkingRG (gebruikt voor netwerkresources).
- Virtuele machines worden uitgevoerd Windows Server 2016 Enterprise Edition van SQL Server 2017. Contoso geen licenties voor dit besturingssysteem, zodat ze een afbeelding in de Azure Marketplace die de licentie als de kosten voor de Azure EA-toezegging biedt gaat gebruiken.

Hier ziet hoe Contoso instellen van het cluster:

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
    - Ze een nieuwe beschikbaarheidsset maken: **SQLAOGAVSET**, met twee foutdomeinen en vijf updatedomeinen.

    ![SQL-VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. In **SQL Server-instellingen**, ze SQL-verbinding met het virtuele netwerk (persoonlijk), de limiet voor de standaardpoort 1433. Voor de verificatie ze dezelfde referenties gebruiken als ze op locatie (**contosoadmin**).

    ![SQL-VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**Meer hulp nodig?**

- [Hulp bij het](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) inrichten van een SQL Server-VM.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) virtuele machines configureren voor verschillende SQL Server-SKU's.

## <a name="step-2-deploy-the-failover-cluster"></a>Stap 2: Het failover-cluster implementeren

Hier ziet hoe Contoso instellen van het cluster:

1. Zij instellen een Azure storage-account om te fungeren als de cloud-witness.
2. Ze toevoegen de SQL Server-VM's aan de Active Directory-domein in de Contoso-on-premises datacentrum.
3. Ze maken van het cluster in Azure.
4. De cloud-witness configureren.
5. Ten slotte kunnen SQL Always On-beschikbaarheidsgroepen.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Instellen van een storage-account als cloud-witness

Als u een cloud-witness instelt, moet Contoso een Azure Storage-account dat de blob-bestand dat is gebruikt voor het cluster arbitrage kan bevatten. Hetzelfde opslagaccount kan worden gebruikt voor het instellen van cloud-witness voor meerdere clusters. 

Contoso maakt een storage-account als volgt:

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

## <a name="set-up-the-cluster"></a>Instellen van het cluster

Voordat u het cluster instelt, momentopname Contoso een van de schijf met het besturingssysteem op elke computer.

![momentopname](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. Voer vervolgens, ze een script dat ze hebt geplaatst om toepassingsgatewayresource te maken van het Windows-failovercluster.

    ![Cluster maken](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Nadat ze het cluster hebt gemaakt, controleren ze of de virtuele machines worden weergegeven als clusterknooppunten.

     ![Cluster maken](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>De cloud-witness configureren

1. Contoso configureren in de cloud-witness met behulp van de **Quorum-configuratiewizard** in Failoverclusterbeheer.
2. Ze selecteren in de wizard het maken van een cloud-witness met de storage-account.
3. Nadat de cloud-witness is geconfigureerd, wordt in weergegeven in de module Failoverclusterbeheer.

    ![Cloud-witness](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>SQL Server Always On-beschikbaarheidsgroepen inschakelen

Contoso kan nu altijd inschakelen:

1. In SQL Server Configuration Manager ze inschakelen **AlwaysOn Availability Groups** voor de **SQL Server (MSSQLSERVER)** service.

    ![Schakel AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Ze start de service voor de wijzigingen worden doorgevoerd.

Contoso kan met AlwaysOn inschakelen instellen van de AlwaysOn-beschikbaarheidsgroep die worden beveiligd door de database SmartHotel.

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) witness en het instellen van een storage-account voor de cloud.
- [Instructies](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) voor het instellen van een cluster en het maken van een beschikbaarheidsgroep.

## <a name="step-3-deploy-the-azure-load-balancer"></a>Stap 3: De Azure Load Balancer implementeren

Contoso wil implementeren van een interne load balancer die bevindt zich voor de clusterknooppunten. De load balancer luistert naar verkeer, en stuurt deze naar het juiste knooppunt.

![Taakverdeling](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Ze maken als volgt de load balancer:

1. In Azure portal > **netwerken** > **Load Balancer**, ze een nieuwe interne load balancer instellen: **ILB-PROD-DB-EUS2-SQLAOG**.
2. Deze plaatst u de load balancer in het productienetwerk **VNET-PROD-EUS2**, in het subnet van de database **PROD-DB-EUS2**.
3. Ze deze een statisch IP-adres toewijzen: 10.245.40.100.
4. Als een VPN-element, ze de load balancer in de netwerkresourcegroep implementeren **ContosoNetworkingRG**.

    ![Taakverdeling](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

Nadat de interne load balancer is geïmplementeerd, moet Contoso instellen. Ze maken van een back-endadresgroep, een statustest instellen en configureren van een regel voor taakverdeling.

### <a name="add-a-backend-pool"></a>Een back-endgroep toevoegen

Voor het distribueren van verkeer naar de virtuele machines in het cluster, instellen Contoso van een back-end-adresgroep met de IP-adressen van de NIC's voor virtuele machines die worden ontvangen van netwerkverkeer van de load balancer.

1. In de load balancer-instellingen in de portal voor Contoso een back-endpool toevoegen: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Deze koppelen pool aan de beschikbaarheidsset SQLAOGAVSET. De virtuele machines in de set (**SQLAOG1** en **SQLAOG2**) aan de groep zijn toegevoegd.

    ![Back-endpool](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Een statustest maken

Contoso wordt een statustest gemaakt zodat de load balancer de status van de app kunt bewaken. De test wordt dynamisch toegevoegd of verwijderd van virtuele machines uit de load balancer-rotatie, op basis van hoe ze op statuscontroles reageren.

Ze maken de test als volgt: 

1. In de load balancer-instellingen in de portal, maakt u een statustest voor Contoso: **SQLAlwaysOnEndPointProbe**.
2. Ze de test om te controleren van virtuele machines op TCP-poort 59999 ingesteld.
3. Ze een interval van vijf seconden tussen tests en een drempelwaarde van 2 hebt ingesteld. Als twee tests is mislukt, de virtuele machine wordt worden als slecht beschouwd.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Configureer de load balancer voor het ontvangen van verkeer


Nu Contoso moet die een load balancer-regel op defins hoe verkeer wordt gedistribueerd naar de virtuele machines.

- Het front-end-IP-adres zorgt voor inkomend verkeer.
- De back-end-IP-adresgroep ontvangt het verkeer.

Ze Maak de regel als volgt:

1. In de load balancer-instellingen in de portal, ze een nieuwe regel voor taakverdeling toevoegen: **SQLAlwaysOnEndPointListener**.
2. Contoso Hiermee stelt u een front-end-listener voor het ontvangen van binnenkomende SQL-client-verkeer op TCP 1433.
3. Deze opgeven met de back-endpool naar welk verkeer wordt gerouteerd, en de poort waarop de virtuele machines luisteren naar verkeer.
4. Contoso kunt zwevend IP (direct server return). Dit is altijd vereist voor de SQL AlwaysOn.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**Meer hulp nodig?**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) van Azure Load Balancer.
- [Meer informatie over](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) het maken van een load balancer.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>Stap 4: Azure voorbereiden voor de Site Recovery-service

Hier volgen de Azure-onderdelen die Contoso moet Site Recovery implementeren:

- Een VNet waarin virtuele machines geplaatst worden moeten nadat ze tijdens de failover maakt.
- Azure storage-account voor het opslaan van gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Ze stelt deze als volgt:

1.  Contoso is al gemaakt/subnet van een netwerk voor Site Recovery kan worden gebruikt wanneer ze [geïmplementeerd van de Azure-infrastructuur](contoso-migration-rehost-vm-sql-ag.md).

    - De app SmartHotel is een productie-app en WEBVM worden gemigreerd naar de-Azure-productienetwerk (VNET-PROD-EUS2) in de primaire regio in de VS-Oost 2.
    - WEBVM wordt geplaatst in de resourcegroep ContosoRG, die wordt gebruikt voor productieresources, en in de productie-subnet (PROD-FE-EUS2).

2. Contoso maakt een Azure storage-account (contosovmsacc20180528) in de primaire regio.

    - Ze gebruiken een account voor algemeen gebruik met de standard-opslag en LRS-replicatie.
    - Het account moet zich in dezelfde regio bevinden als de kluis.

    ![Site Recovery-opslag](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Met de netwerk- en storage-account in plaats ze nu een Recovery Services-kluis maken (**ContosoMigrationVault**), en plaats deze in de **ContosoFailoverRG** resourcegroep in de primaire regio in de VS-Oost 2 .

    ![Recovery Services-kluis](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) instellen van Azure voor Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Stap 4: On-premises VMware voorbereiden voor Site Recovery

Hier volgt een uitleg Contoso on-premises wordt voorbereid:

- Een account op de vCenter-server of vSphere ESXi-host voor het automatiseren van VM-detectie.
- Een account waarmee de automatische installatie van de Mobility-service op VMware-VM's die u wilt repliceren.
- On-premises VM-instellingen, zodat Contoso verbinding met de gerepliceerde Azure-virtuele machine na een failover maken kan.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. 
- Replicatie, failover en failback orkestreren.
- Er is minimaal een alleen-lezen-account vereist. U moet een account die bewerkingen zoals het maken en verwijderen van schijven en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt als volgt u het account:

1. Contoso maakt een rol op vCenter-niveau.
2. Contoso vervolgens toe te wijzen die rol de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer replicatie is ingeschakeld voor de virtuele machine.
- U moet een account met Site Recovery gebruiken kunt voor toegang tot de virtuele machine voor de push-installatie. U geeft dit account bij het instellen van replicatie in de Azure-console.
- Het account kan worden domein of lokale met machtigingen om te installeren op de virtuele machine.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover wil Contoso verbinding maken met virtuele Azure-machines. U doet dit door ze als volgt vóór de migratie:

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


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Stap 5: De on-premises VM's repliceren naar Azure met Site Recovery

Voordat ze kunnen een migratie uitgevoerd naar Azure, Contoso moet instellen en inschakelen van replicatie.

### <a name="set-a-replication-goal"></a>Een replicatiedoel instellen

1. In de kluis onder de kluisnaam (ContosoVMVault) ze een replicatiedoel selecteren (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden** .
2. Ze opgeven dat de machines zich op locatie, die worden uitgevoerd op VMware, en naar Azure te repliceren zijn.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze nodig hebben om te bevestigen dat ze hebben de implementatieplanning uitgevoerd, door te selecteren **Ja, heb ik gedaan**. In dit scenario Contoso zijn alleen migreren van een virtuele machine en plannen van de implementatie niet nodig.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso nodig heeft om de bronomgeving te configureren. U doet dit door ze een OVF-sjabloon downloaden en gebruiken om te implementeren van de Site Recovery-configuratieserver als een maximaal beschikbaar, on-premises VMware-VM. Nadat de configuratieserver actief is, Registreer ze deze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso uitvoeren als volgt te werk:


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

7. Nadat de registratie is voltooid, wordt in de Azure-portal, Contoso dubbele controleert dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen, en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Contoso geeft nu doel replicatie-instellingen.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Contoso kan nu een replicatiebeleid maken.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maken van een beleid **ContosoMigrationPolicy**.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**. De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Het replicatiebeleid koppelen](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Replicatie inschakelen

Contoso kan nu starten WebVM repliceren.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze geven aan dat ze willen VM's inschakelen, selecteert u de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Ze geven nu de doelinstellingen, met inbegrip van de resourcegroep en VNet en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso selecteert de WebVM voor replicatie, controleert of het replicatiebeleid voor en replicatie inschakelen. Site Recovery installeert de Mobility-Service op de virtuele machine wanneer replicatie is ingeschakeld.
 
    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
5. In **Essentials** in Azure portal, Contoso de structuur kunt bekijken voor de virtuele machines repliceren naar Azure.

    ![Infrastructuurweergave](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**Meer hulp nodig?**

- U kunt een overzicht van al deze stappen in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Vindt u meer informatie over [inschakelen van replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>Stap 5: De Database Migration Assistant (DMA) installeren

Contoso wordt de SmartHotel-database migreren naar virtuele Azure-machine **SQLAOG1** met behulp van de DMA. Ze stelt DMA als volgt in:

1. Ze het hulpprogramma downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) naar de on-premises SQL Server-VM (**SQLVM**).
2. Ze uitvoeren setup (DownloadMigrationAssistant.msi) op de virtuele machine.
3. Op de **voltooien** pagina, selecteren ze **Start Microsoft Data Migration Assistant** voordat de wizard is voltooid.

## <a name="step-6-migrate-the-database-with-dma"></a>Stap 6: De database met DMA migreren

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

## <a name="step-7-protect-the-database"></a>Stap 7: De database beveiligen

Met de app-database op **SQLAOG1**, Contoso kan nu beveiligen met behulp van AlwaysOn-beschikbaarheidsgroepen. Ze configureren met behulp van SQL Management Studio AlwaysOn en wijs een listener met behulp van Windows-clustering. 

### <a name="create-an-alwayson-availability-group"></a>Een AlwaysOn-beschikbaarheidsgroep maken

1. In SQL Management Studio, ze met de rechtermuisknop op **altijd op hoge beschikbaarheid** om te beginnen de **Wizard Nieuwe beschikbaarheidsgroep**.
2. In **beveiligingsopties opgeven**, naam van de beschikbaarheidsgroep **SHAOG**. In **Databases selecteren**, ze de SmartHotel-database selecteren.

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

Als laatste stap in het instellen van de SQL-implementatie, Contoso configureert de interne load balancer als de listener op het cluster en de listener online brengt. Ze een script gebruiken om dit te doen.

![Cluster-listener](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>Controleer de configuratie

Alles wat u instelt, hebben Contoso nu een functionele beschikbaarheidsgroep in Azure die gebruikmaakt van de gemigreerde database. Ze controleren dit door verbinding te maken met de interne load balancer in SQL Management Studio.

![ILB-verbinding](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**Meer hulp nodig?**
- Meer informatie over het maken van een [beschikbaarheidsgroep](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) en [listener](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- Handmatig [het cluster ingesteld voor het gebruik van de IP-adres van de load balancer](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [Meer informatie](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) over het maken en met behulp van SAS.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>Stap 8: Migreer de virtuele machine met Site Recovery

Contoso uitvoeren van een snelle testfailover, en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitvoeren om te controleren of alles werkt zoals verwacht vóór de migratie. 

1. Contoso wordt een testfailover uitgevoerd naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 

    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Contoso controleert dat de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, Contoso opschonen van de failover, en noteer eventuele opmerkingen en sla. 

### <a name="run-a-failover"></a>Een failover uitvoeren

1. Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, Contoso maken van een herstelplan voor migratie en WEBVM toevoegen aan het plan.

     ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Ze een failover worden uitgevoerd op het plan. Ze het meest recente herstelpunt selecteren en opgeven dat Site Recovery proberen moet de on-premises VM afsluiten voordat de failover wordt geactiveerd.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Na de failover controleren ze of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

    ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Nadat u hebt de virtuele machine in Azure gecontroleerd, voltooien ze van de migratie voor het voltooien van het migratieproces, replicatie voor de virtuele machine stoppen en Site Recovery-facturering voor de virtuele machine stoppen.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

Contoso bijwerken als de laatste stap in het migratieproces, de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde database die wordt uitgevoerd op de listener SHAOG. Deze configuratie wordt gewijzigd op de WEBVM nu uitgevoerd in Azure.  Deze configuratie bevindt zich in het bestand web.config van de ASP-toepassing. 

1. Zoek het bestand op C:\inetpub\SmartHotelWeb\web.config.  Wijzig de naam van de server in overeenstemming met de FQDN-naam van de AOG: shaog.contoso.com.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Na het bijwerken van het bestand en op te slaan, ze IIS opnieuw starten op WEBVM. Ze doen dit met behulp van de IISRESET /RESTART vanaf een opdrachtprompt.
2. Nadat IIS opnieuw is gestart, wordt de database die wordt uitgevoerd op de SQL-MI nu gebruik van de toepassing.


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) waarop een test-failover wordt uitgevoerd. 
- [Informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) over het maken van een plan voor herstel.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Failover-overschakeling uitvoeren naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Na de migratie, de SmartHotel-app wordt uitgevoerd op een Azure VM en de database SmartHotel bevindt zich in de Azure SQL-cluster.

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

- Ze controleren de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machine om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan voor de toepassing kan worden doorgegeven.
- Ze overweegt de gegevens op de schijf met behulp van Azure Disk Encryption en Key Vault te beveiligen.
- Ze moeten evalueren transparante gegevensversleuteling (TDE) en daarna inschakelen op de SmartHotel-database op de nieuwe SQL AOG. [Meer informatie](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

Contoso wil back-up van de gegevens op WEBVM en SQLAOG1 SQLAOG2 met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

1. Contoso heeft een bestaande licentie voor hun WEBVM en gebruik van Azure Hybrid Benefit.  Ze u de bestaande Azure-VM's om te profiteren van deze prijzen zijn overgestapt.
2. Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management. 

## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso de SmartHotel-app in Azure door het app-front-end virtuele machine migreren naar Azure met behulp van de Site Recovery-service. Ze de app-database migreren naar een SQL Server-cluster dat is ingericht in Azure en deze in een SQL Server AlwaysOn-beschikbaarheidsgroep worden beveiligd.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks, we laten zien hoe Contoso rehost voor hun servicedesk osTicket-app die wordt uitgevoerd op Linux en geïmplementeerd met een MySQL-database.


