---
title: Overzicht van Contoso migratie naar Azure | Microsoft Docs
description: Biedt een overzicht van de strategie voor clientmigratie en scenario's die worden gebruikt door Contoso hun on-premises datacentrum migreren naar Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: raynew
ms.openlocfilehash: 1a86f12fddf9b18700f2c4a2aff840512dfe4a6f
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303639"
---
# <a name="contoso-migration-overview"></a>Migratie van Contoso: overzicht


In dit artikel laat zien hoe het fictieve organisatie Contoso migreert on-premises infrastructuur naar de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) cloud. 

Dit document is de eerste in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso naar Azure migreert. De reeks bevat informatie en scenario's die laten zien hoe u een migratie van de infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en aanvullende artikelen na verloop van tijd toegevoegd. De artikelen wordt beschreven hoe in het bedrijf Contoso de missie van de migratie is voltooid, maar aanwijzers voor algemene lezen en specifieke instructies vindt u in de gehele.

## <a name="introduction"></a>Inleiding

Azure biedt toegang tot een uitgebreide set cloudservices. Als ontwikkelaars en IT-professionals, kunt u deze services bouwen, implementeren en beheren van toepassingen op tal van hulpprogramma's en frameworks, via een mondiaal netwerk van datacenters. Als uw bedrijf de digitale verschuiving een grote uitdaging vindt, kan de Azure-cloud u helpen om uit te zoeken hoe u resources en werkzaamheden optimaliseert, hoe u dit aanpakt met uw klanten en werknemers en hoe u uw producten transformeert.

Ondanks alle voordelen die de cloud biedt op het gebied van snelheid en flexibiliteit, minimale kosten, hoge prestaties en betrouwbaarheid, beseft Azure dat veel organisaties nog enige tijd met on-premises datacenters moeten werken. Om de barrières voor acceptatie van de cloud te overkomen, biedt Azure een hybride cloudstrategie die een brug slaat tussen uw on-premises datacenters en de openbare Azure-cloud. Bijvoorbeeld, met behulp van Azure-cloudresources zoals Azure back-ups van on-premises bronnen of met behulp van Azure analytics voor inzicht in on-premises werkbelastingen. 

Als onderdeel van de hybride-cloudstrategie biedt Azure steeds meer oplossingen voor het migreren van on-premises toepassingen en workloads naar de cloud. Met eenvoudige stappen kunt u uw on-premises resources uitgebreid evalueren om te bepalen hoe ze in de Azure-cloud zullen werken. Wanneer u over een grondige evaluatie beschikt, kunt u resources met een gerust hart migreren naar Azure. Wanneer resources in Azure werken, kunt u ze optimaliseren om toegang, flexibiliteit, beveiliging en betrouwbaarheid te behouden en te verbeteren.

## <a name="migration-strategies"></a>Migratiestrategieën

Strategieën voor migratie naar de cloud kunnen worden onderverdeeld in vier hoofdcategorieën: opnieuw hosten, herstructureren, opnieuw modelleren of opnieuw opbouwen. De strategie die u wilt vaststellen, is afhankelijk van uw zakelijke stimulansen en doelstellingen van de migratie. U kunt meerdere strategieën vast. U kan bijvoorbeeld kiezen om te opnieuw hosten (lift-and-shift) eenvoudig apps of apps die niet essentieel voor uw bedrijf, maar die meer complexe en kritieke zakelijke zijn opnieuw modelleren. We bekijken de strategieën.


