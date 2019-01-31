---
title: Analyseren van uw workload - Azure SQL Data Warehouse | Microsoft Docs
description: Technieken voor het analyseren van de prioriteit van de query voor uw workload in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9025eccabcbf7052131fee741a1e1f6a2139366b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55476754"
---
# <a name="analyze-your-workload-in-azure-sql-data-warehouse"></a>Analyseren van uw workload in Azure SQL Data Warehouse
Technieken voor het analyseren van de prioriteit van de query voor uw workload in Azure SQL Data Warehouse.

## <a name="workload-groups"></a>Werkbelastinggroepen 
Resourceklassen implementeert SQL Data Warehouse met behulp van werkbelastinggroepen. Er zijn een totaal van acht werkbelastinggroepen die het gedrag van de resourceklassen via de verschillende DWU-grootten beheren. SQL Data Warehouse gebruikt voor een DWU slechts vier van de acht werkbelastinggroepen. Deze benadering is logisch, want elke werkbelastinggroep is toegewezen aan een van de vier resourceklassen: smallrc, mediumrc, largerc, of xlargerc. Het belang van informatie over de werkbelastinggroepen is dat sommige van deze werkbelastinggroepen zijn ingesteld op hoger *belang*. Prioriteit wordt gebruikt voor CPU plannen. Query's uitvoeren met hoge urgentie krijg drie keer meer CPU-cycli dan query's uitvoeren met gemiddelde urgentie. Gelijktijdigheid van taken sleuf toewijzingen bepalen daarom ook CPU-prioriteit. Wanneer een query 16 of meer sleuven verbruikt, deze wordt uitgevoerd als hoge urgentie.

De volgende tabel toont de toewijzingen belang voor elke werkbelastinggroep.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Groepstoewijzingen werkbelasting gelijktijdigheidssleuven en urgentie

| Werkbelastinggroepen | Gelijktijdigheid van taken sleuf toewijzing | MB / distributie (elasticiteit) | MB / distributie (rekenen) | Toewijzing van urgentie |
|:---------------:|:------------------------:|:------------------------------:|:---------------------------:|:------------------:|
| SloDWGroupC00   | 1                        |    100                         | 250                         | Middelgroot             |
| SloDWGroupC01   | 2                        |    200                         | 500                         | Middelgroot             |
| SloDWGroupC02   | 4                        |    400                         | 1000                        | Middelgroot             |
| SloDWGroupC03   | 8                        |    800                         | 2000                        | Middelgroot             |
| SloDWGroupC04   | 16                       |  1,600                         | 4000                        | Hoog               |
| SloDWGroupC05   | 32                       |  3,200                         | 8000                        | Hoog               |
| SloDWGroupC06   | 64                       |  6,400                         | 16,000                      | Hoog               |
| SloDWGroupC07   | 128                      | 12,800                         | 32,000                      | Hoog               |
| SloDWGroupC08   | 256                      | 25,600                         | 64,000                      | Hoog               |

<!-- where are the allocation and consumption of concurrency slots charts? --> De **toewijzing en het verbruik van gelijktijdigheidssleuven** grafiek toont een DW500 maakt gebruik van 1, 4, 8 of 16 gelijktijdigheidssleuven voor smallrc, mediumrc of largerc of xlargerc, respectievelijk. Als u het belang voor elke objectklasse resource zoekt, kunt u deze waarden in de voorgaande grafiek opzoeken.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Toewijzing van resourceklassen belang DW500
| Resourceklasse | Werkbelastinggroep | Gelijktijdigheidssleuven gebruikt | MB / distributie | Urgentie |
|:-------------- |:-------------- |:----------------------:|:-----------------:|:---------- |
| smallrc        | SloDWGroupC00  | 1                      | 100               | Middelgroot     |
| mediumrc       | SloDWGroupC02  | 4                      | 400               | Middelgroot     |
| largerc        | SloDWGroupC03  | 8                      | 800               | Middelgroot     |
| xlargerc       | SloDWGroupC04  | 16                     | 1,600             | Hoog       |
| staticrc10     | SloDWGroupC00  | 1                      | 100               | Middelgroot     |
| staticrc20     | SloDWGroupC01  | 2                      | 200               | Middelgroot     |
| staticrc30     | SloDWGroupC02  | 4                      | 400               | Middelgroot     |
| staticrc40     | SloDWGroupC03  | 8                      | 800               | Middelgroot     |
| staticrc50     | SloDWGroupC03  | 16                     | 1,600             | Hoog       |
| staticrc60     | SloDWGroupC03  | 16                     | 1,600             | Hoog       |
| staticrc70     | SloDWGroupC03  | 16                     | 1,600             | Hoog       |
| staticrc80     | SloDWGroupC03  | 16                     | 1,600             | Hoog       |

