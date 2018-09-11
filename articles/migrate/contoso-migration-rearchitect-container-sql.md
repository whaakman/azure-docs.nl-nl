---
title: Opnieuw een Contoso-app in een Azure container en een Azure SQL-Database ontwerpen | Microsoft Docs
description: Meer informatie over hoe een app in Azure Windows-containers en Azure SQL Database opnieuw modelleren in Contoso.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: raynew
ms.openlocfilehash: 00a0f396160c964144019b4cb8014f8abc34fe7a
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44304659"
---
# <a name="contoso-migration-rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>Migratie van Contoso: opnieuw ontwerpen van een on-premises-app op een Azure-container en de Azure SQL Database

In dit artikel ziet u hoe Contoso migreert en opnieuw modelleren van de SmartHotel360-app in Azure. De frontend van de app VM migreert Contoso naar een Windows Azure-container en de app-database naar een Azure SQL-database.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Aanvullende artikelen worden toegevoegd na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar   
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar 
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso een lift-and-shift-migratie van de app van de osTicket Linux Azure-virtuele machines, met behulp van Azure Site Recovery is voltooid | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso de osTicket Linux app overzet naar virtuele Azure-machines met Azure Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso de SmartHotel360-app is gemigreerd naar een Azure-Web-App en de app-database migreert naar een Azure SQL Server-exemplaar met Database Migration Assistant | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar 
[Artikel 11: Herstructureren TFS op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure. | Beschikbaar
Artikel 12: Opnieuw ontwerpen van een app op Azure-Containers en Azure SQL Database | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | In dit artikel
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso databasebeschadiging zijn SmartHotel-app met behulp van een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB... | Beschikbaar    

In dit artikel worden de twee lagen Windows gemigreerd door Contoso. NET SmartHotel360-app die worden uitgevoerd op virtuele VMware-machines naar Azure. Als u wilt deze app wilt gebruiken, wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Zakelijke drijfveren

De Contoso IT de leiding heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen stroomlijnen voor ontwikkelaars en gebruikers.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**: Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie.  Het kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: wanneer het bedrijf met succes groeit, Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde moet opgeven.
- **Kosten**: Contoso wil licentiekosten minimaliseren.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen zijn gebruikt voor het bepalen van de beste migratiemethode.

**Doelstellingen** | **Details**
--- | --- 
**App-vereisten** | De app in Azure blijft als kritiek omdat het is vandaag.<br/><br/> Deze moet dezelfde prestatiemogelijkheden hebben als op dit moment in VMWare.<br/><br/> Contoso wil ondersteunen van Windows Server 2008 R2, waarop de app op dit moment wordt uitgevoerd, en bereid bent te investeren in de app stoppen.<br/><br/> Contoso wil afstappen van SQL Server 2008 R2 naar een moderne PaaS-Database-platform, de noodzaak voor het beheer te minimaliseren.<br/><br/> Contoso wilt gebruikmaken van de investeringen in SQL Server-licenties en Software Assurance waar mogelijk.<br/><br/> Contoso wil mogelijk de weblaag app kan worden uitgebreid.
**Beperkingen** | De app bestaat uit een ASP.NET-app en een WCF-service die wordt uitgevoerd op dezelfde VM. Contoso wil dit verdeeld over twee web-apps met behulp van Azure App Service. 
**Azure-vereisten** | Contoso wil de app naar Azure verplaatsen en de App uitvoeren in een container voor het uitbreiden van de levensduur van de app. Deze wilt niet volledig vanaf maken voor het implementeren van de app in Azure. 
**DevOps** | Contoso wil verplaatsen naar een DevOps-model met dat Azure DevOps-Services voor code bouwt en release-pijplijn.

## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeert het migratieproces, met inbegrip van de Azure-services die Contoso voor de migratie wilt gebruiken.

### <a name="current-app"></a>Huidige app

- De on-premises SmartHotel360 app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.


### <a name="proposed-architecture"></a>Voorgestelde architectuur

- Voor de databaselaag van de app, Contoso in vergelijking met Azure SQL Database met behulp van SQL Server [in dit artikel](https://docs.microsoft.com/azure/sql-database/sql-database-features). Deze besloten om te gaan met Azure SQL Database voor een aantal oorzaken hebben:
    - Azure SQL Database is een beheerde relationele database-service. Dit biedt voorspelbare prestaties op meerdere serviceniveaus, met praktisch zonder beheer. Dynamische schaalbaarheid geen uitvaltijd, ingebouwde intelligente optimalisatie, en wereldwijde schaalbaarheid en beschikbaarheid van de volgende voordelen.
    - Contoso maakt gebruik van de lichtgewicht Data Migration Assistant (DMA) om te beoordelen en de on-premises database migreren naar Azure SQL.
    - Contoso kan de bestaande licenties voor gereduceerde tarieven voor een SQL-Database met behulp van Azure Hybrid Benefit voor SQL Server uitwisselen met Software Assurance. Zodoende kan tot 30% te besparen.
    - SQL Database biedt een aantal beveiligingsfuncties, met inbegrip van altijd versleuteld, dynamische gegevensmaskering en beveiliging/bedreigingsdetectie van beveiliging op rijniveau.
- Voor de laag van de web-app Contoso heeft besloten deze converteren naar de Windows-Container met behulp van Visual Studio.
    - Contoso wordt de app met behulp van Azure Service Fabric implementeren en de installatiekopie van de Windows-container ophalen uit de Azure Container Registry (ACR).
    - Een model voor het uitbreiden van de app sentimentanalyse zal worden geïmplementeerd als een andere service in Service Fabric, verbonden met Cosmos DB.  Dit wordt informatie van Tweets lezen en op de app worden weergegeven.
- Contoso gebruikt voor het implementeren van een DevOps-pijplijn, Azure DevOps-Services voor broncodebeheer (SCM), met Git-opslagplaatsen.  Geautomatiseerde builds en releases wordt gebruikt om code te bouwen en deze implementeren in de Azure Container Registry en Azure Service Fabric.

    ![Scenario-architectuur](./media/contoso-migration-rearchitect-container-sql/architecture.png) 

  
### <a name="solution-review"></a>Beoordelingen van oplossing
Contoso evalueert het ontwerp van de voorgestelde door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Professionals** | De SmartHotel360-app-code moet worden gewijzigd voor migratie naar Azure Service Fabric. De inspanningen is echter slechts weinig, met behulp van de Service Fabric-SDK-hulpprogramma's voor de wijzigingen.<br/><br/> Met de overgang naar Service Fabric beginnen Contoso met het ontwikkelen van microservices om toe te voegen aan de toepassing snel gedurende een periode, zonder risico voor de oorspronkelijke codebasis.<br/><br/> Windows-Containers bieden dezelfde voordelen als containers in het algemeen. Ze verbeteren de flexibiliteit, draagbaarheid en controle.<br/><br/> Contoso kan gebruikmaken van de investering in Software Assurance met behulp van Azure Hybrid Benefit voor SQL Server en Windows Server.<br/><br/> Na de migratie moet deze niet langer ondersteuning voor Windows Server 2008 R2. [Meer informatie](https://support.microsoft.com/lifecycle).<br/><br/> Contoso kunt de weblaag van de app configureren met meerdere instanties, zodat deze niet langer een single point of failure.<br/><br/> Het wordt niet langer afhankelijk van de ouderdom van SQL Server 2008 R2.<br/><br/> SQL Database biedt ondersteuning voor de technische vereisten van Contoso. Beheerders van Contoso de on-premises-database met behulp van de Database Migration Assistant beoordeeld en geconstateerd dat het compatibel is.<br/><br/> SQL-Database heeft ingebouwde fouttolerantie die Contoso niet nodig om in te stellen. Dit zorgt ervoor dat de gegevenslaag niet langer een single point of failover is.
**Nadelen** | Containers zijn complexer dan andere opties voor de migratie. Het leerproces voor containers is mogelijk een probleem voor Contoso.  Ze introduceren een nieuw niveau van complexiteit waarmee veel van de waarde er de curve.<br/><br/> Het operationele team bij Contoso moet aan de slag om te begrijpen en ondersteuning voor Azure, containers en microservices voor de app.<br/><br/> Als Contoso maakt gebruik van de Data Migration Assistant in plaats van de Data Migration Service voor het migreren van de database, het geen de infrastructuur die gereed zijn voor het migreren van databases op schaal.



### <a name="migration-process"></a>Migratieproces

1. Contoso voorziet de Azure service fabric-cluster voor Windows.
2. Deze bepalingen van een Azure SQL-exemplaar en de SmartHotel360-database migreert naar deze.
3. Contoso omgezet de weblaag VM in een Docker-container met behulp van de Service Fabric-SDK-hulpprogramma's.
4. Het service fabric-cluster en de ACR met elkaar verbindt en implementeert de app met behulp van Azure service fabric.

    ![Migratieproces](./media/contoso-migration-rearchitect-container-sql/migration-process.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Evalueert en detecteert compatibiliteitsproblemen die mogelijk van invloed zijn op de databasefunctionaliteit in Azure. DMA de functiepariteit tussen SQL-bronnen en doelen beoordeelt en wordt aanbevolen verbeteringen in de prestaties en betrouwbaarheid. | Het is een gratis hulpprogramma dat kan worden gedownload.
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) | Biedt een intelligente, volledig beheerde relationele clouddatabaseservice. | De kosten op basis van functies, doorvoer en grootte. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Slaat de installatiekopieën voor alle typen containerimplementaties. | De kosten op basis van functies, opslag en duur van gebruik. [Meer informatie](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Bouwt en beschikbare altijd ingeschakelde, schaalbare en gedistribueerde apps | De kosten op basis van grootte, de locatie en de duur van de rekenknooppunten. [Meer informatie](https://azure.microsoft.com/pricing/details/service-fabric/).

## <a name="prerequisites"></a>Vereiste onderdelen

Dit is wat Contoso moet worden uitgevoerd in dit scenario:

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | Contoso abonnementen eerder in dit artikel uit de serie hebt gemaakt. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso eerder een Azure-infrastructuur hebt ingesteld.
**Developer-vereisten** | Contoso heeft de volgende hulpprogramma's op een werkstation ontwikkelaar nodig:<br/><br/> - [Visual Studio 2017 Community Edition: Versie 15.5](https://www.visualstudio.com/)<br/><br/> -.NET workload is ingeschakeld.<br/><br/> - [GIT](https://git-scm.com/)<br/><br/> - [Service Fabric SDK v 3.0 of hoger](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> - [Docker CE (Windows 10) of Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) ingesteld voor het gebruik van Windows-Containers.



## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Een exemplaar van SQL Database in Azure inrichten**: Contoso richt een SQL-exemplaar in Azure. Nadat de front-end web die virtuele machine is gemigreerd naar een Azure-container, wordt het containerexemplaar met de front-end van de web-app verwijzen naar deze database.
> * **Stap 2: Maak een Azure Container Registry (ACR)**: Contoso richt een enterprise-container registry voor de docker-containerinstallatiekopieën.
> * **Stap 3: Richt Azure Service Fabric**: het product biedt een Service Fabric-Cluster.
> * **Stap 4: Service fabric certificaten beheren**: Contoso stelt u de certificaten voor Azure DevOps-Services toegang tot het cluster.
> * **Stap 5: Migreren van de database met DMA**: de app-database met de Database Migration Assistant gemigreerd.
> * **Stap 6: Azure DevOps Services instellen**: Contoso stelt u een nieuw project in Azure DevOps-Services en importeert u de code in de Git-opslagplaats.
> * **Stap 7: Om de app converteren**: Contoso converteert de app naar een container met Visual Studio en SDK's.
> * **Stap 8: Instellen van de build- en**: Contoso stelt u de build- en release-pijplijnen maken en publiceren van de app naar de ACR en Service Fabric-Cluster.
> * **Stap 9: De app uit te breiden**: nadat de app openbaar is, Contoso uitgebreid om te profiteren van de mogelijkheden van Azure en het rapport opnieuw publiceert naar Azure met behulp van de pijplijn.



## <a name="step-1-provision-an-azure-sql-database"></a>Stap 1: Een Azure SQL-Database inrichten

Beheerders van Contoso inrichten een Azure SQL-database.

1. Ze selecteren om te maken een **SQL-Database** in Azure. 

    ![Inrichten SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

2. Ze geven de naam van een database zodat deze overeenkomen met de database die wordt uitgevoerd op de on-premises VM (**SmartHotel.Registration**). Ze plaatsen de database in de resourcegroep ContosoRG. Dit is de resourcegroep die ze voor productieresources in Azure gebruiken.

    ![Inrichten SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

3. Ze instellen van een nieuwe SQL Server-exemplaar (**sql-smarthotel-eus2**) in de primaire regio.

    ![Inrichten SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

4. Ze stelt de prijscategorie aan server en database-behoeften. En ze selecteren om geld te besparen met Azure Hybrid Benefit omdat ze al een SQL Server-licentie hebben.
5. De grootte instelt ze gebruiken op basis van v-Core aanschaffen en stel de limieten voor de verwachte behoeften.

    ![Inrichten SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

6. Ze maken vervolgens de database-instantie.

    ![Inrichten SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

7. Nadat het exemplaar is gemaakt, ze openen van de database en noteer de details die ze nodig hebben wanneer ze de Database Migration Assistance voor migratie gebruiken.

    ![Inrichten SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)


**Meer hulp nodig?**

- [Hulp bij het](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) inrichten van een SQL-Database.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) resourcelimieten v-Core.



## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>Stap 2: Een ACR maken en inrichten van een Azure-Container

De Azure-container is gemaakt met behulp van de geëxporteerde bestanden van de Web-VM. De container is ondergebracht in de Azure Container Registry (ACR).


1. Beheerders van Contoso maken een Containerregister in Azure portal.

     ![Containerregister](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

2. Ze bieden een naam op voor het register (**contosoacreus2**), en plaats deze in de primaire regio, in de resourcegroep die ze voor hun beschikken over infrastructurele resources gebruiken. Deze toegang inschakelen voor gebruikers van de beheerder en deze instellen als een premium-SKU, zodat ze kunnen gebruikmaken van geo-replicatie.

    ![Containerregister](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)  


## <a name="step-3-provision-azure-service-fabric"></a>Stap 3: Richt Azure Service Fabric

De SmartHotel360-container wordt uitgevoerd in de Azure Service Fabric-Sluster. Beheerders van Contoso maken als volgt de Service Fabric-Cluster:

1. Een Service Fabric-resource maken vanuit Azure Marketplace

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

2. In **Basic**, ze bieden een unieke naam van de Active Directory voor het cluster en de referenties voor toegang tot de on-premises VM. Ze de resource in de productie-resourcegroep plaatsen (**ContosoRG**) in de primaire regio in de VS-Oost 2.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png) 

3. In **configuratie van knooppunttypen**, ze Voer een naam van het type knooppunt, duurzaamheid instellingen, VM-grootte en app-eindpunten.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png) 


4. In **sleutelkluis maken**, ze een nieuwe sleutelkluis maken in de resourcegroep voor infrastructuur, ook voor het certificaat.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png) 


5. In **toegangsbeleid** ze toegang tot virtuele machines voor het implementeren van de key vault inschakelen.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric5.png) 

6. Ze opgeven een naam voor het certificaat.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric6.png) 

7. In de pagina overzicht kopiëren zij de koppeling die wordt gebruikt voor het downloaden van het certificaat. Ze moeten dit verbinding maken met de Service Fabric-Cluster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric7.png) 

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric8.png) 

8. Nadat de validatie is geslaagd, is het inrichten van het cluster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric9.png) 

9. Ze importeren de gedownloade certificaat dev-machines in de Wizard Certificaat importeren. Het certificaat wordt gebruikt om het cluster te verifiëren.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric10.png) 

