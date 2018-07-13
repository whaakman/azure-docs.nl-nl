---
title: On-premises workloads evalueren voor Contoso migratie naar Azure | Microsoft Docs
description: Informatie over hoe Contoso beoordeelt hun on-premises computers voor migratie naar Azure met Azure-migratie en Database-migratie
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: fa6fb4ffe1eea98392b2199f379431b0dffc6774
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006563"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migratie van Contoso: on-premises workloads evalueren voor migratie naar Azure

Dit artikel leest u hoe Contoso beoordeelt de app in de on-premises SmartHotel, ter voorbereiding op voor de migratie naar Azure.

Dit document is de derde in de serie artikelen waarbij document hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en een reeks implementatiescenario's die laten hoe u zien voor het instellen van de infrastructuur van een migratie, de geschiktheid van on-premises bronnen voor migratie beoordelen en verschillende typen migraties worden uitgevoerd. Scenario's toeneemt in complexiteit en we hierna zullen toevoegen aanvullende artikelen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | In dit artikel.
[Artikel 4: Een app op Azure VM's en een beheerde SQL-exemplaar opnieuw hosten](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso de app SmartHotel virtuele machines migreren naar Azure-VM's, met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar


## <a name="overview"></a>Overzicht

Als de migratie naar Azure overweegt, wil Contoso bedrijf een technische en financiële evaluatie uitvoeren om te bepalen of de on-premises werkbelastingen wel geschikt voor migratie naar de cloud. In het bijzonder wil Contoso het team beoordelen van machine- en databasecompatibiliteit voor migratie en een schatting maken van capaciteit en kosten voor het uitvoeren van hun bronnen in Azure.

Praktijkervaring en beter begrip van de betreffende technologieën, gaan ze beoordelen twee van hun on-premises toepassingen worden samengevat in de volgende tabel. Houd er rekening mee dat ze die rehost en herstructureren apps voor de migratie voor migratiescenario's evalueren wilt. Meer informatie over het opnieuw hosten en herstructurering de [Contoso Migratieoverzicht](contoso-migration-overview.md).

**Naam van app** | **Platform** | **App-lagen** | **Details**
--- | --- | --- | ---
SmartHotel<br/><br/> Vereisten voor Contoso reizen beheert | Uitgevoerd op Windows met een SQL Server-database | App met de front-end ASP.NET-website die wordt uitgevoerd op een virtuele machine (WEBVM) en de SQL-Server op een andere virtuele machine (SQLVM) met twee lagen | VM's zijn VMware, die worden uitgevoerd op een ESXi host die wordt beheerd door vCenter-server.<br/><br/> De voorbeeld-app kan worden gedownload vanaf [GitHub](https://github.com/Microsoft/SmartHotel360).
OSTicket<br/><br/> Helpdesk-app van Contoso-Service | Uitgevoerd op Linux/Apache, met een MySQL-PHP (LAMP). | Twee lagen-app met een front-PHP-website op een virtuele machine (OSTICKETWEB) en de MySQL-database die wordt uitgevoerd op een andere virtuele machine (OSTICKETMYSQL) | De app wordt gebruikt door de klant service-apps voor het bijhouden van problemen voor interne werknemers en externe klanten.<br/><br/> De voorbeeld-app kan worden gedownload vanaf [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Huidige architectuur


Hier volgt een diagram van de huidige on-premises infrastructuur voor Contoso.

![Contoso-architectuur](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso heeft een hoofddatacenter zich bevindt in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Deze hebt u drie extra lokale branches in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet met een fiber metro Ethernet-verbinding (500 mbps).
- Elke vertakking is lokaal verbonden met internet met behulp van zakelijke klasse verbindingen met IPSec VPN-tunnels terug naar het primaire datacenter. Hiermee wordt het hele netwerk permanent zijn verbonden, en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter is volledig gevirtualiseerd met VMware. Ze hebben twee ESXi 6.5-virtualisatiehosts, beheerd door vCenter Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteits- en DNS-servers in het interne netwerk.
- De domeincontrollers in het datacenter worden uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale branches uitgevoerd op fysieke servers.





## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat het bedrijf wil bereiken met deze migratie:

- **Adres van de groei van het bedrijf**: Contoso wordt uitgebreid en er wordt als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen voor ontwikkelaars en gebruikers van de stroomlijnen.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**: Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie.  Het kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: wanneer het bedrijf met succes groeit, Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde moet opgeven.

## <a name="assessment-goals"></a>Evaluatie van de doelstellingen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor beoordelingen van hun migratie:

- Na de migratie hebt apps in Azure dezelfde prestatiemogelijkheden als in hun on-premises VMWare-omgeving.  Verplaatsen naar de cloud betekent niet dat de app-prestaties minder kritiek is.
- Contoso moet informatie over de compatibiliteit van hun toepassingen en databases met Azure-vereisten, evenals hun hostingopties in Azure.
- Beheer van Contoso database moet worden geminimaliseerd nadat apps zijn verplaatst naar de cloud.  
- Contoso wil weten niet alleen de opties voor de migratie, maar ook de kosten die zijn gekoppeld aan de infrastructuur, nadat deze verplaatst naar de cloud.

## <a name="assessment-tools"></a>Analyseprogramma 's
Contoso maakt gebruik van Microsoft-hulpprogramma's voor de evaluatie. Deze hulpprogramma's zijn afgestemd op hun doelstellingen en moeten bieden met alle informatie die ze nodig hebben.

**Technologie** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Ze DMA gebruikt om te beoordelen en detecteert compatibiliteitsproblemen die mogelijk van invloed zijn op de databasefunctionaliteit in Azure. DMA de functiepariteit tussen SQL-bronnen en doelen beoordeelt en wordt aanbevolen verbeteringen in de prestaties en betrouwbaarheid. | Het is een gratis hulpprogramma dat kan worden gedownload.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso, wordt deze service gebruiken om hun virtuele VMware-machines vast te stellen. De service evalueert de geschiktheid voor migratie van de machines en biedt formaat- en kostenramingen voor de uitvoering in Azure.  | Er is momenteel (mei 2018) geen kosten voor het gebruik van deze service.
[Serviceoverzicht](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Migrate maakt gebruik van Servicetoewijzing om afhankelijkheden weer te geven tussen de computers die u wilt migreren. |  Servicetoewijzing is een onderdeel van Azure Log Analytics. Het kan momenteel gedurende 180 dagen kosteloos worden gebruikt.

In dit scenario, Contoso gedownload en de DMA voor het evalueren van de on-premises SQL Server-database voor de reis-app wordt uitgevoerd. Gebruik van Azure migrate met afhankelijkheidstoewijzing om de app-VM's, voordat de migratie naar Azure vast te stellen.



## <a name="assessment-architecture"></a>Evaluatie-architectuur


![Beoordelingsarchitectuur voor migratie](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso is de naam van een fictieve dat een typische bedrijfsorganisatie vertegenwoordigt.
- Contoso heeft een on-premises datacenter (**contoso-datacenter**), met on-premises domeincontrollers (CONTOSODC1, CONTOSODC2).
- Virtuele VMware-machines bevinden zich op een VMware ESXI-hosts met versie 6.5. Hosts: **contosohost1**, **contosohost2**
- De VMware-omgeving wordt beheerd door vCenter-server 6.5 (**venter**uitgevoerd op een virtuele machine.
- De SmartHotel reis-app:
    - De app is gelaagd over twee virtuele machines van VMware, **WEBVM** en **SQLVM**.
    - De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com**.
    - De virtuele machines worden uitgevoerd van Windows Server 2008 R2 Datacenter met SP1.
- De VMware-omgeving wordt beheerd door vCenter Server (**vcenter.contoso.com**) die wordt uitgevoerd op een virtuele machine.
- De OSTicket helpdesk-app Service:
    - De app is gelaagd over twee virtuele machines, **OSTICKETWEB** en **OSTICKETMYSQL**.
    - De virtuele machines worden uitgevoerd op Ubuntu Linux-Server 16.04-LTS.
    - De OSTICKETWEB-VM wordt uitgevoerd, Apache 2 en PHP 7.0.
    - De VM OSTICKETMYSQL wordt MySQL 5.7.22 uitgevoerd.

![Architectuur](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>Vereisten

Hier volgt Contoso (en u) voor de evaluatie moet:

- Eigenaar of bijdrager toegang voor het Azure-abonnement of voor een resourcegroep in het Azure-abonnement.
- Een on-premises vCenter-server waarop versie 5.5, 6.0 of 6.5 wordt uitgevoerd.
- Een alleen-lezen-account op de vCenter-server of machtigingen om er een te maken.
- Machtigingen om een virtuele machine te maken op de vCenter-server met behulp van een OVA-sjabloon.
- Ten minste één ESXi-host waarop versie 5.0 of hoger wordt uitgevoerd.
- Ten minste twee on-premises virtuele VMware-machines, en op één daarvan moet een SQL Server-database worden uitgevoerd.
- Machtigingen voor het Azure Migrate-agents installeren op elke virtuele machine.
- De virtuele machines moeten directe verbinding met internet hebben.
        - U kunt de toegang tot internet beperken de [vereiste URL's](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).
        -Als machines zonder internetverbinding, de [OMS-gateway](../log-analytics/log-analytics-oms-gateway.md) moet worden geïnstalleerd op deze.
- De FQDN-naam van de virtuele machine waarop het SQL Server-exemplaar wordt uitgevoerd. Deze wordt gebruikt voor database-evaluatie.
- Windows Firewall op de SQL Server-VM moet externe verbindingen toestaan via TCP-poort 1433 (standaard), zodat de DMA verbinding kan maken.


## <a name="assessment-overview"></a>Evaluatie, overzicht

Hier volgen enkele redenen hoe Contoso is gaan doen de evaluatie:


> [!div class="checklist"]
> * **Stap 1: Download en installeer DMA**: bereid DMA voor evaluatie van de on-premises SQL Server-database.
> * **Stap 2: De database met DMA beoordelen**: uitvoeren en analyseren van evaluatie van de database.
> * **Stap 3: Voorbereiden voor evaluatie van de virtuele machine met Azure Migrate**: instellen van on-premises-accounts en Verfijn de VMware-instellingen.
> * **Stap 4: On-premises machines met Azure Migrate detecteren**: een Azure Migrate collector-VM maken. Ze voer vervolgens de collector voor het detecteren van virtuele machines voor evaluatie.
> * **Stap 5: Afhankelijkheidsanalyse met Azure Migrate voorbereiden**: Installeer Azure Migrate-agents op de virtuele machines, zodat ze afhankelijkheidstoewijzing tussen virtuele machines kunnen zien.
> * **Stap 6: De virtuele machines met Azure Migrate beoordelen**: Controleer afhankelijkheden, groepeer de virtuele machines en uitvoeren van de evaluatie. Nadat de evaluatie uitgevoerd is, analyseer ze deze in voorbereiden voor migratie.


## <a name="step-1-download-and-install-the-dma"></a>Stap 1: Download en installeer de DMA

1. Contoso downloadt de DMA via de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - De assistent kan worden geïnstalleerd op elke computer die u verbinding met het SQL-exemplaar maken kunt. U hoeft deze niet uit te voeren op de SQL Server-computer.
    - Deze mag niet worden uitgevoerd op de SQL Server-hostcomputer.
2. Ze het gedownloade installatiebestand (DownloadMigrationAssistant.msi) om de installatie te starten worden uitgevoerd.
3. Op de **voltooien** pagina, selecteren ze **Start Microsoft Data Migration Assistant** voordat de wizard is voltooid.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Stap 2: Uitvoeren en analyseren van de evaluatie van de database voor SmartHotel

Contoso kan nu een evaluatie voor het analyseren van hun on-premises SQL Server voor de app SmartHotel uitvoeren.

1. Klik in de Database Migration Assistant, ze op **nieuw**, selecteer **evaluatie**, en geef de evaluatie van de naam van het project - **SmartHotel**.
2. Ze selecteert de **bronservertype** als **SQL Server op Azure Virtual Machines**.

    ![Bron selecteren](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Op dit moment biedt DMA geen ondersteuning voor evaluatie voor migratie naar een SQL beheerd exemplaar. Als tijdelijke oplossing, Contoso is met behulp van SQL Server op Azure-VM als de veronderstelde doel voor de evaluatie.

3. In **doelversie selecteren**, ze SQL Server 2017 als de doelversie selecteren. Ze moeten dit selecteren omdat het de versie die wordt gebruikt door SQL Managed Instance.
4. Ze selecteren voor het detecteren van informatie over de compatibiliteit en nieuwe functies:
    - **Compatibiliteitsproblemen** Houd er rekening mee wijzigingen die kan defect raken migratie of waarvoor een kleine aanpassing vóór de migratie. Het blijft u op de hoogte van alle functies die momenteel in gebruik die zijn afgeschaft. Problemen zijn op compatibiliteitsniveau ingedeeld.
    - **Aanbeveling voor nieuwe functies** opmerkingen bij de nieuwe functies in de doel-SQL Server-platform die kunnen worden gebruikt voor de database na de migratie. Deze zijn ingedeeld in Prestaties, Beveiliging en Opslag.

    ![Doel selecteren](./media/contoso-migration-assessment/dma-assessment-2.png)

2. In **verbinding maken met een server**, ze de naam van de virtuele machine met de database en de referenties voor toegang tot deze invoeren. Ze nodig hebben om in te schakelen **servercertificaat vertrouwen** om ervoor te zorgen dat ze toegang hebt tot de SQL-Server. En vervolgens klikt de gebruiker op **Connect**.

    ![Doel selecteren](./media/contoso-migration-assessment/dma-assessment-3.png)

3. In **Add source**, ze de database die ze willen beoordelen, en klik op toevoegen **volgende** starten van de evaluatie.
4. De evaluatie is gemaakt.

    ![Evaluatie maken](./media/contoso-migration-assessment/dma-assessment-4.png)

5. In **beoordelingsresultaten**, zien ze de resultaten van de evaluatie.


### <a name="analyze-the-database-assessment"></a>De evaluatie van de database analyseren

Resultaten worden weergegeven zodra ze beschikbaar zijn. Als het oplossen van problemen moet worden geklikt **evaluatie opnieuw starten** de evaluatie opnieuw uit te voeren.

1. In de **compatibiliteitsproblemen** rapporteren, ze controleren of er problemen zijn op elk compatibiliteitsniveau. Compatibiliteitsniveaus worden als volgt aan SQL Server-versies toegewezen:

    - 100: SQL Server 2008/Azure SQL Database
    - 110: SQL Server 2012/Azure SQL Database
    - 120: SQL Server 2014/Azure SQL Database
    - 130: SQL Server 2016/Azure SQL Database
    - 140: SQL Server 2017/Azure SQL Database

    ![Compatibiliteitsproblemen](./media/contoso-migration-assessment/dma-assessment-5.png)

2. In de **Functieaanbevelingen** rapporteren, Contoso functies voor prestaties, beveiliging en opslag die door de evaluatie wordt aanbevolen na de migratie kunt bekijken. Tal van functies worden aanbevolen, waaronder In-Memory OLTP en Columnstore, Stretch Database, Always Encrypted, Dynamic Data Masking en Transparent Data Encryption (TDE).

    ![Functieaanbevelingen](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > We raden aan dat Contoso [kunt TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) voor alle SQL Server-databases, en dit is zelfs belangrijker wanneer databases worden in de cloud. TDE moet alleen worden ingeschakeld na de migratie. Als TDE is ingeschakeld, moet u overstappen van het certificaat of asymmetrische sleutel met de hoofddatabase van de doelserver. [Meer informatie](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Ze kunnen de evaluatie in JSON- of CSV-indeling exporteren.

Houd er rekening mee dat als u een evaluatie op grotere schaal uitvoert kunt:

- Meerdere evaluaties tegelijk uitvoeren en de status van de evaluaties weergeven door het openen van de **alle evaluaties** pagina.
- [Evaluaties samenvoegen in een SQL Server-database](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Evaluaties samenvoegen in een PowerBI-rapport](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>Stap 3: De evaluatie van de VM met Azure Migrate voorbereiden

Contoso moet maken van een VMware-account die Azure Migrate gebruiken voor het automatisch detecteren van virtuele machines voor evaluatie, controleert u of machtigingen voor het maken van een virtuele machine, houd er rekening mee de poorten die moeten worden geopend en stel de statistieken niveau van de instellingen.

### <a name="set-up-a-vmware-account"></a>Een VMware-account instellen

 VM-detectie vereist een alleen-lezen-account in vCenter, met de volgende eigenschappen:

- Gebruikerstype: minimaal een alleen-lezen-gebruiker.
- Machtigingen: Datacentrumobject –> Doorgeven naar onderliggend object, rol=Alleen-lezen.
- Details: gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.
- Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).

### <a name="verify-permissions-to-create-a-vm"></a>Machtigingen voor het maken van een virtuele machine verifiëren

Contoso controleert of er machtigingen voor het maken van een virtuele machine door het importeren van een bestand in. OVA-indeling. [Meer informatie](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Poorten verifiëren

Afhankelijkheidstoewijzing maakt gebruik van de Contoso-evaluatie. Deze functie moet een agent is geïnstalleerd op VM's die u wilt evalueren. De agent moet kunnen verbinding maken met Azure via TCP-poort 443 op elke virtuele machine. [Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) over verbindingsvereisten.


### <a name="set-statistics-settings"></a>Instellingen opgeven voor statistieken

Voordat ze de implementatie, moet de instellingen voor statistieken voor de vCenter-Server op niveau 3 ingesteld door Contoso. Houd rekening met het volgende:

- Na het instellen van het niveau, moet u wacht ten minste een dag voordat u de evaluatie uitvoert. Anders werkt deze mogelijk niet zoals verwacht.
- Als het niveau hoger is dan 3, werkt de beoordeling wel, maar:
    - Prestatiegegevens voor schijven en netwerken worden niet verzameld.
    - Voor opslag wordt door Azure Migrate een standaardschijf in Azure aanbevolen die even groot is als de on-premises schijf.
    - Voor netwerken wordt voor elke on-premises netwerkadapter een netwerkadapter in Azure aanbevolen.
    - Voor rekencapaciteit kijkt Azure Migrate naar de VM-kernen en de geheugengrootte en wordt een Azure-VM met dezelfde configuratie aanbevolen. Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.
- [Meer informatie](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) over groottebepaling op niveau 3.

Ze op het niveau als volgt instellen:

1. Ze open het vCenter server-exemplaar in de vSphere-webclient.
2. In **beheren** > **instellingen** > **algemene**, klikt de gebruiker op **bewerken**.
3. In **statistieken**, ze ingesteld de statistiek niveau-instellingen op **niveau 3**.

    ![Niveau van vCenter-statistieken](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>Stap 4: VM's detecteren

Contoso maakt voor het detecteren van virtuele machines, een Azure Migrate-project. Ze downloaden en instellen van de collector-VM en voer de verzamelaar voor het detecteren van hun on-premises VM's.

### <a name="create-a-project"></a>Een project maken

1. In de [Azure-portal](https://portal.azure.com), ze zoeken **Azure Migrate**, en maak een project (ContosoMigration).
2. Ze geven een projectnaam, de Azure-abonnement, en maken van een nieuwe Azure-resourcegroep, **ContosoFailoverRG**. Houd rekening met het volgende:

    - U kunt alleen een Azure Migrate-project maken in de regio West-centraal VS of VS - oost.
    - U kunt een migratie plannen voor elke doellocatie.
    - De projectlocatie wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines.

    ![Azure Migrate](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Azure Migrate maakt een on-premises virtuele machine die het collector-apparaat wordt genoemd. Deze virtuele machine detecteert on-premises virtuele VMware-machines en stuurt metagegevens van deze machines naar de service Azure Migrate. Als u het collector-apparaat instelt, Contoso downloadt een. OVA-sjabloon en importeert deze met de on-premises vCenter-server om de VM te maken.

1. In de Azure Migrate-project > **aan de slag** > **detecteren en evalueren** > **Machines detecteren**, downloaden ze de. Sjabloon voor OVA-bestanden.
2. Ze Kopieer de project-ID en -sleutel. Deze zijn nodig voor het configureren van de collector.

    ![.OVA-bestand downloaden](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Voordat u de virtuele machine implementeert, Contoso controleert de. OVA-bestand is beveiligd.

1. Ze open een opdrachtvenster voor beheerders op de computer waarop het bestand gedownload.
2. Ze worden uitgevoerd de volgende opdracht voor het genereren van de hash voor het ova-bestand:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen (versie 1.0.9.12)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | d0363e5d1b377a8eb08843cf034ac28a
SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

### <a name="create-the-collector-appliance"></a>Het collector-apparaat maken

Contoso kan nu het gedownloade bestand importeren in de vCenter-Server en de configuratieserver VM inrichten.

1. Klik in de vSphere Client-console op deze **bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/contoso-migration-assessment/vcenter-wizard.png)

2. In de Wizard OVF-sjabloon implementeren > **bron**, ze Geef de locatie van de. OVA-bestand.
3. In **naam en locatie**, ze Geef een beschrijvende naam voor de collector-VM en het inventarisobject waarin de virtuele machine wordt gehost. Ze geven ook de host of cluster op waarop de collector-apparaat wordt uitgevoerd.
5. In **opslag**, opgegeven de opslaglocatie in **schijfindeling**, hoe ze willen richt de opslagruimte.
7. In **netwerktoewijzing**, ze waarmee de collector-VM verbinding maken via het netwerk opgeven. Het netwerk moet verbinding hebben met internet om metagegevens te kunnen verzenden naar Azure.
8. Ze Controleer de instellingen en selecteer **inschakelen na de implementatie**> **voltooien**. Nadat het apparaat is gemaakt, wordt een bericht weergegeven dat alles met succes is voltooid.

### <a name="run-the-collector-to-discover-vms"></a>De collector uitvoeren om virtuele machines te detecteren

Nu ze de collector voor het detecteren van virtuele machines worden uitgevoerd. Houd er rekening mee dat de collector momenteel alleen 'Engels (Verenigde Staten)' als de taal van het besturingssysteem en de taal van de collector-interface ondersteunt.

1. In de vSphere Client-console > **Console openen**, ze geven de taal, de tijdzone en het wachtwoord voorkeuren voor de collector-VM.
2. Op het bureaublad, klikt de gebruiker op de **collector uitvoeren** snelkoppeling.

    ![Snelkoppeling naar collector](./media/contoso-migration-assessment/collector-shortcut.png)

4. In de Azure Migrate Collector > **vereisten instellen**, ze accepteer de licentievoorwaarden en lees de informatie van derden.
5. De collector controleert of de virtuele machine heeft toegang tot internet, zodat de tijd worden gesynchroniseerd en of de collector-service wordt uitgevoerd (dit wordt standaard op de virtuele machine geïnstalleerd). Het installeert ook VMWare PowerCLI.

    > [!NOTE]
    > We gaan ervan uit dat de virtuele machine rechtstreeks toegang heeft tot internet, zonder een proxy.

    ![Vereisten verifiëren](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. In **opgeven van de vCenter-Serverdetails**, ze de FQDN-naam of IP-adres van de vCenter-server opgeven en de alleen-lezen referenties gebruikt voor detectie.
7. Ze selecteert u een bereik voor VM-detectie. De collector kan alleen virtuele machines detecteren binnen het opgegeven bereik. U kunt het bereik instellen op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1500 virtuele machines bevatten.

    ![Verbinding maken met vCenter](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. In **migratieproject opgeven**, ze de Azure Migrate project-ID en de sleutel die is opgehaald uit de portal opgeven. U vindt deze opnieuw in het project **overzicht** pagina > **Machines detecteren**.  

    ![Verbinding maken met Azure](./media/contoso-migration-assessment/collector-connect-azure.png)

7. In **voortgang van verzamelen weergeven** Contoso bekijkt u de detectie en controleren van de metagegevens die worden verzameld van de virtuele machines zijn binnen het bereik. De collector geeft aan hoe lang de detectie ongeveer zal duren.

    ![Bezig met verzamelen](./media/contoso-migration-assessment/collector-collection-process.png)



### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

Nadat de verzameling is voltooid, wordt door Contoso controleert dat de virtuele machines worden weergegeven in de portal.

1. In de Azure Migrate-project > **beheren** > **Machines**, ze controleren of de virtuele machines die u wilt detecteren worden weergegeven.

    ![Gedetecteerde computers](./media/contoso-migration-assessment/discovery-complete.png)

3. Let wel: op de VM's zijn momenteel geen Azure Migrate-agents geïnstalleerd. Contoso moet voor het installeren van de volgende afhankelijkheden weer te geven.

    ![Gedetecteerde computers](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>Stap 5: Afhankelijkheidsanalyse voorbereiden

Als u wilt weergeven van afhankelijkheden tussen virtuele machines die Contoso wilt openen, worden ze downloaden en installeren van agents op de VM's van de app. Contoso heeft dit op alle virtuele machines voor hun apps, zowel Windows als Linux.

### <a name="take-a-snapshot"></a>Een momentopname maken

Ze Bewaar een kopie van de virtuele machine voordat u deze wijzigt door het maken van een momentopname voordat de agents zijn geïnstalleerd.

![Momentopname van VM](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren

1. Op de **Machines** pagina, ze de virtuele machine, selecteren en vervolgens **vereist installatie** in de **afhankelijkheden** kolom.
2. Op de **Machines detecteren** pagina ze het volgende doen:
    - Download de MMA- en afhankelijkheidsmonitors-agent voor elke Windows-VM
    - Download de agent MMA en afhankelijkheden voor elk Linux-VM
3. Nu kopieer ze de werkruimte-ID en -sleutel. Ze nodig deze hebt bij het installeren van de MMA.

    ![Agent downloaden](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>De agents installeren op Windows-VM 's

Ze uitvoeren de installatie op elke virtuele machine.

#### <a name="install-the-mma-on-windows-vms"></a>De MMA installeren op Windows-VM 's

1. Ze Dubbelklik op de gedownloade agent.
2. In **doelmap**, ze desgewenst de standaardinstallatiemap > **volgende**.
2. In **installatieopties voor Agent**, selecteren ze **de agent verbinden met Azure Log Analytics** > **volgende**.

    ![MMA-installatie](./media/contoso-migration-assessment/mma-install.png)

5. In **Azure Log Analytics**, ze plakken in de werkruimte-ID en de sleutel die u hebt gekopieerd uit de portal.

    ![MMA-installatie](./media/contoso-migration-assessment/mma-install2.png)

6. In **gereed voor installatie**, de MMA kan nu worden geïnstalleerd.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>De afhankelijkheidsagent installeren op Windows-VM 's

1. Ze dubbelklikt u op de gedownloade afhankelijkheidsagent.
2. Ze accepteer de licentievoorwaarden en wacht totdat de installatie is voltooid.

    ![Agent voor afhankelijkheden](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>De agents installeren op virtuele Linux-machines

Ze uitvoeren de installatie op elke virtuele machine.

#### <a name="install-the-mma-on-linux-vms"></a>De MMA installeren op virtuele Linux-machines

1. De python-clientbibliotheek voor ctypes worden geïnstalleerd op elke virtuele machine met behulp: **sudo apt-get installeren python-ctypeslib**.
2. Ze moeten de opdracht voor het installeren van de MMA-agent als root worden uitgevoerd.  Hoofdmap uitvoeren worden de volgende opdracht en voer het hoofdwachtwoord: **sudo -i**.
3. Nu dat ze de MMA-agent installeren.
    - De juiste werkruimte-ID en sleutel invoegen in de opdracht.
    - Opdrachten zijn voor 64-bits.
    - De **werkruimte-ID** en **primaire sleutel** vindt u in de OMS-Portal > **instellingen**, in de **verbonden bronnen** tabblad.
    - Voer de volgende opdrachten voor het downloaden van de OMS-agent, het valideren van de controlesom en installatie/vrijgeven de agent.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```



#### <a name="install-the-dependency-agent-on-linux-vms"></a>Installeer de agent voor afhankelijkheden op virtuele Linux-machines

Nadat de MMA is geïnstalleerd, kunt Contoso de agent voor afhankelijkheden installeren op de virtuele Linux-machines.

1. De Agent voor afhankelijkheden is geïnstalleerd op Linux-computers met behulp van InstallDependencyAgent-Linux64.bin, een shell-script met een zichzelf uitpakkend binair bestand. Ze kunnen het bestand uitvoeren met behulp van sh of toevoegen uitvoermachtigingen naar het bestand zelf.

2. Installatie van de agent voor Linux-afhankelijkheden als hoofdmap:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Stap 6: Uitvoeren en analyseren van evaluatie van de VM

Contoso kan nu Controleer VM-afhankelijkheden en maak een groep. Ze voer vervolgens de beoordeling van de groep.

### <a name="verify-dependencies-and-create-a-group"></a>Controleer afhankelijkheden en maak een groep


1. Voor de machines te analyseren, klikt de gebruiker op **afhankelijkheden weergeven**.

    ![Machineafhankelijkheden weergeven](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Voor de SQLVM bevat de afhankelijkheidskaart de volgende details:

    - Procesgroepen/processen met actieve netwerkverbindingen die worden uitgevoerd op SQLVM, tijdens de opgegeven periode (standaard een uur)
    - Inkomende (client) en uitgaande (server) TCP-verbindingen naar en van alle afhankelijke machines.
    - Afhankelijke machines waarop de Azure Migrate-agents zijn geïnstalleerd, worden als afzonderlijke vakken weergegeven
    - Voor machines waarop geen agents zijn geïnstalleerd, worden poort en IP-adres weergegeven.

3. Voor machines waarop de agent is geïnstalleerd (WEBVM) klikt ze u op het VM-vak om meer informatie, waaronder de FQDN-naam, het besturingssysteem en de MAC-adres weer te geven.

    ![Groepsafhankelijkheden weergeven](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Ze nu selecteren voor de virtuele machines om toe te voegen aan de groep (SQLVM en WEBVM).  Ze kunnen gebruik CTRL + klikken om te selecteren van meerdere virtuele machines.
5. Ze klikt u op **groep maken**, en geef een naam op (smarthotelapp).

> [!NOTE]
    > Voor gedetailleerdere afhankelijkheden kunt u het tijdsbereik uitbreiden. U kunt een specifieke duur of begin- en einddatums selecteren.


### <a name="run-an-assessment"></a>Een evaluatie uitvoeren


1. Op de **groepen** pagina, ze de groep (smarthotelapp) openen en op **evaluatie maken**.

    ![Een evaluatie maken](./media/contoso-migration-assessment/run-vm-assessment.png)

2. De evaluatie wordt weergegeven op de pagina **Evaluaties** > **beheren**.

Contoso gebruikt de standaardinstellingen van de evaluatie, maar u kunt instellingen aanpassen. [Meer informatie](how-to-modify-assessment.md).



### <a name="analyze-the-vm-assessment"></a>De evaluatie van de VM analyseren

Een Azure Migrate-evaluatie bevat de informatie over de compatibiliteit van de on-premises virtuele machines voor Azure, juiste formaat voor virtuele Azure-machine voorgesteld en de geschatte maandelijkse kosten van Azure.

![Evaluatierapport](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Betrouwbaarheidsclassificatie controleren

![Evaluatieweergave](./media/contoso-migration-assessment/assessment-display.png)

Een evaluatie krijgt een betrouwbaarheidsclassificatie van 1 ster tot 5 sterren (1 ster de laagste en 5 sterren de hoogste).
- De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen.
- Aan de hand van deze classificatie kunt u beter de betrouwbaarheid inschatten van de aanbevelingen voor de grootte die Azure Migrate geeft.
- Betrouwbaarheidsclassificatie is handig als u de *prestatie gebaseerde schaling* omdat Azure Migrate mogelijk niet voldoende gegevenspunten groottebepaling op basis van gebruik van doen. Voor *het instellen van de grootte op basis van 'zoals on-premises'* is de betrouwbaarheidsclassificatie altijd 5 sterren, omdat Azure Migrate alle benodigde gegevenspunten heeft om de grootte van de VM in te stellen.
- De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

#### <a name="verify-readiness"></a>Gereedheid controleren

![Gereedheid evalueren](./media/contoso-migration-assessment/azure-readiness.png)  

Het evaluatierapport toont de informatie die in de tabel is samengevat. Azure Migrate heeft de volgende informatie nodig om een op prestaties gebaseerde groottebepaling te kunnen weergeven. Als deze informatie niet kan worden verzameld, is de evaluatie van de grootte mogelijk niet juist.

- Verbruiksgegevens voor CPU en geheugen.
- Lees-/schrijf-IOPS en doorvoer voor elke schijf die is gekoppeld aan de VM.
- Informatie over netwerk-in/uit voor elke netwerkadapter die aan de VM is gekoppeld.


**Instelling** | **Indicatie** | **Details**
--- | --- | ---
**Gereed voor Azure van VM** | Geeft aan of de virtuele machine gereed is voor migratie | Mogelijke statussen:</br><br/>- Gereed voor Azure<br/><br/>- Gereed met voorwaarden <br/><br/>- Niet gereed voor Azure<br/><br/>- Gereedheid onbekend<br/><br/> Als een VM niet gereed is, ziet u met welke stappen u dit kunt verhelpen.
**Azure VM-grootte** | Voor virtuele machines die gereed zijn, wordt een Azure VM-grootte aanbevolen. | De aanbevolen grootte is afhankelijk van evaluatie-eigenschappen:<br/><br/>- Als u groottebepaling op basis van prestaties hebt gebruikt, wordt de prestatiegeschiedenis van de virtuele machines mee in overweging genomen.<br/><br/>- Als u 'als on-premises' hebt gebruikt, is de groottebepaling gebaseerd op de grootte van de on-premises VM's, en wordt geen rekening gehouden met verbruiksgegevens.
**Voorgesteld hulpprogramma** | Omdat de agents op de VM's worden uitgevoerd, wordt in Azure Migrate gekeken naar de processen die op de machine worden uitgevoerd en wordt bepaald of de machine een databasemachine is.
**VM-informatie** | Het rapport bevat instellingen voor de on-premises virtuele machine, waaronder informatie over het besturingssysteem, het opstarttype, schijven en opslag.


#### <a name="review-monthly-cost-estimates"></a>Schatting van maandelijkse kosten controleren

Deze weergave toont de totale compute- en opslagkosten om de virtuele machines in Azure uit te voeren, evenals de details per VM.

![Gereedheid evalueren](./media/contoso-migration-assessment/azure-costs.png)

- Kostenschattingen worden berekend aan de hand van de aanbevelingen voor de grootte voor een machine.
- Geschatte maandelijkse kosten voor computing en opslag worden samengevoegd voor alle virtuele machines in de groep.


## <a name="clean-up-after-assessment"></a>Opschonen na de evaluatie

- Nadat de evaluatie is voltooid, wordt het apparaat van de Azure-migratie voor toekomstige evaluaties bewaard, Contoso.
- Ze uitschakelen de VMware virtuele machine. Ze u deze opnieuw gestart wanneer dit beleid extra virtuele machines beoordeelt.
- Ze houden de Contoso-Migration-project in Azure.  Er wordt momenteel geïmplementeerd in de resourcegroep ContosoFailoverRG, in de regio VS-Oost Azure.
-  De collector-VM heeft een licentie van de evaluatieperiode van 180 dagen. Als deze limiet is verlopen, moet ze downloaden en de collector opnieuw instellen.


## <a name="conclusion"></a>Conclusie

In dit scenario beoordeeld Contoso de SmartHotel app-database met behulp van het hulpprogramma DMA en de on-premises machines met behulp van de service Azure Migrate. Ze klikt u vervolgens de beoordelingen gecontroleerd om ervoor te zorgen dat on-premises resources gereed zijn voor migratie naar Azure.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in deze reeks naamconflicten Contoso de SmartHotel-app in Azure met een lift-and-shift-migratie. Contoso migreert het front-end WEBVM voor de app met behulp van Azure Site Recovery en de app-database naar een Azure SQL Managed Instance, met behulp van de Database Migration Service. [Aan de slag](contoso-migration-rehost-vm-sql-managed-instance.md) met deze implementatie.
