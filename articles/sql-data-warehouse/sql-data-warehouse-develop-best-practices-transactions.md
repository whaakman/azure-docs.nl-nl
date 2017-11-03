---
title: Optimaliseren van transacties voor SQL Data Warehouse | Microsoft Docs
description: "Aanbevolen procedurerichtlijn over het schrijven van efficiënte transactie-updates in Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 6f326f26-8a54-49df-a482-9c96a58db371
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: f9f19d75a37351b3562ce8c2f3629df14c5437c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimaliseren van transacties voor SQL Data Warehouse
In dit artikel wordt uitgelegd hoe de prestaties van uw code transactionele terwijl het risico voor lange Rollback minimaliseert optimaliseren.

## <a name="transactions-and-logging"></a>Transacties en logboekregistratie
Transacties zijn een belangrijk onderdeel van een relationele database-engine. SQL Data Warehouse maakt gebruik van transacties tijdens het aanpassen van gegevens. Deze transacties kunnen expliciete of impliciete zijn. Één `INSERT`, `UPDATE` en `DELETE` -instructies zijn alle voorbeelden van impliciete transacties. Expliciete transacties expliciet worden geschreven door een ontwikkelaar met behulp van `BEGIN TRAN`, `COMMIT TRAN` of `ROLLBACK TRAN` en worden doorgaans gebruikt wanneer meerdere instructies voor een wijziging moeten worden gebundeld in een atomic-eenheid. 

Azure SQL Data Warehouse wijzigingen worden doorgevoerd in de database met behulp van de transactielogboeken. Elk distributiepunt heeft een eigen transactielogboek. Transactie logboekschrijfbewerkingen worden automatisch. Er is geen configuratie nodig. Tijdens dit proces zorgt ervoor dat de voor schrijven deze een overhead introduceren in het systeem. U kunt deze gevolgen minimaliseren door transactioneel efficiënte code te schrijven. Transactioneel efficiënte code grote schaal worden onderverdeeld in twee categorieën.

* Waar mogelijk gebruikmaken van minimale logboekregistratie constructies
* Gegevens over het installatieproces bereik batches gebruiken om te voorkomen dat enkelvoud langlopende transacties
* Een patroon voor grote wijzigingen in een bepaalde partitie overschakelen partitie vaststellen

## <a name="minimal-vs-full-logging"></a>Minimale versus volledige logboekregistratie
In tegenstelling tot volledig geregistreerde bewerkingen, die gebruikmaken van het transactielogboek om elke rijwijziging bij te houden, minimaal geregistreerde bewerkingen van bijhouden mate toewijzingen, en alleen de wijzigingen voor de metagegevens. Daarom minimale logboekregistratie omvat alleen de informatie die is vereist voor het terugdraaien van de transactie in het geval van een storing of een expliciete aanvraag logboekregistratie (`ROLLBACK TRAN`). Omdat veel minder gegevens worden bijgehouden in het transactielogboek, presteert een minimaal geregistreerde bewerking beter dan een vergelijkbaar formaat volledig geregistreerde bewerking. Bovendien, omdat minder schrijfbewerkingen het transactielogboek gaat, een veel kleinere hoeveelheid gegevens aan het logboek is gegenereerd en dus meer i/o efficiënt.

De limieten van de veiligheid transactie zijn alleen van toepassing op volledig geregistreerde bewerkingen.

> [!NOTE]
> Minimaal geregistreerde bewerkingen kunnen deelnemen aan expliciete transacties. Zoals alle wijzigingen in de toewijzing van structuren worden bijgehouden, is het mogelijk minimaal geregistreerde bewerkingen terugdraaien. Het is belangrijk te begrijpen dat de wijziging 'minimaal' wordt geregistreerd is niet niet vastgelegd.
> 
> 

## <a name="minimally-logged-operations"></a>Minimaal geregistreerde bewerkingen
De volgende bewerkingen zijn geschikt voor minimaal aan te melden:

