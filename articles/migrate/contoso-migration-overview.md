---
title: Overzicht van Contoso migratie naar Azure | Microsoft Docs
description: Biedt een overzicht van de strategie voor clientmigratie en scenario's die worden gebruikt door Contoso hun on-premises datacentrum migreren naar Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 4cad84d1cabd140f4846cb823e8c4b7ce11bc1f6
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008243"
---
# <a name="contoso-migration-overview"></a>Migratie van Contoso: overzicht


In dit artikel laat zien hoe het fictieve organisatie Contoso migreert met een on-premises infrastructuur naar de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) cloud. 

Dit document is de eerste in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso naar Azure migreert. De reeks bevat informatie en scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en aanvullende artikelen na verloop van tijd toegevoegd. Artikelen wordt beschreven hoe in het bedrijf Contoso de missie van de migratie is voltooid, maar aanwijzers voor algemene lezen en specifieke instructies vindt u in de gehele.

## <a name="introduction"></a>Inleiding

Azure biedt toegang tot een uitgebreide set cloudservices. Als ontwikkelaars en IT-professionals, kunt u deze services bouwen, implementeren en beheren van toepassingen op tal van hulpprogramma's en frameworks, via een mondiaal netwerk van datacenters. Als uw bedrijf de digitale verschuiving een grote uitdaging vindt, kan de Azure-cloud u helpen om uit te zoeken hoe u resources en werkzaamheden optimaliseert, hoe u dit aanpakt met uw klanten en werknemers en hoe u uw producten transformeert.

Ondanks alle voordelen die de cloud biedt op het gebied van snelheid en flexibiliteit, minimale kosten, hoge prestaties en betrouwbaarheid, beseft Azure dat veel organisaties nog enige tijd met on-premises datacenters moeten werken. Om de barrières voor acceptatie van de cloud te overkomen, biedt Azure een hybride cloudstrategie die een brug slaat tussen uw on-premises datacenters en de openbare Azure-cloud. Bijvoorbeeld, het gebruik van Azure-cloudresources, zoals back-ups om on-premises resources te beschermen, of het gebruik van Azure-analysen om inzicht te krijgen in on-premises workloads. 

Als onderdeel van de hybride-cloudstrategie biedt Azure steeds meer oplossingen voor het migreren van on-premises toepassingen en workloads naar de cloud. Met eenvoudige stappen kunt u uw on-premises resources uitgebreid evalueren om te bepalen hoe ze in de Azure-cloud zullen werken. Wanneer u over een grondige evaluatie beschikt, kunt u resources met een gerust hart migreren naar Azure. Wanneer resources in Azure werken, kunt u ze optimaliseren om toegang, flexibiliteit, beveiliging en betrouwbaarheid te behouden en te verbeteren.

## <a name="migration-strategies"></a>Migratiestrategieën

Strategieën voor migratie naar de cloud kunnen worden onderverdeeld in vier hoofdcategorieën: opnieuw hosten, herstructureren, opnieuw modelleren of opnieuw opbouwen. De strategie die u wilt vaststellen, is afhankelijk van uw zakelijke stimulansen en doelstellingen van de migratie. U kunt meerdere strategieën vast. U kan bijvoorbeeld kiezen om te opnieuw hosten (lift-and-shift) eenvoudig apps of apps die niet essentieel voor uw bedrijf, maar die meer complexe en kritieke zakelijke zijn opnieuw modelleren. We bekijken de strategieën.


