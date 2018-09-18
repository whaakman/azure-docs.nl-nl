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
ms.openlocfilehash: 8750670f2acc41cd712254ba11b4d2ec20aa58aa
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981833"
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

## <a name="monitor-resource-use"></a>Resource-gebruik bewaken

U kunt controleren resource gebruik [SQL Database Query Performance Insight](sql-database-query-performance.md) en [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

U kunt ook gebruik van deze twee weergaven met bewaken:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
U kunt de [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) weergeven in elke SQL-database. De **sys.dm_db_resource_stats** weergave toont recente gegevens in de resource gebruiken ten opzichte van de service tier. Gemiddelde percentages voor CPU, gegevens-i/o-logboek geschreven en geheugen elke 15 seconden worden vastgelegd en 1 uur worden bewaard.

Omdat deze weergave een meer gedetailleerd blik op gebruik van bronnen biedt, gebruikt u **sys.dm_db_resource_stats** eerste voor de analyse van de huidige status of het oplossen van problemen. Deze query geeft bijvoorbeeld het gebruik van gemiddelde en maximale resource voor de huidige database in het afgelopen uur:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data IO in percent',
        MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Voor andere query's, Zie de voorbeelden in [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats
De [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) weergeven in de **master** database bevat extra informatie waarmee u de prestaties van uw SQL-database op de specifieke servicelaag controleren en compute-grootte. De gegevens worden verzameld om de 5 minuten en is ongeveer 14 dagen bewaard. In deze weergave is handig voor een langere historische analyse van hoe uw SQL-database maakt gebruik van resources.

In het volgende diagram ziet u het CPU-resource gebruik voor een Premium-database met de grootte van de compute P2 voor elk uur in een week. Deze grafiek begint op een maandag, ziet u 5 werkdagen en geeft vervolgens een weekend, als er veel minder wordt uitgevoerd op de toepassing.

![SQL database resource gebruiken](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

De gegevens, heeft deze database momenteel een piek-CPU-belasting van iets meer dan 50% CPU-gebruik ten opzichte van de P2 compute grootte (twaalf uur 's middags op dinsdag). Als CPU de dominante factor in de resource-profiel van de toepassing is, kunt u vervolgens besluit dat P2 de grootte van de juiste compute om te waarborgen is dat de werkbelasting altijd past. Als u verwacht een toepassing na verloop van tijd toeneemt dat, is het een goed idee om een extra bron buffer zodat de toepassing niet steeds de prestatieniveau-limiet is bereikt. Als u de grootte van de rekencapaciteit vergroten, kunt u helpen te voorkomen dat zichtbaar is voor een klant fouten die optreden wanneer een database beschikt niet over voldoende kracht voor het verwerken van aanvragen effectief, met name in omgevingen met latentiegevoelige. Een voorbeeld is een database die ondersteuning biedt voor een toepassing die u webpagina's op basis van de resultaten van de database-aanroepen tekent.

Andere soorten toepassingen mogelijk dezelfde grafiek anders worden geïnterpreteerd. Bijvoorbeeld, als een toepassing probeert te verwerken gegevens van elke dag en dezelfde grafiek, dit soort 'batch-taak' model kan van pas komen goed bij een P1 compute-grootte. De grootte van de compute P1 heeft 100 dtu's in vergelijking tot 200 dtu's op de P2 compute-grootte. De grootte van de compute P1 biedt dat de helft van de prestaties van de P2 compute grootte. Dus, 50 procent van de CPU-gebruik in P2 is gelijk aan 100 procent CPU-gebruik van P1. Als de toepassing heeft geen time-outs, maakt het niet uit als een taak duurt 2 uur of 2,5 uren in beslag, als het vandaag nog wordt uitgevoerd. Een toepassing in deze categorie kunt waarschijnlijk een grootte van de compute P1 gebruiken. U kunt profiteren van het feit dat er perioden gedurende de dag wanneer Resourcegebruik lager, is zodat eventuele 'big piek' kan worden gelekt via in een van de holten later op de dag. De grootte van de compute P1 kan zijn goed voor die van toepassing (en geld besparen) van het type, zolang de taken op elke dag tijd voltooien kunnen.

Azure SQL Database beschikbaar stelt verbruikt resource-informatie voor elke actieve database in de **sys.resource_stats** weergave van de **master** database op elke server. De gegevens in de tabel wordt samengevoegd voor 5 minuten durende intervallen. De Servicelagen Basic, Standard en Premium, kan de gegevens worden weergegeven in de tabel, zodat deze gegevens nog meer nuttige voor historische analyse in plaats van in de buurt van de realtime-analyse is meer dan vijf minuten maken. Query de **sys.resource_stats** weergeven om te zien van de recente geschiedenis van een database en om te valideren of de reservering die u kiest de prestaties die u wilt dat wanneer dat nodig is geleverd.

> [!NOTE]
> U moet zijn verbonden met de **master** database van de logische SQL database-server op query **sys.resource_stats** in de volgende voorbeelden.
> 
> 

Dit voorbeeld ziet u hoe de gegevens in deze weergave wordt weergegeven:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![De catalogusweergave sys.resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

Het volgende voorbeeld ziet u verschillende manieren waarop u kunt de **sys.resource_stats** catalogusweergave voor informatie over hoe uw SQL-database maakt gebruik van resources:

1. Om te kijken naar de afgelopen week resource voor de database-userdb1 gebruiken, kunt u deze query uitvoeren:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. Om te evalueren hoe goed uw werkbelasting geschikt is voor de compute-grootte, die u wilt inzoomen op elk aspect van de metrische gegevens voor resources: CPU, leesbewerkingen, schrijfbewerkingen, aantal werkrollen en sessies. Hier volgt een gewijzigde query's uitvoeren met behulp van **sys.resource_stats** voor het rapporteren van de gemiddelde en maximale waarden van deze metrische gegevens voor resources:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Met deze informatie over de gemiddelde en maximale waarden van elke metrisch resourcegegeven, kunt u beoordelen hoe goed uw werkbelasting geschikt is voor de compute-grootte die u hebt gekozen. Normaal gesproken gemiddelde waarden van **sys.resource_stats** bieden u een goede basis te gebruiken op basis van de doelgrootte. Uw primaire meting stick dient te zijn. Voor een voorbeeld mogelijk gebruikt u de servicelaag Standard S2 compute-grootte. Het gemiddelde gebruik percentages voor CPU- en i/o-leesbewerkingen, schrijfbewerkingen zijn dan 40 procent, het gemiddelde aantal werknemers lager is dan 50 en het gemiddelde aantal sessies lager is dan 200. Uw workload passen mogelijk in de S1-compute-grootte. Het is gemakkelijk om te zien of uw database in de sessie en werknemer limieten past. Het DTU-nummer van de laagste grootte wijzigen van het DTU-nummer van de grootte van uw huidige compute compute en vervolgens het resultaat vermenigvuldigd met 100 om te zien of een database past de grootte van een lagere compute met betrekking tot CPU-, lees- en schrijfbewerkingen, delen:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    Het resultaat is de relatieve prestatieverschil tussen de twee compute-grootten die in een percentage. Als uw gebruik van de resource niet deze hoeveelheid overschrijdt, kan uw workload passen in de onderste compute-grootte. U moet echter te kijken naar alle bereikwaarden van waarden voor het gebruik van resources, en te bepalen, op percentage, hoe vaak de databaseworkload zou passen in de onderste compute-grootte. De volgende query levert het percentage passend per resourcedimensie, op basis van de drempelwaarde van 40 procent die in dit voorbeeld is berekend:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Op basis van uw database-servicelaag, kunt u bepalen of uw workload de grootte van de lagere compute past. Als het doel van de werkbelasting database 99,9 procent is en de voorgaande query de waarden groter is dan 99,9% voor alle drie resourcedimensies retourneert, wordt uw workload waarschijnlijk in de grootte van de lagere compute past.
   
    Kijken naar het passend percentage geeft ook inzicht in of u naar de volgende hogere compute grootte overstappen moet om te voldoen aan het doel. Userdb1 ziet u bijvoorbeeld het volgende CPU-gebruik voor de afgelopen week:
   
   | Gemiddelde CPU-percentage | Maximale CPU-percentage |
   | --- | --- |
   | 24.5 |100.00 |
   
    De gemiddelde CPU is over een kwart van de limiet van de compute-grootte, die zijn voor de compute-grootte van de database geschikt zou. Maar de maximale waarde te zien dat de database heeft de limiet van de compute-grootte bereikt. Wilt u verplaatsen naar de volgende hogere compute grootte? Bekijken hoe vaak uw werkbelasting bereikt 100 procent en dit vervolgens vergelijken met het doel van de werkbelasting database.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Deze query retourneert een waarde minder dan 99,9 procent voor een van de drie resourcedimensies, kunt u overwegen om een van beide op de volgende hogere compute-grootte of toepassing afstemmen technieken gebruiken om de belasting op de SQL-database te verminderen.
4. In deze oefening wordt ook rekening gehouden uw toename van de verwachte werkbelasting in de toekomst.

Voor elastische pools kunt u afzonderlijke databases in de pool bewaken met de technieken die in deze sectie zijn beschreven. U kunt echter ook de groep als geheel bewaken. Zie [Een elastische pool bewaken en beheren](sql-database-elastic-pool-manage-portal.md) voor meer informatie.


### <a name="maximum-concurrent-requests"></a>Maximum aantal gelijktijdige aanvragen
Als u wilt zien van het aantal gelijktijdige aanvragen, deze Transact-SQL-query op uw SQL-database worden uitgevoerd:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Voor het analyseren van de werkbelasting van een on-premises SQL Server-database, wijzigt u deze query kunt u filteren op de specifieke database die u wilt analyseren. Als u een on-premises database met de naam MijnDatabase hebt, retourneert deze Transact-SQL-query bijvoorbeeld het aantal gelijktijdige aanvragen in de database:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dit is slechts een momentopname op een bepaald tijdstip. Als u een beter inzicht in uw werkbelasting en de vereisten voor gelijktijdige aanvraag, moet u voor het verzamelen van veel voorbeelden na verloop van tijd.

### <a name="maximum-concurrent-logins"></a>Maximum aantal gelijktijdige aanmeldingen
U kunt uw gebruikers en toepassingen patronen voor een beter beeld van de frequentie van aanmeldingen analyseren. U kunt ook echte belastingen uitvoeren in een testomgeving om ervoor te zorgen dat u niet te maken met bent deze of andere limieten we in dit artikel bespreken. Er is een eenvoudige query uitvoeren of dynamische beheerweergave (DMV) waarin kunt u gelijktijdige aanmelding wordt geteld of geschiedenis niet.

Als meerdere clients de verbindingsreeks gebruiken, verifieert de service voor elke aanmelding. Als er 10 gebruikers tegelijk verbinding met een database met behulp van dezelfde gebruikersnaam en wachtwoord, wordt 10 gelijktijdige aanmeldingen. Deze limiet geldt alleen voor de duur van de aanmelding en verificatie. Als dezelfde 10 gebruikers verbinding met de database sequentieel worden verwerkt, wordt het aantal gelijktijdige aanmeldingen nooit groter zijn dan 1.

> [!NOTE]
> Op dit moment is deze limiet niet van toepassing op databases in elastische pools.
> 
> 

### <a name="maximum-sessions"></a>Maximum aantal sessies
Als u wilt zien van het aantal huidige actieve sessies, deze Transact-SQL-query op uw SQL-database worden uitgevoerd:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Als u een on-premises SQL Server-werkbelasting analyseert, wijzigt u de query om zich te richten op een specifieke database. Deze query kunt u mogelijke sessie behoeften voor de database te bepalen of u overweegt u deze verplaatst naar Azure SQL Database.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Deze query's retourneren, een aantal point-in-time. Als u meerdere voorbeelden na verloop van tijd verzamelt, hebt u het beste begrip van de sessie gebruikt.

Voor de analyse van de SQL-Database, kunt u historische statistieken opvragen op sessies door het opvragen van de [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) weergeven en controleren van de **active_session_count** kolom. 

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

