---
title: Tabel selecteert (CTAS) in SQL Data Warehouse maken | Microsoft Docs
description: Tips voor het coderen van met de tabel maken als de instructie select (CTAS) in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 68ac9a94-09f9-424b-b536-06a125a653bd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: a885ba4f455fecd158696faaee38c83c1e4ec0bf
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Table As Select (CTAS) in SQL datawarehouse maken
Maken van de tabel als selecteren of `CTAS` vindt u een van de belangrijkste functies van T-SQL. Het is een volledig parallelized bewerking die u een nieuwe tabel op basis van de uitvoer van een SELECT-instructie maakt. `CTAS`is de snelste en eenvoudigste manier om een kopie van een tabel te maken. Dit document bevat zowel voorbeelden en aanbevolen procedures voor `CTAS`.

## <a name="selectinto-vs-ctas"></a>SELECTEREN... IN de vs. CTAS
U kunt overwegen `CTAS` Super bedrag versie van `SELECT..INTO`.

Hieronder volgt een voorbeeld van een eenvoudige `SELECT..INTO` instructie:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

In het bovenstaande voorbeeld `[dbo].[FactInternetSales_new]` zou worden gemaakt als ROUND_ROBIN gedistribueerde tabel met deze functie een door de GECLUSTERDE COLUMNSTORE-INDEX, omdat dit de standaardinstellingen van de tabel in Azure SQL Data Warehouse zijn.

`SELECT..INTO`echter kunt u niet te wijzigen van de distributiemethode of het indextype als onderdeel van de bewerking. Dit is wanneer `CTAS` van pas komt.

Converteren naar de bovenstaande `CTAS` is heel eenvoudig:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

Met `CTAS` kunt u zowel de distributie van gegevens in de tabel, evenals het tabeltype wijzigen. 

> [!NOTE]
> Als u alleen probeert te wijzigen van de index in uw `CTAS` bewerking en de brontabel is gedistribueerd-hash en vervolgens uw `CTAS` bewerking zal werken het beste als u de dezelfde kolom en gegevens Distributietype onderhouden. Dit voorkomt cross-distributie gegevensverplaatsing tijdens de bewerking die efficiënter is.
> 
> 

## <a name="using-ctas-to-copy-a-table"></a>Kopiëren van een tabel met behulp van CTAS
Mogelijk een van de meest voorkomende maakt gebruik van `CTAS` is een exemplaar maken van een tabel zodat u de DDL kunt wijzigen. Als u bijvoorbeeld uw tabel als oorspronkelijk maakte `ROUND_ROBIN` en nu wilt wijzigen in een tabel die wordt gedistribueerd in een kolom `CTAS` is hoe u de distributie-kolom wilt wijzigen. `CTAS`kan ook worden gebruikt om te partitioneren, te indexeren of kolom typen wijzigen.