## <a name="view-workload-groups"></a>Groepen van de werkbelasting weergeven
De volgende query geeft details weer van de geheugentoewijzing van de resource vanuit het perspectief van de resourceregeling. Dit is handig voor het analyseren van actieve en historische gebruik van de werkbelastinggroepen bij het oplossen van problemen.

```sql
WITH rg
AS
(   SELECT  
     pn.name                                AS node_name
    ,pn.[type]                              AS node_type
    ,pn.pdw_node_id                         AS node_id
    ,rp.name                                AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                                AS group_name
    ,wg.importance                          AS group_importance
    ,wg.request_max_memory_grant_percent    AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                             AS group_max_dop
    ,wg.effective_max_dop                   AS group_effective_max_dop
    ,wg.total_request_count                 AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
    AND     rp.name    = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
        node_name
,       group_request_max_memory_grant_pcnt
,       group_importance
;
```

## <a name="queued-query-detection-and-other-dmvs"></a>In de wachtrij query detectie- en andere DMV 's
U kunt de `sys.dm_pdw_exec_requests` DMV voor het identificeren van query's die te in een wachtrij gelijktijdigheid vinden zijn. Query's die wachten op een gelijktijdigheid van taken sleuf hebben een status van **onderbroken**.

```sql
SELECT  r.[request_id]                           AS Request_ID
,       r.[status]                               AS Request_Status
,       r.[submit_time]                          AS Request_SubmitTime
,       r.[start_time]                           AS Request_StartTime
,       DATEDIFF(ms,[submit_time],[start_time])  AS Request_InitiateDuration_ms
,       r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r
;
```

Werklast-beheerrollen kunnen worden bekeken met `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

De volgende query laat zien welke rol van elke gebruiker is toegewezen aan.

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

SQL Data Warehouse heeft de volgende typen wacht:

* **LocalQueriesConcurrencyResourceType**: Query's die zich buiten het kader van gelijktijdigheid van taken sleuf. DMV-query's en systeem functies, zoals `SELECT @@VERSION` zijn voorbeelden van lokale query's.
* **UserConcurrencyResourceType**: Query's die zich in het kader van gelijktijdigheid van taken sleuf bevinden. Query's op tabellen van de eindgebruiker vertegenwoordigen voorbeelden die dit brontype zou gebruiken.
* **DmsConcurrencyResourceType**: Wachten op het resultaat zijn van bewerkingen voor gegevensverplaatsing.
* **BackupConcurrencyResourceType**: Deze wacht geeft aan dat een database back-up. De maximale waarde voor dit resourcetype is 1. Als meerdere back-ups hebt aangevraagd op hetzelfde moment, de andere wachtrij.

De `sys.dm_pdw_waits` DMV kan worden gebruikt om te zien welke bronnen op een aanvraag wacht.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]                                           AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,       SESSION_ID()                                       AS Current_session
,       s.[status]                                         AS Session_status
,       s.[login_name]
,       s.[query_count]
,       s.[client_id]
,       s.[sql_spid]
,       r.[command]                                        AS Request_command
,       r.[label]
,       r.[status]                                         AS Request_status
,       r.[submit_time]
,       r.[start_time]
,       r.[end_compile_time]
,       r.[end_time]
,       DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,       DATEDIFF(ms,r.[start_time],r.[end_compile_time])   AS Request_compile_time_ms
,       DATEDIFF(ms,r.[end_compile_time],r.[end_time])     AS Request_execution_time_ms
,       r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID()
;
```

De `sys.dm_pdw_resource_waits` DMV toont alleen de resource-wacht dat wordt gebruikt door een bepaalde query. Wachttijd van de resource wordt alleen de tijd die wachten op resources worden verstrekt, in plaats van de wachttijd signaal, dit de tijd die nodig zijn voor de onderliggende SQL servers is voor het plannen van de query naar de CPU.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID()
;
```
U kunt ook de `sys.dm_pdw_resource_waits` DMV berekenen hoe veel gelijktijdigheidssleuven in beslag hebben gekregen.

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots 
FROM    sys.[dm_pdw_resource_waits] 
WHERE   [state]           = 'Granted' 
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

De `sys.dm_pdw_wait_stats` DMV kan worden gebruikt voor trendanalyse van historische van wacht.

```sql
SELECT   w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w
;
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van databasegebruikers en beveiliging [beveiligen van een database in SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md). Zie voor meer informatie over hoe grotere resourceklassen geclusterde columnstore-index kwaliteit verbeteren kan, [opnieuw opbouwen van indexen voor het verbeteren van segmentkwaliteit](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality).


