---
title: 'Prestaties van SQL Database: servicelagen | Microsoft Docs'
description: Vergelijk SQL Database-servicelagen.
keywords: databaseopties, prestaties van de database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/15/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e8bb9e5a02a7caf95dae0101c720abac1c2deff3
ms.openlocfilehash: 7bbdbe345bd468c01e2a790610bcf6c063c11f9b


---
# <a name="sql-database-service-tiers-for-single-databases-and-elastic-database-pools"></a>SQL Database-servicelagen voor enkele databases en pools voor elastische databases
[Azure SQL Database](sql-database-technical-overview.md) biedt drie servicelagen met meerdere prestatieniveaus om verschillende workloads af te handelen. Hogere prestatieniveaus bieden grotere sets van resources die zijn ontworpen om een steeds snellere doorvoer te leveren. U kunt servicelagen en prestatieniveaus dynamisch wijzigen. Zie [Servicelagen en prestatieniveaus van databases wijzigen](sql-database-scale-up.md) voor meer informatie.

U kunt elke database in een eigen [servicelaag](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) met een eigen prestatieniveau beheren. U kunt ook meerdere databases in een [pool voor elastische databases](sql-database-service-tiers.md#elastic-database-pool-service-tiers-and-performance-in-edtus) met een gemeenschappelijke set resources beheren. De resources voor enkele databases worden DTU's (Database Transaction Units) genoemd. De pools voor elastische databases worden elastische DTU's of eDTU's genoemd. Zie [Wat is een DTU?](sql-database-what-is-a-dtu.md) voor meer informatie over DTU's en eDTU's. 

In beide gevallen is er een servicelaag **Basic**, **Standard** en **Premium**. 

## <a name="service-tiers"></a>Servicelagen
Basic-, Standard- en Premium-servicelagen hebben allemaal een SLA van 99,99% beschikbaarheid en bieden voorspelbare prestaties, flexibele opties voor bedrijfscontinuïteit, beveiligingsfuncties en facturering per uur. In de volgende tabel vindt u voorbeelden van de lagen die het meest geschikt zijn voor verschillende toepassingsworkloads.

| Servicelaag | Beoogde workloads |
| :--- | --- |
| **Basic** |Het meest geschikt voor een kleine database, die doorgaans één actieve bewerking tegelijk ondersteunt. Voorbeelden zijn onder meer databases die worden gebruikt voor ontwikkeling of testen, of kleinschalige, onregelmatig gebruikte toepassingen. |
| **Standard** |De optie die wordt gebruikt door de meeste cloudtoepassingen, met ondersteuning voor meerdere gelijktijdige query's. Voorbeelden zijn onder meer werkgroep- of webtoepassingen. |
| **Premium** |Ontworpen voor toepassingen met veel transacties, die veel gelijktijdige gebruikers ondersteunen en optimale mogelijkheden voor bedrijfscontinuïteit vereisen. Voorbeelden zijn databases die bedrijfskritieke toepassingen ondersteunen. |

> [!NOTE]
> De Web- en Business-edities worden niet meer gebruikt. Lees de [Veelgestelde vragen over Sunset](https://azure.microsoft.com/pricing/details/sql-database/web-business/) als u de Web- of Business-editie wilt blijven gebruiken.
> 
> 

## <a name="single-database-service-tiers-and-performance-levels"></a>Servicelagen en prestatieniveaus van afzonderlijke databases
Voor enkele databases zijn er binnen elke servicelaag meerdere prestatieniveaus. U hebt de flexibiliteit om het niveau te kiezen dat het best voldoet aan de eisen van uw workload. Als u omhoog of omlaag wilt schalen, kunt u snel het prestatieniveau van de database wijzigen. Zie [Servicelagen en prestatieniveaus van databases wijzigen](sql-database-scale-up.md) voor meer informatie.

De hieronder vermelde prestatiekenmerken zijn van toepassing op databases die zijn gemaakt met [SQL Database V12](sql-database-technical-overview.md). Ongeacht het aantal gehoste databases, krijgt de database steeds een gegarandeerd aantal resources en heeft dit geen invloed op de verwachte prestatiekenmerken van de database.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Voor een gedetailleerde uitleg van de overige rijen in deze tabel met servicelagen raadpleegt u [Mogelijkheden en beperkingen van servicelagen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 
> 

## <a name="elastic-database-pool-service-tiers-and-performance-in-edtus"></a>Servicelagen van pools voor elastische databases en prestaties in eDTU's
U kunt ook meerdere databases beheren binnen een [pool voor elastische databases](sql-database-elastic-pool.md). Al de databases in een pool voor elastische databases hebben een gemeenschappelijke set resources. De prestatiekenmerken worden gemeten met behulp van *eDTU's* (elastic Database Transaction Units). Net als bij afzonderlijke databases zijn er drie soorten servicelagen: **Basic**, **Standard** en **Premium**. Voor pools bepalen deze drie servicelagen nog steeds de algehele prestatielimieten en een aantal functies.

Dankzij pools kunnen databases DTU-resources delen en verbruiken zonder dat er aan elke database in de pool een specifiek prestatieniveau hoeft te worden toegewezen. Een database in een Standard-pool kan bijvoorbeeld tussen de 0 eDTU's en het maximumaantal eDTU's dat u hebt ingesteld bij de configuratie van de pool, gebruiken voor de database. Zo kunnen meerdere databases met verschillende workloads efficiënt gebruikmaken van de eDTU-resources die voor de hele pool beschikbaar zijn. Zie [Price and performance considerations for an elastic database pool](sql-database-elastic-pool-guidance.md) (Prijs- en prestatieoverwegingen voor een pool voor elastische databases) voor details.

De volgende tabel beschrijft de kenmerken van de Basic-, Standard- en Premium-pools voor elastische databases.

[!INCLUDE [SQL DB service tiers table for elastic database pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Elke database in een pool heeft dezelfde kenmerken als die van een enkele database voor deze laag. Zo heeft de Basic-pool een limiet voor het maximumaantal sessies per groep van 4800 tot 28800, maar één database binnen die Basic-pool heeft een databaselimiet van 300 sessies.

## <a name="choosing-a-service-tier"></a>Het kiezen van een servicelaag
Bepaal eerst of de database een enkele database of onderdeel van een pool van elastische databases moet zijn om een servicelaag te kiezen. 

### <a name="choosing-a-service-tier-for-a-single-database"></a>Een servicelaag kiezen voor een enkele database
Bepaal eerst de databasefuncties die u nodig hebt om uw SQL Database-editie te kiezen, om te beslissen welke servicelaag voor een enkele database u wilt:

* Grootte van de database (2 GB voor Basic, maximaal 250 GB voor Standard en 500 GB tot 1 TB maximum voor Premium, afhankelijk van het prestatieniveau)
* Retentieperiode van de databaseback-up (7 dagen voor Basic, 35 dagen voor Standard en 35 dagen voor Premium)

Na bepaling van de SQL Database-editie, bent u klaar om het prestatieniveau van de database te bepalen (het aantal DTU’s). U kunt gokken en vervolgens dynamisch [omhoog of omlaag schalen](sql-database-scale-up.md) op basis van de werkelijke ervaring. U kunt ook de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om het aantal nodige DTU’s te benaderen. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Een servicelaag kiezen voor een pool voor Elastic Database.
Om te beslissen over een servicelaag voor een pool voor Elastic Database, bepaalt u eerst de databasefuncties die u nodig hebt om de servicelaag voor uw pool te kiezen.

* Grootte van de database (2 GB voor de basisfunctionaliteit, 250 GB voor Standard en 500 GB voor Premium)
* Retentieperiode van de databaseback-up (7 dagen voor Basic, 35 dagen voor Standard en 35 dagen voor Premium)
* Aantal databases per pool (400 voor Basic 400 voor Standard en 50 voor Premium)
* Maximale opslag per pool (117 GB voor Basic, 1200 voor Standard en 750 voor Premium)

Als u de servicelaag hebt vastgesteld voor uw pool, bent u klaar om te bepalen van het prestatieniveau voor de pool (eDTUs). U kunt gokken en vervolgens dynamisch [omhoog of omlaag schalen](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) op basis van de werkelijke ervaring. U kunt de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om het aantal DTU’s nodig voor elke database in een pool te schatten om de bovendrempel voor de pool te helpen bepalen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over prijzen voor deze lagen vindt u in [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* Lees meer over [Pools voor elastische databases](sql-database-elastic-pool-guidance.md) en [Prijs- en prestatieoverwegingen voor pools voor elastische databases](sql-database-elastic-pool-guidance.md).
* Lees hoe u [pools voor elastische databases kunt controleren, beheren en de grootte ervan kunt wijzigen](sql-database-elastic-pool-manage-portal.md) en hoe u [de prestaties van afzonderlijke databases kunt bewaken](sql-database-single-database-monitor.md).
* Nu u bekend bent met de lagen van SQL Database, kunt u ze uitproberen met een [gratis account](https://azure.microsoft.com/pricing/free-trial/) en leren [hoe u uw eerste SQL-database maakt](sql-database-get-started.md).

## <a name="additional-resources"></a>Aanvullende bronnen
* [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Videocursus van Microsoft Virtual Academy over elastische databasemogelijkheden in Azure SQL Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)




<!--HONumber=Nov16_HO4-->


