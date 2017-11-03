---
title: Analyseer uw werkbelasting - Azure SQL Data Warehouse | Microsoft Docs
description: Technieken voor het analyseren van de prioriteit van de query voor de werkbelasting in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: 98617f6b8366662e52d00420adc4c81abffc598d
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="analyze-your-workload"></a>Uw werkbelasting analyseren
Technieken voor het analyseren van de prioriteit van de query voor de werkbelasting in Azure SQL Data Warehouse.

## <a name="workload-groups"></a>Werkbelastinggroepen 
SQL Data Warehouse implementeert resource klassen met behulp van werkbelastinggroepen. Er zijn een totaal van acht werkbelastinggroepen die het gedrag van de resource-klassen over de verschillende grootten van DWU controleren. Voor elke DWU SQL Data Warehouse maakt gebruik van vier van de werkbelastinggroepen acht. Dit is wel zinvol omdat elke werkbelastinggroep is toegewezen aan een van de vier resource klassen: smallrc, mediumrc, largerc, of xlargerc. Het belang van het begrijpen van de werkbelastinggroepen is dat sommige van deze werkbelastinggroepen zijn ingesteld tot hogere *belang*. Urgentie wordt gebruikt voor CPU planning. Query's uitvoeren met een hoge urgentie krijgt drie keer meer CPU-cycli dan die met een gemiddeld urgentie. Daarom bepalen gelijktijdigheid sleuf toewijzingen ook voor CPU-prioriteit. Als een query 16 of meer sleuven verbruikt, voert deze als hoge urgentie.

De volgende tabel toont de toewijzingen belang voor elke werkbelastinggroep.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Groepstoewijzingen werkbelasting gelijktijdigheid sleuven en urgentie

| Werkbelastinggroepen | Gelijktijdigheid sleuf toewijzing | MB / distributie (elasticiteit) | MB / distributie (berekenen) | Toewijzing van belang |
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

<!-- where are the allocation and consumption of concurrency slots charts? -->
Van de **toewijzing en het verbruik van gelijktijdigheid sleuven** grafiek, kunt u zien dat een DW500 1, 4, 8 of 16 gelijktijdigheid sleuven voor smallrc, mediumrc largerc en xlargerc, respectievelijk gebruikt. U kunt deze waarden opzoeken in het voorgaande diagram het belang voor elke objectklasse bron vinden.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Toewijzing van klassen op belang resource DW500
| Bronklasse | Werkbelastinggroep | Gelijktijdigheid sleuven gebruikt | MB / distributie | Urgentie |
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
U kunt de volgende DMV-query om te kijken naar de verschillen in de geheugenbrontoewijzing in detail vanuit het perspectief van de resourceregeling of voor het analyseren van actieve en historische informatie over het gebruik van de werkbelastinggroepen bij het oplossen.

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
U kunt de `sys.dm_pdw_exec_requests` DMV voor het identificeren van query's die in een wachtrij gelijktijdigheid wachten. Query's te wachten op een site gelijktijdigheid van taken heeft een status van **onderbroken**.

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

Werkbelasting management-rollen kunnen worden bekeken met `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

De volgende query ziet u welke rol u elke gebruiker is toegewezen.

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

SQL Data Warehouse heeft de volgende typen wachten:

* **LocalQueriesConcurrencyResourceType**: query's die zich buiten het kader van de sleuf gelijktijdigheid van taken. Functies, zoals DMV-query's en system `SELECT @@VERSION` zijn voorbeelden van lokale query's.
* **UserConcurrencyResourceType**: query's die zich in het kader van de sleuf gelijktijdigheid van taken bevinden. Query's op tabellen van de eindgebruiker vertegenwoordigen voorbeelden die dit brontype wilt gebruiken.
* **DmsConcurrencyResourceType**: wacht ten gevolge van data movement-bewerkingen.
* **BackupConcurrencyResourceType**: deze wachttijd geeft aan dat een database worden back-up. De maximale waarde voor dit resourcetype is 1. Als meerdere back-ups op hetzelfde moment hebt aangevraagd, wordt de andere wachtrij.

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

De `sys.dm_pdw_resource_waits` DMV toont alleen de resource-wacht dat wordt gebruikt door een bepaalde query. Wachttijd resource wordt alleen de tijd die wachten op resources worden opgegeven, in plaats van de wachttijd signaal, is de tijd die nodig is voor de onderliggende SQL servers plannen van de query naar de CPU.

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
U kunt ook de `sys.dm_pdw_resource_waits` DMV berekenen hoeveel gelijktijdigheid sleuven hebben gekregen.

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots 
FROM    sys.[dm_pdw_resource_waits] 
WHERE   [state]           = 'Granted' 
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

De `sys.dm_pdw_wait_stats` DMV kan worden gebruikt voor analyse van de historische trend van wacht.

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
Zie voor meer informatie over het beheren van databasegebruikers en beveiliging [beveiligen van een database in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Zie voor meer informatie over hoe groter resource klassen kan worden verbeterd geclusterde columnstore-index kwaliteit [opnieuw opbouwen van indexen voor het segment verbeteren].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[opnieuw opbouwen van indexen voor het segment verbeteren]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
