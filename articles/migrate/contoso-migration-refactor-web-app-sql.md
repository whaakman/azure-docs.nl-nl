---
title: Een Contoso-app herstructureren door te migreren naar Azure Web App en Azure SQL Database | Microsoft Docs
description: Informatie over hoe Contoso een on-premises app naamconflicten door te migreren naar een Azure-Web-App en Azure SQL Server-database.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: eb046ceebf9c6233fa5178d18603add2f0c26a29
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608014"
---
# <a name="contoso-migration-refactor-an-on-premises-app-to-an-azure-web-app-and-azure-sql-database"></a>Migratie van Contoso: Een on-premises-app met een Azure-Web-App en Azure SQL-database herstructureren

In dit artikel ziet u hoe Contoso promotiebewerkingen hun SmartHotel360-app in Azure. Zij migreren de app front-end virtuele machine naar een Azure-Web-App en de app-database naar een Azure SQL-database.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso de on-premises resources naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Aanvullende artikelen gaan we toevoegen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
[Artikel 3: On-premises resources evalueren](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op Azure VM's en een beheerd exemplaar van SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de app SmartHotel uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso de SmartHotel-app met behulp van Site Recovery alleen VM's migreren. | Beschikbaar
[Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[7-artikel: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[8-artikel: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
9-artikel: Een app herstructureren in Azure Web App en Azure SQL Database | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | In dit artikel
[Artikel 10: Een Linux-app naar Azure WebApps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[11-artikel: Herstructureren van TFS op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert naar Azure DevOps-Services in Azure. | Beschikbaar
[12-artikel: Een app op Azure-containers en Azure SQL Database opnieuw modelleren](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[13-artikel: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar
[14-artikel: Schalen van een migratie naar Azure](contoso-migration-scale.md) | Na het proberen van migratie combinaties, bereidt Contoso worden uitgebreid naar een volledige migratie naar Azure. | Beschikbaar

In dit artikel worden de twee lagen Windows gemigreerd door Contoso. NET SmartHotel360-app die worden uitgevoerd op virtuele VMware-machines naar Azure. Als u wilt deze app wilt gebruiken, wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-team leiderschap heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen stroomlijnen voor ontwikkelaars en gebruikers.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**:  Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie.  Het kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: Wanneer het bedrijf is groeit, moet: Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde opgeven.
- **Kosten**: Contoso wil licentiekosten minimaliseren.

## <a name="migration-goals"></a>Migratiedoelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen zijn gebruikt voor het bepalen van de beste migratiemethode.

**Vereisten** | **Details**
--- | --- 
**App** | De app in Azure blijft als kritiek omdat het is vandaag.<br/><br/> Deze moet dezelfde prestatiemogelijkheden hebben als op dit moment in VMWare.<br/><br/> Het team wil niet investeren in de app. Voorlopig wordt beheerders gewoon de app veilig naar de cloud verplaatsen.<br/><br/> Het team wilt stoppen met het ondersteunen van Windows Server 2008 R2, waarop de app op dit moment wordt uitgevoerd.<br/><br/> Het team wil ook afstappen van SQL Server 2008 R2 naar een moderne PaaS-Database-platform, de noodzaak voor het beheer te minimaliseren.<br/><br/> Contoso wilt gebruikmaken van de investeringen in SQL Server-licenties en Software Assurance waar mogelijk.<br/><br/> Contoso wil verder beperken van de één storingspunt is op de weblaag.
**Beperkingen** | De app bestaat uit een ASP.NET-app en een WCF-service die wordt uitgevoerd op dezelfde VM. Ze willen dit verdeeld over twee web-apps met behulp van Azure App Service. 
**Azure** | Contoso wil overstappen van de app naar Azure, maar niet wilt uitvoeren op virtuele machines. Contoso wil gebruikmaken van Azure PaaS-services voor zowel de web- en gegevenslagen. 
**DevOps** | Contoso wil verplaatsen naar een model voor DevOps, met behulp van Azure DevOps voor hun builds en pipelines vrijgeven.

## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeert het migratieproces, met inbegrip van de Azure-services die worden gebruikt voor de migratie.

### <a name="current-app"></a>Huidige app

- De on-premises SmartHotel360 app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.


### <a name="proposed-solution"></a>Voorgestelde oplossing

- Voor de databaselaag van de app, Contoso in vergelijking met Azure SQL Database met behulp van SQL Server [in dit artikel](https://docs.microsoft.com/azure/sql-database/sql-database-features). Contoso besloten om te gaan met Azure SQL Database voor een aantal oorzaken hebben:
    - Azure SQL Database is een beheerde relationele database-service. Dit biedt voorspelbare prestaties op meerdere serviceniveaus, met praktisch zonder beheer. Dynamische schaalbaarheid geen uitvaltijd, ingebouwde intelligente optimalisatie, en wereldwijde schaalbaarheid en beschikbaarheid van de volgende voordelen.
    - Contoso kan gebruikmaken van de lightweight Data Migration Assistant (DMA) om te beoordelen en de on-premises database migreren naar Azure SQL.
    - Met Software Assurance, kan Contoso uitwisselen van bestaande licenties voor gereduceerde tarieven voor een SQL-Database met behulp van Azure Hybrid Benefit voor SQL Server. Zodoende kan tot 30% te besparen.
    - SQL Database biedt een aantal beveiligingsfuncties, met inbegrip van altijd versleuteld, dynamische gegevensmaskering en beveiliging/bedreigingsdetectie van beveiliging op rijniveau.
- Contoso heeft voor de laag van de web-app besloten om het gebruik van Azure App Service. Met deze PaaS-service kunnen die de app met een paar configuratiewijzigingen implementeren. Contoso wordt Visual Studio gebruiken om de wijziging te maken en implementeren van twee web-apps. Één voor de website, en één voor de WCF-service.
- Contoso heeft om te voldoen aan vereisten voor een DevOps-pijplijn, geselecteerd voor het gebruik van de Azure DevOps voor Source Code Management (SCM) met Git-opslagplaatsen. Geautomatiseerde builds en release wordt gebruikt om de code te bouwen en deze implementeren in de Azure-Web-Apps.
  
### <a name="solution-review"></a>Beoordelingen van oplossing
Contoso evalueert het voorgestelde ontwerp door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Professionals** | De code van de app SmartHotel360 hoeft niet te worden gewijzigd voor migratie naar Azure.<br/><br/> Contoso kan gebruikmaken van hun investering in Software Assurance met behulp van Azure Hybrid Benefit voor SQL Server en Windows Server.<br/><br/> Na de migratie moet Windows Server 2008 R2 wordt niet ondersteund. [Meer informatie](https://support.microsoft.com/lifecycle).<br/><br/> Contoso kunt de weblaag van de app configureren met meerdere instanties, zodat deze niet langer een single point of failure.<br/><br/> De database wordt niet langer afhankelijk van de ouderdom van SQL Server 2008 R2.<br/><br/> SQL Database biedt ondersteuning voor de technische vereisten. Contoso de on-premises-database met behulp van de Database Migration Assistant beoordeeld en dat deze compatibel is gevonden.<br/><br/> SQL-Database heeft ingebouwde fouttolerantie die Contoso niet nodig hebt om in te stellen. Dit zorgt ervoor dat de gegevenslaag niet langer een single point of failover is.
**Nadelen** | Azure App Services ondersteunt alleen één appimplementatie voor elke Web-App. Dit betekent dat twee Web-Apps ingericht (één voor de website) en één voor de WCF-service worden moet.<br/><br/> Als Contoso maakt gebruik van de Data Migration Assistant in plaats van de Data Migration Service voor het migreren van de database, het geen de infrastructuur die gereed zijn voor het migreren van databases op schaal. Contoso moet maken van een andere regio om te controleren of failover als de primaire regio niet beschikbaar is.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

![Scenario-architectuur](media/contoso-migration-refactor-web-app-sql/architecture.png) 


### <a name="migration-process"></a>Migratieproces

1. Contoso-bepalingen van een Azure SQL-exemplaar en de SmartHotel360-database migreert naar deze.
2. Contoso inricht en configureert u de Web-Apps en de SmartHotel360-app wordt geïmplementeerd op deze.

    ![Migratieproces](media/contoso-migration-refactor-web-app-sql/migration-process.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA Contoso gebruikt om te beoordelen en detecteert compatibiliteitsproblemen die mogelijk van invloed zijn op de databasefunctionaliteit in Azure. DMA de functiepariteit tussen SQL-bronnen en doelen beoordeelt en wordt aanbevolen verbeteringen in de prestaties en betrouwbaarheid. | Het is een gratis hulpprogramma dat kan worden gedownload.
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) | Een intelligente, beheerde volledig relationele clouddatabaseservice. | De kosten op basis van functies, doorvoer en grootte. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure App Services - Web-Apps](https://docs.microsoft.com/azure/app-service/overview) | Maken van krachtige cloud-apps met behulp van een volledig beheerd platform | De kosten op basis van de duur van de grootte, locatie en het gebruik. [Meer informatie](https://azure.microsoft.com/pricing/details/app-service/windows/).
[Azure DevOps](https://docs.microsoft.com/azure/azure-portal/tutorial-azureportal-devops) | Biedt een continue integratie en een pijplijn voor continue implementatie (CI/CD) voor app-ontwikkeling. De pijplijn begint met een Git-opslagplaats voor het beheren van app-code, een build-systeem voor het produceren van pakketten en andere build-artefacten en een versiebeheersysteem wijzigingen in het ontwikkelen, testen en productie-omgevingen implementeren. 

## <a name="prerequisites"></a>Vereisten

Hier volgt een Contoso-behoeften om uit te voeren in dit scenario:

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | Contoso abonnementen tijdens een eerdere artikel gemaakt. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Een exemplaar van SQL Database in Azure inrichten**: Contoso richt een SQL-exemplaar in Azure. Nadat de app-website is gemigreerd naar Azure, wordt de web-app van de WCF-service verwijzen naar dit exemplaar.
> * **Stap 2: Migreren van de database met DMA**: De app-database met de Database Migration Assistant, Contoso worden gemigreerd.
> * **Stap 3: Web-Apps inrichten**: Contoso richt de twee web-apps.
> * **Stap 4: Instellen van Azure DevOps**: Contoso maakt een nieuwe Azure DevOps-project en de invoer van de Git-opslagplaats.
> * **Stap 5: Configureren van verbindingsreeksen**: Contoso verbindingsreeksen zo geconfigureerd dat de web-laag web-app, de web-app van de WCF-service en het SQL-exemplaar kunnen communiceren.
> * **Stap 6: Build instellen en pipelines vrijgeven**: Als laatste stap, Contoso stelt u build en versie pijplijnen voor het maken van de app en ze in twee afzonderlijke Azure Web Apps implementeert.


## <a name="step-1-provision-an-azure-sql-database"></a>Stap 1: Een Azure SQL-Database inrichten

1. Beheerders van Contoso selecteren om te maken van een SQL-Database in Azure. 

    ![Inrichten SQL](media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

2. Ze geven de naam van een database zodat deze overeenkomen met de database die wordt uitgevoerd op de on-premises VM (**SmartHotel.Registration**). Ze plaatsen de database in de resourcegroep ContosoRG. Dit is de resourcegroep die ze voor productieresources in Azure gebruiken.

    ![Inrichten SQL](media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

3. Ze instellen van een nieuwe SQL Server-exemplaar (**sql-smarthotel-eus2**) in de primaire regio.

    ![Inrichten SQL](media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

4. Ze stelt de prijscategorie zodat deze overeenkomen met de server en databasebehoeften. En ze selecteren om geld te besparen met Azure Hybrid Benefit omdat ze al een SQL Server-licentie hebben.
5. De grootte instelt ze gebruiken op basis van v-Core aanschaffen en stel de limieten voor de verwachte behoeften.

    ![Inrichten SQL](media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

6. Ze maken vervolgens de database-instantie.

    ![Inrichten SQL](media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

7. Nadat het exemplaar is gemaakt, ze openen van de database en noteer de details die ze nodig hebben wanneer ze de Database Migration Assistance voor migratie gebruiken.

    ![Inrichten SQL](media/contoso-migration-refactor-web-app-sql/provision-sql6.png)


**Meer hulp nodig?**

- [Hulp bij het](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) inrichten van een SQL-Database.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) resourcelimieten v-Core.


## <a name="step-2-migrate-the-database-with-dma"></a>Stap 2: Migreren van de database met DMA

Beheerders van Contoso, worden de SmartHotel360-database met DMA gemigreerd.

### <a name="install-dma"></a>DMA installeren

1. Ze het hulpprogramma downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) naar de on-premises SQL Server-VM (**SQLVM**).
2. Ze uitvoeren setup (DownloadMigrationAssistant.msi) op de virtuele machine.
3. Op de **voltooien** pagina, selecteren ze **Start Microsoft Data Migration Assistant** voordat de wizard is voltooid.

### <a name="migrate-the-database-with-dma"></a>Migreren van de database met DMA

1. In de DMA ze een nieuw project maken (**SmartHotelDB**) en selecteer **migratie** 
2. Ze selecteert u het brontype van de server als **SQL Server**, en het doel als **Azure SQL Database**. 

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-1.png)

3. In de migratie, die ze toevoegen **SQLVM** als de bronserver en de **SmartHotel.Registration** database. 

     ![DMA](media/contoso-migration-refactor-web-app-sql/dma-2.png)

4. Ze foutbericht een weergegeven dat lijkt te zijn gekoppeld aan de verificatie. Nadat u wilt onderzoeken, is het probleem echter de punt (.) in de naam van de database. Als tijdelijke oplossing, die ze hebben besloten voor het inrichten van een nieuwe SQL-database met behulp van de naam van de **SmartHotel registratie**, om het probleem te verhelpen. Wanneer ze DMA opnieuw worden uitgevoerd, ze kunnen selecteren **SmartHotel registratie**, en ga verder met de wizard.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-3.png)

5. In **objecten selecteren**, ze de databasetabellen selecteren en een SQL-script genereren.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-4.png)

6. Nadat het script van DMS wordt gemaakt, klikt de gebruiker op **Deploy schema**.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-5.png)

7. DMA bevestigt dat de implementatie is voltooid.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-6.png)

8. Ze nu beginnen met de migratie.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-7.png)

9. Nadat de migratie is voltooid, kunnen beheerders van Contoso controleren of de database wordt uitgevoerd op de Azure SQL-exemplaar.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-8.png)

10. De extra SQL-database worden verwijderd **SmartHotel.Registration** in Azure portal.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-9.png)


## <a name="step-3-provision-web-apps"></a>Stap 3: Web-Apps inrichten

Met de database hebt gemigreerd, Contoso-beheerders kunnen nu inrichten de twee web-apps.

1. Ze selecteren **Web-App** in de portal.

    ![Web-app](media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. Ze bieden een app-naam (**SHWEB EUS2**), uitvoeren op Windows en plaats het ongedaan maken van de groep van de resources productie **ContosoRG**. Ze maken een nieuwe appservice en een plan.

    ![Web-app](media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. Nadat de web-app is ingericht, worden ze Herhaal het proces voor het maken van een web-app voor de WCF-service (**SHWCF EUS2**)

    ![Web-app](media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. Als ze klaar bent, worden ze, blader naar het adres van de apps om te controleren of dat ze hebt gemaakt.


## <a name="step-4-set-up-azure-devops"></a>Stap 4: Azure DevOps instellen


Contoso heeft nodig om de infrastructuur voor DevOps en pijplijnen voor de toepassing te bouwen.  U doet dit door beheerders van Contoso een nieuw DevOps-project maken, importeren van de code, en vervolgens ingesteld build en pipelines vrijgeven.

1.   In de Contoso Azure DevOps-account, ze een nieuw project maken (**ContosoSmartHotelRefactor**), en selecteer **Git** voor versiebeheer.

    ![Nieuw project](./media/contoso-migration-refactor-web-app-sql/vsts1.png)
2. Ze importeren de Git-opslagplaats die op dit moment de app-code bevat. Deel uitmaakt van een [openbare opslagplaats](https://github.com/Microsoft/SmartHotel360-internal-booking-apps) en kunt u deze downloaden.

    ![App-code downloaden](./media/contoso-migration-refactor-web-app-sql/vsts2.png)
    
3. Nadat de code is geïmporteerd, ze verbinding maken met Visual Studio naar de opslagplaats en kloon de code met behulp van Team Explorer.

    ![Verbinding maken met project](./media/contoso-migration-refactor-web-app-sql/devops1.png)

4. Nadat de opslagplaats hebt gekloond naar de machine voor ontwikkelaars, opent u het oplossingsbestand voor de app. De web-app en wcf-service hebben afzonderlijke-project binnen het bestand.

    ![Oplossingsbestand](./media/contoso-migration-refactor-web-app-sql/vsts4.png)
    

## <a name="step-5-configure-connection-strings"></a>Stap 5: Verbindingsreeksen configureren

Beheerders van Contoso moet ervoor zorgen dat de web-apps en de database kan alle communiceren. U doet dit door configureren ze-verbindingsreeksen in de code en in de web-apps.

1. In de web-app voor de WCF-service (**SHWCF EUS2**) > **instellingen** > **toepassingsinstellingen**, ze een nieuwe verbindingsreeks met de naam toevoegen  **DefaultConnection**.
2. De verbindingsreeks wordt opgehaald uit de **SmartHotel registratie** database en moet worden bijgewerkt met de juiste referenties.

    ![Verbindingsreeks](media/contoso-migration-refactor-web-app-sql/string1.png)

3. Open met behulp van Visual Studio, ze de **SmartHotel.Registration.wcf** -project van het oplossingsbestand. De **connectionStrings** sectie van het bestand web.config voor de WCF-service SmartHotel.Registration.Wcf moet worden bijgewerkt met de verbindingsreeks.

     ![Verbindingsreeks](media/contoso-migration-refactor-web-app-sql/string2.png)

4. De **client** gedeelte van het bestand web.config voor de SmartHotel.Registration.Web moet worden gewijzigd om te verwijzen naar de nieuwe locatie van de WCF-service. Dit is de URL van de WCF-web-app die als host fungeert de service-eindpunt.

    ![Verbindingsreeks](media/contoso-migration-refactor-web-app-sql/strings3.png)

5. Nadat de wijzigingen in de code zijn, moeten beheerders de wijzigingen wilt doorvoeren. Team Explorer in Visual Studio, gebruiken ze commmit en synchroniseren.


## <a name="step-6-set-up-build-and-release-pipelines-in-azure-devops"></a>Stap 6: Build instellen en release-pijplijnen in Azure DevOps

Beheerders van Contoso is nu configureren voor Azure DevOps voor het uitvoeren van build en release proces.

1. Klik in Azure DevOps, ze op **bouwen en uitbrengen** > **nieuwe pijplijn**.

    ![Nieuwe pijplijn](./media/contoso-migration-refactor-web-app-sql/pipeline1.png)

2. Ze selecteren **Azure opslagplaatsen Git** en de relevante opslagplaats.

    ![De opslagplaats en GIT](./media/contoso-migration-refactor-web-app-sql/pipeline2.png)

3. In **selecteert u een sjabloon**, ze de ASP.NET-sjabloon voor hun build selecteren.

     ![ASP.NET-sjabloon](./media/contoso-migration-refactor-web-app-sql/pipeline3.png)
    
4. De naam van de **ContosoSmartHotelRefactor-ASP.NET-CI** wordt gebruikt voor de build. Ze klikt u op **opslaan en in de wachtrij**.

     ![Opslaan en wachtrij](./media/contoso-migration-refactor-web-app-sql/pipeline4.png)

5. Dit begint de eerste build. Ze klikt u op het build-nummer om te bekijken van het proces. Nadat deze voltooid kunnen ze weergeven van de feedback verwerken en op **artefacten** om de resultaten van de build te controleren.

    ![Beoordelen](./media/contoso-migration-refactor-web-app-sql/pipeline5.png)

6. De map **neerzetten** de resultaten van de build bevat.

    - De twee zip-bestanden worden de pakketten die de apps bevatten.
    - Deze bestanden worden gebruikt in de release-pijplijn voor implementatie naar Azure Web Apps

     ![Artefact](./media/contoso-migration-refactor-web-app-sql/pipeline6.png)

7. Ze klikt u op **Releases** > **+ nieuwe pijplijn**.

    ![Nieuwe pijplijn](./media/contoso-migration-refactor-web-app-sql/pipeline7.png)

8. Ze selecteert u de sjabloon van Azure App Service-implementatie.

    ![Azure App Service-sjabloon](./media/contoso-migration-refactor-web-app-sql/pipeline8.png)

9. Naam van de release-pijplijn **ContosoSmartHotel360Refactor**, en geef de naam van de WCF-web-app (SHWCF-EUS2) voor de **fase** naam.

    ![Omgeving](./media/contoso-migration-refactor-web-app-sql/pipeline9.png)

10. Klik onder de fasen die ze op **1 taak, 1 taak** om de implementatie van de WCF-service te configureren.

    ![Implementeren van WCF](./media/contoso-migration-refactor-web-app-sql/pipeline10.png)

11. Ze controleren of het abonnement is geselecteerd en geautoriseerde en selecteer de **App service-naam**.

     ![Selecteer appservice](./media/contoso-migration-refactor-web-app-sql/pipeline11.png)

12. Op de pijplijn > **artefacten**, selecteren ze **+ toevoegen van een artefact**, en selecteer om te bouwen met de **ContosoSmarthotel360Refactor** pijplijn.

     ![Ontwikkelen](./media/contoso-migration-refactor-web-app-sql/pipeline12.png)

15. Ze klikt u op de korte-bolt in het artefact is ingeschakeld. om de trigger voor continue implementatie inschakelen.

     ![Bliksemsnelle bolt](./media/contoso-migration-refactor-web-app-sql/pipeline13.png)

16. De trigger voor continue implementatie moet worden ingesteld op **ingeschakeld**.

   ![Continue implementatie ingeschakeld](./media/contoso-migration-refactor-web-app-sql/pipeline14.png) 

17. Nu, ze terug verplaatsen naar een taak met de fase 1, ik taken, en klik op **Azure App Service implementeren**.

    ![Appservice implementeren](./media/contoso-migration-refactor-web-app-sql/pipeline15.png)

18. In **selecteert u een bestand of map**, ze Zoek de **SmartHotel.Registration.Wcf.zip** -bestand dat is gemaakt tijdens de build- en clilck **opslaan**.-sql

    ![Opslaan van WCF](./media/contoso-migration-refactor-web-app-sql/pipeline16.png)

19. Ze klikt u op **pijplijn** > **fasen** **+ toevoegen**om toe te voegen in een omgeving **SHWEB EUS2**. Ze selecteert u een andere Azure App Service-implementatie.

    ![Omgeving toevoegen](./media/contoso-migration-refactor-web-app-sql/pipeline17.png)

20. Ze Herhaal het proces voor het publiceren van de web-app (**SmartHotel.Registration.Web.zip**)-bestand naar de juiste web-app.

    ![Web-app publiceren](./media/contoso-migration-refactor-web-app-sql/pipeline18.png)

21. Nadat deze opgeslagen, wordt de release-pijplijn wordt als volgt weergegeven.

     ![Overzicht van de release-pijplijn](./media/contoso-migration-refactor-web-app-sql/pipeline19.png)

22. Ze terug verplaatsen naar **bouwen**, en klikt u op **Triggers** > **inschakelen van continue integratie**. Hierdoor wordt de pijplijn zodat wanneer wijzigingen doorgevoerd in de code zijn en volledige build- en publicatie plaatsvindt.

    ![Inschakelen van continue integratie](./media/contoso-migration-refactor-web-app-sql/pipeline20.png)

23. Ze klikt u op **opslaan en in de wachtrij** om uit te voeren van de volledige pijplijn. Een nieuwe build is geactiveerd die op zijn beurt de eerste release van de app in de Azure App Service worden gemaakt.

    ![Pijplijn opslaan](./media/contoso-migration-refactor-web-app-sql/pipeline21.png)

24. Beheerders van Contoso kunt volgt u de build en release pijplijnproces van Azure DevOps. Nadat de build is voltooid, wordt de versie wordt gestart.

    ![Bouwen en uitbrengen van app](./media/contoso-migration-refactor-web-app-sql/pipeline22.png)

25. Nadat de pijplijn is voltooid, worden beide sites zijn geïmplementeerd en wordt de app is actief en werkend online.

    ![Release voltooien](./media/contoso-migration-refactor-web-app-sql/pipeline23.png)

De app wordt op dit moment is gemigreerd naar Azure.



## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Na de migratie moet Contoso deze opschonen stappen uitvoeren:  

- De on-premises VM's van de vCenter-voorraad verwijderen.
- Verwijder de virtuele machines uit lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locaties voor de SmartHotel360-app bijwerken. De database die wordt uitgevoerd in Azure SQL-database en de front-end dat deze wordt uitgevoerd in twee web-apps weergegeven.
- Alle resources die interactie met de uit bedrijf genomen VM's hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.


## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

- Contoso nodig om ervoor te zorgen dat hun nieuwe **SmartHotel registratie** -database is beveiligd. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Contoso moet in het bijzonder de web-apps voor het gebruik van SSL met certificaten bijwerken.

### <a name="backups"></a>Back-ups

- Contoso moet voor een overzicht back-vereisten voor de Azure SQL-Database. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Contoso is ook nodig voor meer informatie over het beheren van back-ups van SQL-Database en wordt hersteld. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups) over automatische back-ups.
- Contoso moet rekening houden met failovergroepen om regionale failover voor de database te implementeren. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Contoso moet rekening houden met het implementeren van de Web-App in de belangrijkste VS-Oost 2 en de regio VS-midden voor flexibiliteit. Contoso kan Traffic Manager om te controleren of failover in het geval van regionale storingen te configureren.

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Nadat alle resources worden geïmplementeerd, Contoso Azure tags op basis van moet toewijzen hun [infrastructuur plannen](contoso-migration-infrastructure.md#set-up-tagging).
- Alle licenties zijn ingebouwd in de kosten van de PaaS-services die van Contoso gebruikmaakt al. Dit wordt afgetrokken van de EA.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.

## <a name="conclusion"></a>Conclusie

In dit artikel is geherstructureerd Contoso de SmartHotel360-app in Azure door het app-front-end virtuele machine migreren naar twee Azure-Web-Apps. De app-database is gemigreerd naar een Azure SQL database.






