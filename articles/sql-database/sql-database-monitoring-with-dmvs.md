---
title: Bewaking van Azure SQL Database met dynamische beheerweergaven | Microsoft Docs
description: Informatie over het detecteren en onderzoeken van veelvoorkomende prestatieproblemen met behulp van dynamische beheerweergaven voor het bewaken van Microsoft Azure SQL Database.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: d08f505f-3c62-47d4-bab7-35c9a834b79b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 298adcad78163e82a926abb684a172d56b4a33dd
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Bewaking van Azure SQL-database met behulp van de dynamische beheerweergave
Microsoft Azure SQL Database kunt een subset van de dynamische Beheerweergave weergaven voor het onderzoeken van prestatieproblemen, wordt door geblokkeerde of langdurige query's, resourceknelpunten en slechte queryplannen veroorzaakt mogelijk. Dit onderwerp bevat informatie over het detecteren van veelvoorkomende prestatieproblemen met behulp van dynamische beheerweergaven.

SQL Database ondersteunt gedeeltelijk drie categorieën van dynamische beheerweergaven:

* Database-gerelateerde dynamische beheerweergaven.
* Uitvoering-gerelateerde dynamische beheerweergaven.
* Transactie-gerelateerde dynamische beheerweergaven.

Zie voor gedetailleerde informatie over dynamische beheerweergaven [functies (Transact-SQL) en dynamische beheerweergaven](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.

## <a name="permissions"></a>Machtigingen
In SQL-Database vereist voor het opvragen van een dynamische Beheerweergave **DATABASE WEERGAVESTATUS** machtigingen. De **DATABASE WEERGAVESTATUS** machtiging retourneert informatie over alle objecten in de huidige database.
Toe te kennen de **DATABASE WEERGAVESTATUS** machtiging aan een gebruiker specifieke database, voer de volgende query:

```GRANT VIEW DATABASE STATE TO database_user; ```

In een lokale SQL Server-exemplaar retourneren dynamische beheerweergaven informatie over de status van de server. Deze retourneren informatie met betrekking tot alleen de huidige logische database in SQL-Database.

## <a name="calculating-database-size"></a>Berekenen van de grootte van de database
De volgende query retourneert de grootte van de database (in megabytes):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

De volgende query retourneert de grootte van afzonderlijke objecten (in MB) in de database:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Controleren van verbindingen
U kunt de [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) weergave informatie ophalen over de verbindingen met een specifieke Azure SQL Database-server en de details van elke verbinding. Bovendien de [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) weergave is handig bij het ophalen van informatie over alle actieve gebruikersverbindingen en interne taken.
De volgende query haalt informatie over de huidige verbinding:

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
> Bij het uitvoeren van de **sys.dm_exec_requests** en **sys.dm_exec_sessions weergaven**, hebt u **DATABASE WEERGAVESTATUS** machtiging op de database u ziet alle uitvoeren sessies op de database. anders ziet u alleen de huidige sessie.
> 
> 

## <a name="monitoring-query-performance"></a>Queryprestaties bewaken
Traag of lang u query's uitvoert, kan dit aanzienlijke systeembronnen gebruiken. Deze sectie wordt gedemonstreerd hoe u met dynamische beheerweergaven enkele algemene query prestatieproblemen detecteren. Een verwijzing naar een oudere, maar nog steeds handig voor het oplossen van problemen, is de [het oplossen van prestatieproblemen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) artikel op Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Top N-query's te zoeken
Het volgende voorbeeld retourneert informatie over de bovenste vijf query basis van het gemiddelde CPU-tijd. In dit voorbeeld maakt een aggregatie van de query's volgens de query-hash, zodat logisch gelijkwaardige query's zijn gegroepeerd op hun cumulatieve resourceverbruik.

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

### <a name="monitoring-blocked-queries"></a>Bewaking van geblokkeerde query 's
Trage of langdurige query's kunnen bijdragen tot overmatige brongebruik en het gevolg was van geblokkeerde query's. De oorzaak van de blokkering kan worden toepassingen die niet goed ontwerp, onjuiste queryplannen, het gebrek aan nuttig indexen, enzovoort. Voor informatie over de huidige activiteit van de vergrendeling in uw Azure SQL Database kunt u de weergave sys.dm_tran_locks. Bijvoorbeeld code, Zie [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.

### <a name="monitoring-query-plans"></a>Bewaking van queryplannen
Een inefficiënte queryplan kan ook CPU-verbruik verhogen. Het volgende voorbeeld wordt de [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) weergeven om te bepalen welke query gebruikt de meest cumulatieve CPU.

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