* MAAK TABLE AS SELECT ([CTAS][CTAS])
* INSERT... SELECTEER
* INDEX MAKEN
* ALTER INDEX REBUILD
* INDEX VERWIJDEREN
* TABEL AFKAPPEN
* TABEL VERWIJDEREN
* ALTER TABLE SWITCH PARTITIE

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> Interne data movement-bewerkingen (zoals `BROADCAST` en `SHUFFLE`) worden niet beïnvloed door de limiet van de veiligheid transactie.
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>Minimale logboekregistratie met bulksgewijs laden
`CTAS`en `INSERT...SELECT` zijn beide bulksgewijs laden bewerkingen. Echter beide zijn beïnvloed door de definitie van de doel-tabel en zijn afhankelijk van de load-scenario. Hieronder vindt u een tabel met uitleg over als uw bulkbewerking volledig of minimaal geregistreerd:  

| Primaire Index | Load Scenario | Modus voor logboekregistratie |
| --- | --- | --- |
| Object-heap |Alle |**Minimale** |
| Geclusterde Index |Lege doeltabel |**Minimale** |
| Geclusterde Index |Geladen rijen niet overlappen met bestaande pagina's in het doel |**Minimale** |
| Geclusterde Index |Geladen rijen overlappen met bestaande pagina's in het doel |Volledig |
| Geclusterde Columnstore-Index |Batchgrootte > = 102,400 per partitie uitgelijnd distributie |**Minimale** |
| Geclusterde Columnstore-Index |Batch-grootte < 102,400 per partitie uitgelijnd distributie |Volledig |

Hierbij moet worden opgemerkt dat er geen schrijfbewerkingen secundaire of niet-geclusterde indexen bijwerken altijd volledig geregistreerde bewerkingen zal zijn.

> [!IMPORTANT]
> SQL Data Warehouse is 60 distributies. Daarom, ervan uitgaande dat alle rijen gelijkmatig worden verdeeld en aanvoer in één partitie, uw batch moet 6,144,000 rijen bevatten of groter minimaal zijn aangemeld bij het schrijven naar een geclusterde Columnstore-Index. Als de tabel is gepartitioneerd en de rijen worden ingevoegd span grenzen van partities, moet u 6,144,000 rijen per partitie grens ervan uitgaande dat zelfs gegevensdistributie. Elke partitie in elke distributie moet onafhankelijk langer zijn dan de drempelwaarde 102.400 rij voor de invoeging moeten minimaal worden vastgelegd in de distributie.
> 
> 

Laden van gegevens naar een niet-lege tabel met een geclusterde index kunt bevatten vaak een combinatie van volledig vastgelegd en minimaal geregistreerde rijen. Een geclusterde index is een evenwichtige structuur (b-tree) van pagina's. Als de pagina wordt geschreven naar bevat al rijen uit een andere transactie, wordt klikt u vervolgens deze schrijfbewerkingen volledig vastgelegd. Echter, als de pagina leeg is vervolgens het schrijven naar deze pagina wordt minimaal vastgelegd.

## <a name="optimizing-deletes"></a>Hiermee verwijdert u optimaliseren
`DELETE`is een volledig geregistreerde bewerking.  Als u verwijderen van een grote hoeveelheid gegevens in een tabel of een partitie wilt, is het vaak meer wel zinvol voor `SELECT` de gegevens die u behouden wilt, en die kunnen worden uitgevoerd als een minimaal geregistreerde bewerking.  Om dit te realiseren, maak een nieuwe tabel met [CTAS][CTAS].  Zodra u hebt gemaakt, gebruikt u [naam] [ RENAME] wisselen uit uw oude tabel met de nieuwe tabel.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Updates optimaliseren
`UPDATE`is een volledig geregistreerde bewerking.  Als u wilt bijwerken van een groot aantal rijen in een tabel of of een partitie kan vaak veel efficiënter om te gebruiken, zoals een minimaal geregistreerde bewerking [CTAS] [ CTAS] om dit te doen.