10. Nadat het cluster is ingericht, wordt ze verbinding maken met de Verkenner van Service Fabric-Cluster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric11.png) 

11. Ze moeten het juiste certificaat selecteren.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric12.png) 

12. De belasting voor de Service Fabric Explorer en de Contoso-Admin kunt beheren voor het cluster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric13.png) 


## <a name="step-4-manage-service-fabric-certificates"></a>Stap 4: Service Fabric certificaten beheren

Contoso moet clustercertificaten voor Azure DevOps-Services toegang tot het cluster. Beheerders van Contoso instellen dit.

1. Ze opent u Azure portal en blader naar de Key Vault.
2. Ze opent u de certificaten en kopieer de vingerafdruk van het certificaat dat is gemaakt tijdens het inrichtingsproces.

    ![Kopieer de vingerafdruk](./media/contoso-migration-rearchitect-container-sql/cert1.png)
 
3. Ze kopiëren naar een tekstbestand voor later gebruik.
4. Nu dat ze een clientcertificaat dat een certificaat voor de Beheerclient op het cluster wordt toevoegen. Hiermee wordt Azure DevOps Services verbinding maken met het cluster voor de implementatie van de app in de release-pijplijn. Ze doen, KeyVault open in de portal en selecteer **certificaten** > **genereren/importeren**.

    ![Clientcertificaat genereren](./media/contoso-migration-rearchitect-container-sql/cert2.png)

