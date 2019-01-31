---
title: Transacties voor Azure SQL Data Warehouse optimaliseren | Microsoft Docs
description: Informatie over het optimaliseren van de prestaties van uw transactionele code in Azure SQL Data Warehouse bij het minimaliseren van risico's voor lange terugdraaiacties.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/19/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: f5e0b2b75ac111f3221108936f84e5883aebfc1a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55476261"
---
# <a name="optimizing-transactions-in-azure-sql-data-warehouse"></a>Transacties in Azure SQL Data Warehouse optimaliseren
Informatie over het optimaliseren van de prestaties van uw transactionele code in Azure SQL Data Warehouse bij het minimaliseren van risico's voor lange terugdraaiacties.

## <a name="transactions-and-logging"></a>Transacties en logboekregistratie
Transacties zijn een belangrijk onderdeel van een relationele database-engine. SQL Data Warehouse maakt gebruik van transacties tijdens het aanpassen van gegevens. Deze transacties kunnen worden expliciete of impliciete. Één INSERT, UPDATE en DELETE-instructies zijn alle voorbeelden van de impliciete transacties. Expliciete transacties BEGIN TRAN, TRAN doorvoeren of TERUGDRAAIEN TRAN gebruiken. Expliciete transacties worden meestal gebruikt wanneer meerdere wijziging instructies moeten samen in één atomische eenheid worden gekoppeld. 

Azure SQL Data Warehouse wijzigingen worden doorgevoerd in de database met behulp van transactielogboeken. Elke distributie heeft een eigen transactielogboek. Transaction logboekschrijfbewerkingen worden automatisch uitgevoerd. Er is geen configuratie nodig. Tijdens dit proces zorgt ervoor dat de schrijfbewerking deze overhead introduceren in het systeem. U kunt deze gevolgen minimaliseren door transactioneel efficiënte code te schrijven. Transactioneel efficiënte code valt breed in twee categorieën.

* Minimale constructies indien mogelijk logboekregistratie van gebruik
* Gegevens verwerken met behulp van binnen het bereik van batches om te voorkomen dat de rapportgebruiker langlopende transacties
* Een partitie overschakelen patroon voor grote wijzigingen in een bepaalde partitie vaststellen

## <a name="minimal-vs-full-logging"></a>Minimale versus volledige logboekregistratie
In tegenstelling tot volledig geregistreerde bewerkingen, die gebruikmaken van het transactielogboek om alle wijzigingen die rij bij te houden, minimaal geregistreerde bewerkingen mate toewijzingen, en alleen de metagegevens voor wijzigingen bijhouden van. Minimale logboekregistratie moet daarom logboekregistratie alleen de informatie die nodig is voor het terugdraaien van de transactie na een storing, of voor een expliciete aanvraag (TERUGDRAAIEN TRAN). Omdat veel minder gegevens worden bijgehouden in het transactielogboek, wordt een minimaal geregistreerde bewerking beter dan een vergelijkbaar formaat volledig geregistreerde bewerking wordt uitgevoerd. Bovendien, omdat minder schrijft het transactielogboek, een veel kleinere hoeveelheid logboekgegevens wordt gegenereerd en dus meer i/o efficiënt.

De limieten van de veiligheid transactie zijn alleen van toepassing op volledig geregistreerde bewerkingen.

> [!NOTE]
> Minimaal geregistreerde bewerkingen kunnen deelnemen aan expliciete transacties. Als u alle wijzigingen in de toewijzing van structuren worden bijgehouden, is het mogelijk om terug te draaien minimaal geregistreerde bewerkingen. 
> 
> 

## <a name="minimally-logged-operations"></a>Minimaal geregistreerde bewerkingen
De volgende bewerkingen zijn geschikt voor minimaal worden geregistreerd:

