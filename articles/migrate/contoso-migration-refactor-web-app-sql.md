---
title: Een Contoso-app herstructureren door te migreren naar Azure-VM's en SQL Server AlwaysOn-beschikbaarheidsgroep | Microsoft Docs
description: Meer informatie over hoe Contoso een on-premises app rehost door te migreren naar een Azure-Web-App-Container en een Azure SQL Server-database.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: f4a348815ef058cb795ed12e8f118b494650a555
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005187"
---
# <a name="contoso-migration-refactor-an-on-premises-app-to-an-azure-web-app-and-azure-sql-database"></a>Migratie van Contoso: herstructureren van een on-premises-app met een Azure-Web-App en Azure SQL-database

In dit artikel ziet u hoe Contoso promotiebewerkingen hun SmartHotel-app in Azure. Zij migreren de app front-end virtuele machine naar een Azure-Web-App en de app-database naar een Azure SQL-database.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso de on-premises resources naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en aanvullende artikelen na verloop van tijd toegevoegd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
[Artikel 3: On-premises resources evalueren](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Een app op Azure VM's en een beheerde SQL-exemplaar opnieuw hosten](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de app SmartHotel uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso de SmartHotel-app met behulp van Site Recovery alleen VM's migreren. | Beschikbaar
[Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
Artikel 9: Een app met een Azure-Web-App en Azure SQL-database herstructureren | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | In dit artikel
[Artikel 10: Een Linux-app naar Azure WebApps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar


In dit artikel worden de twee lagen Windows gemigreerd door Contoso. SmartHotel NET-app die worden uitgevoerd op virtuele VMware-machines naar Azure. Als u wilt deze app wilt gebruiken, wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-team leiderschap heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen stroomlijnen voor ontwikkelaars en gebruikers.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**: Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie.  Het kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: wanneer het bedrijf met succes groeit, Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde moet opgeven.
- **Kosten**: Contoso wil licentiekosten minimaliseren.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen zijn gebruikt voor het bepalen van de beste migratiemethode.

**Vereisten** | **Details**
--- | --- 
**App** | De app in Azure blijft als kritiek omdat het is vandaag.<br/><br/> Deze moet dezelfde prestatiemogelijkheden hebben als op dit moment in VMWare.<br/><br/> Ze wil niet investeren in de app. Voorlopig wilt ze gewoon veilig naar de cloud verplaatsen.<br/><br/> Ze willen stoppen met het ondersteunen van Windows Server 2008 R2, waarop de app op dit moment wordt uitgevoerd.<br/><br/> Ze willen afstappen van SQL Server 2008 R2 naar een moderne PaaS-Database-platform, de noodzaak voor het beheer te minimaliseren.<br/><br/> Contoso wilt gebruikmaken van hun investering in SQL Server-licenties en Software Assurance waar mogelijk.<br/><br/> Bovendien wilt Contoso beperken de één storingspunt is op de weblaag.
**Beperkingen** | De app bestaat uit een ASP.NET-app en een WCF-service die wordt uitgevoerd op dezelfde VM. Ze willen dit verdeeld over twee web-apps met behulp van Azure App Service. 
**Azure** | Ze willen verplaatsen van de app naar Azure, maar ze niet wilt uitvoeren op virtuele machines. Ze willen gebruikmaken van Azure PaaS-services voor zowel de web- en gegevenslagen. 

## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag hun doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeert het migratieproces, met inbegrip van de Azure-services die ze voor de migratie gaat gebruiken.

### <a name="current-app"></a>Huidige app

- De on-premises SmartHotel app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.


### <a name="proposed-solution"></a>Voorgestelde oplossing

- Voor de databaselaag van de app, Contoso in vergelijking met Azure SQL Database met behulp van SQL Server [in dit artikel](https://docs.microsoft.com/azure/sql-database/sql-database-features). Ze hebben besloten om te gaan met Azure SQL Database voor een aantal oorzaken hebben:
    - Azure SQL Database is een beheerde relationele database-service. Dit biedt voorspelbare prestaties op meerdere serviceniveaus, met praktisch zonder beheer. Dynamische schaalbaarheid geen uitvaltijd, ingebouwde intelligente optimalisatie, en wereldwijde schaalbaarheid en beschikbaarheid van de volgende voordelen.
    - Ze kunnen gebruikmaken van de lightweight Data Migration Assistant (DMA) om te beoordelen en de on-premises database migreren naar Azure SQL.
    - Met Software Assurance, kunnen ze hun bestaande licenties voor gereduceerde tarieven voor een SQL-Database met behulp van Azure Hybrid Benefit voor SQL Server uitwisselen. Zodoende kan tot 30% te besparen.
    - SQL Database biedt een aantal beveiligingsfuncties, met inbegrip van altijd versleuteld, dynamische gegevensmaskering en beveiliging/bedreigingsdetectie van beveiliging op rijniveau.
- Voor de laag van de web-app hebt ze besloten Azure App Service gebruikt. Met deze PaaS-service kunnen die de app met een paar configuratiewijzigingen implementeren. Ze Visual Studio gebruiken om de wijziging te maken en implementeren van twee web-apps. Één voor de website, en één voor de WCF-service.
  
### <a name="solution-review"></a>Beoordelingen van oplossing
Contoso evalueert het voorgestelde ontwerp door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Professionals** | De code van de app SmartHotel hoeft niet te worden gewijzigd voor migratie naar Azure.<br/><br/> Ze kunnen gebruikmaken van hun investering in Software Assurance met behulp van Azure Hybrid Benefit voor SQL Server en Windows Server.<br/><br/> Na de migratie moet ze niet langer ondersteuning voor Windows Server 2008 R2. [Meer informatie](https://support.microsoft.com/lifecycle).<br/><br/> Ze kunnen de weblaag van de app configureren met meerdere instanties, zodat deze niet langer een single point of failure.<br/><br/> Deze wordt niet langer afhankelijk van de ouderdom van SQL Server 2008 R2.<br/><br/> SQL Database biedt ondersteuning voor de technische vereisten van Contoso. Ze de on-premises-database met behulp van de Database Migration Assistant bekeken en wordt gevonden of deze compatibel is.<br/><br/> SQL-Database heeft ingebouwde fouttolerantie die Contoso niet nodig hebt om in te stellen. Dit zorgt ervoor dat de gegevenslaag niet langer een single point of failover is.
**Nadelen** | Azure App Services ondersteunt alleen één appimplementatie voor elke Web-App. Dit betekent dat twee Web-Apps ingericht (één voor de website) en één voor de WCF-service worden moet.<br/><br/> Als ze de Data Migration Assistant in plaats van de Data Migration Service gebruiken voor het migreren van de database, Contoso geen de infrastructuur die gereed zijn voor het migreren van databases op schaal. Ze moet bouwen van een andere regio om te controleren of failover als de primaire regio niet beschikbaar is.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

![Scenario-architectuur](media/contoso-migration-refactor-web-app-sql/architecture.png) 


### <a name="migration-process"></a>Migratieproces

1. Contoso inrichten van een Azure SQL-exemplaar en migreren van de database SmartHotel toe.
2. Ze inrichten en Web-Apps configureren en de SmartHotel-app om ze te implementeren.

    ![Migratieproces](media/contoso-migration-refactor-web-app-sql/migration-process.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Ze DMA gebruikt om te beoordelen en detecteert compatibiliteitsproblemen die mogelijk van invloed zijn op de databasefunctionaliteit in Azure. DMA de functiepariteit tussen SQL-bronnen en doelen beoordeelt en wordt aanbevolen verbeteringen in de prestaties en betrouwbaarheid. | Het is een gratis hulpprogramma dat kan worden gedownload.
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) | Een intelligente, beheerde volledig relationele clouddatabaseservice. | De kosten op basis van functies, doorvoer en grootte. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure App Services - Web-Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) | Maken van krachtige cloud-apps met behulp van een volledig beheerd platform | De kosten op basis van de duur van de grootte, locatie en het gebruik. [Meer informatie](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) nodig om uit te voeren in dit scenario:

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al een abonnement hebt gemaakt wanneer u de evaluatie uitgevoerd in de eerste artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Een exemplaar van SQL Database in Azure inrichten**: Contoso richt een SQL-exemplaar in Azure. Nadat de app-website is gemigreerd naar Azure, wordt de web-app van de WCF-service verwijzen naar dit exemplaar.
> * **Stap 2: Migreren van de database met DMA**: zij de app-database met de Database Migration Assistant migreren.
> * **Stap 3: Inrichten Web-Apps**: het inrichten van de twee web-apps.
> * **Stap 4: Configureren van verbindingsreeksen**: ze verbindingsreeksen configureren zodat de web-laag web-app, de web-app van de WCF-service en het SQL-exemplaar kunnen communiceren.
> * **Stap 5: Web-apps publiceren**: als laatste stap, Contoso de apps naar Azure gepubliceerd.


## <a name="step-1-provision-an-azure-sql-database"></a>Stap 1: Een Azure SQL-Database inrichten

1. Ze selecteert u een SQL-Database maken in Azure. 

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

Contoso kan de SmartHotel-database met DMA worden gemigreerd.

### <a name="install-dma"></a>DMA installeren

1. Ze het hulpprogramma downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) naar de on-premises SQL Server-VM (**SQLVM**).
2. Ze uitvoeren setup (DownloadMigrationAssistant.msi) op de virtuele machine.
3. Op de **voltooien** pagina, selecteren ze **Start Microsoft Data Migration Assistant** voordat de wizard is voltooid.

### <a name="migrate-the-database-with-dma"></a>Migreren van de database met DMA

1. Maak een nieuw project in de DMA (**SmartHotelDB**) en selecteer **migratie** 
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

9. Nadat de migratie is voltooid, kan Contoso kunt controleren of de database wordt uitgevoerd op de Azure SQL-exemplaar.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-8.png)

10. De extra SQL-database worden verwijderd **SmartHotel.Registration** in Azure portal.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-9.png)


## <a name="step-3-provision-web-apps"></a>Stap 3: Inrichten Web-Apps

Met de database hebt gemigreerd, Contoso kunnen nu richten de twee web-apps.

1. Ze selecteren **Web-App** in de portal.

    ![Web-app](media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. Ze bieden een app-naam (**SHWEB EUS2**), uitvoeren op Windows en plaats het ongedaan maken van de groep van de resources productie **ContosoRG**. Ze maken een nieuwe appservice en een plan.

    ![Web-app](media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. Nadat de web-app is ingericht, worden ze Herhaal het proces voor het maken van een web-app voor de WCF-service (**SHWCF EUS2**)

    ![Web-app](media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. Als ze klaar bent, worden ze, blader naar het adres van de apps om te controleren of dat ze hebt gemaakt.

## <a name="step-4-configure-connection-strings"></a>Stap 4:-Verbindingsreeksen configureren

Contoso nodig heeft om te controleren of de web-apps en de database kan alle communiceren. U doet dit door configureren ze-verbindingsreeksen in de code en in de web-apps.

1. In de web-app voor de WCF-service (**SHWCF EUS2**) > **instellingen** > **toepassingsinstellingen**, ze een nieuwe verbindingsreeks met de naam toevoegen  **DefaultConnection**.
2. De verbindingsreeks wordt opgehaald uit de **SmartHotel registratie** database en moet worden bijgewerkt met de juiste referenties.

    ![Verbindingsreeks](media/contoso-migration-refactor-web-app-sql/string1.png)

3. Met behulp van Visual Studio, Contoso wordt het oplossingsbestand geopend vanuit de **SmartHotel360-interne-na-apps** map. De **connectionStrings** sectie van het bestand web.config voor de WCF-service SmartHotel.Registration.Wcf moet worden bijgewerkt met de verbindingsreeks.

     ![Verbindingsreeks](media/contoso-migration-refactor-web-app-sql/string2.png)

4. De **client** gedeelte van het bestand web.config voor de SmartHotel.Registration.Web moet worden gewijzigd om te verwijzen naar de nieuwe locatie van de WCF-service. Dit is de URL van de WCF-web-app die als host fungeert de service-eindpunt.

    ![Verbindingsreeks](media/contoso-migration-refactor-web-app-sql/strings3.png)


## <a name="step-5-publish-web-apps"></a>Stap 5: Web-apps publiceren

Als laatste stap, Contoso de web-apps naar Azure gepubliceerd.

1. In Visual Studio, ze met de rechtermuisknop op het project SmartHotel.REgistration.Wcf > **publiceren**.

    ![Publiceren](media/contoso-migration-refactor-web-app-sql/publish-web1.png)

2. Ze start publiceren.

    ![Publiceren](media/contoso-migration-refactor-web-app-sql/publish-web2.png)

3. Ze selecteert u een bestaande App-Service omdat ze de web-app al hebt gemaakt.

    ![Publiceren](media/contoso-migration-refactor-web-app-sql/publish-web3.png)

4. Nadat ze de WCF-app hebt geselecteerd, wordt het door Visual Studio implementeert.

    ![Publiceren](media/contoso-migration-refactor-web-app-sql/publish-web4.png)

5. Ze vervolgens herhalen het proces voor het publiceren van de web-app - SmartHotel.Registration.Web.

    ![Publiceren](media/contoso-migration-refactor-web-app-sql/publish-web5.png)


De toepassing is op dit moment is gemigreerd naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Na de migratie moet Contoso deze opschonen stappen uitvoeren:  

- De on-premises VM's van de vCenter-voorraad verwijderen.
- Verwijder de virtuele machines uit lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locaties voor de app SmartHotel bijwerken. De database die wordt uitgevoerd in Azure SQL-database en de front-end dat deze wordt uitgevoerd in twee web-apps weergegeven.
- Alle resources die interactie met de uit bedrijf genomen VM's hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.


## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

- Contoso nodig om ervoor te zorgen dat hun nieuwe **SmartHotel registratie** -database is beveiligd. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- In het bijzonder, moeten zij de web-apps voor het gebruik van SSL met certificaten bijwerken.

### <a name="backups"></a>Back-ups

- Contoso moet voor een overzicht back-vereisten voor de Azure SQL-Database. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Ze moeten rekening houden met failovergroepen om regionale failover voor de database te implementeren. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Contoso moet rekening houden met het implementeren van de Web-App in de belangrijkste VS-Oost 2 en de regio VS-midden voor flexibiliteit. Ze kunnen Traffic Manager om te controleren of failover in het geval van regionale storingen configureren.

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Nadat alle resources worden geïmplementeerd, Contoso Azure tags op basis van moet toewijzen hun [infrastructuur plannen](contoso-migration-infrastructure.md#set-up-tagging).
- Alle licenties zijn ingebouwd in de kosten van de PaaS-services die van Contoso gebruikmaakt al. Dit wordt afgetrokken van de EA.
1. Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management. 

## <a name="conclusion"></a>Conclusie

In dit artikel is geherstructureerd Contoso de SmartHotel-app in Azure door het app-front-end virtuele machine migreren naar twee Azure-Web-Apps. Ze de app-database hebt gemigreerd naar een Azure SQL database.






