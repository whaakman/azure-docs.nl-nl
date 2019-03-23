---
title: Azure Monitor ondersteunde metrische gegevens op resourcetype
description: Overzicht van metrische gegevens beschikbaar zijn voor elk resourcetype met Azure Monitor.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 09/14/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: e9b562cb04bb8916245d9df7b9b6d526bd443a24
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58352133"
---
# <a name="supported-metrics-with-azure-monitor"></a>Ondersteunde metrische gegevens met Azure Monitor

Azure Monitor biedt verschillende manieren om te communiceren met metrische gegevens, zoals ze in het portaal grafieken, toegang hebben tot deze via de REST-API of uitvoeren van deze query's met behulp van PowerShell of CLI. Hieronder vindt u een volledige lijst van alle metrische gegevens op dit moment met metrische gegevens van Azure Monitor-pijplijn. Andere metrische gegevens is mogelijk beschikbaar in de portal of met verouderde API's. Deze lijst hieronder bevat alleen beschikbaar via de pijplijn voor het metrische gegevens van Azure Monitor samengevoegde metrische gegevens. Query's uitvoeren voor en toegang tot deze metrische gegevens gebruik de [2018-01-01-api-versie](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Bijvoorbeeld*: De metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|qpu_metric|QPU|Count|Gemiddeld|QPU. Bereik 0-100 voor S1, 0-200 voor S2 en 0-400 voor S4|ServerResourceType|
|memory_metric|Geheugen|Bytes|Gemiddeld|Geheugen. Bereik van 0-25 GB voor S1, 0-50 GB voor S2 en 0-100 GB voor S4|ServerResourceType|
|TotalConnectionRequests|Totaal aantal verbindingsaanvragen|Count|Gemiddeld|Totaal aantal verbindingsaanvragen. Dit zijn ontvangsten.|ServerResourceType|
|SuccessfullConnectionsPerSec|Geslaagde verbindingen Per seconde|CountPerSecond|Gemiddeld|Snelheid van aanvullen met de verbinding is geslaagd.|ServerResourceType|
|TotalConnectionFailures|Totaal aantal verbindingsfouten|Count|Gemiddeld|Totaal aantal mislukte verbindingspogingen.|ServerResourceType|
|CurrentUserSessions|Huidige gebruikerssessies|Count|Gemiddeld|Huidig aantal vastgestelde gebruikerssessies.|ServerResourceType|
|QueryPoolBusyThreads|Query Pool-Jobs onderzoeken|Count|Gemiddeld|Het aantal actieve threads in de querythreadpool.|ServerResourceType|
|CommandPoolJobQueueLength|Lengte van taakwachtrij Pool opdracht|Count|Gemiddeld|Het aantal taken in de wachtrij van de opdrachtthreadpool.|ServerResourceType|
|ProcessingPoolJobQueueLength|Lengte van taakwachtrij van toepassingen verwerken|Count|Gemiddeld|Aantal niet-I/O-taken in de wachtrij van de verwerkende ThreadPool.|ServerResourceType|
|CurrentConnections|Verbinding: Huidige verbindingen|Count|Gemiddeld|Huidig aantal clientverbindingen dat tot stand gebracht.|ServerResourceType|
|CleanerCurrentPrice|Geheugen: Schoner, huidige prijs|Count|Gemiddeld|Huidige prijs van geheugen, wat $/ byte/tijd, genormaliseerd voor 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Geheugen: Schoner geheugen verkleinbaar|Bytes|Gemiddeld|Hoeveelheid geheugen in bytes, leeggemaakt Opruimprogramma op de achtergrond.|ServerResourceType|
|CleanerMemoryNonshrinkable|Geheugen: Schoner geheugen niet verkleinbaar|Bytes|Gemiddeld|Hoeveelheid geheugen in bytes, niet kan worden leeggemaakt Opruimprogramma op de achtergrond.|ServerResourceType|
|MemoryUsage|Geheugen: Geheugengebruik|Bytes|Gemiddeld|Het geheugengebruik van het serverproces zoals gebruikt bij het berekenen van de prijs van schoner geheugen. Gelijk aan de teller Process\PrivateBytes plus de grootte van het geheugen toegewezen gegevens, worden alle geheugen die is toegewezen of door de xVelocity in-memory analyse-engine (VertiPaq) boven de geheugenlimiet van de xVelocity-engine genegeerd.|ServerResourceType|
|MemoryLimitHard|Geheugen: Vaste geheugenlimiet|Bytes|Gemiddeld|Vaste geheugenlimiet, van configuratiebestand.|ServerResourceType|
|MemoryLimitHigh|Geheugen: Hoge geheugenlimiet|Bytes|Gemiddeld|Hoge geheugenlimiet, van configuratiebestand.|ServerResourceType|
|MemoryLimitLow|Geheugen: Lage geheugenlimiet|Bytes|Gemiddeld|Lage geheugenlimiet, van configuratiebestand.|ServerResourceType|
|MemoryLimitVertiPaq|Geheugen: Memory Limit VertiPaq|Bytes|Gemiddeld|In-memory limiet, van configuratiebestand.|ServerResourceType|
|Quota|Geheugen: Quota|Bytes|Gemiddeld|Het huidige geheugenquotum, in bytes. Geheugenquotum wordt ook wel bekend als een geheugenreservering grant of geheugen.|ServerResourceType|
|QuotaBlocked|Geheugen: Geblokkeerd quotum|Count|Gemiddeld|Huidig aantal quotumaanvragen dat is geblokkeerd totdat andere geheugenquotums worden vrijgemaakt.|ServerResourceType|
|VertiPaqNonpaged|Geheugen: VertiPaq Nonpaged|Bytes|Gemiddeld|Bytes van het geheugen in de werkset voor gebruik door de in-memory engine vergrendeld.|ServerResourceType|
|VertiPaqPaged|Geheugen: VertiPaq Paged|Bytes|Gemiddeld|Bytes wisselbaar geheugen in gebruik voor in-memory-gegevens.|ServerResourceType|
|RowsReadPerSec|Verwerken: Gelezen rijen per seconde|CountPerSecond|Gemiddeld|Het aantal rijen lezen van alle relationele databases.|ServerResourceType|
|RowsConvertedPerSec|Verwerken: Rijen per seconde worden geconverteerd|CountPerSecond|Gemiddeld|Het aantal rijen dat tijdens verwerking is geconverteerd.|ServerResourceType|
|RowsWrittenPerSec|Verwerken: Geschreven rijen per seconde|CountPerSecond|Gemiddeld|Het aantal rijen dat tijdens de verwerking is geschreven.|ServerResourceType|
|CommandPoolBusyThreads|Threads: Opdracht pool-Jobs onderzoeken|Count|Gemiddeld|Het aantal actieve threads in de opdrachtthreadpool.|ServerResourceType|
|CommandPoolIdleThreads|Threads: Opdracht groep niet-actieve threads|Count|Gemiddeld|Het aantal niet-actieve threads in de opdrachtthreadpool.|ServerResourceType|
|LongParsingBusyThreads|Threads: Threads voor lang parseren bezet|Count|Gemiddeld|Het aantal actieve threads in de ThreadPool voor lang parseren.|ServerResourceType|
|LongParsingIdleThreads|Threads: Niet-actieve threads lang parseren|Count|Gemiddeld|Het aantal niet-actieve threads in de ThreadPool voor lang parseren.|ServerResourceType|
|LongParsingJobQueueLength|Threads: Lengte van taakwachtrij lang parseren|Count|Gemiddeld|Het aantal taken in de wachtrij van de ThreadPool voor lang parseren.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Threads: Groep bezet i/o-taakthreads van verwerkende|Count|Gemiddeld|Het aantal threads waarmee i/o-taken worden uitgevoerd in de verwerkende ThreadPool.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Threads: Threads van verwerkende pool bezet niet-I/O|Count|Gemiddeld|Het aantal threads waarmee niet-I/O-taken worden uitgevoerd in de verwerkende ThreadPool.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Threads: Van verwerkende pool lengte van taakwachtrij i/o|Count|Gemiddeld|Het aantal i/o-taken in de wachtrij van de verwerkende ThreadPool.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Threads: Groep niet-actieve i/o-taakthreads van verwerkende|Count|Gemiddeld|Het aantal niet-actieve threads voor i/o-taken in de verwerkende ThreadPool.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Threads: Threads van verwerkende pool niet-actieve niet-I/O|Count|Gemiddeld|Het aantal niet-actieve threads in de verwerkende ThreadPool toegewezen aan niet-I/O-taken.|ServerResourceType|
|QueryPoolIdleThreads|Threads: Niet-actieve threads voor query-groep|Count|Gemiddeld|Het aantal niet-actieve threads voor i/o-taken in de verwerkende ThreadPool.|ServerResourceType|
|QueryPoolJobQueueLength|Threads: Lengte van taakwachtrij pool query|Count|Gemiddeld|Het aantal taken in de wachtrij van de querythreadpool.|ServerResourceType|
|ShortParsingBusyThreads|Threads: Kort parseren actieve threads|Count|Gemiddeld|Het aantal actieve threads in de ThreadPool voor kort parseren.|ServerResourceType|
|ShortParsingIdleThreads|Threads: Niet-actieve threads van kort parseren|Count|Gemiddeld|Het aantal niet-actieve threads in de ThreadPool voor kort parseren.|ServerResourceType|
|ShortParsingJobQueueLength|Threads: Kort parseren lengte van taakwachtrij|Count|Gemiddeld|Het aantal taken in de wachtrij van de ThreadPool voor kort parseren.|ServerResourceType|
|memory_thrashing_metric|Geheugenthrashing|Procent|Gemiddeld|Gemiddelde geheugenthrashing.|ServerResourceType|
|mashup_engine_qpu_metric|M-Engine QPU|Count|Gemiddeld|QPU-gebruik door mashupengineprocessen|ServerResourceType|
|mashup_engine_memory_metric|M-Enginegeheugen|Bytes|Gemiddeld|Geheugengebruik door mashup-engineprocessen|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalRequests|Totaal aantal Gateway-aanvragen|Count|Totaal|Aantal gateway-aanvragen|Locatie, hostnaam|
|Succesfulrequests|Geslaagde Gateway-aanvragen|Count|Totaal|Aantal geslaagde gateway-aanvragen|Locatie, hostnaam|
|UnauthorizedRequests|Niet-gemachtigde Gateway-aanvragen|Count|Totaal|Aantal niet-gemachtigde gateway-aanvragen|Locatie, hostnaam|
|FailedRequests|Mislukte Gateway-aanvragen|Count|Totaal|Aantal fouten in de gateway-aanvragen|Locatie, hostnaam|
|OtherRequests|Overige Gateway-aanvragen|Count|Totaal|Het aantal overige gateway-aanvragen|Locatie, hostnaam|
|Duur|Totale duur van de Gateway-aanvragen|Milliseconden|Gemiddeld|Totale duur van de Gateway-aanvragen in milliseconden|Locatie, hostnaam|
|Capaciteit|Capaciteit|Procent|Gemiddeld|Gebruik metrische gegevens voor ApiManagement-service|Locatie|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalJob|Totaal aantal taken|Count|Totaal|Het totale aantal taken|Status-Runbook|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CoreCount|Toegewezen Kerngeheugens|Count|Totaal|Totale aantal toegewezen kerngeheugens in de batch-account|Er zijn geen dimensies|
|TotalNodeCount|Aantal toegewezen knooppunten|Count|Totaal|Totale aantal toegewezen knooppunten in het batch-account|Er zijn geen dimensies|
|LowPriorityCoreCount|Aantal LowPriority-kernen|Count|Totaal|Totale aantal kernen in de batch-account met lage prioriteit|Er zijn geen dimensies|
|TotalLowPriorityNodeCount|Aantal knooppunten met lage prioriteit|Count|Totaal|Totale aantal knooppunten met lage prioriteit in de batch-account|Er zijn geen dimensies|
|CreatingNodeCount|Het aantal knooppunten maken|Count|Totaal|Aantal knooppunten wordt gemaakt|Er zijn geen dimensies|
|StartingNodeCount|Aantal knooppunten starten|Count|Totaal|Aantal knooppunten starten|Er zijn geen dimensies|
|WaitingForStartTaskNodeCount|Wachten op voor begin taak aantal knooppunten|Count|Totaal|Aantal knooppunten die wachten op de taak starten om te voltooien|Er zijn geen dimensies|
|StartTaskFailedNodeCount|Begintaak aantal knooppunten is mislukt|Count|Totaal|Aantal knooppunten waar de taak starten is mislukt|Er zijn geen dimensies|
|IdleNodeCount|Aantal niet-actieve knooppunten|Count|Totaal|Aantal niet-actieve knooppunten|Er zijn geen dimensies|
|OfflineNodeCount|Aantal offline knooppunten|Count|Totaal|Aantal knooppunten dat offline|Er zijn geen dimensies|
|RebootingNodeCount|Aantal knooppunten opnieuw opstarten|Count|Totaal|Aantal knooppunten opnieuw opstarten|Er zijn geen dimensies|
|ReimagingNodeCount|Installatiekopie terugzetten aantal knooppunten|Count|Totaal|Aantal knooppunten installatiekopie terugzetten|Er zijn geen dimensies|
|RunningNodeCount|Aantal actieve knooppunten|Count|Totaal|Aantal actieve knooppunten|Er zijn geen dimensies|
|LeavingPoolNodeCount|Aantal van de knooppunten groep verlaten|Count|Totaal|Aantal knooppunten dat de groep verlaten|Er zijn geen dimensies|
|UnusableNodeCount|Aantal onbruikbaar knooppunten|Count|Totaal|Aantal onbruikbaar knooppunten|Er zijn geen dimensies|
|PreemptedNodeCount|Aantal knooppunten verschoven|Count|Totaal|Aantal knooppunten verschoven|Er zijn geen dimensies|
|TaskStartEvent|Taak Start gebeurtenissen|Count|Totaal|Totale aantal taken die zijn gestart|Er zijn geen dimensies|
|TaskCompleteEvent|Taak voltooid gebeurtenissen|Count|Totaal|Totale aantal taken die zijn voltooid|Er zijn geen dimensies|
|TaskFailEvent|Taak is mislukt-gebeurtenissen|Count|Totaal|Totale aantal taken die in een foutstatus zijn voltooid|Er zijn geen dimensies|
|PoolCreateEvent|Groep van toepassingen gebeurtenissen maken|Count|Totaal|Totaal aantal pools die zijn gemaakt|Er zijn geen dimensies|
|PoolResizeStartEvent|Pool Resize Start gebeurtenissen|Count|Totaal|Totaal aantal pool-formaat die zijn gestart|Er zijn geen dimensies|
|PoolResizeCompleteEvent|Complete pool Resize-gebeurtenissen|Count|Totaal|Totaal aantal pool wijzigen van de grootte die zijn voltooid|Er zijn geen dimensies|
|PoolDeleteStartEvent|Pool verwijderen starten gebeurtenissen|Count|Totaal|Totaal aantal toepassingen verwijderd die zijn gestart|Er zijn geen dimensies|
|PoolDeleteCompleteEvent|Pool verwijderen voltooid gebeurtenissen|Count|Totaal|Totaal aantal toepassingen verwijderd die zijn voltooid|Er zijn geen dimensies|
|JobDeleteCompleteEvent|Taak verwijderen voltooid gebeurtenissen|Count|Totaal|Totale aantal taken die zijn verwijderd.|Er zijn geen dimensies|
|JobDeleteStartEvent|Taak Start gebeurtenissen|Count|Totaal|Totale aantal taken die u hebt aangevraagd om te worden verwijderd.|Er zijn geen dimensies|
|JobDisableCompleteEvent|Taak uitschakelen Complete-gebeurtenissen|Count|Totaal|Totale aantal taken die zijn uitgeschakeld.|Er zijn geen dimensies|
|JobDisableStartEvent|Taak uitschakelen Start gebeurtenissen|Count|Totaal|Totale aantal taken die u hebt aangevraagd om te worden uitgeschakeld.|Er zijn geen dimensies|
|JobStartEvent|Taak Start gebeurtenissen|Count|Totaal|Totale aantal taken dat is gestart.|Er zijn geen dimensies|
|JobTerminateCompleteEvent|Taak beëindigen Complete-gebeurtenissen|Count|Totaal|Totale aantal taken die zijn beëindigd.|Er zijn geen dimensies|
|JobTerminateStartEvent|Taak beëindigen Start gebeurtenissen|Count|Totaal|Totale aantal taken die u hebt aangevraagd te beëindigen.|Er zijn geen dimensies|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|connectedclients|Verbonden clients|Count|Maximum||ShardId|
|totalcommandsprocessed|Totaal aantal bewerkingen|Count|Totaal||ShardId|
|cachehits|Cachetreffers|Count|Totaal||ShardId|
|cachemisses|Cachemissers|Count|Totaal||ShardId|
|getcommands|Opgehaalde items|Count|Totaal||ShardId|
|setcommands|Sets|Count|Totaal||ShardId|
|operationsPerSecond|Bewerkingen Per seconde|Count|Maximum||ShardId|
|evictedkeys|Verwijderde sleutels|Count|Totaal||ShardId|
|totalkeys|Totaal aantal sleutels|Count|Maximum||ShardId|
|expiredkeys|Verlopen sleutels|Count|Totaal||ShardId|
|usedmemory|Gebruikt geheugen|Bytes|Maximum||ShardId|
|usedmemorypercentage|Percentage gebruikt geheugen|Procent|Maximum||ShardId|
|usedmemoryRss|Gebruikt geheugen RSS|Bytes|Maximum||ShardId|
|serverLoad|Serverbelasting|Procent|Maximum||ShardId|
|cacheWrite|Cache schrijven|BytesPerSecond|Maximum||ShardId|
|cacheRead|Gelezen uit cache|BytesPerSecond|Maximum||ShardId|
|percentProcessorTime|CPU|Procent|Maximum||ShardId|
|cacheLatency|Cache Latency Microseconds (Preview)|Count|Gemiddeld||ShardId, SampleType|
|fouten|Fouten|Count|Maximum||ShardId, ErrorType|
|connectedclients0|Verbonden Clients (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed0|Totaal aantal bewerkingen (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|cachehits0|Treffers in cache (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|cachemisses0|Missers in cache (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|getcommands0|Hiermee wordt opgehaald (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|setcommands0|Sets (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond0|Bewerkingen Per seconde (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys0|Verwijderde sleutels (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|totalkeys0|Totaal aantal sleutels (Shard 0)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys0|Verlopen sleutels (Shard 0)|Count|Totaal||Er zijn geen dimensies|
|usedmemory0|Gebruikt geheugen (Shard 0)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss0|Gebruikt geheugen RSS (Shard 0)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad0|Belasting van de server (Shard 0)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite0|Cache schrijven (Shard 0)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead0|Gelezen uit cache (Shard 0)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime0|CPU (Shard 0)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients1|Verbonden Clients (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed1|Totaal aantal bewerkingen (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|cachehits1|Treffers in cache (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|cachemisses1|Missers in cache (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|getcommands1|Hiermee wordt opgehaald (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|setcommands1|Sets (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond1|Bewerkingen Per seconde (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys1|Verwijderde sleutels (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|totalkeys1|Totaal aantal sleutels (Shard 1)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys1|Verlopen sleutels (Shard 1)|Count|Totaal||Er zijn geen dimensies|
|usedmemory1|Gebruikt geheugen (Shard 1)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss1|Gebruikt geheugen RSS (Shard 1)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad1|Belasting van de server (Shard 1)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite1|Cache schrijven (Shard 1)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead1|Gelezen uit cache (Shard 1)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime1|CPU (Shard 1)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients2|Verbonden Clients (Shard 2)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed2|Totaal aantal bewerkingen (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|cachehits2|Treffers in cache (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|cachemisses2|Missers in cache (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|getcommands2|Hiermee wordt opgehaald (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|setcommands2|Sets (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond2|Bewerkingen Per seconde (Shard 2)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys2|Verwijderde sleutels (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|totalkeys2|Totaal aantal sleutels (Shard 2)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys2|Verlopen sleutels (Shard 2)|Count|Totaal||Er zijn geen dimensies|
|usedmemory2|Gebruikt geheugen (Shard 2)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss2|Gebruikt geheugen RSS (Shard 2)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad2|Belasting van de server (Shard 2)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite2|Cache schrijven (Shard 2)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead2|Gelezen uit cache (Shard 2)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime2|CPU (Shard 2)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients3|Verbonden Clients (Shard 3)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed3|Totaal aantal bewerkingen (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|cachehits3|Treffers in cache (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|cachemisses3|Missers in cache (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|getcommands3|Hiermee wordt opgehaald (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|setcommands3|Sets (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond3|Bewerkingen Per seconde (Shard 3)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys3|Verwijderde sleutels (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|totalkeys3|Totaal aantal sleutels (Shard 3)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys3|Verlopen sleutels (Shard 3)|Count|Totaal||Er zijn geen dimensies|
|usedmemory3|Gebruikt geheugen (Shard 3)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss3|Gebruikt geheugen RSS (Shard 3)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad3|Server Load (Shard 3)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite3|Cache schrijven (Shard 3)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead3|Gelezen uit cache (Shard 3)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime3|CPU (Shard 3)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients4|Verbonden Clients (Shard 4)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed4|Totaal aantal bewerkingen (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|cachehits4|Treffers in cache (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|cachemisses4|Missers in cache (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|getcommands4|Hiermee wordt opgehaald (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|setcommands4|Sets (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond4|Bewerkingen Per seconde (Shard 4)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys4|Verwijderde sleutels (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|totalkeys4|Totaal aantal sleutels (Shard 4)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys4|Verlopen sleutels (Shard 4)|Count|Totaal||Er zijn geen dimensies|
|usedmemory4|Gebruikt geheugen (Shard 4)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss4|Gebruikt geheugen RSS (Shard 4)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad4|Belasting van de server (Shard 4)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite4|Cache schrijven (Shard 4)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead4|Gelezen uit cache (Shard 4)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime4|CPU (Shard 4)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients5|Verbonden Clients (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed5|Totaal aantal bewerkingen (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|cachehits5|Treffers in cache (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|cachemisses5|Missers in cache (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|getcommands5|Hiermee wordt opgehaald (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|setcommands5|Sets (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond5|Bewerkingen Per seconde (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys5|Verwijderde sleutels (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|totalkeys5|Totaal aantal sleutels (Shard 5)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys5|Verlopen sleutels (Shard 5)|Count|Totaal||Er zijn geen dimensies|
|usedmemory5|Gebruikt geheugen (Shard 5)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss5|Gebruikt geheugen RSS (Shard 5)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad5|Server Load (Shard 5)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite5|Cache schrijven (Shard 5)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead5|Cache Read (Shard 5)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime5|CPU (Shard 5)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients6|Verbonden Clients (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed6|Totaal aantal bewerkingen (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|cachehits6|Treffers in cache (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|cachemisses6|Missers in cache (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|getcommands6|Hiermee wordt opgehaald (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|setcommands6|Sets (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond6|Operations Per Second (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys6|Verwijderde sleutels (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|totalkeys6|Totaal aantal sleutels (Shard 6)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys6|Verlopen sleutels (Shard 6)|Count|Totaal||Er zijn geen dimensies|
|usedmemory6|Gebruikt geheugen (Shard 6)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss6|Gebruikt geheugen RSS (Shard 6)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad6|Server Load (Shard 6)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite6|Cache schrijven (Shard 6)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead6|Cache Read (Shard 6)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime6|CPU (Shard 6)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients7|Verbonden Clients (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed7|Totaal aantal bewerkingen (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|cachehits7|Treffers in cache (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|cachemisses7|Missers in cache (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|getcommands7|Hiermee wordt opgehaald (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|setcommands7|Sets (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond7|Operations Per Second (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys7|Verwijderde sleutels (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|totalkeys7|Totaal aantal sleutels (Shard 7)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys7|Verlopen sleutels (Shard 7)|Count|Totaal||Er zijn geen dimensies|
|usedmemory7|Gebruikt geheugen (Shard 7)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss7|Gebruikt geheugen RSS (Shard 7)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad7|Server Load (Shard 7)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite7|Cache schrijven (Shard 7)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead7|Cache Read (Shard 7)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime7|CPU (Shard 7)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients8|Verbonden Clients (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed8|Totaal aantal bewerkingen (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|cachehits8|Treffers in cache (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|cachemisses8|Cache Misses (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|getcommands8|Hiermee wordt opgehaald (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|setcommands8|Sets (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond8|Operations Per Second (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys8|Verwijderde sleutels (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|totalkeys8|Totaal aantal sleutels (Shard 8)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys8|Verlopen sleutels (Shard 8)|Count|Totaal||Er zijn geen dimensies|
|usedmemory8|Gebruikt geheugen (Shard 8)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss8|Gebruikt geheugen RSS (Shard 8)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad8|Server Load (Shard 8)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite8|Cache schrijven (Shard 8)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead8|Cache Read (Shard 8)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime8|CPU (Shard 8)|Procent|Maximum||Er zijn geen dimensies|
|connectedclients9|Verbonden Clients (Shard 9)|Count|Maximum||Er zijn geen dimensies|
|totalcommandsprocessed9|Totaal aantal bewerkingen (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|cachehits9|Treffers in cache (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|cachemisses9|Missers in cache (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|getcommands9|Hiermee wordt opgehaald (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|setcommands9|Sets (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|operationsPerSecond9|Bewerkingen Per seconde (Shard 9)|Count|Maximum||Er zijn geen dimensies|
|evictedkeys9|Verwijderde sleutels (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|totalkeys9|Totaal aantal sleutels (Shard 9)|Count|Maximum||Er zijn geen dimensies|
|expiredkeys9|Verlopen sleutels (Shard 9)|Count|Totaal||Er zijn geen dimensies|
|usedmemory9|Gebruikt geheugen (Shard 9)|Bytes|Maximum||Er zijn geen dimensies|
|usedmemoryRss9|Gebruikt geheugen RSS (Shard 9)|Bytes|Maximum||Er zijn geen dimensies|
|serverLoad9|Server Load (Shard 9)|Procent|Maximum||Er zijn geen dimensies|
|cacheWrite9|Cache schrijven (Shard 9)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|cacheRead9|Cache Read (Shard 9)|BytesPerSecond|Maximum||Er zijn geen dimensies|
|percentProcessorTime9|CPU (Shard 9)|Procent|Maximum||Er zijn geen dimensies|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddeld|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt.|Er zijn geen dimensies|
|Netwerk in|Netwerk in|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer).|Er zijn geen dimensies|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer).|Er zijn geen dimensies|
|Gelezen bytes per seconde op de schijf|Schijf lezen|BytesPerSecond|Gemiddeld|Gemiddeld aantal gelezen bytes op de schijf tijdens de bewakingsperiode.|Er zijn geen dimensies|
|Geschreven bytes per seconde|Schijf schrijven|BytesPerSecond|Gemiddeld|Gemiddeld aantal geschreven bytes op de schijf tijdens de bewakingsperiode.|Er zijn geen dimensies|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Gelezen IOP's op de schijf.|Er zijn geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Geschreven IOP's op de schijf.|Er zijn geen dimensies|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddeld|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt.|RoleInstanceId|
|Netwerk in|Netwerk in|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer).|RoleInstanceId|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer).|RoleInstanceId|
|Gelezen bytes per seconde op de schijf|Schijf lezen|BytesPerSecond|Gemiddeld|Gemiddeld aantal gelezen bytes op de schijf tijdens de bewakingsperiode.|RoleInstanceId|
|Geschreven bytes per seconde|Schijf schrijven|BytesPerSecond|Gemiddeld|Gemiddeld aantal geschreven bytes op de schijf tijdens de bewakingsperiode.|RoleInstanceId|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Gelezen IOP's op de schijf.|RoleInstanceId|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Geschreven IOP's op de schijf.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalCalls|Totaal aantal aanroepen|Count|Totaal|Totaal aantal aanroepen.|Er zijn geen dimensies|
|SuccessfulCalls|Geslaagde aanroepen|Count|Totaal|Aantal geslaagde aanroepen.|Er zijn geen dimensies|
|TotalErrors|Totaal aantal fouten|Count|Totaal|Het totaal aantal aanroepen met een foutbericht (HTTP-antwoordcode 4xx of 5xx).|Er zijn geen dimensies|
|BlockedCalls|Geblokkeerde aanroepen|Count|Totaal|Het aantal aanroepen die de tarief- of quotumlimiet hebben overschreden.|Er zijn geen dimensies|
|ServerErrors|Serverfouten|Count|Totaal|Het aantal aanroepen met een interne servicefout (HTTP-responscode 5xx).|Er zijn geen dimensies|
|ClientErrors|Clientfouten|Count|Totaal|Het aantal aanroepen met een fout aan de clientzijde (HTTP-responscode 4xx).|Er zijn geen dimensies|
|DataIn|Inkomende gegevens|Bytes|Totaal|Grootte van inkomende gegevens in bytes.|Er zijn geen dimensies|
|DataOut|Uitgaande gegevens|Bytes|Totaal|Grootte van uitgaande gegevens in bytes.|Er zijn geen dimensies|
|Latentie|Latentie|MilliSeconds|Gemiddeld|Latentie in milliseconden|Er zijn geen dimensies|
|CharactersTranslated|Tekens vertaald|Count|Totaal|Totaal aantal tekens in inkomende tekstaanvraag.|Er zijn geen dimensies|
|SpeechSessionDuration|Duur van de spraaksessie|Seconden|Totaal|Totale duur van de spraaksessie in seconden.|Er zijn geen dimensies|
|TotalTransactions|Totaal van transacties|Count|Totaal|Het totale aantal transacties.|Er zijn geen dimensies|
|TotalTokenCalls|Totaal aantal tokenaanroepen|Count|Totaal|Het totale aantal tokenaanroepen.|Er zijn geen dimensies|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddeld|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt|Er zijn geen dimensies|
|Netwerk in|Netwerk in|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer)|Er zijn geen dimensies|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer)|Er zijn geen dimensies|
|Gelezen bytes op de schijf|Gelezen bytes op de schijf|Bytes|Totaal|Totaal aantal gelezen bytes van schijf tijdens de controleperiode|Er zijn geen dimensies|
|Geschreven bytes op de schijf|Geschreven bytes op de schijf|Bytes|Totaal|Totaal aantal bytes geschreven naar de schijf tijdens de controleperiode|Er zijn geen dimensies|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Gelezen IOP's op de schijf|Er zijn geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Geschreven IOP's op de schijf|Er zijn geen dimensies|
|Resterend CPU-tegoed|Resterend CPU-tegoed|Count|Gemiddeld|Totale tegoed beschikbaar voor burst|Er zijn geen dimensies|
|Verbruikt CPU-tegoed|Verbruikt CPU-tegoed|Count|Gemiddeld|Totale tegoed gebruikt door de virtuele machine|Er zijn geen dimensies|
|Per schijf gelezen bytes per seconde|Van de gegevensschijf gelezen bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal Bytes per seconde voor het lezen van één schijf tijdens de controleperiode|SlotId|
|Per schijf geschreven bytes per seconde|Naar de gegevensschijf geschreven bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal naar één schijf tijdens de controleperiode geschreven Bytes per seconde|SlotId|
|Leesbewerkingen per schijf per seconde|Leesbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het lezen van één schijf tijdens de controleperiode|SlotId|
|Schrijfbewerkingen per schijf per seconde|Schrijfbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het schrijven naar één schijf tijdens de controleperiode|SlotId|
|Wachtrijomvang per schijf|Gegevensschijf Wachtrijdiepte (Preview)|Count|Gemiddeld|Wachtrijomvang (of wachtrijlengte) voor de gegevensschijf|SlotId|
|Per besturingssysteemschijf gelezen bytes per seconde|Van de besturingssysteemschijf gelezen bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal Bytes per seconde van één schijf tijdens de controleperiode voor de besturingssysteemschijf gelezen.|Er zijn geen dimensies|
|Per besturingssysteemschijf geschreven bytes per seconde|Naar de besturingssysteemschijf geschreven bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal naar één schijf tijdens de controleperiode voor de besturingssysteemschijf geschreven Bytes per seconde|Er zijn geen dimensies|
|Leesbewerkingen per besturingssysteemschijf per seconde|Leesbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het lezen van één schijf tijdens de controleperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Schrijfbewerkingen per besturingssysteemschijf per seconde|Schrijfbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het schrijven naar één schijf tijdens de controleperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Wachtrijomvang per besturingssysteemschijf|Besturingssysteemschijf Wachtrijomvang (Preview)|Count|Gemiddeld|Wachtrijomvang (of wachtrijlengte) voor de besturingssysteemschijf|Er zijn geen dimensies|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddeld|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt|Er zijn geen dimensies|
|Netwerk in|Netwerk in|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer)|Er zijn geen dimensies|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer)|Er zijn geen dimensies|
|Gelezen bytes op de schijf|Gelezen bytes op de schijf|Bytes|Totaal|Totaal aantal gelezen bytes van schijf tijdens de controleperiode|Er zijn geen dimensies|
|Geschreven bytes op de schijf|Geschreven bytes op de schijf|Bytes|Totaal|Totaal aantal bytes geschreven naar de schijf tijdens de controleperiode|Er zijn geen dimensies|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Gelezen IOP's op de schijf|Er zijn geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Geschreven IOP's op de schijf|Er zijn geen dimensies|
|Resterend CPU-tegoed|Resterend CPU-tegoed|Count|Gemiddeld|Totale tegoed beschikbaar voor burst|Er zijn geen dimensies|
|Verbruikt CPU-tegoed|Verbruikt CPU-tegoed|Count|Gemiddeld|Totale tegoed gebruikt door de virtuele machine|Er zijn geen dimensies|
|Per schijf gelezen bytes per seconde|Van de gegevensschijf gelezen bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal Bytes per seconde voor het lezen van één schijf tijdens de controleperiode|SlotId|
|Per schijf geschreven bytes per seconde|Naar de gegevensschijf geschreven bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal naar één schijf tijdens de controleperiode geschreven Bytes per seconde|SlotId|
|Leesbewerkingen per schijf per seconde|Leesbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het lezen van één schijf tijdens de controleperiode|SlotId|
|Schrijfbewerkingen per schijf per seconde|Schrijfbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het schrijven naar één schijf tijdens de controleperiode|SlotId|
|Wachtrijomvang per schijf|Gegevensschijf Wachtrijdiepte (Preview)|Count|Gemiddeld|Wachtrijomvang (of wachtrijlengte) voor de gegevensschijf|SlotId|
|Per besturingssysteemschijf gelezen bytes per seconde|De Besturingssysteemschijf gelezen Bytes per seconde|CountPerSecond|Gemiddeld|Totaal aantal Bytes per seconde van één schijf tijdens de controleperiode voor de besturingssysteemschijf gelezen.|Er zijn geen dimensies|
|Per besturingssysteemschijf geschreven bytes per seconde|Naar de besturingssysteemschijf geschreven bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal naar één schijf tijdens de controleperiode voor de besturingssysteemschijf geschreven Bytes per seconde|Er zijn geen dimensies|
|Leesbewerkingen per besturingssysteemschijf per seconde|Leesbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het lezen van één schijf tijdens de controleperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Schrijfbewerkingen per besturingssysteemschijf per seconde|Schrijfbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het schrijven naar één schijf tijdens de controleperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Wachtrijomvang per besturingssysteemschijf|Besturingssysteemschijf Wachtrijomvang (Preview)|Count|Gemiddeld|Wachtrijomvang (of wachtrijlengte) voor de besturingssysteemschijf|Er zijn geen dimensies|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddeld|Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt|Er zijn geen dimensies|
|Netwerk in|Netwerk in|Bytes|Totaal|Het aantal ontvangen bytes op alle netwerkinterfaces door de virtuele machine(s) (binnenkomend verkeer)|Er zijn geen dimensies|
|Netwerk uit|Netwerk uit|Bytes|Totaal|Het aantal verzonden bytes op alle netwerkinterfaces door de virtuele machine(s) (uitgaand verkeer)|Er zijn geen dimensies|
|Gelezen bytes op de schijf|Gelezen bytes op de schijf|Bytes|Totaal|Totaal aantal gelezen bytes van schijf tijdens de controleperiode|Er zijn geen dimensies|
|Geschreven bytes op de schijf|Geschreven bytes op de schijf|Bytes|Totaal|Totaal aantal bytes geschreven naar de schijf tijdens de controleperiode|Er zijn geen dimensies|
|Leesbewerkingen op de schijf/seconde|Leesbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Gelezen IOP's op de schijf|Er zijn geen dimensies|
|Schrijfbewerkingen op de schijf/seconde|Schrijfbewerkingen op de schijf/seconde|CountPerSecond|Gemiddeld|Geschreven IOP's op de schijf|Er zijn geen dimensies|
|Resterend CPU-tegoed|Resterend CPU-tegoed|Count|Gemiddeld|Totale tegoed beschikbaar voor burst|Er zijn geen dimensies|
|Verbruikt CPU-tegoed|Verbruikt CPU-tegoed|Count|Gemiddeld|Totale tegoed gebruikt door de virtuele machine|Er zijn geen dimensies|
|Per schijf gelezen bytes per seconde|Van de gegevensschijf gelezen bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal Bytes per seconde voor het lezen van één schijf tijdens de controleperiode|SlotId|
|Per schijf geschreven bytes per seconde|Naar de gegevensschijf geschreven bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal naar één schijf tijdens de controleperiode geschreven Bytes per seconde|SlotId|
|Leesbewerkingen per schijf per seconde|Leesbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het lezen van één schijf tijdens de controleperiode|SlotId|
|Schrijfbewerkingen per schijf per seconde|Schrijfbewerkingen op de gegevensschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het schrijven naar één schijf tijdens de controleperiode|SlotId|
|Wachtrijomvang per schijf|Gegevensschijf Wachtrijdiepte (Preview)|Count|Gemiddeld|Wachtrijomvang (of wachtrijlengte) voor de gegevensschijf|SlotId|
|Per besturingssysteemschijf gelezen bytes per seconde|Van de besturingssysteemschijf gelezen bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal Bytes per seconde van één schijf tijdens de controleperiode voor de besturingssysteemschijf gelezen.|Er zijn geen dimensies|
|Per besturingssysteemschijf geschreven bytes per seconde|Naar de besturingssysteemschijf geschreven bytes per seconde (preview)|CountPerSecond|Gemiddeld|Totaal aantal naar één schijf tijdens de controleperiode voor de besturingssysteemschijf geschreven Bytes per seconde|Er zijn geen dimensies|
|Leesbewerkingen per besturingssysteemschijf per seconde|Leesbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het lezen van één schijf tijdens de controleperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Schrijfbewerkingen per besturingssysteemschijf per seconde|Schrijfbewerkingen op de besturingssysteemschijf per seconde (preview)|CountPerSecond|Gemiddeld|Totale IOPS uitgevoerde tijdens het schrijven naar één schijf tijdens de controleperiode voor de besturingssysteemschijf|Er zijn geen dimensies|
|Wachtrijomvang per besturingssysteemschijf|Besturingssysteemschijf Wachtrijomvang (Preview)|Count|Gemiddeld|Wachtrijomvang (of wachtrijlengte) voor de besturingssysteemschijf|Er zijn geen dimensies|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuUsage|CPU-gebruik|Count|Gemiddeld|CPU-gebruik op alle kerngeheugens in millicores.|containerName|
|MemoryUsage|Geheugengebruik|Bytes|Gemiddeld|Totaal geheugengebruik in bytes.|containerName|
|NetworkBytesReceivedPerSecond|Netwerkbytes ontvangen per seconde|Bytes|Gemiddeld|Het aantal bytes dat het netwerk per seconde heeft ontvangen.|Er zijn geen dimensies|
|NetworkBytesTransmittedPerSecond|Netwerkbytes verzonden per seconde|Bytes|Gemiddeld|Het aantal bytes dat het netwerk per seconde heeft verzonden.|Er zijn geen dimensies|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Totaal aantal beschikbare cpu-kernen in een beheerde cluster|Count|Totaal|Totaal aantal beschikbare cpu-kernen in een beheerde cluster|Er zijn geen dimensies|
|kube_node_status_allocatable_memory_bytes|Totale hoeveelheid beschikbaar geheugen in een beheerde cluster|Bytes|Totaal|Totale hoeveelheid beschikbaar geheugen in een beheerde cluster|Er zijn geen dimensies|
|kube_pod_status_ready|Het aantal schillen in de status gereed|Count|Totaal|Het aantal schillen in de status gereed|naamruimte, pod|
|kube_node_status_condition|Statussen van verschillende situaties waarbij het knooppunt|Count|Totaal|Statussen van verschillende situaties waarbij het knooppunt|voorwaarde, status, knooppunt|
|kube_pod_status_phase|Het aantal schillen per fase|Count|Totaal|Het aantal schillen per fase|fase, naamruimte, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|DCIApiCalls|Customer Insights API-aanroepen|Count|Totaal||Er zijn geen dimensies|
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
|DCIOutboundKpiExportStarted|Uitgaande Kpi exporteren is gestart|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundKpiRecordCount|Aantal uitgaande Kpi-records|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundProfileExportCount|Telling van uitgaande profiel exporteren|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportFailed|Uitgaande profiel exporteren is mislukt|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportSucceeded|Uitgaande profiel exporteren is geslaagd|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialKpiExportFailed|Uitgaande initiële Kpi exporteren is mislukt|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialKpiExportSucceeded|Uitgaande initiële Kpi exporteren is voltooid|Seconden|Totaal||Er zijn geen dimensies|
|DCIOutboundInitialProfileExportDurationInSeconds|Uitgaande profiel exporteren duur In seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiFailed|Adla-taak voor standaard-Kpi is mislukt In seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiTimeOut|Adla-taak voor Standard Kpi time-out In seconden|Seconden|Totaal||Er zijn geen dimensies|
|AdlaJobForStandardKpiCompleted|Adla-taak voor standaard-Kpi In een paar seconden voltooid|Seconden|Totaal||Er zijn geen dimensies|
|ImportASAValuesFailed|Kan het importeren ASA waarden niet tellen|Count|Totaal||Er zijn geen dimensies|
|ImportASAValuesSucceeded|Importeren ASA waarden tellen is voltooid|Count|Totaal||Er zijn geen dimensies|
|DCIProfilesCount|Profiel-exemplaren|Count|Laatste||Er zijn geen dimensies|
|DCIInteractionsPerMonthCount|Interacties per maand tellen|Count|Laatste||Er zijn geen dimensies|
|DCIKpisCount|KPI-aantal|Count|Laatste||Er zijn geen dimensies|
|DCISegmentsCount|Aantal segmenten|Count|Laatste||Er zijn geen dimensies|
|DCIPredictiveMatchPoliciesCount|Aantal voorspellende overeenkomsten|Count|Laatste||Er zijn geen dimensies|
|DCIPredictionsCount|Aantal voorspelling|Count|Laatste||Er zijn geen dimensies|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|FailedRuns|Mislukte verwerkingen|Count|Totaal||pipelineName, activityName, windowEnd, windowStart|
|SuccessfulRuns|Geslaagde verwerkingen|Count|Totaal||pipelineName, activityName, windowEnd, windowStart|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PipelineFailedRuns|Pijplijn-runs metrische gegevens is mislukt|Count|Totaal||FailureType, naam|
|PipelineSucceededRuns|Pijplijn-runs metrische gegevens is voltooid|Count|Totaal||FailureType, naam|
|ActivityFailedRuns|Kan geen metrische gegevens van activiteiten wordt uitgevoerd|Count|Totaal||ActivityType, PipelineName, FailureType, geef de naam|
|ActivitySucceededRuns|Metrische gegevens de uitvoeringen van activiteiten is voltooid|Count|Totaal||ActivityType, PipelineName, FailureType, geef de naam|
|TriggerFailedRuns|Trigger wordt uitgevoerd metrische gegevens is mislukt|Count|Totaal||Name, FailureType|
|TriggerSucceededRuns|Trigger wordt uitgevoerd metrische gegevens is voltooid|Count|Totaal||Name, FailureType|
|IntegrationRuntimeCpuPercentage|Integration runtime CPU-gebruik|Procent|Gemiddeld||IntegrationRuntimeName, knooppuntnaam|
|IntegrationRuntimeAvailableMemory|Beschikbaar geheugen voor Integration runtime|Bytes|Gemiddeld||IntegrationRuntimeName, knooppuntnaam|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|JobEndedSuccess|Voltooide taken|Count|Totaal|Het aantal voltooide taken.|Er zijn geen dimensies|
|JobEndedFailure|Mislukte taken|Count|Totaal|Het aantal mislukte taken.|Er zijn geen dimensies|
|JobEndedCancelled|Geannuleerde taken|Count|Totaal|Aantal geannuleerde taken.|Er zijn geen dimensies|
|JobAUEndedSuccess|Geslaagde AU-tijd|Seconden|Totaal|Totale AU-tijd voor voltooide taken.|Er zijn geen dimensies|
|JobAUEndedFailure|Mislukte AU-tijd|Seconden|Totaal|Totale AU-tijd voor mislukte taken.|Er zijn geen dimensies|
|JobAUEndedCancelled|Geannuleerde AU-tijd|Seconden|Totaal|Totale AU-tijd voor de geannuleerde taken.|Er zijn geen dimensies|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TotalStorage|Totale opslag|Bytes|Maximum|Totale hoeveelheid gegevens die zijn opgeslagen in het account.|Er zijn geen dimensies|
|Datawritten door|Gegevens geschreven|Bytes|Totaal|Totale hoeveelheid gegevens geschreven naar het account.|Er zijn geen dimensies|
|DataRead|Gegevens lezen|Bytes|Totaal|Totale hoeveelheid gegevens gelezen uit het account.|Er zijn geen dimensies|
|WriteRequests|Aanvragen schrijven|Count|Totaal|Telling van gegevens schrijven aanvragen naar het account.|Er zijn geen dimensies|
|ReadRequests|Aanvragen lezen|Count|Totaal|Telling van gegevens gelezen aanvragen voor het account.|Er zijn geen dimensies|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Er zijn geen dimensies|
|memory_percent|Percentage geheugen|Procent|Gemiddeld|Percentage geheugen|Er zijn geen dimensies|
|io_consumption_percent|I/o-percentage|Procent|Gemiddeld|I/o-percentage|Er zijn geen dimensies|
|storage_percent|Percentage van de opslag|Procent|Gemiddeld|Percentage van de opslag|Er zijn geen dimensies|
|storage_used|Gebruikte opslag|Bytes|Gemiddeld|Gebruikte opslag|Er zijn geen dimensies|
|storage_limit|Limiet voor opslag|Bytes|Gemiddeld|Limiet voor opslag|Er zijn geen dimensies|
|serverlog_storage_percent|Server Log opslag procent|Procent|Gemiddeld|Server Log opslag procent|Er zijn geen dimensies|
|serverlog_storage_usage|Server logboekopslag gebruikt|Bytes|Gemiddeld|Server logboekopslag gebruikt|Er zijn geen dimensies|
|serverlog_storage_limit|Limiet voor opslag van server-logboek|Bytes|Gemiddeld|Limiet voor opslag van server-logboek|Er zijn geen dimensies|
|active_connections|Actieve verbindingen|Count|Gemiddeld|Actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|network_bytes_egress|Netwerk uit|Bytes|Totaal|Uitgaand netwerkverkeer voor actieve verbindingen|Er zijn geen dimensies|
|network_bytes_ingress|Netwerk in|Bytes|Totaal|Netwerk In voor de actieve verbindingen|Er zijn geen dimensies|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Er zijn geen dimensies|
|memory_percent|Percentage geheugen|Procent|Gemiddeld|Percentage geheugen|Er zijn geen dimensies|
|io_consumption_percent|I/o-percentage|Procent|Gemiddeld|I/o-percentage|Er zijn geen dimensies|
|storage_percent|Percentage van de opslag|Procent|Gemiddeld|Percentage van de opslag|Er zijn geen dimensies|
|storage_used|Gebruikte opslag|Bytes|Gemiddeld|Gebruikte opslag|Er zijn geen dimensies|
|storage_limit|Limiet voor opslag|Bytes|Gemiddeld|Limiet voor opslag|Er zijn geen dimensies|
|serverlog_storage_percent|Server Log opslag procent|Procent|Gemiddeld|Server Log opslag procent|Er zijn geen dimensies|
|serverlog_storage_usage|Server logboekopslag gebruikt|Bytes|Gemiddeld|Server logboekopslag gebruikt|Er zijn geen dimensies|
|serverlog_storage_limit|Limiet voor opslag van server-logboek|Bytes|Gemiddeld|Limiet voor opslag van server-logboek|Er zijn geen dimensies|
|active_connections|Actieve verbindingen|Count|Gemiddeld|Actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|seconds_behind_master|Vertraging van replicatie in een paar seconden|Count|Gemiddeld|Vertraging van replicatie in een paar seconden|Er zijn geen dimensies|
|network_bytes_egress|Netwerk uit|Bytes|Totaal|Uitgaand netwerkverkeer voor actieve verbindingen|Er zijn geen dimensies|
|network_bytes_ingress|Netwerk in|Bytes|Totaal|Netwerk In voor de actieve verbindingen|Er zijn geen dimensies|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Er zijn geen dimensies|
|memory_percent|Percentage geheugen|Procent|Gemiddeld|Percentage geheugen|Er zijn geen dimensies|
|io_consumption_percent|I/o-percentage|Procent|Gemiddeld|I/o-percentage|Er zijn geen dimensies|
|storage_percent|Percentage van de opslag|Procent|Gemiddeld|Percentage van de opslag|Er zijn geen dimensies|
|storage_used|Gebruikte opslag|Bytes|Gemiddeld|Gebruikte opslag|Er zijn geen dimensies|
|storage_limit|Limiet voor opslag|Bytes|Gemiddeld|Limiet voor opslag|Er zijn geen dimensies|
|serverlog_storage_percent|Server Log opslag procent|Procent|Gemiddeld|Server Log opslag procent|Er zijn geen dimensies|
|serverlog_storage_usage|Server logboekopslag gebruikt|Bytes|Gemiddeld|Server logboekopslag gebruikt|Er zijn geen dimensies|
|serverlog_storage_limit|Limiet voor opslag van server-logboek|Bytes|Gemiddeld|Limiet voor opslag van server-logboek|Er zijn geen dimensies|
|active_connections|Actieve verbindingen|Count|Gemiddeld|Actieve verbindingen|Er zijn geen dimensies|
|connections_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|network_bytes_egress|Netwerk uit|Bytes|Totaal|Uitgaand netwerkverkeer voor actieve verbindingen|Er zijn geen dimensies|
|network_bytes_ingress|Netwerk in|Bytes|Totaal|Netwerk In voor de actieve verbindingen|Er zijn geen dimensies|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrie-bericht verzenden pogingen|Count|Totaal|Aantal telemetrieberichten dat apparaat-naar-cloud heeft geprobeerd om te worden verzonden naar uw IoT-hub|Er zijn geen dimensies|
|d2c.telemetry.ingress.success|Berichten over telemetrie verzonden|Count|Totaal|Aantal telemetrieberichten dat is verzonden naar uw IoT hub apparaat-naar-cloud|Er zijn geen dimensies|
|c2d.commands.egress.complete.success|Opdrachten voltooid|Count|Totaal|Aantal opdrachten voor cloud-naar-apparaat is voltooid door het apparaat|Er zijn geen dimensies|
|c2d.commands.egress.abandon.success|Opdrachten afgebroken|Count|Totaal|Aantal cloud-naar-apparaatopdrachten afgebroken door het apparaat|Er zijn geen dimensies|
|c2d.commands.egress.reject.success|Opdrachten geweigerd|Count|Totaal|Aantal cloud-naar-apparaatopdrachten geweigerd door het apparaat|Er zijn geen dimensies|
|devices.totalDevices|Totaal aantal apparaten (afgeschaft)|Count|Totaal|Aantal apparaten die zijn geregistreerd met uw IoT hub|Er zijn geen dimensies|
|devices.connectedDevices.allProtocol|Verbonden apparaten (afgeschaft) |Count|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|d2c.telemetry.egress.success|Routering: telemetrieberichten geleverd|Count|Totaal|Het aantal keren dat berichten zijn bezorgd bij alle eindpunten met behulp van IoT Hub-routering. Als een bericht wordt doorgestuurd naar meerdere eindpunten, wordt deze waarde verhoogd met één voor elke geslaagde levering. Als een bericht wordt bezorgd in het hetzelfde eindpunt meerdere keren, wordt deze waarde verhoogd met één voor elke geslaagde levering.|Er zijn geen dimensies|
|d2c.telemetry.egress.dropped|Routering: telemetrieberichten verwijderd |Count|Totaal|Het aantal keren dat berichten door de IoT-Hub routering vanwege andere eindpunten zijn verwijderd. Deze waarde is, telt niet berichten die in de alternatieve route, zoals verwijderde berichten worden er niet bezorgd.|Er zijn geen dimensies|
|d2c.telemetry.egress.orphaned|Routering: telemetrieberichten zwevende |Count|Totaal|Het aantal keren dat berichten door de IoT Hub routering zwevende omdat ze niet overeenkomen met alle regels voor doorsturen (met inbegrip van de alternatieve regel). |Er zijn geen dimensies|
|d2c.telemetry.egress.invalid|Routering: telemetrieberichten niet compatibel|Count|Totaal|Het aantal keren dat berichten worden verzonden vanwege compatibiliteitsproblemen met het eindpunt van de IoT-Hub routering kan niet. Deze waarde bevat geen nieuwe pogingen.|Er zijn geen dimensies|
|d2c.telemetry.egress.fallback|Routering: berichten worden afgeleverd bij terugval|Count|Totaal|Het aantal keren dat het IoT-Hub routering berichten naar het eindpunt dat is gekoppeld aan de alternatieve route geleverd.|Er zijn geen dimensies|
|d2c.endpoints.egress.eventHubs|Routering: berichten die naar Event Hub worden geleverd|Count|Totaal|Het aantal keren routering is IoT-Hub die berichten worden geleverd aan Event Hub-eindpunten.|Er zijn geen dimensies|
|d2c.endpoints.latency.eventHubs|Routering: bericht latentie voor Event Hub|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en bericht inkomend verkeer naar een Event Hub-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusQueues|Routering: berichten worden afgeleverd bij Service Bus-wachtrij|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten geleverd aan Service Bus-wachtrij-eindpunten.|Er zijn geen dimensies|
|d2c.endpoints.latency.serviceBusQueues|Routering: bericht latentie voor Service Bus-wachtrij|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een Service Bus-wachtrij-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusTopics|Routering: berichten worden afgeleverd bij Service Bus-onderwerp|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan Service Bus-onderwerp eindpunten geleverd.|Er zijn geen dimensies|
|d2c.endpoints.latency.serviceBusTopics|Routering: bericht latentie voor Service Bus-onderwerp|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een Service Bus-onderwerp-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.builtIn.events|Routering: berichten worden afgeleverd bij berichten/gebeurtenissen|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan het ingebouwde eindpunt (berichten/gebeurtenissen) geleverd.|Er zijn geen dimensies|
|d2c.endpoints.latency.builtIn.events|Routering: bericht latentie voor berichten/gebeurtenissen|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer naar het eindpunt van de ingebouwde (berichten/gebeurtenissen).|Er zijn geen dimensies|
|d2c.endpoints.egress.storage|Routering: berichten die naar de opslag worden geleverd|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan opslag eindpunten geleverd.|Er zijn geen dimensies|
|d2c.endpoints.latency.storage|Routering: bericht latentie voor opslag|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een storage-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.storage.bytes|Routering: gegevens geleverd aan opslag|Bytes|Totaal|De hoeveelheid gegevens (bytes) routering IoT-Hub die worden geleverd aan opslag-eindpunten.|Er zijn geen dimensies|
|d2c.endpoints.egress.storage.blobs|Routering: blobs die worden geleverd aan opslag|Count|Totaal|Het aantal keren dat het IoT-Hub routering blobs geleverd aan opslag-eindpunten.|Er zijn geen dimensies|
|d2c.twin.read.success|Geslaagde dubbele leesbewerkingen van apparaten|Count|Totaal|Het aantal voltooide dubbele apparaat geïnitieerde leesbewerkingen.|Er zijn geen dimensies|
|d2c.twin.read.failure|Dubbele leesbewerkingen van apparaten is mislukt|Count|Totaal|De telling van alle dubbele apparaat geïnitieerde leesbewerkingen mislukt.|Er zijn geen dimensies|
|d2c.twin.read.size|Reactiegrootte van dubbele leesbewerkingen van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde apparaat geïnitieerde twin leesbewerkingen.|Er zijn geen dimensies|
|d2c.twin.update.success|Geslaagde apparaatdubbel werkt bij van apparaten|Count|Totaal|Het aantal voltooide apparaat geïnitieerde apparaatdubbel werkt.|Er zijn geen dimensies|
|d2c.twin.update.failure|Apparaatdubbel werkt bij van apparaten is mislukt|Count|Totaal|De telling van alle mislukte dubbel apparaat geïnitieerde updates.|Er zijn geen dimensies|
|d2c.twin.update.size|Grootte van apparaatdubbel werkt bij van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde apparaat geïnitieerde twin updates.|Er zijn geen dimensies|
|c2d.methods.success|Geslaagde rechtstreekse methodeaanroepen|Count|Totaal|Het aantal voltooide rechtstreekse methodeaanroepen.|Er zijn geen dimensies|
|c2d.Methods.failure|Rechtstreekse methodeaanroepen is mislukt|Count|Totaal|De telling van alle mislukte aanroepen van de directe methode.|Er zijn geen dimensies|
|c2d.methods.requestSize|Grootte van de aanvraag van een rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde aanvragen voor directe methode.|Er zijn geen dimensies|
|c2d.methods.responseSize|Reactiegrootte van rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde antwoorden in de directe methode.|Er zijn geen dimensies|
|c2d.twin.read.success|Geslaagde dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart dubbele leesbewerkingen.|Er zijn geen dimensies|
|c2d.twin.read.failure|Mislukte dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle back-end-gestart dubbele leesbewerkingen mislukt.|Er zijn geen dimensies|
|c2d.twin.read.size|Reactiegrootte van dubbele leesbewerkingen van back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde back-end-gestart twin leesbewerkingen.|Er zijn geen dimensies|
|c2d.twin.update.success|Geslaagde apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart apparaatdubbel werkt.|Er zijn geen dimensies|
|c2d.twin.update.failure|Mislukte apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle mislukte back-end-gestart dubbele updates.|Er zijn geen dimensies|
|c2d.twin.update.size|Grootte van apparaatdubbel werkt bij vanaf back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde back-end-gestart twin updates.|Er zijn geen dimensies|
|twinQueries.success|Geslaagde apparaatdubbel-query 's|Count|Totaal|De telling van alle geslaagde apparaatdubbel-query's.|Er zijn geen dimensies|
|twinQueries.failure|Mislukte apparaatdubbel-query 's|Count|Totaal|De telling van alle mislukte apparaatdubbel-query's.|Er zijn geen dimensies|
|twinQueries.resultSize|Grootte van apparaatdubbel-query 's|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van de grootte van het resultaat van alle geslaagde apparaatdubbel-query's.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.success|Geslaagde bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle is gemaakt van dubbele update taken.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.failure|Mislukte bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle mislukte aanmaak van dubbele taken bijwerken.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.success|Geslaagde bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle is gemaakt van een rechtstreekse methode aanroepen van taken.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.failure|Mislukte bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle mislukte aanmaak van rechtstreekse methode aanroepen van taken.|Er zijn geen dimensies|
|jobs.listJobs.success|Geslaagde aanroepen naar de lijst met taken|Count|Totaal|Het aantal geslaagde aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs.listJobs.failure|Mislukte aanroepen naar de lijst met taken|Count|Totaal|Het aantal mislukte aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs.cancelJob.success|Geslaagde taakannuleringen|Count|Totaal|Het aantal geslaagde aanroepen naar een taak annuleren.|Er zijn geen dimensies|
|jobs.cancelJob.failure|Mislukte taakannuleringen|Count|Totaal|Het aantal mislukte aanroepen naar een taak annuleren.|Er zijn geen dimensies|
|jobs.queryJobs.success|Taak query 's|Count|Totaal|Het aantal geslaagde aanroepen naar query taken.|Er zijn geen dimensies|
|jobs.queryJobs.failure|Taak query's is mislukt|Count|Totaal|Het aantal mislukte aanroepen naar query taken.|Er zijn geen dimensies|
|jobs.completed|Voltooide taken|Count|Totaal|De telling van alle voltooide taken.|Er zijn geen dimensies|
|jobs.failed|Mislukte taken|Count|Totaal|De telling van alle mislukte taken.|Er zijn geen dimensies|
|d2c.telemetry.ingress.sendThrottle|Aantal beperkingsfouten|Count|Totaal|Aantal beperkingsfouten vanwege apparaat doorvoer beperkt|Er zijn geen dimensies|
|dailyMessageQuotaUsed|Totaal aantal berichten dat is gebruikt|Count|Gemiddeld|Het aantal totaal aantal berichten momenteel gebruikt. Dit is een cumulatieve waarde die is ingesteld op nul om 00:00 UTC elke dag.|Er zijn geen dimensies|
|deviceDataUsage|Totaal aantal devicedata gebruik (verouderd)|Bytes|Totaal|Bytes overgebracht naar en van alle apparaten die zijn verbonden met IotHub|Er zijn geen dimensies|
|deviceDataUsageV2|Totaal aantal apparaat gegevensgebruik (preview)|Bytes|Totaal|Bytes overgebracht naar en van alle apparaten die zijn verbonden met IotHub|Er zijn geen dimensies|
|totalDeviceCount|Totaal aantal apparaten (preview)|Count|Gemiddeld|Aantal apparaten die zijn geregistreerd met uw IoT hub|Er zijn geen dimensies|
|connectedDeviceCount|Verbonden apparaten (preview)|Count|Gemiddeld|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|Configuraties|Configuratie van metrische gegevens|Count|Totaal|Metrische gegevens voor configuratiebewerkingen|Er zijn geen dimensies|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|RegistrationAttempts|Pogingen tot registratie|Count|Totaal|Nummer van het apparaatregistraties geprobeerd|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Apparaten die zijn toegewezen|Count|Totaal|Aantal apparaten dat is toegewezen aan een IoT-hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Attestation-pogingen|Count|Totaal|Aantal apparaat verklaringen geprobeerd|ProvisioningServiceName, Status, Protocol|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

### <a name="request-metrics"></a>Aanvraag voor metrische gegevens

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---|---|---| ---| ---| ---|
| TotalRequests |   Totaal aantal aanvragen| Count   | Count | Aantal aanvragen|  DatabaseName, CollectionName, Region, StatusCode|   Alle |   TotalRequests, Http 2xx, Http 3xx, Http-fout 400, 401 Http-, interne serverfout Service niet beschikbaar, aanvragen beperkt, Gemiddeld aantal aanvragen per seconde |    Gebruikt voor het bewaken van aanvragen per statuscode, verzameling met een granulariteit van één minuut. Als u verzoeken per seconde, minuut aantal aggregatie gebruiken en delen door 60. |
| MetadataRequests |    Aanvragen voor metagegevens   |Count| Count   | Het aantal aanvragen voor metagegevens. Azure Cosmos DB houdt verzameling systeemmetagegevens voor elk account, waarmee u het inventariseren van verzamelingen, databases, enzovoort, en de bijbehorende configuraties, gratis.    | DatabaseName, CollectionName, Region, StatusCode| Alle|  |Gebruikt voor het bewaken van vertragingen vanwege aanvragen voor metagegevens.|
| MongoRequests |   Mongo-aanvragen| Count | Count|  Aantal aanvragen van Mongo   | Databasenaam, CollectionName, regio, CommandName, foutcode| Alle |Snelheid van aanvragen voor mongo-Query, Mongo-Update aanvragen snelheid, Mongo verwijderen snelheid van aanvragen, Mongo invoegen snelheid van aanvragen, snelheid van aanvragen voor Mongo-aantal|   Gebruikt voor het bewaken van Mongo aanvraag fouten, typt u het gebruik van per opdracht. |

### <a name="request-unit-metrics"></a>Aanvraag eenheid metrische gegevens

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---|---|---| ---| ---| ---|
| MongoRequestCharge|   Mongo-aanvraag kosten in rekening gebracht |  Count   |Totaal  |Mongo-Aanvraageenheden verbruikt|  Databasenaam, CollectionName, regio, CommandName, foutcode|   Alle |Mongo-Query aanvraag kosten in rekening gebracht, Mongo Update kosten in rekening gebracht, Mongo verwijderen aanvraag kosten in rekening gebracht, Mongo invoegen aanvraag kosten in rekening gebracht, Mongo aantal aanvraag kosten aanvragen| Gebruikt voor het bewaken van Mongo-resource ru's in een minuut.|
| TotalRequestUnits |Totaal aantal Aanvraageenheden|   Count|  Totaal|  Aanvraag dat eenheden verbruikt| DatabaseName, CollectionName, Region, StatusCode    |Alle|   TotalRequestUnits|  Gebruikt om het totaal aantal RU-gebruik op een minute granulariteit bewaken. Als u gemiddelde ru's per seconde worden verbruikt, totale aggregatie op minuut gebruiken en delen door 60.|
| ProvisionedThroughput |Ingerichte doorvoer|    Count|  Maximum |Ingerichte doorvoer via de granulatie van de verzameling|  DatabaseName, CollectionName|   5 MIN.| |   Gebruikt voor het bewaken van de ingerichte doorvoer per verzameling.|

### <a name="storage-metrics"></a>Metrische gegevens over opslag

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---|---|---| ---| ---| ---|
| AvailableStorage| Beschikbare opslag   |Bytes| Totaal|  Totale beschikbare opslag gerapporteerd op 5 minuten granulatie per regio|   DatabaseName, CollectionName, Region|   5 MIN.| Beschikbare opslag|   Gebruikt voor het bewaken van beschikbare opslag (alleen van toepassing op vaste opslag verzamelingen) capaciteit minimale granulariteit moet 5 minuten.|
| DataUsage |Gegevensgebruik |Bytes| Totaal   |Gebruik van de totale hoeveelheid gegevens die zijn gerapporteerd op 5 minuten granulatie per regio|    DatabaseName, CollectionName, Region|   5 MIN.  |Gegevensgrootte  | Wordt gebruikt om het gebruik van de totale hoeveelheid gegevens op de verzameling en de regio bewaken, moet minimale granulariteit 5 minuten.|
| IndexUsage|   Gebruik van de index|    Bytes|  Totaal   |Totaal gebruik van de Index wordt gerapporteerd op 5 minuten granulatie per regio|    DatabaseName, CollectionName, Region|   5 MIN.| Indexgrootte| Wordt gebruikt om het gebruik van de totale hoeveelheid gegevens op de verzameling en de regio bewaken, moet minimale granulariteit 5 minuten. |
| DocumentQuota|    Document quotum| Bytes|  Totaal|  Totale opslagquotum gerapporteerd op 5 minuten granulatie per regio. Van toepassing op vaste opslag verzamelingen| DatabaseName, CollectionName, Region|   5 MIN.  |Opslagcapaciteit|  Wordt gebruikt voor het bewaken van de totale quotum verzameling en regio, moet minimale granulariteit 5 minuten.|
| DocumentCount|    Aantal documenten| Count   |Totaal  |Totaldocument aantal dat is gerapporteerd aan vijf minuten granulatie per regio|  DatabaseName, CollectionName, Region|   5 MIN.  |Aantal documenten|Wordt gebruikt voor het bewaken van het documentaantal op de verzameling en regio, moet minimale granulariteit 5 minuten.|

### <a name="latency-metrics"></a>Metrieken voor latentie

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies| Tijd granulaties| Gebruik |
|---|---|---|---|---|---| ---| ---|
| ReplicationLatency    | Replicatielatentie|  MilliSeconds|   Minimum, Maximum, gemiddelde | Replicatievertraging P99 tussen de bron- en regio's voor geo-ingeschakelde account| SourceRegion, TargetRegion| Alle | Gebruikt voor het bewaken van de replicatielatentie P99 tussen elke twee regio's voor een geo-replicatie-account. |

### <a name="availability-metrics"></a>Metrische gegevens over beschikbaarheid

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---|---|---| ---| ---| ---|
| ServiceAvailability   | Beschikbaarheid van de service| Procent |Minimum,Maximum|   Beschikbaarheid van aanvragen bij de granulariteit van één uur|  |   1 UUR  | Beschikbaarheid van de service  | Dit is het percentage van totaal aantal geslaagde aanvragen. Een aanvraag wordt beschouwd als worden is mislukt vanwege een systeemfout is als de statuscode 410 is, 500 of 503 wordt gebruikt om de beschikbaarheid van het account op uur granulariteit te controleren. |

### <a name="cassandra-api-metrics"></a>Metrische gegevens over Cassandra-API

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies| Tijd granulaties| Gebruik |
|---|---|---|---|---|---| ---| ---|
| CassandraRequests | Cassandra-aanvragen |  Count|  Count|  Aantal aanvragen voor Cassandra-API|  Databasenaam, CollectionName, ErrorCode, regio, OperationType, ResourceType|   Alle| Gebruikt voor het bewaken van de Cassandra-aanvragen met een granulariteit van één minuut. Als u verzoeken per seconde, minuut aantal aggregatie gebruiken en delen door 60.|
| CassandraRequestCharges|  Cassandra-aanvraag kosten in rekening gebracht| Count|   Sum, Min, Max, Avg| Aanvraageenheden gebruikt door de Cassandra-API-aanvragen|   DatabaseName, CollectionName, Region, OperationType, ResourceType|  Alle| Gebruikt voor het bewaken van ru's per minuut wordt gebruikt door een Cassandra-API-account.|
| CassandraConnectionClosures   | Cassandra-verbinding is gesloten |Count| Count   |Aantal Cassandra verbindingen gesloten|    ClosureReason, Region|  Alle | Gebruikt voor het bewaken van de verbinding tussen clients en de Cassandra-API van Azure Cosmos DB.|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PublishSuccessCount|Gepubliceerde gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen die zijn gepubliceerd in dit onderwerp|Er zijn geen dimensies|
|PublishFailCount|Mislukte gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen publiceren naar dit onderwerp is mislukt|ErrorType, Error|
|UnmatchedEventCount|Niet-overeenkomende gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat niet overeenkomt met een van de gebeurtenisabonnementen voor dit onderwerp|Er zijn geen dimensies|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|MatchedEventCount|Overeenkomende gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen die zijn gekoppeld aan dit gebeurtenisabonnement|Er zijn geen dimensies|
|DeliveryAttemptFailCount|Gebeurtenissen de bezorging is mislukt|Count|Totaal|Totaal aantal gebeurtenissen is mislukt om te leveren aan dit gebeurtenisabonnement|Error, ErrorType|
|DeliverySuccessCount|Geleverde gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen die worden geleverd aan dit gebeurtenisabonnement|Er zijn geen dimensies|
|DestinationProcessingDurationInMs|Duur van de verwerking van doel|Milliseconden|Gemiddeld|Duur van de verwerking van de bestemming in milliseconden|Er zijn geen dimensies|
|DroppedEventCount|Verwijderde gebeurtenissen|Count|Totaal|Totaal aantal verwijderde gebeurtenissen die overeenkomen met voor dit gebeurtenisabonnement|Er zijn geen dimensies|
|DeadLetteredCount|Dode letters gebeurtenissen|Count|Totaal|Totaal aantal dode letters gebeurtenissen die overeenkomen met voor dit gebeurtenisabonnement|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PublishSuccessCount|Gepubliceerde gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen die zijn gepubliceerd in dit onderwerp|Er zijn geen dimensies|
|PublishFailCount|Mislukte gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen publiceren naar dit onderwerp is mislukt|ErrorType, Error|
|UnmatchedEventCount|Niet-overeenkomende gebeurtenissen|Count|Totaal|Totaal aantal gebeurtenissen dat niet overeenkomt met een van de gebeurtenisabonnementen voor dit onderwerp|Er zijn geen dimensies|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Succesfulrequests|Geslaagde aanvragen (Preview)|Count|Totaal|Voltooide aanvragen voor Microsoft.EventHub. (Preview)|EntityName|
|ServerErrors|Serverfouten. (Preview)|Count|Totaal|Serverfouten voor Microsoft.EventHub. (Preview)|EntityName|
|UserErrors|Gebruikersfouten. (Preview)|Count|Totaal|Gebruikersfouten voor Microsoft.EventHub. (Preview)|EntityName|
|QuotaExceededErrors|Fout met overschreden quotum. (Preview)|Count|Totaal|Fout met overschreden quotum voor Microsoft.EventHub. (Preview)|EntityName|
|ThrottledRequests|Beperkte aanvragen. (Preview)|Count|Totaal|Beperkte aanvragen voor Microsoft.EventHub. (Preview)|EntityName|
|IncomingRequests|Inkomende aanvragen (Preview)|Count|Totaal|Binnenkomende aanvragen voor Microsoft.EventHub. (Preview)|EntityName|
|IncomingMessages|Binnenkomende berichten (Preview)|Count|Totaal|Binnenkomende berichten voor Microsoft.EventHub. (Preview)|EntityName|
|OutgoingMessages|Uitgaande berichten (Preview)|Count|Totaal|Uitgaande berichten voor Microsoft.EventHub. (Preview)|EntityName|
|IncomingBytes|Binnenkomende bytes. (Preview)|Bytes|Totaal|Binnenkomende bytes voor Microsoft.EventHub. (Preview)|EntityName|
|OutgoingBytes|Uitgaande bytes. (Preview)|Bytes|Totaal|Uitgaande bytes voor Microsoft.EventHub. (Preview)|EntityName|
|ActiveConnections|ActiveConnections (Preview)|Count|Gemiddeld|Totaal aantal actieve verbindingen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|ConnectionsOpened|Geopende verbindingen. (Preview)|Count|Gemiddeld|Geopende verbindingen voor Microsoft.EventHub. (Preview)|EntityName|
|ConnectionsClosed|Gesloten verbindingen. (Preview)|Count|Gemiddeld|Gesloten verbindingen voor Microsoft.EventHub. (Preview)|EntityName|
|CaptureBacklog|Achterstand vastleggen. (Preview)|Count|Totaal|Achterstand vastleggen voor Microsoft.EventHub. (Preview)|EntityName|
|CapturedMessages|Vastgelegde berichten. (Preview)|Count|Totaal|Vastgelegde berichten voor Microsoft.EventHub. (Preview)|EntityName|
|CapturedBytes|Vastgelegde bytes. (Preview)|Bytes|Totaal|Vastgelegde bytes voor Microsoft.EventHub. (Preview)|EntityName|
|Grootte|Grootte (Preview)|Bytes|Gemiddeld|Grootte van een EventHub in bytes. (Preview)|EntityName|
|INREQS|Binnenkomende aanvragen|Count|Totaal|Totaal aantal inkomende aanvragen voor een naamruimte|Er zijn geen dimensies|
|SUCCREQ|Geslaagde aanvragen|Count|Totaal|Totaal aantal verwerkte aanvragen voor een naamruimte|Er zijn geen dimensies|
|FAILREQ|Mislukte aanvragen|Count|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|Er zijn geen dimensies|
|SVRBSY|Fouten omdat de server bezet is|Count|Totaal|Totaal aantal server bezet-fouten voor een naamruimte|Er zijn geen dimensies|
|INTERR|Interne serverfouten|Count|Totaal|Totaal aantal interne serverfouten voor een naamruimte|Er zijn geen dimensies|
|MISCERR|Overige fouten|Count|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|Er zijn geen dimensies|
|INMSGS|Binnenkomende berichten (afgeschaft)|Count|Totaal|Totaal aantal binnenkomende berichten voor een naamruimte. Deze statistiek is afgeschaft. Gebruik de statistiek voor binnenkomende berichten in plaats daarvan|Er zijn geen dimensies|
|EHINMSGS|Binnenkomende berichten|Count|Totaal|Totaal aantal inkomende berichten voor een naamruimte|Er zijn geen dimensies|
|OUTMSGS|Uitgaande berichten (afgeschaft)|Count|Totaal|Totaal aantal uitgaande berichten voor een naamruimte. Deze statistiek is afgeschaft. Gebruik de statistiek voor uitgaande berichten in plaats daarvan|Er zijn geen dimensies|
|EHOUTMSGS|Uitgaande berichten|Count|Totaal|Totaal aantal uitgaande berichten voor een naamruimte|Er zijn geen dimensies|
|EHINMBS|Binnenkomende bytes (afgeschaft)|Bytes|Totaal|Event Hub binnenkomende doorvoer berichten voor een naamruimte. Deze statistiek is afgeschaft. Gebruik in plaats daarvan statistiek voor binnenkomende bytes|Er zijn geen dimensies|
|EHINBYTES|Binnenkomende bytes|Bytes|Totaal|Doorvoer van inkomende Event Hub-berichten voor een naamruimte|Er zijn geen dimensies|
|EHOUTMBS|Uitgaande bytes (afgeschaft)|Bytes|Totaal|Event Hub uitgaande doorvoer berichten voor een naamruimte. Deze statistiek is afgeschaft. Gebruik in plaats daarvan statistiek voor uitgaande bytes|Er zijn geen dimensies|
|EHOUTBYTES|Uitgaande bytes|Bytes|Totaal|Doorvoer van uitgaande Event Hub-berichten voor een naamruimte|Er zijn geen dimensies|
|EHABL|Achterstallige berichten van archief|Count|Totaal|Achterstallige Event Hub-archiefberichten voor een naamruimte|Er zijn geen dimensies|
|EHAMSGS|Berichten archiveren|Count|Totaal|Gearchiveerde Event Hub-berichten in een naamruimte|Er zijn geen dimensies|
|EHAMBS|Berichtdoorvoer van archief|Bytes|Totaal|Doorvoer van gearchiveerde Event Hub-berichtenin een naamruimte|Er zijn geen dimensies|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Succesfulrequests|Geslaagde aanvragen (Preview)|Count|Totaal|Voltooide aanvragen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|ServerErrors|Serverfouten. (Preview)|Count|Totaal|Serverfouten voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|UserErrors|Gebruikersfouten. (Preview)|Count|Totaal|Gebruikersfouten voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|QuotaExceededErrors|Fout met overschreden quotum. (Preview)|Count|Totaal|Fout met overschreden quotum voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|ThrottledRequests|Beperkte aanvragen. (Preview)|Count|Totaal|Beperkte aanvragen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|IncomingRequests|Inkomende aanvragen (Preview)|Count|Totaal|Binnenkomende aanvragen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|IncomingMessages|Binnenkomende berichten (Preview)|Count|Totaal|Binnenkomende berichten voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|OutgoingMessages|Uitgaande berichten (Preview)|Count|Totaal|Uitgaande berichten voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|IncomingBytes|Binnenkomende bytes. (Preview)|Bytes|Totaal|Binnenkomende bytes voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|OutgoingBytes|Uitgaande bytes. (Preview)|Bytes|Totaal|Uitgaande bytes voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|ActiveConnections|ActiveConnections (Preview)|Count|Gemiddeld|Totaal aantal actieve verbindingen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|ConnectionsOpened|Geopende verbindingen. (Preview)|Count|Gemiddeld|Geopende verbindingen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|ConnectionsClosed|Gesloten verbindingen. (Preview)|Count|Gemiddeld|Gesloten verbindingen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|CaptureBacklog|Achterstand vastleggen. (Preview)|Count|Totaal|Achterstand vastleggen voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|CapturedMessages|Vastgelegde berichten. (Preview)|Count|Totaal|Vastgelegde berichten voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|CapturedBytes|Vastgelegde bytes. (Preview)|Bytes|Totaal|Vastgelegde bytes voor Microsoft.EventHub. (Preview)|Er zijn geen dimensies|
|CPU|CPU (Preview)|Procent|Maximum|CPU-gebruik voor het Event Hub-cluster als percentage|Rol|
|AvailableMemory|Beschikbaar geheugen (Preview)|Count|Maximum|Beschikbaar geheugen voor het Event Hub-cluster in bytes|Rol|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|GatewayRequests|Gateway-aanvragen|Count|Totaal|Aantal gateway-aanvragen|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Gecategoriseerde Gateway-aanvragen|Count|Totaal|Aantal gateway-aanvragen per categorie (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HttpStatus|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ObservedMetricValue|Waargenomen metrische waarde|Count|Gemiddeld|De waarde die is berekend door automatische schaalaanpassing wanneer deze is uitgevoerd|MetricTriggerSource|
|MetricThreshold|Drempel voor een metrische waarde|Count|Gemiddeld|De geconfigureerde drempelwaarde voor automatische schaalaanpassing wanneer automatische schaalaanpassing is uitgevoerd.|MetricTriggerRule|
|ObservedCapacity|Waargenomen capaciteit|Count|Gemiddeld|De capaciteit die is gerapporteerd aan automatische schaalaanpassing wanneer dit is uitgevoerd.|Er zijn geen dimensies|
|ScaleActionsInitiated|Schaalacties geactiveerd|Count|Totaal|De richting van de schaalbewerking.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

(Preview-versie)

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|availabilityResults/duur|Testduur|MilliSeconds|Gemiddeld|Testduur|availabilityResult/name, availabilityResult /-locatie, availabilityResult/geslaagd|
|billingMeters/telemetryCount|Gegevenspuntaantal|Count|Totaal|Het aantal gegevenspunten die worden verzonden naar deze Application Insights-resource. Deze metrische gegevens worden verwerkt met een latentie van maximaal twee uur.|billing/telemetryItemType, billing/telemetryItemSource|
|billingMeters/telemetrySize|Gegevenspuntvolume|Bytes|Totaal|De hoeveelheid gegevens die naar deze Application Insights-resource verzonden. Deze metrische gegevens worden verwerkt met een latentie van maximaal twee uur.|billing/telemetryItemType, billing/telemetryItemSource|
|browserTimings/networkDuration|Netwerkverbindingstijd voor laden van pagina|MilliSeconds|Gemiddeld|Tijd tussen gebruikersaanvraag en netwerkverbinding gebruikersverbinding. Omvat de DNS-zoekopdracht en transportverbinding verbinding.|Er zijn geen dimensies|
|browserTimings/processingDuration|Verwerkingstijd client|MilliSeconds|Gemiddeld|Tijd tussen ontvangst van de laatste byte van een document totdat de DOM wordt geladen. Asynchrone aanvragen kunnen nog worden verwerkt.|Er zijn geen dimensies|
|browserTimings/receiveDuration|Reactietijd voor ontvangen|MilliSeconds|Gemiddeld|Tijd tussen de eerste en de laatste byte of tot verbreking van de verbinding.|Er zijn geen dimensies|
|browserTimings/sendDuration|Verzoektijd voor verzenden|MilliSeconds|Gemiddeld|Tijd tussen netwerkverbinding en ontvangst van eerste byte.|Er zijn geen dimensies|
|browserTimings/totalDuration|Laadtijd van browserpagina|MilliSeconds|Gemiddeld|Tijd vanaf gebruikersaanvraag totdat DOM, opmaakmodellen, scripts en afbeeldingen zijn geladen.|Er zijn geen dimensies|
|afhankelijkheden/aantal|Afhankelijkheidsaanroepen|Count|Count|Aantal aanroepen van de toepassing naar externe bronnen.|afhankelijkheidstype /, afhankelijkheid/performanceBucket, afhankelijkheid/succes, bewerking/synthetische, cloud/roleInstance, cloud/roleName|
|afhankelijkheden/duur|Duur van afhankelijkheid|MilliSeconds|Gemiddeld|Duur van de aanroepen van de toepassing naar externe bronnen.|afhankelijkheidstype /, afhankelijkheid/performanceBucket, afhankelijkheid/succes, bewerking/synthetische, cloud/roleInstance, cloud/roleName|
|/ afhankelijkheden zijn mislukt|Afhankelijkheidsfouten|Count|Count|Aantal mislukte afhankelijkheidsaanroepen van de toepassing naar externe resources.|afhankelijkheidstype /, afhankelijkheid/performanceBucket, bewerking/synthetische, cloud/roleInstance, cloud/roleName|
|pageViews/count|Paginaweergaven|Count|Count|Aantal paginaweergaven.|bewerking/synthetische|
|pageViews/duration|Laadtijd voor paginaweergave|MilliSeconds|Gemiddeld|Laadtijd voor paginaweergave|bewerking/synthetische|
|performanceCounters/requestExecutionTime|Uitvoeringstijd van de HTTP-aanvraag|MilliSeconds|Gemiddeld|Uitvoeringstijd van de meest recente aanvraag.|cloud/roleInstance|
|performanceCounters/requestsInQueue|HTTP-aanvragen in de toepassingswachtrij|Count|Gemiddeld|Lengte van de rij met toepassingsaanvragen.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|HTTP-aanvraagsnelheid|CountPerSecond|Gemiddeld|Snelheid per seconde van alle aanvragen aan de toepassing vanaf ASP.NET.|cloud/roleInstance|
|performanceCounters/exceptionsPerSecond|Aantal uitzonderingen|CountPerSecond|Gemiddeld|Het aantal verwerkte en onverwerkte uitzonderingen dat is gerapporteerd aan Windows, inclusief .NET-uitzonderingen en onbeheerde uitzonderingen die zijn geconverteerd naar .NET-uitzonderingen.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|I/O-snelheid voor proces|BytesPerSecond|Gemiddeld|Totaal aantal per seconde gelezen en naar bestanden, netwerk en apparaten geschreven bytes.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|CPU voor proces|Procent|Gemiddeld|Het percentage verstreken tijd dat alle threads de processor hebben gebruikt voor het uitvoeren van instructies. Dit kan variëren van 0 tot 100. Deze metrische waarde geeft de prestaties van het w3wp-proces alleen.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Processortijd|Procent|Gemiddeld|Het tijdspercentage dat de processor spendeert aan niet-inactieve threads.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Beschikbaar geheugen|Bytes|Gemiddeld|Fysiek geheugen dat direct beschikbaar is voor toewijzing aan een proces of voor systeemgebruik.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Proceseigen bytes|Bytes|Gemiddeld|Geheugen exclusief toegewezen aan de processen van de gecontroleerde toepassing.|cloud/roleInstance|
|aanvragen/duur|Serverreactietijd|MilliSeconds|Gemiddeld|Tijd tussen de ontvangst van een HTTP-aanvraag en het voltooien van het verzenden van de reactie.|aanvraag/performanceBucket, aanvraag/resultCode, bewerking/synthetische, cloud/roleInstance, aanvraag-geslaagd, cloud/roleName|
|requests/count|Serveraanvragen|Count|Count|Aantal voltooide HTTP-aanvragen.|aanvraag/performanceBucket, aanvraag/resultCode, bewerking/synthetische, cloud/roleInstance, aanvraag-geslaagd, cloud/roleName|
|mislukte aanvragen /|Mislukte aanvragen|Count|Count|Aantal van de HTTP-aanvragen gemarkeerd als mislukt. In de meeste gevallen zijn dit aanvragen met een responscode > = 400 en niet gelijk aan 401.|aanvraag/performanceBucket, aanvraag/resultCode, bewerking/synthetische, cloud/roleInstance, cloud/roleName|
|uitzonderingen/aantal|Uitzonderingen|Count|Totaal|Gecombineerd aantal niet-onderschepte uitzonderingen.|cloud/roleName, cloud/roleInstance, client /-type|
|uitzonderingen/de browser|Browseruitzonderingen|Count|Totaal|Aantal niet-onderschepte uitzonderingen in de browser.|Er zijn geen dimensies|
|uitzonderingen of de server|Serveruitzonderingen|Count|Totaal|Aantal niet-onderschepte uitzonderingen in de servertoepassing.|cloud/roleName, cloud/roleInstance|
|traceringen/aantal|Aantal traces|Count|Totaal|Aantal traceerdocumenten|tracering/severityLevel, bewerking/synthetische, cloud/roleName, cloud/roleInstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ServiceApiHit|Totaalaantal treffers service-API|Count|Count|Totaalaantal treffers voor service-API|ActivityType, ActivityName|
|ServiceApiLatency|Algemene latentie service-API|Milliseconden|Gemiddeld|Algemene latentie van service-API-aanvragen|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Totaalaantal resultaten service-API|Count|Count|Totaalaantal resultaten voor service-API|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ClusterDataCapacityFactor|Cache-gebruik|Procent|Gemiddeld|Het gebruiksniveau van binnen het bereik van het cluster|Er zijn geen dimensies|
|QueryDuration|Queryduur van de|Milliseconden|Gemiddeld|Query's de duur in seconden|QueryStatus|
|IngestionsLoadFactor|Opname-gebruik|Procent|Gemiddeld|Verhouding van de sleuven gebruikte opname in het cluster|Er zijn geen dimensies|
|IsEngineAnsweringQuery|Actief houden|Count|Gemiddeld|Controle geeft aan dat het cluster reageert op query 's|Er zijn geen dimensies|
|IngestCommandOriginalSizeInMb|Opname-Volume (In MB)|Count|Totaal|Totale hoeveelheid opgenomen gegevens aan het cluster (in MB)|Er zijn geen dimensies|
|EventAgeSeconds|Opnamelatentie (In seconden)|Seconden|Gemiddeld|Opname-tijd van de bron (bijvoorbeeld bericht is in de Event hub) aan het cluster in seconden|Er zijn geen dimensies|
|EventReceivedFromEventHub|Gebeurtenissen die worden verwerkt (voor Eventhubs)|Count|Totaal|Aantal gebeurtenissen dat door het cluster worden verwerkt wanneer het opnemen van Event Hub|Er zijn geen dimensies|
|IngestionResult|Opname-resultaat|Count|Count|Aantal opname-bewerkingen|IngestionResultDetails|
|EngineCPU|CPU|Procent|Gemiddeld|Het gebruiksniveau van CPU|Er zijn geen dimensies|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Gebruik|Gebruik|Count|Count|Aantal van de API-aanroepen|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|RunsStarted|Gestarte uitvoeringen|Count|Totaal|Aantal gestarte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCompleted|Voltooide uitvoeringen|Count|Totaal|Aantal voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsSucceeded|Geslaagde uitvoeringen|Count|Totaal|Aantal geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsFailed|Mislukte uitvoeringen|Count|Totaal|Aantal mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunsCancelled|Geannuleerde uitvoeringen|Count|Totaal|Aantal geannuleerde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunLatency|Latentie van uitvoeringen|Seconden|Gemiddeld|Latentie van voltooide werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunSuccessLatency|Latentie van geslaagde uitvoeringen|Seconden|Gemiddeld|Latentie van geslaagde werkstroomuitvoeringen.|Er zijn geen dimensies|
|RunThrottledEvents|Door uitvoering vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomactie of trigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|RunFailurePercentage|Percentage mislukte uitvoeringen|Procent|Totaal|Percentage mislukte werkstroomuitvoeringen.|Er zijn geen dimensies|
|ActionsStarted|Gestarte acties |Count|Totaal|Aantal gestarte werkstroomacties.|Er zijn geen dimensies|
|ActionsCompleted|Voltooide acties |Count|Totaal|Aantal voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionsSucceeded|Geslaagde acties |Count|Totaal|Aantal geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionsFailed|Mislukte acties|Count|Totaal|Aantal mislukte werkstroomacties.|Er zijn geen dimensies|
|ActionsSkipped|Overgeslagen acties |Count|Totaal|Aantal overgeslagen werkstroomacties.|Er zijn geen dimensies|
|ActionLatency|Actielatentie |Seconden|Gemiddeld|Latentie van voltooide werkstroomacties.|Er zijn geen dimensies|
|ActionSuccessLatency|Latentie geslaagde acties |Seconden|Gemiddeld|Latentie van geslaagde werkstroomacties.|Er zijn geen dimensies|
|ActionThrottledEvents|Door actie vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomactie vertraagde gebeurtenissen.|Er zijn geen dimensies|
|TriggersStarted|Gestarte triggers |Count|Totaal|Aantal gestarte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersCompleted|Voltooide triggers |Count|Totaal|Aantal voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSucceeded|Geslaagde triggers |Count|Totaal|Aantal geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFailed|Mislukte triggers |Count|Totaal|Aantal mislukte werkstroomtriggers.|Er zijn geen dimensies|
|TriggersSkipped|Overgeslagen triggers|Count|Totaal|Aantal overgeslagen werkstroomtriggers.|Er zijn geen dimensies|
|TriggersFired|Geactiveerde triggers |Count|Totaal|Aantal geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerLatency|Latentie van trigger |Seconden|Gemiddeld|Latentie van voltooide werkstroomtriggers.|Er zijn geen dimensies|
|TriggerFireLatency|Latentie van triggeractivering |Seconden|Gemiddeld|Latentie van geactiveerde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerSuccessLatency|Latentie van geslaagde triggers |Seconden|Gemiddeld|Latentie van geslaagde werkstroomtriggers.|Er zijn geen dimensies|
|TriggerThrottledEvents|Door trigger vertraagde gebeurtenissen|Count|Totaal|Aantal door werkstroomtrigger vertraagde gebeurtenissen.|Er zijn geen dimensies|
|BillableActionExecutions|Factureerbare uitvoeringen van acties|Count|Totaal|Aantal uitvoeringen van werkstroomacties dat wordt gefactureerd.|Er zijn geen dimensies|
|BillableTriggerExecutions|Factureerbare uitvoeringen van activeringen|Count|Totaal|Aantal uitvoeringen van werkstroomactiveringen dat wordt gefactureerd.|Er zijn geen dimensies|
|TotalBillableExecutions|Totaal aantal factureerbare uitvoeringen|Count|Totaal|Aantal werkstroomuitvoeringen dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageNativeOperation|Facturering van gebruik voor systeemeigen uitvoerbewerkingen|Count|Totaal|Aantal systeemeigen uitvoerbewerkingen dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStandardConnector|Facturering van gebruik voor uitvoeringen voor Standard-connector|Count|Totaal|Aantal uitvoeringen van de standaardconnector dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStorageConsumption|Facturering van gebruik voor uitvoeringen van opslagverbruik|Count|Totaal|Aantal uitvoeringen van het opslagverbruik dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageNativeOperation|Facturering van gebruik voor systeemeigen uitvoerbewerkingen|Count|Totaal|Aantal systeemeigen uitvoerbewerkingen dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStandardConnector|Facturering van gebruik voor uitvoeringen voor Standard-connector|Count|Totaal|Aantal uitvoeringen van de standaardconnector dat wordt gefactureerd.|Er zijn geen dimensies|
|BillingUsageStorageConsumption|Facturering van gebruik voor uitvoeringen van opslagverbruik|Count|Totaal|Aantal uitvoeringen van het opslagverbruik dat wordt gefactureerd.|Er zijn geen dimensies|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/Volumes

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|AverageOtherLatency|Gemiddelde andere latentie|ms/op|Gemiddeld|Gemiddelde andere (dat wil zeggen niet lezen of schrijven) latentie in milliseconden per bewerking|Er zijn geen dimensies|
|AverageReadLatency|Gemiddelde latentie lezen|ms/op|Gemiddeld|Gemiddelde latentie in milliseconden per bewerking lezen|Er zijn geen dimensies|
|AverageTotalLatency|Gemiddelde totale latentie|ms/op|Gemiddeld|Gemiddelde totale latentie in milliseconden per bewerking|Er zijn geen dimensies|
|AverageWriteLatency|Schrijflatentie van gemiddelde|ms/op|Gemiddeld|De gemiddelde schrijflatentie in milliseconden per bewerking|Er zijn geen dimensies|
|FilesystemOtherOps|Bestandssysteem andere bewerkingen|ops|Gemiddeld|Aantal bestandssysteem andere bewerkingen (dat wil zeggen niet lezen of schrijven)|Er zijn geen dimensies|
|FilesystemReadOps|Leesbewerkingen voor bestandssysteem|ops|Gemiddeld|Aantal bestandssysteem lezen bewerkingen|Er zijn geen dimensies|
|FilesystemTotalOps|Totaal aantal ops bestandssysteem|ops|Gemiddeld|Som van alle bestandssysteembewerkingen|Er zijn geen dimensies|
|FilesystemWriteOps|Bestandssysteem schrijven ops|ops|Gemiddeld|Aantal bewerkingen voor het schrijven van bestandssysteem|Er zijn geen dimensies|
|IoBytesPerOtherOps|I/o-bytes per andere bewerkingen|bytes/op|Gemiddeld|Aantal In/out bytes per andere bewerkingen (dat wil zeggen niet lezen of schrijven)|Er zijn geen dimensies|
|IoBytesPerReadOps|I/o-bytes per lezen ops|bytes/op|Gemiddeld|Aantal In/uit-aantal bytes per leesbewerking|Er zijn geen dimensies|
|IoBytesPerTotalOps|I/o-bytes per op voor alle bewerkingen|bytes/op|Gemiddeld|Som van alle In/uit bytes bewerking|Er zijn geen dimensies|
|IoBytesPerWriteOps|I/o-bytes per schrijven ops|bytes/op|Gemiddeld|Aantal In/out bytes per schrijfbewerking|Er zijn geen dimensies|
|OtherIops|Andere iops|bewerkingen per seconde|Gemiddeld|Andere In/uit bewerking per seconde|Er zijn geen dimensies|
|OtherThroughput|Andere doorvoer|MBps|Gemiddeld|Andere doorvoer (dat wil zeggen niet lezen of schrijven) in megabytes per seconde|Er zijn geen dimensies|
|ReadIops|Gelezen IOP 's|bewerkingen per seconde|Gemiddeld|Lees In/uit-bewerkingen per seconde|Er zijn geen dimensies|
|ReadThroughput|Doorvoer lezen|MBps|Gemiddeld|Doorvoer in MB per seconde gelezen|Er zijn geen dimensies|
|TotalIops|Totale iops|bewerkingen per seconde|Gemiddeld|Som van alle In/uit-bewerkingen per seconde|Er zijn geen dimensies|
|TotalThroughput|Totale doorvoer|MBps|Gemiddeld|Som van alle doorvoer in MB per seconde|Er zijn geen dimensies|
|VolumeAllocatedSize|Volume toegewezen grootte|bytes|Gemiddeld|Toegewezen grootte van het volume (niet de werkelijke gebruikt bytes)|Er zijn geen dimensies|
|VolumeLogicalSize|De logische grootte van volume|bytes|Gemiddeld|Logische grootte van het volume (gebruikte bytes)|Er zijn geen dimensies|
|VolumeSnapshotSize|De grootte van de momentopname van volume|bytes|Gemiddeld|Grootte van alle momentopnamen in volume|Er zijn geen dimensies|
|WriteIops|Iops-schrijfbewerkingen|bewerkingen per seconde|Gemiddeld|Schrijven In/uit-bewerkingen per seconde|Er zijn geen dimensies|
|WriteThroughput|Schrijven van doorvoer|MBps|Gemiddeld|Schrijven van doorvoer in MB per seconde|Er zijn geen dimensies|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|De grootte van de groep toegewezen volume|bytes|Gemiddeld|Toegewezen grootte van de groep (niet de werkelijke gebruikt bytes)|Er zijn geen dimensies|
|VolumePoolAllocatedUsed|Toegewezen volume van toepassingen die worden gebruikt|bytes|Gemiddeld|Toegewezen gebruikte grootte van de groep|Er zijn geen dimensies|
|VolumePoolTotalLogicalSize|Totaal aantal logische grootte van volume van toepassingen|bytes|Gemiddeld|Som van de logische grootte van alle volumes die behoren tot de groep|Er zijn geen dimensies|
|VolumePoolTotalSnapshotSize|Totale momentopname van de volumegrootte van toepassingen|bytes|Gemiddeld|Som van alle momentopnamen in groep|Er zijn geen dimensies|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BytesSentRate|Verzonden bytes|Count|Totaal|Aantal bytes dat de netwerkinterface die is verzonden|Er zijn geen dimensies|
|BytesReceivedRate|Ontvangen bytes|Count|Totaal|Nummer van de netwerkinterface ontvangen bytes|Er zijn geen dimensies|
|PacketsSentRate|Pakketten die worden verzonden|Count|Totaal|Aantal pakketten dat de netwerkinterface die is verzonden|Er zijn geen dimensies|
|PacketsReceivedRate|Ontvangen pakketten|Count|Totaal|Aantal pakketten dat de netwerkinterface die is ontvangen|Er zijn geen dimensies|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|VipAvailability|Pad van beschikbaarheid van gegevens|Count|Gemiddeld|Gemiddelde beschikbaarheid van Load Balancer gegevens pad per duur|FrontendIPAddress, FrontendPort|
|DipAvailability|De integriteitsstatus van de test|Count|Gemiddeld|Gemiddelde Load Balancer de integriteitsstatus test van per duur|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|ByteCount|Aantal bytes|Count|Totaal|Totaal aantal Bytes verzonden binnen een bepaalde periode|FrontendIPAddress, FrontendPort, Direction|
|PacketCount|Telling pakket|Count|Totaal|Totale aantal pakketten binnen een periode wordt verzonden|FrontendIPAddress, FrontendPort, Direction|
|SYNCount|SYN tellen|Count|Totaal|Totaal aantal SYN pakketten binnen een periode wordt verzonden|FrontendIPAddress, FrontendPort, Direction|
|SnatConnectionCount|Aantal SNAT-verbindingen|Count|Totaal|Totaal aantal nieuwe SNAT-verbindingen gemaakt binnen een bepaalde periode|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Toegewezen SNAT poorten (Preview)|Count|Totaal|Totaal aantal SNAT-poorten die zijn toegewezen binnen een bepaalde periode|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Gebruikte SNAT poorten (Preview)|Count|Totaal|Totaal aantal SNAT-poorten die worden gebruikt binnen een bepaalde periode|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QueryVolume|Queryvolume|Count|Totaal|Aantal query's uitgevoerd voor een DNS-zone|Er zijn geen dimensies|
|RecordSetCount|Set-aantal records|Count|Maximum|Aantal recordsets in een DNS-zone|Er zijn geen dimensies|
|RecordSetCapacityUtilization|Gebruik van capaciteit van de recordset|Procent|Maximum|Percentage van de recordset capaciteit die door een DNS-zone wordt gebruikt|Er zijn geen dimensies|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|PacketsInDDoS|Het aantal inkomende pakketten DDoS|CountPerSecond|Maximum|Het aantal inkomende pakketten DDoS|Er zijn geen dimensies|
|PacketsDroppedDDoS|Inkomende pakketten verwijderd DDoS|CountPerSecond|Maximum|Inkomende pakketten verwijderd DDoS|Er zijn geen dimensies|
|PacketsForwardedDDoS|Inkomende pakketten doorgestuurd DDoS|CountPerSecond|Maximum|Inkomende pakketten doorgestuurd DDoS|Er zijn geen dimensies|
|TCPPacketsInDDoS|Inkomende pakketten dat TCP DDoS|CountPerSecond|Maximum|Inkomende pakketten dat TCP DDoS|Er zijn geen dimensies|
|TCPPacketsDroppedDDoS|Inkomende pakketten dat TCP verwijderd DDoS|CountPerSecond|Maximum|Inkomende pakketten dat TCP verwijderd DDoS|Er zijn geen dimensies|
|TCPPacketsForwardedDDoS|Binnenkomend TCP-pakketten doorgestuurd DDoS|CountPerSecond|Maximum|Binnenkomend TCP-pakketten doorgestuurd DDoS|Er zijn geen dimensies|
|UDPPacketsInDDoS|Binnenkomend UDP-pakketten DDoS|CountPerSecond|Maximum|Binnenkomend UDP-pakketten DDoS|Er zijn geen dimensies|
|UDPPacketsDroppedDDoS|Binnenkomend UDP-pakketten verwijderd DDoS|CountPerSecond|Maximum|Binnenkomend UDP-pakketten verwijderd DDoS|Er zijn geen dimensies|
|UDPPacketsForwardedDDoS|Binnenkomend UDP-pakketten doorgestuurd DDoS|CountPerSecond|Maximum|Binnenkomend UDP-pakketten doorgestuurd DDoS|Er zijn geen dimensies|
|BytesInDDoS|Binnenkomende bytes DDoS|BytesPerSecond|Maximum|Binnenkomende bytes DDoS|Er zijn geen dimensies|
|BytesDroppedDDoS|Binnenkomende bytes verwijderd DDoS|BytesPerSecond|Maximum|Binnenkomende bytes verwijderd DDoS|Er zijn geen dimensies|
|BytesForwardedDDoS|Binnenkomende bytes DDoS doorgestuurd|BytesPerSecond|Maximum|Binnenkomende bytes DDoS doorgestuurd|Er zijn geen dimensies|
|TCPBytesInDDoS|Binnenkomende TCP-bytes DDoS|BytesPerSecond|Maximum|Binnenkomende TCP-bytes DDoS|Er zijn geen dimensies|
|TCPBytesDroppedDDoS|Binnenkomend TCP-bytes verwijderd DDoS|BytesPerSecond|Maximum|Binnenkomend TCP-bytes verwijderd DDoS|Er zijn geen dimensies|
|TCPBytesForwardedDDoS|Binnenkomend TCP-bytes DDoS doorgestuurd|BytesPerSecond|Maximum|Binnenkomend TCP-bytes DDoS doorgestuurd|Er zijn geen dimensies|
|UDPBytesInDDoS|Binnenkomend UDP-bytes DDoS|BytesPerSecond|Maximum|Binnenkomend UDP-bytes DDoS|Er zijn geen dimensies|
|UDPBytesDroppedDDoS|Binnenkomend UDP-bytes verwijderd DDoS|BytesPerSecond|Maximum|Binnenkomend UDP-bytes verwijderd DDoS|Er zijn geen dimensies|
|UDPBytesForwardedDDoS|Binnenkomend UDP-bytes DDoS doorgestuurd|BytesPerSecond|Maximum|Binnenkomend UDP-bytes DDoS doorgestuurd|Er zijn geen dimensies|
|IfUnderDDoSAttack|Bij DDoS-aanvallen, of niet|Count|Maximum|Bij DDoS-aanvallen, of niet|Er zijn geen dimensies|
|DDoSTriggerTCPPackets|Binnenkomend TCP-pakket voor het activeren van DDoS-risicobeperking|CountPerSecond|Maximum|Binnenkomend TCP-pakket voor het activeren van DDoS-risicobeperking|Er zijn geen dimensies|
|DDoSTriggerUDPPackets|Binnenkomend UDP-pakketten voor het activeren van DDoS-risicobeperking|CountPerSecond|Maximum|Binnenkomend UDP-pakketten voor het activeren van DDoS-risicobeperking|Er zijn geen dimensies|
|DDoSTriggerSYNPackets|Het aantal inkomende pakketten SYN voor het activeren van DDoS-risicobeperking|CountPerSecond|Maximum|Het aantal inkomende pakketten SYN voor het activeren van DDoS-risicobeperking|Er zijn geen dimensies|
|VipAvailability|Pad van beschikbaarheid van gegevens|Count|Gemiddeld|Gemiddelde beschikbaarheid van de IP-adres per duur|Poort|
|ByteCount|Aantal bytes|Count|Totaal|Totaal aantal Bytes verzonden binnen een bepaalde periode|Poort, richting|
|PacketCount|Telling pakket|Count|Totaal|Totale aantal pakketten binnen een periode wordt verzonden|Poort, richting|
|SynCount|SYN tellen|Count|Totaal|Totaal aantal SYN pakketten binnen een periode wordt verzonden|Poort, richting|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Doorvoer|Doorvoer|BytesPerSecond|Totaal|Aantal bytes per seconde die de Application Gateway is geleverd|Er zijn geen dimensies|
|UnhealthyHostCount|Aantal niet in orde Backendpool|Count|Gemiddeld|Aantal slechte back-end-hosts|BackendSettingsPool|
|HealthyHostCount|Aantal in orde Backendpool|Count|Gemiddeld|Aantal in orde back-end-hosts|BackendSettingsPool|
|TotalRequests|Totaal aantal aanvragen|Count|Totaal|Aantal geslaagde aanvragen die Application Gateway is bereikbaar|BackendSettingsPool|
|FailedRequests|Mislukte aanvragen|Count|Totaal|Aantal mislukte aanvragen die Application Gateway is geleverd|BackendSettingsPool|
|ResponseStatus|Reactiestatus|Count|Totaal|Status van de HTTP-antwoord geretourneerd door Application Gateway|HttpStatusGroup|
|CurrentConnections|Huidige verbindingen|Count|Totaal|Telling van actieve verbindingen tot stand gebracht met Application Gateway|Er zijn geen dimensies|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|AverageBandwidth|Gateway S2S bandbreedte|BytesPerSecond|Gemiddeld|Gemiddelde bandbreedte van de site-naar-site van een gateway in bytes per seconde|Er zijn geen dimensies|
|P2SBandwidth|Gateway P2S bandbreedte|BytesPerSecond|Gemiddeld|Gemiddelde bandbreedte voor punt-naar-site van een gateway in bytes per seconde|Er zijn geen dimensies|
|P2SConnectionCount|Aantal voor P2S-verbindingen|Count|Maximum|Telling van de punt-naar-site-verbinding van een gateway|Protocol|
|TunnelAverageBandwidth|-Tunnelbandbreedte|BytesPerSecond|Gemiddeld|Gemiddelde bandbreedte van een tunnel in bytes per seconde|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tunnel uitgaande Bytes|Bytes|Totaal|Uitgaande bytes van een tunnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tunnel Ingress Bytes|Bytes|Totaal|Binnenkomende bytes van een tunnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tunnel uitgaande pakketten|Count|Totaal|Uitgaande pakket telling van een tunnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tunnel inkomende pakketten|Count|Totaal|Inkomende pakketten telling van een tunnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Verloren gegane pakketten tunnel uitgaande TS verschil|Count|Totaal|Uitgaande pakket drop-telling van het verkeer selector niet overeenstemmen van een tunnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Verloren gegane pakketten tunnel inkomend TS verschil|Count|Totaal|Inkomende pakketten drop-telling van het verkeer selector niet overeenstemmen van een tunnel|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Gemiddeld|Bits ingressing Azure per seconde|Er zijn geen dimensies|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Gemiddeld|Bits egressing Azure per seconde|Er zijn geen dimensies|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Gemiddeld|Bits ingressing Azure per seconde|Er zijn geen dimensies|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Gemiddeld|Bits egressing Azure per seconde|Er zijn geen dimensies|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Gemiddeld|Bits ingressing Azure per seconde|Er zijn geen dimensies|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Gemiddeld|Bits egressing Azure per seconde|Er zijn geen dimensies|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QpsByEndpoint|Query's op eindpunt geretourneerd|Count|Totaal|Aantal keren dat een Traffic Manager-eindpunt is geretourneerd in een bepaalde periode|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Eindpuntstatus door eindpunt|Count|Maximum|1 als de status van een eindpunt-test "ingeschakeld", 0 anders.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Tests is mislukt|Procent|Gemiddeld|% van de bewakingstests verbinding is mislukt|Er zijn geen dimensies|
|AverageRoundtripMs|Gem. Retourtijd (ms)|MilliSeconds|Gemiddeld|Gemiddelde netwerk retourtijd (ms) voor connectiviteit bewakingstests verzonden tussen bron en bestemming|Er zijn geen dimensies|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|RequestCount|Aantal aanvragen|Count|Totaal|Het aantal aanvragen van clients geleverd door de proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Aanvraaggrootte|Bytes|Totaal|Het aantal bytes dat is verzonden op aanvragen van clients naar de proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Antwoordgrootte|Bytes|Totaal|Het aantal bytes dat als de antwoorden van proxy HTTP/S naar clients worden verzonden|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Aantal back-end-aanvragen|Count|Totaal|Het aantal aanvragen dat is verzonden van de proxy HTTP/S back-ends|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Latentie van aanvraag van de back-end|MilliSeconds|Gemiddeld|De tijd berekend op basis van wanneer de aanvraag is verzonden door de proxy HTTP/S naar de back-end totdat de proxy HTTP/S de laatste byte van de reactie van de back-end ontvangen|Back-end|
|TotalLatency|Totale latentie|MilliSeconds|Gemiddeld|De tijd berekend op basis van wanneer de clientaanvraag is ontvangen door de proxy HTTP/S totdat de client de laatste byte van de reactie van de proxy HTTP/S bevestigd|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Percentage van de back-Endstatus|Procent|Gemiddeld|Het percentage van geslaagde statusrapporten tests van de proxy HTTP/S back-ends|Backend, BackendPool|
|WebApplicationFirewallRequestCount|Aantal Web Application Firewall-aanvragen|Count|Totaal|Het aantal aanvragen van clients verwerkt door de Web Application Firewall|PolicyName, RuleName, actie|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Registration.all|Registratiebewerkingen|Count|Totaal|Het aantal registratiebewerkingen (maken, bijwerken, query's uitvoeren en verwijderen). |Er zijn geen dimensies|
|Registration.Create|Bewerkingen voor het maken van registraties|Count|Totaal|Het aantal gemaakte registraties.|Er zijn geen dimensies|
|registration.update|Bewerkingen voor het bijwerken van registraties|Count|Totaal|Het aantal voltooide registratie-updates.|Er zijn geen dimensies|
|registration.get|Bewerkingen voor het lezen van registraties|Count|Totaal|Het aantal uitgevoerde registratiequery's.|Er zijn geen dimensies|
|registration.delete|Bewerkingen voor het verwijderen van registraties|Count|Totaal|Het aantal verwijderde registraties.|Er zijn geen dimensies|
|binnenkomende|Binnenkomende berichten|Count|Totaal|Het aantal voltooide verzonden API-aanroepen. |Er zijn geen dimensies|
|incoming.scheduled|Geplande pushmeldingen verzonden|Count|Totaal|Geplande pushmeldingen geannuleerd|Er zijn geen dimensies|
|incoming.scheduled.cancel|Geplande pushmeldingen geannuleerd|Count|Totaal|Geplande pushmeldingen geannuleerd|Er zijn geen dimensies|
|Scheduled.Pending|Geplande meldingen in behandeling|Count|Totaal|Geplande meldingen in behandeling|Er zijn geen dimensies|
|installation.all|Installatiebeheerbewerkingen|Count|Totaal|Installatiebeheerbewerkingen|Er zijn geen dimensies|
|installation.get|Installatiebewerkingen ophalen|Count|Totaal|Installatiebewerkingen ophalen|Er zijn geen dimensies|
|Installation.upsert|Installatiebewerkingen maken of bijwerken|Count|Totaal|Installatiebewerkingen maken of bijwerken|Er zijn geen dimensies|
|installation.patch|Installatiebewerkingen patchen|Count|Totaal|Installatiebewerkingen patchen|Er zijn geen dimensies|
|installation.delete|Installatiebewerkingen verwijderen|Count|Totaal|Installatiebewerkingen verwijderen|Er zijn geen dimensies|
|outgoing.allpns.success|Voltooide meldingen|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.allpns.invalidpayload|Fouten met nettolading|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS een fout met een onjuiste payload heeft geretourneerd.|Er zijn geen dimensies|
|outgoing.allpns.pnserror|Fouten met het externe meldingssysteem|Count|Totaal|Het aantal pushes dat is mislukt vanwege een probleem in de communicatie met PNS (exclusief verificatieproblemen).|Er zijn geen dimensies|
|outgoing.allpns.channelerror|Fouten met kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat het kanaal ongeldig is, niet is gekoppeld aan de juiste app, beperkt is of verlopen is.|Er zijn geen dimensies|
|outgoing.allpns.badorexpiredchannel|Fouten met slecht of verlopen kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat het kanaal/het token/de registratie-id in de registratie is verlopen of ongeldig is.|Er zijn geen dimensies|
|outgoing.wns.success|Meldingen over voltooide WNS|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.wns.invalidcredentials|WNS-verificatiefouten (ongeldige referenties)|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd. (Windows Live herkent niet de referenties).|Er zijn geen dimensies|
|outgoing.wns.badchannel|WNS-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal-URI in de registratie niet is herkend (WNS-status: 404 niet gevonden).|Er zijn geen dimensies|
|outgoing.wns.expiredchannel|WNS-fout wegens verlopen kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal-URI is verlopen (WNS-status: 410 verwijderd).|Er zijn geen dimensies|
|outgoing.wns.throttled|Meldingen over beperkte WNS|Count|Totaal|Het aantal pushes dat is mislukt omdat WNS deze app beperkt (WNS-status: 406 niet acceptabel).|Er zijn geen dimensies|
|outgoing.wns.tokenproviderunreachable|WNS-verificatiefouten (onbereikbaar)|Count|Totaal|Windows Live is niet bereikbaar.|Er zijn geen dimensies|
|outgoing.wns.invalidtoken|WNS-verificatiefouten (ongeldig token)|Count|Totaal|Het token dat is opgegeven voor WNS is niet geldig (WNS-status: 401 unauthorized).|Er zijn geen dimensies|
|outgoing.wns.wrongtoken|WNS-verificatiefouten (onjuist token)|Count|Totaal|Het token dat is opgegeven voor WNS is geldig, maar voor een andere toepassing (WNS-status: 403-verboden). Dit kan gebeuren als de kanaal-URI in de registratie gekoppeld aan een andere app is. Controleer of de client-app is gekoppeld aan dezelfde app waarvan de referenties die in de notification hub.|Er zijn geen dimensies|
|outgoing.wns.invalidnotificationformat|Ongeldige indeling van de WNS-melding|Count|Totaal|De indeling van de melding is ongeldig (WNS-status: 400). Houd er rekening mee dat WNS biedt niet alle ongeldige payloads weigeren.|Er zijn geen dimensies|
|outgoing.wns.invalidnotificationsize|Fout met ongeldige grootte van de WNS-melding|Count|Totaal|De nettolading van de melding is te groot (WNS-status: 413).|Er zijn geen dimensies|
|outgoing.wns.channelthrottled|De verbinding met het WNS-kanaal is beperkt|Count|Totaal|De melding is verwijderd omdat de kanaal-URI in de registratie is beperkt (WNS-reactieheader: X - WNS - Channelthrottled).|Er zijn geen dimensies|
|outgoing.wns.channeldisconnected|De verbinding met het WNS-kanaal is verbroken|Count|Totaal|De melding is verwijderd omdat de kanaal-URI in de registratie is beperkt (WNS-reactieheader: X-WNS-DeviceConnectionStatus: verbinding verbroken).|Er zijn geen dimensies|
|outgoing.wns.dropped|Meldingen over verloren WNS|Count|Totaal|De melding is verwijderd omdat de kanaal-URI in de registratie is beperkt (X-WNS-NotificationStatus: dropped, maar niet X-WNS-DeviceConnectionStatus: disconnected).|Er zijn geen dimensies|
|outgoing.wns.pnserror|WNS-fouten|Count|Totaal|De melding is niet afgeleverd omdat er fouten zijn opgetreden in de communicatie met WNS.|Er zijn geen dimensies|
|Outgoing.WNS.authenticationerror|WNS-verificatiefouten|Count|Totaal|De melding is niet afgeleverd omdat er fouten zijn opgetreden in de communicatie met Windows Live, vanwege ongeldige referenties of vanwege een onjuist token.|Er zijn geen dimensies|
|outgoing.apns.success|Meldingen over voltooide APNS|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.apns.invalidcredentials|APNS-verificatiefouten|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|outgoing.apns.badchannel|APNS-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat het token ongeldig is (APNS-statuscode: 8).|Er zijn geen dimensies|
|outgoing.apns.expiredchannel|APNS-fout wegens verlopen kanaal|Count|Totaal|Het aantal tokens dat ongeldig is gemaakt door het APNS-feedbackkanaal.|Er zijn geen dimensies|
|outgoing.apns.invalidnotificationsize|Fout met ongeldige grootte van APNS-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload te groot is (APNS-statuscode: 7).|Er zijn geen dimensies|
|outgoing.apns.pnserror|APNS-fouten|Count|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met APNS.|Er zijn geen dimensies|
|outgoing.gcm.success|Meldingen over voltooide GCM|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.gcm.invalidcredentials|GCM-verificatiefouten (ongeldige referenties)|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|outgoing.gcm.badchannel|GCM-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de registratie-id in de registratie niet is herkend (GCM-resultaat: Ongeldige registratie).|Er zijn geen dimensies|
|outgoing.gcm.expiredchannel|GCM-fout wegens verlopen kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de registratie-id in de registratie is verlopen (GCM-resultaat: NotRegistered).|Er zijn geen dimensies|
|Outgoing.GCM.throttled|Meldingen over beperkte GCM|Count|Totaal|Het aantal pushes dat is mislukt omdat GCM deze app beperkt (GCM-statuscode: 501-599 of result: niet beschikbaar).|Er zijn geen dimensies|
|outgoing.gcm.invalidnotificationformat|Ongeldige indeling van GCM-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload niet juist is opgemaakt (GCM-resultaat: InvalidDataKey of InvalidTtl).|Er zijn geen dimensies|
|outgoing.gcm.invalidnotificationsize|Fout met ongeldige grootte van GCM-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload te groot is (GCM-resultaat: MessageTooBig).|Er zijn geen dimensies|
|outgoing.gcm.wrongchannel|GCM-fout wegens onjuist kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de registratie-id in de registratie niet gekoppeld aan de huidige app is (GCM-resultaat: InvalidPackageName).|Er zijn geen dimensies|
|outgoing.gcm.pnserror|GCM-fouten|Count|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met GCM.|Er zijn geen dimensies|
|Outgoing.GCM.authenticationerror|GCM-verificatiefouten|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties de referenties niet accepteert, worden geblokkeerd of id van de afzender niet juist is geconfigureerd in de app (GCM-resultaat: MismatchedSenderId).|Er zijn geen dimensies|
|outgoing.mpns.success|Meldingen over voltooide MPNS|Count|Totaal|Het aantal voltooide meldingen.|Er zijn geen dimensies|
|outgoing.mpns.invalidcredentials|Ongeldige MPNS-referenties|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|outgoing.mpns.badchannel|MPNS-fout wegens slecht kanaal|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal-URI in de registratie niet is herkend (MPNS-status: 404 niet gevonden).|Er zijn geen dimensies|
|Outgoing.mpns.throttled|Meldingen over beperkte MPNS|Count|Totaal|Het aantal pushes dat is mislukt omdat MPNS deze app beperkt (WNS MPNS: 406 niet acceptabel).|Er zijn geen dimensies|
|outgoing.mpns.invalidnotificationformat|Ongeldige indeling van de MPNS-melding|Count|Totaal|Het aantal pushes dat is mislukt omdat de payload van de melding te groot is.|Er zijn geen dimensies|
|outgoing.mpns.channeldisconnected|De verbinding met het MPNS-kanaal is verbroken|Count|Totaal|Het aantal pushes dat is mislukt omdat de kanaal-URI in de registratie niet is verbonden (MPNS-status: 412-niet gevonden).|Er zijn geen dimensies|
|Outgoing.mpns.Dropped|Meldingen over verloren MPNS|Count|Totaal|Het aantal pushes dat is verwijderd door MPNS (MPNS-reactieheader: X-NotificationStatus: QueueFull of onderdrukte).|Er zijn geen dimensies|
|outgoing.mpns.pnserror|MPNS-fouten|Count|Totaal|Het aantal pushes dat is mislukt vanwege fouten in de communicatie met MPNS.|Er zijn geen dimensies|
|Outgoing.mpns.authenticationerror|MPNS-verificatiefouten|Count|Totaal|Het aantal pushes dat is mislukt omdat PNS de opgegeven referenties niet accepteert of omdat de referenties zijn geblokkeerd.|Er zijn geen dimensies|
|notificationhub.pushes|Alle uitgaande meldingen|Count|Totaal|Alle uitgaande meldingen van de Notification Hub|Er zijn geen dimensies|
|incoming.all.requests|Alle binnenkomende aanvragen|Count|Totaal|Totaal aantal binnenkomende aanvragen voor een Notification Hub|Er zijn geen dimensies|
|Incoming.all.failedrequests|Alle binnenkomende mislukte aanvragen|Count|Totaal|Totaal aantal binnenkomende mislukte aanvragen voor een Notification Hub|Er zijn geen dimensies|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Average_ percentage beschikbare Inodes|Percentage vrije Inodes|Count|Gemiddeld|Average_ percentage beschikbare Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ % vrije ruimte|Percentage vrije ruimte|Count|Gemiddeld|Average_ % vrije ruimte|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage gebruikte Inodes|Percentage gebruikte Inodes|Count|Gemiddeld|Average_ percentage gebruikte Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage gebruikte ruimte|Percentage gebruikte ruimte|Count|Gemiddeld|Average_ percentage gebruikte ruimte|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk gelezen Bytes per seconde|Bytes gelezen op schijf/sec|Count|Gemiddeld|Average_Disk gelezen Bytes per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Schijf lezen per seconde|Count|Gemiddeld|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Schijfoverdrachten per seconde|Schijfoverdrachten per seconde|Count|Gemiddeld|Average_Disk Schijfoverdrachten per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk geschreven Bytes per seconde|Bytes geschreven naar schijf/sec|Count|Gemiddeld|Average_Disk geschreven Bytes per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk schrijfbewerkingen per seconde|Schijf schrijven per seconde|Count|Gemiddeld|Average_Disk schrijfbewerkingen per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Beschikbare Megabytes|Count|Gemiddeld|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Logische schijf Bytes per seconde|Count|Gemiddeld|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage beschikbaar geheugen|Percentage beschikbaar geheugen|Count|Gemiddeld|Average_ percentage beschikbaar geheugen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Percentage beschikbare wisselruimte Average_|Percentage beschikbare wisselruimte|Count|Gemiddeld|Percentage beschikbare wisselruimte Average_|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage gebruikt geheugen|Percentage gebruikt geheugen|Count|Gemiddeld|Average_ percentage gebruikt geheugen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage gebruikte wisselruimte|Percentage gebruikte wisselruimte|Count|Gemiddeld|Average_ percentage gebruikte wisselruimte|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available megabytes (MB) geheugen|Beschikbaar geheugen in megabytes|Count|Gemiddeld|Average_Available megabytes (MB) geheugen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available megabytes voor wisselen|Beschikbare megabytes voor wisselen|Count|Gemiddeld|Average_Available megabytes voor wisselen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page leesbewerkingen per seconde|Paginaleesbewerkingen per seconde|Count|Gemiddeld|Average_Page leesbewerkingen per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page schrijfbewerkingen per seconde|Paginaschrijfbewerkingen per seconde|Count|Gemiddeld|Average_Page schrijfbewerkingen per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Pagina's per seconde|Count|Gemiddeld|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|Gebruikte wisselruimte in megabytes|Count|Gemiddeld|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used geheugen in megabytes|Gebruikt geheugen in megabytes|Count|Gemiddeld|Average_Used geheugen in megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes verzonden|Totaal aantal verzonden Bytes|Count|Gemiddeld|Average_Total Bytes verzonden|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes ontvangen|Totaal aantal ontvangen Bytes|Count|Gemiddeld|Average_Total Bytes ontvangen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Totaal aantal bytes|Count|Gemiddeld|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total pakketten verzonden|Totaal aantal verzonden pakketten|Count|Gemiddeld|Average_Total pakketten verzonden|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total pakketten ontvangen|Totaal aantal ontvangen pakketten|Count|Gemiddeld|Average_Total pakketten ontvangen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx-fouten|Totaal aantal Rx-fouten|Count|Gemiddeld|Average_Total Rx-fouten|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Tx-fouten|Totaal aantal Tx-fouten|Count|Gemiddeld|Average_Total Tx-fouten|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total conflicten|Totaal aantal conflicten|Count|Gemiddeld|Average_Total conflicten|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Schijf sec/lezen|Gem. Schijf sec/lezen|Count|Gemiddeld|Average_Avg. Schijf sec/lezen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Schijfoverdrachten per seconde|Gem. Schijfoverdrachten per seconde|Count|Gemiddeld|Average_Avg. Schijfoverdrachten per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Schijf sec/schrijven|Gem. Schijf sec/schrijven|Count|Gemiddeld|Average_Avg. Schijf sec/schrijven|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical schijf Bytes per seconde|Fysieke schijf Bytes per seconde|Count|Gemiddeld|Average_Physical schijf Bytes per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Tijd in beschermde modus Average_Pct|PCT gemachtigde tijd|Count|Gemiddeld|Tijd in beschermde modus Average_Pct|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Tijd in gebruikersmodus van Average_Pct|PCT in gebruikersmodus|Count|Gemiddeld|Tijd in gebruikersmodus van Average_Pct|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used geheugen kB|Gebruikt geheugen kB|Count|Gemiddeld|Average_Used geheugen kB|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual gedeeld geheugen|Virtuele gedeeld geheugen|Count|Gemiddeld|Average_Virtual gedeeld geheugen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage DPC-tijd|Percentage DPC-tijd|Count|Gemiddeld|Average_ percentage DPC-tijd|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ % niet-actieve tijd|% Niet-actieve tijd|Count|Gemiddeld|Average_ % niet-actieve tijd|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage Interrupt-tijd|% Interrupt Time|Count|Gemiddeld|Average_ percentage Interrupt-tijd|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ % i/o-wachttijd|Percentage wachttijd I/O|Count|Gemiddeld|Average_ % i/o-wachttijd|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Tijd in Nice Average_|Percentage tijd in nice|Count|Gemiddeld|Tijd in Nice Average_|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ % gemachtigde tijd|% Gemachtigde tijd|Count|Gemiddeld|Average_ % gemachtigde tijd|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ % processortijd|% Processortijd|Count|Gemiddeld|Average_ % processortijd|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage tijd in gebruikersmodus|Percentage tijd in gebruikersmodus|Count|Gemiddeld|Average_ percentage tijd in gebruikersmodus|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free fysiek geheugen|Vrij fysiek geheugen|Count|Gemiddeld|Average_Free fysiek geheugen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free ruimte in Wisselgeheugenbestanden|Vrije ruimte in Wisselgeheugenbestanden|Count|Gemiddeld|Average_Free ruimte in Wisselgeheugenbestanden|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free virtueel geheugen|Vrij virtueel geheugen|Count|Gemiddeld|Average_Free virtueel geheugen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|Processen|Count|Gemiddeld|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size opgeslagen In Pagineringbestanden|Grootte opgeslagen In Pagineringbestanden|Count|Gemiddeld|Average_Size opgeslagen In Pagineringbestanden|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Actieve tijdsduur|Count|Gemiddeld|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Gebruikers|Count|Gemiddeld|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Schijf sec/lezen|Gem. Schijf sec/lezen|Count|Gemiddeld|Average_Avg. Schijf sec/lezen|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Schijf sec/schrijven|Gem. Schijf sec/schrijven|Count|Gemiddeld|Average_Avg. Schijf sec/schrijven|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Current wachtrijlengte voor schijf|Huidige wachtrijlengte voor schijf|Count|Gemiddeld|Average_Current wachtrijlengte voor schijf|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Schijf lezen per seconde|Count|Gemiddeld|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Schijfoverdrachten per seconde|Schijfoverdrachten per seconde|Count|Gemiddeld|Average_Disk Schijfoverdrachten per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk schrijfbewerkingen per seconde|Schijf schrijven per seconde|Count|Gemiddeld|Average_Disk schrijfbewerkingen per seconde|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Beschikbare Megabytes|Count|Gemiddeld|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ % vrije ruimte|Percentage vrije ruimte|Count|Gemiddeld|Average_ % vrije ruimte|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available megabytes (MB)|Beschikbare megabytes (MB)|Count|Gemiddeld|Average_Available megabytes (MB)|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ percentage toegewezen Bytes In gebruik|% Toegewezen Bytes In gebruik|Count|Gemiddeld|Average_ percentage toegewezen Bytes In gebruik|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Ontvangen bytes per seconde|Count|Gemiddeld|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Verzonden bytes per seconde|Count|Gemiddeld|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes in totaal/sec|Totaal aantal bytes per seconde|Count|Gemiddeld|Average_Bytes in totaal/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_ % processortijd|% Processortijd|Count|Gemiddeld|Average_ % processortijd|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processor-wachtrijlengte|Lengte van de processorwachtrij|Count|Gemiddeld|Average_Processor-wachtrijlengte|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Heartbeat|Heartbeat|Count|Totaal|Heartbeat|Computer, OSType, versie, SourceComputerId|
|Update|Update|Count|Gemiddeld|Update|Computer, Product, classificatie, UpdateState, optioneel, goedgekeurde|
|Gebeurtenis|Gebeurtenis|Count|Gemiddeld|Gebeurtenis|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QueryDuration|Queryduur van de|Milliseconden|Gemiddeld|Duur van de DAX-Query in de laatste-interval|Er zijn geen dimensies|
|QueryPoolJobQueueLength|Threads: Lengte van taakwachtrij pool query|Count|Gemiddeld|Het aantal taken in de wachtrij van de querythreadpool.|Er zijn geen dimensies|
|qpu_high_utilization_metric|Hoog QPU-gebruik|Count|Totaal|QPU hoog gebruik In de laatste minuut, 1 voor hoge QPU-gebruik, anders 0|Er zijn geen dimensies|
|memory_metric|Geheugen|Bytes|Gemiddeld|Geheugen. Bereik 0-3 GB voor A1-, 0-5 GB voor A2-, 0-10 GB voor de A3, 0-25 GB voor A4, 0-50 GB voor a5 tot en 0-100 GB voor A6|Er zijn geen dimensies|
|memory_thrashing_metric|Geheugenthrashing|Procent|Gemiddeld|Gemiddelde geheugenthrashing.|Er zijn geen dimensies|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Count|Totaal|Voltooide ListenerConnections voor Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Count|Totaal|ClientError op ListenerConnections voor Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Count|Totaal|ServerError op ListenerConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Count|Totaal|Voltooide SenderConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Count|Totaal|ClientError op SenderConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Count|Totaal|ServerError op SenderConnections voor Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Count|Totaal|Totale ListenerConnections voor Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Count|Totaal|Totale SenderConnections-aanvragen voor Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Count|Totaal|Totale ActiveConnections voor Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Count|Totaal|Totale ActiveListeners voor Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Count|Totaal|Totale BytesTransferred voor Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Count|Totaal|Totale ListenerDisconnects voor Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Count|Totaal|Total SenderDisconnects for Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|SearchLatency|Zoeklatentie|Seconden|Gemiddeld|Gemiddelde zoeklatentie voor de search-service|Er zijn geen dimensies|
|SearchQueriesPerSecond|Zoekquery's per seconde|CountPerSecond|Gemiddeld|Zoekquery's per seconde voor de search-service|Er zijn geen dimensies|
|ThrottledSearchQueriesPercentage|Percentage beperkte zoekquery's|Procent|Gemiddeld|Percentage van de zoekquery's die zijn beperkt voor de search-service|Er zijn geen dimensies|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Succesfulrequests|Geslaagde aanvragen (Preview)|Count|Totaal|Totaal aantal verwerkte aanvragen voor een naamruimte (Preview)|EntityName|
|ServerErrors|Serverfouten. (Preview)|Count|Totaal|Serverfouten voor Microsoft.ServiceBus. (Preview)|EntityName|
|UserErrors|Gebruikersfouten. (Preview)|Count|Totaal|Gebruikersfouten voor Microsoft.ServiceBus. (Preview)|EntityName|
|ThrottledRequests|Beperkte aanvragen. (Preview)|Count|Totaal|Beperkte aanvragen voor Microsoft.ServiceBus. (Preview)|EntityName|
|IncomingRequests|Inkomende aanvragen (Preview)|Count|Totaal|Binnenkomende aanvragen voor Microsoft.ServiceBus. (Preview)|EntityName|
|IncomingMessages|Binnenkomende berichten (Preview)|Count|Totaal|Binnenkomende berichten voor Microsoft.ServiceBus. (Preview)|EntityName|
|OutgoingMessages|Uitgaande berichten (Preview)|Count|Totaal|Uitgaande berichten voor Microsoft.ServiceBus. (Preview)|EntityName|
|ActiveConnections|ActiveConnections (Preview)|Count|Totaal|Totaal aantal actieve verbindingen voor Microsoft.ServiceBus. (Preview)|Er zijn geen dimensies|
|Grootte|Grootte (Preview)|Bytes|Gemiddeld|Grootte van een wachtrij/onderwerp in bytes. (Preview)|EntityName|
|Berichten|Aantal berichten in een wachtrij/onderwerp. (Preview)|Count|Gemiddeld|Aantal berichten in een wachtrij/onderwerp. (Preview)|EntityName|
|ActiveMessages|Aantal actieve berichten in een wachtrij/onderwerp. (Preview)|Count|Gemiddeld|Aantal actieve berichten in een wachtrij/onderwerp. (Preview)|EntityName|
|CPUXNS|CPU-gebruik per naamruimte|Procent|Maximum|Metrische gegevens voor het CPU-gebruik van de Premium-naamruimte voor Service Bus|Er zijn geen dimensies|
|WSXNS|Geheugengebruik per naamruimte|Procent|Maximum|Metrische gegevens voor het geheugengebruik van de Premium-naamruimte voor Service Bus|Er zijn geen dimensies|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ConnectionCount|Aantal verbindingen|Count|Maximum|De hoeveelheid gebruikersverbinding.|Eindpunt|
|MessageCount|Aantal berichten|Count|Totaal|De totale hoeveelheid berichten.|Er zijn geen dimensies|
|InboundTraffic|Binnenkomend verkeer|Bytes|Totaal|Het inkomende verkeer van de service|Er zijn geen dimensies|
|OutboundTraffic|Uitgaand verkeer|Bytes|Totaal|Het uitgaande verkeer van de service|Er zijn geen dimensies|
|UserErrors|Gebruikersfouten|Procent|Maximum|Het percentage van de gebruikersfouten|Er zijn geen dimensies|
|SystemErrors|Systeemfouten|Procent|Maximum|Het percentage van fouten in het bestandssysteem|Er zijn geen dimensies|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Er zijn geen dimensies|
|physical_data_read_percent|Gegevens-I/O-percentage|Procent|Gemiddeld|Gegevens-I/O-percentage|Er zijn geen dimensies|
|log_write_percent|Percentage logboek-IO|Procent|Gemiddeld|Percentage logboek-IO|Er zijn geen dimensies|
|dtu_consumption_percent|DTU-percentage|Procent|Gemiddeld|DTU-percentage|Er zijn geen dimensies|
|opslag|Totale databasegrootte|Bytes|Maximum|Totale databasegrootte|Er zijn geen dimensies|
|connection_successful|Geslaagde verbindingen|Count|Totaal|Geslaagde verbindingen|Er zijn geen dimensies|
|connection_failed|Mislukte verbindingen|Count|Totaal|Mislukte verbindingen|Er zijn geen dimensies|
|blocked_by_firewall|Geblokkeerd door Firewall|Count|Totaal|Geblokkeerd door Firewall|Er zijn geen dimensies|
|Impasse|Deadlocks|Count|Totaal|Deadlocks|Er zijn geen dimensies|
|storage_percent|Databaseomvangpercentage|Procent|Maximum|Databaseomvangpercentage|Er zijn geen dimensies|
|xtp_storage_percent|Percentage van in-Memory OLTP-opslag|Procent|Gemiddeld|Percentage van in-Memory OLTP-opslag|Er zijn geen dimensies|
|workers_percent|Werknemerspercentage|Procent|Gemiddeld|Werknemerspercentage|Er zijn geen dimensies|
|sessions_percent|Sessiepercentage|Procent|Gemiddeld|Sessiepercentage|Er zijn geen dimensies|
|dtu_limit|DTU-limiet|Count|Gemiddeld|DTU-limiet|Er zijn geen dimensies|
|dtu_used|DTU gebruikt|Count|Gemiddeld|DTU gebruikt|Er zijn geen dimensies|
|dwu_limit|DWU-limiet|Count|Maximum|DWU-limiet|Er zijn geen dimensies|
|dwu_consumption_percent|DWU-percentage|Procent|Maximum|DWU-percentage|Er zijn geen dimensies|
|dwu_used|Gebruikte DWU|Count|Maximum|Gebruikte DWU|Er zijn geen dimensies|
|dw_cpu_percent|DW knooppuntniveau CPU-percentage|Procent|Gemiddeld|DW knooppuntniveau CPU-percentage|DwLogicalNodeId|
|dw_physical_data_read_percent|DW knooppunt niveau gegevens i/o-percentage|Procent|Gemiddeld|DW knooppunt niveau gegevens i/o-percentage|DwLogicalNodeId|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Er zijn geen dimensies|
|physical_data_read_percent|Gegevens-I/O-percentage|Procent|Gemiddeld|Gegevens-I/O-percentage|Er zijn geen dimensies|
|log_write_percent|Percentage logboek-IO|Procent|Gemiddeld|Percentage logboek-IO|Er zijn geen dimensies|
|dtu_consumption_percent|DTU-percentage|Procent|Gemiddeld|DTU-percentage|Er zijn geen dimensies|
|storage_percent|Opslagpercentage|Procent|Gemiddeld|Opslagpercentage|Er zijn geen dimensies|
|workers_percent|Werknemerspercentage|Procent|Gemiddeld|Werknemerspercentage|Er zijn geen dimensies|
|sessions_percent|Sessiepercentage|Procent|Gemiddeld|Sessiepercentage|Er zijn geen dimensies|
|eDTU_limit|eDTU-limiet|Count|Gemiddeld|eDTU-limiet|Er zijn geen dimensies|
|storage_limit|Limiet voor opslag|Bytes|Gemiddeld|Limiet voor opslag|Er zijn geen dimensies|
|eDTU_used|eDTU gebruikt|Count|Gemiddeld|eDTU gebruikt|Er zijn geen dimensies|
|storage_used|Gebruikte opslag|Bytes|Gemiddeld|Gebruikte opslag|Er zijn geen dimensies|
|xtp_storage_percent|Percentage van in-Memory OLTP-opslag|Procent|Gemiddeld|Percentage van in-Memory OLTP-opslag|Er zijn geen dimensies|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|virtual_core_count|Aantal virtuele kernen|Count|Gemiddeld|Aantal virtuele kernen|Er zijn geen dimensies|
|avg_cpu_percent|Gemiddelde CPU-percentage|Procent|Gemiddeld|Gemiddelde CPU-percentage|Er zijn geen dimensies|
|reserved_storage_mb|Gereserveerde opslagruimte|Count|Gemiddeld|Gereserveerde opslagruimte|Er zijn geen dimensies|
|storage_space_used_mb|Opslagruimte die wordt gebruikt|Count|Gemiddeld|Opslagruimte die wordt gebruikt|Er zijn geen dimensies|
|io_requests|Aantal i/o-aanvragen|Count|Gemiddeld|Aantal i/o-aanvragen|Er zijn geen dimensies|
|io_bytes_read|I/o-bytes lezen|Bytes|Gemiddeld|I/o-bytes lezen|Er zijn geen dimensies|
|io_bytes_written|I/o-bytes geschreven|Bytes|Gemiddeld|I/o-bytes geschreven|Er zijn geen dimensies|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|UsedCapacity|Gebruikte capaciteit|Bytes|Totaal|Gebruikte capaciteit van account|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|De ResponseType, GeoType, ApiName, verificatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|GeoType, ApiName, verificatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|GeoType, ApiName, verificatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in AverageE2ELatency.|GeoType, ApiName, verificatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|GeoType, ApiName, verificatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|GeoType, ApiName, verificatie|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BlobCapacity|Blobcapaciteit|Bytes|Totaal|De hoeveelheid opslag die wordt gebruikt door de Blob-service van het opslagaccount (in bytes).|BlobType|
|BlobCount|Aantal blobs|Count|Totaal|Het aantal blobs in de Blob-service van het opslagaccount.|BlobType|
|ContainerCount|Aantal blobcontainers|Count|Gemiddeld|Het aantal containers in de Blob-service van het opslagaccount.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|De ResponseType, GeoType, ApiName, verificatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|GeoType, ApiName, verificatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|GeoType, ApiName, verificatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in AverageE2ELatency.|GeoType, ApiName, verificatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|GeoType, ApiName, verificatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|GeoType, ApiName, verificatie|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|FileCapacity|Bestandscapaciteit|Bytes|Gemiddeld|De hoeveelheid opslag die wordt gebruikt door de File-service van het opslagaccount (in bytes).|Er zijn geen dimensies|
|FileCount|Aantal bestanden|Count|Gemiddeld|Het aantal bestanden in de File-service van het opslagaccount.|Er zijn geen dimensies|
|FileShareCount|Aantal bestandsshares|Count|Gemiddeld|Het aantal bestandsshares in de File-service van het opslagaccount.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|De ResponseType, GeoType, ApiName, verificatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|GeoType, ApiName, verificatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|GeoType, ApiName, verificatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in AverageE2ELatency.|GeoType, ApiName, verificatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|GeoType, ApiName, verificatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|GeoType, ApiName, verificatie|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|QueueCapacity|Wachtrijcapaciteit|Bytes|Gemiddeld|De hoeveelheid opslag die wordt gebruikt door de Queue-service van het opslagaccount (in bytes).|Er zijn geen dimensies|
|QueueCount|Aantal wachtrijen|Count|Gemiddeld|Het aantal wachtrijen in de Queue-service van het opslagaccount.|Er zijn geen dimensies|
|QueueMessageCount|Aantal wachtrijberichten|Count|Gemiddeld|Het geschatte aantal wachtrijberichten in de Queue-service van het opslagaccount.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|De ResponseType, GeoType, ApiName, verificatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|GeoType, ApiName, verificatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|GeoType, ApiName, verificatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in AverageE2ELatency.|GeoType, ApiName, verificatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|GeoType, ApiName, verificatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|GeoType, ApiName, verificatie|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|TableCapacity|Tabelcapaciteit|Bytes|Gemiddeld|De hoeveelheid opslag die wordt gebruikt door de Table-service van het opslagaccount (in bytes).|Er zijn geen dimensies|
|TableCount|Aantal tabellen|Count|Gemiddeld|Het aantal tabellen in de Table-service van het opslagaccount.|Er zijn geen dimensies|
|TableEntityCount|Aantal tabelentiteiten|Count|Gemiddeld|Het aantal tabelentiteiten in de Table-service van het opslagaccount.|Er zijn geen dimensies|
|Transacties|Transacties|Count|Totaal|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie.|De ResponseType, GeoType, ApiName, verificatie|
|Inkomend verkeer|Inkomend verkeer|Bytes|Totaal|De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure.|GeoType, ApiName, verificatie|
|Uitgaand verkeer|Uitgaand verkeer|Bytes|Totaal|De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer.|GeoType, ApiName, verificatie|
|SuccessServerLatency|Geslaagde serverlatentie|Milliseconden|Gemiddeld|De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde bevat niet de netwerklatentie die is opgegeven in AverageE2ELatency.|GeoType, ApiName, verificatie|
|SuccessE2ELatency|Geslaagde E2E-latentie|Milliseconden|Gemiddeld|De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen.|GeoType, ApiName, verificatie|
|Beschikbaarheid|Beschikbaarheid|Procent|Gemiddeld|Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot een afgenomen beschikbaarheid voor de opslagservice of de opgegeven API-bewerking.|GeoType, ApiName, verificatie|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|ResourceUtilization|Gebruikspercentage voor Streaming-eenheden|Procent|Maximum|Gebruikspercentage voor Streaming-eenheden|Er zijn geen dimensies|
|Invoergebeurtenissen groter is dan|Invoergebeurtenissen|Count|Totaal|Invoergebeurtenissen|Er zijn geen dimensies|
|InputEventBytes|Invoergebeurtenisbytes|Bytes|Totaal|Invoergebeurtenisbytes|Er zijn geen dimensies|
|LateInputEvents|Late invoergebeurtenissen|Count|Totaal|Late invoergebeurtenissen|Er zijn geen dimensies|
|Uitvoergebeurtenissen|Uitvoergebeurtenis|Count|Totaal|Uitvoergebeurtenis|Er zijn geen dimensies|
|ConversionErrors|Gegevensconversiefouten|Count|Totaal|Gegevensconversiefouten|Er zijn geen dimensies|
|Fouten|Runtimefouten|Count|Totaal|Runtimefouten|Er zijn geen dimensies|
|DroppedOrAdjustedEvents|Gebeurtenissen met een andere volgorde|Count|Totaal|Gebeurtenissen met een andere volgorde|Er zijn geen dimensies|
|AMLCalloutRequests|Functieaanvragen|Count|Totaal|Functieaanvragen|Er zijn geen dimensies|
|AMLCalloutFailedRequests|Mislukte functieaanvragen|Count|Totaal|Mislukte functieaanvragen|Er zijn geen dimensies|
|AMLCalloutInputEvents|Functiegebeurtenissen|Count|Totaal|Functiegebeurtenissen|Er zijn geen dimensies|
|DeserializationError|Fouten in invoerdeserialisatie|Count|Totaal|Fouten in invoerdeserialisatie|Er zijn geen dimensies|
|EarlyInputEvents|Vroege-invoergebeurtenissen|Count|Totaal|Vroege-invoergebeurtenissen|Er zijn geen dimensies|
|OutputWatermarkDelaySeconds|Watermerkvertraging|Seconden|Maximum|Watermerkvertraging|Er zijn geen dimensies|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|IngressReceivedMessages|Binnenkomende berichten ontvangen|Count|Totaal|Aantal berichten lezen van alle Event hub of IoT-hub bronnen van gebeurtenissen|Er zijn geen dimensies|
|IngressReceivedInvalidMessages|Inkomende gegevens ontvangen ongeldig berichten|Count|Totaal|Aantal ongeldige berichten lezen van alle Event hub of IoT-hub bronnen van gebeurtenissen|Er zijn geen dimensies|
|IngressReceivedBytes|Inkomend verkeer ontvangen Bytes|Bytes|Totaal|Aantal bytes lezen van alle bronnen van gebeurtenissen|Er zijn geen dimensies|
|IngressStoredBytes|Binnenkomende Bytes opgeslagen|Bytes|Totaal|Totale grootte van gebeurtenissen is verwerkt en beschikbaar voor query 's|Er zijn geen dimensies|
|IngressStoredEvents|Inkomende gebeurtenissen opgeslagen|Count|Totaal|Telt het aantal samengevoegde gebeurtenissen is verwerkt en beschikbaar voor query 's|Er zijn geen dimensies|
|IngressReceivedMessagesTimeLag|Vertraging voor inkomend verkeer ontvangen berichten|Seconden|Maximum|Verschil tussen de tijd die het bericht is in de wachtrij in de gebeurtenisbron en de tijd die in inkomend verkeer wordt verwerkt|Er zijn geen dimensies|
|IngressReceivedMessagesCountLag|Aantal vertraging van inkomend verkeer ontvangen berichten|Count|Gemiddeld|Verschil tussen het volgnummer van bericht van de laatste in de wachtrij geplaatste gegevensbron in de gebeurtenis partitie en volgorde aantal bericht dat wordt verwerkt in de inkomende gegevens|Er zijn geen dimensies|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|IngressReceivedMessages|Binnenkomende berichten ontvangen|Count|Totaal|Aantal berichten lezen uit de bron van gebeurtenis|Er zijn geen dimensies|
|IngressReceivedInvalidMessages|Inkomende gegevens ontvangen ongeldig berichten|Count|Totaal|Aantal ongeldige berichten lezen uit de bron van gebeurtenis|Er zijn geen dimensies|
|IngressReceivedBytes|Inkomend verkeer ontvangen Bytes|Bytes|Totaal|Aantal bytes dat is gelezen uit de bron van gebeurtenis|Er zijn geen dimensies|
|IngressStoredBytes|Binnenkomende Bytes opgeslagen|Bytes|Totaal|Totale grootte van gebeurtenissen is verwerkt en beschikbaar voor query 's|Er zijn geen dimensies|
|IngressStoredEvents|Inkomende gebeurtenissen opgeslagen|Count|Totaal|Telt het aantal samengevoegde gebeurtenissen is verwerkt en beschikbaar voor query 's|Er zijn geen dimensies|
|IngressReceivedMessagesTimeLag|Vertraging voor inkomend verkeer ontvangen berichten|Seconden|Maximum|Verschil tussen de tijd die het bericht is in de wachtrij in de gebeurtenisbron en de tijd die in inkomend verkeer wordt verwerkt|Er zijn geen dimensies|
|IngressReceivedMessagesCountLag|Aantal vertraging van inkomend verkeer ontvangen berichten|Count|Gemiddeld|Verschil tussen het volgnummer van bericht van de laatste in de wachtrij geplaatste gegevensbron in de gebeurtenis partitie en volgorde aantal bericht dat wordt verwerkt in de inkomende gegevens|Er zijn geen dimensies|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuPercentage|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Procent|Gemiddeld|Geheugenpercentage|Exemplaar|
|DiskQueueLength|Lengte van de wachtrij voor de schijf|Count|Gemiddeld|Lengte van de wachtrij voor de schijf|Exemplaar|
|HttpQueueLength|Lengte van de HTTP-wachtrij|Count|Gemiddeld|Lengte van de HTTP-wachtrij|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (met uitzondering van de functies)

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|Aanvragen|Aanvragen|Count|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http101|HTTP-fout 101|Count|Totaal|HTTP-fout 101|Exemplaar|
|Http2xx|HTTP 2xx|Count|Totaal|HTTP 2xx|Exemplaar|
|Http3xx|HTTP 3xx|Count|Totaal|HTTP 3xx|Exemplaar|
|Http401|HTTP 401|Count|Totaal|HTTP 401|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP-fout 404|Count|Totaal|HTTP-fout 404|Exemplaar|
|Http406|HTTP-fout 406|Count|Totaal|HTTP-fout 406|Exemplaar|
|Http4xx|Http 4xx|Count|Totaal|Http 4xx|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Gemiddeld|Geheugenwerkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddeld geheugenwerkset|Bytes|Gemiddeld|Gemiddeld geheugenwerkset|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddeld|Gemiddelde reactietijd|Exemplaar|
|AppConnections|Verbindingen|Count|Gemiddeld|Verbindingen|Exemplaar|
|Ingangen|Aantal ingangen|Count|Gemiddeld|Aantal ingangen|Exemplaar|
|Threads|Aantal threads|Count|Gemiddeld|Aantal threads|Exemplaar|
|PrivateBytes|Privébytes|Bytes|Gemiddeld|Privébytes|Exemplaar|
|IoReadBytesPerSecond|I/O gelezen bytes per seconde|BytesPerSecond|Totaal|I/O gelezen bytes per seconde|Exemplaar|
|IoWriteBytesPerSecond|I/O geschreven bytes per seconde|BytesPerSecond|Totaal|I/O geschreven bytes per seconde|Exemplaar|
|IoOtherBytesPerSecond|I/O overige bytes per seconde|BytesPerSecond|Totaal|I/O overige bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|I/O gelezen bewerkingen per seconde|BytesPerSecond|Totaal|I/O gelezen bewerkingen per seconde|Exemplaar|
|IoWriteOperationsPerSecond|I/O geschreven bewerkingen per seconde|BytesPerSecond|Totaal|I/O geschreven bewerkingen per seconde|Exemplaar|
|IoOtherOperationsPerSecond|I/O overige bewerkingen per seconde|BytesPerSecond|Totaal|I/O overige bewerkingen per seconde|Exemplaar|
|RequestsInApplicationQueue|Aanvragen in de toepassingswachtrij|Count|Gemiddeld|Aanvragen in de toepassingswachtrij|Exemplaar|
|CurrentAssemblies|Huidige assembly's|Count|Gemiddeld|Huidige assembly's|Exemplaar|
|TotalAppDomains|Totaal aantal appdomeinen|Count|Gemiddeld|Totaal aantal appdomeinen|Exemplaar|
|TotalAppDomainsUnloaded|Totaal aantal appdomeinen verwijderd|Count|Gemiddeld|Totaal aantal appdomeinen verwijderd|Exemplaar|
|Gen0Collections|Garbagecollections van generatie 0|Count|Totaal|Garbagecollections van generatie 0|Exemplaar|
|Gen1Collections|Garbagecollections van generatie 1|Count|Totaal|Garbagecollections van generatie 1|Exemplaar|
|Gen2Collections|Garbagecollections van generatie 2|Count|Totaal|Garbagecollections van generatie 2|Exemplaar|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (functions)

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Gemiddeld|Geheugenwerkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddeld geheugenwerkset|Bytes|Gemiddeld|Gemiddeld geheugenwerkset|Exemplaar|
|FunctionExecutionUnits|Functie-uitvoeringseenheden|Count|Totaal|Functie-uitvoeringseenheden|Exemplaar|
|FunctionExecutionCount|Aantal uitvoeringen van functie|Count|Totaal|Aantal uitvoeringen van functie|Exemplaar|
|PrivateBytes|Privébytes|Bytes|Gemiddeld|Privébytes|Exemplaar|
|IoReadBytesPerSecond|I/O gelezen bytes per seconde|BytesPerSecond|Totaal|I/O gelezen bytes per seconde|Exemplaar|
|IoWriteBytesPerSecond|I/O geschreven bytes per seconde|BytesPerSecond|Totaal|I/O geschreven bytes per seconde|Exemplaar|
|IoOtherBytesPerSecond|I/O overige bytes per seconde|BytesPerSecond|Totaal|I/O overige bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|I/O gelezen bewerkingen per seconde|BytesPerSecond|Totaal|I/O gelezen bewerkingen per seconde|Exemplaar|
|IoWriteOperationsPerSecond|I/O geschreven bewerkingen per seconde|BytesPerSecond|Totaal|I/O geschreven bewerkingen per seconde|Exemplaar|
|IoOtherOperationsPerSecond|I/O overige bewerkingen per seconde|BytesPerSecond|Totaal|I/O overige bewerkingen per seconde|Exemplaar|
|RequestsInApplicationQueue|Aanvragen in de toepassingswachtrij|Count|Gemiddeld|Aanvragen in de toepassingswachtrij|Exemplaar|
|CurrentAssemblies|Huidige assembly's|Count|Gemiddeld|Huidige assembly's|Exemplaar|
|TotalAppDomains|Totaal aantal appdomeinen|Count|Gemiddeld|Totaal aantal appdomeinen|Exemplaar|
|TotalAppDomainsUnloaded|Totaal aantal appdomeinen verwijderd|Count|Gemiddeld|Totaal aantal appdomeinen verwijderd|Exemplaar|
|Gen0Collections|Garbagecollections van generatie 0|Count|Totaal|Garbagecollections van generatie 0|Exemplaar|
|Gen1Collections|Garbagecollections van generatie 1|Count|Totaal|Garbagecollections van generatie 1|Exemplaar|
|Gen2Collections|Garbagecollections van generatie 2|Count|Totaal|Garbagecollections van generatie 2|Exemplaar|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|Exemplaar|
|Aanvragen|Aanvragen|Count|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http101|HTTP-fout 101|Count|Totaal|HTTP-fout 101|Exemplaar|
|Http2xx|HTTP 2xx|Count|Totaal|HTTP 2xx|Exemplaar|
|Http3xx|HTTP 3xx|Count|Totaal|HTTP 3xx|Exemplaar|
|Http401|HTTP 401|Count|Totaal|HTTP 401|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP-fout 404|Count|Totaal|HTTP-fout 404|Exemplaar|
|Http406|HTTP-fout 406|Count|Totaal|HTTP-fout 406|Exemplaar|
|Http4xx|Http 4xx|Count|Totaal|Http 4xx|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|MemoryWorkingSet|Geheugenwerkset|Bytes|Gemiddeld|Geheugenwerkset|Exemplaar|
|AverageMemoryWorkingSet|Gemiddeld geheugenwerkset|Bytes|Gemiddeld|Gemiddeld geheugenwerkset|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddeld|Gemiddelde reactietijd|Exemplaar|
|FunctionExecutionUnits|Functie-uitvoeringseenheden|Count|Totaal|Functie-uitvoeringseenheden|Exemplaar|
|FunctionExecutionCount|Aantal uitvoeringen van functie|Count|Totaal|Aantal uitvoeringen van functie|Exemplaar|
|AppConnections|Verbindingen|Count|Gemiddeld|Verbindingen|Exemplaar|
|Ingangen|Aantal ingangen|Count|Gemiddeld|Aantal ingangen|Exemplaar|
|Threads|Aantal threads|Count|Gemiddeld|Aantal threads|Exemplaar|
|PrivateBytes|Privébytes|Bytes|Gemiddeld|Privébytes|Exemplaar|
|IoReadBytesPerSecond|I/O gelezen bytes per seconde|BytesPerSecond|Totaal|I/O gelezen bytes per seconde|Exemplaar|
|IoWriteBytesPerSecond|I/O geschreven bytes per seconde|BytesPerSecond|Totaal|I/O geschreven bytes per seconde|Exemplaar|
|IoOtherBytesPerSecond|I/O overige bytes per seconde|BytesPerSecond|Totaal|I/O overige bytes per seconde|Exemplaar|
|IoReadOperationsPerSecond|I/O gelezen bewerkingen per seconde|BytesPerSecond|Totaal|I/O gelezen bewerkingen per seconde|Exemplaar|
|IoWriteOperationsPerSecond|I/O geschreven bewerkingen per seconde|BytesPerSecond|Totaal|I/O geschreven bewerkingen per seconde|Exemplaar|
|IoOtherOperationsPerSecond|I/O overige bewerkingen per seconde|BytesPerSecond|Totaal|I/O overige bewerkingen per seconde|Exemplaar|
|RequestsInApplicationQueue|Aanvragen in de toepassingswachtrij|Count|Gemiddeld|Aanvragen in de toepassingswachtrij|Exemplaar|
|CurrentAssemblies|Huidige assembly's|Count|Gemiddeld|Huidige assembly's|Exemplaar|
|TotalAppDomains|Totaal aantal appdomeinen|Count|Gemiddeld|Totaal aantal appdomeinen|Exemplaar|
|TotalAppDomainsUnloaded|Totaal aantal appdomeinen verwijderd|Count|Gemiddeld|Totaal aantal appdomeinen verwijderd|Exemplaar|
|Gen0Collections|Garbagecollections van generatie 0|Count|Totaal|Garbagecollections van generatie 0|Exemplaar|
|Gen1Collections|Garbagecollections van generatie 1|Count|Totaal|Garbagecollections van generatie 1|Exemplaar|
|Gen2Collections|Garbagecollections van generatie 2|Count|Totaal|Garbagecollections van generatie 2|Exemplaar|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|Aanvragen|Aanvragen|Count|Totaal|Aanvragen|Exemplaar|
|BytesReceived|Inkomende gegevens|Bytes|Totaal|Inkomende gegevens|Exemplaar|
|BytesSent|Uitgaande gegevens|Bytes|Totaal|Uitgaande gegevens|Exemplaar|
|Http101|HTTP-fout 101|Count|Totaal|HTTP-fout 101|Exemplaar|
|Http2xx|HTTP 2xx|Count|Totaal|HTTP 2xx|Exemplaar|
|Http3xx|HTTP 3xx|Count|Totaal|HTTP 3xx|Exemplaar|
|Http401|HTTP 401|Count|Totaal|HTTP 401|Exemplaar|
|Http403|HTTP-fout 403|Count|Totaal|HTTP-fout 403|Exemplaar|
|Http404|HTTP-fout 404|Count|Totaal|HTTP-fout 404|Exemplaar|
|Http406|HTTP-fout 406|Count|Totaal|HTTP-fout 406|Exemplaar|
|Http4xx|Http 4xx|Count|Totaal|Http 4xx|Exemplaar|
|Http5xx|HTTP-serverfouten|Count|Totaal|HTTP-serverfouten|Exemplaar|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddeld|Gemiddelde reactietijd|Exemplaar|
|CpuPercentage|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Procent|Gemiddeld|Geheugenpercentage|Exemplaar|
|DiskQueueLength|Lengte van de wachtrij voor de schijf|Count|Gemiddeld|Lengte van de wachtrij voor de schijf|Exemplaar|
|HttpQueueLength|Lengte van de HTTP-wachtrij|Count|Gemiddeld|Lengte van de HTTP-wachtrij|Exemplaar|
|ActiveRequests|Actieve aanvragen|Count|Totaal|Actieve aanvragen|Exemplaar|
|TotalFrontEnds|Totaalaantal front-ends|Count|Gemiddeld|Totaalaantal front-ends|Er zijn geen dimensies|
|SmallAppServicePlanInstances|Werkrollen kleine App Plan Service-plan|Count|Gemiddeld|Werkrollen kleine App Plan Service-plan|Er zijn geen dimensies|
|MediumAppServicePlanInstances|Werkrollen middelgrote App Service-plan|Count|Gemiddeld|Werkrollen middelgrote App Service-plan|Er zijn geen dimensies|
|LargeAppServicePlanInstances|Werkrollen grote App Service-plan|Count|Gemiddeld|Werkrollen grote App Service-plan|Er zijn geen dimensies|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Description|Dimensies|
|---|---|---|---|---|---|
|WorkersTotal|Totaalaantal werkrollen|Count|Gemiddeld|Totaalaantal werkrollen|Er zijn geen dimensies|
|WorkersAvailable|Beschikbare werkrollen|Count|Gemiddeld|Beschikbare werkrollen|Er zijn geen dimensies|
|WorkersUsed|Gebruikte werkrollen|Count|Gemiddeld|Gebruikte werkrollen|Er zijn geen dimensies|
|CpuPercentage|CPU-percentage|Procent|Gemiddeld|CPU-percentage|Exemplaar|
|MemoryPercentage|Geheugenpercentage|Procent|Gemiddeld|Geheugenpercentage|Exemplaar|

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over metrische gegevens in Azure Monitor](../../azure-monitor/platform/data-collection.md)
* [Waarschuwingen over metrische gegevens maken](../../azure-monitor/platform/alerts-overview.md)
* [Metrische gegevens exporteren naar opslag, Event Hub of Log Analytics](../../azure-monitor/platform/diagnostic-logs-overview.md)