* CREATE TABLE AS SELECT ([CTAS](sql-data-warehouse-develop-ctas.md))
* INSERT..SELECT
* INDEX MAKEN
* ALTER INDEX OPNIEUW OPBOUWEN
* INDEX VERWIJDEREN
* TRUNCATE TABLE
* TABEL VERWIJDEREN
* ALTER TABLE SWITCH PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> Interne bewerkingen voor gegevensverplaatsing (zoals UITZENDING en SHUFFLE) worden niet beïnvloed door de Transactielimiet veiligheid.
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>Minimale logboekregistratie met bulksgewijs laden
CTAS en invoegen... Selecteer zijn beide bewerkingen van bulksgewijs laden. Echter, beide worden beïnvloed door de definitie van de doel-tabel en afhankelijk van de load-scenario. De volgende tabel wordt uitgelegd wanneer bulkbewerkingen worden volledig of minimaal geregistreerd:  

| Primaire Index | Belasting-Scenario | Modus voor logboekregistratie |
| --- | --- | --- |
| Heap |Alle |**Minimal** |
| Geclusterde Index |Lege doeltabel |**Minimal** |
| Geclusterde Index |Geladen rijen niet overlappen met bestaande pagina's in het doel |**Minimal** |
| Geclusterde Index |Geladen rijen overlappen met bestaande pagina's in het doel |Volledig |
| Geclusterde Columnstore-Index |Batchgrootte > = 102,400 per partitie uitgelijnd distributie |**Minimal** |
| Geclusterde Columnstore-Index |Batch-grootte < 102,400 per partitie uitgelijnd distributie |Volledig |

Het is vermelden waard schrijfbewerkingen om bij te werken van secundaire of niet-geclusterde indexen worden altijd volledig geregistreerde bewerkingen.

> [!IMPORTANT]
> SQL Data Warehouse heeft 60 distributies. Daarom, aangenomen dat alle rijen gelijkmatig worden gedistribueerd en terechtkomen in één partitie, uw batch moet 6,144,000 rijen bevatten of groter minimaal zijn aangemeld bij het schrijven naar een geclusterde Columnstore-Index. Als de tabel is gepartitioneerd en de rijen worden ingevoegd grenzen van partities omvatten, moet u 6,144,000 rijen per partitie grens ervan uitgaande dat het zelfs de verdeling van de gegevens. Elke partitie in elke distributie moet onafhankelijk groter zijn dan de drempelwaarde 102.400 rij voor de insert moeten minimaal worden vastgelegd in de distributie.
> 
> 

Het laden van gegevens naar een niet-lege tabel met een geclusterde index, kan een combinatie van rijen volledig geregistreerd en minimaal geregistreerde vaak bevatten. Een geclusterde index is een evenwichtige structuur (b-tree) van pagina's. Als de pagina wordt geschreven naar bevat al rijen uit een andere transactie, worden klikt u vervolgens deze schrijfbewerkingen volledig geregistreerd. Echter, als de pagina leeg is vervolgens het schrijven naar die pagina worden minimaal geregistreerd.

## <a name="optimizing-deletes"></a>Verwijderingen optimaliseren
VERWIJDEREN is een volledig geregistreerde bewerking.  Als u verwijderen van een grote hoeveelheid gegevens in een tabel of een partitie wilt, het vaak zinvol meer om te `SELECT` de gegevens die u behouden wilt, en die kunnen worden uitgevoerd als een minimaal geregistreerde bewerking.  Als u wilt de gegevens selecteert, maakt u een nieuwe tabel met [CTAS](sql-data-warehouse-develop-ctas.md).  Zodra u hebt gemaakt, gebruikt u [naam](/sql/t-sql/statements/rename-transact-sql) aan uw oude tabel met de zojuist gemaakte tabel vervangen.

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
Er is een volledig geregistreerde bewerking in de UPDATE.  Als u nodig hebt om bij te werken van een groot aantal rijen in een tabel of een partitie, dit kan vaak worden veel efficiënter te gebruiken, zoals een minimaal geregistreerde bewerking [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) om dit te doen.

