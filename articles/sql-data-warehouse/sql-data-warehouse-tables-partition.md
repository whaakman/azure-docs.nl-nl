---
title: Partitioneren van tabellen in Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen en voorbeelden voor het gebruik van de Tabelpartities in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/18/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: d3557be2fd8fdb459571d2c792302963e17e4471
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189390"
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>Partitioneren van tabellen in SQL Data Warehouse
Aanbevelingen en voorbeelden voor het gebruik van de Tabelpartities in Azure SQL Data Warehouse.

## <a name="what-are-table-partitions"></a>Wat zijn Tabelpartities?
Tabelpartities kunnen u uw gegevens onderverdelen in kleinere groepen van gegevens. In de meeste gevallen worden Tabelpartities gemaakt op een datumkolom. Partitioneren wordt ondersteund op alle typen voor SQL Data Warehouse-tabel; inclusief geclusterde columnstore, geclusterde index en heap. Partitioneren wordt ook ondersteund voor alle typen van de distributie, met inbegrip van hash- of gedistribueerd middels round robin.  

Partitioneren kan profiteren van de prestaties van de onderhoud en de query van de gegevens. Of deze beide of slechts een voordelen is afhankelijk van hoe gegevens worden geladen en of de dezelfde kolom voor beide doeleinden kan worden gebruikt omdat partitioneren kan alleen worden uitgevoerd op één kolom.

### <a name="benefits-to-loads"></a>Voordelen bij het laden
Het belangrijkste voordeel van partitionering in SQL Data Warehouse is voor het verbeteren van de efficiëntie en prestaties van het laden van gegevens met behulp van de partitie verwijderen, overschakelen en samenvoegen. In de meeste gevallen worden de gegevens gepartitioneerd op een datumkolom die nauw is verbonden met de volgorde waarin de gegevens in de database wordt geladen. Een van de grootste voordelen van het gebruik van partities voor het beheren van gegevens wordt het vermijden van transactielogboeken. Terwijl gewoon invoegen, bijwerken of verwijderen van gegevens de eenvoudigste manier, met wat creativiteit en inspanning is kan partitioneren tijdens het laadproces aanzienlijk prestaties verbeteren.

Overschakelen van de partitie kan worden gebruikt om snel te verwijderen of vervangen door een gedeelte van een tabel.  Een verkoop feitentabel bevat mogelijk alleen gegevens voor de afgelopen 36 maanden. Aan het einde van elke maand, wordt de oudste maand aan verkoopgegevens uit de tabel verwijderd.  Deze gegevens kan worden verwijderd met behulp van een delete-instructie te verwijderen van de gegevens voor de oudste maand. Echter kan verwijderen van een grote hoeveelheid gegevens per rij met een delete-instructie te veel tijd in beslag nemen, evenals het risico van grote transacties die lange tijd duren voordat terugdraaien als er iets misgaat. Een meer optimale aanpak is om te verwijderen van de oudste partitie van de gegevens. Waar het verwijderen van de afzonderlijke rijen uur kan duren kan als u een volledige partitie seconden duren.

### <a name="benefits-to-queries"></a>Voordelen voor query 's
Partitioneren kan ook worden gebruikt om queryprestaties te verbeteren. Een query die een filter is van toepassing op gepartitioneerde gegevens kunt beperken de scan naar alleen de partities die daarvoor in aanmerking komen. Deze methode van filters kunt te voorkomen dat een volledige tabelcontrole uitgevoerd en alleen een subset van gegevens te scannen. Met de introductie van geclusterde columnstore-indexen, de voordelen van de prestaties predicaat opheffing minder nuttig zijn, maar in sommige gevallen kan er een voordeel op query's. Bijvoorbeeld, als de verkoop feitentabel is verdeeld over 36 maanden met behulp van het veld datum voor verkoop en vervolgens met het filter een query uitgevoerd op de verkoopdatum kunt overslaan zoeken in partities, die niet overeenkomen met het filter.

## <a name="sizing-partitions"></a>Formaat van partities
Terwijl partities kan worden gebruikt om prestaties te verbeteren enkele scenario's, het maken van een tabel met **te veel** partities kunnen de prestaties nadelig beïnvloeden onder bepaalde omstandigheden.  Deze problemen zijn vooral van toepassing voor geclusterde columnstore-tabellen. Voor partitionering om nuttig zijn, is het belangrijk om te begrijpen wanneer u partitioneren en het aantal partities te maken. Er is geen vaste snelle regel garantie voor het aantal partities te veel zijn, dat hangt ervan af op uw gegevens en het aantal partities u tegelijkertijd wordt geladen. Een geslaagde partitieschema heeft meestal tientallen, honderden of duizenden-partities.

