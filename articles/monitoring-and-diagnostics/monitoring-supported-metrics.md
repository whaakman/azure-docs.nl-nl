---
title: Azure Monitor metrieken - ondersteunde metrische gegevens per resourcetype | Microsoft Docs
description: Lijst met metrische gegevens beschikbaar zijn voor elk resourcetype met Azure-Monitor.
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: ancav
ms.openlocfilehash: 05830547a5b8a24a59571edf6dd44d101b660189
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Ondersteunde metrische gegevens met Azure-Monitor
Azure biedt verschillende manieren om te communiceren met metrische gegevens, inclusief grafieken ze in de portal of opvragen ze toegang hebben tot deze via de REST-API met PowerShell of CLI. Hieronder volgt een volledige lijst met alle metrische gegevens op dit moment met metrische gegevens van de Monitor van het Azure-pipeline.

> [!NOTE]
> Het is mogelijk dat andere metrische gegevens beschikbaar zijn in de portal of met oudere API's. Deze lijst bevat alleen beschikbaar via de geconsolideerde Azure Monitor metrische pijplijn metrische gegevens. Zoeken naar en toegang hebben tot de metrische gegevens met dimensies gebruik de [2017-05-01-preview-api-versie](https://docs.microsoft.com/en-us/rest/api/monitor/metricdefinitions)
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|qpu_metric|QPU|Count|Gemiddelde|QPU. Bereik 0-100 voor S1, 0-200 voor S2 en 0-400 voor S4|Er zijn geen dimensies|
|memory_metric|Geheugen|Bytes|Gemiddelde|Geheugen. Bereik van 0-25 GB voor S1, 0-50 GB voor S2 en 0-100 GB voor S4|Er zijn geen dimensies|
|TotalConnectionRequests|Totaal aantal verbindingsaanvragen|Count|Gemiddelde|Totaal aantal verbindingsaanvragen. Dit zijn ontvangsten.|Er zijn geen dimensies|
|SuccessfullConnectionsPerSec|Geslaagde verbindingen Per seconde|CountPerSecond|Gemiddelde|Verhouding van geslaagde verbinding voltooiingen.|Er zijn geen dimensies|
|TotalConnectionFailures|Totaal aantal verbindingsfouten|Aantal|Gemiddelde|Totaal aantal mislukte verbindingspogingen.|Er zijn geen dimensies|
|CurrentUserSessions|Huidige gebruikerssessies|Count|Gemiddelde|Huidige aantal gebruikerssessies.|Er zijn geen dimensies|
|QueryPoolBusyThreads|Query groepen bezet Threads|Count|Gemiddelde|Het aantal bezet threads in de query-thread-groep.|Er zijn geen dimensies|
|CommandPoolJobQueueLength|Opdracht taak wachtrijlengte|Count|Gemiddelde|Het aantal taken in de wachtrij van de threadgroep opdracht.|Er zijn geen dimensies|
|ProcessingPoolJobQueueLength|Wachtrijlengte taak verwerken|Count|Gemiddelde|Het aantal niet-I/O-taken in de wachtrij van de thread-pool.|Er zijn geen dimensies|
|CurrentConnections|Verbinding: Actieve verbindingen|Count|Gemiddelde|Huidig aantal verbindingen van clients tot stand gebracht.|Er zijn geen dimensies|
|CleanerCurrentPrice|Geheugen: Schonere huidige prijs|Count|Gemiddelde|Huidige prijs geheugen, byte-$/ tijd, genormaliseerd tot en met 1000.|Er zijn geen dimensies|
|CleanerMemoryShrinkable|Geheugen: Schonere geheugen verkleining|Bytes|Gemiddelde|De hoeveelheid geheugen in bytes, onderworpen aan een schoner opschonen door de achtergrond.|Er zijn geen dimensies|
|CleanerMemoryNonshrinkable|Geheugen: Schonere geheugen nonshrinkable|Bytes|Gemiddelde|Hoeveelheid geheugen in bytes niet onderworpen aan een schoner opschonen door de achtergrond.|Er zijn geen dimensies|
|MemoryUsage|: Geheugengebruik geheugen|Bytes|Gemiddelde|Het geheugengebruik van de server-proces gebruikt bij het berekenen van de prijs van schonere geheugen. Die gelijk is aan het prestatiemeteritem Process\PrivateBytes plus de grootte van het geheugen toegewezen gegevens, die is toegewezen of door de xVelocity in het geheugen analyse-engine (VertiPaq) die groter zijn dan de engine xVelocity geheugenlimiet toegewezen geheugen wordt genegeerd.|Er zijn geen dimensies|
|MemoryLimitHard|Geheugen: Geheugenlimiet harde|Bytes|Gemiddelde|Vaste geheugenlimiet uit het configuratiebestand.|Er zijn geen dimensies|
|MemoryLimitHigh|Geheugen: Geheugen beperken hoog|Bytes|Gemiddelde|Maximale geheugenlimiet van configuratiebestand.|Er zijn geen dimensies|
|MemoryLimitLow|Geheugen: Geheugen limiet laag|Bytes|Gemiddelde|Lage geheugenlimiet uit het configuratiebestand.|Er zijn geen dimensies|
|MemoryLimitVertiPaq|Geheugen: Geheugen limiet VertiPaq|Bytes|Gemiddelde|In het geheugen limiet uit het configuratiebestand.|Er zijn geen dimensies|
|Quota|Geheugen: quotum|Bytes|Gemiddelde|Huidige geheugenquotum, in bytes. Geheugenquotum wordt ook wel een grant of geheugen reservering voor geheugen.|Er zijn geen dimensies|
|QuotaBlocked|: Geheugenquotum geblokkeerd|Aantal|Gemiddelde|Huidig aantal quotum aanvragen die worden geblokkeerd totdat het andere Geheugenquota zijn vrijgegeven.|Er zijn geen dimensies|
|VertiPaqNonpaged|Geheugen: VertiPaq-wisselbaar geheugen:|Bytes|Gemiddelde|Bytes van geheugen is vergrendeld in de werkset voor gebruik door de engine in het geheugen.|Er zijn geen dimensies|
|VertiPaqPaged|Geheugen: VertiPaq wisselbaar geheugen:|Bytes|Gemiddelde|Bytes wisselbaar geheugen in gebruik voor gegevens in het geheugen.|Er zijn geen dimensies|
|RowsReadPerSec|Verwerking: Rijen lezen per seconde|CountPerSecond|Gemiddelde|Het aantal rijen van alle relationele databases worden gelezen.|Er zijn geen dimensies|
|RowsConvertedPerSec|Verwerking: Rijen geconverteerd per seconde|CountPerSecond|Gemiddelde|Het aantal rijen wordt geconverteerd tijdens de verwerking.|Er zijn geen dimensies|
|RowsWrittenPerSec|Verwerking: Rijen geschreven per seconde|CountPerSecond|Gemiddelde|De frequentie van geschreven tijdens het verwerken van rijen.|Er zijn geen dimensies|
|CommandPoolBusyThreads|Threads: Opdracht groepen bezet threads|Count|Gemiddelde|Het aantal bezet threads in de opdracht thread-groep.|Er zijn geen dimensies|
|CommandPoolIdleThreads|Threads: Opdracht groep niet-actieve threads|Count|Gemiddelde|Het aantal niet-actieve threads in de opdracht thread-groep.|Er zijn geen dimensies|
|LongParsingBusyThreads|Threads: Long bezet threads parseren|Count|Gemiddelde|Het aantal bezet threads in de lang parseren thread-groep.|Er zijn geen dimensies|
|LongParsingIdleThreads|Threads: Long bij het parseren van niet-actieve threads|Count|Gemiddelde|Het aantal niet-actieve threads in de lang parseren thread-groep.|Er zijn geen dimensies|
|LongParsingJobQueueLength|Threads: Parseren lang taak wachtrijlengte|Count|Gemiddelde|Het aantal taken in de wachtrij van de threadgroep lang parseren.|Er zijn geen dimensies|
|ProcessingPoolBusyIOJobThreads|Threads: Groepen bezet i/o-taak threads verwerken|Aantal|Gemiddelde|Het aantal actieve threads i/o-taken in de thread-pool.|Er zijn geen dimensies|
|ProcessingPoolBusyNonIOThreads|Threads: Groep bezet niet-I/O-threads verwerkt|Count|Gemiddelde|Het aantal threads worden uitgevoerd van niet-I/O-taken in de thread-pool.|Er zijn geen dimensies|
|ProcessingPoolIOJobQueueLength|Threads: Verwerken i/o-taak wachtrijlengte van toepassingen|Count|Gemiddelde|Het aantal i/o-taken in de wachtrij van de thread-pool.|Er zijn geen dimensies|
|ProcessingPoolIdleIOJobThreads|Threads: Groep niet-actieve i/o-taak threads verwerken|Count|Gemiddelde|Het aantal niet-actieve threads voor i/o-taken in de thread-pool.|Er zijn geen dimensies|
|ProcessingPoolIdleNonIOThreads|Threads: Groep niet-actieve niet-I/O-threads verwerken|Count|Gemiddelde|Het aantal niet-actieve threads in de thread-pool toegewezen aan niet-I/O-taken.|Er zijn geen dimensies|
|QueryPoolIdleThreads|Threads: Query groep niet-actieve threads|Count|Gemiddelde|Het aantal niet-actieve threads voor i/o-taken in de thread-pool.|Er zijn geen dimensies|
|QueryPoolJobQueueLength|Threads: Query groep taak wachtrij lengt|Aantal|Gemiddelde|Het aantal taken in de wachtrij van de threadgroep query.|Er zijn geen dimensies|
|ShortParsingBusyThreads|Threads: Korte bezet threads parseren|Count|Gemiddelde|Het aantal bezet threads in de korte parseren thread-groep.|Er zijn geen dimensies|
|ShortParsingIdleThreads|Threads: Korte bij het parseren van niet-actieve threads|Count|Gemiddelde|Het aantal niet-actieve threads in de korte parseren thread-groep.|Er zijn geen dimensies|
|ShortParsingJobQueueLength|Threads: Korte wachtrijlengte taak parseren|Count|Gemiddelde|Het aantal taken in de wachtrij van de threadgroep voor korte parseren.|Er zijn geen dimensies|
|memory_thrashing_metric|Geheugen voorkomen|Procent|Gemiddelde|Gemiddelde geheugen thrashing.|Er zijn geen dimensies|
|mashup_engine_qpu_metric|M-Engine QPU|Aantal|Gemiddelde|Gebruik QPU door mashup-engine-processen|Er zijn geen dimensies|
|mashup_engine_memory_metric|M-Engine-geheugen|Bytes|Gemiddelde|Geheugengebruik door processen voor mashup-engine|Er zijn geen dimensies|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|TotalRequests|Totaal aantal Gateway aanvragen|Count|Totaal|Het aantal aanvragen van de gateway|Locatie, hostnaam|
|SuccessfulRequests|De Gateway aanvragen|Aantal|Totaal|Aantal geslaagde gateway aanvragen|Locatie, hostnaam|
|UnauthorizedRequests|Niet-geautoriseerde Gateway aanvragen|Count|Totaal|Het aantal niet-geautoriseerde gateway aanvragen|Locatie, hostnaam|
|FailedRequests|Mislukte Gateway aanvragen|Count|Totaal|Aantal fouten in gateway aanvragen|Locatie, hostnaam|
|OtherRequests|Andere Gateway aanvragen|Aantal|Totaal|Aantal andere gateway aanvragen|Locatie, hostnaam|
|Duur|Totale duur van de Gateway aanvragen|milliseconden|Gemiddelde|Totale duur van de Gateway-aanvragen in milliseconden|Locatie, hostnaam|
|Capaciteit|Capaciteit (Preview)|Procent|Maximum|Gebruik van metrische gegevens voor ApiManagement service|Locatie|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|TotalJob|Totaal aantal taken|Aantal|Totaal|Het totale aantal taken|Er zijn geen dimensies|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CoreCount|Speciale Core-telling|Aantal|Totaal|Totale aantal toegewezen kernen in de batch-account|Er zijn geen dimensies|
|TotalNodeCount|Aantal toegewezen knooppunten|Aantal|Totaal|Totale aantal toegewezen knooppunten in het batch-account|Er zijn geen dimensies|
|LowPriorityCoreCount|Core-telling LowPriority|Count|Totaal|Totaal aantal kernen prioriteit laag in de batch-account|Er zijn geen dimensies|
|TotalLowPriorityNodeCount|Aantal knooppunten met lage prioriteit|Aantal|Totaal|Totale aantal knooppunten in de batch-account lage prioriteit|Er zijn geen dimensies|
|CreatingNodeCount|Aantal knooppunten maken|Aantal|Totaal|Het aantal knooppunten dat wordt gemaakt|Er zijn geen dimensies|
|StartingNodeCount|Aantal knooppunten wordt gestart|Count|Totaal|Het aantal knooppunten starten|Er zijn geen dimensies|
|WaitingForStartTaskNodeCount|Wachttijd voor begin taak aantal knooppunten|Count|Totaal|Het aantal knooppunten die wachten op de taak starten is voltooid|Er zijn geen dimensies|
|StartTaskFailedNodeCount|Begintaak aantal knooppunten is mislukt|Aantal|Totaal|Het aantal knooppunten waar de taak starten is mislukt|Er zijn geen dimensies|
|IdleNodeCount|Aantal niet-actieve knooppunten|Count|Totaal|Aantal niet-actieve knooppunten|Er zijn geen dimensies|
|OfflineNodeCount|Aantal offline knooppunten|Count|Totaal|Aantal knooppunten dat offline|Er zijn geen dimensies|
|RebootingNodeCount|Aantal knooppunten aan het opstarten|Aantal|Totaal|Het aantal knooppunten aan het opstarten|Er zijn geen dimensies|
|ReimagingNodeCount|Aantal knooppunten teruggezet|Aantal|Totaal|Het aantal knooppunten teruggezet|Er zijn geen dimensies|
|RunningNodeCount|Aantal actieve knooppunten|Aantal|Totaal|Aantal actieve knooppunten|Er zijn geen dimensies|
|LeavingPoolNodeCount|Aantal van de knooppunten groep verlaten|Count|Totaal|Aantal knooppunten dat de groep verlaten|Er zijn geen dimensies|
|UnusableNodeCount|Aantal onbruikbaar knooppunten|Count|Totaal|Aantal onbruikbaar knooppunten|Er zijn geen dimensies|
|PreemptedNodeCount|Aantal knooppunten afgebroken|Count|Totaal|Aantal afgebroken knooppunten|Er zijn geen dimensies|
|TaskStartEvent|Taak Start gebeurtenissen|Count|Totaal|Totaal aantal taken die zijn gestart|Er zijn geen dimensies|
|TaskCompleteEvent|Taak Complete-gebeurtenissen|Count|Totaal|Totale aantal taken die zijn voltooid|Er zijn geen dimensies|
|TaskFailEvent|Gebeurtenissen van taak mislukt|Count|Totaal|Totaal aantal taken die in een foutstatus voltooid|Er zijn geen dimensies|
|PoolCreateEvent|Gebeurtenissen van toepassingen maken|Count|Totaal|Totale aantal groepen die zijn gemaakt|Er zijn geen dimensies|
|PoolResizeStartEvent|Groep van toepassingen starten formaatwijzigingen|Count|Totaal|Totaal aantal toepassingen Hiermee wordt de grootte die zijn gestart|Er zijn geen dimensies|
|PoolResizeCompleteEvent|Groep formaat Complete-gebeurtenissen|Count|Totaal|Totaal aantal toepassingen Hiermee wordt de grootte die zijn voltooid|Er zijn geen dimensies|
|PoolDeleteStartEvent|Gebeurtenissen van toepassingen starten|Count|Totaal|Totaal aantal toepassingen verwijderen die zijn gestart|Er zijn geen dimensies|
|PoolDeleteCompleteEvent|Toepassingen verwijderen Complete-gebeurtenissen|Aantal|Totaal|Totaal aantal toepassingen verwijderen die zijn voltooid|Er zijn geen dimensies|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|connectedclients|Verbonden Clients|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed|Totaal aantal bewerkingen|Aantal|Totaal||Er zijn geen dimensies|
|cachehits|Treffers in cache|Count|Totaal||Er zijn geen dimensies|
|cachemisses|Cachemissers|Aantal|Totaal||Er zijn geen dimensies|
|getcommands|Opgehaald|Count|Totaal||Er zijn geen dimensies|
|setcommands|Sets|Count|Totaal||Er zijn geen dimensies|
|evictedkeys|Verwijderde sleutels|Count|Totaal||Er zijn geen dimensies|
|totalkeys|Totale aantal sleutels|Aantal|Maximum||Er zijn geen dimensies|
|expiredkeys|Verlopen sleutels|Count|Totaal||Er zijn geen dimensies|
|usedmemory|Gebruikt geheugen|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss|Gebruikt geheugen RSS|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad|Belasting van de server|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite|Cache schrijven|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead|Cache lezen|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime|CPU|Procent|Maximum||Er zijn geen dimensies|
|connectedclients0|Verbonden Clients (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed0|Totaal aantal bewerkingen (Shard 0)|Aantal|Totaal||Er zijn geen dimensies|
|cachehits0|Treffers in cache (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|cachemisses0|Cachemissers (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|getcommands0|Opgehaald (Shard 0)|Aantal|Totaal||Er zijn geen dimensies|
|setcommands0|Sets (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|evictedkeys0|Verwijderde sleutels (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|totalkeys0|Totale aantal sleutels (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys0|Verlopen sleutels (Shard 0)|Aantal|Totaal||Er zijn geen dimensies|
|usedmemory0|Gebruikt geheugen (Shard 0)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss0|Gebruikt geheugen RSS (Shard 0)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad0|Belasting van de server (Shard 0)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite0|Cache schrijven (Shard 0)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead0|Cache lezen (Shard 0)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime0|CPU (Shard 0)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients1|Verbonden Clients (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed1|Totaal aantal bewerkingen (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|cachehits1|Treffers in cache (Shard 1)|Aantal|Totaal||Er zijn geen dimensies|
|cachemisses1|Cachemissers (Shard 1)|Aantal|Totaal||Er zijn geen dimensies|
|getcommands1|Opgehaald (Shard 1)|Aantal|Totaal||Er zijn geen dimensies|
|setcommands1|Sets (Shard 1)|Aantal|Totaal||Er zijn geen dimensies|
|evictedkeys1|Verwijderde sleutels (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|totalkeys1|Totale aantal sleutels (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys1|Verlopen sleutels (Shard 1)|Aantal|Totaal||Er zijn geen dimensies|
|usedmemory1|Gebruikt geheugen (Shard 1)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss1|Gebruikt geheugen RSS (Shard 1)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad1|Belasting van de server (Shard 1)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite1|Cache schrijven (Shard 1)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead1|Cache lezen (Shard 1)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime1|CPU (Shard 1)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients2|Verbonden Clients (Shard 2)|Aantal|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed2|Totaal aantal bewerkingen (Shard 2)|Aantal|Totaal||Er zijn geen dimensies|
|cachehits2|Treffers in cache (Shard 2)|Aantal|Totaal||Er zijn geen dimensies|
|cachemisses2|Cachemissers (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|getcommands2|Opgehaald (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|setcommands2|Sets (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|evictedkeys2|Verwijderde sleutels (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|totalkeys2|Totale aantal sleutels (Shard 2)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys2|Verlopen sleutels (Shard 2)|Aantal|Totaal||Er zijn geen dimensies|
|usedmemory2|Gebruikt geheugen (Shard 2)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss2|Gebruikt geheugen RSS (Shard 2)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad2|Belasting van de server (Shard 2)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite2|Cache schrijven (Shard 2)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead2|Cache lezen (Shard 2)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime2|CPU (Shard 2)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients3|Verbonden Clients (Shard 3)|Aantal|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed3|Totaal aantal bewerkingen (Shard 3)|Aantal|Totaal||Er zijn geen dimensies|
|cachehits3|Treffers in cache (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|cachemisses3|Cachemissers (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|getcommands3|Opgehaald (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|setcommands3|Sets (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|evictedkeys3|Verwijderde sleutels (Shard 3)|Aantal|Totaal||Er zijn geen dimensies|
|totalkeys3|Totale aantal sleutels (Shard 3)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys3|Verlopen sleutels (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|usedmemory3|Gebruikt geheugen (Shard 3)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss3|Gebruikt geheugen RSS (Shard 3)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad3|Belasting van de server (Shard 3)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite3|Cache schrijven (Shard 3)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead3|Cache lezen (Shard 3)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime3|CPU (Shard 3)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients4|Verbonden Clients (Shard 4)|Aantal|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed4|Totaal aantal bewerkingen (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|cachehits4|Treffers in cache (Shard 4)|Aantal|Totaal||Er zijn geen dimensies|
|cachemisses4|Cachemissers (Shard 4)|Aantal|Totaal||Er zijn geen dimensies|
|getcommands4|Opgehaald (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|setcommands4|Sets (Shard 4)|Aantal|Totaal||Er zijn geen dimensies|
|evictedkeys4|Verwijderde sleutels (Shard 4)|Aantal|Totaal||Er zijn geen dimensies|
|totalkeys4|Totale aantal sleutels (Shard 4)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys4|Verlopen sleutels (Shard 4)|Aantal|Totaal||Er zijn geen dimensies|
|usedmemory4|Gebruikt geheugen (Shard 4)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss4|Gebruikt geheugen RSS (Shard 4)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad4|Belasting van de server (Shard 4)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite4|Cache schrijven (Shard 4)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead4|Cache lezen (Shard 4)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime4|CPU (Shard 4)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients5|Verbonden Clients (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed5|Totaal aantal bewerkingen (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|cachehits5|Treffers in cache (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|cachemisses5|Cachemissers (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|getcommands5|Opgehaald (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|setcommands5|Sets (Shard 5)|Aantal|Totaal||Er zijn geen dimensies|
|evictedkeys5|Verwijderde sleutels (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|totalkeys5|Totale aantal sleutels (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys5|Verlopen sleutels (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|usedmemory5|Gebruikt geheugen (Shard 5)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss5|Gebruikt geheugen RSS (Shard 5)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad5|Belasting van de server (Shard 5)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite5|Cache schrijven (Shard 5)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead5|Cache lezen (Shard 5)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime5|CPU (Shard 5)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients6|Verbonden Clients (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed6|Totaal aantal bewerkingen (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|cachehits6|Treffers in cache (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|cachemisses6|Cachemissers (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|getcommands6|Opgehaald (Shard 6)|Aantal|Totaal||Er zijn geen dimensies|
|setcommands6|Sets (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|evictedkeys6|Verwijderde sleutels (Shard 6)|Aantal|Totaal||Er zijn geen dimensies|
|totalkeys6|Totale aantal sleutels (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys6|Verlopen sleutels (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|usedmemory6|Gebruikt geheugen (Shard 6)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss6|Gebruikt geheugen RSS (Shard 6)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad6|Belasting van de server (Shard 6)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite6|Cache schrijven (Shard 6)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead6|Cache lezen (Shard 6)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime6|CPU (Shard 6)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients7|Verbonden Clients (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed7|Totaal aantal bewerkingen (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|cachehits7|Treffers in cache (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|cachemisses7|Cachemissers (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|getcommands7|Opgehaald (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|setcommands7|Sets (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|evictedkeys7|Verwijderde sleutels (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|totalkeys7|Totale aantal sleutels (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys7|Verlopen sleutels (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|usedmemory7|Gebruikt geheugen (Shard 7)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss7|Gebruikt geheugen RSS (Shard 7)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad7|Belasting van de server (Shard 7)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite7|Cache schrijven (Shard 7)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead7|Cache lezen (Shard 7)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime7|CPU (Shard 7)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients8|Verbonden Clients (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed8|Totaal aantal bewerkingen (Shard 8)|Aantal|Totaal||Er zijn geen dimensies|
|cachehits8|Treffers in cache (Shard 8)|Aantal|Totaal||Er zijn geen dimensies|
|cachemisses8|Cachemissers (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|getcommands8|(Shard 8) opgehaald|Aantal|Totaal||Er zijn geen dimensies|
|setcommands8|Sets (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|evictedkeys8|Verwijderde sleutels (Shard 8)|Aantal|Totaal||Er zijn geen dimensies|
|totalkeys8|Totale aantal sleutels (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys8|Verlopen sleutels (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|usedmemory8|Gebruikt geheugen (Shard 8)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss8|Gebruikt geheugen RSS (Shard 8)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad8|Belasting van de server (Shard 8)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite8|Cache schrijven (Shard 8)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead8|Cache lezen (Shard 8)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime8|CPU (Shard 8)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients9|Verbonden Clients (Shard 9)|Aantal|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed9|Totaal aantal bewerkingen (Shard 9)|Aantal|Totaal||Er zijn geen dimensies|
|cachehits9|Treffers in cache (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|cachemisses9|Cachemissers (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|getcommands9|Opgehaald (Shard 9)|Aantal|Totaal||Er zijn geen dimensies|
|setcommands9|Sets (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|evictedkeys9|Verwijderde sleutels (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|totalkeys9|Totale aantal sleutels (Shard 9)|Aantal|Maximum||Er zijn geen dimensies|
|expiredkeys9|Verlopen sleutels (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|usedmemory9|Gebruikt geheugen (Shard 9)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss9|Gebruikt geheugen RSS (Shard 9)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad9|Belasting van de server (Shard 9)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite9|Cache schrijven (Shard 9)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead9|Cache lezen (Shard 9)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime9|CPU (Shard 9)|Procent|Maximum||Er zijn geen dimensies|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CPU-percentage|CPU-percentage|Procent|Gemiddelde|Het percentage toegewezen compute-eenheden die momenteel in gebruik door de virtuele machine.|Er zijn geen dimensies|
|Het netwerk|Het netwerk|Bytes|Totaal|Het aantal bytes dat door de virtuele machine (inkomend verkeer) op alle netwerkinterfaces worden ontvangen.|Er zijn geen dimensies|
|Uit het netwerk|Uit het netwerk|Bytes|Totaal|Het aantal bytes in alle netwerkinterfaces door de virtuele machine (uitgaande verkeer).|Er zijn geen dimensies|
|Gelezen Bytes per seconde|Schijf lezen|BytesPerSecond|Gemiddelde|Gemiddelde aantal bytes lezen van de schijf tijdens de bewaking van de periode.|Er zijn geen dimensies|
|Geschreven Bytes per seconde|Schijf schrijven|BytesPerSecond|Gemiddelde|Gemiddelde aantal bytes geschreven naar de schijf tijdens de bewaking van de periode.|Er zijn geen dimensies|
|Gelezen bewerkingen per seconde|Gelezen bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf lezen IOPS.|Er zijn geen dimensies|
|Geschreven bewerkingen per seconde|Geschreven bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IOPS.|Er zijn geen dimensies|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|TotalCalls|Totaal aantal aanroepen|Count|Totaal|Totaal aantal aanroepen.|Er zijn geen dimensies|
|SuccessfulCalls|Geslaagde aanroepen|Aantal|Totaal|Aantal geslaagde aanroepen.|Er zijn geen dimensies|
|TotalErrors|Totale aantal fouten|Count|Totaal|Totaal aantal aanroepen met foutmelding (HTTP-antwoord code 4xx of 5xx).|Er zijn geen dimensies|
|BlockedCalls|Geblokkeerde aanroepen|Aantal|Totaal|Het aantal aanroepen dat overschreden snelheid of quotumlimiet.|Er zijn geen dimensies|
|ServerErrors|Server-fouten|Aantal|Totaal|Het aantal aanroepen met interne servicefout (HTTP-antwoord code 5xx).|Er zijn geen dimensies|
|ClientErrors|Client-fouten|Aantal|Totaal|Het aantal aanroepen met de fout op de client (HTTP-antwoord code 4xx).|Er zijn geen dimensies|
|DataIn|Gegevens In|Bytes|Totaal|Grootte van binnenkomende gegevens in bytes.|Er zijn geen dimensies|
|DataOut|Gegevens uit|Bytes|Totaal|Grootte van uitgaande gegevens in bytes.|Er zijn geen dimensies|
|Latentie|Latentie|Milliseconden|Gemiddelde|Latentie in milliseconden.|Er zijn geen dimensies|
|CharactersTranslated|Vertaald tekens|Count|Totaal|Totaal aantal tekens in inkomende aanvraag opgegeven tekst.|Er zijn geen dimensies|
|SpeechSessionDuration|Spraak sessieduur|Seconden|Totaal|Totale duur van de sessie spraak in seconden.|Er zijn geen dimensies|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CPU-percentage|CPU-percentage|Procent|Gemiddelde|Het percentage toegewezen compute-eenheden die zich momenteel in gebruik door de virtuele machine|Er zijn geen dimensies|
|Het netwerk|Het netwerk|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerkinterfaces door de virtuele machine (inkomend verkeer)|Er zijn geen dimensies|
|Uit het netwerk|Uit het netwerk|Bytes|Totaal|Het aantal bytes in alle netwerkinterfaces door de virtuele machine (uitgaande verkeer)|Er zijn geen dimensies|
|Schijf lezen Bytes|Schijf lezen Bytes|Bytes|Totaal|Totaal aantal bytes lezen van de schijf tijdens de periode bewaking|Er zijn geen dimensies|
|Schijf schrijven Bytes|Schijf schrijven Bytes|Bytes|Totaal|Totaal aantal bytes geschreven naar de schijf tijdens de periode bewaking|Er zijn geen dimensies|
|Gelezen bewerkingen per seconde|Gelezen bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf lezen IOP 's|Er zijn geen dimensies|
|Geschreven bewerkingen per seconde|Geschreven bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IOP 's|Er zijn geen dimensies|
|CPU-tegoed resterend|CPU-tegoed resterend|Count|Gemiddelde|Totaal aantal tegoed beschikbaar voor burst|Er zijn geen dimensies|
|CPU-tegoed verbruikt|CPU-tegoed verbruikt|Count|Gemiddelde|Totaal aantal tegoed is gebruikt door de virtuele Machine|Er zijn geen dimensies|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CPU-percentage|CPU-percentage|Procent|Gemiddelde|Het percentage toegewezen compute-eenheden die zich momenteel in gebruik door de virtuele machine|Er zijn geen dimensies|
|Het netwerk|Het netwerk|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerkinterfaces door de virtuele machine (inkomend verkeer)|Er zijn geen dimensies|
|Uit het netwerk|Uit het netwerk|Bytes|Totaal|Het aantal bytes in alle netwerkinterfaces door de virtuele machine (uitgaande verkeer)|Er zijn geen dimensies|
|Schijf lezen Bytes|Schijf lezen Bytes|Bytes|Totaal|Totaal aantal bytes lezen van de schijf tijdens de periode bewaking|Er zijn geen dimensies|
|Schijf schrijven Bytes|Schijf schrijven Bytes|Bytes|Totaal|Totaal aantal bytes geschreven naar de schijf tijdens de periode bewaking|Er zijn geen dimensies|
|Gelezen bewerkingen per seconde|Gelezen bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf lezen IOP 's|Er zijn geen dimensies|
|Geschreven bewerkingen per seconde|Geschreven bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IOP 's|Er zijn geen dimensies|
|CPU-tegoed resterend|CPU-tegoed resterend|Count|Gemiddelde|Totaal aantal tegoed beschikbaar voor burst|Er zijn geen dimensies|
|CPU-tegoed verbruikt|CPU-tegoed verbruikt|Count|Gemiddelde|Totaal aantal tegoed is gebruikt door de virtuele Machine|Er zijn geen dimensies|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CPU-percentage|CPU-percentage|Procent|Gemiddelde|Het percentage toegewezen compute-eenheden die zich momenteel in gebruik door de virtuele machine|Er zijn geen dimensies|
|Het netwerk|Het netwerk|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerkinterfaces door de virtuele machine (inkomend verkeer)|Er zijn geen dimensies|
|Uit het netwerk|Uit het netwerk|Bytes|Totaal|Het aantal bytes in alle netwerkinterfaces door de virtuele machine (uitgaande verkeer)|Er zijn geen dimensies|
|Schijf lezen Bytes|Schijf lezen Bytes|Bytes|Totaal|Totaal aantal bytes lezen van de schijf tijdens de periode bewaking|Er zijn geen dimensies|
|Schijf schrijven Bytes|Schijf schrijven Bytes|Bytes|Totaal|Totaal aantal bytes geschreven naar de schijf tijdens de periode bewaking|Er zijn geen dimensies|
|Gelezen bewerkingen per seconde|Gelezen bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf lezen IOP 's|Er zijn geen dimensies|
|Geschreven bewerkingen per seconde|Geschreven bewerkingen per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IOP 's|Er zijn geen dimensies|
|CPU-tegoed resterend|CPU-tegoed resterend|Count|Gemiddelde|Totaal aantal tegoed beschikbaar voor burst|Er zijn geen dimensies|
|CPU-tegoed verbruikt|CPU-tegoed verbruikt|Count|Gemiddelde|Totaal aantal tegoed is gebruikt door de virtuele Machine|Er zijn geen dimensies|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|DCIApiCalls|Klant Insights API-aanroepen|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationSuccessfulLines|Geslaagde bewerkingsregels toewijzing importeren|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationFailedLines|Toewijzing van de importbewerking is mislukt voor regels|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationTotalLines|Totaal aantal bewerkingsregels toewijzing importeren|Count|Totaal||Er zijn geen dimensies|
|DCIMappingImportOperationRuntimeInSeconds|Toewijzing importeren bewerking Runtime In seconden|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportSucceeded|Uitgaande profiel exporteren is geslaagd|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportFailed|Uitgaande profiel exporteren is mislukt|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportDuration|Duur van de uitgaande profiel exporteren|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportSucceeded|Uitgaande Kpi exporteren is voltooid|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportFailed|Uitgaande Kpi exporteren is mislukt|Count|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportDuration|Duur van de uitgaande Kpi exporteren|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiExportStarted|Uitgaande Kpi Export gestart|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiRecordCount|Aantal uitgaande Kpi-records|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportCount|Telling van uitgaande profiel exporteren|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportFailed|Uitgaande profiel exporteren is mislukt|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportSucceeded|Uitgaande profiel exporteren is geslaagd|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialKpiExportFailed|Uitgaande initiële Kpi exporteren is mislukt|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialKpiExportSucceeded|Uitgaande initiële Kpi exporteren is voltooid|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportDurationInSeconds|Uitgaande profiel exporteren duur In seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiFailed|De taak Adla voor standaard-Kpi is mislukt In seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiTimeOut|De taak Adla voor standaard Kpi time-out In seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiCompleted|De taak Adla voor standaard-Kpi voltooid In seconden|Seconden|Totaal||Er zijn geen dimensies|
|ImportASAValuesFailed|Importeren ASA waarden aantal is mislukt|Count|Totaal||Er zijn geen dimensies|
|ImportASAValuesSucceeded|Importeren ASA waarden aantal is voltooid|Count|Totaal||Er zijn geen dimensies|
|DCIProfilesCount|Aantal exemplaren op profiel|Count|laatste||Er zijn geen dimensies|
|DCIInteractionsPerMonthCount|Interacties aantal per maand|Aantal|laatste||Er zijn geen dimensies|
|DCIKpisCount|Aantal van de KPI|Aantal|laatste||Er zijn geen dimensies|
|DCISegmentsCount|Aantal eenheden|Count|laatste||Er zijn geen dimensies|
|DCIPredictiveMatchPoliciesCount|Aantal voorspellende overeenkomsten|Count|laatste||Er zijn geen dimensies|
|DCIPredictionsCount|Aantal voorspelling|Aantal|laatste||Er zijn geen dimensies|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|JobEndedSuccess|Mislukte taken|Aantal|Totaal|Het aantal mislukte taken.|Er zijn geen dimensies|
|JobEndedFailure|Mislukte taken|Count|Totaal|Het aantal mislukte taken.|Er zijn geen dimensies|
|JobEndedCancelled|Geannuleerde taken|Count|Totaal|Het aantal geannuleerde taken.|Er zijn geen dimensies|
|JobAUEndedSuccess|Geslaagde Australië tijd|Seconden|Totaal|Totale tijd Australië voor mislukte taken.|Er zijn geen dimensies|
|JobAUEndedFailure|Mislukte Australië tijd|Seconden|Totaal|Totale tijd Australië voor mislukte taken.|Er zijn geen dimensies|
|JobAUEndedCancelled|Geannuleerde Australië tijd|Seconden|Totaal|Totale tijd Australië voor geannuleerde taken.|Er zijn geen dimensies|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|TotalStorage|Totale opslagruimte|Bytes|Maximum|Totale hoeveelheid gegevens die zijn opgeslagen in het account.|Er zijn geen dimensies|
|dataWritten|Gegevens die worden geschreven|Bytes|Totaal|Totale hoeveelheid gegevens geschreven naar het account.|Er zijn geen dimensies|
|dataRead|Gegevens lezen|Bytes|Totaal|Totale hoeveelheid gegevens gelezen uit het account.|Er zijn geen dimensies|
|WriteRequests|Schrijven van aanvragen|Count|Totaal|Telling van gegevens schrijfaanvragen aan het account.|Er zijn geen dimensies|
|ReadRequests|Alleen aanvragen|Count|Totaal|Telling van gegevens lezen-aanvragen voor het account.|Er zijn geen dimensies|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddelde|CPU-percentage|Er zijn geen dimensies|
|compute_limit|COMPUTE eenheid limiet|Count|Gemiddelde|COMPUTE eenheid limiet|Er zijn geen dimensies|
|compute_consumption_percent|Percentage van de eenheid berekenen|Procent|Gemiddelde|Percentage van de eenheid berekenen|Er zijn geen dimensies|
|memory_percent|Percentage geheugen|Procent|Gemiddelde|Percentage geheugen|Er zijn geen dimensies|
|io_consumption_percent|I/o-percentage|Procent|Gemiddelde|I/o-percentage|Er zijn geen dimensies|
|storage_percent|Opslagpercentage|Procent|Gemiddelde|Opslagpercentage|Er zijn geen dimensies|
|storage_used|Gebruikte opslag|Bytes|Gemiddelde|Gebruikte opslag|Er zijn geen dimensies|
|storage_limit|Opslaglimiet bereikt|Bytes|Gemiddelde|Opslaglimiet bereikt|Er zijn geen dimensies|
|active_connections|Totaal aantal actieve verbindingen|Count|Gemiddelde|Totaal aantal actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Totaal aantal mislukte verbindingen|Aantal|Gemiddelde|Totaal aantal mislukte verbindingen|Er zijn geen dimensies|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddelde|CPU-percentage|Er zijn geen dimensies|
|compute_limit|COMPUTE eenheid limiet|Aantal|Gemiddelde|COMPUTE eenheid limiet|Er zijn geen dimensies|
|compute_consumption_percent|Percentage van de eenheid berekenen|Procent|Gemiddelde|Percentage van de eenheid berekenen|Er zijn geen dimensies|
|memory_percent|Percentage geheugen|Procent|Gemiddelde|Percentage geheugen|Er zijn geen dimensies|
|io_consumption_percent|I/o-percentage|Procent|Gemiddelde|I/o-percentage|Er zijn geen dimensies|
|storage_percent|Opslagpercentage|Procent|Gemiddelde|Opslagpercentage|Er zijn geen dimensies|
|storage_used|Gebruikte opslag|Bytes|Gemiddelde|Gebruikte opslag|Er zijn geen dimensies|
|storage_limit|Opslaglimiet bereikt|Bytes|Gemiddelde|Opslaglimiet bereikt|Er zijn geen dimensies|
|active_connections|Totaal aantal actieve verbindingen|Count|Gemiddelde|Totaal aantal actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Totaal aantal mislukte verbindingen|Count|Gemiddelde|Totaal aantal mislukte verbindingen|Er zijn geen dimensies|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrie-bericht verzenden pogingen|Count|Totaal|Aantal voor apparaat-naar-cloud-telemetrieberichten worden verzonden naar uw IoT-hub geprobeerd|Er zijn geen dimensies|
|d2c.telemetry.ingress.Success|Telemetrieberichten|Count|Totaal|Aantal voor apparaat-naar-cloud-telemetrieberichten is verzonden naar uw IoT-hub|Er zijn geen dimensies|
|c2d.Commands.egress.complete.Success|Opdrachten voltooid|Count|Totaal|Aantal opdrachten voor cloud-naar-apparaat is voltooid door het apparaat|Er zijn geen dimensies|
|c2d.Commands.egress.Abandon.Success|Opdrachten afgebroken|Count|Totaal|Aantal cloud-naar-apparaatopdrachten afgebroken door het apparaat|Er zijn geen dimensies|
|c2d.Commands.egress.Reject.Success|Opdrachten geweigerd|Aantal|Totaal|Aantal cloud-naar-apparaatopdrachten geweigerd door het apparaat|Er zijn geen dimensies|
|devices.totalDevices|Totaal aantal apparaten|Count|Totaal|Aantal apparaten geregistreerd bij uw IoT-hub|Er zijn geen dimensies|
|devices.connectedDevices.allProtocol|Verbonden apparaten|Aantal|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|d2c.telemetry.egress.Success|Telemetrieberichten geleverd|Count|Totaal|Aantal keren dat berichten zijn geschreven naar eindpunten (totaal)|Er zijn geen dimensies|
|d2c.telemetry.egress.Dropped|Verwijderde berichten|Count|Totaal|Aantal berichten dat is verwijderd omdat het eindpunt voor de levering van inactieve|Er zijn geen dimensies|
|d2c.telemetry.egress.orphaned|Zwevende berichten|Aantal|Totaal|Het aantal berichten die niet voldoen aan alle routes met inbegrip van de route voor terugval|Er zijn geen dimensies|
|d2c.telemetry.egress.Invalid|Ongeldige-berichten|Aantal|Totaal|Het aantal berichten niet worden bezorgd omdat ze niet compatibel met het eindpunt|Er zijn geen dimensies|
|d2c.telemetry.egress.fallback|Berichten die overeenkomt met alternatieve voorwaarde|Aantal|Totaal|Aantal berichten dat is geschreven naar het eindpunt voor terugval|Er zijn geen dimensies|
|d2c.endpoints.egress.eventHubs|Berichten in de Event Hub-eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar de Event Hub-eindpunten|Er zijn geen dimensies|
|d2c.endpoints.Latency.eventHubs|Bericht latentie voor Event Hub-eindpunten|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in een Event Hub-eindpunt, in milliseconden|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusQueues|Berichten worden afgeleverd bij Service Bus-wachtrij-eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar Service Bus-wachtrij-eindpunten|Er zijn geen dimensies|
|d2c.endpoints.Latency.serviceBusQueues|Bericht latentie voor Service Bus-wachtrij-eindpunten|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in een Service Bus-wachtrij eindpunt, in milliseconden|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusTopics|Berichten worden afgeleverd bij Service Bus-onderwerp eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar Service Bus-onderwerp eindpunten|Er zijn geen dimensies|
|d2c.endpoints.Latency.serviceBusTopics|Bericht latentie voor Service Bus-onderwerp eindpunten|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in een eindpunt Service Bus-onderwerp in milliseconden|Er zijn geen dimensies|
|d2c.endpoints.egress.builtIn.events|Berichten die worden geleverd aan het ingebouwde eindpunt (berichten/gebeurtenissen)|Aantal|Totaal|Aantal keren dat berichten zijn geschreven naar het ingebouwde eindpunt (berichten/gebeurtenissen)|Er zijn geen dimensies|
|d2c.endpoints.Latency.builtIn.events|Bericht latentie voor het ingebouwde eindpunt (berichten/gebeurtenissen)|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in het ingebouwde eindpunt (berichten/gebeurtenissen), in milliseconden |Er zijn geen dimensies|
|d2c.endpoints.egress.Storage|Berichten in de opslag-eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar de opslag-eindpunten|Er zijn geen dimensies|
|d2c.endpoints.Latency.Storage|Bericht latentie voor opslag-eindpunten|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in een opslag-eindpunt in milliseconden|Er zijn geen dimensies|
|d2c.endpoints.egress.Storage.bytes|Gegevens die zijn geschreven naar de opslag|Bytes|Totaal|Hoeveelheid gegevens in bytes geschreven naar de opslag-eindpunten|Er zijn geen dimensies|
|d2c.endpoints.egress.Storage.blobs|BLOBs die zijn geschreven naar de opslag|Aantal|Totaal|Aantal blobs geschreven naar de opslag-eindpunten|Er zijn geen dimensies|
|d2c.Twin.Read.Success|Geslaagde twin leesbewerkingen van apparaten|Count|Totaal|De telling van alle geslaagde apparaat geïnitieerde twin leest.|Er zijn geen dimensies|
|d2c.Twin.Read.failure|Twin leesbewerkingen van apparaten is mislukt|Count|Totaal|De telling van alle apparaat geïnitieerde twin leesbewerkingen mislukt.|Er zijn geen dimensies|
|d2c.Twin.Read.Size|De grootte van de antwoorden twin leesbewerkingen van apparaten|Bytes|Gemiddelde|De gemiddelde min en max van alle geslaagde apparaat geïnitieerde twin leest.|Er zijn geen dimensies|
|d2c.Twin.update.Success|Geslaagde twin updates van apparaten|Count|Totaal|Het aantal updates van alle geslaagde twin apparaat gestart.|Er zijn geen dimensies|
|d2c.Twin.update.failure|Twin updates van apparaten is mislukt|Count|Totaal|De telling van alle apparaat geïnitieerde twin updates mislukt.|Er zijn geen dimensies|
|d2c.Twin.update.Size|Grootte van de updates twin van apparaten|Bytes|Gemiddelde|De gemiddelde min. en max. grootte van alle geslaagde apparaat geïnitieerde twin updates.|Er zijn geen dimensies|
|c2d.Methods.Success|Geslaagde directe methode aanroepen|Aantal|Totaal|De telling van alle geslaagde directe methodeaanroepen.|Er zijn geen dimensies|
|c2d.Methods.failure|Directe methode aanroepen is mislukt|Count|Totaal|De telling van alle directe methodeaanroepen mislukt.|Er zijn geen dimensies|
|c2d.Methods.requestSize|Aanvraaggrootte van directe methode aanroepen|Bytes|Gemiddelde|De gemiddelde, min en max van alle geslaagde aanvragen voor directe methode.|Er zijn geen dimensies|
|c2d.Methods.responseSize|Reactiegrootte van directe methode aanroepen|Bytes|Gemiddelde|De gemiddelde, min en max van alle geslaagde directe methode antwoorden.|Er zijn geen dimensies|
|c2d.Twin.Read.Success|Geslaagde twin leest uit back-end|Aantal|Totaal|De telling van alle geslaagde back-end-geïnitieerde twin leest.|Er zijn geen dimensies|
|c2d.Twin.Read.failure|Mislukte twin leest uit back-end|Aantal|Totaal|De telling van alle kan geen back-end-geïnitieerde twin leest.|Er zijn geen dimensies|
|c2d.Twin.Read.Size|De grootte van de antwoorden twin leesbewerkingen van back-end|Bytes|Gemiddelde|De gemiddelde min en max van alle geslaagde back-end-geïnitieerde twin leest.|Er zijn geen dimensies|
|c2d.Twin.update.Success|Geslaagde twin updates van de back-end|Aantal|Totaal|De telling van alle geslaagde back-end-geïnitieerde twin updates.|Er zijn geen dimensies|
|c2d.Twin.update.failure|Mislukte twin updates van de back-end|Count|Totaal|De telling van alle back-end-geïnitieerde twin updates mislukt.|Er zijn geen dimensies|
|c2d.Twin.update.Size|Grootte van twin updates van de back-end|Bytes|Gemiddelde|De gemiddelde min. en max. grootte van alle geslaagde back-end-geïnitieerde twin updates.|Er zijn geen dimensies|
|twinQueries.success|Geslaagde twin query 's|Count|Totaal|De telling van alle geslaagde twin query's.|Er zijn geen dimensies|
|twinQueries.failure|Mislukte twin query 's|Count|Totaal|De telling van alle mislukte twin query's.|Er zijn geen dimensies|
|twinQueries.resultSize|Grootte van query's Twin|Bytes|Gemiddelde|De gemiddelde, min en max van de grootte van het resultaat van alle geslaagde twin query's.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.success|Geslaagde bewerkingen voor het maken van twin taken bijwerken|Count|Totaal|De telling van alle geslaagde twin update taken worden gemaakt.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.failure|Mislukte bewerkingen voor het maken van twin taken bijwerken|Aantal|Totaal|De telling van alle mislukte twin update taken worden gemaakt.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.success|Geslaagde bewerkingen voor het maken van de methode aanroepen van taken|Aantal|Totaal|De telling van alle geslaagde directe methode aanroepen taken worden gemaakt.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.failure|Mislukte bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle maken van taken voor directe methode-aanroep is mislukt.|Er zijn geen dimensies|
|jobs.listJobs.success|Geslaagde aanroepen naar de lijst met taken|Count|Totaal|De telling van alle geslaagde aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs.listJobs.failure|Kan niet aanroepen voor een lijst met taken|Count|Totaal|De telling van alle mislukte aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs.cancelJob.success|Geslaagde taakannuleringen|Aantal|Totaal|De telling van alle geslaagde aanroepen naar een taak annuleren.|Er zijn geen dimensies|
|jobs.cancelJob.failure|Mislukte taakannuleringen|Aantal|Totaal|De telling van alle mislukte aanroepen voor een taak annuleren.|Er zijn geen dimensies|
|jobs.queryJobs.success|Taak query 's|Count|Totaal|De telling van alle geslaagde aanroepen naar query taken.|Er zijn geen dimensies|
|jobs.queryJobs.failure|Taak voert een query is mislukt|Aantal|Totaal|De telling van alle mislukte aanroepen naar query taken.|Er zijn geen dimensies|
|jobs.completed|Voltooide taken|Count|Totaal|De telling van alle voltooide taken.|Er zijn geen dimensies|
|jobs.failed|Mislukte taken|Count|Totaal|De telling van alle mislukte taken.|Er zijn geen dimensies|
|d2c.telemetry.ingress.sendThrottle|Aantal bandbreedteregeling fouten|Aantal|Totaal|Beperkt aantal bandbreedteregeling fouten als gevolg van de doorvoer van apparaat|Er zijn geen dimensies|
|dailyMessageQuotaUsed|Totaal aantal berichten dat is gebruikt|Count|Gemiddelde|Nummer van het totaal aantal berichten die op dit moment gebruikt|Er zijn geen dimensies|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|RegistrationAttempts|Registratie pogingen|Count|Totaal|Aantal apparaat registraties geprobeerd|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Apparaten die zijn toegewezen|Aantal|Totaal|Aantal apparaten dat is toegewezen aan een IoT-hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Attestation pogingen|Count|Totaal|Het aantal apparaten verklaringen geprobeerd|ProvisioningServiceName, Status,-Protocol|

## <a name="microsoftdocumentdbdatabaseaccounts-cosmosdb"></a>Microsoft.DocumentDB/databaseAccounts (CosmosDB)
|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|TotalRequests|Totaal aantal aanvragen|Aantal|Count|Aantal aanvragen|DatabaseAccount, CollectionName, DatabaseName, regio, StatusCode|
|MongoRequests|Mongo-aanvragen|Count|Aantal|Aantal Mongo-aanvragen|DatabaseAccount, CollectionName, DatabaseName, regio, ErrorCode, CommandName|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|INREQS|Binnenkomende verzendaanvragen|Count|Totaal|Totaal aantal binnenkomende aanvragen voor een naamruimte verzenden|Er zijn geen dimensies|
|SUCCREQ|Geslaagde aanvragen|Count|Totaal|Totaal aantal geslaagde aanvragen voor een naamruimte|Er zijn geen dimensies|
|FAILREQ|Mislukte aanvragen|Count|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|Er zijn geen dimensies|
|SVRBSY|Server bezet fouten|Count|Totaal|Totaal aantal server bezet fouten voor een naamruimte|Er zijn geen dimensies|
|INTERR|Interne serverfouten|Aantal|Totaal|Totaal aantal interne serverfouten voor een naamruimte|Er zijn geen dimensies|
|MISCERR|Andere fouten|Count|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|Er zijn geen dimensies|
|INMSGS|Binnenkomende berichten (afgeschaft)|Count|Totaal|Totaal aantal binnenkomende berichten voor een naamruimte. Met deze metriek is afgeschaft. Gebruik in plaats daarvan binnenkomende berichten metrische gegevens|Er zijn geen dimensies|
|EHINMSGS|Binnenkomende berichten|Count|Totaal|Totaal aantal binnenkomende berichten voor een naamruimte|Er zijn geen dimensies|
|OUTMSGS|Uitgaande berichten (afgeschaft)|Count|Totaal|Totaal aantal uitgaande berichten voor een naamruimte. Met deze metriek is afgeschaft. Gebruik in plaats daarvan de metriek uitgaande berichten|Er zijn geen dimensies|
|EHOUTMSGS|Uitgaande berichten|Aantal|Totaal|Totaal aantal uitgaande berichten voor een naamruimte|Er zijn geen dimensies|
|EHINMBS|Binnenkomende bytes (afgeschaft)|Bytes|Totaal|Event Hub inkomende bericht doorvoer voor een naamruimte. Met deze metriek is afgeschaft. Gebruik in plaats daarvan binnenkomende bytes metrische gegevens|Er zijn geen dimensies|
|EHINBYTES|Binnenkomende bytes|Bytes|Totaal|Event Hub inkomende bericht doorvoer voor een naamruimte|Er zijn geen dimensies|
|EHOUTMBS|Uitgaande bytes (afgeschaft)|Bytes|Totaal|Event Hub uitgaande bericht doorvoer voor een naamruimte. Met deze metriek is afgeschaft. Gebruik in plaats daarvan uitgaande bytes metrische gegevens|Er zijn geen dimensies|
|EHOUTBYTES|Uitgaande bytes|Bytes|Totaal|Event Hub uitgaande bericht doorvoer voor een naamruimte|Er zijn geen dimensies|
|EHABL|Berichten van de achterstand archiveren|Aantal|Totaal|Berichten van Event Hub archiveren achterstallige voor een naamruimte|Er zijn geen dimensies|
|EHAMSGS|Berichten archiveren|Count|Totaal|Event Hub gearchiveerde berichten in een naamruimte|Er zijn geen dimensies|
|EHAMBS|Archief bericht doorvoer|Bytes|Totaal|Event Hub gearchiveerd bericht doorvoer in een naamruimte|Er zijn geen dimensies|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|ObservedMetricValue|Waargenomen metrische waarde|Count|Gemiddelde|De waarde die wordt berekend door automatisch schalen die bij uitvoering|MetricTriggerSource|
|MetricThreshold|Metrische drempelwaarde|Count|Gemiddelde|De geconfigureerde automatisch schalen drempelwaarde wanneer automatisch schalen is uitgevoerd.|MetricTriggerRule|
|ObservedCapacity|Waargenomen capaciteit|Count|Gemiddelde|De capaciteit gerapporteerd aan automatisch schalen wanneer deze uitgevoerd.|Er zijn geen dimensies|
|ScaleActionsInitiated|Scale-acties die worden gestart|Count|Totaal|De richting van de scale-bewerking.|ScaleDirection|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|RunsStarted|Wordt uitgevoerd die zijn gestart|Aantal|Totaal|Aantal gestarte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCompleted|Voltooid wordt uitgevoerd|Count|Totaal|Aantal voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsSucceeded|Wordt uitgevoerd is voltooid|Count|Totaal|Aantal geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsFailed|Wordt uitgevoerd, is mislukt|Aantal|Totaal|Aantal mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCancelled|Wordt uitgevoerd, is geannuleerd|Count|Totaal|Aantal geannuleerde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunLatency|Latentie uitvoeren|Seconden|Gemiddelde|Latentie van voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunSuccessLatency|Geslaagde latentie uitvoeren|Seconden|Gemiddelde|Latentie van geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunThrottledEvents|Beperkte gebeurtenissen uitvoeren|Aantal|Totaal|Aantal door werkstroomactie of trigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|RunFailurePercentage|Fout Percentage uitvoeren|Procent|Totaal|Percentage mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|ActionsStarted|Acties die zijn gestart |Count|Totaal|Aantal gestarte werkstroomacties.|Er zijn geen dimensies|
|ActionsCompleted|Acties voltooid |Count|Totaal|Aantal voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionsSucceeded|Acties is voltooid |Count|Totaal|Aantal geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionsFailed|Acties is mislukt|Count|Totaal|Aantal mislukte werkstroomacties.|Er zijn geen dimensies|
|ActionsSkipped|Acties die zijn overgeslagen |Count|Totaal|Aantal overgeslagen werkstroomacties.|Er zijn geen dimensies|
|ActionLatency|Actie latentie |Seconden|Gemiddelde|Latentie van voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionSuccessLatency|Actie geslaagd latentie |Seconden|Gemiddelde|Latentie van geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionThrottledEvents|Actie beperkt gebeurtenissen|Count|Totaal|Aantal door werkstroomactie vertraagde gebeurtenissen.|Er zijn geen dimensies|
|TriggersStarted|Triggers is gestart |Aantal|Totaal|Aantal gestarte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersCompleted|Triggers voltooid |Aantal|Totaal|Aantal voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSucceeded|Triggers is voltooid |Count|Totaal|Aantal geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFailed|Triggers is mislukt |Count|Totaal|Aantal mislukte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSkipped|Triggers overgeslagen|Count|Totaal|Aantal overgeslagen werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFired|Deze gebeurtenis wordt gestart triggers |Count|Totaal|Aantal geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerLatency|Trigger latentie |Seconden|Gemiddelde|Latentie van voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggerFireLatency|Trigger Fire latentie |Seconden|Gemiddelde|Latentie van geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerSuccessLatency|Trigger geslaagd latentie |Seconden|Gemiddelde|Latentie van geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerThrottledEvents|Trigger beperkt gebeurtenissen|Aantal|Totaal|Aantal door werkstroomtrigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|BillableActionExecutions|Factureerbare actie uitvoeringen|Count|Totaal|Aantal uitvoeringen van werkstroomacties dat wordt gefactureerd.|Er zijn geen dimensies|
|BillableTriggerExecutions|Factureerbare Trigger-uitvoeringen|Count|Totaal|Aantal uitvoeringen van werkstroomactiveringen dat wordt gefactureerd.|Er zijn geen dimensies|
|TotalBillableExecutions|Totale factureerbare uitvoeringen|Count|Totaal|Aantal werkstroomuitvoeringen dat wordt gefactureerd.|Er zijn geen dimensies|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|VipAvailability|VIP-beschikbaarheid|Aantal|Gemiddelde|Beschikbaarheid van de VIP-eindpunten op basis van resultaten van de test|VipAddress, VipPort|
|DipAvailability|Beschikbaarheid van de DIP|Aantal|Gemiddelde|Beschikbaarheid van de DIP-eindpunten op basis van resultaten van de test|ProtocolType, DipPort, VipAddress, VipPort, DipAddress|
|ByteCount|Aantal bytes|Count|Totaal|Totaal aantal Bytes verzonden binnen een periode|VipAddress, VipPort, richting|
|PacketCount|Aantal|Count|Totaal|Totale aantal pakketten die zijn verzonden binnen een periode|VipAddress, VipPort, richting|
|SYNCount|SYN tellen|Aantal|Totaal|Totaal aantal SYN-pakketten die zijn verzonden binnen een periode|VipAddress, VipPort, richting|
|SnatConnectionCount|Aantal snat omzetten-verbindingen|Aantal|Totaal|Totaal aantal nieuwe snat omzetten verbindingen gemaakt binnen een periode|De ConnectionState VipAddress, DipAddress,|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|PacketsInDDoS|Inkomende pakketten DDoS|CountPerSecond|Gemiddelde|Inkomende pakketten DDoS|Er zijn geen dimensies|
|PacketsDroppedDDoS|Inkomende pakketten verwijderd DDoS|CountPerSecond|Gemiddelde|Inkomende pakketten verwijderd DDoS|Er zijn geen dimensies|
|PacketsForwardedDDoS|Inkomende pakketten DDoS doorgestuurd|CountPerSecond|Gemiddelde|Inkomende pakketten DDoS doorgestuurd|Er zijn geen dimensies|
|TCPPacketsInDDoS|Inkomende pakketten dat TCP DDoS|CountPerSecond|Gemiddelde|Inkomende pakketten dat TCP DDoS|Er zijn geen dimensies|
|TCPPacketsDroppedDDoS|Inkomende pakketten dat TCP verwijderd DDoS|CountPerSecond|Gemiddelde|Inkomende pakketten dat TCP verwijderd DDoS|Er zijn geen dimensies|
|TCPPacketsForwardedDDoS|Binnenkomend TCP-pakket doorgestuurd DDoS|CountPerSecond|Gemiddelde|Binnenkomend TCP-pakket doorgestuurd DDoS|Er zijn geen dimensies|
|UDPPacketsInDDoS|Binnenkomend UDP-pakketten DDoS|CountPerSecond|Gemiddelde|Binnenkomend UDP-pakketten DDoS|Er zijn geen dimensies|
|UDPPacketsDroppedDDoS|Binnenkomend UDP-pakketten verwijderd DDoS|CountPerSecond|Gemiddelde|Binnenkomend UDP-pakketten verwijderd DDoS|Er zijn geen dimensies|
|UDPPacketsForwardedDDoS|Binnenkomend UDP-pakketten DDoS doorgestuurd|CountPerSecond|Gemiddelde|Binnenkomend UDP-pakketten DDoS doorgestuurd|Er zijn geen dimensies|
|BytesInDDoS|Binnenkomende bytes DDoS|BytesPerSecond|Gemiddelde|Binnenkomende bytes DDoS|Er zijn geen dimensies|
|BytesDroppedDDoS|Binnenkomende bytes verwijderd DDoS|BytesPerSecond|Gemiddelde|Binnenkomende bytes verwijderd DDoS|Er zijn geen dimensies|
|BytesForwardedDDoS|Binnenkomende bytes DDoS doorgestuurd|BytesPerSecond|Gemiddelde|Binnenkomende bytes DDoS doorgestuurd|Er zijn geen dimensies|
|TCPBytesInDDoS|Binnenkomende TCP bytes DDoS|BytesPerSecond|Gemiddelde|Binnenkomende TCP bytes DDoS|Er zijn geen dimensies|
|TCPBytesDroppedDDoS|Binnenkomend TCP-bytes verwijderd DDoS|BytesPerSecond|Gemiddelde|Binnenkomend TCP-bytes verwijderd DDoS|Er zijn geen dimensies|
|TCPBytesForwardedDDoS|Binnenkomend TCP-bytes DDoS doorgestuurd|BytesPerSecond|Gemiddelde|Binnenkomend TCP-bytes DDoS doorgestuurd|Er zijn geen dimensies|
|UDPBytesInDDoS|Binnenkomend UDP bytes DDoS|BytesPerSecond|Gemiddelde|Binnenkomend UDP bytes DDoS|Er zijn geen dimensies|
|UDPBytesDroppedDDoS|Binnenkomend UDP bytes verwijderd DDoS|BytesPerSecond|Gemiddelde|Binnenkomend UDP bytes verwijderd DDoS|Er zijn geen dimensies|
|UDPBytesForwardedDDoS|Binnenkomend UDP bytes DDoS doorgestuurd|BytesPerSecond|Gemiddelde|Binnenkomend UDP bytes DDoS doorgestuurd|Er zijn geen dimensies|
|IfUnderDDoSAttack|Onder DDoS-aanvallen, of niet|Count|Gemiddelde|Onder DDoS-aanvallen, of niet|Er zijn geen dimensies|
|DDoSTriggerTCPPackets|Inkomende TCP-pakketten voor het activeren van DDoS risicobeperking|CountPerSecond|Gemiddelde|Inkomende TCP-pakketten voor het activeren van DDoS risicobeperking|Er zijn geen dimensies|
|DDoSTriggerUDPPackets|Binnenkomend UDP-pakketten voor het activeren van DDoS risicobeperking|CountPerSecond|Gemiddelde|Binnenkomend UDP-pakketten voor het activeren van DDoS risicobeperking|Er zijn geen dimensies|
|VipAvailability|Beschikbaarheid|Count|Gemiddelde|Gemiddelde beschikbaarheid van de IP-adres binnen een periode|Poort|
|ByteCount|Aantal bytes|Aantal|Totaal|Totaal aantal Bytes verzonden binnen een periode|Poort, richting|
|PacketCount|Aantal|Count|Totaal|Totale aantal pakketten die zijn verzonden binnen een periode|Poort, richting|
|SynCount|SYN tellen|Count|Totaal|Totaal aantal SYN-pakketten die zijn verzonden binnen een periode|Poort, richting|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|Doorvoer|Doorvoer|BytesPerSecond|Gemiddelde|Het aantal bytes per seconde is die de toepassingsgateway is geleverd|Er zijn geen dimensies|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Tunnel-bandbreedte|BytesPerSecond|Gemiddelde|Gemiddelde bandbreedte van een tunnel in bytes per seconde|ConnectionName, RemoteIP|
|TunnelPeakBandwidth|Bandbreedte-tunnel piek|BytesPerSecond|Gemiddelde|Maximale bandbreedte van een tunnel in bytes per seconde|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tunnel uitgaande Bytes|Bytes|Gemiddelde|Uitgaande bytes van een tunnel in een interval van vijf minuten|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tunnel inkomend Bytes|Bytes|Gemiddelde|Binnenkomende bytes van een tunnel in een interval van vijf minuten|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tunnel uitgaande pakketten|Aantal|Gemiddelde|Aantal uitgaande van een tunnel in een interval van vijf minuten|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tunnel inkomende pakketten|Count|Gemiddelde|Aantal binnenkomende van een tunnel in een interval van vijf minuten|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Tunnel uitgaande TS verschil pakket verwijderen|Count|Gemiddelde|Uitgaande pakket neerzetten van verkeer selector niet-overeenkomende een tunnel in een interval van vijf minuten|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Tunnel inkomend TS verschil pakket verwijderen|Aantal|Gemiddelde|Binnenkomende pakket neerzetten van verkeer selector niet-overeenkomende een tunnel in een interval van vijf minuten|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|BytesIn|BytesIn|Aantal|Totaal|Bytes ingressing Azure|Er zijn geen dimensies|
|BytesOut|BytesOut|Count|Totaal|Azure egressing bytes|Er zijn geen dimensies|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Gemiddelde|Bits ingressing Azure per seconde|Er zijn geen dimensies|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Gemiddelde|Bits Azure egressing per seconde|Er zijn geen dimensies|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|QpsByEndpoint|Query's door eindpunt geretourneerd|Aantal|Totaal|Aantal keren dat een Traffic Manager-eindpunt is geretourneerd in de bepaalde periode|endpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Status endpoint door eindpunt|Count|Maximum|1 als de status van een eindpunt-test "ingeschakeld", anders 0.|endpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|Registration.all|Registratie-bewerkingen|Count|Totaal|De telling van alle bewerkingen van succesvolle registratie (query's aangemaakte-updates en verwijderingen). |Er zijn geen dimensies|
|Registration.Create|Registratie van de bewerkingen maken|Aantal|Totaal|De telling van alle succesvolle registratie aangemaakte.|Er zijn geen dimensies|
|Registration.update|Updatebewerkingen voor registratie|Aantal|Totaal|De telling van alle updates van succesvolle registratie.|Er zijn geen dimensies|
|Registration.Get|Registratie-leesbewerkingen|Count|Totaal|De telling van alle query's van succesvolle registratie.|Er zijn geen dimensies|
|Registration.Delete|Registratie-Delete-bewerkingen|Count|Totaal|De telling van alle succesvolle registratie verwijderingen.|Er zijn geen dimensies|
|binnenkomende|Binnenkomende berichten|Aantal|Totaal|De telling van alle geslaagde verzenden API-aanroepen. |Er zijn geen dimensies|
|Incoming.Scheduled|Geplande Pushmeldingen verzonden|Count|Totaal|Geplande Pushmeldingen geannuleerd|Er zijn geen dimensies|
|Incoming.Scheduled.Cancel|Geplande Pushmeldingen geannuleerd|Count|Totaal|Geplande Pushmeldingen geannuleerd|Er zijn geen dimensies|
|Scheduled.Pending|In behandeling zijnde geplande meldingen|Aantal|Totaal|In behandeling zijnde geplande meldingen|Er zijn geen dimensies|
|Installation.all|Beheerbewerkingen voor installatie|Count|Totaal|Beheerbewerkingen voor installatie|Er zijn geen dimensies|
|Installation.Get|Installatiebewerkingen ophalen|Count|Totaal|Installatiebewerkingen ophalen|Er zijn geen dimensies|
|Installation.upsert|Installatiebewerkingen maken of bijwerken|Aantal|Totaal|Installatiebewerkingen maken of bijwerken|Er zijn geen dimensies|
|Installation.patch|Installatiebewerkingen patch|Count|Totaal|Installatiebewerkingen patch|Er zijn geen dimensies|
|Installation.Delete|Verwijderen van installatiebewerkingen|Count|Totaal|Verwijderen van installatiebewerkingen|Er zijn geen dimensies|
|Outgoing.allpns.Success|Geslaagde meldingen|Count|Totaal|De telling van alle geslaagde meldingen.|Er zijn geen dimensies|
|Outgoing.allpns.invalidpayload|De nettolading van fouten|Count|Totaal|Het aantal pushes die is mislukt, omdat de PNS heeft een ongeldige nettolading-fout geretourneerd.|Er zijn geen dimensies|
|Outgoing.allpns.pnserror|Externe meldingen systeemfouten|Aantal|Totaal|Het aantal pushes die is mislukt omdat er een probleem bij het communiceren met de PNS (met uitzondering van verificatieproblemen).|Er zijn geen dimensies|
|Outgoing.allpns.channelerror|Kanaal-fouten|Count|Totaal|Het aantal pushes die is mislukt omdat het kanaal is ongeldig niet zijn gekoppeld aan de juiste app beperkt of verlopen.|Er zijn geen dimensies|
|Outgoing.allpns.badorexpiredchannel|Fouten met slecht of verlopen kanaal|Aantal|Totaal|Het aantal pushes die is mislukt omdat het kanaal/token/registratie-id in de registratie verlopen of ongeldig is.|Er zijn geen dimensies|
|Outgoing.WNS.Success|Geslaagde WNS-meldingen|Count|Totaal|De telling van alle geslaagde meldingen.|Er zijn geen dimensies|
|Outgoing.WNS.invalidcredentials|WNS autorisatie fouten (ongeldige aanmeldingsgegevens)|Count|Totaal|Het aantal pushes die is mislukt omdat de PNS heeft de opgegeven referenties niet geaccepteerd of de referenties worden geblokkeerd. (Windows Live worden niet herkend door de referenties).|Er zijn geen dimensies|
|Outgoing.WNS.badchannel|Fout in het WNS onjuiste kanaal|Count|Totaal|Het aantal pushes die is mislukt, omdat de ChannelURI in de registratie is niet herkend (WNS-status: 404 niet gevonden).|Er zijn geen dimensies|
|Outgoing.WNS.expiredchannel|WNS verlopen kanaal fout|Count|Totaal|Het aantal pushes die is mislukt omdat de ChannelURI is verlopen (WNS-status: 410 Gone).|Er zijn geen dimensies|
|Outgoing.WNS.throttled|WNS beperkt meldingen|Count|Totaal|Het aantal pushes die is mislukt, omdat deze app is beperking van WNS (WNS-status: 406 niet geaccepteerd).|Er zijn geen dimensies|
|Outgoing.WNS.tokenproviderunreachable|WNS autorisatie fouten (onbereikbaar)|Count|Totaal|Windows Live is niet bereikbaar.|Er zijn geen dimensies|
|Outgoing.WNS.invalidtoken|WNS autorisatie fouten (ongeldige Token)|Count|Totaal|Het token dat is opgegeven voor WNS is niet geldig (WNS-status: niet-gemachtigd 401).|Er zijn geen dimensies|
|Outgoing.WNS.wrongtoken|WNS autorisatie fouten (verkeerd Token)|Count|Totaal|Het token dat is opgegeven voor WNS is geldig, maar voor een andere toepassing (WNS-status: 403-verboden). Dit kan gebeuren als de ChannelURI in de registratie gekoppeld aan een andere app is. Controleer of de client-app is gekoppeld aan dezelfde app met de referenties die zich in de notification hub.|Er zijn geen dimensies|
|Outgoing.WNS.invalidnotificationformat|WNS ongeldig Meldingsresultaat indeling|Count|Totaal|De indeling van de melding is ongeldig (WNS-status: 400). Houd er rekening mee dat WNS alle ongeldige nettoladingen komt niet afwijzen.|Er zijn geen dimensies|
|Outgoing.WNS.invalidnotificationsize|Fout met ongeldige grootte van de WNS-melding|Count|Totaal|De nettolading van de melding is te groot (WNS-status: 413).|Er zijn geen dimensies|
|Outgoing.WNS.channelthrottled|WNS-kanaal beperkt|Aantal|Totaal|De melding is verwijderd omdat de ChannelURI in de registratie is beperkt (WNS antwoordheader: X-WNS-NotificationStatus:channelThrottled).|Er zijn geen dimensies|
|Outgoing.WNS.channeldisconnected|WNS-kanaal is verbroken|Count|Totaal|De melding is verwijderd omdat de ChannelURI in de registratie is beperkt (WNS antwoordheader: X-WNS-DeviceConnectionStatus: verbroken).|Er zijn geen dimensies|
|Outgoing.WNS.Dropped|WNS verwijderd meldingen|Count|Totaal|De melding is verwijderd omdat de ChannelURI in de registratie is beperkt (X-WNS-NotificationStatus: uitgevallen, maar niet X-WNS-DeviceConnectionStatus: verbroken).|Er zijn geen dimensies|
|Outgoing.WNS.pnserror|WNS-fouten|Count|Totaal|De melding is niet bezorgd vanwege fouten bij het communiceren met WNS.|Er zijn geen dimensies|
|Outgoing.WNS.authenticationerror|WNS verificatiefouten|Count|Totaal|De melding niet worden bezorgd omdat er fouten zijn communicatie met Windows Live ongeldige referenties of verkeerd token.|Er zijn geen dimensies|
|Outgoing.apns.Success|Geslaagde meldingen APNS|Aantal|Totaal|De telling van alle geslaagde meldingen.|Er zijn geen dimensies|
|Outgoing.apns.invalidcredentials|APNS-autorisatie-fouten|Count|Totaal|Het aantal pushes die is mislukt omdat de PNS heeft de opgegeven referenties niet geaccepteerd of de referenties worden geblokkeerd.|Er zijn geen dimensies|
|Outgoing.apns.badchannel|Fout in het APNS onjuiste kanaal|Count|Totaal|Het aantal pushes die is mislukt omdat het token ongeldig is (APNS-statuscode: 8).|Er zijn geen dimensies|
|Outgoing.apns.expiredchannel|APNS verlopen kanaal fout|Aantal|Totaal|Het aantal token die ongeldig zijn gemaakt door de APNS feedback kanaal.|Er zijn geen dimensies|
|Outgoing.apns.invalidnotificationsize|Fout met ongeldige grootte van APNS-melding|Count|Totaal|Het aantal pushes die is mislukt, omdat de nettolading te groot is (APNS-statuscode: 7).|Er zijn geen dimensies|
|Outgoing.apns.pnserror|APNS-fouten|Count|Totaal|Het aantal pushes die is mislukt wegens fouten bij het communiceren met APNS.|Er zijn geen dimensies|
|Outgoing.GCM.Success|Geslaagde GCM-meldingen|Count|Totaal|De telling van alle geslaagde meldingen.|Er zijn geen dimensies|
|Outgoing.GCM.invalidcredentials|GCM autorisatie fouten (ongeldige aanmeldingsgegevens)|Aantal|Totaal|Het aantal pushes die is mislukt omdat de PNS heeft de opgegeven referenties niet geaccepteerd of de referenties worden geblokkeerd.|Er zijn geen dimensies|
|Outgoing.GCM.badchannel|Fout bij GCM onjuiste kanaal|Aantal|Totaal|Het aantal pushes die is mislukt, omdat de registratie bij de registratie-id is niet herkend (GCM resultaat: ongeldige registratie).|Er zijn geen dimensies|
|Outgoing.GCM.expiredchannel|GCM verlopen kanaal fout|Aantal|Totaal|Het aantal pushes die is mislukt omdat de registratie-id in de registratie is verlopen (GCM resultaat: NotRegistered).|Er zijn geen dimensies|
|Outgoing.GCM.throttled|GCM beperkt meldingen|Count|Totaal|Het aantal pushes die is mislukt, omdat GCM deze app beperkt (GCM-statuscode: 501-599 ligt of resultaat: niet beschikbaar).|Er zijn geen dimensies|
|Outgoing.GCM.invalidnotificationformat|GCM ongeldig Meldingsresultaat indeling|Aantal|Totaal|Het aantal pushes die is mislukt, omdat de nettolading is niet juist geformatteerd (GCM resultaat: InvalidDataKey of InvalidTtl).|Er zijn geen dimensies|
|Outgoing.GCM.invalidnotificationsize|Fout met ongeldige grootte van GCM-melding|Count|Totaal|Het aantal pushes die is mislukt, omdat de nettolading te groot is (GCM resultaat: MessageTooBig).|Er zijn geen dimensies|
|Outgoing.GCM.wrongchannel|Fout bij GCM onjuiste kanaal|Aantal|Totaal|Het aantal pushes die is mislukt, omdat de registratie bij de registratie-id niet gekoppeld aan de huidige app is (GCM resultaat: InvalidPackageName).|Er zijn geen dimensies|
|Outgoing.GCM.pnserror|GCM-fouten|Count|Totaal|Het aantal pushes die is mislukt wegens fouten bij het communiceren met GCM.|Er zijn geen dimensies|
|Outgoing.GCM.authenticationerror|GCM verificatiefouten|Count|Totaal|Het aantal pushes die is mislukt, omdat de PNS heeft de opgegeven referenties van de referenties niet geaccepteerd, worden geblokkeerd of de SenderId is niet correct geconfigureerd in de app (GCM resultaat: MismatchedSenderId).|Er zijn geen dimensies|
|Outgoing.mpns.Success|Geslaagde MPNS-meldingen|Aantal|Totaal|De telling van alle geslaagde meldingen.|Er zijn geen dimensies|
|Outgoing.mpns.invalidcredentials|Ongeldige referenties MPNS|Aantal|Totaal|Het aantal pushes die is mislukt omdat de PNS heeft de opgegeven referenties niet geaccepteerd of de referenties worden geblokkeerd.|Er zijn geen dimensies|
|Outgoing.mpns.badchannel|Fout in het MPNS onjuiste kanaal|Count|Totaal|Het aantal pushes die is mislukt, omdat de ChannelURI in de registratie is niet herkend (MPNS-status: 404 niet gevonden).|Er zijn geen dimensies|
|Outgoing.mpns.throttled|MPNS beperkt meldingen|Count|Totaal|Het aantal pushes die is mislukt, omdat deze app is beperking van MPNS (WNS MPNS: 406 niet geaccepteerd).|Er zijn geen dimensies|
|Outgoing.mpns.invalidnotificationformat|MPNS ongeldig Meldingsresultaat indeling|Count|Totaal|Het aantal pushes die is mislukt, omdat de nettolading van de melding te groot is.|Er zijn geen dimensies|
|Outgoing.mpns.channeldisconnected|MPNS kanaal verbroken|Aantal|Totaal|Het aantal pushes die is mislukt omdat de ChannelURI in de registratie is verbroken (MPNS-status: 412 niet gevonden).|Er zijn geen dimensies|
|Outgoing.mpns.Dropped|MPNS verwijderd meldingen|Count|Totaal|Het aantal pushes dat is verwijderd door de MPNS (MPNS antwoordheader: X NotificationStatus: QueueFull of Suppressed).|Er zijn geen dimensies|
|Outgoing.mpns.pnserror|MPNS-fouten|Aantal|Totaal|Het aantal pushes die is mislukt wegens fouten bij het communiceren met MPNS.|Er zijn geen dimensies|
|Outgoing.mpns.authenticationerror|Verificatiefouten MPNS|Count|Totaal|Het aantal pushes die is mislukt omdat de PNS heeft de opgegeven referenties niet geaccepteerd of de referenties worden geblokkeerd.|Er zijn geen dimensies|
|notificationhub.pushes|Alle uitgaande meldingen|Count|Totaal|Alle uitgaande meldingen van de notification hub|Er zijn geen dimensies|
|Incoming.all.Requests|Alle inkomende aanvragen|Aantal|Totaal|Totaal aantal binnenkomende aanvragen voor een notification hub|Er zijn geen dimensies|
|Incoming.all.failedrequests|Alle binnenkomende mislukte aanvragen|Count|Totaal|Totaal aantal binnenkomende mislukte aanvragen voor een notification hub|Er zijn geen dimensies|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|SearchLatency|Latentie zoeken|Seconden|Gemiddelde|Gemiddelde zoeken latentie voor de search-service|Er zijn geen dimensies|
|SearchQueriesPerSecond|Zoekquery's per seconde|CountPerSecond|Gemiddelde|Zoekquery's per seconde voor de search-service|Er zijn geen dimensies|
|ThrottledSearchQueriesPercentage|Percentage van beperkte zoeken query 's|Procent|Gemiddelde|Percentage van de zoekquery's die zijn beperkt voor de search-service|Er zijn geen dimensies|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CPUXNS|CPU-gebruik per naamruimte|Procent|Maximum|Service bus premium naamruimte CPU-gebruik metrische gegevens|Er zijn geen dimensies|
|WSXNS|Geheugengebruik per naamruimte|Procent|Maximum|Service bus premium naamruimte geheugen gebruik metrische gegevens|Er zijn geen dimensies|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddelde|CPU-percentage|Er zijn geen dimensies|
|physical_data_read_percent|Gegevens-I/O-percentage|Procent|Gemiddelde|Gegevens-I/O-percentage|Er zijn geen dimensies|
|log_write_percent|Percentage van logboek-i/o|Procent|Gemiddelde|Percentage van logboek-i/o|Er zijn geen dimensies|
|dtu_consumption_percent|DTU-percentage|Procent|Gemiddelde|DTU-percentage|Er zijn geen dimensies|
|Opslag|Totale grootte|Bytes|Maximum|Totale grootte|Er zijn geen dimensies|
|connection_successful|Geslaagde verbindingen|Aantal|Totaal|Geslaagde verbindingen|Er zijn geen dimensies|
|connection_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|blocked_by_firewall|Geblokkeerd door een Firewall|Aantal|Totaal|Geblokkeerd door een Firewall|Er zijn geen dimensies|
|impasse|Impassen|Aantal|Totaal|Impassen|Er zijn geen dimensies|
|storage_percent|Databaseomvangpercentage|Procent|Maximum|Databaseomvangpercentage|Er zijn geen dimensies|
|xtp_storage_percent|In het geheugen OLTP opslag procent|Procent|Gemiddelde|In het geheugen OLTP opslag procent|Er zijn geen dimensies|
|workers_percent|Percentage van de werknemers|Procent|Gemiddelde|Percentage van de werknemers|Er zijn geen dimensies|
|sessions_percent|Percentage van sessies|Procent|Gemiddelde|Percentage van sessies|Er zijn geen dimensies|
|dtu_limit|DTU limiet|Aantal|Gemiddelde|DTU limiet|Er zijn geen dimensies|
|dtu_used|DTU gebruikt|Aantal|Gemiddelde|DTU gebruikt|Er zijn geen dimensies|
|dwu_limit|DWU-limiet|Count|Maximum|DWU-limiet|Er zijn geen dimensies|
|dwu_consumption_percent|DWU-percentage|Procent|Maximum|DWU-percentage|Er zijn geen dimensies|
|dwu_used|DWU gebruikt|Aantal|Maximum|DWU gebruikt|Er zijn geen dimensies|
|dw_cpu_percent|DW-knooppuntniveau CPU-percentage|Procent|Gemiddelde|DW-knooppuntniveau CPU-percentage|dw_logical_node_id|
|dw_physical_data_read_percent|DW knooppunt niveau gegevens IO-percentage|Procent|Gemiddelde|DW knooppunt niveau gegevens IO-percentage|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddelde|CPU-percentage|Er zijn geen dimensies|
|database_cpu_percent|CPU-percentage|Procent|Gemiddelde|CPU-percentage|DatabaseResourceId|
|physical_data_read_percent|Gegevens-I/O-percentage|Procent|Gemiddelde|Gegevens-I/O-percentage|Er zijn geen dimensies|
|database_physical_data_read_percent|Gegevens-I/O-percentage|Procent|Gemiddelde|Gegevens-I/O-percentage|DatabaseResourceId|
|log_write_percent|Percentage van logboek-i/o|Procent|Gemiddelde|Percentage van logboek-i/o|Er zijn geen dimensies|
|database_log_write_percent|Percentage van logboek-i/o|Procent|Gemiddelde|Percentage van logboek-i/o|DatabaseResourceId|
|dtu_consumption_percent|DTU-percentage|Procent|Gemiddelde|DTU-percentage|Er zijn geen dimensies|
|database_dtu_consumption_percent|DTU-percentage|Procent|Gemiddelde|DTU-percentage|DatabaseResourceId|
|storage_percent|Opslagpercentage|Procent|Gemiddelde|Opslagpercentage|Er zijn geen dimensies|
|workers_percent|Percentage van de werknemers|Procent|Gemiddelde|Percentage van de werknemers|Er zijn geen dimensies|
|database_workers_percent|Percentage van de werknemers|Procent|Gemiddelde|Percentage van de werknemers|DatabaseResourceId|
|sessions_percent|Percentage van sessies|Procent|Gemiddelde|Percentage van sessies|Er zijn geen dimensies|
|database_sessions_percent|Percentage van sessies|Procent|Gemiddelde|Percentage van sessies|DatabaseResourceId|
|eDTU_limit|limiet voor eDTU|Aantal|Gemiddelde|limiet voor eDTU|Er zijn geen dimensies|
|storage_limit|Opslaglimiet bereikt|Bytes|Gemiddelde|Opslaglimiet bereikt|Er zijn geen dimensies|
|eDTU_used|eDTU gebruikt|Count|Gemiddelde|eDTU gebruikt|Er zijn geen dimensies|
|storage_used|Gebruikte opslag|Bytes|Gemiddelde|Gebruikte opslag|Er zijn geen dimensies|
|database_storage_used|Gebruikte opslag|Bytes|Gemiddelde|Gebruikte opslag|DatabaseResourceId|
|xtp_storage_percent|In het geheugen OLTP opslag procent|Procent|Gemiddelde|In het geheugen OLTP opslag procent|Er zijn geen dimensies|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|dtu_consumption_percent|DTU-percentage|Procent|Gemiddelde|DTU-percentage|ElasticPoolResourceId|
|database_dtu_consumption_percent|DTU-percentage|Procent|Gemiddelde|DTU-percentage|DatabaseResourceId, ElasticPoolResourceId|
|storage_used|Gebruikte opslag|Bytes|Gemiddelde|Gebruikte opslag|ElasticPoolResourceId|
|database_storage_used|Gebruikte opslag|Bytes|Gemiddelde|Gebruikte opslag|DatabaseResourceId, ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|UsedCapacity|Gebruikte capaciteit|Bytes|Gemiddelde|Capaciteit van de account die wordt gebruikt|Er zijn geen dimensies|
|Transacties|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen als aanvragen die fouten geproduceerd. ResponseType dimensie gebruiken voor het aantal ander type van het antwoord.|ResponseType, GeoType, ApiName|
|Binnenkomend|Binnenkomend|Bytes|Totaal|De hoeveelheid inkomend gegevens, in bytes. Dit is inclusief de inkomende gegevens via een externe client in Azure Storage, evenals een inkomend binnen Azure.|GeoType, ApiName|
|Uitgaande|Uitgaande|Bytes|Totaal|De hoeveelheid gegevens van de uitgaande, in bytes. Dit is inclusief uitgaande van een externe client in Azure Storage, evenals een uitgaande binnen Azure. Dit nummer is daardoor niet factureerbare uitgaande doorgevoerd.|GeoType, ApiName|
|SuccessServerLatency|Geslaagde Server latentie|milliseconden|Gemiddelde|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van aanvraag is gelukt, in milliseconden. Deze waarde is niet de netwerklatentie die is opgegeven in AverageE2ELatency opgenomen.|GeoType, ApiName|
|SuccessE2ELatency|Geslaagde E2E-latentie|milliseconden|Gemiddelde|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage te lezen van de aanvraag, antwoord verzenden en ontvangen van de bevestiging van het antwoord.|GeoType, ApiName|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddelde|Het percentage van de beschikbaarheid van de storage-service of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de waarde TotalBillableRequests nemen en deze te delen door het aantal van toepassing aanvragen, waaronder de geproduceerd onverwachte fouten. Alle onverwachte fouten leiden tot beperkte beschikbaarheid voor de storage-service of de opgegeven API-bewerking.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|BlobCapacity|BLOB-capaciteit|Bytes|Gemiddelde|De hoeveelheid opslagruimte die wordt gebruikt door de storage-account Blob-service in bytes.|BlobType|
|BlobCount|Aantal BLOB|Count|Gemiddelde|Het aantal Blob in Blob-service van het opslagaccount.|BlobType|
|ContainerCount|Telling van BLOB-Container|Count|Gemiddelde|Het aantal containers in het opslagaccount Blob-service.|Er zijn geen dimensies|
|Transacties|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen als aanvragen die fouten geproduceerd. ResponseType dimensie gebruiken voor het aantal ander type van het antwoord.|ResponseType, GeoType, ApiName|
|Binnenkomend|Binnenkomend|Bytes|Totaal|De hoeveelheid inkomend gegevens, in bytes. Dit is inclusief de inkomende gegevens via een externe client in Azure Storage, evenals een inkomend binnen Azure.|GeoType, ApiName|
|Uitgaande|Uitgaande|Bytes|Totaal|De hoeveelheid gegevens van de uitgaande, in bytes. Dit is inclusief uitgaande van een externe client in Azure Storage, evenals een uitgaande binnen Azure. Dit nummer is daardoor niet factureerbare uitgaande doorgevoerd.|GeoType, ApiName|
|SuccessServerLatency|Geslaagde Server latentie|milliseconden|Gemiddelde|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van aanvraag is gelukt, in milliseconden. Deze waarde is niet de netwerklatentie die is opgegeven in AverageE2ELatency opgenomen.|GeoType, ApiName|
|SuccessE2ELatency|Geslaagde E2E-latentie|milliseconden|Gemiddelde|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage te lezen van de aanvraag, antwoord verzenden en ontvangen van de bevestiging van het antwoord.|GeoType, ApiName|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddelde|Het percentage van de beschikbaarheid van de storage-service of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de waarde TotalBillableRequests nemen en deze te delen door het aantal van toepassing aanvragen, waaronder de geproduceerd onverwachte fouten. Alle onverwachte fouten leiden tot beperkte beschikbaarheid voor de storage-service of de opgegeven API-bewerking.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|TableCapacity|Capaciteit van de tabel|Bytes|Gemiddelde|De hoeveelheid opslagruimte die wordt gebruikt door de storage-account tabelservice in bytes.|Er zijn geen dimensies|
|TableCount|Aantal van de tabel|Count|Gemiddelde|Het nummer van de tabel in de tabel-service van het storage-account.|Er zijn geen dimensies|
|TableEntityCount|Tabel entiteit tellen|Count|Gemiddelde|Het aantal tabelentiteiten in de tabel-service van het storage-account.|Er zijn geen dimensies|
|Transacties|Transacties|Aantal|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen als aanvragen die fouten geproduceerd. ResponseType dimensie gebruiken voor het aantal ander type van het antwoord.|ResponseType, GeoType, ApiName|
|Binnenkomend|Binnenkomend|Bytes|Totaal|De hoeveelheid inkomend gegevens, in bytes. Dit is inclusief de inkomende gegevens via een externe client in Azure Storage, evenals een inkomend binnen Azure.|GeoType, ApiName|
|Uitgaande|Uitgaande|Bytes|Totaal|De hoeveelheid gegevens van de uitgaande, in bytes. Dit is inclusief uitgaande van een externe client in Azure Storage, evenals een uitgaande binnen Azure. Dit nummer is daardoor niet factureerbare uitgaande doorgevoerd.|GeoType, ApiName|
|SuccessServerLatency|Geslaagde Server latentie|milliseconden|Gemiddelde|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van aanvraag is gelukt, in milliseconden. Deze waarde is niet de netwerklatentie die is opgegeven in AverageE2ELatency opgenomen.|GeoType, ApiName|
|SuccessE2ELatency|Geslaagde E2E-latentie|milliseconden|Gemiddelde|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage te lezen van de aanvraag, antwoord verzenden en ontvangen van de bevestiging van het antwoord.|GeoType, ApiName|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddelde|Het percentage van de beschikbaarheid van de storage-service of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de waarde TotalBillableRequests nemen en deze te delen door het aantal van toepassing aanvragen, waaronder de geproduceerd onverwachte fouten. Alle onverwachte fouten leiden tot beperkte beschikbaarheid voor de storage-service of de opgegeven API-bewerking.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|QueueCapacity|Capaciteit van de wachtrij|Bytes|Gemiddelde|De hoeveelheid opslagruimte die wordt gebruikt door de storage-account Queue-service in bytes.|Er zijn geen dimensies|
|QueueCount|Aantal van de wachtrij|Count|Gemiddelde|Het nummer van de wachtrij in het opslagaccount Queue-service.|Er zijn geen dimensies|
|QueueMessageCount|Aantal wachtrij-berichten|Count|Gemiddelde|Het geschatte aantal berichtenwachtrij-berichten in het opslagaccount Queue-service.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen als aanvragen die fouten geproduceerd. ResponseType dimensie gebruiken voor het aantal ander type van het antwoord.|ResponseType, GeoType, ApiName|
|Binnenkomend|Binnenkomend|Bytes|Totaal|De hoeveelheid inkomend gegevens, in bytes. Dit is inclusief de inkomende gegevens via een externe client in Azure Storage, evenals een inkomend binnen Azure.|GeoType, ApiName|
|Uitgaande|Uitgaande|Bytes|Totaal|De hoeveelheid gegevens van de uitgaande, in bytes. Dit is inclusief uitgaande van een externe client in Azure Storage, evenals een uitgaande binnen Azure. Dit nummer is daardoor niet factureerbare uitgaande doorgevoerd.|GeoType, ApiName|
|SuccessServerLatency|Geslaagde Server latentie|milliseconden|Gemiddelde|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van aanvraag is gelukt, in milliseconden. Deze waarde is niet de netwerklatentie die is opgegeven in AverageE2ELatency opgenomen.|GeoType, ApiName|
|SuccessE2ELatency|Geslaagde E2E-latentie|milliseconden|Gemiddelde|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage te lezen van de aanvraag, antwoord verzenden en ontvangen van de bevestiging van het antwoord.|GeoType, ApiName|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddelde|Het percentage van de beschikbaarheid van de storage-service of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de waarde TotalBillableRequests nemen en deze te delen door het aantal van toepassing aanvragen, waaronder de geproduceerd onverwachte fouten. Alle onverwachte fouten leiden tot beperkte beschikbaarheid voor de storage-service of de opgegeven API-bewerking.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|FileCapacity|Bestand-capaciteit|Bytes|Gemiddelde|De hoeveelheid opslagruimte die wordt gebruikt door de storage-account File-service in bytes.|Er zijn geen dimensies|
|FileCount|Aantal bestanden|Aantal|Gemiddelde|Het nummer van het bestand in het opslagaccount File-service.|Er zijn geen dimensies|
|FileShareCount|Bestandsshare mee|Aantal|Gemiddelde|Het aantal bestand deelt in het opslagaccount File-service.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen als aanvragen die fouten geproduceerd. ResponseType dimensie gebruiken voor het aantal ander type van het antwoord.|ResponseType, GeoType, ApiName|
|Binnenkomend|Binnenkomend|Bytes|Totaal|De hoeveelheid inkomend gegevens, in bytes. Dit is inclusief de inkomende gegevens via een externe client in Azure Storage, evenals een inkomend binnen Azure.|GeoType, ApiName|
|Uitgaande|Uitgaande|Bytes|Totaal|De hoeveelheid gegevens van de uitgaande, in bytes. Dit is inclusief uitgaande van een externe client in Azure Storage, evenals een uitgaande binnen Azure. Dit nummer is daardoor niet factureerbare uitgaande doorgevoerd.|GeoType, ApiName|
|SuccessServerLatency|Geslaagde Server latentie|milliseconden|Gemiddelde|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van aanvraag is gelukt, in milliseconden. Deze waarde is niet de netwerklatentie die is opgegeven in AverageE2ELatency opgenomen.|GeoType, ApiName|
|SuccessE2ELatency|Geslaagde E2E-latentie|milliseconden|Gemiddelde|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage te lezen van de aanvraag, antwoord verzenden en ontvangen van de bevestiging van het antwoord.|GeoType, ApiName|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddelde|Het percentage van de beschikbaarheid van de storage-service of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de waarde TotalBillableRequests nemen en deze te delen door het aantal van toepassing aanvragen, waaronder de geproduceerd onverwachte fouten. Alle onverwachte fouten leiden tot beperkte beschikbaarheid voor de storage-service of de opgegeven API-bewerking.|GeoType, ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|ResourceUtilization|SU % gebruik|Procent|Maximum|SU % gebruik|Er zijn geen dimensies|
|InputEvents|Invoervelden|Count|Totaal|Invoervelden|Er zijn geen dimensies|
|InputEventBytes|Invoergebeurtenis Bytes|Bytes|Totaal|Invoergebeurtenis Bytes|Er zijn geen dimensies|
|LateInputEvents|Laat invoervelden|Aantal|Totaal|Laat invoervelden|Er zijn geen dimensies|
|OutputEvents|Uitvoergebeurtenissen|Aantal|Totaal|Uitvoergebeurtenissen|Er zijn geen dimensies|
|ConversionErrors|Conversie van fouten|Count|Totaal|Conversie van fouten|Er zijn geen dimensies|
|Fouten|Runtime-fouten|Count|Totaal|Runtime-fouten|Er zijn geen dimensies|
|DroppedOrAdjustedEvents|Gebeurtenissen in andere volgorde|Aantal|Totaal|Gebeurtenissen in andere volgorde|Er zijn geen dimensies|
|AMLCalloutRequests|Aanvragen van functie|Aantal|Totaal|Aanvragen van functie|Er zijn geen dimensies|
|AMLCalloutFailedRequests|Mislukte functie aanvragen|Aantal|Totaal|Mislukte functie aanvragen|Er zijn geen dimensies|
|AMLCalloutInputEvents|Gebeurtenissen voor de functie|Count|Totaal|Gebeurtenissen voor de functie|Er zijn geen dimensies|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CpuPercentage|CPU-percentage|Procent|Gemiddelde|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Procent|Gemiddelde|Geheugenpercentage|Exemplaar|
|DiskQueueLength|Wachtrijlengte voor schijf|Count|Totaal|Wachtrijlengte voor schijf|Exemplaar|
|HttpQueueLength|HTTP-wachtrijlengte|Count|Totaal|HTTP-wachtrijlengte|Exemplaar|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|Exemplaar|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (met uitzondering van de functies)

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|Aanvragen|Aanvragen|Aantal|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|Exemplaar|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|Http101|HTTP 101|Count|Totaal|HTTP 101|Exemplaar|
|Http2xx|HTTP-2xx|Count|Totaal|HTTP-2xx|Exemplaar|
|Http3xx|HTTP-3xx|Count|Totaal|HTTP-3xx|Exemplaar|
|Http401|401 HTTP|Count|Totaal|401 HTTP|Exemplaar|
|Http403|HTTP-fout 403|Aantal|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP 404|Count|Totaal|HTTP 404|Exemplaar|
|Http406|HTTP 406|Aantal|Totaal|HTTP 406|Exemplaar|
|Http4xx|HTTP-4xx|Count|Totaal|HTTP-4xx|Exemplaar|
|Http5xx|HTTP-Server-fouten|Count|Totaal|HTTP-Server-fouten|Exemplaar|
|MemoryWorkingSet|Geheugen-werkset|Bytes|Gemiddelde|Geheugen-werkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddelde geheugen werkset|Bytes|Gemiddelde|Gemiddelde geheugen werkset|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddelde|Gemiddelde reactietijd|Exemplaar|
|FunctionExecutionUnits|Eenheden van de functie kan worden uitgevoerd|Count|Gemiddelde|Eenheden van de functie kan worden uitgevoerd|Exemplaar|
|FunctionExecutionCount|Aantal keer uitgevoerd voor de functie|Count|Gemiddelde|Aantal keer uitgevoerd voor de functie|Exemplaar|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (functies)

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|Exemplaar|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|Http5xx|HTTP-Server-fouten|Count|Totaal|HTTP-Server-fouten|Exemplaar|
|MemoryWorkingSet|Geheugen-werkset|Bytes|Gemiddelde|Geheugen-werkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddelde geheugen werkset|Bytes|Gemiddelde|Gemiddelde geheugen werkset|Exemplaar|
|FunctionExecutionUnits|Eenheden van de functie kan worden uitgevoerd|Count|Gemiddelde|Eenheden van de functie kan worden uitgevoerd|Exemplaar|
|FunctionExecutionCount|Aantal keer uitgevoerd voor de functie|Count|Gemiddelde|Aantal keer uitgevoerd voor de functie|Exemplaar|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|Aanvragen|Aanvragen|Count|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|Exemplaar|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|Http101|HTTP 101|Aantal|Totaal|HTTP 101|Exemplaar|
|Http2xx|HTTP-2xx|Aantal|Totaal|HTTP-2xx|Exemplaar|
|Http3xx|HTTP-3xx|Count|Totaal|HTTP-3xx|Exemplaar|
|Http401|401 HTTP|Count|Totaal|401 HTTP|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP 404|Count|Totaal|HTTP 404|Exemplaar|
|Http406|HTTP 406|Aantal|Totaal|HTTP 406|Exemplaar|
|Http4xx|HTTP-4xx|Count|Totaal|HTTP-4xx|Exemplaar|
|Http5xx|HTTP-Server-fouten|Count|Totaal|HTTP-Server-fouten|Exemplaar|
|MemoryWorkingSet|Geheugen-werkset|Bytes|Gemiddelde|Geheugen-werkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddelde geheugen werkset|Bytes|Gemiddelde|Gemiddelde geheugen werkset|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddelde|Gemiddelde reactietijd|Exemplaar|
|FunctionExecutionUnits|Eenheden van de functie kan worden uitgevoerd|Aantal|Gemiddelde|Eenheden van de functie kan worden uitgevoerd|Exemplaar|
|FunctionExecutionCount|Aantal keer uitgevoerd voor de functie|Count|Gemiddelde|Aantal keer uitgevoerd voor de functie|Exemplaar|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|Aanvragen|Aanvragen|Aantal|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|Exemplaar|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|Exemplaar|
|Http101|HTTP 101|Aantal|Totaal|HTTP 101|Exemplaar|
|Http2xx|HTTP-2xx|Count|Totaal|HTTP-2xx|Exemplaar|
|Http3xx|HTTP-3xx|Count|Totaal|HTTP-3xx|Exemplaar|
|Http401|401 HTTP|Aantal|Totaal|401 HTTP|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP 404|Count|Totaal|HTTP 404|Exemplaar|
|Http406|HTTP 406|Count|Totaal|HTTP 406|Exemplaar|
|Http4xx|HTTP-4xx|Aantal|Totaal|HTTP-4xx|Exemplaar|
|Http5xx|HTTP-Server-fouten|Count|Totaal|HTTP-Server-fouten|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddelde|Gemiddelde reactietijd|Exemplaar|
|CpuPercentage|CPU-percentage|Procent|Gemiddelde|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Procent|Gemiddelde|Geheugenpercentage|Exemplaar|
|DiskQueueLength|Wachtrijlengte voor schijf|Count|Totaal|Wachtrijlengte voor schijf|Exemplaar|
|HttpQueueLength|HTTP-wachtrijlengte|Count|Totaal|HTTP-wachtrijlengte|Exemplaar|
|ActiveRequests|Actieve aanvragen|Count|Totaal|Actieve aanvragen|Exemplaar|
|TotalFrontEnds|Totaal aantal Front-Ends|Count|Gemiddelde|Totaal aantal Front-Ends|Exemplaar|
|SmallAppServicePlanInstances|Werknemers van kleine App Service-Plan|Count|Gemiddelde|Werknemers van kleine App Service-Plan|Exemplaar|
|MediumAppServicePlanInstances|Gemiddeld App Service Plan werknemers|Aantal|Gemiddelde|Gemiddeld App Service Plan werknemers|Exemplaar|
|LargeAppServicePlanInstances|Grote App Service Plan werknemers|Count|Gemiddelde|Grote App Service Plan werknemers|Exemplaar|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|WorkersTotal|Totaal aantal werknemers|Count|Gemiddelde|Totaal aantal werknemers|Exemplaar|
|WorkersAvailable|Beschikbare werknemers|Count|Gemiddelde|Beschikbare werknemers|Exemplaar|
|WorkersUsed|Gebruikte werknemers|Count|Gemiddelde|Gebruikte werknemers|Exemplaar|

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over metrische gegevens in Azure-Monitor](monitoring-overview-metrics.md)
* [Waarschuwingen maken op de metrische gegevens](insights-receive-alert-notifications.md)
* [Metrische gegevens exporteren naar opslag, Event Hub of Log Analytics](monitoring-overview-of-diagnostic-logs.md)
