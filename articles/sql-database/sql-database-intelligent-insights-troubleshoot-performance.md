---
title: Oplossen van prestatieproblemen Azure SQL Database met Intelligent Insights | Microsoft Docs
description: Intelligent Insights helpt bij het oplossen van prestatieproblemen met de Azure SQL Database.
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
editor: carlrab
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/25/2017
ms.author: v-daljep
ms.openlocfilehash: cce112929ff2f4fb48c2c6e2ddc2d4eee743b790
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshoot-azure-sql-database-performance-issues-with-intelligent-insights"></a>Azure SQL Database prestatieproblemen met Intelligent Insights oplossen

Deze pagina bevat informatie over Azure SQL Database-prestatieproblemen die worden gedetecteerd door de [Intelligent Insights](sql-database-intelligent-insights.md) database prestatiecontrole melden. Dit logboek diagnostische gegevens kan worden verzonden naar [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage), of een oplossing van derden voor aangepaste DevOps waarschuwingen en rapportages mogelijkheden.

> [!NOTE]
> Zie voor een snelle SQL-Database prestaties probleemoplossingsgids via Intelligent inzichten, de [aanbevolen oplossingen stroom](sql-database-intelligent-insights-troubleshoot-performance.md#recommended-troubleshooting-flow) stroomdiagram in dit document.
>

## <a name="detectable-database-performance-patterns"></a>Waarneembaar database prestatiepatronen

Intelligent Insights detecteert automatisch prestatieproblemen met SQL-Database op basis van de query-uitvoering wachttijden, fouten of time-outs. Deze vervolgens levert prestatiepatronen in het logboek diagnostics gedetecteerde. Waarneembaar prestatiepatronen worden in de volgende tabel samengevat:

| Prestatiepatronen waarneembaar | Details output |
| :------------------- | ------------------- |
| [Limieten bereiken](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) | Verbruik van beschikbare resources (dtu's), werkthreads database of database aanmelding sessies beschikbaar is op het bewaakte abonnement is bereikt-limieten, waardoor prestatieproblemen met de SQL-Database. |
| [Toename van de werkbelasting](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) | Toename van de werkbelasting of continue opeenstapeling van werkbelasting op de database is gedetecteerd, waardoor prestatieproblemen met de SQL-Database. |
| [Geheugendruk](sql-database-intelligent-insights-troubleshoot-performance.md#memory-pressure) | Werknemers die aangevraagd geheugen verleent moeten wachten voor geheugentoewijzingen statistisch aanzienlijke hoeveelheid tijd. Of een hogere opeenstapeling van werknemers die aangevraagd geheugen verleent bestaat, die van invloed op prestaties van de SQL-Database. |
| [Vergrendelen](sql-database-intelligent-insights-troubleshoot-performance.md#locking) | Vergrendelen van overmatige database is gedetecteerd, die van invloed op prestaties van de SQL-Database. |
| [Verbeterde MAXDOP](sql-database-intelligent-insights-troubleshoot-performance.md#increased-maxdop) | De maximale mate van parallelle uitvoering optie (MAXDOP) is gewijzigd en is van invloed op de efficiëntie van de uitvoering van query. |
| [Pagelatch conflicten](sql-database-intelligent-insights-troubleshoot-performance.md#pagelatch-contention) | Aantal conflicten Pagelatch is gedetecteerd die van invloed op prestaties van de SQL-Database. Meerdere threads krijgen tegelijk toegang de gegevens in het geheugen buffer pagina's. Dit leidt tot verbeterde wachttijden, die van invloed op prestaties van de SQL-Database. |
| [Ontbrekende Index](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index) | Een probleem met de ontbrekende index is gedetecteerd die van invloed op prestaties van de SQL-Database. |
| [Nieuwe Query](sql-database-intelligent-insights-troubleshoot-performance.md#new-query) | Een nieuwe query er is gedetecteerd die van invloed op prestaties van de SQL-Database. |
| [Ongebruikelijke wacht statistiek](sql-database-intelligent-insights-troubleshoot-performance.md#unusual-wait-statistic) | Wachttijden ongebruikelijke database er zijn gedetecteerd die van invloed op prestaties van de SQL-Database. |
| [TempDB conflicten](sql-database-intelligent-insights-troubleshoot-performance.md#tempdb-contention) | Meerdere threads probeert te krijgen tot de dezelfde tempDB-bronnen, waardoor een knelpunt dat van invloed op prestaties van de SQL-Database. |
| [DTU tekort aan resources van elastische groep](sql-database-intelligent-insights-troubleshoot-performance.md#elastic-pool-dtu-shortage) | Een gebrek aan beschikbare edtu's in de elastische groep is van invloed op prestaties van de SQL-Database. |
| [Regressie plannen](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression) | Een nieuw plan of een wijziging in de werkbelasting van een bestaand abonnement is gedetecteerd, die van invloed op prestaties van de SQL-Database. |
| [Database-Scoped waarde configuratiewijziging](sql-database-intelligent-insights-troubleshoot-performance.md#database-scoped-configuration-value-change) | Een configuratiewijziging op de database is van invloed op prestaties van de SQL-Database. |
| [Trage Client](sql-database-intelligent-insights-troubleshoot-performance.md#slow-client) | Een trage toepassingsclient dat niet kan gebruiken voor uitvoer van de SQL-Database snel genoeg is gedetecteerd die van invloed op prestaties van de SQL-Database. |
| [Laag Downgrade prijzen](sql-database-intelligent-insights-troubleshoot-performance.md#pricing-tier-downgrade) | Een prijscategorie laag downgrade actie verlaagd beschikbare bronnen die van invloed op prestaties van de SQL-Database. |

> [!TIP]
> Inschakelen voor de optimalisatie van de continue prestaties van SQL-Database, [automatische afstemming van Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-automatic-tuning). Deze unieke functie van de SQL-Database ingebouwde intelligentie continu bewaakt uw SQL-database, automatisch verbetert de prestatie indexen en query uitvoering plan correcties van toepassing is.
>

De volgende sectie beschrijft de eerder vermelde waarneembaar prestatiepatronen in meer detail.

## <a name="reaching-resource-limits"></a>Limieten bereiken

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties combineert prestatieproblemen die gerelateerd zijn aan de beschikbare resource limieten, worker limieten en sessielimieten is bereikt. Nadat u dit probleem wordt gedetecteerd, een beschrijvingsveld van het logboek diagnostics geeft aan of het prestatieprobleem betrekking heeft op de resource, worker of sessielimieten.

Bronnen op de SQL-Database worden gewoonlijk aangeduid als [DTU-bronnen](https://docs.microsoft.com/azure/sql-database/sql-database-what-is-a-dtu). Ze bestaan uit een gecombineerde meting van CPU- en i/o-resources voor (gegevens en transactie logboekregistratie i/o). Het patroon van limieten bereiken wanneer gedetecteerd wordt herkend verslechtering van de query-prestaties wordt veroorzaakt door een van de gemeten limieten bereiken.

De sessie limieten-resource geeft aan dat het aantal beschikbare gelijktijdige aanmeldingen bij de SQL-database. Dit patroon prestaties wordt herkend als het aantal beschikbare gelijktijdige aanmeldingen bij de database hebt bereikt door toepassingen die zijn verbonden met de SQL-databases. Als toepassingen probeert te gebruiken meer sessies dan beschikbaar zijn op een database, wordt de prestaties van query's beïnvloed.

Worker-limieten bereiken is een specifieke aanvraag limieten bereiken omdat beschikbare werknemers worden niet meegeteld in het DTU-gebruik. Worker-limieten voor een database is bereikt, kan de toename van de resource-specifieke wachttijden, wat tot prestatievermindering query leidt veroorzaken.

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert query-hashes van query's die van invloed op de prestaties en het resource-verbruikspercentages. U kunt deze informatie gebruiken als een beginpunt voor het optimaliseren van de werkbelasting van uw database. In het bijzonder, kunt u de query's die invloed hebben op de prestaties achteruitgaan door toe te voegen indexen optimaliseren. Of u toepassingen met een meer zelfs Werkbelastingsverdeling kunt optimaliseren. Als u geen werkbelastingen verminderen of optimalisaties maken bent, kunt u de prijscategorie van uw SQL database-abonnement te verhogen van de hoeveelheid beschikbare bronnen vergroten.

Als u de beschikbare sessiecontext limieten zijn bereikt, kunt u uw toepassingen optimaliseren door het aantal aanmeldingen die zijn aangebracht in de database te verminderen. Als je je niet Verminder het aantal aanmeldingen van uw toepassingen aan de database, kunt u de prijscategorie van uw database te verhogen. Of u kunt splitsen en uw database verplaatsen naar meerdere databases voor een meer evenwichtige Werkbelastingsverdeling.

Zie voor meer suggesties over het oplossen van sessielimieten [omgaan met de grenzen van de SQL-Database maximale aanmeldingen](https://blogs.technet.microsoft.com/latam/2015/06/01/how-to-deal-with-the-limits-of-azure-sql-database-maximum-logins/). Zie voor meer informatie over de beschikbare resource limieten voor de laag van uw abonnement, [limieten voor SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits).

## <a name="workload-increase"></a>Toename van de werkbelasting

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon prestaties identificeert problemen veroorzaakt door een verhoging van de werkbelasting of, in de strengere vorm, een werkbelasting pile-up.

Deze detectie wordt uitgevoerd door een combinatie van verschillende metrische gegevens. De basic metriek gemeten is een toename opsporen in werkbelasting vergeleken met de afgelopen werkbelasting basislijn. De andere vorm van detectie is gebaseerd op een grote toename in actieve werkthreads die groot genoeg is van invloed zijn op de prestaties van query's te meten.

In de vorm ingrijpender is de werkbelasting mogelijk continu opstapelen vanwege het feit dat niet de SQL-database voor het afhandelen van de werkbelasting. Het resultaat is een voortdurend groeiende werkbelastingsgrootte de werkbelasting pile-up voorwaarde is. Als gevolg van deze voorwaarde groeit de wachttijd voor de werkbelasting voor uitvoering. Deze voorwaarde vertegenwoordigt een van de meest ernstige prestatieproblemen met de database. Dit probleem is gedetecteerd door middel van controle van de toename van het aantal werkthreads afgebroken. 

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert het aantal query's waarvan de uitvoering is toegenomen en de query-hash van de query met de grootste bijdrage aan de toename van de werkbelasting. U kunt deze informatie gebruiken als een beginpunt voor het optimaliseren van de werkbelasting. De query die wordt geïdentificeerd als de grootste Inzender aan de werkbelasting verhoging is vooral nuttig als uw beginpunt.

U kunt overwegen voor het distribueren van de werkbelastingen meer evenredig met de database. U kunt de query die van invloed op de prestaties door toe te voegen indexen. U kunt ook uw werkbelasting tussen meerdere databases distribueren. Als deze oplossingen niet mogelijk is, kunt u de prijscategorie van uw SQL database-abonnement te verhogen van de hoeveelheid beschikbare bronnen vergroten.

## <a name="memory-pressure"></a>Geheugendruk

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon van prestaties geeft verslechtering van de huidige databaseprestaties veroorzaakt door geheugendruk of in de vorm ernstige geheugen pile-up voorwaarde, vergeleken met de basislijn voor de afgelopen zeven dagen prestaties.

Geheugendruk geeft aan dat een prestatieniveau waarin zich een groot aantal werkthreads aanvragende geheugen in de SQL-database verleent. Hoog volume zorgt ervoor dat een hoog probleem met het geheugengebruik waarin de SQL-database is efficiënt kan geheugen niet toewijzen aan alle werknemers die deze aanvragen. Een van de meest voorkomende redenen voor dit probleem betrekking heeft op de hoeveelheid geheugen die beschikbaar zijn voor de SQL-database enerzijds. Aan de andere kant een toename van de werkbelasting een zorgt ervoor dat de toename in werkthreads en de geheugendruk.

De strengere vorm van geheugendruk is de pile-up geheugen beschikbaar. Deze voorwaarde geeft aan dat een hoger aantal werkthreads geheugen verleent aanvraagt dan er zijn query's vrijgeven van het geheugen. Dit aantal werkthreads aanvragende geheugen ook verleent mogelijk worden continu verhogen (opstapelen) omdat de SQL database-engine niet kan efficiënt voldoende geheugen toewijzen om te voldoen aan de vraag. De voorwaarde van geheugen pile-up vertegenwoordigt een van de meest ernstige prestatieproblemen met de database.

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert de details van gegevensarchief object geheugen clerk (dat wil zeggen werkthread) gemarkeerd als de hoogste reden voor veel geheugen en relevante tijdstempels. U kunt deze informatie gebruiken als basis voor het oplossen van problemen. 

U kunt optimaliseren of verwijderen van query's die zijn gerelateerd aan de clerks met het hoogste geheugengebruik. Ook kunt u ervoor dat u zijn niet opvragen van gegevens die u niet wilt gebruiken. Het wordt aangeraden gebruik altijd een WHERE-component in uw query's. Bovendien is het raadzaam dat u niet-geclusterde indexen van de gegevens te zoeken in plaats van deze scannen maakt.

U kunt ook de werkbelasting verminderen door optimaliseren of verdelen over meerdere databases. Of u uw werkbelasting tussen meerdere databases kunt distribueren. Als deze oplossingen niet mogelijk is, kunt u de prijscategorie van uw SQL database-abonnement te verhogen van de hoeveelheid geheugenresources beschikbaar zijn voor de database te verhogen.

Zie voor aanvullende suggesties voor probleemoplossing [geheugen verleent meditation: de mysterieuze SQL Server-geheugen consument met veel namen](https://blogs.msdn.microsoft.com/sqlmeditation/2013/01/01/memory-meditation-the-mysterious-sql-server-memory-consumer-with-many-names/).

## <a name="locking"></a>Vergrendelen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon van prestaties geeft verslechtering van de huidige prestaties van de database waarin overmatige database vergrendelen wordt gedetecteerd vergeleken met de basislijn voor de afgelopen zeven dagen prestaties. 

In moderne RDBMS-vergrendeling is essentieel voor het implementeren van meerdere threads systemen waarin de prestaties worden gemaximaliseerd door het uitvoeren van meerdere gelijktijdige werknemers en parallelle databasetransacties waar mogelijk. In deze context vergrendelen verwijst naar de ingebouwde mechanisme waarin slechts één transactie exclusief toegang tot de rijen, pagina's, tabellen en bestanden die vereist zijn en niet van invloed zijn op een andere transactie voor bronnen. Als de transactie die vergrendeld van de resources voor gebruik met hen is voltooid, wordt de vergrendeling van deze bronnen vrijgegeven, kunnen de andere transacties voor toegang tot de vereiste resources. Zie voor meer informatie over het vergrendelen [in de database-engine vergrendelen](https://msdn.microsoft.com/library/ms190615.aspx).

Als de transacties die worden uitgevoerd door de SQL-engine wachten langere perioden voor toegang tot resources die zijn vergrendeld voor gebruik, is deze wachttijd zorgt ervoor dat de vertraging van de prestaties van de workload kan worden uitgevoerd. 

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert vergrendelingsfout details die u als basis voor het oplossen van problemen gebruiken kunt. U kunt de gerapporteerde blokkerende query's, dat wil zeggen, de query's die de vergrendeling prestatievermindering introduceren analyseren en verwijder ze. In sommige gevallen is het mogelijk dat u geslaagd voor de optimalisatie van de blokkering query's.

De eenvoudigste en veiligste manier om het probleem te verhelpen is het transacties kort houden en om de voetafdruk van de vergrendeling van de meest dure query's te verkleinen. U kunt een grote batch van bewerkingen in kleinere bewerkingen opheffen. Het wordt aangeraden om te beperken van de voetafdruk van de vergrendeling query door het maken van de query zo efficiënt mogelijk. Grote scans verminderen omdat ze kans impassen verhogen en nadelige invloed heeft op de algehele prestaties van de database. U kunt maken van nieuwe indexen of kolommen toevoegen aan de bestaande index om te voorkomen dat de tabelscans voor geïdentificeerde query's die ertoe leiden dat vergrendelen. 

Zie voor meer suggesties [blokkerende problemen die worden veroorzaakt door uitbreiding van de vergrendeling in SQL Server oplossen](https://support.microsoft.com/en-us/help/323630/how-to-resolve-blocking-problems-that-are-caused-by-lock-escalation-in).

## <a name="increased-maxdop"></a>Verbeterde MAXDOP

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft aan een voorwaarde waarin een uitvoeringsplan gekozen query is meer dan moet deze zijn geparallelliseerde. SQL-Database queryoptimalisatie kan de prestaties van de workload verbeteren door het uitvoeren van query's parallel te versnellen dingen waar mogelijk. In sommige gevallen besteden parallelle verwerking van een query-medewerkers meer tijd wachten op elkaar om te synchroniseren en resultaten vergeleken met dezelfde query uitvoeren met minder parallelle werknemers, of zelfs in sommige gevallen vergeleken met een enkele werkthread samenvoegen.

Het systeem expert analyseert de huidige databaseprestaties in vergelijking met de basislijnperiode. Hiermee wordt bepaald of een eerder gestarte query trager dan voordat omdat het uitvoeringsplan query is meer dan geparallelliseerde moet wordt uitgevoerd.

De MAXDOP serverconfiguratieoptie op SQL-Database wordt gebruikt om te bepalen hoeveel CPU-kernen kunnen worden gebruikt voor dezelfde query parallel uitvoeren. 

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert query-hashes die betrekking hebben op query's waarvoor de duur van de uitvoering van verhoogd omdat ze zijn meer dan ze had moeten zijn geparallelliseerde. Het logboek wordt ook de wachttijden CXP uitvoer. Deze tijd geeft de tijd die een enkele organisator/coordinator thread (thread 0) tot andere threads wacht worden voltooid voordat het samenvoegen van de resultaten en de feitelijke. Het logboek diagnostische uitvoer bovendien de wachttijden die slecht presterende query's bij het uitvoeren van algemene stond. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

Eerst optimaliseren of complexe query's vereenvoudigen. Het wordt aangeraden om op te lang batchtaken in kleinere delen. Bovendien kunt u ervoor dat u indexen ter ondersteuning van uw query's gemaakt. U kunt ook handmatig de maximale mate van parallelle uitvoering (MAXDOP) afdwingen voor een query die is gemarkeerd als slechte uitvoeren. Zie voor meer informatie over het configureren van deze bewerking via de T-SQL [configureren van de serverconfiguratieoptie MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

Het instellen van de server MAXDOP configuratieoptie op nul (0) als een standaardwaarde geeft aan dat SQL-Database alle beschikbare logische CPU-kernen parallelize threads kunt voor het uitvoeren van één query. Instelling MAXDOP op één (1) geeft aan dat slechts één core kan worden gebruikt voor een enkele queryuitvoering. Dit betekent dat de parallelle uitvoering is uitgeschakeld in de praktijk. Afhankelijk van de basis per geval beschikbaar kernen aan de database en diagnostische gegevens vastleggen, kunt u de optie MAXDOP voor het aantal kernen gebruikt voor parallelle queryuitvoering die mogelijk los het probleem in uw geval afstemmen.

## <a name="pagelatch-contention"></a>Pagelatch conflicten

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon van prestaties geeft aan dat de huidige database werkbelasting prestaties achteruitgaan als gevolg van conflicten van pagelatch ten opzichte van de afgelopen zeven dagen werkbelasting basislijn.

Vergrendelingen zijn lightweight synchronisatiemechanismen die zijn gebruikt door SQL-Database om in te schakelen multithreading. Ze garanderen consistentie van de structuren in het geheugen met indexen, data-pagina's en andere interne structuren.

Er zijn veel verschillende soorten grendels zijn beschikbaar op de SQL-database. Buffer vergrendelingen worden voor eenvoud toepassing wordt gebruikt voor beveiliging van pagina's in het geheugen in de buffergroep. I/o-vergrendelingen worden gebruikt voor het beveiligen van pagina's nog niet zijn geladen in de buffergroep. Wanneer gegevens worden geschreven naar of van een pagina in de buffergroep lezen, moet een werkthread eerst verkrijgen van een vergrendeling buffer voor de pagina. Wanneer een werkthread probeert te krijgen tot een pagina die nog niet beschikbaar in de buffergroep in het geheugen, wordt een i/o-aanvraag verzonden naar de vereiste gegevens uit de opslag laden. Deze reeks gebeurtenissen geeft aan dat een ernstige vorm van verminderde prestaties.

Aantal conflicten op de pagina's treedt op wanneer meerdere threads tegelijk probeert te verkrijgen van vergrendelingen op dezelfde in-memory structuur, waardoor een verhoogde wachttijd voor het uitvoeren van de query. In het geval van pagelatch i/o-conflicten, wanneer gegevens worden geopend vanaf opslag, moet deze wachttijd is nog groter. Het kan werkbelasting prestaties aanzienlijk beïnvloeden. Pagelatch conflicten is het meest voorkomende scenario threads wachten op elkaar en concurreren voor bronnen op meerdere CPU-systemen.

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert pagelatch belemmering vormt voor meer informatie. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

Omdat een pagelatch een mechanisme voor interne controle van SQL-Database is, wordt automatisch bepaald wanneer ze te gebruiken. Beslissingen voor de toepassing, met inbegrip van het schemaontwerp van het kunnen beïnvloeden pagelatch vanwege het deterministische gedrag van grendels zijn.

Een methode voor het verwerken van de vergrendeling conflicten is een sequentiële Indexsleutel vervangen door een niet-aaneengesloten sleutel voegt gelijkmatig wordt verdeeld over een indexbereik. Normaal gesproken distribueert een toonaangevende kolom in de index de werkbelasting proportioneel. Een andere methode rekening houden met het partitioneren van tabel is. Maken van een partitieschema met een berekende kolom in een gepartitioneerde tabel hash is een algemene benadering voor het oplossen van conflicten overmatige vergrendeling. In het geval van pagelatch i/o-conflicten kunt introductie van indexen u dit probleem te verhelpen. 

Zie voor meer informatie [diagnosticeren en oplossen conflicten op SQL Server vergrendelingen](http://download.microsoft.com/download/B/9/E/B9EDF2CD-1DBF-4954-B81E-82522880A2DC/SQLServerLatchContention.pdf) (download PDF).

## <a name="missing-index"></a>Ontbrekende Index

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon van prestaties geeft aan dat de huidige database werkbelasting verminderde prestaties ten opzichte van de afgelopen zeven dagen basislijn vanwege een ontbrekende index.

Een index wordt gebruikt om de prestaties van query's te versnellen. Deze snelle toegang biedt tot tabelgegevens vermindert het aantal pagina's de gegevensset die moeten worden bezocht of gescand.

Specifieke query's die verminderde prestaties veroorzaakt worden geïdentificeerd door deze detectie waarvoor maken van indexen nuttig om de prestaties zijn zou.

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert query hashes voor de query's die zijn geïdentificeerd om de prestaties van de workload te beïnvloeden. U kunt de indexen voor deze query's opbouwen. U kunt ook optimaliseren of verwijderen van deze query's als ze niet vereist. Er is een goede prestaties practice om te voorkomen dat een query die u niet gebruikt.

> [!TIP]
> Wist u dat SQL-Database ingebouwde intelligentie automatisch de best presterende indexen voor uw databases kunt beheren?
>
> Voor de optimalisatie van de continue prestaties van SQL-Database, het is raadzaam dat u inschakelt [automatische afstemming van SQL-Database](sql-database-automatic-tuning.md). Deze unieke functie van de SQL-Database ingebouwde intelligentie continu bewaakt de SQL-database en automatisch verbetert de prestatie en maakt indexen voor uw databases.
>

## <a name="new-query"></a>Nieuwe query

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon van prestaties geeft aan dat een nieuwe query wordt gedetecteerd die is slecht uitvoeren en de prestaties van de werkbelasting in vergelijking met de basislijn voor de zeven dagen prestaties beïnvloeden.

Soms een goed presterende query schrijven, kan een uitdaging zijn. Zie voor meer informatie over het schrijven van query's [schrijven van SQL-query's](https://msdn.microsoft.com/library/bb264565.aspx). Bestaande om queryprestaties te optimaliseren, Zie [Query afstemmen](https://msdn.microsoft.com/library/ms176005.aspx).

### <a name="troubleshooting"></a>Problemen oplossen

De diagnostische gegevens logboekgegevens uitvoer maximaal twee nieuwe meeste CPU verbruikt query's, met inbegrip van de query-hashes. Omdat de gedetecteerde query is van invloed op de prestaties van de werkbelasting, kunt u uw query te optimaliseren. Het wordt aangeraden om op te halen, alleen de gegevens die u wilt gebruiken. Bovendien aangeraden om het gebruik van query's met een WHERE-component. Ook wordt aangeraden dat u complexe query's vereenvoudigen en in kleinere query's onderverdelen. Een andere aanbevolen procedure is om op te splitsen grote batch-query's in kleinere query's voor batch. Introductie van indexen voor de nieuwe query's is meestal een goede gewoonte om dit probleem te verhelpen.

Overweeg het gebruik van [Azure SQL Database Query Performance Insight](sql-database-query-performance.md).

## <a name="unusual-wait-statistic"></a>Ongebruikelijke wacht statistiek

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft aan dat een verslechtering van de werkbelasting prestaties waarin slecht presterende query's worden geïdentificeerd aan de afgelopen zeven dagen werkbelasting basislijn vergeleken.

In dit geval wordt het systeem kan niet de slecht presterende query's onder andere categorieën standard waarneembaar prestaties classificeren, maar de wachttijd statistieken die verantwoordelijk is voor de regressie gevonden. Daarom wordt beschouwd als onderdelen van query's met *ongebruikelijke wacht statistieken*, waarbij de statistiek ongebruikelijke wacht die verantwoordelijk is voor de regressie is ook beschikbaar gemaakt. 

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert informatie over ongebruikelijke wacht tijd details, query-hashes van de betrokken query's en de wachttijden.

Omdat de hoofdoorzaak van slecht presterende query's kan niet met succes door het systeem worden geïdentificeerd, is de diagnostische gegevens een goed uitgangspunt voor het oplossen van handmatige. U kunt de prestaties van deze query's optimaliseren. Het wordt aangeraden alleen gegevens die u wilt gebruiken en vereenvoudigen en complexe query's in kleinere eenheden uitgesplitst ophaalt. 

Zie voor meer informatie over het optimaliseren van de prestaties van query's [Query afstemmen](https://msdn.microsoft.com/library/ms176005.aspx).

## <a name="tempdb-contention"></a>TempDB conflicten

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft aan dat een database prestatieniveau waarin een knelpunt threads probeert te krijgen tot bronnen van tempDB bestaat. (Deze voorwaarde niet i/o gerelateerde.) De typisch scenario voor dit probleem is honderden gelijktijdige query's alle maken, gebruiken, en zet kleine tempDB-tabellen. Het systeem gedetecteerd dat het aantal gelijktijdige query's met behulp van dezelfde tempDB-tabellen verhoogd met voldoende statistische significantie databaseprestaties vergeleken met de basislijn voor de afgelopen zeven dagen prestaties beïnvloeden.

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek diagnostische uitvoer tempDB belemmering vormt voor meer informatie. Gebruik de informatie als startpunt voor het oplossen van problemen. Er zijn twee dingen die u voor het verlichten van dit soort conflicten en verhoogt de doorvoer van de algehele werkbelasting kunt oefenen: U kunt stoppen met behulp van de tijdelijke tabellen. U kunt ook tabellen geoptimaliseerd voor geheugen gebruiken. 

Zie voor meer informatie [Inleiding tot tabellen geoptimaliseerd voor geheugen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables). 

## <a name="elastic-pool-dtu-shortage"></a>DTU tekort aan resources van elastische groep

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft aan dat een verslechtering van de huidige database werkbelasting prestaties ten opzichte van de afgelopen zeven dagen basislijn. Het is vanwege het gebrek aan beschikbare dtu's in de elastische pool van uw abonnement. 

Bronnen op de SQL-Database worden gewoonlijk aangeduid als [DTU-bronnen](sql-database-what-is-a-dtu.md), die bestaan uit een gecombineerde meting van CPU- en i/o-resources voor (gegevens en transactie logboekregistratie i/o). [Azure elastische groep resources](sql-database-elastic-pool.md) worden gebruikt als een groep beschikbaar eDTU-resources die zijn gedeeld tussen meerdere databases voor het schalen van toepassing. Wanneer de beschikbare eDTU-resources in uw elastische pool niet voldoende voor de ondersteuning van alle databases in de groep, wordt een prestatieprobleem elastische pool DTU tekort gedetecteerd door het systeem.

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert informatie over de elastische groep, ziet u de bovenste DTU verbruikende databases en een percentage van de groep DTU gebruikt door de database boven verbruikende.

Omdat deze prestatieniveau betrekking heeft op meerdere databases met behulp van dezelfde groep edtu's in de elastische groep, wordt de stappen voor probleemoplossing zich richten op de bovenste DTU verbruikende databases. U kunt de werkbelasting van de databases boven verbruikende waaronder optimalisatie van de query top verbruikende's op die databases verminderen. Ook kunt u ervoor zorgen dat u de gegevens die u niet gebruikt worden niet opvragen. Een andere manier is het optimaliseren van toepassingen met behulp van de bovenste DTU verbruikende databases en distribueren van de werkbelasting tussen meerdere databases.

Als vermindering en optimalisatie van de huidige werkbelasting op de bovenste DTU verbruikende databases niet mogelijk is, kunt u uw elastische pool prijscategorie verhogen. Dergelijke verhoging resulteert in de toename van het beschikbare aantal dtu's in de elastische groep.

## <a name="plan-regression"></a>Regressie plannen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft aan een voorwaarde in welke SQL-Database maakt gebruik van suboptimale queryplan kan worden uitgevoerd. Het plan suboptimale wordt doorgaans toegenomen queryuitvoering, wat leidt tot langere wachttijden voor de huidige en de andere query's.

De SQL-database bepaalt het queryplan kan worden uitgevoerd met de minste kosten om een query kan worden uitgevoerd. Soms de bestaande abonnementen niet langer efficiënt zijn als het type van query's en werkbelastingen wijzigen, of misschien een goede beoordeling geen wijzigingen hebt aangebracht in SQL-Database. Als een kwestie van correctie kunnen queryplannen uitvoering worden handmatig afgedwongen.

Dit patroon waarneembaar prestaties combineert drie verschillende gevallen plan regressie: nieuwe plan regressie, oude plan regressie en werkbelasting met bestaande abonnementen die zijn gewijzigd. Het gebruikte type regressie plan dat is opgetreden vindt u in de *details* eigenschap in het logboek voor diagnostische gegevens.

De nieuwe plan regressie voorwaarde verwijst naar een status waarin SQL-Database wordt uitgevoerd van een nieuwe query uitvoeringsplan die niet zo efficiënt het oude plan. De oude plan regressie voorwaarde verwijst naar de status als SQL-Database verandert van het gebruik van een nieuwe, efficiënter plan aan het oude plan, hetgeen niet zo efficiënt het nieuwe plan. De bestaande abonnementen gewijzigd werkbelasting regressie verwijst naar de staat waarin de oude en nieuwe plannen continu alternatieve, met het saldo meer gaat naar het plan slecht presterende.

Zie voor meer informatie over plan regressies [wat plan regressie in SQL Server is?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/06/09/what-is-plan-regression-in-sql-server/). 

### <a name="troubleshooting"></a>Problemen oplossen

Het logboek voor diagnostische gegevens levert de query-hashes, goed plan-ID, onjuiste plan-ID en query-id's. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

U kunt analyseren welke plan is beter uitvoeren voor uw specifieke query's die u met de opgegeven query-hashes identificeren kunt. Nadat u hebt bepaald welke plan is beter geschikt voor uw query's, kunt u het handmatig afdwingt. 

Zie voor meer informatie [meer informatie over hoe SQL Server wordt voorkomen dat plan regressies](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/04/25/you-shall-not-regress-how-sql-server-2017-prevents-plan-regressions/).

> [!TIP]
> Wist u dat SQL-Database ingebouwde intelligentie automatisch de best presterende query uitvoering plannen voor uw databases kunt beheren?
>
> Voor de optimalisatie van de continue prestaties van SQL-Database, het is raadzaam dat u inschakelt [automatische afstemming van SQL-Database](sql-database-automatic-tuning.md). Deze unieke functie van de SQL-Database ingebouwde intelligentie continu bewaakt de SQL-database en automatisch verbetert de prestatie en maakt best presterende query uitvoering plannen voor uw databases.
>

## <a name="database-scoped-configuration-value-change"></a>Database-Scoped waarde configuratiewijziging

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft aan een voorwaarde waarin een wijziging in de configuratie van de database-scoped prestaties regressie die is gedetecteerd in vergelijking met de afgelopen zeven dagen database werkbelasting gedrag veroorzaakt. Dit patroon geeft aan dat een recente wijziging in de configuratie van de database-scoped lijkt niet te handig zijn om de databaseprestaties van uw.

Wijzigingen in de database-scoped configuratie kunnen worden ingesteld voor elke afzonderlijke database. Deze configuratie wordt gebruikt door geval om de afzonderlijke prestaties van uw database te optimaliseren. De volgende opties kunnen worden geconfigureerd voor elke afzonderlijke database: MAXDOP, LEGACY_CARDINALITY_ESTIMATION PARAMETER_SNIFFING, QUERY_OPTIMIZER_HOTFIXES en DUIDELIJKE PROCEDURE_CACHE.

### <a name="troubleshooting"></a>Problemen oplossen

De diagnostische gegevens Meld uitvoer database-scoped configuratiewijzigingen die onlangs zijn aangebracht die verminderde prestaties in vergelijking met het vorige zeven dagen werkbelasting gedrag veroorzaakt. U kunt wijzigingen in de configuratie op de vorige waarden herstellen. U kunt ook waarde door waarde afstemmen tot het gewenste prestatieniveau is bereikt. U kunt database bereik configuratiewaarden kopiëren uit een database vergelijkbaar met voldoende prestaties. Als u zich niet kunt oplossen met de prestaties, terugkeren naar de standaardwaarden van de standaard SQL-Database en proberen te starten vanaf deze basislijn aanpassen.

Zie voor meer informatie over het optimaliseren van de database-scoped configuratie en T-SQL-syntaxis voor het wijzigen van de configuratie [Alter database-scoped configuration (Transact-SQL)](https://msdn.microsoft.com/en-us/library/mt629158.aspx).

## <a name="slow-client"></a>Trage Client

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft snel verzendt de resultaten van de database aan een voorwaarde waarin de client met behulp van de SQL-database de uitvoer van de database kan niet gebruiken. Omdat SQL-Database is niet in een buffer opslaan van resultaten van de uitgevoerde query's, vertraagt en wordt gewacht op de client gebruiken voor de uitvoer van de query verzonden voordat u doorgaat. Dit probleem mogelijk ook betrekking op een netwerk dat niet voldoende snel genoeg voor het verzenden van uitvoer van de SQL-database naar de client in beslag neemt.

Deze voorwaarde wordt alleen gegenereerd als een regressie prestaties ten opzichte van het gedrag van de afgelopen zeven dagen database werkbelasting is gedetecteerd. Dit probleem is gedetecteerd dat alleen als een afname van statistisch aanzienlijke prestaties in vergelijking met vorige prestatiegedrag.

### <a name="troubleshooting"></a>Problemen oplossen

Dit patroon waarneembaar prestaties geeft aan dat een client-side '-voorwaarde. Het oplossen van problemen is vereist bij de client-side '-toepassing of het netwerk van de clientzijde. Het logboek voor diagnostische gegevens levert de query-hashes en wachttijden die lijken te zijn de meest voor de client te gebruiken in de afgelopen twee uur wachten. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

U kunt de prestaties van uw toepassing voor het gebruik van deze query's optimaliseren. U kunt ook mogelijk netwerklatentieproblemen overwegen. Omdat het prestatieprobleem vermindering is gebaseerd op een wijziging in de afgelopen zeven dagen prestaties basislijn, kunt u onderzoeken of recente wijzigingen voor toepassings- of voorwaarde deze prestaties regressie gebeurtenis veroorzaakt. 

## <a name="pricing-tier-downgrade"></a>Laag Downgrade prijzen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit patroon waarneembaar prestaties geeft aan een voorwaarde waarin de prijscategorie van uw SQL-Database-abonnement is verlaagd. Vanwege de vermindering van de resources (dtu's) beschikbaar voor de database, het systeem gedetecteerd van een afname in de huidige databaseprestaties in vergelijking met de afgelopen zeven dagen basislijn.

Bovendien kan er een voorwaarde waarin de prijscategorie van uw SQL-Database-abonnement is een downgrade uitgevoerd en vervolgens een upgrade uitgevoerd naar een hogere laag binnen een korte periode. Detectie van deze tijdelijke prestatievermindering wordt gegenereerd in het gedeelte details van het logboek diagnostische gegevens als een prijscategorie laag downgrade en upgrade.

### <a name="troubleshooting"></a>Problemen oplossen

Als u uw prijscategorie en dus het aantal dtu's beschikbaar zijn voor SQL-Database verminderd, en u tevreden met de prestaties bent, is er niets hoeft te doen. Als u uw prijscategorie verminderd, en u ontevreden met de prestaties van uw SQL-database bent, of verlaag de werkbelasting van uw database of vergroot de prijscategorie naar een hoger niveau.

## <a name="recommended-troubleshooting-flow"></a>Aanbevolen oplossingen stroom

 Volg het stroomdiagram voor een aanbevolen aanpak voor het oplossen van prestatieproblemen met behulp van Intelligent Insights.

Intelligent Insights toegang via de Azure portal door te gaan met Azure SQL Analytics. Proberen te vinden van de binnenkomende Prestatiewaarschuwing en selecteer deze. Bepalen wat er gebeurt op de pagina detecties. De analyse van de opgegeven hoofdmap oorzaak van het probleem, querytekst, query tijd trends en incident evolutie zien. Poging tot het probleem oplossen door de aanbeveling Intelligent Insights voor het oplossen van het prestatieprobleem. 

[![Stroomdiagram voor probleemoplossing](./media/sql-database-intelligent-insights/intelligent-insights-troubleshooting-flowchart.png)](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/intelligent-insight/Troubleshoot%20Azure%20SQL%20Database%20performance%20issues%20using%20Intelligent%20Insight.pdf)

> [!TIP]
> Selecteer het stroomdiagram voor het downloaden van een PDF-versie.

Intelligent inzicht moeten gewoonlijk één uur van de tijd voor de analyse van de hoofdmap oorzaak van het prestatieprobleem. Als u uw probleem in Intelligent Insights vinden kunt en het is essentieel voor u, gebruikt u het Queryarchief handmatig identificeren de hoofdoorzaak van het prestatieprobleem. (Deze problemen zijn meestal minder dan een uur oud.) Zie voor meer informatie [prestaties bewaken met behulp van de queryopslag](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Intelligent Insights](sql-database-intelligent-insights.md) concepten.
- Gebruik de [Intelligent Insights Azure SQL Database diagnostics prestatielogboek](sql-database-intelligent-insights-use-diagnostics-log.md).
- Monitor [Azure SQL Database met behulp van Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Informatie over het [verzamelen en gebruiken van de logboekgegevens van uw Azure-resources](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).
