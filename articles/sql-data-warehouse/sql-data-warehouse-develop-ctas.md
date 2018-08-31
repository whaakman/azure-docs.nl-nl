---
title: TABLE AS SELECT (CTAS) maken in Azure SQL datawarehouse | Microsoft Docs
description: Tips voor het coderen met de instructie CREATE TABLE AS SELECT (CTAS) in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: dad0b1570f54cde1b1d474d8ebfc78f793724ef4
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43247750"
---
# <a name="using-create-table-as-select-ctas-in-azure-sql-data-warehouse"></a>Met behulp van CREATE TABLE AS SELECT (CTAS) in Azure SQL datawarehouse
Tips voor het coderen met de instructie CREATE TABLE AS SELECT (CTAS) T-SQL in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="what-is-create-table-as-select-ctas"></a>Wat is CREATE TABLE AS SELECT (CTAS)?

De [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) of CTAS-instructie is een van de belangrijkste T-SQL-functies beschikbaar. Dit is een parallelle bewerking die u een nieuwe tabel op basis van de uitvoer van een instructie SELECT maakt. CTASD is de snelste en eenvoudigste manier om een kopie van een tabel te maken. 

## <a name="selectinto-vs-ctas"></a>SELECTEREN... IN de vs. CTAS
U kunt overwegen CTAS te maken met zeer in rekening gebracht versie van de [selecteren... IN](/sql/t-sql/queries/select-into-clause-transact-sql) instructie.

Hieronder volgt een voorbeeld van een eenvoudige SELECT... IN:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

In het voorgaande voorbeeld `[dbo].[FactInternetSales_new]` is als ROUND_ROBIN gedistribueerde tabel met een GECLUSTERDE COLUMNSTORE-INDEX worden gemaakt omdat dit de standaardinstellingen van de tabel in Azure SQL Data Warehouse.

SELECTEREN... IN, maar kunt u niet te wijzigen van de methode voor het distribueren of het indextype als onderdeel van de bewerking. Dit is waar CTAS is beschikbaar in.

Als u wilt converteren van het vorige voorbeeld in op CTAS is vrij eenvoudig:

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

Met CTAS bent u kunt zowel de distributie van gegevens in de tabel, evenals het tabeltype wijzigen. 

> [!NOTE]
> Als u alleen wilt wijzigen van de index in uw `CTAS` bewerking en de brontabel is gedistribueerd hash vervolgens uw `CTAS` bewerking beste wordt uitgevoerd of onderhouden van de dezelfde distributie kolom en het gegevenstype. Dit voorkomt dat cross-distributie gegevensverplaatsing tijdens de bewerking die efficiënter is.
> 
> 

## <a name="using-ctas-to-copy-a-table"></a>Een tabel kopiëren met behulp van CTAS
Misschien een van de meest voorkomende maakt gebruik van `CTAS` maakt een kopie van een tabel zodat u kunt de DDL wijzigen. Als u bijvoorbeeld een tabel als oorspronkelijk gemaakt `ROUND_ROBIN` en nu wilt wijzigen in een tabel die is gedistribueerd op een kolom, `CTAS` is hoe zou u de distributiekolom wijzigen. `CTAS` kan ook worden gebruikt om te wijzigen van typen partitionering, indexering of kolom.

Stel dat u hebt gemaakt in deze tabel met behulp van het standaardtype voor distributie van `ROUND_ROBIN` gedistribueerd omdat er is geen distributiekolom is opgegeven de `CREATE TABLE`.

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

Nu u een nieuwe kopie van deze tabel maken met een geclusterde Columnstore-Index, zodat u van de prestaties van de geclusterde Columnstore-tabellen profiteren kunt. U ook wilt distribueren van deze tabel op ProductKey omdat u verbindingen in deze kolom zijn planningscyclus en om te voorkomen dat de verplaatsing van gegevens tijdens joins op ProductKey. Tot slot wilt u ook om toe te voegen partitioneren op OrderDateKey zodat u snel oude gegevens verwijderen kunt door het verwijderen van oude partities. Hier is de CTAS-instructie waarmee uw oude tabel in een nieuwe tabel wilt kopiëren.

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

Ten slotte kunt u uw tabellen om te wisselen in uw nieuwe tabel en zet uw oude tabel wijzigen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> Azure SQL Data Warehouse bevat nog geen functionaliteit voor het automatisch maken of bijwerken van statistieken.  Voor optimale resultaten van uw query's is het belangrijk dat u statistieken maakt voor alle kolommen van alle tabellen nadat de gegevens voor het eerst zijn geladen of wanneer de gegevens substantieel zijn gewijzigd.  Zie het onderwerp [Statistics] [Statistics] in de groep onderwerpen voor een gedetailleerde uitleg van statistieken.
> 
> 

## <a name="using-ctas-to-work-around-unsupported-features"></a>Met behulp van CTAS omzeilen niet-ondersteunde functies
CTAS kan ook worden gebruikt om een aantal van de niet-ondersteunde functies in de lijst hieronder te omzeilen. Dit kan vaak bewijzen moet een situatie win/win niet alleen uw code is compatibel, maar dit wordt vaak sneller uitgevoerd op SQL Data Warehouse. Dit is als gevolg van het ontwerp van de volledig geparallelliseerde. Scenario's die kunnen worden gewerkt om CTAS zijn:

* ANSI-join op UPDATEs
* ANSI-join op verwijderen
* MERGE-instructie