**Strategie** | **Definitie** | **Wanneer u** 
--- | --- | --- 
**Opnieuw hosten** | Vaak aangeduid als een 'lift-and-shift'-migratie. Deze optie vereist geen code hoeft te wijzigen en laten we u uw bestaande apps migreren naar Azure snel. Elke app is gemigreerd is, naar de vruchten plukken van de cloud, zonder het risico en de kosten die gepaard gaan met de code hoeft te wijzigen. | Wanneer u moet apps snel naar de cloud verplaatsen.<br/><br/> Als u wilt verplaatsen van een app zonder het te wijzigen.<br/><br/> Als uw apps zijn zodanig ontworpen dat ze kunnen gebruikmaken van [Azure IaaS](https://azure.microsoft.com/overview/what-is-iaas/) schaalbaarheid na de migratie.<br/><br/> Wanneer apps zijn belangrijk voor uw bedrijf, maar u hoeft geen directe wijzigingen in de app-mogelijkheden.
**Herstructureren** | Vaak aangeduid als "opnieuw verpakken", herstructurering vereist, is de minimale wijzigingen in apps, zodat ze verbinding met maken kunnen [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas/), en gebruik van cloudaanbiedingen.<br/><br/> Bijvoorbeeld, kon u bestaande apps migreren naar Azure App Service of Azure Kubernetes Service (AKS).<br/><br/> Of u relationele en niet-relationele databases kan herstructureren in opties, zoals Azure SQL Database Managed Instance, Azure Database for MySQL, Azure Database voor PostgreSQL en Azure Cosmos DB. | Als uw app kan eenvoudig worden gebruikt om te werken in Azure.<br/><br/> Als u wilt toepassen innovatieve DevOps-praktijken die door Azure, of u erover denkt over DevOps met behulp van een strategie voor een container voor werkbelastingen.<br/><br/> Voor de herstructurering, moet u om na te denken over de draagbaarheid van uw bestaande codebasis en vaardigheden in webontwikkeling beschikbaar.
**Opnieuw modelleren** | Opnieuw voor de migratie is gericht op aanpassen en uitbreiden van app-functionaliteit en codebasis het optimaliseren van de app-architectuur voor cloudschaalbaarheid van de.<br/><br/> U kunt bijvoorbeeld een monolithische toepassing in een groep van microservices die samenwerken en schaal eenvoudig opsplitsen.<br/><br/> Of u opnieuw relationele kan modelleren en niet-relationele databases naar volledig beheerde DBaaS oplossingen, zoals Azure SQL Database Managed Instance, Azure Database for MySQL, Azure Database for PostgreSQL en Azure Cosmos DB. | Wanneer belangrijke wijzigingen op te nemen van nieuwe mogelijkheden en efficiënt werken op een platform nodig hebt in uw apps.<br/><br/> Als u gebruiken van bestaande investeringen in de toepassing wilt, voldoen aan de vereisten voor schaalbaarheid, innovatieve Azure DevOps-praktijken toepassen en zo het gebruik van virtuele machines.
**Opnieuw maken** | Opnieuw opbouwen duurt dingen een stap verder door het opnieuw opbouwen van een app maken met Azure-cloud-technologieën.<br/><br/> U kunt bijvoorbeeld groen veld apps met cloud-eigen technologieën zoals zonder server, Azure AI, Azure SQL Database Managed Instance en Azure Cosmos DB bouwen. | Als u wilt dat snelle ontwikkeling en bestaande apps hebben beperkte functionaliteit en levensduur.<br/><br/> Wanneer u bent klaar om te versnellen van innovatie van zakelijke (met inbegrip van DevOps-praktijken verstrekt door Azure), nieuwe toepassingen met behulp van technologieën voor cloudtoepassingen te bouwen en te profiteren van verbeteringen in AI, blockchains en IoT.

## <a name="migration-articles"></a>Migratieartikelen

De artikelen in de reeks worden samengevat in de onderstaande tabel.  

- Elke migratiescenario wordt aangedreven door enigszins zakelijke doelstellingen bepalen van de strategie voor clientmigratie.
- Voor elk implementatiescenario bieden we informatie over zakelijke stimulansen en doelstellingen, een voorgestelde architectuur, stappen om uit te voeren van de migratie en aanbevelingen voor het opschonen en de volgende stappen nadat de migratie is voltooid.

**Artikel** | **Details** | **Status**
--- | --- | ---
Artikel 1: overzicht | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | In dit artikel.
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
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






### <a name="demo-apps"></a>Demo-apps

De artikelen twee demo-apps - SmartHotel en osTicket gebruiken.

- **SmartHotel360**: deze app is ontwikkeld door Microsoft als een test-app die u gebruiken kunt bij het werken met Azure. Dit wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360). Het is een ASP.NET-app die is verbonden met een SQL Server-database. De app is momenteel in twee virtuele VMware-machines waarop Windows Server 2008 R2 en SQL Server 2008 R2 wordt uitgevoerd. De app virtuele machines zijn on-premises gehost en beheerd door vCenter Server.
- **osTicket**: een open source service voor helpdesk-tickets van de app die wordt uitgevoerd op Linux. U kunt dit ook downloaden via [GitHub](https://github.com/osTicket/osTicket). Huidige de app zich op twee virtuele VMware-machines met Ubuntu 16.04LTS, met behulp van Apache 2, PHP 7.0 en MySQL 5.7
    

## <a name="next-steps"></a>Volgende stappen

[Informatie over hoe](contoso-migration-infrastructure.md) Contoso stelt u een on-premises en Azure-infrastructuur voorbereiden voor migratie.