In het voorbeeld hieronder een volledige tabel is bijwerken geconverteerd naar een CTAS zodat minimale logboekregistratie mogelijk is.

In dit geval zijn er een korting retroactief toegevoegd aan de verkoop in de tabel:

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
> Grote tabellen opnieuw te maken kan profiteren van beheerfuncties voor SQL Data Warehouse workload. Zie voor meer informatie, [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).
> 
> 

## <a name="optimizing-with-partition-switching"></a>Optimaliseren met partitie overschakelen
Als u momenteel geconfronteerd met grootschalige wijzigingen binnen een [partitie tabel](sql-data-warehouse-tables-partition.md), en vervolgens een schakelen tussen patroon partities zinvol. Als de wijziging van de gegevens belangrijk is en meerdere partities omvat, geven klikt u vervolgens iteratie van de partities hetzelfde resultaat.

De stappen voor het uitvoeren van een partitie-switch zijn als volgt:

1. Maak een lege partitie
2. De 'update' uitvoeren als een CTAS
3. Overschakelen van de bestaande gegevens naar de tabel
4. Ga in de nieuwe gegevens
5. De gegevens opschonen

Maar om te identificeren van de partities om over te schakelen, maken de volgende Help-procedure.  

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

Deze procedure wordt gemaximaliseerd hergebruik van code en blijft de partitie voorbeeld meer compacte overschakelen.

De volgende code ziet u de stappen die eerder is vermeld voor een volledige partitie routine overschakelen.

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
Voor bewerkingen voor het wijzigen van grote hoeveelheden gegevens, kan het zinvol zijn de bewerking in segmenten of batch-bestanden om het bereik van de eenheid van het werk te verdelen.

Een volgende code is een werkvoorbeeld van een. De batchgrootte is ingesteld op een trivial getal dat de techniek. In werkelijkheid, zou de batchgrootte aanzienlijk groter zijn. 

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

## <a name="pause-and-scaling-guidance"></a>Onderbreken en schalen van richtlijnen
Azure SQL Data Warehouse kunt u [onderbreken, hervatten en schalen](sql-data-warehouse-manage-compute-overview.md) uw datawarehouse op aanvraag. Wanneer u onderbreken of schalen van uw SQL Data Warehouse, is het belangrijk om te begrijpen dat eventuele actieve transacties onmiddellijk; worden beëindigd zorgt ervoor dat alle open transacties worden teruggedraaid. Als uw werkbelasting een wijziging van de gegevens lang actief is en niet voltooid voordat de bewerking onderbreken of schalen uitgegeven heeft, klikt u vervolgens moet dit werk ongedaan worden gemaakt. Deze ongedaan te maken heeft mogelijk invloed op de tijd die nodig zijn om te onderbreken of schalen van uw Azure SQL Data Warehouse-database. 

> [!IMPORTANT]
> Beide `UPDATE` en `DELETE` zijn volledig geregistreerde bewerkingen en zodat deze ongedaan maken/opnieuw bewerkingen kunnen aanzienlijk langer duren dan equivalent minimaal operations geregistreerd. 
> 
> 

De beste scenario is het flight data wijziging transacties voltooid voordat het onderbreken of schalen van SQL Data Warehouse te laten. Echter, in dit scenario altijd mogelijk niet praktisch. Als u wilt het risico van een lange terugdraaiactie, houd rekening met een van de volgende opties:

* Herschrijven langlopende bewerkingen met behulp van [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)
* De bewerking opdelen in segmenten; besturingssysteem op een subset van de rijen

## <a name="next-steps"></a>Volgende stappen
Zie [transacties in SQL Data Warehouse](sql-data-warehouse-develop-transactions.md) voor meer informatie over isolatieniveaus van en transactionele limieten.  Zie voor een overzicht van andere Best Practices, [aanbevolen procedures voor SQL Data Warehouse](sql-data-warehouse-best-practices.md).

