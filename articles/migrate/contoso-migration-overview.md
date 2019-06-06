---
title: Contoso migratie reeks | Microsoft Docs
description: Biedt een overzicht van de strategie voor clientmigratie en scenario's die worden gebruikt door Contoso hun on-premises datacentrum migreren naar Azure.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: raynew
ms.openlocfilehash: 048772edadca36a63870a2965c703ca7e6ec8c63
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729888"
---
# <a name="contoso-migration-series"></a>Migratieserie Contoso


We hebben een reeks artikelen die u laat zien hoe het fictieve organisatie Contoso migreert on-premises infrastructuur naar de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) cloud. 

De reeks bevat informatie en scenario's die laten zien hoe u een migratie van de infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit navigatie. De artikelen wordt beschreven hoe in het bedrijf Contoso de missie van de migratie is voltooid, maar aanwijzers voor algemene lezen en specifieke instructies vindt u in de gehele.

## <a name="migration-articles"></a>Migratieartikelen

De artikelen in de reeks worden samengevat in de onderstaande tabel.  

- Elke migratiescenario wordt aangedreven door enigszins zakelijke doelstellingen bepalen van de strategie voor clientmigratie.
- Voor elk implementatiescenario bieden we informatie over zakelijke stimulansen en doelstellingen, een voorgestelde architectuur, stappen om uit te voeren van de migratie en aanbevelingen voor het opschonen en de volgende stappen nadat de migratie is voltooid.

**Artikel** | **Details** 
--- | --- 
[Artikel 1: Overzicht](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. 
[Artikel 2: Azure-infrastructuur implementeren](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. 
[Artikel 3: On-premises resources voor migratie naar Azure evalueren](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-assessment)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant.
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview).
[Artikel 5: Een app op Azure VM's opnieuw hosten](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso migreert de VM's van de SmartHotel360-app naar Azure-VM's met behulp van de Site Recovery-service. 
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep rehost](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) |Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. 
[7-artikel: Een Linux-app op Azure VM's opnieuw hosten](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso is een lift-and-shift-migratie van de Linux-osTicket-app naar Azure-VM's, met behulp van de Site Recovery-service voltooid.
[8-artikel: Een Linux-app op Azure VM's en Azure Database for MySQL rehost](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso migreert de Linux-osTicket-app naar Azure-VM's met behulp van Site Recovery. Deze migreert de app-database naar Azure Database for MySQL via MySQL Workbench. 
[9-artikel: Een app in een Azure-web-app en Azure SQL Database herstructureren](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso de SmartHotel360-app migreert naar een Azure-web-app en de app-database migreert naar een Azure SQL Server-exemplaar met de Database Migration Assistant.     
[Artikel 10: Een Linux-app in een Azure-web-app en de Azure Database for MySQL herstructureren](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. 
[11-artikel: Herstructureren van Team Foundation Server op Azure DevOps-Services](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure.
[12-artikel: Een app in Azure-containers en Azure SQL Database opnieuw modelleren](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rearchitect-container-sql) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. 
[13-artikel: Opnieuw opbouwen van een app in Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB.  
[14-artikel: Schalen van een migratie naar Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Na het proberen van migratie combinaties, bereidt Contoso worden uitgebreid naar een volledige migratie naar Azure. 


    

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/) cloudmigratie. 

