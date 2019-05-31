---
title: Bewaking en het afstemmen van prestaties - Azure SQL Database | Microsoft Docs
description: Tips voor het afstemmen in Azure SQL Database via evaluatie en verbetering van de prestaties.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: cae0fbd450e6b392e1689d4642181f6e5279752b
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393213"
---
# <a name="monitoring-and-performance-tuning"></a>Prestaties bewaken en afstemmen

Azure SQL Database kunt u eenvoudig gebruik te bewaken, toevoegen of verwijderen van resources (CPU, geheugen, i/o), de mogelijke problemen en zoek de aanbevelingen die de prestaties van uw database kunnen verbeteren. Azure SQL-Database bevat veel functies die u kunnen automatisch los de problemen in uw databases als u wilt dat de database aan uw workloads aanpassen en automatisch prestaties te optimaliseren. Er zijn echter enkele aangepaste problemen die u mogelijk nodig hebt om op te lossen. In dit artikel wordt uitgelegd enkele aanbevolen procedures en hulpprogramma's die u kunt de prestatieproblemen oplossen.

Er zijn twee belangrijkste activiteiten die u moet doen om ervoor te zorgen dat u de database wordt uitgevoerd zonder problemen:
- [Databaseprestaties bewaken](#monitoring-database-performance) om ervoor te zorgen dat uw workload kunnen worden verwerkt door de resources die zijn toegewezen aan uw database. Als u ziet dat u de resource-limiet hebt bereikt, moet u om te bepalen van de belangrijkste resources verbruiken van query's en ze optimaliseren of meer resources toevoegen door te upgraden van de servicelaag.
- [Problemen met prestaties](#troubleshoot-performance-issues) identificeren om te achterhalen waarom sommige potentiële probleem is opgetreden, de hoofdoorzaak van het probleem en de actie die het probleem wordt opgelost.

## <a name="monitoring-database-performance"></a>Databaseprestaties bewaken

Het bewaken van de prestaties van een SQL-database in Azure begint met het bewaken van het resourcegebruik ten opzichte van het gekozen niveau van databaseprestaties. U moet voor het bewaken van de volgende bronnen:
 - **CPU-gebruik** -moet u controleren zijn u 100% van de CPU-gebruik in een langere periode bereikt. Dit kan betekenen dat u mogelijk een upgrade uitgevoerd of instantie of identificeren en afstemmen van de query's die van de meeste van de compute-kracht gebruikmaken.
 - **Wachtstatistieken** -moet u controleren wat waarom uw query's op bepaalde resources wachten. Queriesmig wacht tot de gegevens worden opgehaald of zijn opgeslagen in de databasebestanden, wachten omdat een resourcelimiet is bereikt, enzovoort.
 - **I/o-gebruik** -moet u controleren zijn bereikt de i/o-limieten van de onderliggende opslag.
 - **Geheugengebruik** -de hoeveelheid geheugen die beschikbaar is voor uw database of instantie in verhouding met het aantal vCores staat, en moet u controleren het genoeg is voor uw workload. Levensverwachting voor pagina is een van de parameters die kunnen duiden op de pagina's snel verwijderd uit het geheugen.

Azure SQL Database **biedt de adviezen per cheque die u kunnen helpen en mogelijke prestatieproblemen oplossen**. U kunt eenvoudig vaststellen kansen te verbeteren en optimaliseren van prestaties van query's zonder te hoeven wijzigen van resources aan de hand [aanbevelingen voor afstemming](sql-database-advisor.md). Ontbrekende indexen en onvoldoende geoptimaliseerde query's zijn veelvoorkomende redenen voor slechte databaseprestaties. U kunt deze aanbevelingen voor afstemming voor het verbeteren van de prestaties van uw workload kunt toepassen. U kunt ook laat Azure SQL database tot [automatisch de prestaties van uw query's optimaliseren](sql-database-automatic-tuning.md) door het toepassen van alle aanbevelingen en te controleren dat ze betere databaseprestaties geïdentificeerd.

U hebt de volgende opties voor controle en probleemoplossing van de prestaties van de database:

- In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u de database en gebruik vervolgens de controle grafiek om te zoeken naar resources hun maximale nadert. DTU-verbruik wordt standaard weergegeven. Klik op **bewerken** te wijzigen van het tijdsbereik en de waarden die worden weergegeven.
- Hulpprogramma's zoals SQL Server Management Studio kunt u vele nuttige rapporten, zoals een [Prestatiedashboard](https://docs.microsoft.com/sql/relational-databases/performance/performance-dashboard?view=sql-server-2017) waar u kunt het Resourcegebruik en bovenste die veel resources verbruiken van query's identificeren of [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store#Regressed)waar u de query's met verminderde prestaties kunt identificeren.
- Gebruik [Query Performance Insight](sql-database-query-performance.md) de [Azure-portal](https://portal.azure.com) in voor het identificeren van de query's die besteden aan het meest van resources. Deze functie is alleen beschikbaar in de individuele databases en elastische Pools.
- Gebruik [SQL Database Advisor](sql-database-advisor-portal.md) om aanbevelingen voor het maken en verwijderen van indexen, parametriseren query's en schema-problemen oplossen weer te geven. Deze functie is alleen beschikbaar in de individuele databases en elastische Pools.
- Gebruik [Intelligent Insights van Azure SQL](sql-database-intelligent-insights.md) voor de automatische bewaking van de databaseprestaties van uw. Zodra een prestatieprobleem wordt gedetecteerd, wordt een diagnostisch logboek gegenereerd met de details en Root oorzaak Analysis (RCA) van het probleem. Prestaties verbetering aanbeveling wordt geleverd, indien mogelijk.
- [Automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md) en laat Azure SQL database automatisch correctie geïdentificeerd prestatieproblemen kunnen voordoen.
- Gebruik [dynamische beheerweergave (DMV's)](sql-database-monitoring-with-dmvs.md), [uitgebreid gebeurtenissen](sql-database-xevent-db-diff-from-svr.md), en de [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) voor meer het oplossen van prestatieproblemen.

> [!TIP]
> Zie [prestatierichtlijnen](sql-database-performance-guidance.md) technieken die u gebruiken kunt voor het verbeteren van de prestaties van Azure SQL Database na het identificeren van het prestatieprobleem met behulp van een of meer van de bovenstaande manieren vinden.

## <a name="troubleshoot-performance-issues"></a>Prestatieproblemen oplossen

Voor het opsporen en oplossen van prestatieproblemen, begint u inzicht hebt in de status van elke actieve query en de voorwaarden die relevant zijn voor de status van elke werkbelasting prestatieproblemen veroorzaken. Azure SQL Database om prestaties te verbeteren, te begrijpen dat elke actieve query-aanvraag vanuit uw toepassing in een die wordt uitgevoerd of in een wachtrij staat. Bij het oplossen van een prestatieprobleem in Azure SQL Database, houd rekening met als u in dit artikel kunt vaststellen en oplossen van prestatieproblemen Lees het volgende diagram.

![Statussen van de werkbelasting](./media/sql-database-monitor-tune-overview/workload-states.png)

Voor een werkbelasting met problemen met prestaties, het prestatieprobleem kan worden veroorzaakt door CPU conflicten (een **uitvoeren met betrekking tot** voorwaarde) of afzonderlijke query's zijn er wordt gewacht op iets (een **wachten met betrekking tot** voorwaarde ).

De oorzaken of **uitvoeren met betrekking tot** problemen kunnen zijn:
- **Compilatieproblemen** -SQL-queryoptimalisatieprogramma suboptimale plan vanwege verouderde statistieken, onjuist schatting van het aantal rijen dat wordt verwerkt, of de schatting van de vereiste geheugen kunnen produceren. Als u weet dat deze query is sneller uitgevoerd in het verleden of op andere exemplaar (Managed Instance of SQL Server-exemplaar), zijn nemen de daadwerkelijke uitvoering plannen en te vergelijken om te zien in het andere. Probeer queryhints of opnieuw worden opgebouwd statistieken of indexen om op te halen van de betere plan toe te passen. Schakel automatische abonnementcorrectie in Azure SQL Database om deze problemen automatisch te beperken.
- **Uitvoeringsproblemen** : als het queryplan optimaal is en dit waarschijnlijk te met enkele resourcebeperkingen in de database zoals logboek schrijven-doorvoer maken is of deze mogelijk gebruik van gedefragmenteerde indexen die moeten opnieuw worden opgebouwd. Een groot aantal gelijktijdige query's die de resources zijn uitgaven mogelijk ook de oorzaak van problemen kan worden uitgevoerd. **Wachten met betrekking tot** problemen zijn in de meeste gevallen met betrekking tot de problemen worden uitgevoerd, omdat de query's die niet efficiënt worden uitgevoerd waarschijnlijk tot enkele resources wacht zijn.

De oorzaken of **wachten met betrekking tot** problemen kunnen zijn:
- **Blokkeren** -één query kan de vergrendeling houdt op sommige objecten in de database terwijl anderen toegang probeert te krijgen van de dezelfde objecten. U kunt eenvoudig de blokkerende query's met behulp van DMV of controlehulpprogramma's identificeren.
- **I/o-problemen** -query's mogelijk moet wachten tot de pagina's naar de gegevens of logboek-bestanden moeten worden geschreven. In dit geval ziet u `INSTANCE_LOG_RATE_GOVERNOR`, `WRITE_LOG`, of `PAGEIOLATCH_*` statistieken in de DMV wachten.
- **Problemen met TempDB** : als u van een groot aantal tijdelijke tabellen gebruikmaakt of er een groot aantal TempDB morsen in uw plannen voor uw query's die u mogelijk een probleem met het TempDB-doorvoer. 
- **Geheugenproblemen** -kunt u wellicht niet voldoende geheugen voor uw workload zodat de levensverwachting voor pagina kan verwijderen, of uw query's minder geheugentoekenning krijgen dan nodig is. In sommige gevallen wordt de ingebouwde intelligentie in queryoptimalisatieprogramma deze problemen opgelost.
 
In de volgende secties wordt dit uitgelegd hoe u om te identificeren en oplossen van deze problemen.

## <a name="running-related-performance-issues"></a>Problemen met prestaties met betrekking tot die wordt uitgevoerd

Als een algemene richtlijn als het CPU-gebruik is consistent op of boven 80%, hebt u een probleem met betrekking tot het uitvoeren. Als er een probleem met betrekking tot die wordt uitgevoerd, kan dit worden veroorzaakt door onvoldoende CPU-resources of het verband houden met een van de volgende voorwaarden:

- Te veel actieve query 's
- Te veel compileren query 's
- Een of meer uitvoeren query's met behulp van een suboptimale queryplan

Als u vaststelt dat er een prestatieprobleem is uitgevoerd met betrekking tot, wordt het doel is het identificeren van het exacte probleem met behulp van een of meer methoden. De meest voorkomende methoden voor het identificeren van problemen met betrekking tot actief zijn:

- Gebruik de [Azure-portal](sql-database-manage-after-migration.md#monitor-databases-using-the-azure-portal) voor het bewaken van CPU-percentage gebruik.
- Gebruik de volgende [dynamische beheerweergaven](sql-database-monitoring-with-dmvs.md):

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) verbruik van CPU, i/o en geheugen voor een Azure SQL Database-database geretourneerd. Er bestaat één rij voor elke 15 seconden, zelfs als er geen activiteit in de database. Historische gegevens worden bijgehouden voor één uur.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) CPU-gebruik en de opslag-gegevens geretourneerd voor een Azure SQL-Database. De gegevens worden verzameld en samengevoegd binnen vijf minuten durende intervallen.

> [!IMPORTANT]
> Zie voor een verzameling een T-SQL-query's deze DMV's gebruiken voor het oplossen van problemen met het gebruik van de CPU, [prestatieproblemen identificeren CPU](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

### <a name="ParamSniffing"></a> Query's met de parameter-gevoelige query uitvoering plan problemen oplossen

Probleem met de parameter gevoelige plan (PSP) verwijst naar een scenario waarbij het queryoptimalisatieprogramma genereert een plan voor het uitvoeren van query die is geoptimaliseerd voor een specifieke parameterwaarde (of een set waarden) en het in de cache-abonnement is niet optimaal zijn voor de parameterwaarden die worden gebruikt in opeenvolgende uitvoeringen. Plannen voor niet-optimale kunnen vervolgens leiden tot prestatieproblemen van query's en de algehele doorvoer verslechtering van de werkbelasting. Zie voor meer informatie over het bekijken van de parameter en de verwerking van query's, de [Query verwerken Architecture Guide](/sql/relational-databases/query-processing-architecture-guide#ParamSniffing).

Er zijn verschillende oplossingen die worden gebruikt voor het oplossen van problemen, elk met een bijbehorende optimalisatie- en nadelen:

- Gebruik de [COMPILEREN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) queryhint in elke query kan worden uitgevoerd. Deze tijdelijke oplossing wisselt lokale compilatietijd en hogere CPU voor een hogere kwaliteit van het abonnement. Met behulp van de `RECOMPILE` optie is vaak niet mogelijk voor workloads waarvoor een hoge doorvoer.
- Gebruik de [optie (OPTIMALISEREN voor...) ](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) queryhint voor de onderdrukking van de feitelijke parameter-waarde met een typische parameterwaarde die een goed genoeg plan voor de meeste mogelijkheden voor parameter-waarde produceert.   Deze optie vereist een goed begrip van de optimale parameterwaarden en bijbehorende plan kenmerken.
- Gebruik [optie (OPTIMALISEREN voor onbekend)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) queryhint voor de onderdrukking van de werkelijke parameterwaarde ruil met behulp van het gemiddelde van de vector dichtheid. Een andere manier om dit te doen is door de binnenkomende parameterwaarden vastleggen in lokale variabelen en vervolgens met behulp van de lokale variabelen binnen de predicaten in plaats van de parameters zelf. De gemiddelde dichtheid moet *goed genoeg* met deze specifieke oplossing.
- Uitschakelen van de parameter bekijken met de [DISABLE_PARAMETER_SNIFFING](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) queryhint.
- Gebruik de [KEEPFIXEDPLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) queryhint om te voorkomen dat gecompileerd in de cache. Deze oplossing wordt ervan uitgegaan dat de *goede enough* algemene plan is al in de cache. U kunt ook automatisch bijwerken van statistieken uitschakelen om te kunnen verminderen de kans op het goede plan wordt verwijderd en een nieuw ongeldig plan wordt gecompileerd.
- Afdwingen van het abonnement met behulp van expliciet [USE PLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) queryhint (door expliciet op te geven, door in te stellen van een specifiek plan met behulp van de Query Store of door in te schakelen [automatisch afstemmen](sql-database-automatic-tuning.md).
- De enkele procedure vervangen door een geneste set procedures die kunnen worden gebruikt op basis van voorwaardelijke logica en de bijbehorende parameterwaarden.
- Maak dynamische tekenreeks uitvoering alternatieven voor de proceduredefinitie van een statische.

Zie voor meer informatie over het oplossen van dit soort problemen:

- Dit [ik een parameter Pluk](https://blogs.msdn.microsoft.com/queryoptteam/2006/03/31/i-smell-a-parameter/) blogbericht
- Dit [dynamische sql ten opzichte van de kwaliteit van de planning voor query's met parameters](https://blogs.msdn.microsoft.com/conor_cunningham_msft/2009/06/03/conor-vs-dynamic-sql-vs-procedures-vs-plan-quality-for-parameterized-queries/) blogbericht
- Dit [optimalisatietechnieken van SQL-Query in SQL Server: Parameter Sniffing](https://www.sqlshack.com/query-optimization-techniques-in-sql-server-parameter-sniffing/) blogbericht

### <a name="troubleshooting-compile-activity-due-to-improper-parameterization"></a>Het compileren activiteit vanwege onjuiste parameterisering oplossen

Wanneer een query letterlijke waarden heeft, de database-engine kiest voor het automatisch voorzien van de instructie of een gebruiker kan deze expliciet voorzien om te reduceren aantal compileert. Een groot aantal compileert van een query met behulp van de hetzelfde patroon, maar andere letterlijke waarden kan leiden tot intensief CPU-gebruik. Op dezelfde manier als u een query die nog steeds letterlijke tekenreeksen zijn slechts gedeeltelijk voorzien, heeft de database-engine geen parameter van het verder.  Hieronder volgt een voorbeeld van een gedeeltelijk geparameteriseerde query:

```sql
SELECT * FROM t1 JOIN t2 ON t1.c1 = t2.c1
WHERE t1.c1 = @p1 AND t2.c2 = '961C3970-0E54-4E8E-82B6-5545BE897F8F'
```

In het voorgaande voorbeeld `t1.c1` duurt `@p1` maar `t2.c2` blijft houden GUID als letterlijke waarde. In dit geval, als u de waarde voor wijzigt `c2`, de query wordt beschouwd als een andere query en een nieuwe verzameling wordt uitgevoerd. De oplossing is ook voorzien van de GUID compilaties in het voorgaande voorbeeld, verminderen.

De volgende query geeft het aantal query's op queryhash om te bepalen als een query goed met parameters of niet:

```sql
SELECT  TOP 10  
  q.query_hash
  , count (distinct p.query_id ) AS number_of_distinct_query_ids
  , min(qt.query_sql_text) AS sampled_query_text
FROM sys.query_store_query_text AS qt
  JOIN sys.query_store_query AS q
     ON qt.query_text_id = q.query_text_id
  JOIN sys.query_store_plan AS p 
     ON q.query_id = p.query_id
  JOIN sys.query_store_runtime_stats AS rs 
     ON rs.plan_id = p.plan_id
  JOIN sys.query_store_runtime_stats_interval AS rsi
     ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE
  rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
  AND query_parameterization_type_desc IN ('User', 'None')
GROUP BY q.query_hash
ORDER BY count (distinct p.query_id) DESC
```

### <a name="resolve-problem-queries-or-provide-more-resources"></a>Opgelost probleem query's of geef andere bronnen

Wanneer u het probleem hebt geïdentificeerd, kunt u de probleem-query's af te stemmen of upgrade uitvoeren voor de compute-grootte of servicelaag om de capaciteit van uw Azure SQL-database te begrijpen van de CPU-vereisten te vergroten. Zie voor informatie over het schalen van resources voor individuele databases, [schalen van één database-resources in Azure SQL Database](sql-database-single-database-scale.md) en voor het schalen van resources voor elastische pools, Zie [resources voor elastische pool schalen in Azure SQL Database](sql-database-elastic-pool-scale.md). Zie voor meer informatie over het omhoog schalen van een beheerd exemplaar [resourcelimieten voor Instance-level](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

### <a name="determine-if-running-issues-due-to-increase-workload-volume"></a>Bepalen of het uitvoeren van toepassingsproblemen vanwege een toename van de werkbelasting volume

Een toename in de toepassingsverkeer en werkbelasting kunt rekening voor hogere CPU-gebruik, maar u moet zorgvuldig om dit probleem goed vast te stellen. Beantwoord deze vragen om te bepalen als inderdaad een toename van de CPU veroorzaakt door de werkbelasting volume worden gewijzigd wordt in een hoog CPU-scenario:

1. Zijn de query's van de toepassing de oorzaak van het hoge CPU-probleem?
2. Voor de hoogste CPU verbruikt query's (die kunnen worden geïdentificeerd):

   - Bepalen of er meerdere uitvoering plannen die zijn gekoppeld aan dezelfde query zijn. Als dit het geval is, te bepalen waarom.
   - Bepaal voor query's met hetzelfde abonnement worden uitgevoerd, als de uitvoertijd consistent zijn en het aantal uitvoeringen verhoogd. Zo ja, zijn er waarschijnlijk prestatieproblemen vanwege een toename van de werkbelasting.

Om samen te vatten, als het queryplan kan worden uitgevoerd anders niet uitvoeren, maar CPU-gebruik verhoogd samen met het aantal uitvoeringen, is er waarschijnlijk een probleem van de prestaties met betrekking tot toename van de werkbelasting.

Het is niet altijd eenvoudig te sluiten er is een wijziging voor het volume van werkbelasting die vraagt om een probleem met de CPU.   Factoren om te overwegen: 

- **Resourcegebruik gewijzigd**

  Neem bijvoorbeeld een scenario waarbij de CPU wordt verhoogd tot 80% voor een lange periode.  CPU-gebruik alleen betekent niet werkbelasting volume gewijzigd.  Uitvoeringsplan regressie en distributie van gegevenswijzigingen kunnen ook bijdragen aan meer Resourcegebruik zelfs als de toepassing exact dezelfde werkbelasting uitvoeren van is een query uitvoeren.

- **Nieuwe query wordt weergegeven**

   Een toepassing kan een nieuwe set query's op verschillende tijdstippen station.

- **Aantal aanvragen worden verhoogd of verlaagd**

   Dit scenario is de meest opvallende meting van de werkbelasting. Het aantal query's komt niet altijd in meer Resourcegebruik van de overeen. Met deze metriek is echter nog steeds een aanzienlijke signaal ervan uitgaande dat andere factoren zijn niet gewijzigd.

## <a name="waiting-related-performance-issues"></a>Problemen met prestaties met betrekking tot wachten

Als u er zeker van bent dat u niet een hoge CPU, die wordt uitgevoerd met betrekking tot prestatieprobleem ondervindt, ondervindt u een probleem met betrekking tot wachten. Dat wil zeggen worden het CPU-resources niet gebruikt efficiënt omdat de CPU op een andere bron wacht. In dit geval worden de volgende stap is om te bepalen wat de CPU-bronnen wacht op. De meest voorkomende methoden voor het weergeven van de bovenkant wachten categorieën van het type:

- De [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) voorziet in statistieken per query Wacht na verloop van tijd. In Query Store, zijn wacht typen gecombineerd tot wacht categorieën. De toewijzing van de categorieën wait moet worden gewacht typen is beschikbaar in [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) retourneert informatie over alle wacht geconfronteerd threads die tijdens de bewerking wordt uitgevoerd. Deze geaggregeerde weergave kunt u prestatieproblemen met Azure SQL Database en met specifieke query's en batches.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) retourneert informatie over de wachtrij wacht van taken die op een resource wachten.

In scenario's met hoge CPU, de Query Store en wacht statistieken zijn niet altijd aan de CPU-gebruik voor deze twee redenen:

- Hoge CPU in beslag nemen query's mogelijk nog steeds uitgevoerd en de query's zijn nog niet klaar
- De hoge CPU in beslag nemen query's worden uitgevoerd wanneer een failover is opgetreden

Query Store en wacht statistieken bij te houden dynamische beheerweergaven alleen resultaten weergeven voor query's is voltooid en is een time-out en gegevens voor instructies dat momenteel wordt uitgevoerd (tot ze voltooid), worden niet weergegeven. De dynamische Beheerweergave [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) kunt u query's dat momenteel wordt uitgevoerd en de bijbehorende werknemer bijhouden.

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
> - [TigerToolbox - wacht en vergrendelingen](https://github.com/Microsoft/tigertoolbox/tree/master/Waits-and-Latches)
> - [TigerToolbox - usp_whatsup](https://github.com/Microsoft/tigertoolbox/tree/master/usp_WhatsUp)

## <a name="improving-database-performance-with-more-resources"></a>Verbetering van de prestaties van de database met andere bronnen

Ten slotte, als er geen bruikbare items die de prestaties van uw database kunnen verbeteren, kunt u de hoeveelheid beschikbare resources in Azure SQL Database. U kunt meer resources toewijzen door het veranderen van de [DTU-servicelaag](sql-database-service-tiers-dtu.md) van een individuele database of een toename van het aantal edtu's van een elastische pool op elk gewenst moment. U kunt ook als u de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md), kunt u de servicelaag wijzigen of de resources die zijn toegewezen aan uw database te verhogen.

1. Voor individuele databases, kunt u [Servicelagen](sql-database-single-database-scale.md) of [rekenresources](sql-database-single-database-scale.md) on-demand om betere databaseprestaties.
2. Overweeg het gebruik van meerdere databases, [elastische pools](sql-database-elastic-pool-guidance.md) bronnen automatisch schalen.

## <a name="tune-and-refactor-application-or-database-code"></a>Afstemmen en herstructureren toepassing of databasecode

U kunt de code van de toepassing als u wilt meer optimaal gebruik van de database, wijzigen van indexen, geforceerd plannen of hints gebruiken om handmatig de database aan uw workload kunt wijzigen. Vindt u enkele richtlijnen en tips voor handmatige afstemmen en voor het herschrijven van de code in de [prestaties richtlijnen onderwerp](sql-database-performance-guidance.md) artikel.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt inschakelen automatisch afstemmen in Azure SQL Database en laat de functie voor automatisch afstemmen uw werkbelasting volledig te beheren, Zie [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).
- Voor het gebruik van handmatige afstemmen, kunt u bekijken [aanbevelingen in Azure-portal voor het afstemmen](sql-database-advisor-portal.md) en handmatig toe te passen degene die de prestaties van uw query's verbeteren.
- Wijzigen van de resources die beschikbaar in uw database door het veranderen van zijn [Azure SQL Database-Servicelagen](sql-database-performance-guidance.md)
