<properties
    pageTitle="Prestaties en opties van SQL Database: servicelagen | Microsoft Azure"
    description="Vergelijk de prestatie- en bedrijfscontinuïteitsfuncties op de verschillende servicelagen van SQL Database met balanskosten en capaciteit terwijl u schaalt."
    keywords="databaseopties, prestaties van de database"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>


# Opties en prestaties van SQL Database: inzicht in wat er beschikbaar is in elke servicelaag

[Azure SQL Database](sql-database-technical-overview.md) biedt drie servicelagen met meerdere prestatieniveaus om verschillende workloads af te handelen. Elke prestatieniveau biedt meer resources die zijn ontworpen om een steeds snellere doorvoer te leveren. U kunt elke database in een eigen [servicelaag](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) met een eigen prestatieniveau beheren. U kunt ook meerdere databases in een [elastische pool](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) met een gemeenschappelijke set bronnen beheren. De resources voor zelfstandige databases worden DTU's genoemd (Database Transaction Units) en de resources voor elastische pools worden elastische DTU's of eDTU's genoemd. Zie [Wat is een DTU?](sql-database-what-is-a-dtu.md) voor meer informatie over DTU's en eDTU's. 

In beide gevallen is er een servicelaag **Basic**, **Standard** en **Premium**. Databaseopties in deze lagen zijn vergelijkbaar voor individuele databases en elastische pools, maar voor elastische pools zijn er aanvullende overwegingen. In dit artikel vindt u details over servicelagen voor individuele databases en elastische pools.

## Servicelagen en database-opties
Basic-, Standard- en Premium-servicelagen hebben allemaal een SLA van 99,99% beschikbaarheid en bieden voorspelbare prestaties, flexibele opties voor bedrijfscontinuïteit, beveiligingsfuncties en facturering per uur. In de volgende tabel vindt u voorbeelden van de lagen die het meest geschikt zijn voor verschillende toepassingsworkloads.

| Servicelaag | Beoogde workloads |
|---|---|
| **Basic** | Het meest geschikt voor een kleine database, die doorgaans één actieve bewerking tegelijk ondersteunt. Voorbeelden zijn onder meer databases die worden gebruikt voor ontwikkeling of testen, of kleinschalige, onregelmatig gebruikte toepassingen. |
| **Standard** | De optie die wordt gebruikt door de meeste cloudtoepassingen, met ondersteuning voor meerdere gelijktijdige query's. Voorbeelden zijn onder meer werkgroep- of webtoepassingen. |
| **Premium** | Ontworpen voor toepassingen met veel transacties, die veel gelijktijdige gebruikers ondersteunen en optimale mogelijkheden voor bedrijfscontinuïteit vereisen. Voorbeelden zijn databases die bedrijfskritieke toepassingen ondersteunen. |