**Strategie** | **Definitie** | **Wanneer u** 
--- | --- | --- 
**Opnieuw hosten** | Vaak aangeduid als een 'lift-and-shift'-migratie. Deze optie vereist geen code hoeft te wijzigen en laten we u uw bestaande apps migreren naar Azure snel. Elke app is gemigreerd is, naar de vruchten plukken van de cloud, zonder het risico en de kosten die gepaard gaan met de code hoeft te wijzigen. | Wanneer u moet apps snel naar de cloud verplaatsen.<br/><br/> Als u wilt verplaatsen van een app zonder het te wijzigen.<br/><br/> Als uw apps zijn zodanig ontworpen dat ze kunnen gebruikmaken van [Azure IaaS](https://azure.microsoft.com/overview/what-is-iaas/) schaalbaarheid na de migratie.<br/><br/> Wanneer apps zijn belangrijk voor uw bedrijf, maar u hoeft geen directe wijzigingen in de app-mogelijkheden.
**Herstructureren** | Vaak aangeduid als "opnieuw verpakken", herstructurering vereist, is de minimale wijzigingen in apps, zodat ze verbinding met maken kunnen [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas/), en gebruik van cloudaanbiedingen.<br/><br/> Bijvoorbeeld, kon u bestaande apps migreren naar Azure App Service of Azure Kubernetes Service (AKS).<br/><br/> Of u relationele en niet-relationele databases kan herstructureren in opties, zoals Azure SQL Database Managed Instance, Azure Database for MySQL, Azure Database voor PostgreSQL en Azure Cosmos DB. | Als uw app kan eenvoudig worden gebruikt om te werken in Azure.<br/><br/> Als u wilt toepassen innovatieve DevOps-praktijken die door Azure, of u erover denkt over DevOps met behulp van een strategie voor een container voor werkbelastingen.<br/><br/> Voor de herstructurering, moet u om na te denken over de draagbaarheid van uw bestaande codebasis en vaardigheden in webontwikkeling beschikbaar.
**Opnieuw modelleren** | Opnieuw voor de migratie is gericht op aanpassen en uitbreiden van app-functionaliteit en de codebasis het optimaliseren van de app-architectuur voor cloudschaalbaarheid van de.<br/><br/> U kunt bijvoorbeeld een monolithische toepassing in een groep van microservices die samenwerken en schaal eenvoudig opsplitsen.<br/><br/> Of u kunt opnieuw modelleren relationele en niet-relationele databases naar een volledig beheerde DBaaS-oplossingen, zoals Azure SQL Database Managed Instance, Azure Database for MySQL, Azure Database for PostgreSQL en Azure Cosmos DB. | Wanneer belangrijke wijzigingen op te nemen van nieuwe mogelijkheden en efficiënt werken op een platform nodig hebt in uw apps.<br/><br/> Als u gebruiken van bestaande investeringen in de toepassing wilt, voldoen aan de vereisten voor schaalbaarheid, innovatieve Azure DevOps-praktijken toepassen en zo het gebruik van virtuele machines.
**Opnieuw maken** | Opnieuw opbouwen duurt dingen een stap verder door het opnieuw opbouwen van een app maken met Azure-cloud-technologieën.<br/><br/> U kunt bijvoorbeeld groen veld apps met cloud-eigen technologieën zoals Azure Functions, Azure AI, Azure SQL Database Managed Instance en Azure Cosmos DB bouwen. | Als u wilt dat snelle ontwikkeling en bestaande apps hebben beperkte functionaliteit en levensduur.<br/><br/> Wanneer u bent klaar om te versnellen van innovatie van zakelijke (met inbegrip van DevOps-praktijken verstrekt door Azure), nieuwe toepassingen met behulp van technologieën voor cloudtoepassingen te bouwen en te profiteren van verbeteringen in AI, Blockchains en IoT.

## <a name="migration-articles"></a>Migratieartikelen

De artikelen in de reeks worden samengevat in de onderstaande tabel.  

- Elke migratiescenario wordt aangedreven door enigszins zakelijke doelstellingen bepalen van de strategie voor clientmigratie.
- Voor elk implementatiescenario bieden we informatie over zakelijke stimulansen en doelstellingen, een voorgestelde architectuur, stappen om uit te voeren van de migratie en aanbevelingen voor het opschonen en de volgende stappen nadat de migratie is voltooid.

**Artikel** | **Details** | **Status**
--- | --- | ---
Artikel 1: overzicht | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | In dit artikel
[Artikel 2: Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar   
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Contoso migreert de VM's van de SmartHotel360-app naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbare [artikel 5: een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbare [artikel 7: een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso is een lift-and-shift-migratie van de Linux-osTicket-app naar Azure-VM's, met behulp van de Site Recovery-service voltooid. | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure Database for MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migreert de Linux-osTicket-app naar Azure-VM's met behulp van Site Recovery. Deze migreert de app-database naar Azure Database for MySQL via MySQL Workbench. | Beschikbaar
[Artikel 9: Een app in een Azure-web-app en Azure SQL Database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso de SmartHotel360-app migreert naar een Azure-web-app en de app-database migreert naar een Azure SQL Server-exemplaar met de Database Migration Assistant. | Beschikbaar    
[Artikel 10: Een Linux-app in een Azure-web-app en de Azure Database for MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren Team Foundation Server op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app in Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | Beschikbaar    
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar 


In dit artikel die Contoso u alle Infrastructuurelementen stelt moet alle scenario's voor migratie te voltooien. 







### <a name="demo-apps"></a>Demo-apps

De artikelen twee demo-apps - SmartHotel360 en osTicket gebruiken.

- **SmartHotel360**: deze app is ontwikkeld door Microsoft als een test-app die u gebruiken kunt bij het werken met Azure. Dit wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360). Het is een ASP.NET-app die is verbonden met een SQL Server-database. De app is momenteel in twee virtuele VMware-machines waarop Windows Server 2008 R2 en SQL Server 2008 R2 wordt uitgevoerd. De app virtuele machines zijn on-premises gehost en beheerd door vCenter Server.
- **osTicket**: een open source service voor helpdesk-tickets van de app die wordt uitgevoerd op Linux. U kunt dit ook downloaden via [GitHub](https://github.com/osTicket/osTicket). Huidige de app zich op twee virtuele VMware-machines waarop Ubuntu 16.04 LTS wordt uitgevoerd, met behulp van Apache 2, PHP 7.0 en MySQL 5.7
    

## <a name="next-steps"></a>Volgende stappen

[Informatie over hoe](contoso-migration-infrastructure.md) Contoso stelt u een on-premises en Azure-infrastructuur voorbereiden voor migratie.



