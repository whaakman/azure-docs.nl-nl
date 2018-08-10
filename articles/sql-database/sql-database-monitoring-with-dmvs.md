---
title: Azure SQL Database controleren met Dynamic Management Views | Microsoft Docs
description: Informatie over het detecteren en onderzoeken van problemen met de algemene prestaties met behulp van dynamische beheerweergaven voor het bewaken van Microsoft Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: carlrab
ms.openlocfilehash: c4d1170bd2fe4acb135c88191b447f734e312723
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715954"
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Bewaking van Azure SQL-database met behulp van de dynamische beheerweergave
Microsoft Azure SQL Database kunt een subset van de dynamische beheerweergaven voor het vaststellen van prestatieproblemen, die kunnen worden veroorzaakt door geblokkeerd of langlopende query's, resourceknelpunten en slechte queryplannen. In dit onderwerp bevat informatie over het vaststellen van problemen met de algemene prestaties met behulp van dynamische beheerweergaven.

SQL Database ondersteunt gedeeltelijk drie categorieën van dynamische beheerweergaven:

* Database-gerelateerde dynamische beheerweergaven.
* Met betrekking tot uitvoering van dynamische beheerweergaven.
* Transactie-gerelateerde dynamische beheerweergaven.

Zie voor gedetailleerde informatie over dynamische beheerweergaven [functies (Transact-SQL) en dynamische beheerweergaven](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.

## <a name="permissions"></a>Machtigingen
In SQL-Database, uitvoeren van query's een dynamische Beheerweergave vereist **VIEW DATABASE STATE** machtigingen. De **VIEW DATABASE STATE** machtiging retourneert informatie over alle objecten in de huidige database.
Om te verlenen de **VIEW DATABASE STATE** machtiging voor een specifieke databasegebruiker, voer de volgende query uit:

```GRANT VIEW DATABASE STATE TO database_user; ```

Dynamische beheerweergaven retourneren in een on-premises SQL Server-exemplaar, informatie over de status van de server. In SQL-Database retourneert deze informatie met betrekking tot alleen de huidige logische database.

## <a name="calculating-database-size"></a>Berekenen van de grootte van de database
De volgende query retourneert de grootte van uw database (in MB):

```
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

De volgende query retourneert de grootte van afzonderlijke objecten (in MB) in uw database:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Verbindingen controleren
U kunt de [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) weergave informatie ophalen over de verbindingen met een specifieke Azure SQL Database-server en de details van elke verbinding. Bovendien de [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) weergave is handig bij het ophalen van informatie over alle actieve gebruikersverbindingen en interne taken.
De volgende query haalt informatie op de huidige verbinding:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> Bij het uitvoeren van de **sys.dm_exec_requests** en **sys.dm_exec_sessions weergaven**, hebt u **VIEW DATABASE STATE** machtiging op de database, ziet u alle uitvoeren sessies op de database. anders is, ziet u alleen de huidige sessie.
> 
> 

## <a name="monitoring-query-performance"></a>Bewaking van prestaties van query 's
Trage of lang uitvoeren van query's kan aanzienlijke systeembronnen gebruiken. In deze sectie ziet u hoe u dynamische beheerweergaven gebruiken voor het detecteren van enkele veelvoorkomende problemen met de queryprestaties. De verwijzing naar een oudere, maar wel nog steeds nuttig voor het oplossen van problemen, is de [problemen met prestaties oplossen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) artikel op Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Top N-query's zoeken
Het volgende voorbeeld retourneert informatie over de top vijf van query's gerangschikt op gemiddelde CPU-tijd. In dit voorbeeld maakt een aggregatie van de query's op basis van de queryhash, zodat logisch gelijkwaardige query's zijn gegroepeerd op hun cumulatieve resourceverbruik.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Geblokkeerde query's controleren
Trage of langlopende query's kunnen bijdragen tot overmatig resourceverbruik en de gevolgen van geblokkeerde query's. De oorzaak van de blokkering is slecht presteert ontwerp, ongeldige queryplannen, het ontbreken van nuttige indexen, enzovoort. U kunt de weergave sys.dm_tran_locks gebruiken voor informatie over de huidige activiteit van de vergrendeling in uw Azure SQL Database. Bijvoorbeeld code, Zie [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.

### <a name="monitoring-query-plans"></a>Bewaking van de query-plannen
Een inefficiënte queryplan kan worden verhoogd CPU-verbruik. Het volgende voorbeeld wordt de [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) weergeven om te bepalen welke query het meest cumulatieve CPU gebruikt.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Zie ook
[Inleiding tot SQL Database](sql-database-technical-overview.md)

