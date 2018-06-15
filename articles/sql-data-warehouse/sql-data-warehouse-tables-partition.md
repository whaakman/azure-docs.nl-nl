---
title: Partitioneren van tabellen in Azure SQL Data Warehouse | Microsoft-documenten
description: Aanbevelingen en voorbeelden voor het gebruik van de Tabelpartities in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: ada55950ee36222e70809e2ef423c63612cd61ed
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526774"
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>Tabellen in SQL Data Warehouse partitioneren
Aanbevelingen en voorbeelden voor het gebruik van de Tabelpartities in Azure SQL Data Warehouse.

## <a name="what-are-table-partitions"></a>Wat zijn de Tabelpartities?
Tabelpartities kunnen u uw gegevens onderverdelen in kleinere groepen van gegevens. In de meeste gevallen Tabelpartities gemaakt op een datumkolom. Partitioneren wordt ondersteund op alle SQL Data Warehouse tabeltypen; inclusief geclusterde columnstore geclusterde index en heap. Partitioneren wordt ook ondersteund voor alle typen distributiepunten, inclusief zowel hash of round-robin gedistribueerd.  

Partitioneren, profiteert gegevens onderhoud en query-prestaties. Hiermee wordt aangegeven of deze voordelen biedt voor beide of slechts één is afhankelijk van hoe gegevens worden geladen en of dezelfde kolom voor beide doeleinden kan worden gebruikt omdat het partitioneren kan alleen worden uitgevoerd op één kolom.

### <a name="benefits-to-loads"></a>Voordelen voor belasting
Het belangrijkste voordeel van partities in SQL Data Warehouse is voor het verbeteren van de efficiëntie en prestaties van het laden van gegevens via het gebruik van de partitie verwijderen, overschakelen en samenvoegen. In de meeste gevallen worden de gegevens op een datumkolom die nauw is verbonden met de volgorde waarin de gegevens in de database is geladen gepartitioneerd. Een van de grootste voordelen van het gebruik van partities voor het behouden van gegevens wordt het vermijden van transactielogboeken. Gewoon invoegen, bijwerken of verwijderen van gegevens kan de eenvoudigste manier, met een beetje gedachte en moeite, zijn met behulp van partities tijdens het laadproces aanzienlijk prestaties kan verbeteren.

Overschakelen van de partitie kan worden gebruikt om snel te verwijderen of een gedeelte van een tabel te vervangen.  Een verkoop feitentabel bevat mogelijk alleen gegevens voor de afgelopen 36 maanden. Aan het einde van elke maand, wordt de oudste maand verkoopgegevens verwijderd uit de tabel.  Deze gegevens kan worden verwijderd met behulp van een instructie delete verwijderen van de gegevens voor de oudste maand. Echter kunt verwijderen van een grote hoeveelheid gegevens per rij met een instructie delete te veel tijd in beslag nemen, evenals het risico van grote transacties die veel tijd om terugdraaien nemen als er iets mis gaat maken. Een meer optimale aanpak is het verwijderen van de oudste partitie van gegevens. Wanneer het verwijderen van de afzonderlijke rijen kan uren duren, kan verwijderen van een volledige partitie seconden duren.

### <a name="benefits-to-queries"></a>Voordelen voor query 's
Partitioneren kan ook worden gebruikt om queryprestaties te verbeteren. Een query die een filter voor gepartitioneerde gegevens geldt kunt beperken de scan voor alleen de in aanmerking komend partities. Deze methode voor het filteren van kan een volledige tabelscan vermijden en alleen een subset van gegevens te scannen. Door de introductie van de geclusterde columnstore-indexen, de voordelen van de prestaties predikaat eliminatie minder nuttig zijn, maar in sommige gevallen kan er een voordeel op query's. Bijvoorbeeld, als de verkoop feitentabel in 36 maanden met behulp van de verkoop datumveld is gepartitioneerd, gaat u query's met het filter op de verkoopdatum kunt overslaan zoeken in partities die niet overeenkomen met het filter.

