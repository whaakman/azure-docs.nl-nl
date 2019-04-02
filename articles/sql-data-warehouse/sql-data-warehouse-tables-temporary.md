---
title: Tijdelijke tabellen in SQL Data Warehouse | Microsoft Docs
description: Essentiële instructies voor het gebruik van tijdelijke tabellen en belangrijke functies van de beginselen van de sessie op tijdelijke tabellen.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/01/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 23a62e28700ad5fd733040c43ea0eec225fd286f
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793098"
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>Tijdelijke tabellen in SQL Data Warehouse
In dit artikel bevat essentiële instructies voor het gebruik van tijdelijke tabellen en markeert u de beginselen van de sessie op tijdelijke tabellen. Gebruik de informatie in dit artikel kunt u modularize van uw code, om zowel herbruikbaarheid en eenvoudig onderhoud van uw code te verbeteren.

## <a name="what-are-temporary-tables"></a>Wat zijn tijdelijke tabellen?
Tijdelijke tabellen zijn nuttig bij het verwerken van gegevens - met name tijdens de transformatie waar de tussenliggende resultaten tijdelijk zijn. In SQL Data Warehouse aanwezig tijdelijke tabellen op het sessieniveau van de.  Ze zijn alleen zichtbaar voor de sessie waarin ze zijn gemaakt en worden automatisch verwijderd wanneer die sessie zich afmeldt.  Tijdelijke tabellen bieden een voordeel van de prestaties omdat de resultaten worden geschreven naar een lokaal in plaats van de externe opslag.

## <a name="create-a-temporary-table"></a>Maak een tijdelijke tabel
Tijdelijke tabellen worden gemaakt door het voorvoegsel van de tabelnaam van uw met een `#`.  Bijvoorbeeld:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Tijdelijke tabellen kunnen ook worden gemaakt met een `CTAS` exact dezelfde benadering gebruiken:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> `CTAS` is een krachtige opdracht en de toegevoegde voordeel van efficiënte in het gebruik van transactielogboekruimte. 
> 
> 

## <a name="dropping-temporary-tables"></a>Tijdelijke tabellen verwijderen
Als u een nieuwe sessie maakt, wordt geen tijdelijke tabellen moeten bestaan.  Echter, als u verbinding maakt met de dezelfde opgeslagen procedure, die zorgt voor een tijdelijke met dezelfde naam, om ervoor te zorgen dat uw `CREATE TABLE` instructies zijn gelukt een eenvoudige vooraf bestaan uit met een `DROP` kan worden gebruikt zoals in het volgende voorbeeld:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Voor consistentie met coderen, is het raadzaam om te gebruiken van dit patroon voor tabellen en tijdelijke tabellen.  Het is ook een goed idee om te gebruiken `DROP TABLE` tijdelijke tabellen worden verwijderd wanneer u klaar bent met hen in uw code.  Bij de ontwikkeling van de opgeslagen procedure wordt is het gebruikelijk om te zien van dat de opdrachten drop samen gebundeld aan het einde van een procedure om te controleren of dat deze objecten worden opgeschoond.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing code
Omdat de tijdelijke tabellen kunnen overal worden weergegeven in de sessie van een gebruiker, kan dit worden misbruikt zodat u kunt uw toepassingscode modularize.  Bijvoorbeeld, genereert de volgende opgeslagen procedure DDL voor het bijwerken van alle statistische gegevens in de database door de naam van statistieken.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

In dit stadium is de enige actie die heeft plaatsgevonden het maken van een opgeslagen procedure die een tijdelijke tabel, #stats_ddl, met de DDL-componenten genereert.  Deze opgeslagen procedure komt #stats_ddl als deze al bestaat, om ervoor te zorgen mislukt de query niet als meer dan één keer uitgevoerd in een sessie.  Echter, omdat er geen `DROP TABLE` aan het einde van de opgeslagen procedure, wanneer de opgeslagen procedure is voltooid, laat u de gemaakte tabel zodat deze buiten de opgeslagen procedure kan worden gelezen.  In SQL Data Warehouse is in tegenstelling tot andere SQL Server-databases, het mogelijk gebruik van de tijdelijke tabel buiten de procedure die u hebt gemaakt.  Tijdelijke tabellen van SQL Data Warehouse kunnen worden gebruikt **overal** binnen de sessie. Dit kan leiden tot meer modulaire en beheerbare code zoals in het volgende voorbeeld:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Beperkingen van de tijdelijke tabel
SQL Data Warehouse een aantal beperkingen opleggen bij het implementeren van tijdelijke tabellen.  Op dit moment alleen tijdens de sessie binnen het bereik van tijdelijke tabellen worden ondersteund.  Globale tijdelijke tabellen worden niet ondersteund.  Bovendien kunnen niet weergaven worden gemaakt op tijdelijke tabellen.  Tijdelijke tabellen kunnen alleen worden gemaakt met hash of round robin-verdeling.  Distributie van gerepliceerde tijdelijke tabel wordt niet ondersteund. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwikkelen van tabellen, het [tabeloverzicht](sql-data-warehouse-tables-overview.md).