Bij het maken van partities op **geclusterde columnstore** tabellen, het is belangrijk om te overwegen hoeveel rijen u deel uitmaken van elke partitie. Voor optimale compressie en prestaties van de geclusterde columnstore-tabellen, een minimum van 1 miljoen rijen per distributie en partitie vereist. Voordat u partities maakt, verdeelt SQL Data Warehouse al elke tabel in 60 gedistribueerde databases. Alle partitioneren toegevoegd aan een tabel is naast de distributies achter de schermen gemaakt. Met behulp van dit voorbeeld als de verkoop feitentabel 36 maandelijkse partities opgenomen en gezien het feit dat SQL Data Warehouse 60 distributies is, klikt u vervolgens de verkoop feitentabel moet 60 miljoen rijen per maand, of 2.1 miljard rijen bevatten wanneer alle maanden worden ingevuld. Als een tabel minder dan de aanbevolen minimum aantal rijen per partitie bevat, kunt u overwegen minder partities om te verhogen van het aantal rijen per partitie. Zie voor meer informatie de [indexeren](sql-data-warehouse-tables-index.md) artikel, waaronder query's die de kwaliteit van de geclusterde columnstore-indexen kunnen evalueren.

## <a name="syntax-differences-from-sql-server"></a>Van de syntaxisverschillen met SQL Server
SQL Data Warehouse introduceert een manier voor het definiëren van partities die is eenvoudiger dan SQL Server. Partitionering functies en schema's worden niet gebruikt in SQL Data Warehouse zoals in SQL Server. U hoeft alleen is in plaats daarvan gepartitioneerde kolom en de grens punten identificeren. De syntaxis van het partitioneren van mogelijk enigszins afwijken van de SQL Server, zijn de basisconcepten hetzelfde. SQL Server en SQL Data Warehouse ondersteuning voor één partitiekolom per tabel, die variabele partitie kan zijn. Zie voor meer informatie over het partitioneren, [gepartitioneerde tabellen en indexen](/sql/relational-databases/partitions/partitioned-tables-and-indexes).

Het volgende voorbeeld wordt de [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) instructie voor het partitioneren van de tabel FactInternetSales op de kolom OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Partitioneren van SQL Server migreren
Definities van de SQL Server-partitie gewoon naar SQL Data Warehouse migreren:

- De SQL-Server te elimineren [partitieschema](/sql/t-sql/statements/create-partition-scheme-transact-sql).
- Voeg de [partitiefunctie](/sql/t-sql/statements/create-partition-function-transact-sql) definitie aan uw tabel maken.

Als u een gepartitioneerde tabel van een SQL Server-exemplaar migreert, de volgende SQL kan u helpen te achterhalen van het aantal rijen dat in elke partitie. Houd er rekening mee dat als de dezelfde partitioneren granulatie wordt gebruikt in SQL Data Warehouse, het aantal rijen per partitie wordt verlaagd met een factor van 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="partition-switching"></a>Partitie overschakelen
SQL Data Warehouse biedt ondersteuning voor partitie te splitsen, samenvoegen en overschakelen. Elk van deze functies wordt uitgevoerd met behulp van de [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql) instructie.

Als u wilt overschakelen van partities tussen twee tabellen, moet u ervoor zorgen dat de partities op hun respectieve grenzen bevinden uitlijnen en dat de tabeldefinities overeen. Als controleren beperkingen zijn niet beschikbaar voor het afdwingen van het bereik van waarden in een tabel, moet de partitiegrenzen van het dezelfde als de doeltabel bevatten in de brontabel. Als de grenzen van partities niet vervolgens dezelfde, klikt u vervolgens mislukt de partitie-switch omdat de partitiemetagegevens van de niet gesynchroniseerd.

### <a name="how-to-split-a-partition-that-contains-data"></a>Het splitsen van een partitie met gegevens
De meest efficiënte methode voor het splitsen van een partitie die al gegevens bevat, is met een `CTAS` instructie. Als de gepartitioneerde tabel een geclusterde columnstore is, klikt u vervolgens moet de tabelpartitie leeg zijn voordat deze kan worden gesplitst.