5. Voer de naam van het certificaat en ze bieden een x.509-DN-naam in **onderwerp**.

     ![De naam van certificaat](./media/contoso-migration-rearchitect-container-sql/cert3.png)

6. Nadat het certificaat is gemaakt, downloaden ze deze lokaal in PFX-indeling.

     ![Certificaat downloaden](./media/contoso-migration-rearchitect-container-sql/cert4.png)

7. Nu, ze gaat u terug naar de lijst met certificaten in de Key Vault en kopieer de vingerafdruk van het clientcertificaat dat zojuist is gemaakt. Ze opslaan in het tekstbestand.

     ![Client certificaatvingerafdruk](./media/contoso-migration-rearchitect-container-sql/cert5.png)

8. Voor de implementatie van Azure DevOps-Services moeten ze de met Base64-waarde van het certificaat te bepalen. Ze doen dit op de lokale developer-werkstation met behulp van PowerShell. Ze plak de uitvoer in een tekstbestand voor later gebruik.

    ```
        [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("C:\path\to\certificate.pfx")) 
    ```

     ![Base64-waarde](./media/contoso-migration-rearchitect-container-sql/cert6.png)

9. Ze ten slotte toevoegen het nieuwe certificaat aan de Service Fabric-cluster. Om dit te doen, in de portal ze openen van het cluster en klikt u op **Security**.

     ![Clientcertificaat toevoegen](./media/contoso-migration-rearchitect-container-sql/cert7.png)