> [!NOTE]
> Probeer om na te denken "CTAS eerste '. Als u denkt dat u kunt oplossen door een probleem met `CTAS` vervolgens die is doorgaans de beste manier om het - benadering, zelfs als u meer gegevens als gevolg hiervan worden geschreven.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>Vervanging van ANSI-join voor update-instructies
U merkt u misschien hebt u een complexe update die lid wordt van meer dan twee tabellen samen met ANSI-syntaxis toevoegen voor het uitvoeren van de UPDATE of DELETE.

Stel dat u had om bij te werken in deze tabel:

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

De oorspronkelijke query kan hebt gezien er ongeveer als volgt:

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

Omdat SQL Data Warehouse biedt geen ondersteuning voor ANSI joins in de `FROM` -component van een `UPDATE` -instructie kan niet u deze code via kopiëren zonder deze iets wijzigt.

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

## <a name="ansi-join-replacement-for-delete-statements"></a>Vervanging van ANSI-join voor het verwijderen van instructies
Soms de aanbevolen aanpak voor het verwijderen van gegevens is het gebruik van `CTAS`. Selecteer de gegevens die u wilt behouden in plaats van de gegevens te verwijderen. Deze vooral van toepassing voor `DELETE` statements die gebruikmaken van ansi lid te worden syntaxis omdat SQL Data Warehouse biedt geen ondersteuning voor ANSI joins in de `FROM` -component van een `DELETE` instructie.

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

## <a name="replace-merge-statements"></a>Vervang de instructies voor samenvoegen
De instructies voor samenvoegen worden vervangen, ten minste deel, met behulp van CTAS. U kunt de INSERT- en de UPDATE samenvoegen in één instructie. Verwijderde records moeten worden gesloten uit in een tweede instructie.

Hier volgt een voorbeeld van een UPSERT:

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

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS aanbeveling: expliciet status gegevenstype en de null-waarden van uitvoer
Bij het migreren van code vindt u mogelijk dat u uitvoeren voor dit type codering patroon:

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

Instinctief denkt u moet deze code migreren naar een CTAS, terwijl u juist. Er is echter een verborgen probleem hier.

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

U ziet dat de kolom "resultaat" doorsturen aan het type en de null-waarde bevat waarden van de expressie uitvoert. Dit kan leiden tot subtiele verschillen in waarden als u niet oplet.

Probeer het volgende voorbeeld:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

De waarde die is opgeslagen voor resultaat verschilt. Als de persistente waarde in de kolom met resultaten wordt gebruikt in andere expressies wordt de fout nog belangrijker.

![CTAS resultaten](media/sql-data-warehouse-develop-ctas/ctas-results.png)

Dit is vooral belangrijk voor migraties. Zelfs als de tweede query weliswaar nauwkeuriger is is er een probleem. De gegevens is anders in vergelijking met het bronsysteem en die leidt tot vragen van de integriteit van de migratie. Dit is een van deze zeldzame gevallen waarbij het "onjuist" antwoord feitelijk de juiste is is.

De reden dat we deze verschillen tussen de twee resultaten zien is op impliciete type casten. De tabel definieert in het eerste voorbeeld de kolomdefinitie. Een impliciete conversie vindt plaats wanneer de rij wordt ingevoegd. In het tweede voorbeeld is het geen impliciete conversie omdat het gegevenstype van de kolom definieert. U ziet ook dat de kolom in het tweede voorbeeld is gedefinieerd als een kolom met Null-waarden dat in het eerste voorbeeld het niet is. Wanneer de tabel is gemaakt in het eerste voorbeeld van de kolom null-waarden is expliciet is gedefinieerd. In het tweede resulteert voorbeeld die het is net links op de expressie en standaard dit in een NULL-definitie.  

Om deze problemen te verhelpen moet u expliciet de typeconversie van het en de null-waarden in de SELECT-gedeelte van de instructie CTAS instellen. U kunt deze eigenschappen niet instellen in het gedeelte van de tabel maken.

In het volgende voorbeeld ziet u hoe u de code repareert:

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
* ISNULL wordt gebruikt om af te dwingen van null-waarden niet samenvoegen
* ISNULL is de buitenste functie
* Het tweede gedeelte van de ISNULL is een constante bijv. 0

> [!NOTE]
> Is het essentieel om ISNULL- en niet samenvoegen te gebruiken voor de null-waarden correct zijn ingesteld. SAMENVOEGEN is niet een deterministische functie en het resultaat van de expressie wordt dus altijd worden Null-waarden bevatten. ISNULL komt niet overeen. Het is deterministisch. Daarom wanneer het tweede gedeelte van de functie ISNULL is een constante zijn of een letterlijke waarde en vervolgens de resulterende waarde is NOT NULL.
> 
> 

Deze tip is niet alleen handig om ervoor te zorgen dat de integriteit van de berekeningen. Het is ook belangrijk om te schakelen van de tabel-partitie. Stel, dat u hebt deze tabel die is gedefinieerd als uw feit:

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

Het waardeveld is echter een berekende expressie die het maakt geen deel uit van de brongegevens.

Voor het maken van uw gepartitioneerde gegevensset kunt u om dit te doen:

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

De query wordt uitgevoerd geen bezwaar. Het probleem wordt geleverd wanneer u probeert uit te voeren van de partitie-switch. De tabeldefinities komen niet overeen. Moet worden gewijzigd zodat de tabeldefinities overeen met de CTAS.

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

U kunt daarom zien dat type consistentie en onderhouden van null-waarde bevat eigenschappen van een CTAS is een goede technische aanbevolen procedure. Dit zorgt ervoor dat de integriteit in uw berekeningen te behouden en zorgt er ook voor dat het overschakelen van de partitie mogelijk is.

Raadpleeg de [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) documentatie. Er is een van de belangrijkste instructies in Azure SQL Data Warehouse. Zorg ervoor dat deze grondig inzicht.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