Het volgende voorbeeld wordt een gepartitioneerde columnstore-tabel. Een rij wordt ingevoegd in elke partitie:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);
```

De volgende query wordt het aantal rijen gevonden met behulp van de `sys.partitions` catalogusweergave:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

De volgende opdracht splitsen ontvangt een foutbericht weergegeven:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, 15 niveau 1 staat, regel 44 GESPLITST component van de instructie ALTER PARTITION is mislukt omdat de partitie niet leeg zijn. Alleen lege partities kunnen worden gesplitst wanneer een columnstore-index op de tabel bestaat. Overweeg het uitschakelen van de columnstore-index voor de instructie ALTER PARTITION geeft en bouw vervolgens de columnstore-index nadat ALTER PARTITION voltooid is.

U kunt echter `CTAS` om een nieuwe tabel voor het opslaan van de gegevens te maken.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Als de grenzen van partities zijn uitgelijnd, is een switch toegestaan. Dit betekent dat de brontabel met een lege partitie die u later kunt splitsen.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Alleen nog is het uitlijnen van de gegevens naar de nieuwe partitiegrenzen met behulp van `CTAS`, en schakel vervolgens de gegevens terug naar de hoofdtabel.

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Nadat u de verplaatsing van de gegevens hebt voltooid, is het een goed idee om te vernieuwen van de statistieken voor de doeltabel. Bijwerken van statistieken, zorgt ervoor dat de statistieken de nieuwe distributie van de gegevens in hun respectieve partities nauwkeurig weergeven.

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="load-new-data-into-partitions-that-contain-data-in-one-step"></a>Nieuwe gegevens laden in partities die gegevens in één stap bevatten
Het laden van gegevens in partities met het overschakelen van de partitie is een handige manier fase nieuwe gegevens in een tabel die is niet zichtbaar voor gebruikers de switch in de nieuwe gegevens.  Het kan lastig zijn op bezet systemen te bekommeren om de vergrendeling conflicten die zijn gekoppeld aan het overschakelen van de partitie.  Om te wissen uit de bestaande gegevens in een partitie een `ALTER TABLE` gebruikt om te worden vereist om over te schakelen om de gegevens.  Vervolgens een andere `ALTER TABLE` is vereist om over te schakelen in de nieuwe gegevens.  In SQL Data Warehouse de `TRUNCATE_TARGET` optie wordt ondersteund in de `ALTER TABLE` opdracht.  Met `TRUNCATE_TARGET` de `ALTER TABLE` opdracht overschrijft bestaande gegevens in de partitie met nieuwe gegevens.  Hieronder volgt een voorbeeld dat gebruikmaakt van `CTAS` naar een nieuwe tabel maken met de bestaande gegevens, voegt de nieuwe gegevens en switches die alle gegevens weer terug in de doeltabel, overschrijft de bestaande gegevens.

```sql
CREATE TABLE [dbo].[FactInternetSales_NewSales]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

INSERT INTO dbo.FactInternetSales_NewSales
VALUES (1,20000101,2,2,2,2,2,2);

ALTER TABLE dbo.FactInternetSales_NewSales SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2 WITH (TRUNCATE_TARGET = ON);  
```

### <a name="table-partitioning-source-control"></a>Tabel partitioneren broncodebeheer
Om te voorkomen dat de definitie van de tabel van **roesten** in uw bronbeheersysteem, kunt u rekening houden met de volgende benadering:

1. Maken van de tabel als een gepartitioneerde tabel, maar geen waarden partitie

    ```sql
    CREATE TABLE [dbo].[FactInternetSales]
    (
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
    )
    WITH
    (   CLUSTERED COLUMNSTORE INDEX
    ,   DISTRIBUTION = HASH([ProductKey])
    ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES () )
    )
    ;
    ```

1. `SPLIT` de tabel als onderdeel van het implementatieproces:

    ```sql
     -- Create a table containing the partition boundaries

    CREATE TABLE #partitions
    WITH
    (
        LOCATION = USER_DB
    ,   DISTRIBUTION = HASH(ptn_no)
    )
    AS
    SELECT  ptn_no
    ,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
    FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
    ) a
    ;

     -- Iterate over the partition boundaries and split the table

    DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
    ,       @i INT = 1                                 --iterator for while loop
    ,       @q NVARCHAR(4000)                          --query
    ,       @p NVARCHAR(20)     = N''                  --partition_number
    ,       @s NVARCHAR(128)    = N'dbo'               --schema
    ,       @t NVARCHAR(128)    = N'FactInternetSales' --table
    ;

    WHILE @i <= @c
    BEGIN
        SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
        SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

        -- PRINT @q;
        EXECUTE sp_executesql @q;
        SET @i+=1;
    END

     -- Code clean-up

    DROP TABLE #partitions;
    ```

Met deze methode de code in broncodebeheer statische blijft en de partitionering waarden zijn toegestaan dynamisch; na verloop van tijd aan het magazijn in ontwikkeling.

## <a name="next-steps"></a>Volgende stappen
Zie de artikelen voor meer informatie over het ontwikkelen van tabellen op [tabeloverzicht](sql-data-warehouse-tables-overview.md).