10. Ze klikt u op **toevoegen** > **Beheerclient**, en plak deze in de vingerafdruk van het nieuwe certificaat. En vervolgens klikt de gebruiker op **toevoegen**. Dit kan tot 15 minuten duren.

     ![Clientcertificaat toevoegen](./media/contoso-migration-rearchitect-container-sql/cert8.png)

## <a name="step-5-migrate-the-database-with-dma"></a>Stap 5: De database met DMA migreren

Contoso-beheerders kunnen nu de SmartHotel360-database met DMA migreren.

### <a name="install-dma"></a>DMA installeren

1. Ze het hulpprogramma downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) naar de on-premises SQL Server-VM (**SQLVM**).
2. Ze uitvoeren setup (DownloadMigrationAssistant.msi) op de virtuele machine.
3. Op de **voltooien** pagina, selecteren ze **Start Microsoft Data Migration Assistant** voordat de wizard is voltooid.

### <a name="configure-the-firewall"></a>De firewall configureren

Verbinding maken met de Azure SQL Database, beheerders van Contoso een firewallregel instellen om toegang te verlenen.

1. In de **Firewall en virtuele netwerken** eigenschappen voor de database, toegang tot Azure-services toestaan en een regel voor de client-IP-adres van de on-premises SQL Server-VM toevoegen.
2. Een firewallregel op serverniveau is gemaakt.

    ![Firewall](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

Hebt u meer hulp nodig?

[Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#creating-and-managing-firewall-rules) maken en beheren van de firewallregels voor Azure SQL Database.

### <a name="migrate"></a>Migreren

Beheerders van Contoso is nu migreren van de database.

1. Maak een nieuw project in de DMA (**SmartHotelDB**) en selecteer **migratie** 
2. Ze selecteert u het brontype van de server als **SQL Server**, en het doel als **Azure SQL Database**. 

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-1.png)

3. In de migratie, die ze toevoegen **SQLVM** als de bronserver en de **SmartHotel.Registration** database. 

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-2.png)

