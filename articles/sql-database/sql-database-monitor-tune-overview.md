---
title: Bewaking en het afstemmen van prestaties - Azure SQL Database | Microsoft Docs
description: Tips voor het afstemmen in Azure SQL Database via evaluatie en verbetering van de prestaties.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 10/22/2018
ms.openlocfilehash: 60f60b9ef055ea38a2036c4f9c5b6aa2c1f6526d
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648051"
---
# <a name="monitoring-and-performance-tuning"></a>Bewaking en prestatieafstemming

Azure SQL-Database wordt automatisch beheerd en flexibele gegevensservice waar u kunt eenvoudig gebruik te bewaken, toevoegen of verwijderen resources (CPU, geheugen, i/o), aanbevelingen die kunnen verbeteren de prestaties van uw database of database aan te passen aan uw workload kunt vinden en automatisch optimaliseren.

## <a name="monitoring-database-performance"></a>Databaseprestaties bewaken

Het bewaken van de prestaties van een SQL-database in Azure begint met het bewaken van het resourcegebruik ten opzichte van het gekozen niveau van databaseprestaties. Azure SQL Database kunt u voor het identificeren van kansen te verbeteren en optimaliseren van prestaties van query's zonder te hoeven wijzigen van resources aan de hand [aanbevelingen voor afstemming](sql-database-advisor.md). Ontbrekende indexen en onvoldoende geoptimaliseerde query's zijn veelvoorkomende redenen voor slechte databaseprestaties. U kunt deze aanbevelingen voor afstemming voor het verbeteren van de prestaties van uw workload kunt toepassen. U kunt ook laat Azure SQL database tot [automatisch de prestaties van uw query's optimaliseren](sql-database-automatic-tuning.md) door het toepassen van alle aanbevelingen en te controleren dat ze betere databaseprestaties geïdentificeerd.

U hebt de volgende opties voor controle en probleemoplossing van de prestaties van de database:

- In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u de database en gebruik vervolgens de controle grafiek om te zoeken naar resources hun maximale nadert. DTU-verbruik wordt standaard weergegeven. Klik op **bewerken** te wijzigen van het tijdsbereik en de waarden die worden weergegeven.
- Gebruik [Query Performance Insight](sql-database-query-performance.md) voor het identificeren van de query's die besteden aan het meest van resources.
- Gebruik [SQL Database Advisor](sql-database-advisor-portal.md) om aanbevelingen voor het maken en verwijderen van indexen, parametriseren query's en schema-problemen oplossen weer te geven.
- Gebruik [Intelligent Insights van Azure SQL](sql-database-intelligent-insights.md) voor de automatische bewaking van de databaseprestaties van uw. Zodra een prestatieprobleem wordt gedetecteerd, wordt een diagnostisch logboek gegenereerd met de details en Root oorzaak Analysis (RCA) van het probleem. Prestaties verbetering aanbeveling wordt geleverd, indien mogelijk.
- [Automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md) en laat Azure SQL database automatisch correctie geïdentificeerd prestatieproblemen kunnen voordoen.
- Gebruik [dynamische beheerweergave (DMV's)](sql-database-monitoring-with-dmvs.md), [uitgebreid gebeurtenissen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-xevent-db-diff-from-svr), en de [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) voor meer het oplossen van prestatieproblemen.

> [!TIP]
> Zie [prestatierichtlijnen](sql-database-performance-guidance.md) technieken die u gebruiken kunt voor het verbeteren van de prestaties van Azure SQL Database na het identificeren van het prestatieprobleem met behulp van een of meer van de bovenstaande manieren vinden.

## <a name="monitor-databases-using-the-azure-portal"></a>Databases bewaken via de Azure-portal

In de [Azure-portal](https://portal.azure.com/), u kunt een individuele database s-gebruik controleren door te klikken en selecteer uw database de **bewaking** grafiek. Nu wordt een venster **Metrische gegevens** geopend, dat u kunt wijzigen door op de knop **Grafiek bewerken** te klikken. Voeg de volgende metrische gegevens toe:

- CPU-percentage
- DTU-percentage
- Gegevens-I/O-percentage
- Databaseomvangpercentage

Als u deze metrische gegevens hebt toegevoegd, kunt u doorgaan met deze bekijken in de **bewaking** grafiek met meer informatie over de **Metric** venster. De vier metrische gegevens tonen het gemiddelde gebruikspercentage ten opzichte van de **DTU** van uw database. Zie de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) artikelen voor meer informatie over Servicelagen.  

![Servicelaagbewaking van databaseprestaties.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

U kunt ook meldingen configureren voor prestatiewaarden. Klik op de knop **Melding toevoegen** in het venster **Metrische gegevens**. Volg de wizard om de melding te configureren. U hebt de keuze om een melding weer te geven als de metrische gegevens een bepaalde drempelwaarde overschrijden of als het meetpunt onder een bepaalde drempelwaarde komt.

Als u bijvoorbeeld verwacht dat de workload van de database zal toenemen, kunt u configureren dat er een e-mailmelding wordt verstuurd wanneer de database 80% van een van de prestatiewaarden heeft bereikt. U kunt dit gebruiken als een vroegtijdige waarschuwing te achterhalen wanneer u misschien moet overschakelen naar de volgende hoogste compute-grootte.

De maatstaven voor prestaties kunt u bepalen of u moet downgraden naar een lagere compute-grootte. Stel dat u een Standard S2-database gebruikt en alle prestatiewaarden aangeven dat de database gemiddeld nooit meer dan 10% gebruikt. Het is dan waarschijnlijk dat de database in Standard S1 goed werkt. Echter rekening houden met workloads die pieken of fluctueren, voordat u de beslissing om te verplaatsen naar een lagere compute-grootte.

## <a name="troubleshoot-performance-issues"></a>Prestatieproblemen oplossen

Voor het opsporen en oplossen van prestatieproblemen, begint u inzicht hebt in de status van elke actieve query en de voorwaarden die relevant zijn voor de status van elke werkbelasting prestatieproblemen veroorzaken. Azure SQL Database om prestaties te verbeteren, te begrijpen dat elke actieve query-aanvraag vanuit uw toepassing in een die wordt uitgevoerd of in een wachtrij staat. Bij het oplossen van een prestatieprobleem in Azure SQL Database, houd rekening met als u in dit artikel kunt vaststellen en oplossen van prestatieproblemen Lees het volgende diagram.

![Statussen van de werkbelasting](./media/sql-database-monitor-tune-overview/workload-states.png)

Voor een werkbelasting met problemen met prestaties, het prestatieprobleem kan worden veroorzaakt door CPU conflicten (een **uitvoeren met betrekking tot** voorwaarde) of afzonderlijke query's zijn er wordt gewacht op iets (een **wachten met betrekking tot** voorwaarde ).

## <a name="running-related-performance-issues"></a>Problemen met prestaties met betrekking tot die wordt uitgevoerd

Als een algemene richtlijn als het CPU-gebruik is consistent op of boven 80%, hebt u een probleem met betrekking tot het uitvoeren. Als er een probleem met betrekking tot die wordt uitgevoerd, kan dit worden veroorzaakt door onvoldoende CPU-resources of het verband houden met een van de volgende voorwaarden:

- Te veel actieve query 's
- Te veel compileren query 's
- Een of meer uitvoeren query's met behulp van een suboptimale queryplan

Als u vaststelt dat er een prestatieprobleem is uitgevoerd met betrekking tot, wordt het doel is het identificeren van het exacte probleem met behulp van een of meer methoden. De meest voorkomende methoden voor het identificeren van problemen met betrekking tot actief zijn:

- Gebruik de [Azure-portal](#monitor-databases-using-the-azure-portal) voor het bewaken van CPU-percentage gebruik.
- Gebruik de volgende [dynamische beheerweergaven](sql-database-monitoring-with-dmvs.md):

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) verbruik van CPU, i/o en geheugen voor een Azure SQL Database-database geretourneerd. Er bestaat één rij voor elke 15 seconden, zelfs als er geen activiteit in de database. Historische gegevens worden bijgehouden voor één uur.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) CPU-gebruik en de opslag-gegevens geretourneerd voor een Azure SQL-Database. De gegevens worden verzameld en samengevoegd binnen vijf minuten durende intervallen.

> [!IMPORTANT]
> Zie voor een verzameling een T-SQL-query's deze DMV's gebruiken voor het oplossen van problemen met het gebruik van de CPU, [prestatieproblemen identificeren CPU](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

Wanneer u het probleem hebt geïdentificeerd, kunt u de probleem-query's af te stemmen of upgrade uitvoeren voor de compute-grootte of servicelaag om de capaciteit van uw Azure SQL-database te begrijpen van de CPU-vereisten te vergroten. Zie voor informatie over het schalen van resources voor individuele databases, [schalen van één database-resources in Azure SQL Database](sql-database-single-database-scale.md) en voor het schalen van resources voor elastische pools, Zie [resources voor elastische pool schalen in Azure SQL Database](sql-database-elastic-pool-scale.md). Zie voor meer informatie over het omhoog schalen van een beheerd exemplaar [resourcelimieten voor Instance-level](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

## <a name="waiting-related-performance-issues"></a>Problemen met prestaties met betrekking tot wachten

Als u er zeker van bent dat u niet een hoge CPU, die wordt uitgevoerd met betrekking tot prestatieprobleem ondervindt, ondervindt u een probleem met betrekking tot wachten. Dat wil zeggen worden het CPU-resources niet gebruikt efficiënt omdat de CPU op een andere bron wacht. In dit geval worden de volgende stap is om te bepalen wat de CPU-bronnen wacht op. De meest voorkomende methoden voor het weergeven van de bovenkant wachten categorieën van het type:

- De [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) voorziet in statistieken per query Wacht na verloop van tijd. In Query Store, zijn wacht typen gecombineerd tot wacht categorieën. De toewijzing van de categorieën wait moet worden gewacht typen is beschikbaar in [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql?view=sql-server-2017#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) retourneert informatie over alle wacht geconfronteerd threads die tijdens de bewerking wordt uitgevoerd. Deze geaggregeerde weergave kunt u prestatieproblemen met Azure SQL Database en met specifieke query's en batches.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) retourneert informatie over de wachtrij wacht van taken die op een resource wachten.

Zoals u in het vorige diagram, zijn de meest voorkomende wacht:

- Vergrendelingen (blokkeren)
- I/O
- `tempdb`-gerelateerde conflicten
- Wachten op het verlenen van geheugen

> [!IMPORTANT]
> Zie voor een verzameling een T-SQL-query's deze DMV's gebruiken voor het oplossen van deze problemen met betrekking tot wachten:
>
> - [Identificeren van problemen met de i/o-prestaties](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificeer `tempdb` prestatieproblemen](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Wachten op het verlenen van geheugen identificeren](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)

## <a name="improving-database-performance-with-more-resources"></a>Verbetering van de prestaties van de database met andere bronnen

Ten slotte, als er geen bruikbare items die de prestaties van uw database kunnen verbeteren, kunt u de hoeveelheid beschikbare resources in Azure SQL Database. U kunt meer resources toewijzen door het veranderen van de [DTU-servicelaag](sql-database-service-tiers-dtu.md) van een individuele database of een toename van het aantal edtu's van een elastische pool op elk gewenst moment. U kunt ook als u de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md), kunt u de servicelaag wijzigen of de resources die zijn toegewezen aan uw database te verhogen.

1. Voor individuele databases, kunt u [Servicelagen](sql-database-service-tiers-dtu.md) of [rekenresources](sql-database-service-tiers-vcore.md) on-demand om betere databaseprestaties.
2. Overweeg het gebruik van meerdere databases, [elastische pools](sql-database-elastic-pool-guidance.md) bronnen automatisch schalen.

## <a name="tune-and-refactor-application-or-database-code"></a>Afstemmen en herstructureren toepassing of databasecode

U kunt de code van de toepassing als u wilt meer optimaal gebruik van de database, wijzigen van indexen, geforceerd plannen of hints gebruiken om handmatig de database aan uw workload kunt wijzigen. Vindt u enkele richtlijnen en tips voor handmatige afstemmen en voor het herschrijven van de code in de [prestaties richtlijnen onderwerp](sql-database-performance-guidance.md) artikel.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt inschakelen automatisch afstemmen in Azure SQL Database en laat de functie voor automatisch afstemmen uw werkbelasting volledig te beheren, Zie [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).
- Voor het gebruik van handmatige afstemmen, kunt u bekijken [aanbevelingen in Azure-portal voor het afstemmen](sql-database-advisor-portal.md) en handmatig toe te passen degene die de prestaties van uw query's verbeteren.
- Wijzigen van de resources die beschikbaar in uw database door het veranderen van zijn [Azure SQL Database-Servicelagen](sql-database-performance-guidance.md)
