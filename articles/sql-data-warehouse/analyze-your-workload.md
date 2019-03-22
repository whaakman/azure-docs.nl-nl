---
title: Analyseren van uw workload - Azure SQL Data Warehouse | Microsoft Docs
description: Technieken voor het analyseren van de prioriteit van de query voor uw workload in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 434cbb18a109308844dbc7ff219d40948678e86e
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58310724"
---
# <a name="analyze-your-workload-in-azure-sql-data-warehouse"></a>Analyseren van uw workload in Azure SQL Data Warehouse

Technieken voor het analyseren van uw workload in Azure SQL Data Warehouse.

## <a name="resource-classes"></a>Resourceklassen

SQL Data Warehouse biedt resourceklassen om toe te wijzen van systeembronnen op query's.  Zie voor meer informatie over resourceklassen [resourceklassen en werklastbeheer resourcebeheer](resource-classes-for-workload-management.md).  Query's wordt gewacht tot als de resourceklasse toegewezen aan een query meer bronnen moet dan die momenteel beschikbaar zijn.

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
* **BackupConcurrencyResourceType**: Deze wacht geeft aan dat een database back-up. De maximale waarde voor dit resourcetype is 1. Als meerdere back-ups hebt aangevraagd op hetzelfde moment, de andere wachtrij. In het algemeen wordt aangeraden een minimale tijd tussen opeenvolgende momentopnamen van 10 minuten. 

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

De `sys.dm_pdw_resource_waits` DMV toont de informatie van de wachttijd voor een bepaalde query. Resource wachttijd tijdmetingen het wachten op resources worden opgegeven. Signaal wachttijd is de tijd die nodig zijn voor de onderliggende SQL servers voor het plannen van de query naar de CPU.

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
