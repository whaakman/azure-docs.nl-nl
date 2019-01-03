---
title: Oplossen van prestatieproblemen met de Azure SQL Database met intelligente inzichten | Microsoft Docs
description: Intelligent Insights helpt bij het oplossen van prestatieproblemen met de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: ad7d56b3a23d163cfbc6c9ca14c2788c5f96486b
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53600859"
---
# <a name="troubleshoot-azure-sql-database-performance-issues-with-intelligent-insights"></a>Oplossen van prestatieproblemen met de Azure SQL Database met intelligente inzichten

Deze pagina bevat informatie over Azure SQL Database en prestatieproblemen Managed Instance gedetecteerd via de [Intelligent Insights](sql-database-intelligent-insights.md) database prestaties-diagnoselogboek. De telemetrie diagnostisch logboek kan worden gestreamd naar [Azure Log Analytics](../azure-monitor/insights/azure-sql.md), [Azure Event Hubs](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage), of een oplossing van derden voor aangepaste waarschuwingen van DevOps en rapportagemogelijkheden.

> [!NOTE]
> Zie voor een snelle prestaties van SQL Database problemen oplossen met behulp van intelligente inzichten met, de [aanbevolen oplossingen stroom](sql-database-intelligent-insights-troubleshoot-performance.md#recommended-troubleshooting-flow) stroomdiagram in dit document.
>

## <a name="detectable-database-performance-patterns"></a>Detecteerbare patronen voor prestaties

Intelligent Insights detecteert automatisch prestatieproblemen met SQL Database en de Managed Instance-databases op basis van wachttijden van query-uitvoering, fouten of time-outs. Het levert van gedetecteerde prestatiepatronen in het logboek met diagnostische gegevens. Detecteerbare prestatiepatronen worden samengevat in de onderstaande tabel.

| Detecteerbare prestatiepatronen | Beschrijving voor Azure SQL Database en elastische pools | Beschrijving voor de databases in het beheerde exemplaar |
| :------------------- | ------------------- | ------------------- |
| [Bronlimieten bereikt](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) | Verbruik van beschikbare resources (dtu's), werkthreads database of database-aanmeldingssessies die beschikbaar is op de bewaakte abonnement heeft limieten bereikt. Dit is die betrekking hebben op de prestaties van de SQL-Database. | Verbruik van CPU-resources bereikt beheerd exemplaar limieten. Dit is die betrekking hebben op de prestaties van de database. |
| [Toename van de werkbelasting](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) | Workload toe- of doorlopende opeenstapeling van werkbelasting op de database is gedetecteerd. Dit is die betrekking hebben op de prestaties van de SQL-Database. | Toename van de werkbelasting is gedetecteerd. Dit is die betrekking hebben op de prestaties van de database. |
| [Geheugendruk](sql-database-intelligent-insights-troubleshoot-performance.md#memory-pressure) | Werknemers die aangevraagd geheugen verleent moeten wachten voor geheugentoewijzingen statistisch aanzienlijke hoeveelheden tijd. Of een hogere opeenstapeling van de werknemers die aangevraagd geheugen verleent bestaat. Dit is die betrekking hebben op de prestaties van de SQL-Database. | Werknemers die zijn aangevraagd geheugen verleent wachten geheugentoewijzingen voor een statistisch aanzienlijke hoeveelheid tijd. Dit is die betrekking hebben op de prestaties van de database. |
| [Vergrendelen](sql-database-intelligent-insights-troubleshoot-performance.md#locking) | Vergrendelen van overmatige database gedetecteerd die betrekking hebben op de prestaties van de SQL-Database. | Vergrendelen van overmatige database gedetecteerd die betrekking hebben op de prestaties van de database. |
| [Verbeterde MAXDOP](sql-database-intelligent-insights-troubleshoot-performance.md#increased-maxdop) | De maximale graad van parallelle uitvoering optie (MAXDOP) is gewijzigd die betrekking hebben op de efficiëntie van de uitvoering van query. Dit is die betrekking hebben op de prestaties van de SQL-Database. | De maximale graad van parallelle uitvoering optie (MAXDOP) is gewijzigd die betrekking hebben op de efficiëntie van de uitvoering van query. Dit is die betrekking hebben op de prestaties van de database. |
| [Pagelatch conflicten](sql-database-intelligent-insights-troubleshoot-performance.md#pagelatch-contention) | Meerdere threads probeert gelijktijdig te krijgen tot de dezelfde in-memory-gegevens buffer's leidt tot verbeterde wachttijden en pagelatch conflicten veroorzaakt. Dit is die betrekking hebben op de prestaties van de SQL-database. | Meerdere threads probeert gelijktijdig te krijgen tot de dezelfde in-memory-gegevens buffer's leidt tot verbeterde wachttijden en pagelatch conflicten veroorzaakt. Dit is die betrekking hebben op de prestaties van de database. |
| [Ontbrekende Index](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index) | Ontbrekende index is gedetecteerd die betrekking hebben op de prestaties van de SQL-database. | Ontbrekende index is gedetecteerd die betrekking hebben op de prestaties van de database. |
| [Nieuwe Query](sql-database-intelligent-insights-troubleshoot-performance.md#new-query) | Nieuwe query gedetecteerd die betrekking hebben op de algehele prestaties van de SQL-Database. | Nieuwe query gedetecteerd die betrekking hebben op de algehele prestaties van de database. |
| [Verbeterde wacht statistiek](sql-database-intelligent-insights-troubleshoot-performance.md#increased-wait-statistic) | Verbeterde database wachttijden zijn gedetecteerd die betrekking hebben op de prestaties van de SQL-database. | Verbeterde database wachttijden zijn gedetecteerd die betrekking hebben op de prestaties van de database. |
| [TempDB conflicten](sql-database-intelligent-insights-troubleshoot-performance.md#tempdb-contention) | Meerdere threads probeert te krijgen tot de dezelfde TempDB-resource een knelpunt veroorzaakt. Dit is die betrekking hebben op de prestaties van de SQL-Database. | Meerdere threads probeert te krijgen tot de dezelfde TempDB-resource een knelpunt veroorzaakt. Dit is die betrekking hebben op de prestaties van de database. |
| [Elastische Pool-DTU-tekort](sql-database-intelligent-insights-troubleshoot-performance.md#elastic-pool-dtu-shortage) | Gebrek aan beschikbare edtu's in de elastische pool is die betrekking hebben op prestaties van SQL Database. | Niet beschikbaar voor Managed Instance aangezien hierbij vCore-model. |
| [Regressie plannen](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression) | Nieuw plan of een wijziging in de werkbelasting van een bestaand plan is gedetecteerd. Dit is die betrekking hebben op de prestaties van de SQL-Database. | Nieuw plan of een wijziging in de werkbelasting van een bestaand plan is gedetecteerd. Dit is die betrekking hebben op de prestaties van de database. |
| [Het wijzigen van de database Scoped Configuration](sql-database-intelligent-insights-troubleshoot-performance.md#database-scoped-configuration-value-change) | Wijziging in de configuratie op de SQL-Database is gedetecteerd die betrekking hebben op de prestaties van de database. | Wijziging in de configuratie van de database is gedetecteerd die betrekking hebben op de prestaties van de database. |
| [Trage Client](sql-database-intelligent-insights-troubleshoot-performance.md#slow-client) | Trage toepassingsclient is niet snel genoeg uitvoer van de database gebruiken. Dit is die betrekking hebben op de prestaties van de SQL-Database. | Trage toepassingsclient is niet snel genoeg uitvoer van de database gebruiken. Dit is die betrekking hebben op de prestaties van de database. |
| [Downgrade van laag-prijzen](sql-database-intelligent-insights-troubleshoot-performance.md#pricing-tier-downgrade) | Beschikbare resources prijzen laag downgrade actie worden verlaagd. Dit is die betrekking hebben op de prestaties van de SQL-Database. | Beschikbare resources prijzen laag downgrade actie worden verlaagd. Dit is die betrekking hebben op de prestaties van de database. |

> [!TIP]
> Inschakelen om met SQL Database continu de prestaties te optimaliseren, [Azure SQL Database automatisch afstemmen](https://docs.microsoft.com/azure/sql-database/sql-database-automatic-tuning). Deze unieke functie van SQL Database ingebouwde intelligentie continu bewaakt uw SQL-database, automatisch afgestemd indexen en past query uitvoering plan correcties.
>

De volgende sectie wordt beschreven detecteerbare prestatiepatronen in meer detail.

## <a name="reaching-resource-limits"></a>Bronlimieten bereikt

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties combineert prestatieproblemen die gerelateerd zijn aan de beschikbare resourcelimieten, werknemer-limieten en sessielimieten is bereikt. Nadat u dit probleem wordt gedetecteerd, wordt een beschrijvingsveld van de logboekbestanden met diagnostische gegevens geeft aan of het prestatieprobleem gerelateerd is aan de resource, werknemer of sessielimieten.

Resources voor SQL Database meestal worden aangeduid [DTU](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu) of [vCore](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-vcore) resources. Het patroon van de resourcelimieten bereiken wanneer gedetecteerd wordt herkend prestatievermindering query wordt veroorzaakt door een van de gemeten resourcelimieten wordt bereikt.

De sessie limieten resource geeft aan dat het aantal beschikbare gelijktijdige aanmeldingen bij de SQL-database. Dit patroon prestaties wordt herkend als toepassingen die zijn verbonden met de SQL-databases hebt bereikt van het aantal beschikbare gelijktijdige aanmeldingen bij de database. Als toepassingen proberen te gebruiken meer sessies dan beschikbaar zijn op een database, wordt de prestaties van query's beïnvloed.

Werknemer limieten bereiken is een specifieke aanvraag resourcebeperkingen bereikt omdat beschikbare werkrollen worden niet meegeteld in het gebruik van DTU of vCore. Werknemer beperkingen met betrekking tot een database wordt bereikt, kan de opkomst van wachttijden van resource-specifieke, wat tot prestatievermindering query leidt veroorzaken.

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens voert query-hashes van query's die de prestaties en de resource verbruikspercentages beïnvloed. U kunt deze informatie gebruiken als uitgangspunt voor het optimaliseren van de werkbelasting van uw database. In het bijzonder, kunt u de query's die invloed hebben op de prestaties achteruitgaan door het toevoegen van indexen optimaliseren. Of u toepassingen met een meer zelfs Werkbelastingsverdeling kunt optimaliseren. Als u geen workloads verminderen of optimalisaties, houd rekening met de prijscategorie van uw SQL database-abonnement te verhogen van de hoeveelheid beschikbare resources te verhogen.

Als u de beschikbare limieten zijn bereikt, kunt u uw toepassingen optimaliseren door het aantal aanmeldingen die zijn aangebracht in de database te verminderen. Als u niet te verminderen van het aantal aanmeldingen van uw toepassingen met de database, houd rekening met de prijscategorie van uw database te verhogen. Of u kunt splitsen en verplaatsen van uw database in meerdere databases voor een meer evenwichtige verdeling van de werkbelasting.

Zie voor meer suggesties voor het oplossen van sessielimieten, [omgaan met de beperkingen van de SQL-Database maximale aanmeldingen](https://blogs.technet.microsoft.com/latam/2015/06/01/how-to-deal-with-the-limits-of-azure-sql-database-maximum-logins/). Zie [overzicht van resource beperkt op een logische server](sql-database-resource-limits-logical-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.

## <a name="workload-increase"></a>Toename van de werkbelasting

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon prestaties identificeert problemen veroorzaakt door een toename van de werkbelasting of, in de strengere vorm een pile-up werkbelasting.

Deze detectie wordt gedaan door een combinatie van verschillende metrische gegevens. De basic-metric gemeten detecteert een toename van de vergeleken met de afgelopen basislijn van de werkbelasting van de workload. De andere vorm van detectie is gebaseerd op een grote toename in actieve werkthreads die groot genoeg is van invloed zijn op de prestaties van query's te meten.

In de vorm ernstigere de werkbelasting mogelijk continu opstapelen vanwege de modeldatabase groter van de SQL-database voor het afhandelen van de werkbelasting. Het resultaat is een voortdurend groeiende werkbelastingsgrootte de werkbelasting pile-up voorwaarde is. Vanwege dit probleem groeit de tijd dat de werkbelasting moet tot de uitvoering van wachten. Deze voorwaarde vertegenwoordigt een van de meest ernstige prestatieproblemen met de database. Dit probleem wordt gedetecteerd door middel van controle van de toename van het aantal afgebroken werkthreads. 

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens levert het aantal query's waarvan de uitvoering is toegenomen en de queryhash van de query's uitvoeren met de grootste bijdrage aan de toename van de werkbelasting. U kunt deze informatie gebruiken als uitgangspunt voor het optimaliseren van de werkbelasting. De query die is geïdentificeerd als het grootste aandeel in de toename van de werkbelasting is vooral nuttig als uw beginpunt.

U kunt overwegen de belasting meer evenredig met de database te distribueren. U kunt de query die invloed heeft op de prestaties door het toevoegen van indexen. U kunt ook uw werkbelasting tussen meerdere databases distribueren. Als deze oplossingen niet mogelijk is, kunt u overwegen de prijscategorie van uw SQL database-abonnement te verhogen van de hoeveelheid beschikbare resources te verhogen.

## <a name="memory-pressure"></a>Geheugendruk

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon prestaties geeft aan dat de afname van de huidige databaseprestaties veroorzaakt door geheugendruk of in de vorm ernstigere geheugen pile-up voorwaarde, vergeleken met de basislijn van de afgelopen zeven dagen prestaties.

Geheugendruk geeft aan dat een prestatieniveau waarin zich een groot aantal werkthreads aanvragende geheugen in de SQL-database verleent. Hoog volume zorgt ervoor dat een probleem met het hoog geheugengebruik waar de SQL-database wordt uitgevoerd kan geen geheugen efficiënt toewijzen aan alle werknemers die deze aanvragen. Een van de meest voorkomende oorzaken voor dit probleem is gerelateerd aan de hoeveelheid geheugen beschikbaar is voor de SQL-database op één hand. Aan de andere kant een toename van werkbelasting een zorgt ervoor dat de toename in werkthreads en de geheugendruk.

De strengere vorm van geheugendruk is de voorwaarde van de pile-up geheugen. Deze voorwaarde wordt aangegeven dat een groter aantal werkthreads geheugen verleent aanvraagt dan er query's het vrijgeven van het geheugen. Dit aantal werkthreads aanvragende geheugen ook verleent kan worden continu te vergroten (opstapelen) omdat de SQL database-engine kan geen geheugen efficiënt genoeg om te voldoen aan de vraag toewijzen. De voorwaarde van geheugen pile-up vertegenwoordigt een van de meest ernstige prestatieproblemen met de database.

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens levert de details van gegevensarchief object geheugen met de medewerker (dat wil zeggen, werkthread) die is gemarkeerd als de hoogste reden voor hoog geheugengebruik en relevante tijdstempels. U kunt deze informatie gebruiken als basis voor het oplossen van problemen. 

U kunt optimaliseren of verwijderen van query's met betrekking tot de clerks met het hoogste geheugengebruik. Ook kunt u ervoor zorgen dat u gegevens die u niet van plan bent te gebruiken zijn niet opvragen. Het wordt aangeraden om altijd te gebruiken een WHERE-component in uw query's. Bovendien is het raadzaam dat u niet-geclusterde indexen om te zoeken naar de gegevens in plaats van scant u dit maken.

U kunt ook de werkbelasting beperken door optimaliseren of te verdelen over meerdere databases. Of u uw werkbelasting tussen meerdere databases kunt verdelen. Als deze oplossingen niet mogelijk is, kunt u overwegen de prijscategorie van uw SQL database-abonnement te verhogen van de hoeveelheid geheugenresources beschikbaar zijn voor de database te verhogen.

Zie voor meer suggesties voor probleemoplossing, [geheugen verleent meditation: De verwarrende SQL Server-geheugen consument met veel namen](https://blogs.msdn.microsoft.com/sqlmeditation/2013/01/01/memory-meditation-the-mysterious-sql-server-memory-consumer-with-many-names/).

## <a name="locking"></a>Vergrendelen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon prestaties geeft aan dat de afname van de huidige databaseprestaties waarin overmatige database vergrendelen wordt gedetecteerd in vergelijking met de basislijn van de afgelopen zeven dagen prestaties. 

In moderne RDBMS van TOPKLASSE is het essentieel is voor de implementatie met meerdere threads systemen waarin prestaties worden gemaximaliseerd door meerdere gelijktijdige werknemers en parallelle databasetransacties waar mogelijk vergrendelen. Vergrendelen in deze context verwijst naar het ingebouwde mechanisme voor waarin slechts één transactie exclusief toegang tot de rijen, pagina's, tabellen en bestanden die zijn vereist en niet met een andere transactie voor resources concurreren. Als de transactie die de resources voor gebruik vergrendeld met hen is voltooid, wordt de vergrendeling op deze resources vrijgegeven, waardoor andere transacties voor toegang tot de vereiste resources. Zie voor meer informatie over het vergrendelen van [vergrendelen in de database-engine](https://msdn.microsoft.com/library/ms190615.aspx).

Als transacties uitgevoerd door de SQL-engine gedurende langere perioden wachten voor toegang tot resources die zijn vergrendeld voor gebruik, is deze wachttijd zorgt ervoor dat de vertraging van de prestaties van de werkbelastingen worden uitgevoerd. 

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens levert vergrendelen details die u als basis voor het oplossen van problemen gebruiken kunt. U kunt de gerapporteerde blokkerende query's, dat wil zeggen, de query's die de vergrendeling prestatievermindering, analyseren en verwijder ze. In sommige gevallen kunt u mogelijk lukt dit in de blokkerende query's optimaliseren.

De eenvoudigste en veiligste manier om het probleem te verhelpen is transacties kort te houden en om de voetafdruk van de vergrendeling van de meest dure query's te verkleinen. U kunt splitst u een grote batch van bewerkingen in kleinere bewerkingen. Het wordt aangeraden de query vergrendeling om voetafdruk te verkleinen door de query zo efficiënt mogelijk te maken. Grote scans verminderen omdat ze verhoogt de kans op impassen en nadelige invloed heeft op de algehele prestaties van de database. U kunt voor geïdentificeerde query's die ertoe leiden dat vergrendelen, nieuwe indexen maken of kolommen toevoegen aan de bestaande index om te voorkomen dat de tabel scant. 

Zie voor meer suggesties [hoe blokkerende problemen die worden veroorzaakt door het escalatiebeleid vergrendeling in SQL Server op te lossen](https://support.microsoft.com/help/323630/how-to-resolve-blocking-problems-that-are-caused-by-lock-escalation-in).

## <a name="increased-maxdop"></a>Verbeterde MAXDOP

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft aan een voorwaarde waarin een uitvoeringsplan gekozen query is meer dan moet deze zijn geparallelliseerd. Het queryoptimalisatieprogramma SQL-Database kan de prestaties van de werkbelastingen verbeteren door het uitvoeren van query's parallel te versnellen dingen waar mogelijk. In sommige gevallen langer parallelle werknemers verwerking van een query wachten op elkaar om te synchroniseren en vergeleken met de dezelfde query wordt uitgevoerd met minder parallelle werknemers, of zelfs in sommige gevallen in vergelijking met een enkele werkthread resultaten samenvoegen.

Het systeem expert analyseert de huidige databaseprestaties in vergelijking met de basislijnperiode. Hiermee bepaalt u of een eerder gestarte query langzamer dan voordat omdat het abonnement van de uitvoering van query is meer dan geparallelliseerd moet wel wordt uitgevoerd.

De MAXDOP-serverconfiguratieoptie op SQL-Database wordt gebruikt om te bepalen hoeveel CPU-kernen kunnen worden gebruikt om dezelfde query parallel uitvoeren. 

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens voert query-hashes met betrekking tot query's waarvan de duur van de uitvoering van omdat ze zijn meer dan ze had moeten geparallelliseerd verhoogd. Het logboek voert ook CXP wachttijden. Deze tijd geeft de tijd die een enkele organisator/coordinator thread (thread 0) wordt gewacht tot alle andere threads voltooid voordat u de resultaten samenvoegen en verder te verplaatsen. Bovendien voert de logboekbestanden met diagnostische gegevens de wachttijden die slecht presterende query's zijn nog in de algehele worden uitgevoerd. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

Eerst optimaliseren of Vereenvoudig complexe query's. Het wordt aangeraden het opsplitsen van lange batch-taken in kleinere eenheden. Bovendien kunt u ervoor dat u indexen ter ondersteuning van uw query's gemaakt. U kunt ook handmatig de maximale graad van parallelle uitvoering (MAXDOP) afdwingen voor een query die is gemarkeerd als het uitvoeren van slechte. Zie voor meer informatie over het configureren van deze bewerking met behulp van T-SQL [configureren van de serverconfiguratieoptie MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

Instellen van de MAXDOP-server met de optie voor nul (0) als een standaardwaarde geeft aan dat SQL-Database alle beschikbare logische CPU-kernen parallel threads kunt voor het uitvoeren van een eenvoudige query uitvoeren. Instelling MAXDOP op één (1) geeft aan dat slechts één core kan worden gebruikt voor het uitvoeren van een enkele query's. Dit betekent dat de parallelle uitvoering is uitgeschakeld in de praktijk. Afhankelijk van de per geval beoordeeld, beschikbare kerngeheugens met de database en diagnostische gegevens vastleggen, kunt u de optie MAXDOP voor het aantal kernen gebruikt voor parallelle queryuitvoering die mogelijk los het probleem in uw geval afstemmen.

## <a name="pagelatch-contention"></a>Pagelatch conflicten

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon prestaties geeft de huidige database werkbelasting prestaties afnemen als gevolg van pagelatch conflicten ten opzichte van de afgelopen zeven dagen werkbelasting basislijn aan.

Vergrendelingen zijn lichtgewicht synchronisatiemechanismen gebruikt door SQL Database om in te schakelen multithreading. Ze garantie voor de consistentie van de structuren in het geheugen met indexen, gegevenspagina's en andere interne structuren.

Er zijn veel soorten vergrendelingen beschikbaar op de SQL-database. Buffer vergrendelingen worden oog op gemak wordt gebruikt om de beveiliging van pagina's in het geheugen in de buffergroep. I/o-vergrendelingen worden gebruikt voor het beveiligen van pagina's nog niet in de buffergroep is geladen. Wanneer gegevens worden geschreven naar of van een pagina in de buffergroep lezen, moet een werkthread eerst verkrijgen van een vergrendeling buffer voor de pagina. Wanneer er geen werkthread probeert te krijgen tot een pagina die nog niet beschikbaar in de buffergroep in het geheugen, wordt een i/o-aanvraag wordt gedaan bij het laden van de vereiste gegevens uit de opslag. Deze reeks gebeurtenissen geeft aan dat een strengere vorm van prestatievermindering.

Conflicten op de pagina's treedt op wanneer meerdere threads gelijktijdig probeert aan te schaffen vergrendelingen op de dezelfde structuur in het geheugen, die resulteert in een verhoogde wachttijd voor het uitvoeren van query's. In het geval van pagelatch i/o-conflicten, wanneer gegevens moeten worden geopend vanuit de opslag, is deze wachttijd nog grotere. Deze kan de prestaties van de werkbelastingen aanzienlijk beïnvloeden. Pagelatch conflicten is het meest voorkomende scenario van threads op elkaar af en de concurrentie voor resources die op meerdere CPU-systemen.

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens levert pagelatch conflicten details. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

Omdat een pagelatch een mechanisme voor interne controle van SQL-Database is, bepaald het automatisch wanneer ze worden gebruikt. Beslissingen voor de toepassing, met inbegrip van schemaontwerp, kunnen invloed hebben op pagelatch gedrag vanwege de deterministische gedrag van vergrendelingen.

Een methode voor het verwerken van vergrendeling conflicten wordt vervangen door een sequentiële Indexsleutel met een niet-aaneengesloten sleutel invoegen gelijkmatig wordt verdeeld over een bereik van de index. Normaal gesproken verdeelt een toonaangevende kolom in de index de belasting evenredig. Een andere methode om te overwegen Tabelpartities is. Het maken van een partitieschema met een berekende kolom in een gepartitioneerde tabel hash is een veelgebruikte manier voor het oplossen van conflicten overmatige vergrendeling. In het geval van pagelatch i/o-conflicten kunt Maak kennis met indexen u dit probleem verhelpen. 

Zie voor meer informatie, [vaststellen en oplossen conflicten op SQL Server vergrendelingen](https://download.microsoft.com/download/B/9/E/B9EDF2CD-1DBF-4954-B81E-82522880A2DC/SQLServerLatchContention.pdf) (downloaden als PDF).

## <a name="missing-index"></a>Ontbrekende Index

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon prestaties geeft de huidige database werkbelasting prestatievermindering in vergelijking met de afgelopen zeven dagen basislijn vanwege een ontbrekende index.

Een index wordt gebruikt om de prestaties van query's te versnellen. Het biedt snelle toegang tot gegevens in een tabel door te verminderen van het aantal pagina's de gegevensset die moet worden bezocht of gescand.

Specifieke query's die verminderde prestaties veroorzaakt worden geïdentificeerd door deze detectie waarvoor maken van indexen nuttig is om de prestaties zijn zou.

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens voert query-hashes voor de query's die zijn geïdentificeerd voor de werkbelasting beïnvloeden. U kunt bouwen indexen voor deze query's. U kunt ook optimaliseren of verwijderen van deze query's als ze niet vereist. Een goede prestaties practice is om te voorkomen dat het opvragen van gegevens die u niet gebruikt.

> [!TIP]
> Wist u dat de best presterende indexen voor uw databases automatisch door SQL Database ingebouwde intelligentie kunt beheren?
>
> Voor de optimalisatie van de continue prestaties van SQL-Database, wordt aangeraden dat u inschakelt [SQL Database automatisch afstemmen](sql-database-automatic-tuning.md). Deze unieke functie van SQL Database ingebouwde intelligentie continu bewaakt uw SQL-database en automatisch afgestemd en indexen voor uw databases.
>

## <a name="new-query"></a>Nieuwe query

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon prestaties geeft aan dat een nieuwe query wordt gedetecteerd die slecht presteert en die betrekking hebben op de prestaties van de werkbelastingen in vergelijking met de basislijn van prestaties van zeven dagen.

Soms een goed presterende query schrijven, kan een uitdaging zijn. Zie voor meer informatie over het schrijven van query's [schrijven van SQL-query's](https://msdn.microsoft.com/library/bb264565.aspx). Zie voor het optimaliseren van prestaties van de bestaande query's, [Query afstemmen](https://msdn.microsoft.com/library/ms176005.aspx).

### <a name="troubleshooting"></a>Problemen oplossen

Diagnostische gegevens over de logboekgegevens uitvoer maximaal twee nieuwe de meeste CPU verbruikt query's, met inbegrip van de query-hashes. Omdat de gedetecteerde query is van invloed op de prestaties van de werkbelastingen, kunt u uw query te optimaliseren. Het wordt aangeraden om op te halen, alleen gegevens die u wilt gebruiken. We raden u ook aan met behulp van query's met een WHERE-component. Ook wordt aangeraden dat u complexe query's te vereenvoudigen en splits in kleinere query's. Een andere goede gewoonte is om op te splitsen in kleinere query's voor batch grote batch-query's. Introductie van indexen voor de nieuwe query's is meestal een goede gewoonte om dit probleem te verhelpen.

Overweeg het gebruik van [Azure SQL Database Query Performance Insight](sql-database-query-performance.md).

## <a name="increased-wait-statistic"></a>Verbeterde wacht statistiek

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft aan dat een verslechtering van de werkbelasting prestaties die slecht presterende query's worden geïdentificeerd, vergeleken met de afgelopen zeven dagen werkbelasting basislijn.

In dit geval wordt het systeem kan niet de slecht presterende query's onder andere categorieën detecteerbare standaardprestaties classificeren, maar de wachttijd statistieken die verantwoordelijk is voor de regressie gevonden. Daarom wordt deze beschouwt als query's met *verhoogd wacht statistieken*, waar ook de wachttijd statistieken die verantwoordelijk is voor de regressie wordt weergegeven. 

### <a name="troubleshooting"></a>Problemen oplossen

De uitvoer-informatie over verbeterde wacht tijd-gegevens en query-hashes van de betrokken query's-diagnoselogboek.

Omdat het systeem kan niet met succes de hoofdoorzaak van slecht presterende query's identificeren, wordt de diagnostische gegevens in een goed uitgangspunt voor het oplossen van handmatige. U kunt de prestaties van deze query's optimaliseren. Er is een goede gewoonte om op te halen, alleen gegevens die u wilt gebruiken en vereenvoudigen en complexe query's in kleinere opdelen. 

Zie voor meer informatie over het optimaliseren van prestaties van query's [Query afstemmen](https://msdn.microsoft.com/library/ms176005.aspx).

## <a name="tempdb-contention"></a>TempDB conflicten

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft aan dat de prestatieniveau van een database waarin een knelpunt van threads probeert te krijgen tot bronnen van tempDB bestaat. (Dit probleem niet i/o die betrekking hebben.) De typisch scenario voor dit probleem is honderden gelijktijdige query's dat alle maken, gebruiken en zet vervolgens neer kleine tempDB-tabellen. Er is gedetecteerd dat het aantal gelijktijdige query's met behulp van de dezelfde tempDB-tabellen met voldoende statistisch significante invloed heeft op de prestaties van de database is in vergelijking met de basislijn van de afgelopen zeven dagen prestaties worden verhoogd.

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens levert tempDB conflicten details. Gebruik de informatie als startpunt voor het oplossen van problemen. Er zijn twee dingen die u kunt een om te verlichten van dit soort conflicten en verhoogt de doorvoer van de algemene workload: U kunt stoppen met behulp van de tijdelijke tabellen. U kunt ook tabellen geoptimaliseerd voor geheugen gebruiken. 

Zie voor meer informatie, [Inleiding tot tabellen geoptimaliseerd voor geheugen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables). 

## <a name="elastic-pool-dtu-shortage"></a>Elastische Pool-DTU-tekort

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft aan dat een verslechtering van de in de huidige database werkbelastingsprestaties in vergelijking met de basislijn van de afgelopen zeven dagen. Het is vanwege het gebrek aan beschikbare dtu's in de elastische pool van uw abonnement. 

Resources voor SQL Database worden gewoonlijk aangeduid als [DTU-bronnen](sql-database-service-tiers.md#dtu-based-purchasing-model), die bestaan uit een samengestelde meting van CPU- en i/o-resources voor (gegevens en transactielogboeken logboek-IO). [Elastische groep van Azure-resources](sql-database-elastic-pool.md) worden gebruikt als een pool van beschikbare eDTU-resources die zijn gedeeld tussen meerdere databases voor het schalen van toepassing. Wanneer beschikbaar eDTU-resources in de elastische pool niet voldoende voor de ondersteuning van alle databases in de groep, wordt een elastische pool DTU tekort prestatieprobleem gedetecteerd door het systeem.

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens levert informatie over de elastische pool, geeft een lijst van de bovenste DTU verbruikt databases en levert een percentage van de pool-DTU gebruikt door de database boven verbruikt.

Omdat dit prestatieniveau is gekoppeld aan meerdere databases met behulp van dezelfde groep van edtu's in de elastische pool, zijn de stappen voor probleemoplossing gericht op de bovenste DTU verbruikt databases. U kunt de werkbelasting van de databases boven verbruikt, waaronder de optimalisatie van de query top verbruikt's op deze databases verminderen. Ook kunt u ervoor zorgen dat u gegevens die u niet gebruikt worden niet opvragen. Een andere benadering is het optimaliseren van toepassingen met behulp van de bovenste DTU verbruikt databases en distribueren van de werkbelasting tussen meerdere databases.

Als vermindering en optimalisatie van de huidige werkbelasting op uw belangrijkste DTU verbruikt databases niet mogelijk is, kunt u overwegen de elastische pool prijscategorie verhogen. Dergelijke verhoging resultaten in de toename van het beschikbare aantal dtu's in de elastische pool.

## <a name="plan-regression"></a>Regressie plannen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft aan dat een voorwaarde in welke SQL-Database maakt gebruik van suboptimale queryplan kan worden uitgevoerd. Het plan suboptimale is doorgaans zorgt ervoor dat verbeterde queryuitvoering, die leiden tot langere wachttijden voor de huidige en andere query's.

De SQL-database, bepaalt het queryplan kan worden uitgevoerd met de minste kosten verbonden aan het uitvoeren van een query's. Als het type wijziging van query's en workloads, de bestaande abonnementen zijn soms niet meer efficiënte of misschien SQL-Database een goede beoordeling niet hebt gedaan. Als een kwestie van correctie kunnen queryplannen uitvoering worden handmatig afgedwongen.

Dit patroon detecteerbare prestaties combineert drie andere gevallen van regressie plan: nieuw plan regressie, oude plan regressie en bestaande workload-abonnementen die zijn gewijzigd. Het specifieke type regressie plan dat is opgetreden is opgegeven in de *details* eigenschap in de logboekbestanden met diagnostische gegevens.

Het nieuwe abonnement regressie voorwaarde verwijst naar een status waarin SQL-Database wordt uitgevoerd van een nieuwe query uitvoeringsplan die niet zo efficiënt als het oude plan. De oude plan regressie voorwaarde verwijst naar de status als SQL-Database stapt over van het gebruik van een nieuwe, efficiënter-abonnement op het oude plan, dat niet zo efficiënt als het nieuwe abonnement. De bestaande abonnementen gewijzigd werkbelasting regressie verwijst naar de status waarin de oude en de nieuwe plannen continu alternatieve, met de balans meer gaat naar het plan slechte prestaties.

Zie voor meer informatie over planning regressie [wat plan regressie in SQL Server is?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/06/09/what-is-plan-regression-in-sql-server/). 

### <a name="troubleshooting"></a>Problemen oplossen

De logboekbestanden met diagnostische gegevens levert de query-hashes, goede plan-ID, ongeldige abonnement-ID en query-id's. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

U kunt analyseren welk plan is het beter uitvoeren voor uw specifieke query's die u met de opgegeven query-hashes identificeren kunt. Nadat u hebt vastgesteld welke werkt dit plan beter voor uw query's, kunt u het handmatig afdwingt. 

Zie voor meer informatie, [informatie over hoe SQL-Server voorkomt voor plan regressie](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/04/25/you-shall-not-regress-how-sql-server-2017-prevents-plan-regressions/).

> [!TIP]
> Wist u dat de best presterende query-uitvoering plannen voor uw databases automatisch door SQL Database ingebouwde intelligentie kunt beheren?
>
> Voor de optimalisatie van de continue prestaties van SQL-Database, wordt aangeraden dat u inschakelt [SQL Database automatisch afstemmen](sql-database-automatic-tuning.md). Deze unieke functie van SQL Database ingebouwde intelligentie continu bewaakt uw SQL-database en automatisch afgestemd en best presterende query-uitvoering plannen voor uw databases maakt.
>

## <a name="database-scoped-configuration-value-change"></a>Het wijzigen van de database Scoped Configuration

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft aan een voorwaarde waarin een wijziging in de configuratie van de database-scoped zorgt ervoor dat prestaties regressie die is gedetecteerd in vergelijking met het gedrag van de afgelopen zeven dagen database werkbelasting. Dit patroon geeft aan dat een recente wijziging hebt aangebracht in de configuratie van de database-scoped lijkt niet te worden nuttig is om de databaseprestaties van uw.

Wijzigingen in de database-scoped configuratie kunnen worden ingesteld voor elke afzonderlijke database. Deze configuratie wordt gebruikt op een per geval beoordeeld om de afzonderlijke prestaties van uw database te optimaliseren. De volgende opties kunnen worden geconfigureerd voor elke afzonderlijke database: MAXDOP, LEGACY_CARDINALITY_ESTIMATION, PARAMETER_SNIFFING, QUERY_OPTIMIZER_HOTFIXES en DUIDELIJKE PROCEDURE_CACHE.

### <a name="troubleshooting"></a>Problemen oplossen

De uitvoer databasebereik configuratiewijzigingen die onlangs zijn aangebracht waardoor prestatievermindering in vergelijking met het gedrag van het vorige zeven dagen werkbelasting-diagnoselogboek. U kunt de configuratiewijzigingen in de vorige waarden herstellen. U kunt ook waarde door waarde afstemmen totdat het gewenste prestatieniveau is bereikt. U kunt database-scope-configuratiewaarden kopiëren uit een database vergelijkbaar met voldoende prestaties. Als u niet om op te lossen van de prestaties, terugkeren naar de standaardwaarden van de standaard-SQL-Database en proberen te starten vanaf deze basislijn aanpassen.

Zie voor meer informatie over het optimaliseren van binnen het bereik van database-configuratie en T-SQL-syntaxis voor het wijzigen van de configuratie [Alter database scoped configuration (Transact-SQL)](https://msdn.microsoft.com/library/mt629158.aspx).

## <a name="slow-client"></a>Trage Client

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft zo snel mogelijk verzendt de resultaten van de database aan een voorwaarde waarin de client met behulp van de SQL-database, de uitvoer van de database kan niet gebruiken. Omdat SQL Database is niet in een buffer opslaan van resultaten van de uitgevoerde query's, het vertraagt en wacht op de client gebruiken voor de verzonden query-uitvoer voordat u doorgaat. Dit probleem mogelijk ook betrekking op een netwerk dat is niet voldoende snel genoeg voor het verzenden van uitvoer van de SQL-database naar de client in beslag nemen.

Deze voorwaarde wordt alleen gegenereerd als een regressie prestaties ten opzichte van de afgelopen zeven dagen database werkbelasting gedrag wordt gedetecteerd. Dit probleem wordt gedetecteerd alleen als er een statistisch merkbare prestatievermindering optreedt in vergelijking met vorige prestatiegedrag.

### <a name="troubleshooting"></a>Problemen oplossen

Dit patroon detecteerbare prestaties geeft aan dat een client-side '-voorwaarde. Probleemoplossing is vereist op de client-side '-toepassing of de client-side '-netwerk. De logboekbestanden met diagnostische gegevens levert de query-hashes en wachttijden die lijken te zijn het meest voor de client te gebruiken in de afgelopen twee uur wachten. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

U kunt de prestaties van uw toepassing voor gebruik van deze query's optimaliseren. U kunt ook mogelijk netwerklatentieproblemen overwegen. Omdat het prestatieprobleem degradatie is gebaseerd op wijzigingen in de afgelopen zeven dagen prestaties basislijn, kunt u onderzoeken of deze prestaties regressie-gebeurtenis is veroorzaakt door recente wijzigingen voor toepassing of het netwerk voorwaarde. 

## <a name="pricing-tier-downgrade"></a>Downgrade van laag-prijzen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon detecteerbare prestaties geeft aan een voorwaarde waarin de prijscategorie van uw SQL Database-abonnement Downgrade is uitgevoerd. Er is een afname vanwege vermindering van resources (dtu's) die beschikbaar zijn in de database gedetecteerd in de huidige databaseprestaties in vergelijking met de basislijn van de afgelopen zeven dagen.

Bovendien kan er een voorwaarde waarin de prijscategorie van uw SQL Database-abonnement is een downgrade uitgevoerd en vervolgens een upgrade uitgevoerd naar een hogere laag binnen een korte periode. Detectie van deze tijdelijke prestatievermindering is gegenereerd in het gedeelte details van de logboekbestanden met diagnostische gegevens als een pricing tier downgrade en upgraden.

### <a name="troubleshooting"></a>Problemen oplossen

Als u over uw prijscategorie, en dus het aantal dtu's beschikbaar zijn voor SQL-Database beperkte, en u tevreden met de prestaties bent, is er niets hoeft te doen. Als u uw prijscategorie verminderd en u tevreden met de prestaties van uw SQL-database bent, workloads van uw databases verlagen of vergroot de prijscategorie naar een hoger niveau.

## <a name="recommended-troubleshooting-flow"></a>Aanbevolen oplossen van problemen met flow

 Volg de stroomdiagram voor een aanbevolen aanpak voor het oplossen van prestatieproblemen met behulp van intelligente inzichten.

Intelligent Insights toegang via Azure portal door te gaan naar Azure SQL Analytics. Proberen te vinden van de binnenkomende Prestatiewaarschuwing en selecteer deze. Identificeer wat er gebeurt op de pagina detecties. Bekijk de opgegeven hoofdoorzaakanalyses van het probleem, querytekst, query tijd trends en incidenten ontwikkeling. Poging tot het probleem oplossen door de Intelligent Insights-aanbeveling voor het oplossen van het prestatieprobleem. 

[![Stroomdiagram voor probleemoplossing](./media/sql-database-intelligent-insights/intelligent-insights-troubleshooting-flowchart.png)](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/intelligent-insight/Troubleshoot%20Azure%20SQL%20Database%20performance%20issues%20using%20Intelligent%20Insight.pdf)

> [!TIP]
> Selecteer de stroomdiagram om een PDF-versie te downloaden.

Intelligent Insights moet meestal één uur van de tijd om uit te voeren van de analyses van hoofdoorzaken van het prestatieprobleem. Als u uw probleem niet kan in intelligente inzichten vinden en het is essentieel dat u, gebruikt u de Query Store handmatig de hoofdoorzaak van het prestatieprobleem identificeren. (Deze problemen zijn meestal minder dan een uur oud.) Zie voor meer informatie, [prestaties bewaken met behulp van de Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store).

## <a name="next-steps"></a>Volgende stappen
- Informatie over [Intelligent Insights](sql-database-intelligent-insights.md) concepten.
- Gebruik de [diagnoselogboek voor Intelligent Insights Azure SQL Database performance](sql-database-intelligent-insights-use-diagnostics-log.md).
- Monitor [Azure SQL Database met behulp van Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Meer informatie over het [verzamelen en gebruiken van logboekgegevens van uw Azure-resources](../azure-monitor/platform/diagnostic-logs-overview.md).
