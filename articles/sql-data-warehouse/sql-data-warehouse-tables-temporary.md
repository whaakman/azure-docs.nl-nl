---
title: Tijdelijke tabellen in SQL Data Warehouse | Microsoft Docs
description: Aan de slag met tijdelijke tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 9b1119eb-7f54-46d0-ad74-19c85a2a555a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: e3b2f9017ecea7d9f78c07476f96c3dd8d031863
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>Tijdelijke tabellen in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Gegevenstypen][Data Types]
> * [Distribueren][Distribute]
> * [Index][Index]
> * [Partitie][Partition]
> * [Statistieken][Statistics]
> * [Tijdelijke][Temporary]
> 
> 

Tijdelijke tabellen zijn nuttig bij het verwerken van gegevens - met name tijdens transformatie waar de tussenliggende resultaten tijdelijke zijn. In SQL Data Warehouse bestaan tijdelijke tabellen op het niveau van de sessie.  Ze zijn alleen zichtbaar voor de sessie waarin ze zijn gemaakt en worden automatisch verwijderd wanneer die sessie zich afmeldt.  Tijdelijke tabellen bieden een voordeel van de prestaties omdat de resultaten worden geschreven naar lokale in plaats van de externe opslag.  Tijdelijke tabellen zijn enigszins afwijken in Azure SQL Data Warehouse Azure SQL Database, omdat ze toegankelijk zijn vanuit een willekeurige plaats in de sessie, inclusief zowel binnen als buiten een opgeslagen procedure.

In dit artikel bevat essentiële instructies voor het gebruik van tijdelijke tabellen en markeert de beginselen van sessie niveau tijdelijke tabellen. Gebruik de informatie in dit artikel kunt u modularize uw code, zowel hergebruik en het gemak van onderhoud van uw code te verbeteren.

## <a name="create-a-temporary-table"></a>Maak een tijdelijke tabel
Tijdelijke tabellen zijn gemaakt door het voorvoegsel van de tabelnaam van uw met een `#`.  Bijvoorbeeld:

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

Tijdelijke tabellen kunnen ook worden gemaakt met een `CTAS` met exact dezelfde manier:

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
> `CTAS`is een krachtige opdracht en de bijkomend voordeel is efficiënte tijdens het gebruik van ruimte voor transactielogboeken. 
> 
> 

## <a name="dropping-temporary-tables"></a>Tijdelijke tabellen verwijderen
Wanneer een nieuwe sessie wordt gemaakt, moeten er zijn geen tijdelijke tabellen bestaan.  Echter, als u verbinding maakt met de dezelfde opgeslagen procedure, die een tijdelijke met dezelfde naam maakt, om ervoor te zorgen dat uw `CREATE TABLE` -instructies zijn geslaagd een eenvoudige vooraf bestaan uit met een `DROP` kan worden gebruikt zoals in het volgende voorbeeld:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Het is verstandig om te gebruiken dit patroon voor tabellen en tijdelijke tabellen voor het coderen van de consistentie.  Het is ook een goed idee om te gebruiken `DROP TABLE` tijdelijke tabellen verwijderen wanneer u klaar bent met hen in uw code.  In de ontwikkeling van de opgeslagen procedure is het gebruikelijk om te zien van dat de opdrachten drop samen gebundeld aan het einde van een procedure om te controleren of dat deze objecten worden opgeschoond.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing code
Omdat de tijdelijke tabellen ziet u een willekeurige plaats in een gebruikerssessie, kan dit zodat u uw toepassingscode modularize worden misbruikt.  Bijvoorbeeld, genereert de volgende opgeslagen procedure DDL om bij te werken van alle statistische gegevens in de database met de naam van statistieken.

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

In deze fase is de enige actie die is opgetreden het maken van een opgeslagen procedure die generatess een tijdelijke tabel #stats_ddl, met de DDL-instructies.  Deze opgeslagen procedure zakt #stats_ddl als deze al bestaat om ervoor te zorgen mislukt de query niet als meer dan één keer worden uitgevoerd in een sessie.  Echter, omdat er geen `DROP TABLE` aan het einde van de opgeslagen procedure, wanneer de opgeslagen procedure is voltooid, laat u de gemaakte tabel zodat deze buiten de opgeslagen procedure kan worden gelezen.  In SQL Data Warehouse is in tegenstelling tot andere SQL Server-databases het mogelijk om te gebruiken van de tijdelijke tabel buiten de procedure waarvan deze is gemaakt.  SQL Data Warehouse tijdelijke tabellen kunnen worden gebruikt **overal** binnen de sessie. Dit kan leiden tot meer modulaire en beheerbare code zoals in het volgende voorbeeld:

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
SQL Data Warehouse een aantal beperkingen opleggen bij het implementeren van tijdelijke tabellen.  Op dit moment alleen tijdens de sessie bereik tijdelijke tabellen worden ondersteund.  Globale tijdelijke tabellen worden niet ondersteund.  Bovendien kunnen niet weergaven worden gemaakt op tijdelijke tabellen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie de artikelen op [tabel overzicht][Overview], [tabel gegevenstypen][Data Types], [distribueren van een tabel][Distribute], [indexeren van een tabel][Index], [partitioneren van een tabel] [ Partition] en [tabelstatistieken onderhouden][Statistics].  Zie voor meer informatie over best practices [aanbevolen procedures van SQL Data Warehouse][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