Stel dat u deze tabel met het standaardtype voor de distributie van gemaakt `ROUND_ROBIN` gedistribueerd omdat er geen distributiepunt-kolom is opgegeven de `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nu u een nieuw exemplaar van deze tabel maken met een geclusterde Columnstore-Index, zodat u van de prestaties van de geclusterde Columnstore-tabellen profiteren kunt. U ook wilt distribueren in deze tabel op ProductKey omdat u de verbindingen in deze kolom zijn anticiperen en wilt voorkomen dat verplaatsing van gegevens tijdens joins op ProductKey. Tot slot wilt u ook voor het partitioneren op OrderDateKey zodat u snel kunt u oude gegevens verwijderen door het verwijderen van oude partities toevoegen. Hier volgt de CTAS-instructie waarmee uw oude tabel in een nieuwe tabel wilt kopiëren.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Ten slotte kunt u uw tabellen om het wisselen van een nieuwe tabel en zet de oude tabel wijzigen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> Azure SQL Data Warehouse bevat nog geen functionaliteit voor het automatisch maken of bijwerken van statistieken.  Voor optimale resultaten van uw query's is het belangrijk dat u statistieken maakt voor alle kolommen van alle tabellen nadat de gegevens voor het eerst zijn geladen of wanneer de gegevens substantieel zijn gewijzigd.  Zie het onderwerp [Statistieken][Statistics] in de groep onderwerpen voor ontwikkelaars voor gedetailleerde uitleg van statistieken.
> 
> 

## <a name="using-ctas-to-work-around-unsupported-features"></a>Met behulp van CTAS omzeilen niet-ondersteunde functies
`CTAS`kan ook worden gebruikt om een aantal van de onderstaande niet-ondersteunde functies te omzeilen. Dit kan vaak blijken een situatie win/win als het niet alleen uw code compatibel worden, maar dit wordt vaak sneller uitgevoerd op de SQL Data Warehouse. Dit is als gevolg van het ontwerp van de volledig parallelized. Scenario's die kunnen worden uitgevoerd om met CTAS omvatten:

* ANSI JOINS op UPDATEs
* ANSI-verbindingen in verwijderen
* MERGE-instructie

> [!NOTE]
> Probeer om na te denken ' CTAS eerste '. Als u denkt dat u kunt ook een probleem met oplossen `CTAS` vervolgens die wordt doorgaans de beste manier om deze - zelfs als u meer gegevens als gevolg hiervan worden geschreven.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI join vervanging voor update-instructies
Wellicht hebt u een complexe update waar lid van meer dan twee tabellen samen met ANSI-syntaxis toevoegen voor het uitvoeren van de UPDATE of DELETE.

Stel u moest u deze tabel bij te werken:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

De oorspronkelijke query kan hebt gezien ongeveer het volgende:

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Omdat SQL Data Warehouse biedt geen ondersteuning voor ANSI lid wordt de `FROM` -component van een `UPDATE` instructie, u kunt deze code via niet kopiëren zonder dat een enigszins gewijzigd.

U kunt een combinatie van een `CTAS` en een impliciete join ter vervanging van deze code:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI join vervanging voor delete-instructies
Soms de aanbevolen aanpak voor het verwijderen van gegevens is met `CTAS`. Selecteer de gegevens die u wilt behouden in plaats van de gegevens te verwijderen. Deze vooral voor `DELETE` instructies die gebruikmaken van ansi-syntaxis van de gekoppelde omdat SQL Data Warehouse biedt geen ondersteuning voor ANSI lid wordt in de `FROM` -component van een `DELETE` instructie.

Hieronder vindt u een voorbeeld van een geconverteerde DELETE-instructie:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Vervang merge-instructies
Merge-instructies kunnen worden vervangen, ten minste gedeeltelijk met behulp van `CTAS`. U kunt consolideren de `INSERT` en de `UPDATE` in één instructie. Verwijderde records moet in een tweede instructie worden afgesloten.

Een voorbeeld van een `UPSERT` hieronder:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS aanbeveling: status expliciet gegevenstype en null-waarden van uitvoer
Bij het migreren van code is het wellicht tegenkomen van dit type codering patroon:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinctief denkt u moet deze code migreren naar een CTAS en u onjuist zou zijn. Er is echter een verborgen probleem hier.

De volgende code is niet hetzelfde resultaat opleveren:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

U ziet dat de kolom 'resultaat' doorsturen waarden voor het type en de null-waarden van de expressie uitvoert. Dit kan leiden tot subtiele verschillen in de waarden niet.

Probeer het volgende voorbeeld:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

De waarde die is opgeslagen voor resultaat verschilt. Zoals de persistente waarde in de kolom met resultaten wordt gebruikt in andere expressies wordt de fout nog meer belangrijke.

![][1]

Dit is vooral belangrijk voor migraties van gegevens. Er is een probleem ondanks dat de tweede query weliswaar nauwkeuriger is. De gegevens zou afwijken vergeleken met het bronsysteem en die leidt tot vragen van de integriteit van de migratie. Dit is een van deze zeldzame gevallen waarbij het 'verkeerde' antwoord daadwerkelijk het juiste is!

De reden zien we deze verschillen tussen de twee resultaten wordt naar beneden op het impliciete type casten. De tabel definieert in het eerste voorbeeld de kolomdefinitie. Een impliciete conversie treedt op wanneer de rij wordt ingevoegd. In het tweede voorbeeld is het geen conversie van het impliciete type als de expressie gegevenstype van de kolom definieert. U ziet ook dat de kolom in het tweede voorbeeld is gedefinieerd als een kolom waarvoor null is toegestaan dat in het eerste voorbeeld dit niet is. Wanneer de tabel is gemaakt in het eerste voorbeeld kolom nullability is expliciet gedefinieerd. In het tweede zou voorbeeld van die het bericht is zojuist hebt achtergelaten met de expressie en standaard dit leiden tot een NULL-definitie.  

Deze problemen oplossen moet u expliciet instellen de typeconversie en de null-waarden in de `SELECT` gedeelte van de `CTAS` instructie. U kunt deze eigenschappen niet instellen in het gedeelte van de tabel maken.

Het volgende voorbeeld laat zien hoe de code vast:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Houd rekening met het volgende:

* CAST of CONVERT kan zijn gebruikt
* ISNULL wordt gebruikt om af te dwingen van null-waarden niet COALESCE
* ISNULL is de buitenste functie
* Het tweede gedeelte van de ISNULL is een constante bijv. 0

> [!NOTE]
> Voor de null-waarden zijn correct ingesteld is het essentieel om te gebruiken `ISNULL` en niet `COALESCE`. `COALESCE`is niet een deterministische functie en het resultaat van de expressie wordt dus altijd worden Null-waarden bevatten. `ISNULL`verschilt. Het is deterministisch. Daarom wanneer het tweede gedeelte van de `ISNULL` functie is een constante zijn of een letterlijke waarde en vervolgens de resulterende waarde is NOT NULL.
> 
> 

Deze tip is niet alleen nuttig voor het garanderen van de integriteit van de berekeningen. Het is ook belangrijk voor het overschakelen van de tabel-partitie. Stel, dat u hebt deze tabel die is gedefinieerd als uw fact:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Het waardeveld is echter een berekende-expressie is niet deel uitmaakt van de brongegevens.

Als u wilt maken van uw gepartitioneerde gegevensset is het raadzaam om dit te doen:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

De query geen bezwaar uitgevoerd. Het probleem wordt geleverd wanneer u probeert uit te voeren van de partitie-switch. De tabeldefinities komen niet overeen. Als u wilt dat de tabeldefinities overeen met de CTAS moet worden gewijzigd.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

U kunt daarom zien type consistentie en onderhouden van null-waarde bevat eigenschappen van een CTAS wordt goed engineering aanbevolen. Het helpt integriteit van uw berekeningen te handhaven en zorgt er ook voor dat het overschakelen van de partitie mogelijk is.

Zie MSDN voor meer informatie over het gebruik van [CTAS][CTAS]. Er is een van de belangrijkste instructies in Azure SQL Data Warehouse. Zorg ervoor dat grondig begrijpen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