>[AZURE.NOTE] De Web- en Business-edities worden niet meer gebruikt. Lees de [Veelgestelde vragen over Sunset](https://azure.microsoft.com/pricing/details/sql-database/web-business/) als u de Web- of Business-editie wilt blijven gebruiken.

## Servicelagen en prestatieniveaus van individuele databases
Voor individuele databases zijn er binnen elke servicelaag meerdere prestatieniveaus. U hebt de flexibiliteit om het niveau te kiezen dat het best voldoet aan de eisen van uw workload. Als u omhoog of omlaag wilt schalen, kunt u de lagen van de database eenvoudig wijzigen. Zie [Servicelagen en prestatieniveaus van databases wijzigen](sql-database-scale-up.md) voor meer informatie.

De hieronder vermelde prestatiekenmerken zijn van toepassing op databases die zijn gemaakt met [SQL Database V12](sql-database-v12-whats-new.md). Ongeacht het aantal gehoste databases, krijgt de database steeds een gegarandeerd aantal resources en heeft dit geen invloed op de verwachte prestatiekenmerken van de database.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Voor een gedetailleerde uitleg van de overige rijen in deze tabel met servicelagen raadpleegt u [Mogelijkheden en beperkingen van servicelagen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## Servicelagen en prestaties in eDTU's van elastische pools
Behalve dat u een individuele database kunt maken en schalen, hebt u ook de mogelijkheid om meerdere databases te beheren in een [elastische pool](sql-database-elastic-pool.md). Alle databases in een elastische pool delen een gemeenschappelijke set bronnen. De prestatiekenmerken worden gemeten met behulp van *eDTU's* (elastic Database Transaction Units). Net als bij individuele databases zijn er drie soorten servicelagen: **Basic**, **Standard** en **Premium**. Voor pools bepalen deze drie servicelagen nog steeds de algehele prestatielimieten en een aantal functies.

Dankzij pools kunnen databases DTU-resources delen en verbruiken zonder dat er aan elke database in de pool een specifiek prestatieniveau hoeft te worden toegewezen. Zo kan een individuele database in een Standard-pool van 0 eDTU's tot het maximumaantal eDTU's voor de database gaan dat u hebt ingesteld bij de configuratie van de pool. Zo kunnen meerdere databases met verschillende workloads efficiënt gebruikmaken van de eDTU-resources die voor de hele pool beschikbaar zijn. Zie [Prijs- en prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool-guidance.md) voor meer informatie.

In de volgende tabel vindt u een beschrijving van de kenmerken van de servicelagen van de pool.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Elke database in een pool heeft dezelfde kenmerken als die van een individuele database voor die laag. Zo heeft de Basic-pool een limiet voor het maximumaantal sessies per groep van 4800 tot 28800, maar één database binnen die Basic-pool heeft een databaselimiet van 300 sessies.

## Het kiezen van een servicelaag

Om te beslissen over een servicelaag, begint u met te bepalen of de database een individuele database of onderdeel van een elastische pool is. 

### Een servicelaag kiezen voor een individuele database

Om te beslissen over een servicelaag voor een individuele database, bepaalt u eerst de databasefuncties die u nodig hebt om uw SQL Database-editie te bepalen:

- Grootte van de database (2 GB voor Basic, maximaal 250 GB voor Standard en 500 GB tot 1 TB maximum voor Premium, afhankelijk van het prestatieniveau)
- Retentieperiode van de databaseback-up (7 dagen voor Basic, 35 dagen voor Standard en 35 dagen voor Premium)

Na bepaling van de SQL Database-editie, bent u klaar om het prestatieniveau van de database te bepalen (het aantal DTU’s). U kunt gokken en vervolgens dynamisch [omhoog of omlaag schalen](sql-database-scale-up.md) op basis van de werkelijke ervaring. U kunt ook de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om het aantal nodige DTU’s te benaderen. 

### Een servicelaag kiezen voor een pool voor Elastic Database.

Om te beslissen over een servicelaag voor een pool voor Elastic Database, bepaalt u eerst de databasefuncties die u nodig hebt om de servicelaag voor uw pool te kiezen.

- Grootte van de database (2 GB voor de basisfunctionaliteit, 250 GB voor Standard en 500 GB voor Premium)
- Retentieperiode van de databaseback-up (7 dagen voor Basic, 35 dagen voor Standard en 35 dagen voor Premium)
- Aantal databases per pool (400 voor Basic 400 voor Standard en 50 voor Premium)
- Maximale opslag per pool (117 GB voor Basic, 1200 voor Standard en 750 voor Premium)

Als u de servicelaag hebt vastgesteld voor uw pool, bent u klaar om te bepalen van het prestatieniveau voor de pool (eDTUs). U kunt gokken en vervolgens dynamisch [omhoog of omlaag schalen](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) op basis van de werkelijke ervaring. U kunt de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om het aantal DTU’s nodig voor elke database in een pool te schatten om de bovendrempel voor de pool te helpen bepalen.

## Volgende stappen
- Meer informatie over prijzen voor deze lagen vindt u in [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Lees meer over [Pools voor elastische pools](sql-database-elastic-pool-guidance.md) en [Prijs- en prestatieoverwegingen voor elastische pools](sql-database-elastic-pool-guidance.md).
- Lees [Elastische pools controleren, beheren en van formaat veranderen](sql-database-elastic-pool-manage-portal.md) en [De prestaties van individuele databases bewaken](sql-database-single-database-monitor.md).
- Nu u bekend bent met de lagen van SQL Database, kunt u ze uitproberen met een [gratis account](https://azure.microsoft.com/pricing/free-trial/) en leren [hoe u uw eerste SQL-database maakt](sql-database-get-started.md).

## Aanvullende bronnen

- [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Videocursus van Microsoft Virtual Academy over elastische databasemogelijkheden in Azure SQL Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)



<!--HONumber=Oct16_HO3-->


