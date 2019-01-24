---
title: On-premises workloads evalueren voor Contoso migratie naar Azure | Microsoft Docs
description: Meer informatie over hoe Contoso de on-premises computers voor migratie naar Azure met behulp van Azure Migrate en Data Migration Assistant beoordeelt.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: 18e8b7699a388dabbf0e4f5cff7d4fb927e549c2
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853813"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migratie van Contoso: On-premises workloads evalueren voor migratie naar Azure

In dit artikel beoordeelt Contoso zijn on-premises SmartHotel360-app voor migratie naar Azure.

In dit artikel maakt deel uit van een serie die documenten hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en gedetailleerde implementatie-scenario's die laten zien hoe u voor het instellen van de infrastructuur van een migratie, de geschiktheid van on-premises bronnen voor migratie beoordelen en verschillende typen migraties worden uitgevoerd. Scenario's toeneemt in complexiteit. Artikelen wordt toegevoegd aan de reeks na verloop van tijd.

Artikel | Details | Status
--- | --- | ---
[Artikel 1: Overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle artikelen in de reeks. | Beschikbaar
Artikel 3: On-premises resources voor migratie naar Azure evalueren | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | In dit artikel
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. De front-end met behulp van de Azure Site Recovery-service-app worden gemigreerd. Deze migreert de app-database naar een Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service. | Beschikbaar
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep rehost](contoso-migration-rehost-vm-sql-ag.md) | Contoso migreert de SmartHotel360-app met behulp van Site Recovery voor het migreren van virtuele machines van de app en de Database Migration Service voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[7-artikel: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso is een lift-and-shift-migratie van de Linux-osTicket-app naar Azure-VM's, met behulp van de Site Recovery-service voltooid. | Beschikbaar
[8-artikel: Een Linux-app op Azure VM's en Azure Database for MySQL rehost](contoso-migration-rehost-linux-vm-mysql.md) | De Linux-app osTicket migreert Contoso naar Azure-VM's met behulp van Site Recovery. Deze migreert de app-database naar Azure Database for MySQL via MySQL Workbench. | Beschikbaar
[9-artikel: Een app in een Azure-web-app en Azure SQL Database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso-migreert de SmartHotel360-app naar een Azure-web-app en de app-database migreert naar een Azure SQL Server-exemplaar met de Database Migration Assistant. | Beschikbaar
[Artikel 10: Een Linux-app in een Azure-web-app en de Azure Database for MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar
[11-artikel: Herstructureren van Team Foundation Server op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure. | Beschikbaar
[12-artikel: Een app in Azure-containers en Azure SQL Database opnieuw modelleren](contoso-migration-rearchitect-container-sql.md) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | Beschikbaar
[13-artikel: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar
[14-artikel: Schalen van een migratie naar Azure](contoso-migration-scale.md) | Na het proberen van migratie combinaties, bereidt Contoso worden uitgebreid naar een volledige migratie naar Azure. | Beschikbaar


## <a name="overview"></a>Overzicht

Contoso acht migreren naar Azure, wil het bedrijf een technische en financiële evaluatie om te bepalen of de on-premises werkbelastingen wel geschikt voor migratie naar de cloud uitvoeren. In het bijzonder wil de Contoso-team voor de beoordeling van machine- en databasecompatibiliteit voor migratie. Het bedrijf wil schatting capaciteit en de kosten voor het uitvoeren van de Contoso-resources in Azure.

Aan de slag en meer inzicht in de betreffende technologieën, Contoso beoordeelt twee van de on-premises apps, in de volgende tabel samengevat. Het bedrijf beoordeelt voor migratiescenario's die rehost en herstructureren apps voor migratie. Meer informatie over het opnieuw hosten en herstructurering de [Contoso Migratieoverzicht](contoso-migration-overview.md).

Naam van app | Platform | App-lagen | Details
--- | --- | --- | ---
SmartHotel360<br/><br/> (beheert Contoso reizen vereisten) | Wordt uitgevoerd op Windows met een SQL Server-database | App met twee lagen. De front-end-ASP.NET-website wordt uitgevoerd op één virtuele machine (**WEBVM**) en de SQL-Server wordt uitgevoerd op een andere virtuele machine (**SQLVM**). | VM's zijn VMware, die worden uitgevoerd op een ESXi host die wordt beheerd door vCenter-Server.<br/><br/> U kunt de voorbeeld-app vanuit downloaden [GitHub](https://github.com/Microsoft/SmartHotel360).
osTicket<br/><br/> (Contoso-servicedesk-app) | Wordt uitgevoerd op Linux/Apache met MySQL PHP (LAMP) | App met twee lagen. Een front-end-PHP-website wordt uitgevoerd op één virtuele machine (**OSTICKETWEB**) en de MySQL-database wordt uitgevoerd op een andere virtuele machine (**OSTICKETMYSQL**). | De app wordt gebruikt door de klant service-apps voor het bijhouden van problemen voor interne werknemers en externe klanten.<br/><br/> U kunt het voorbeeld van downloaden [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Huidige architectuur

In dit diagram ziet u de huidige on-premises infrastructuur voor Contoso:

![Huidige Contoso-architectuur](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso heeft een belangrijkste datacenter. Het datacenter bevindt zich in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Contoso heeft drie extra lokale branches in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet met een fiber Metro Ethernet-verbinding (500 MBps).
- Elke vertakking is lokaal verbonden met het internet met behulp van kwalitatief hoogwaardige verbindingen met IPsec VPN-tunnels terug naar het primaire datacenter. De installatie, kan de hele netwerk van Contoso permanent zijn verbonden en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter is volledig gevirtualiseerd met VMware. Contoso heeft twee ESXi 6.5-virtualisatiehosts die worden beheerd door vCenter Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteitsbeheer. Contoso maakt gebruik van DNS-servers in het interne netwerk.
- De domeincontrollers in het datacenter worden uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale branches uitgevoerd op fysieke servers.

## <a name="business-drivers"></a>Zakelijke drijfveren

Het Contoso IT de leiding heeft nauw samengewerkt met zakelijke partners van het bedrijf om te begrijpen wat het bedrijf wil bereiken met deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit. Als gevolg toegenomen druk op de on-premises systemen en de infrastructuur van het bedrijf.
- **De efficiëntie verhogen**: Contoso moet voor het verwijderen van onnodige procedures en processen voor de ontwikkelaars en gebruikers te stroomlijnen. De zakelijke behoeften IT zijn snel en om niet afval tijd of geld, dus het bedrijf kunnen sneller leveren op eisen van klanten.
- **De veerkracht vergroten**:  Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk sneller dan de wijzigingen die plaatsvinden in de marketplace voor het bedrijf om in een globale economie succesvol te kunnen reageren. IT bij Contoso moet niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: Als van het bedrijf is groeit, moet: Contoso IT-systemen die hetzelfde tempo kunnen groeien opgeven.

## <a name="assessment-goals"></a>Evaluatie van de doelstellingen

Het Contoso-cloud-team heeft de doelstellingen voor de migratie-evaluaties geïdentificeerd:

- Na de migratie, moeten de apps in Azure dezelfde als de prestatiemogelijkheden die apps vandaag nog in on-premises VMWare-omgeving van Contoso hebben hebben. Verplaatsen naar de cloud betekent niet dat de app-prestaties minder kritiek is.
- Contoso moet informatie over de compatibiliteit van de toepassingen en -databases met Azure-vereisten. Contoso moet ook informatie over de hostingopties in Azure.
- Beheer van Contoso database moet worden geminimaliseerd nadat apps naar de cloud verplaatsen.  
- Contoso wil weten niet alleen de opties voor de migratie, maar ook de kosten die zijn gekoppeld aan de infrastructuur, nadat deze is verplaatst naar de cloud.

## <a name="assessment-tools"></a>Analyseprogramma 's

Contoso maakt gebruik van Microsoft-hulpprogramma's voor de migratie-analyse. De hulpprogramma's zijn afgestemd op de doelstellingen van het bedrijf en moeten voorzien van Contoso alle gegevens die nodig zijn.

Technologie | Description | Kosten
--- | --- | ---
[Gegevensmigratieassistent](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso maakt gebruik van Data Migration Assistant om te beoordelen en detecteert compatibiliteitsproblemen die invloed kunnen hebben op de databasefunctionaliteit in Azure. Data Migration Assistant beoordeelt de functiepariteit tussen SQL-bronnen en doelen. Dit is de aanbevolen verbeteringen in de prestaties en betrouwbaarheid. | Data Migration Assistant is een gratis, downloadbare-hulpprogramma.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso maakt gebruik van de service Azure Migrate voor de beoordeling van de virtuele VMware-machines. Azure Migrate beoordeelt de geschiktheid voor migratie van de machines. Het biedt formaat wijzigen en kostenramingen voor het uitvoeren in Azure.  | Vanaf mei 2018 is Azure Migrate een gratis service.
[Serviceoverzicht](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Migrate maakt gebruik van servicetoewijzing om afhankelijkheden tussen de machines die het bedrijf wil migreren weer te geven. | Servicetoewijzing is een onderdeel van Azure Log Analytics. Op dit moment kunt Contoso Serviceoverzicht voor 180 dagen kosteloos gebruiken.

In dit scenario, Contoso downloadt en Data Migration Assistant voor het evalueren van de on-premises SQL Server-database voor de reis-app wordt uitgevoerd. Contoso maakt gebruik van Azure Migrate met afhankelijkheidstoewijzing om te beoordelen van de VM's van de app voorafgaand aan de migratie naar Azure.

## <a name="assessment-architecture"></a>Evaluatie-architectuur

![Beoordelingsarchitectuur voor migratie](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso is een fictieve naam die een typische bedrijfsorganisatie vertegenwoordigt.
- Contoso heeft een on-premises datacenter (**contoso-datacenter**) en on-premises domeincontrollers (**CONTOSODC1**, **CONTOSODC2**).
- Virtuele VMware-machines bevinden zich op VMware ESXi-hosts met versie 6.5 (**contosohost1**, **contosohost2**).
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**uitgevoerd op een virtuele machine).
- De SmartHotel360 reis-app heeft de volgende kenmerken:
    - De app is gelaagd over twee virtuele VMware-machines (**WEBVM** en **SQLVM**).
    - De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com**.
    - De virtuele machines worden uitgevoerd van Windows Server 2008 R2 Datacenter met SP1.
- De VMware-omgeving wordt beheerd door vCenter Server (**vcenter.contoso.com**) die wordt uitgevoerd op een virtuele machine.
- De osTicket servicedesk-app:
    - De app is gelaagd over twee virtuele machines (**OSTICKETWEB** en **OSTICKETMYSQL**).
    - De virtuele machines zijn waarop Ubuntu Linux-Server 16.04-LTS wordt uitgevoerd.
    - **OSTICKETWEB** Apache 2 en PHP 7.0 wordt uitgevoerd.
    - **OSTICKETMYSQL** MySQL 5.7.22 wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

Contoso en andere gebruikers moeten voldoen aan de volgende vereisten voor de evaluatie:

- Eigenaar of bijdrager machtigingen voor het Azure-abonnement of voor een resourcegroep in de Azure-abonnement.
- Een on-premises vCenter Server-exemplaar waarop versie 6.5, 6.0 of 5.5 wordt uitgevoerd.
- Een alleen-lezen-account in vCenter-Server of machtigingen voor het maken van een.
- Machtigingen voor het maken van een virtuele machine op de vCenter-Server-exemplaar met behulp van een ova-sjabloon.
- Ten minste één ESXi-host waarop versie 5.0 of hoger.
- Ten minste twee on-premises virtuele VMware-machines, en op één daarvan moet een SQL Server-database worden uitgevoerd.
- Machtigingen voor het Azure Migrate-agents installeren op elke virtuele machine.
- De virtuele machines moeten directe verbinding met internet hebben.  
    - U kunt de toegang tot internet beperken de [vereiste URL's](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).  
    - Als uw virtuele machines niet over een internetverbinding beschikt, de Azure [Log Analytics Gateway](../azure-monitor/platform/gateway.md) moet worden geïnstalleerd op deze en verkeer dat bestemd is via deze agent.
- De FQDN-naam van de virtuele machine waarop het SQL Server-exemplaar wordt uitgevoerd. Deze wordt gebruikt voor database-evaluatie.
- Windows Firewall op de SQL Server-VM moet externe verbindingen toestaan via TCP-poort 1433 (standaard). Deze instelling kan Data Migration Assistant om verbinding te maken.

## <a name="assessment-overview"></a>Evaluatie, overzicht

Hier ziet u hoe Contoso de evaluatie uitvoert:

> [!div class="checklist"]
> * **Stap 1: Download en installeer Data Migration Assistant**: Contoso bereidt u Data Migration Assistant voor evaluatie van de on-premises SQL Server-database.
> * **Stap 2: Beoordeling van de database met behulp van Data Migration Assistant**: Contoso wordt uitgevoerd en analyseert de evaluatie van de database.
> * **Stap 3: Voorbereiden voor de evaluatie van de virtuele machine met behulp van Azure Migrate**: Contoso stelt on-premises-accounts en past u de instellingen voor VMware.
> * **Stap 4: On-premises VM's detecteren met behulp van Azure Migrate**: Contoso maakt u een Azure Migrate collector-VM. Vervolgens wordt de collector voor het detecteren van virtuele machines voor evaluatie uitgevoerd door Contoso.
> * **Stap 5: Met behulp van Azure Migrate afhankelijkheidsanalyse voorbereiden**: Contoso installeert Azure Migrate-agents op de virtuele machines, zodat het bedrijf afhankelijkheidstoewijzing tussen virtuele machines zien kan.
> * **Stap 6: De virtuele machines met behulp van Azure Migrate beoordelen**: Contoso controleert afhankelijkheden, de virtuele machines, groepen en de evaluatie wordt uitgevoerd. Wanneer de evaluatie uitgevoerd is, analyseert Contoso de evaluatie in voorbereiding voor migratie.

## <a name="step-1-download-and-install-data-migration-assistant"></a>Stap 1: Download en installeer Data Migration Assistant

1. Contoso Data Migration Assistant van downloadt de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - Data Migration Assistant kan worden geïnstalleerd op elke computer die u verbinding met de SQL Server-exemplaar maken kunt. Contoso hoeft niet uit te voeren op de SQL Server-machine.
    - Data Migration Assistant al dan niet mogen worden uitgevoerd op de SQL Server-hostcomputer.
2. Contoso voert het gedownloade installatiebestand (DownloadMigrationAssistant.msi) om de installatie te starten.
3. Op de **voltooien** pagina Contoso selecteert **Start Microsoft Data Migration Assistant** voordat de wizard is voltooid.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel360"></a>Stap 2: Uitvoeren en analyseren van de evaluatie van de database voor SmartHotel360

Contoso Voer nu een evaluatie voor het analyseren van de on-premises SQL Server-database voor de SmartHotel360-app.

1. In Data Migration Assistant, selecteert u Contoso **nieuw** > **evaluatie**, en verstrekt vervolgens de evaluatie van de naam van het project.
2. Voor **bronservertype**, selecteert u Contoso **SQL Server op Azure Virtual Machines**.

    ![Data Migration Assistant - bron selecteren](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Op dit moment Data Migration Assistant biedt geen ondersteuning voor evaluatie voor het migreren naar een Azure SQL Database Managed Instance. Als tijdelijke oplossing, Contoso maakt gebruik van SQL Server op een Azure-VM als de veronderstelde doel voor de evaluatie.

3. In **doelversie selecteren**, Contoso SQL Server 2017 geselecteerd als de doelversie. Contoso moet deze versie selecteert, omdat dit de versie die wordt gebruikt door de SQL Database Managed Instance.
4. Contoso selecteert rapporten om u te helpen bij het detecteren van informatie over de compatibiliteit en nieuwe functies:
    - **Compatibiliteitsproblemen** Houd er rekening mee wijzigingen die kan defect raken migratie of waarvoor een kleine aanpassing vóór de migratie. Dit rapport houdt Contoso op de hoogte van alle functies die momenteel in gebruik die zijn afgeschaft. Problemen zijn op compatibiliteitsniveau ingedeeld.
    - **Aanbeveling voor nieuwe functies** opmerkingen bij de nieuwe functies in de doel-SQL Server-platform die kunnen worden gebruikt voor de database na de migratie. Nieuwe functieaanbevelingen wat zijn ingedeeld in de rubrieken **prestaties**, **Security**, en **opslag**.

    ![Data Migration Assistant - compatibiliteitsproblemen en nieuwe functies](./media/contoso-migration-assessment/dma-assessment-2.png)

2. In **verbinding maken met een server**, Contoso de naam van de virtuele machine waarop de database en de referenties voor toegang tot deze wordt ingevoerd. Contoso selecteert **servercertificaat vertrouwen** om ervoor te zorgen dat de virtuele machine toegang tot de SQL Server. Vervolgens selecteert u Contoso **Connect**.

    ![Data Migration Assistant - verbinding maken met een server](./media/contoso-migration-assessment/dma-assessment-3.png)

3. In **Add source**, Contoso voegt u de database die het bedrijf wil beoordelen en vervolgens selecteert **volgende** starten van de evaluatie.
4. De evaluatie is gemaakt.

    ![Data Migration Assistant - evaluatie maken](./media/contoso-migration-assessment/dma-assessment-4.png)

5. In **beoordelingsresultaten**, Contoso bekijkt de resultaten van de evaluatie.

### <a name="analyze-the-database-assessment"></a>De evaluatie van de database analyseren

Resultaten worden weergegeven zodra ze beschikbaar zijn. Als Contoso problemen opgelost, moet deze selecteren **evaluatie opnieuw starten** de evaluatie opnieuw uit te voeren.

1. In de **compatibiliteitsproblemen** rapporteren, Contoso wordt gecontroleerd of er problemen zijn op elk compatibiliteitsniveau. Compatibiliteitsniveaus worden als volgt aan SQL Server-versies toegewezen:

    - 100: SQL Server 2008/Azure SQL Database
    - 110: SQL Server 2012/Azure SQL Database
    - 120: SQL Server 2014/Azure SQL Database
    - 130: SQL Server 2016/Azure SQL Database
    - 140: SQL Server 2017/Azure SQL Database

    ![Data Migration Assistant - compatibiliteit van rapport](./media/contoso-migration-assessment/dma-assessment-5.png)

2. In de **Functieaanbevelingen** rapporteren, Contoso weergaven functies voor prestaties, beveiliging en opslag die door de evaluatie wordt aanbevolen na de migratie. Tal van functies worden aanbevolen, waaronder In-Memory OLTP, columnstore-indexen, Stretch Database, Always Encrypted, dynamische gegevensmaskering en transparante gegevensversleuteling.

    ![Data Migration Assistant - functie aanbevelingen rapport](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Contoso moet [transparent data encryption inschakelen](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) voor alle SQL Server-databases. Dit is zelfs belangrijker wanneer een database in de cloud is dan wanneer het on-premises is gehost. Transparante gegevensversleuteling moet alleen na de migratie worden ingeschakeld. Als transparante gegevensversleuteling is ingeschakeld, moet Contoso het certificaat of asymmetrische sleutel verplaatsen naar de hoofddatabase van de doelserver. Meer informatie over het [een transparante versleuteling beschermde-database verplaatsen naar een andere SQL Server-exemplaar](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Contoso kan de evaluatie in JSON- of CSV-indeling exporteren.

> [!NOTE]
> Voor grootschalige evaluaties:
> - Meerdere evaluaties tegelijk uitvoeren en de status van de evaluaties weergeven op de **alle evaluaties** pagina.
> - Evaluaties samenvoegen in een [SQL Server-database](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017).
> - Evaluaties samenvoegen in een [Power BI-rapport](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).

## <a name="step-3-prepare-for-vm-assessment-by-using-azure-migrate"></a>Stap 3: Evaluatie van de virtuele machine met behulp van Azure Migrate voorbereiden

Contoso moet maken van een VMware-account die Azure Migrate gebruiken kunt voor het automatisch detecteren van virtuele machines voor evaluatie, controleert u of rechten voor het maken van een virtuele machine, houd er rekening mee de poorten die moeten worden geopend en stel het niveau van de instellingen voor statistieken.

### <a name="set-up-a-vmware-account"></a>Een VMware-account instellen

VM-detectie vereist een alleen-lezen-account in vCenter-Server met de volgende eigenschappen:

- **Gebruikerstype**: Ten minste een alleen-lezen-gebruiker.
- **Machtigingen**: Voor de datacenter-object, selecteert u de **doorgeven naar de onderliggende objecten** selectievakje. Voor **rol**, selecteer **alleen-lezen**.
- **Details**: De gebruiker is toegewezen op het datacentrumniveau van het, met toegang tot alle objecten in het datacenter.
- Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven naar onderliggend** object aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM's en netwerken).

### <a name="verify-permissions-to-create-a-vm"></a>Machtigingen voor het maken van een virtuele machine verifiëren

Contoso controleert of het beschikt over machtigingen voor het maken van een virtuele machine door een bestand in .ova-indeling te importeren. Meer informatie over het [maken en toewijzen van een rol met bevoegdheden](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Poorten verifiëren

Afhankelijkheidstoewijzing maakt gebruik van de Contoso-evaluatie. Afhankelijkheidstoewijzing vereist een agent worden geïnstalleerd op virtuele machines die worden beoordeeld. De agent moet kunnen verbinding maken met Azure via TCP-poort 443 op elke virtuele machine. Meer informatie over [verbindingsvereisten](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid).

### <a name="set-statistics-settings"></a>Instellingen opgeven voor statistieken

Voordat de Contoso de implementatie begint, moet deze de instellingen voor statistieken voor de vCenter-Server op niveau 3 ingesteld.

> [!NOTE]
> - Na het instellen van het niveau van Contoso moet wachten tot ten minste een dag voordat deze de evaluatie wordt uitgevoerd. De evaluatie mogelijk anders niet werkt zoals verwacht.
> - Als het niveau hoger dan 3 is, werkt de evaluatie, maar:
>    - Prestatiegegevens voor schijven en netwerken is niet verzameld.
>    - Voor opslag wordt door Azure Migrate een standaardschijf in Azure aanbevolen die even groot is als de on-premises schijf.
>    - Een netwerkadapter wordt aanbevolen voor netwerken wordt voor elke netwerkadapter on-premises in Azure.
>    - Voor compute, Azure Migrate gekeken naar de VM-kernen en de geheugengrootte en wordt een Azure-VM met dezelfde configuratie aanbevolen. Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.
> - Zie voor meer informatie over schaling met behulp van niveau 3 [Sizing](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Het niveau instellen:

1. Contoso is geopend, is het vCenter Server-exemplaar in de vSphere-webclient.
2. Contoso selecteert **beheren** > **instellingen** > **algemene** > **bewerken**.
3. In **statistieken**, Contoso stelt de statistiek in instellingen op **niveau 3**.

    ![vCenter Server statistiekniveau](./media/contoso-migration-assessment/vcenter-statistics-level.png)

## <a name="step-4-discover-vms"></a>Stap 4: VM's detecteren

Contoso maakt voor het detecteren van virtuele machines, een Azure Migrate-project. Contoso downloadt en stelt u de collector-VM. Contoso voert vervolgens de collector voor het detecteren van de on-premises VM's.

### <a name="create-a-project"></a>Een project maken

1. In de [Azure-portal](https://portal.azure.com), zoekt naar Contoso **Azure Migrate**. Contoso maakt vervolgens een project.
2. Contoso Hiermee geeft u de naam van het project (**ContosoMigration**) en de Azure-abonnement. Een nieuwe Azure-resourcegroep wordt gemaakt (**ContosoFailoverRG**).
    > [!NOTE]
    > - U kunt een Azure Migrate-project alleen maken in de West-Centraal VS of regio VS-Oost.
    > - U kunt een migratie plannen voor elke doellocatie.
    > - De projectlocatie wordt alleen gebruikt om de metagegevens die worden verzameld van on-premises VM's worden opgeslagen.

    ![Azure Migrate - migration-project maken](./media/contoso-migration-assessment/project-1.png)

### <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Azure Migrate maakt een on-premises-VM bekend als de *collector-apparaat*. De virtuele machine detecteert on-premises VMware-machines en stuurt metagegevens over de VM's naar de service Azure Migrate. Als u het collector-apparaat instelt, Contoso downloadt van een OVA-sjabloon en importeert deze vervolgens naar de on-premises vCenter Server-exemplaar te maken van de virtuele machine.

1. In de Azure Migrate-project, selecteert u Contoso **aan de slag** > **detecteren en evalueren** > **Machines detecteren**. Contoso downloadt het OVA-sjabloonbestand.
2. Contoso kopieert de project-ID en de sleutel. Het project en de ID zijn vereist voor het configureren van de collector.

    ![Azure Migrate - het OVA-bestand downloaden](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Voordat u de virtuele machine implementeert, controleert Contoso dat het OVA-bestand beveiligd is:

1. Op de machine waarop het bestand is gedownload, wordt een administrator-opdrachtprompt geopend door Contoso.
2. Contoso de volgende opdracht voor het genereren van de hash voor het OVA-bestand wordt uitgevoerd:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    **Voorbeeld**

    ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen (versie 1.0.9.15):

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
    SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
    SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

### <a name="create-the-collector-appliance"></a>Het collector-apparaat maken

Contoso kan nu, importeer het gedownloade bestand naar de vCenter-Server-exemplaar en inrichten van de virtuele machine van de collector-apparaat:

1. In de vSphere Client-console selecteert u Contoso **bestand** > **OVF-sjabloon implementeren**.

    ![vSphere-webclient - implementeren van OVF-sjabloon](./media/contoso-migration-assessment/vcenter-wizard.png)

2. In de Wizard OVF-sjabloon implementeren Contoso selecteert **bron**, en vervolgens geeft u de locatie van het OVA-bestand.
3. In **naam en locatie**, Contoso Hiermee geeft u een weergavenaam voor de collector-VM. Vervolgens wordt het inventarisobject waarin u voor het hosten van de virtuele machine geselecteerd. Contoso geeft ook aan de host of cluster op voor het uitvoeren van het collector-apparaat.
4. In **opslag**, Contoso Hiermee geeft u de opslaglocatie. In **schijfindeling**, Contoso selecteert u hoe het bedrijf wil richt de opslagruimte.
5. In **netwerktoewijzing**, Contoso Hiermee geeft u het netwerk waarin u kunt verbinding maken met de collector-VM. Het netwerk moet verbinding met internet voor het verzenden van metagegevens naar Azure.
6. Contoso-beoordelingen van de instellingen en selecteert vervolgens **inschakelen na de implementatie** > **voltooien**. Een bericht weergegeven waarin wordt bevestigd dat de bewerking is voltooid wordt weergegeven wanneer het apparaat is gemaakt.

### <a name="run-the-collector-to-discover-vms"></a>De collector uitvoeren om virtuele machines te detecteren

Contoso voert nu de collector voor het detecteren van virtuele machines. Op dit moment de collector ondersteunt momenteel alleen **Engels (Verenigde Staten)** als de taal van besturingssysteem en de taal van de collector-interface.

1. In de vSphere Client-console selecteert u Contoso **Console openen**. Contoso Hiermee geeft u de taal, de tijdzone en het wachtwoord voorkeuren voor de collector-VM.
2. Op het bureaublad, Contoso selecteert de **collector uitvoeren** snelkoppeling.

    ![vSphere Client-console - snelkoppeling naar Collector](./media/contoso-migration-assessment/collector-shortcut.png)

3. In Azure Migrate Collector Contoso selecteert **vereisten instellen**. Contoso de licentievoorwaarden accepteert en leest de gegevens van derden.
4. De collector controleert of de virtuele machine toegang tot internet, dat de tijd is gesynchroniseerd en of de collector-service wordt uitgevoerd. (De collector-service wordt standaard op de virtuele machine geïnstalleerd.) Contoso installeert ook VMware PowerCLI.

    > [!NOTE]
    > Ervan wordt uitgegaan dat de virtuele machine rechtstreeks toegang heeft tot internet zonder een proxy.

    ![Azure Migrate Collector - vereisten controleren](./media/contoso-migration-assessment/collector-verify-prereqs.png)

5. In **opgeven van de vCenter-Serverdetails**, Contoso krijgt de FQDN-naam of IP-adres van de vCenter-Server-exemplaar en de alleen-lezen referenties gebruikt voor detectie.
6. Contoso selecteert u een bereik voor VM-detectie. De collector kan alleen virtuele machines die zich binnen het opgegeven bereik detecteren. Het bereik kan worden ingesteld op een specifieke map, een datacenter of een cluster. Het bereik mag niet meer dan 1500 virtuele machines bevatten.

    ![vCenter Server-gegevens opgeven](./media/contoso-migration-assessment/collector-connect-vcenter.png)

7. In **migratieproject opgeven**, Contoso voert de Azure Migrate project-ID en de sleutel die zijn gekopieerd uit de portal. Als u wilt de project-ID en sleutel ophalen, Contoso kan gaan naar het project **overzicht** pagina > **Machines detecteren**.  

    ![Migratieproject opgeven](./media/contoso-migration-assessment/collector-connect-azure.png)

8. In **voortgang van verzamelen weergeven**, Contoso kunt bekijkt u de detectie en controleren van de metagegevens die worden verzameld van de virtuele machines zijn binnen het bereik. De collector geeft aan hoe lang de detectie ongeveer zal duren.

    ![Voortgang van verzamelen weergeven](./media/contoso-migration-assessment/collector-collection-process.png)

### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

Wanneer de verzameling is voltooid, wordt door Contoso controleert dat de virtuele machines worden weergegeven in de portal:

1. In de Azure Migrate-project, selecteert u Contoso **beheren** > **Machines**. Contoso controleert dat de virtuele machines die het bedrijf wil detecteren worden weergegeven.

    ![Azure Migrate - gedetecteerde computers](./media/contoso-migration-assessment/discovery-complete.png)

2. De machines hebt op dit moment geen Azure Migrate-agents geïnstalleerd. Contoso moet de agents om afhankelijkheden weer te installeren.

    ![Azure Migrate - installatie van de Agent is vereist](./media/contoso-migration-assessment/machines-no-agent.png)

## <a name="step-5-prepare-for-dependency-analysis"></a>Stap 5: Afhankelijkheidsanalyse voorbereiden

Als u wilt weergeven van afhankelijkheden tussen VM's die het bedrijf wil beoordelen, Contoso downloadt en installeert agents op de VM's van de app. Contoso installeert agents op alle VM's voor de apps, zowel voor Windows en Linux.

### <a name="take-a-snapshot"></a>Een momentopname maken

Om te voorkomen dat een kopie van de virtuele machines voordat u ze aanpast, momentopname Contoso een voordat de agents zijn geïnstalleerd.

![Momentopname van VM](./media/contoso-migration-assessment/snapshot-vm.png)

### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren

1. In **Machines**, Contoso de machine selecteert. In de **afhankelijkheden** kolom Contoso selecteert **vereist installatie**.
2. In de **Machines detecteren** deelvenster Contoso:
    - Downloads van Microsoft Monitoring Agent (MMA) en voor elke Windows-VM-Agent voor afhankelijkheden.
    - De MMA en de Agent voor afhankelijkheden voor elke virtuele Linux-machine downloadt.
3. Contoso kopieert de werkruimte-ID en de sleutel. Contoso heeft de werkruimte-ID en sleutel nodig bij het installeren van de MMA.

    ![Agent downloaden](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>De agents installeren op Windows-VM 's

Contoso wordt de installatie uitgevoerd op elke virtuele machine.

#### <a name="install-the-mma-on-windows-vms"></a>De MMA installeren op Windows-VM 's

1. Contoso dubbelklikt op de gedownloade agent.
2. In **doelmap**, Contoso blijft de standaardinstallatiemap en selecteert vervolgens **volgende**.
3. In **installatieopties voor Agent**, selecteert u Contoso **de agent verbinden met Azure Log Analytics** > **volgende**.

    ![Microsoft Monitoring Agent-installatie - installatieopties voor Agent](./media/contoso-migration-assessment/mma-install.png)

4. In **Azure Log Analytics**, Contoso plakt u de werkruimte-ID en de sleutel die wordt gekopieerd uit de portal.

    ![Microsoft Monitoring Agent-installatie - Azure-logboekanalyse](./media/contoso-migration-assessment/mma-install2.png)

5. In **gereed voor installatie**, installeert Contoso de MMA.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>De afhankelijkheidsagent installeren op Windows-VM 's

1. Contoso dubbelklikt op de gedownloade Afhankelijkheidsagent.
2. Contoso de licentievoorwaarden accepteert en wacht tot de installatie is voltooid.

    ![Agent voor afhankelijkheden Setup - installatie](./media/contoso-migration-assessment/dependency-agent.png)

### <a name="install-the-agents-on-linux-vms"></a>De agents installeren op virtuele Linux-machines

Contoso wordt de installatie uitgevoerd op elke virtuele machine.

#### <a name="install-the-mma-on-linux-vms"></a>De MMA installeren op virtuele Linux-machines

1. Contoso installeert de Python-clientbibliotheek voor ctypes op elke virtuele machine met behulp van de volgende opdracht uit:

    `sudo apt-get install python-ctypeslib`
2. Contoso moet de opdracht voor het installeren van de MMA-agent als root uitvoeren. Als u wilt worden hoofdmap, Contoso de volgende opdracht wordt uitgevoerd, en voert het hoofdwachtwoord:

    `sudo -i`
3. De MMA, installeert Contoso:
    - Contoso voert de werkruimte-ID en de sleutel in de opdracht.
    - Opdrachten zijn voor 64-bits.
    - De werkruimte-ID en primaire sleutel bevinden zich in de Log Analytics-werkruimte in de Azure-portal. Selecteer **instellingen**, en selecteer vervolgens de **verbonden bronnen** tabblad.
    - Voer de volgende opdrachten de Log Analytics-agent downloaden, het valideren van de controlesom en het installeren en vrijgeven de agent:

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```

#### <a name="install-the-dependency-agent-on-linux-vms"></a>Installeer de Agent voor afhankelijkheden op virtuele Linux-machines

Nadat de MMA is geïnstalleerd, installeert Contoso de Agent voor afhankelijkheden op de Linux-VM's:

1. De Agent voor afhankelijkheden is geïnstalleerd op Linux-computers met behulp van InstallDependencyAgent-Linux64.bin, een shellscript dat een zelfuitpakkend binair bestand is. Contoso wordt uitgevoerd het bestand met behulp van sh of deze wordt toegevoegd uitvoermachtigingen naar het bestand zelf.

2. Als root installeert Contoso de Linux-Agent voor afhankelijkheden:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```

## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Stap 6: De evaluatie van de virtuele machine uitvoeren en analyseren

Contoso kan nu Controleer VM-afhankelijkheden en maak een groep. Vervolgens wordt de beoordeling van de groep uitgevoerd.

### <a name="verify-dependencies-and-create-a-group"></a>Controleer afhankelijkheden en maak een groep

1. Om te bepalen welke computers om te analyseren, selecteert u Contoso **afhankelijkheden weergeven**.

    ![Azure Migrate - machineafhankelijkheden weergeven](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Voor de sqlvm bevat afhankelijkheidskaart de de volgende gegevens:

    - Verwerken van groepen of processen met actieve netwerkverbindingen die zijn uitgevoerd op SQLVM, tijdens de opgegeven periode (standaard een uur).
    - Inkomende (client) en uitgaande (server) TCP-verbindingen naar en van alle afhankelijke machines.
    - Afhankelijke machines waarvoor de Azure Migrate-agents geïnstalleerd zijn als afzonderlijke vakken weergegeven.
    - Machines waarvoor geen agents zijn geïnstalleerd weergeven poort en IP-adresgegevens.

3. Contoso selecteert voor machines die de agent is geïnstalleerd (WEBVM), het VM-vak voor meer informatie. De informatie bevat de FQDN-naam, het besturingssysteem en de MAC-adres.

    ![Azure Migrate - groepsafhankelijkheden weergeven](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Contoso selecteert de virtuele machines om toe te voegen aan de groep (SQLVM en WEBVM). Contoso maakt gebruik van Ctrl + klikken om te selecteren van meerdere virtuele machines.
5. Contoso selecteert **groep maken**, en voert vervolgens een naam (**smarthotelapp**).

    > [!NOTE]
    > Voor gedetailleerdere afhankelijkheden kunt u het tijdsbereik uitbreiden. U kunt selecteren van een specifieke duur of begin- en einddatums.

### <a name="run-an-assessment"></a>Een evaluatie uitvoeren

1. In **groepen**, Contoso Hiermee opent u de groep (**smarthotelapp**), en selecteert vervolgens **evaluatie maken**.

    ![Azure Migrate - een evaluatie maken](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Als u wilt weergeven van de evaluatie, selecteert u Contoso **beheren** > **evaluaties**.

Contoso maakt gebruik van de standaardevaluatie-instellingen, maar u kunt [instellingen aanpassen](how-to-modify-assessment.md).

### <a name="analyze-the-vm-assessment"></a>De evaluatie van de VM analyseren

Een Azure Migrate-evaluatie bevat de informatie over de compatibiliteit van on-premises met Azure, juiste formaat voor virtuele Azure-machine voorgesteld en de geschatte maandelijkse kosten van Azure.

![Azure Migrate - rapport over de beoordeling](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Betrouwbaarheidsclassificatie controleren

![Azure Migrate - evaluatie weergeven](./media/contoso-migration-assessment/assessment-display.png)

Een evaluatie heeft een betrouwbaarheidsclassificatie van van 1 ster tot 5 sterren (1 ster is de laagste en 5 sterren de hoogste is).
- De betrouwbaarheidsclassificatie is toegewezen aan een beoordeling op basis van de beschikbaarheid van gegevenspunten die nodig zijn voor het berekenen van de evaluatie.
- De classificatie kunt u een schatting maken van de betrouwbaarheid van de aanbevelingen voor de grootte die worden geleverd door Azure Migrate.
- De betrouwbaarheidsclassificatie is handig als u de *prestatie gebaseerde schaling*. Azure Migrate mogelijk niet voldoende gegevenspunten voor groottebepaling op basis van gebruik van. Voor *zoals on-premises* grootte, de betrouwbaarheidsclassificatie is altijd 5 sterren omdat Azure Migrate alle gegevenspunten heeft heeft om de grootte van de virtuele machine.
- De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten:

   Beschikbaarheid van de gegevenspunten | Betrouwbaarheidsclassificatie
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

#### <a name="verify-azure-readiness"></a>Azure-gereedheid controleren

![Azure Migrate - evaluatie van gereedheid](./media/contoso-migration-assessment/azure-readiness.png)  

Het evaluatierapport toont de informatie die worden samengevat in de tabel. Als u wilt weergeven op prestatie gebaseerde schaling, heeft Azure Migrate de volgende informatie nodig. Als de gegevens kan niet worden verzameld, evaluatie van de grootte mogelijk niet nauwkeurig.

- Verbruiksgegevens voor CPU en geheugen.
- Lees-/schrijf-IOPS en doorvoer voor elke schijf die is gekoppeld aan de VM.
- Informatie over netwerk-in/uit voor elke netwerkadapter die aan de VM is gekoppeld.

Instelling | Vermelding | Details
--- | --- | ---
**Gereed voor Azure van VM** | Geeft aan of de virtuele machine gereed voor migratie. | Mogelijke statussen:</br><br/>- Gereed voor Azure<br/><br/>- Gereed met voorwaarden <br/><br/>- Niet gereed voor Azure<br/><br/>- Gereedheid onbekend<br/><br/> Als een virtuele machine is niet klaar bent, ziet u welke stappen u Azure Migrate.
**Azure VM-grootte** | Voor virtuele machines die gereed, met de Azure Migrate biedt de aanbeveling voor een Azure-VM-grootte. | De aanbevolen grootte is afhankelijk van evaluatie-eigenschappen:<br/><br/>- Als u groottebepaling op basis van prestaties hebt gebruikt, wordt de prestatiegeschiedenis van de virtuele machines mee in overweging genomen.<br/><br/>-Als u gebruikt *zoals on-premises*, grootte is gebaseerd op de on-premises VM-grootte en gegevens over het gebruik wordt niet gebruikt.
**Voorgesteld hulpprogramma** | Omdat de agents op virtuele Azure machines worden uitgevoerd, Azure Migrate gekeken naar de processen die worden uitgevoerd op de machine. Deze geeft aan of de machine een databasemachine is.
**VM-informatie** | Dit rapport bevat instellingen voor de on-premises virtuele machine, zoals besturingssysteem, Opstarttype en schijf-en storage.

#### <a name="review-monthly-cost-estimates"></a>Schatting van maandelijkse kosten controleren

Deze weergave toont de totale kosten voor berekeningen en opslag van het uitvoeren van de virtuele machines in Azure. U ziet ook details per VM.

![Azure Migrate - Azure-kosten](./media/contoso-migration-assessment/azure-costs.png)

- Geschatte kosten worden berekend met behulp van de aanbevelingen voor de grootte voor een virtuele machine.
- Geschatte maandelijkse kosten voor computing en opslag worden samengevoegd voor alle virtuele machines in de groep.

## <a name="clean-up-after-assessment"></a>Opschonen na de evaluatie

- Wanneer de evaluatie is voltooid, wordt het apparaat Azure Migrate als u wilt gebruiken in de toekomst evaluaties bewaard, Contoso.
- Contoso schakelt u de VMware-VM. Contoso wordt het opnieuw gebruiken wanneer u extra virtuele machines worden geëvalueerd.
- Contoso houdt de **Contoso migratie** project in Azure. Het project wordt momenteel geïmplementeerd de **ContosoFailoverRG** resourcegroep in de oostelijke VS Azure-regio.
-  De collector-VM heeft een licentie van de evaluatieperiode van 180 dagen. Als deze limiet is verlopen, moet Contoso voor het downloaden van de collector en het opnieuw instellen.

## <a name="conclusion"></a>Conclusie

In dit scenario beoordeelt Contoso de SmartHotel360-app-database met behulp van het hulpprogramma voor migratie-analyse met gegevens. Deze beoordeelt de on-premises VM's met behulp van de service Azure Migrate. Contoso beoordeelt de evaluaties om ervoor te zorgen dat on-premises resources gereed zijn voor migratie naar Azure.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks naamconflicten Contoso de SmartHotel360-app in Azure met behulp van een lift-and-shift-migratie. De front-end-WEBVM voor de app, Contoso worden gemigreerd met behulp van Azure Site Recovery. Deze migreert de app-database naar een Azure SQL Database Managed Instance met behulp van de Database Migration Service. [Aan de slag](contoso-migration-rehost-vm-sql-managed-instance.md) met deze implementatie.