4. Ze foutbericht een weergegeven dat lijkt te zijn gekoppeld aan de verificatie. Nadat u wilt onderzoeken, is het probleem echter de punt (.) in de naam van de database. Als tijdelijke oplossing, die ze hebben besloten voor het inrichten van een nieuwe SQL-database met behulp van de naam van de **SmartHotel registratie**, om het probleem te verhelpen. Wanneer ze DMA opnieuw worden uitgevoerd, ze kunnen selecteren **SmartHotel registratie**, en ga verder met de wizard.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-3.png)

5. In **objecten selecteren**, ze de databasetabellen selecteren en een SQL-script genereren.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-4.png)

6. Nadat het script van DMS wordt gemaakt, klikt de gebruiker op **Deploy schema**.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-5.png)

7. DMA bevestigt dat de implementatie is voltooid.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-6.png)

8. Ze nu beginnen met de migratie.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-7.png)

9. Nadat de migratie is voltooid, kan Contoso kunt controleren of de database wordt uitgevoerd op de Azure SQL-exemplaar.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-8.png)

10. De extra SQL-database worden verwijderd **SmartHotel.Registration** in Azure portal.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-9.png)


## <a name="step-6-set-up-azure-devops-services"></a>Stap 6: Azure DevOps Services instellen

Contoso heeft nodig om de infrastructuur voor DevOps en pijplijnen voor de toepassing te bouwen.  U doet dit door beheerders van Contoso een nieuw Azure DevOps-project maken, importeren van hun code bouwen en pipelines vrijgeven.

1.   In de Contoso Azure DevOps-account, ze een nieuw project maken (**ContosoSmartHotelRearchitect**), en selecteer **Git** voor versiebeheer.

    ![Nieuw project](./media/contoso-migration-rearchitect-container-sql/vsts1.png)

