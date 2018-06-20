---
title: Overzicht van de Contoso-migratie naar Azure | Microsoft Docs
description: Biedt een overzicht van de strategie voor clientmigratie en scenario's die worden gebruikt door Contoso naar hun on-premises datacentrum migreren naar Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: ec0308bb2e39c3801305748f19783e70d58b0b7e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232392"
---
# <a name="contoso-migration-overview"></a>Migratie van Contoso: overzicht


Dit is het eerste artikel in een serie die laat zien hoe de fictieve organisatie Contoso migreert hun on-premises infrastructuur voor de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) cloud. 

De reeks artikel biedt Voorbeeldimplementaties die u helpen te begrijpen en andere opties voor migratie naar Azure, en voor het gebruik van een hybride omgeving uitproberen. Artikelen aangeven hoe door het bedrijf Contoso de missie migratie is voltooid, maar aanwijzers voor algemene lezen en specifieke instructies vindt u in de gehele.

## <a name="introduction"></a>Inleiding

Azure biedt toegang tot een uitgebreide set van cloudservices. Als ontwikkelaars en IT-professionals, kunt u deze services te maken, implementeren en beheren van toepassingen op tal van hulpprogramma's en frameworks, via een wereldwijd netwerk van datacenters. Als uw bedrijf de digitale verschuiving een grote uitdaging vindt, kan de Azure-cloud u helpen om uit te zoeken hoe u resources en werkzaamheden optimaliseert, hoe u dit aanpakt met uw klanten en werknemers en hoe u uw producten transformeert.

Ondanks alle voordelen die de cloud biedt op het gebied van snelheid en flexibiliteit, minimale kosten, hoge prestaties en betrouwbaarheid, beseft Azure dat veel organisaties nog enige tijd met on-premises datacenters moeten werken. Om de barrières voor acceptatie van de cloud te overkomen, biedt Azure een hybride cloudstrategie die een brug slaat tussen uw on-premises datacenters en de openbare Azure-cloud. Bijvoorbeeld, het gebruik van Azure-cloudresources, zoals back-ups om on-premises resources te beschermen, of het gebruik van Azure-analysen om inzicht te krijgen in on-premises workloads. 

Als onderdeel van de strategie van hybride cloud biedt Azure groeiende oplossingen voor migratie lokale toepassingen en werkbelastingen naar de cloud. Met eenvoudige stappen kunt u uw on-premises resources uitgebreid evalueren om te bepalen hoe ze in de Azure-cloud zullen werken. Wanneer u over een grondige evaluatie beschikt, kunt u resources met een gerust hart migreren naar Azure. Wanneer resources in Azure werken, kunt u ze optimaliseren om toegang, flexibiliteit, beveiliging en betrouwbaarheid te behouden en te verbeteren.

## <a name="migration-strategies"></a>Migratiestrategieën

Strategieën voor migratie naar de cloud kunnen worden onderverdeeld in vier hoofdcategorieën: rehost, opsplitsen, rearchitect of opnieuw opbouwen. De strategie die u wilt vaststellen, is afhankelijk van uw zakelijke drijfveren en doelstellingen van de migratie. U kunt meerdere strategieën vast. U kan bijvoorbeeld rehost (lift-en-shift) eenvoudige apps of apps die niet essentieel zijn voor uw bedrijf wilt, maar die wel een complexere en bedrijfskritieke rearchitect. Bekijk de strategieën.