In het voorbeeld hieronder een volledige tabel update is geconverteerd naar een `CTAS` zodat minimale logboekregistratie mogelijk is.

In dit geval zijn er een korting achteraf toevoegen aan de verkoop in de tabel:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> Opnieuw maken van grote tabellen kan profiteren van beheerfuncties voor SQL Data Warehouse werkbelasting. Voor meer informatie raadpleegt u de sectie werkbelasting beheren in de [gelijktijdigheid] [ concurrency] artikel.
> 
> 

## <a name="optimizing-with-partition-switching"></a>Aan het overschakelen van de partitie optimaliseren
Als u bijvoorbeeld grote schaal wijzigingen in een [tabel partitie][table partition], en vervolgens een partitie overschakelen patroon een groot aantal zin maakt. Als de wijziging van gegevens belangrijk is en verspreid over meerdere partities, klikt u vervolgens bij gewoon iteratie van de partities hetzelfde resultaat bereikt.

De stappen voor het uitvoeren van een switch partitie zijn als volgt:

1. Maak een lege van de partitie
2. De 'update' uitvoeren als een CTAS
3. De bestaande gegevens-tabel activeren
4. Ga in de nieuwe gegevens
5. De gegevens opschonen

Echter, ter identificatie van de partities om over te schakelen we er eerst voor het bouwen van een procedure helper zoals hieronder. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id] 
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Deze procedure wordt gemaximaliseerd hergebruik van code en blijft de partitie voorbeeld compacter overschakelen.

De onderstaande code ziet u de vijf stappen die hierboven worden genoemd als u een volledige partitie routine overschakelen.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' TO [dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' TO [dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Logboekregistratie met kleine batches minimaliseren
Voor bewerkingen voor het wijzigen van grote hoeveelheden gegevens kan het zinvol zijn om te verdelen van de bewerking in segmenten of batches als bereik voor de werkeenheid.

Hieronder vindt u een voorbeeld van een werkende. De batchgrootte is ingesteld op een trivial getal de techniek markeren. In werkelijkheid zou de batchgrootte aanzienlijk groter worden. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Onderbreken en richtlijnen schalen
Azure SQL Data Warehouse kunt u onderbreken, hervatten en schalen van uw datawarehouse op aanvraag. Wanneer u onderbreken of schalen van uw SQL Data Warehouse is het belangrijk te weten dat onderweg transacties onmiddellijk; zijn beëindigd waardoor open transacties worden teruggezet. Als uw werkbelasting een wijziging van de gegevens lang actief is en niet voltooid voordat de bewerking onderbreken of schaal uitgegeven heeft, moet dit werk ongedaan worden gemaakt. Dit mogelijk invloed op de tijd die nodig zijn om te onderbreken of schalen van uw Azure SQL Data Warehouse-database. 

> [!IMPORTANT]
> Beide `UPDATE` en `DELETE` zijn volledig geregistreerde bewerkingen en zodat deze ongedaan maken/opnieuw bewerkingen kunnen aanzienlijk langer duren dan equivalent minimaal operations geregistreerd. 
> 
> 

De beste scenario is het toelaten vlucht wijziging gegevenstransacties voltooid voordat het onderbreken of schalen van SQL Data Warehouse. Maar kan dit niet altijd praktisch zijn. Overweeg een van de volgende opties om het risico van een lange terugdraaiactie heeft plaatsgevonden:

* Herschrijven langlopende bewerkingen met behulp van [CTAS][CTAS]
* De bewerking onderverdelen in stukken verdeeld; uitgevoerd op een subset van de rijen

## <a name="next-steps"></a>Volgende stappen
Zie [transacties in SQL Data Warehouse] [ Transactions in SQL Data Warehouse] voor meer informatie over de isolatieniveaus en transactionele limieten.  Zie voor een overzicht van de andere aanbevelingen [aanbevolen procedures van SQL Data Warehouse][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Transactions in SQL Data Warehouse]: ./sql-data-warehouse-develop-transactions.md
[table partition]: ./sql-data-warehouse-tables-partition.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