2. Ze importeren de Git-opslagplaats die op dit moment de app-code bevat. Deel uitmaakt van een [openbare opslagplaats](https://github.com/Microsoft/SmartHotel360-internal-booking-apps) en kunt u deze downloaden.

    ![App-code downloaden](./media/contoso-migration-rearchitect-container-sql/vsts2.png)

3. Nadat de code is geïmporteerd, ze verbinding maken met Visual Studio naar de opslagplaats en kloon de code met behulp van Team Explorer.

    ![Verbinding maken met de opslagplaats](./media/contoso-migration-rearchitect-container-sql/vsts3.png)

4. Nadat de opslagplaats hebt gekloond naar de machine voor ontwikkelaars, opent u het oplossingsbestand voor de app. De web-app en wcf-service hebben afzonderlijke-project binnen het bestand.

    ![Oplossingsbestand](./media/contoso-migration-rearchitect-container-sql/vsts4.png)

## <a name="step-7-convert-the-app-to-a-container"></a>Stap 7: De app converteren naar een container

De on-premises-app is een traditionele app met drie lagen:

- Het bevat webformulieren en een WCF-Service verbinding maken met SQL Server.
- Entity Framework wordt gebruikt om te integreren met de gegevens in de SQL-database, beschikbaar te maken via een WCF-service.
- De toepassing webforms en communiceert met de WCF-service.

Contoso-beheerders om de app converteren naar een container met behulp van isual Studio en de SDK-hulpprogramma's als volgt:


1. Met behulp van Visual Studio, zij het oplossingsbestand open-(SmartHotel.Registration.sln) bekijken in de **SmartHotel360-interne-na-apps\src\Registration** map van de lokale opslagplaats.  Twee apps worden weergegeven. De webfrontend SmartHotel.Registration.Web en de WCF-service-app SmartHotel.Registration.WCF.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container2.png)


2. Ze met de rechtermuisknop op de web-app > **toevoegen** > **Orchestrator Containerondersteuning**.
3. In **Containerondersteuning orkest toevoegen**, selecteren ze **Service Fabric**.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container3.png)
    
