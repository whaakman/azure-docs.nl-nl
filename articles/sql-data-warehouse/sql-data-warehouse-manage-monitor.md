---
title: Uw workload controleren met DMV's | Microsoft Docs
description: Leer hoe u uw workload controleren met DMV's.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/18/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: e2360b5587d204ec87fe82c029391c7252d27914
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189543"
---
# <a name="monitor-your-workload-using-dmvs"></a>Monitor your workload using DMVs
Dit artikel wordt beschreven hoe u uw workload controleren met dynamische beheerweergaven (DMV's). Dit omvat het onderzoeken van de uitvoering van de query in Azure SQL Data Warehouse.

## <a name="permissions"></a>Machtigingen
Om te vragen de DMV's in dit artikel, moet u de machtiging VIEW DATABASE STATE of BESTURINGSELEMENT. Verlenen VIEW DATABASE STATE is meestal de voorkeur machtiging omdat deze nog veel meer beperkende.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Monitor-verbindingen
Alle aanmeldingen met SQL Data Warehouse worden geregistreerd in [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  Deze DMV bevat de laatste 10.000 aanmeldingen.  Het type session_id is de primaire sleutel en sequentieel worden verwerkt voor elke nieuwe aanmelding is toegewezen.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Query uitvoeren van de monitor
Alle query's uitgevoerd op SQL Data Warehouse worden geregistreerd in [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  Deze DMV bevat de laatste 10.000 query's uitgevoerd.  De request_id uniek identificeert elke query en de primaire sleutel voor deze DMV.  De request_id sequentieel is toegewezen voor elke nieuwe query en wordt voorafgegaan door QID voor de query-ID staat.  Uitvoeren van query's deze DMV voor een bepaald type session_id ziet u alle query's voor een bepaalde aanmelding.

> [!NOTE]
> Opgeslagen procedures meerdere aanvraag-id's gebruiken.  Aanvraag-id's zijn toegewezen in opeenvolgende volgorde. 
> 
> 

Hier volgen de stappen te volgen voor het onderzoeken van query-uitvoering plannen en tijden voor een bepaalde query.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>STAP 1: Identificeren van de query die u wilt onderzoeken
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

In de voorgaande queryresultaten, **Houd er rekening mee de aanvraag-ID** van de query die u wilt onderzoeken.

Query's in de **onderbroken** staat in de wachtrij vanwege een groot aantal actieve query's kan worden geplaatst. Deze query's worden ook weergegeven in de [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql) wachten op query's uitvoeren met een type UserConcurrencyResourceType. Zie voor meer informatie over gelijktijdigheidslimieten [prestatielagen](performance-tiers.md) of [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md). Query's kunnen ook wachten om andere redenen, zoals voor het object wordt vergrendeld.  Als uw query tot een resource wacht, Zie [onderzoeken van query's die wachten op resources] [ Investigating queries waiting for resources] verderop in dit artikel.

Voor het vereenvoudigen van het opzoeken van een query in de [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) tabel, gebruikt u [LABEL] [ LABEL] om toe te wijzen een opmerking aan de query die kan worden opgezocht in de sys.dm_pdw_exec_ aanvragen weergeven.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>STAP 2: Het queryplan onderzoeken
De aanvraag-ID gebruiken om op te halen van de query gedistribueerde SQL (DSQL) het plan is uit [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Wanneer u een plan DSQL duurt langer dan verwacht, kan de oorzaak een complexe planning met veel DSQL stappen of slechts één stap duurt lang zijn.  Als het plan veel stappen met verschillende verplaatsingsbewerkingen voor het is, kunt u overwegen uw distributies tabel om te beperken van gegevensverplaatsing te optimaliseren. De [tabeldistributie] [ Table distribution] artikel wordt uitgelegd waarom de gegevens voor het oplossen van een query moet worden verplaatst en beschrijft enkele distributiestrategieën om te beperken van gegevensverplaatsing.

Voor meer informatie over één stap, voor verder onderzoek de *operation_type* kolom van de langlopende querystap en noteer de **Stapindex**:

* Doorgaan met stap 3a voor **SQL-bewerkingen**: OnOperation, RemoteOperation, ReturnOperation.
* Doorgaan met stap 3b voor **gegevensverplaatsing operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>STAP 3a: SQL onderzoeken voor de gedistribueerde databases
Gebruik van de aanvraag-ID en de Index van de stap voor het ophalen van de details van [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], bevat informatie van de uitvoering van de querystap op alle van de gedistribueerde databases.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Wanneer de querystap van de wordt uitgevoerd, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan worden gebruikt om op te halen van de geschatte plan van SQL Server uit de cache van de SQL Server-abonnement voor de stap die wordt uitgevoerd op een bepaalde distributie.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>STAP 3b: Gegevensverplaatsing voor de gedistribueerde databases onderzoeken
Gebruik van de aanvraag-ID en de Index van de stap voor het ophalen van informatie over een data movement stap die wordt uitgevoerd op elke distributie van [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Controleer de *total_elapsed_time* kolom om te zien als een bepaalde distributie aanzienlijk langer dan de andere voor verplaatsing van gegevens duurt.
* Voor de distributie van langlopende, Controleer de *rows_processed* kolom om te zien of het aantal rijen wordt verplaatst van dit distributiepunt aanzienlijk groter dan andere. Als dit het geval is, kunt u deze zoeken achterhalen scheeftrekken van de onderliggende gegevens.

Als de query wordt uitgevoerd, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan worden gebruikt om op te halen van de geschatte plan van SQL Server uit de cache van SQL Server-abonnement voor de momenteel actieve SQL-stap binnen een bepaald distributie.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>Monitor wachtrij-query 's
Als u ontdekt dat de query niet wordt uitgevoerd is beschikbaar omdat er wordt gewacht tot een resource, is hier een query waarin alle resources dat een query wordt gewacht.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Als de query actief op de resources van een andere query wacht is, wordt de status is **AcquireResources**.  Als de query alle vereiste resources heeft, wordt de status is **verleend**.

## <a name="monitor-tempdb"></a>Monitor voor tempdb
Hoge tempdb-gebruik, kan de hoofdoorzaak van trage prestaties en naar geheugenproblemen zijn. Houd rekening met uw datawarehouse schalen als u tempdb uiterste te belasten bereiken tijdens het uitvoeren van query's kunt vinden. De volgende informatie wordt beschreven hoe u tempdb-gebruik per query op elk knooppunt te identificeren. 

De volgende weergave om te koppelen van de juiste knooppunt-ID voor sys.dm_pdw_sql_requests maken. Met het knooppunt-ID, kunt u andere Pass Through-DMV's gebruiken en deelnemen aan deze tabellen met sys.dm_pdw_sql_requests.

```sql
-- sys.dm_pdw_sql_requests with the correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
Voer de volgende query uit voor het controleren van tempdb:

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a>Memory bewaken

Geheugen kan de hoofdoorzaak van trage prestaties en naar geheugenproblemen zijn. Houd rekening met uw datawarehouse schalen als u SQL Server-geheugengebruik uiterste te belasten bereiken tijdens het uitvoeren van query's kunt vinden.

De volgende query retourneert SQL Server-gebruik en het geheugen geheugendruk per knooppunt:   
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a>De grootte van de transactie-logboekbestand bewaken
De volgende query retourneert de grootte van het transactielogboek op elk distributiepunt. Als een van de logboekbestanden van 160 GB bereikt, moet u rekening houden met uw exemplaar omhoog of de grootte van de transactie te beperken. 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
```
## <a name="monitor-transaction-log-rollback"></a>Logboekbestand terugdraaien van de transactie controleren
Als uw query's zijn mislukt of lang duurt om door te gaan, kunt u controleren en bewaken, hebt u alle transacties terug te draaien.
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over DMV's [systeemweergaven][System views].


<!--Image references-->

<!--Article references-->
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: https://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