## <a name="sizing-partitions"></a>Formaat van partities
Terwijl partities kan worden gebruikt om prestaties te verbeteren sommige scenario's, maken van een tabel met **te veel** partities prestaties in bepaalde omstandigheden kunnen schaden.  Deze problemen zijn vooral voor geclusterde columnstore-tabellen. Voor het partitioneren van voor het handig zijn, is het belangrijk om te begrijpen wanneer gebruikt u partitioneren en het aantal partities te maken. Er is geen vaste snelle regel garantie voor het aantal partities te veel zijn, deze afhankelijk is van uw gegevens en het aantal partities u tegelijkertijd te laden. Een geslaagde partitieschema heeft meestal tientallen, honderden niet duizendtallen-partities.

Bij het maken van partities op **geclusterde columnstore** tabellen, het is belangrijk rekening te houden hoeveel rijen deel uitmaken van elke partitie. Voor optimale compressie en prestaties van de geclusterde columnstore-tabellen, is een minimum van 1 miljoen rijen per partitie distributie en het nodig. Voordat partities worden gemaakt, wordt elke tabel al verdeeld met SQL Data Warehouse in 60 gedistribueerde databases. Alle partitioneren toegevoegd aan een tabel is naast de distributies achter de schermen gemaakt. In dit voorbeeld gebruiken als de verkoop feitentabel 36 maandelijkse partities bevat en het feit dat de SQL Data Warehouse is 60 distributies, klikt u vervolgens de verkoop feitentabel bevatten moet 60 miljoen rijen per maand of 2.1 miljard rijen wanneer alle maanden worden ingevuld. Als een tabel minder dan het aanbevolen minimum aantal rijen per partitie bevat, kunt u overwegen minder partities om het aantal rijen per partitie verhogen. Zie voor meer informatie de [indexering](sql-data-warehouse-tables-index.md) artikel, waaronder query's die de kwaliteit van de cluster columnstore-indexen kunnen beoordelen.

## <a name="syntax-differences-from-sql-server"></a>Syntaxisverschillen van SQL Server
SQL Data Warehouse introduceert een manier voor het definiëren van de partities die eenvoudiger is dan SQL Server. Partitionering functies en schema's worden niet gebruikt in SQL Data Warehouse omdat ze zich in SQL Server. In plaats daarvan is hoeft u gepartitioneerde kolom en de punten van de grens te identificeren. Hoewel de syntaxis van het partitioneren van mogelijk enigszins afwijken van de SQL Server, wordt de basisconcepten zijn hetzelfde. SQL Server en SQL Data Warehouse ondersteuning voor één partitiekolom per tabel, wat kan varieerde partitie. Zie voor meer informatie over partitioneren, [gepartitioneerde tabellen en indexen](/sql/relational-databases/partitions/partitioned-tables-and-indexes).

Het volgende voorbeeld wordt de [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) instructie voor het partitioneren van heeft de tabel voor de kolom OrderDateKey:

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

## <a name="migrating-partitioning-from-sql-server"></a>Partitioneren van SQL-Server migreren
SQL Server partitie definities gewoon naar SQL Data Warehouse migreren:

- De SQL Server elimineren [partitieschema](/sql/t-sql/statements/create-partition-scheme-transact-sql).
- Voeg de [partitiefunctie](/sql/t-sql/statements/create-partition-function-transact-sql) uw CREATE TABLE-definitie.

Als u een gepartitioneerde tabel van een SQL Server-exemplaar migreert, de volgende SQL kan u helpen te achterhalen van het aantal rijen dat in elke partitie. Houd er rekening mee dat als dezelfde partitionering granulariteit in SQL Data Warehouse wordt gebruikt, het aantal rijen per partitie met een factor van 60 verlaagt.  

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

## <a name="workload-management"></a>Werklastbeheer
Een definitieve overweging voor de tweevoudige voor uw tabel partitie beslissing is [werkbelasting management](resource-classes-for-workload-management.md). Beheer van de werkbelasting in SQL Data Warehouse is hoofdzakelijk voor het beheer van geheugen en een gelijktijdigheid van taken. In SQL Data Warehouse, wordt de maximale hoeveelheid geheugen toegewezen aan elk distributiepunt tijdens het uitvoeren van query beheerst door resource klassen. De partities zijn in het ideale geval grootte tegen andere factoren zoals de geheugenvereisten van het bouwen van de geclusterde columnstore-indexen. Geclusterde columnstore-indexen voordeel aanzienlijk wanneer ze meer geheugen worden toegewezen. Daarom wilt u ervoor te zorgen dat opnieuw opbouwen van een index partitie niet van het geheugen is tekort komt. Vergroot de hoeveelheid geheugen die beschikbaar zijn voor uw query kan worden bereikt door het overschakelen van de standaard-rol, smallrc, op een van de andere functies, zoals largerc.