4. Ze Herhaal het proces voor SmartHotel.Registration.WCF app.
5. Zij controleren nu hoe de oplossing is gewijzigd.

    - De nieuwe app is **SmartHotel.RegistrationApplication/**
    - Deze twee services bevat: **SmartHotel.Registration.WCF** en **SmartHotel.Registration.Web**.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container4.png)

6. Visual Studio het Docker-bestand gemaakt en opgevraagd de vereiste afbeeldingen lokaal op de machine voor ontwikkelaars.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container5.png)

7. Een manifestbestand (**ServiceManifest.xml**) wordt gemaakt en geopend in Visual Studio. Dit bestand legt uit hoe het configureren van de container als deze geïmplementeerd in Azure Service Fabric.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container6.png)

8. Een andere manifestbestand (** ApplicationManifest.xml) bevat de configuratie van toepassingen voor de containers.

    ![Container](./media/contoso-migration-rearchitect-container-sql/container7.png)

9. Ze opent de **ApplicationParameters/Cloud.xml** -bestand en de verbindingsreeks voor de app verbinding met de Azure SQL-database bijwerken. De verbindingsreeks kan zich bevinden in de database in Azure portal.

    ![Verbindingsreeks](./media/contoso-migration-rearchitect-container-sql/container8.png)

10. Ze de bijgewerkte code doorvoeren en pushen naar Azure DevOps-Services.

    ![Doorvoeren](./media/contoso-migration-rearchitect-container-sql/container9.png)

## <a name="step-8-build-and-release-pipelines-in-azure-devops-services"></a>Stap 8: Bouw en release-pijplijnen in Azure DevOps-Services

Beheerders van Contoso is nu configureren voor Azure DevOps-Services voor het uitvoeren van build en release proces actie de DevOps-praktijken.

1. Klik in Azure DevOps-Services, ze op **bouwen en uitbrengen** > **nieuwe pijplijn**.

    ![Nieuwe pijplijn](./media/contoso-migration-rearchitect-container-sql/pipeline1.png)

2. Ze selecteren **Azure DevOps Services Git** en de relevante opslagplaats.

    ![De opslagplaats en GIT](./media/contoso-migration-rearchitect-container-sql/pipeline2.png)

3. In **selecteert u een sjabloon**, selecteren ze fabric met Docker-ondersteuning.

     ![Fabric- en Docker](./media/contoso-migration-rearchitect-container-sql/pipeline3.png)
    
4. Ze wijzigen van de installatiekopieën van het label voor het bouwen van de installatiekopie en de taak voor het gebruik van de ingerichte ACR configureren.

     ![Register](./media/contoso-migration-rearchitect-container-sql/pipeline4.png)

5. In de **installatiekopieën pushen** taak, ze de afbeelding om te worden puhed naar de ACR configureren en selecteren om op te nemen van de meest recente code.
6. In **Triggers**, ze inschakelen van continue integratie en toevoegen van de master-vertakking.

    ![Triggers](./media/contoso-migration-rearchitect-container-sql/pipeline5.png)

7. Ze klikt u op **Save and Queue** een build te starten.
8. Nadat de build is voltooid, gaan ze naar de release-pijplijn. Klik in Azure DevOps-Services die ze op **Releases** > **nieuwe pijplijn**.

    ![Release-pijplijn](./media/contoso-migration-rearchitect-container-sql/pipeline6.png)    

9. Ze selecteert de **implementatie van Azure Service Fabric** sjabloon en de naam van de omgeving (**SmartHotelSF**).

    ![Omgeving](./media/contoso-migration-rearchitect-container-sql/pipeline7.png)

10. Ze bieden de pijplijnnaam van een (**ContosoSmartHotelRearchitect**). Voor de omgeving, klikt de gebruiker op **fase 1, 1 taak** om de Service Fabric-implementatie te configureren.

    ![Fase en taak](./media/contoso-migration-rearchitect-container-sql/pipeline8.png)

11. Klik nu op ze **nieuw** om toe te voegen een nieuwe clusterverbinding.

    ![Nieuwe verbinding](./media/contoso-migration-rearchitect-container-sql/pipeline9.png)

12. In **toevoegen Service Fabric-serviceverbinding**, configureren van de verbinding en de verificatie-instellingen die worden gebruikt door Azure DevOps-Services om de app te implementeren. Het clustereindpunt kan zich bevinden in de Azure-portal, en ze toevoegen **tcp: / /** als voorvoegsel.
13. De verzamelde gegevens van het certificaat is ingevoerd in **vingerafdruk voor servercertificaat** en **clientcertificaat**.

    ![Certificaat](./media/contoso-migration-rearchitect-container-sql/pipeline10.png)

13. Ze klikt u op de pijplijn > **toevoegen van een artefact**.

     ![Artefact](./media/contoso-migration-rearchitect-container-sql/pipeline11.png)

14. Ze selecteert u het project en build-pijplijn, met behulp van de meest recente versie.

     ![Build](./media/contoso-migration-rearchitect-container-sql/pipeline12.png)

15. Houd er rekening mee dat de lightning-bolt op het artefact is ingeschakeld.

     ![Status van het artefact](./media/contoso-migration-rearchitect-container-sql/pipeline13.png)

16. Houd er bovendien rekening mee dat de trigger voor continue implementatie is ingeschakeld.

   ![Continue implementatie ingeschakeld](./media/contoso-migration-rearchitect-container-sql/pipeline14.png) 

17. Ze klikt u op **opslaan** > **maken van een release**.

    ![Release](./media/contoso-migration-rearchitect-container-sql/pipeline15.png)

18. Nadat de implementatie is voltooid, wordt de Service Fabric van SmartHotel360 wordt nu uitgevoerd.

    ![Publiceren](./media/contoso-migration-rearchitect-container-sql/publish4.png)

19. Voor verbinding met de app, zorgt ze ervoor dat verkeer naar het openbare IP-adres van de Azure load balancer vóór de Service Fabric-knooppunten.

    ![Publiceren](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-9-extend-the-app-and-republish"></a>Stap 9: De app uit te breiden en opnieuw publiceren

Wanneer de SmartHotel360-app en database worden uitgevoerd in Azure, wil Contoso de app uit te breiden.

- Contoso ontwikkelaars zijn prototypen een nieuwe .NET Core-toepassing die wordt uitgevoerd op de Service Fabric-cluster.
- De app zal worden gebruikt om gevoel gegevens ophalen uit cosmos DB.
- Deze gegevens worden in de vorm van Tweets die worden verwerkt met behulp van een Azure-functie zonder server en de Cognitive Services Text Analysis API.

### <a name="provision-azure-cosmos-db"></a>Azure Cosmos DB inrichten

Als een eerste stap is voor beheerders van Contoso een Azure Cosmos-database inrichten.

1. Een Azure Cosmos DB-resource maken ze vanuit de Azure Marketplace.

    ![Breid uit](./media/contoso-migration-rearchitect-container-sql/extend1.png)

2. Ze bieden een databasenaam (**contososmarthotel**), selecteert u de SQL-API en plaatst u de resource in de productie-resourcegroep, in de primaire regio in de VS-Oost 2.

    ![Breid uit](./media/contoso-migration-rearchitect-container-sql/extend2.png)

3. In **aan de slag**, selecteren ze **Data Explorer**, en een nieuwe verzameling toevoegen.
4. In **verzameling toevoegen** ze bieden id's en opslagcapaciteit en doorvoer.

    ![Breid uit](./media/contoso-migration-rearchitect-container-sql/extend3.png)

5. In de portal, ze de nieuwe database openen > **verzameling** > **documenten** en klikt u op **Nieuw Document**.
6. Ze plak de volgende JSON-code in het documentvenster. Dit is de voorbeeldgegevens in de vorm van een enkele tweet.

    ```
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel360",
            "language": "en",
            "sentiment": 0.5,
            "retweet_count": 1,
            "followers": 500, 
            "hashtags": [
                ""
            ]
    }
    ```

    ![Breid uit](./media/contoso-migration-rearchitect-container-sql/extend4.png)

7. Ze Zoek het Cosmos DB-eindpunt en de verificatiesleutel. Deze worden gebruikt in de app verbinding maken met de verzameling. In de database, klikt de gebruiker op **sleutels**, en kopieer de URI en primaire sleutel naar Kladblok.

    ![Breid uit](./media/contoso-migration-rearchitect-container-sql/extend5.png)

### <a name="update-the-sentiment-app"></a>De sentiment-app bijwerken

Met de Cosmos DB is ingericht, wordt de verbinding maken met de app in Contoso-beheerders kunnen configureren.

1. Ze openen bestand ApplicationModern\ApplicationParameters\cloud.xml in Solution Explorer in Visual Studio.

    ![Sentiment-app](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

2. Ze vullen in de volgende twee parameters:

   ```
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```
   
   ```
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![Sentiment-app](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>De app publiceren

Nadat de app is uitgebreid, beheerders van Contoso opnieuw publiceren naar Azure met behulp van de pijplijn.

1. Ze deze doorvoeren en hun code hebben gepusht naar Azure DevOps-Services. Dit is serversysteemstatus van de build- en release-pijplijnen.

2. Na de build en de implementatie is voltooid, wordt SmartHotel360 nu uitgevoerd Service Fabric. Drie services worden nu weergegeven in de console Beheer van uw Fabric gevolgd.

    ![Opnieuw publiceren](./media/contoso-migration-rearchitect-container-sql/republish3.png)

3. Ze kunnen nu via de services om te zien dat de SentimentIntegration-app actief en werkend is klikken

    ![Opnieuw publiceren](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Na de migratie moet Contoso deze opschonen stappen uitvoeren:  

- De on-premises VM's van de vCenter-voorraad verwijderen.
- Verwijder de virtuele machines uit lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locaties voor de SmartHotel360-app bijwerken. De database die wordt uitgevoerd in Azure SQL-database en de front-end dat deze wordt uitgevoerd in Service Fabric weergeven.
- Alle resources die interactie met de uit bedrijf genomen VM's hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.


## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

- Beheerders van Contoso nodig om ervoor te zorgen dat hun nieuwe **SmartHotel registratie** -database is beveiligd. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- In het bijzonder, moeten zij de container voor het gebruik van SSL met certificaten bijwerken.
- Ze moeten overwegen KeyVault-geheimen voor hun Service Fabric-apps beveiligen. [Meer informatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups"></a>Back-ups

- Contoso moet voor een overzicht back-vereisten voor de Azure SQL-Database. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Contoso-beheerders moet rekening houden met failovergroepen om regionale failover voor de database te implementeren. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Geo-replicatie voor de premium-SKU van de ACR kunnen gebruikmaken van. [Meer informatie](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
- Contoso moet rekening houden met de Web-App in de belangrijkste VS-Oost 2 en de regio VS-midden implementeren als u Web App for Containers beschikbaar. Beheerders van Contoso kan Traffic Manager om te controleren of failover in het geval van regionale storingen te configureren.
- Cosmos DB maakt automatisch een reservekopie van. Contoso [meer informatie over](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore) dit proces voor meer informatie.

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Nadat alle resources worden geïmplementeerd, Contoso Azure tags op basis van moet toewijzen [infrastructuur plannen](contoso-migration-infrastructure.md#set-up-tagging).
- Alle licenties zijn ingebouwd in de kosten van de PaaS-services die van Contoso gebruikmaakt al. Dit wordt afgetrokken van de EA.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.

## <a name="conclusion"></a>Conclusie

In dit artikel is geherstructureerd Contoso de SmartHotel360-app in Azure door het app-front-end virtuele machine migreren naar Service Fabric. De app-database is gemigreerd naar een Azure SQL database.