**Een strategie voor** | **Definitie** | **Wanneer gebruiken** 
--- | --- | --- 
**Rehost** | Vaak aangeduid als een 'lift-en-shift'-migratie. Deze optie niet codewijzigingen nodig en Hiermee kunt u uw bestaande apps migreren naar Azure snel. Elke app is gemigreerd zoals is te profiteren van de cloud, zonder de risico's en kosten in verband met codewijzigingen. | Wanneer u moet apps snel verplaatsen naar de cloud.<br/><br/> Als u wilt verplaatsen van een app zonder het te wijzigen.<br/><br/> Wanneer uw apps zijn ontworpen zodat ze gebruikmaken van kunnen [Azure IaaS](https://azure.microsoft.com/overview/what-is-iaas/) schaalbaarheid na de migratie.<br/><br/> Wanneer apps zijn belangrijk in uw bedrijf, maar u hoeft niet direct wijzigingen in de app-mogelijkheden.
**Opsplitsen** | Vaak genoemd 'opnieuw inpakken', refactoring vereist, is de minimale wijzigingen in apps, zodat ze verbinding met maken kunnen [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas/), en de offerings cloud te gebruiken.<br/><br/> U kan bijvoorbeeld bestaande apps migreren naar Azure App Service- of Azure Kubernetes Service (AKS).<br/><br/> Of u relationele en niet-relationele databases kan opsplitsen in opties zoals Azure SQL Database-beheerd instantie, Azure-Database voor MySQL, Azure-Database voor PostgreSQL en Azure Cosmos DB. | Als uw app kan eenvoudig worden gebruikt om te kunnen werken in Azure.<br/><br/> Als u wilt toepassen innovatieve DevOps-procedures die door Azure, of u overweegt DevOps met behulp van een strategie voor een container voor werkbelastingen.<br/><br/> Voor het refactoring, moet u nadenken over de draagbaarheid van uw bestaande codebasis en ontwikkelingsvaardigheden beschikbaar.
**Rearchitect** | Opnieuw vormgeven van voor de migratie is gericht op aanpassen en uitbreiden van app-functionaliteit en codebasis optimaliseren van de app-architectuur voor cloudschaalbaarheid.<br/><br/> Bijvoorbeeld, u een monolithisch toepassing in een groep microservices die samenwerken en eenvoudig worden geschaald uitgesplitst.<br/><br/> Of u kan rearchitect relationele en niet-relationele databases DBaaS oplossingen, zoals Azure SQL Database-beheerd instantie, Azure-Database voor MySQL, Azure-Database voor PostgreSQL en Azure Cosmos DB volledig beheerd. | Wanneer uw apps nodig hebt van belangrijke wijzigingen op te nemen van nieuwe mogelijkheden of efficiënt werken op een cloudplatform.<br/><br/> Wanneer u gebruiken van bestaande investeringen in de toepassing wilt, voldoen aan de vereisten voor schaalbaarheid, innovatieve Azure DevOps praktijken passen en zo het gebruik van virtuele machines.
**Opnieuw samenstellen** | Opnieuw samenstellen vergt dingen een stap verder door het opnieuw opbouwen van een app maken met behulp van Azure-cloud-technologieën.<br/><br/> U kan bijvoorbeeld groen veld apps met cloud-systeemeigen technologieën zoals zonder server, AI Azure, Azure SQL Database-beheerd instantie en Azure Cosmos DB samenstellen. | Wanneer u wilt dat snelle ontwikkeling en bestaande apps hebben beperkte functionaliteit en levensduur.<br/><br/> Wanneer u klaar bent voor snellere business innovatie (inclusief DevOps-procedures die zijn verstrekt door Azure), nieuwe toepassingen bouwen met cloud-systeemeigen technologieën en te profiteren van verbeteringen in AI, blockchain en IoT.

## <a name="migration-articles"></a>Migratie-artikelen

De artikelen die gezamenlijk de migratie van de reeks Contoso worden samengevat in de onderstaande tabel.  

- De scenario's van het verhogen van de complexiteit en we toevoegt aan deze gedurende een bepaalde periode.
- Elk migratiescenario wordt aangedreven door iets anders zakelijke doelstellingen die bepalen van de strategie voor clientmigratie.
- We bieden informatie over zakelijke drijfveren en doelstellingen, een voorgestelde architectuur, stappen voor het uitvoeren van de migratie en aanbeveling voor opruimen en de volgende stappen nadat de migratie is voltooid voor elk implementatiescenario.

**Artikel** | **Details** | **Status**
--- | --- | ---
Artikel 1: Overzicht (in dit artikel) | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps gebruikt. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-overview.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: Beoordelen lokale bronnen](contoso-migration-assessment.md) | Toont hoe een beoordeling van de lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Evalueren van de virtuele machines van een app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Azure Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Rehost virtuele machines in Azure en een beheerde SQL-exemplaar](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstreert hoe Contoso de app SmartHotel migreert naar Azure. Migreren van de app web VM via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database in een SQL-exemplaar worden beheerd met de [Azure databasemigratie](https://docs.microsoft.com/azure/dms/dms-overview) service. | Beschikbaar
[Artikel 5: Rehost voor virtuele machines in Azure](contoso-migration-rehost-vm.md) | Toont hoe de hun app SmartHotel virtuele machines met de Site Recovery-service voor het migreren van Contoso.
[Artikel 6: Rehost virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van de VM-app en de migratie van de Database-service om te migreren van de app-database naar een SQL Server AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een app Linux virtuele machines in Azure Rehost](contoso-migration-rehost-linux-vm.md) | Demonstreert hoe Contoso de app virtuele machines migreert naar Azure met Site Recovery. | Gepland
[Artikel 8: Rehost een Linux-app voor virtuele machines in Azure en Azure MySQL-Server](contoso-migration-rehost-linux-vm-mysql.md) | Laat zien hoe de app-VM met Site Recovery worden gemigreerd in Contoso en maakt gebruik van MySQL-Workbench om te migreren naar een Azure MySQL Server-exemplaar. | Gepland



### <a name="demo-apps"></a>Demo-apps

De artikelen twee demo-apps - SmartHotel en osTicket gebruiken.

- **SmartHotel360**: deze app is ontwikkeld door Microsoft als een test-app die u gebruiken kunt bij het werken met Azure. Als de open-source opgegeven en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360). Het is een ASP.NET-app die is verbonden met een SQL Server-database. De app is momenteel op twee virtuele VMware-machines met Windows Server 2008 R2 en SQL Server 2008 R2. De app VMs gehoste on-premises en beheerd door de vCenter-Server.
- **osTicket**: een open source-service voor helpdesk-tickets app die wordt uitgevoerd op Linux. U kunt downloaden van [GitHub](https://github.com/osTicket/osTicket). Huidige de app is op twee virtuele VMware-machines uitgevoerd Ubuntu 16.04LTS, met Apache 2, PHP 7.0 en MySQL 5.7
    

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over hoe](contoso-migration-infrastructure.md) Contoso stelt hun on-premises en Azure-infrastructuur bij het voorbereiden voor migratie.