Informatie over het toewijzen van geheugen per distributie is beschikbaar door het opvragen van de dynamische beheerweergaven voor Resourceregeling. Uw geheugentoekenning is in werkelijkheid is kleiner dan de resultaten van de volgende query. Deze query bevat echter een niveau van de richtlijnen die u gebruiken kunt wanneer het formaat van partities voor gegevens beheerbewerkingen. Probeer om te voorkomen dat uw partities groter dan de geheugentoekenning geleverd door de extra grote resourceklasse formaat. Als uw partities afgezien van deze afbeelding toenemen, loopt u het risico van geheugendruk, wat op zijn beurt tot minder optimale compressie leidt.

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Partitie overschakelen
SQL Data Warehouse ondersteunt partitie splitsen en overschakelen samenvoegen. Elk van deze functies wordt uitgevoerd met de [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql) instructie.

Als u wilt overschakelen van partities tussen twee tabellen, moet u ervoor zorgen dat de partities zijn uitgelijnd op hun respectieve grenzen en dat de tabeldefinities overeenkomen. Als controleren beperkingen zijn niet beschikbaar voor het afdwingen van het bereik van waarden in een tabel, moet de dezelfde partitiegrenzen als de doeltabel in de brontabel bevatten. Als de partitiegrenzen niet vervolgens dezelfde, mislukken de switch partitie als de partitiemetagegevens van de niet gesynchroniseerd.

### <a name="how-to-split-a-partition-that-contains-data"></a>Het splitsen van een partitie die gegevens bevat
De meest efficiënte manier om op te splitsen van een partitie die al gegevens bevat, is met een `CTAS` instructie. Als de gepartitioneerde tabel een geclusterde columnstore, moet klikt u vervolgens de partitie van tabel niet leeg zijn voordat deze kan worden gesplitst.

Het volgende voorbeeld maakt een gepartitioneerde columnstore-tabel. Een rij wordt ingevoegd in elke partitie:

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


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> De tabelmetagegevens van de is door het maken van het object statistiek nauwkeuriger. Als u statistieken weglaat, wordt SQL Data Warehouse standaardwaarden gebruikt. Raadpleeg voor meer informatie over statistieken [statistieken](sql-data-warehouse-tables-statistics.md).
> 
> 

De volgende query vindt u het aantal rijen met behulp van de `sys.partitions` catalogusweergave:

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

Bericht 35346, 15 niveau 1 staat, regel 44 GESPLITST component van de instructie ALTER PARTITION is mislukt omdat de partitie niet leeg is. Alleen lege partities kunnen worden gesplitst wanneer een columnstore-index op de tabel bestaat. Houd rekening met het uitschakelen van de columnstore-index voordat u de instructie ALTER PARTITION geeft en bouw vervolgens de columnstore-index nadat ALTER PARTITION voltooid is.

U kunt echter `CTAS` voor het maken van een nieuwe tabel de gegevens kan bevatten.

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

Als de grenzen van partities zijn uitgelijnd, is een switch toegestaan. Dit betekent dat de brontabel met een lege partitie die u later kunt verdelen.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Alle die altijd is ingeschakeld op de gegevens naar de nieuwe grenzen van partities met één lijn wordt `CTAS`, en schakel vervolgens de gegevens terug naar de hoofdtabel.

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

Nadat u het verkeer van de gegevens hebt voltooid, is het een goed idee om te vernieuwen van de statistieken voor de doeltabel. Bijwerken van statistieken, zorgt u ervoor dat de statistieken goed beeld vormen van de nieuwe verdeling van de gegevens in hun respectieve partities.

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabel broncodebeheer partitioneren
Om te voorkomen dat de tabeldefinitie van de van **roesten** in uw bronbeheersysteem mogelijk wilt u de volgende benadering:

1. Maken van de tabel als een gepartitioneerde tabel, maar zonder waarden partitie

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

Met deze methode de code in broncodebeheer statische blijft en de partitionering waarden zijn toegestaan dynamisch; met de datawarehouse ontwikkeling gedurende een bepaalde periode.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwikkelen van tabellen de artikelen op [tabel overzicht](sql-data-warehouse-tables-overview.md).

