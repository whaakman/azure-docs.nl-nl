---
title: TABLE AS SELECT (CTAS) maken in Azure SQL datawarehouse | Microsoft Docs
description: Uitleg over en voorbeelden van de instructie CREATE TABLE AS SELECT (CTAS) in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/26/2019
ms.author: mlee3gsd
ms.reviewer: jrasnick
ms.custom: seoapril2019
ms.openlocfilehash: c8e9f3ccdfaee64f75443f6a4eb89a3df7c48b0e
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680091"
---
# <a name="create-table-as-select-ctas-in-azure-sql-data-warehouse"></a>CREATE TABLE AS SELECT (CTAS) in Azure SQL Data Warehouse

In dit artikel wordt uitgelegd van de instructie CREATE TABLE AS SELECT (CTAS) T-SQL in Azure SQL Data Warehouse om oplossingen te ontwikkelen. Dit artikel bevat ook voorbeelden van code.

## <a name="create-table-as-select"></a>CREATE TABLE AS SELECT

De [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) (CTAS)-instructie is een van de belangrijkste T-SQL-functies beschikbaar. CTAS is een parallelle bewerking die u een nieuwe tabel op basis van de uitvoer van een instructie SELECT maakt. CTAS is de snelste en eenvoudigste manier om te maken en gegevens invoegen in een tabel met één opdracht.

## <a name="selectinto-vs-ctas"></a>SELECTEREN... IN de vs. CTAS

CTAS is een meer aanpasbare versie van de [selecteren... IN](/sql/t-sql/queries/select-into-clause-transact-sql) instructie.

Hier volgt een voorbeeld van een eenvoudige SELECT... IN:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

SELECTEREN... IN kunt u niet wijzigen van de methode voor het distribueren of het indextype als onderdeel van de bewerking. U maakt `[dbo].[FactInternetSales_new]` met behulp van het standaardtype voor distributie van ROUND_ROBIN, en de standaard-tabelstructuur van GECLUSTERDE COLUMNSTORE-INDEX.

Met CTAS, aan de andere kant kunt u zowel de distributie van gegevens in de tabel, evenals het tabeltype van de structuur. Het vorige voorbeeld in op CTAS converteren:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
   ,CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [!NOTE]
> Als u alleen probeert te wijzigen van de index in de CTAS-bewerking en de brontabel gedistribueerd hash is, behoudt u de dezelfde distributie kolom en het gegevenstype. Hiermee voorkomt u cross-verdeling gegevensverplaatsing tijdens de bewerking efficiënter is.

## <a name="use-ctas-to-copy-a-table"></a>CTAS gebruiken om te kopiëren van een tabel

Een van de meest voorkomende gebruik van CTAS maakt bijvoorbeeld een kopie van een tabel om te wijzigen van de DDL. Laten we zeggen dat u oorspronkelijk hebt gemaakt uw tabel als `ROUND_ROBIN`, en nu wilt wijzigen in een tabel die is gedistribueerd op een kolom. CTAS is hoe zou u de distributiekolom wijzigen. U kunt ook CTAS typen partitionering, indexering of kolom wijzigen.

Laten we zeggen dat u deze tabel gemaakt met behulp van het standaardtype voor distributie van `ROUND_ROBIN`, niet op te geven van een distributiekolom in de `CREATE TABLE`.

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

Nu u maken van een nieuwe kopie van deze tabel wilt, met een `Clustered Columnstore Index`, zodat u van de prestaties van de geclusterde Columnstore-tabellen profiteren kunt. Wilt u ook deze tabel distributie op `ProductKey`, omdat u bent anticiperen op verbindingen in deze kolom en om te voorkomen dat verplaatsing van gegevens tijdens joins op `ProductKey`. Ten slotte wordt u ook wilt toevoegen partitioneren op `OrderDateKey`, zodat u snel oude gegevens verwijderen kunt door het verwijderen van oude partities. Hier volgt de CTAS-instructie, die uw oude tabel in een nieuwe tabel opgehaald.

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

Ten slotte kunt u uw tabellen, om te wisselen in uw nieuwe tabel en zet uw oude tabel wijzigen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

## <a name="use-ctas-to-work-around-unsupported-features"></a>CTAS gebruiken om te werken om niet-ondersteunde functies

U kunt ook CTAS gebruiken om een aantal van de niet-ondersteunde functies in de lijst hieronder te omzeilen. Deze methode kan vaak bewijzen handig, omdat u niet alleen uw code compatibel is, maar dit wordt vaak sneller uitgevoerd op SQL Data Warehouse. Deze prestaties is een resultaat van het ontwerp van de volledig geparallelliseerde. Scenario's omvatten:

* ANSI-join op UPDATEs
* ANSI-join op verwijderen
* MERGE-instructie

> [!TIP]
> Probeer om na te denken "CTAS eerste." Het oplossen van een probleem met behulp van CTAS is meestal een goede benadering, zelfs als u meer gegevens als gevolg hiervan schrijft.

## <a name="ansi-join-replacement-for-update-statements"></a>Vervanging van ANSI-join voor update-instructies

U vindt u misschien dat u een complexe update hebt. De update worden meer dan twee tabellen samen gekoppeld met behulp van de ANSI-syntaxis om uit te voeren van de UPDATE of DELETE.

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

De oorspronkelijke query mogelijk iets zoals in dit voorbeeld hebt gezien:

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

SQL Data Warehouse biedt geen ondersteuning voor ANSI joins in de `FROM` -component van een `UPDATE` -instructie, zodat u kunt het vorige voorbeeld niet gebruiken zonder het te wijzigen.

U kunt een combinatie van een CTAS en een impliciete join gebruiken en vervang het vorige voorbeeld:

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

Soms de aanbevolen aanpak voor het verwijderen van gegevens is het gebruik van CTAS, met name voor `DELETE` statements die gebruikmaken van ANSI join-syntaxis. Dit komt doordat SQL Data Warehouse biedt geen ondersteuning voor ANSI joins in de `FROM` -component van een `DELETE` instructie. In plaats van de gegevens te verwijderen, selecteert u de gegevens die u wilt behouden.

Hieronder volgt een voorbeeld van een geconverteerde `DELETE` instructie:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you want to keep
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

U kunt de instructies voor samenvoegen, ten minste vervangen deel, met behulp van CTAS. U kunt combineren de `INSERT` en de `UPDATE` in één instructie. Verwijderde records moeten worden beperkt in de `SELECT` instructie moeten worden weggelaten uit de resultaten.

Het volgende voorbeeld is voor een `UPSERT`:

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
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];
```

## <a name="explicitly-state-data-type-and-nullability-of-output"></a>Status die expliciet gegevenstype en de null-waarden van uitvoer

Bij het migreren van code, vindt u mogelijk dat u uitvoeren voor dit type codering patroon:

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

U denkt moet u deze code migreren naar CTAS, en u weet juist zijn. Er is echter een verborgen probleem hier.

De volgende code wordt niet hetzelfde resultaat opleveren:

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

U ziet dat de kolom "resultaat" doorsturen aan het type en de null-waarde bevat waarden van de expressie uitvoert. Uitvoering van de gegevens kan naar voren type leiden tot subtiele verschillen in waarden als u niet oplet.

Probeer het volgende voorbeeld:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

De waarde die is opgeslagen voor resultaat verschilt. Als de persistente waarde in de kolom met resultaten wordt gebruikt in andere expressies, wordt de fout nog belangrijker.

![Schermafbeelding van CTAS resultaten](media/sql-data-warehouse-develop-ctas/ctas-results.png)

Dit is belangrijk voor migraties. Hoewel de tweede query weliswaar nauwkeurigere is, is er een probleem. De gegevens is anders in vergelijking met het bronsysteem en die leidt tot vragen van de integriteit van de migratie. Dit is een van deze zeldzame gevallen waarbij het "onjuist" antwoord feitelijk de juiste is is.

De reden zien we een verschillen tussen de twee resultaten is vanwege een impliciet type casten. De tabel definieert in het eerste voorbeeld de kolomdefinitie. Wanneer de rij wordt ingevoegd, treedt er een impliciete conversie op. In het tweede voorbeeld is het geen impliciete conversie omdat het gegevenstype van de kolom wordt gedefinieerd door de expressie.

U ziet ook dat de kolom in het tweede voorbeeld is gedefinieerd als een kolom, terwijl in het eerste voorbeeld het niet is. Wanneer de tabel is gemaakt in het eerste voorbeeld, is null-waarde bevat kolom expliciet is gedefinieerd. In het tweede voorbeeld, het is van links naar de expressie en standaard zou leiden tot een NULL-definitie.

Als u wilt oplossen, moet u expliciet de typeconversie van het en de null-waarden in de SELECT-gedeelte van de instructie CTAS instellen. U kunt deze eigenschappen niet instellen in de tabel maken.
Het volgende voorbeeld ziet u hoe u de code repareert:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Houd rekening met het volgende:

* U kunt CAST of converteren.
* Gebruik ISNULL, niet samenvoegen, om af te dwingen van null-waarde bevat. Zie de opmerking hieronder.
* ISNULL is de buitenste functie.
* Het tweede gedeelte van de ISNULL is een constante, 0.

> [!NOTE]
> Voor de null-waarden correct zijn ingesteld, is het essentieel om het gebruik van ISNULL- en niet samenvoegen. SAMENVOEGEN is niet een deterministische functie en het resultaat van de expressie wordt dus altijd worden Null-waarden bevatten. ISNULL komt niet overeen. Het is deterministisch. Dus als het tweede gedeelte van de functie ISNULL een constante zijn of een letterlijke waarde is de resulterende waarde is NOT NULL.

Ervoor te zorgen dat de integriteit van de berekeningen is ook belangrijk om te schakelen van de tabel-partitie. Stel, dat u hebt deze tabel die is gedefinieerd als een feitentabel:

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

Het bedragveld is echter een berekende-expressie. Het is onderdeel van de brongegevens niet.

Voor het maken van uw gepartitioneerde gegevensset, kunt u de volgende code gebruiken:

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

De query zou perfect goed uitgevoerd. Het probleem wordt geleverd wanneer u probeert uit te voeren van de partitie-switch. De tabeldefinities komen niet overeen. Om de tabeldefinities overeen, wijzigt u de CTAS toe te voegen een `ISNULL` functie voor het behouden van de kolom-kenmerk voor null-waarde bevat.

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

U kunt zien dat type consistentie en onderhouden van null-waarde bevat eigenschappen van een CTAS is een technische best practice om. Het helpt om integriteit in uw berekeningen te behouden en zorgt er ook voor dat het overschakelen van de partitie mogelijk is.

CTAS is een van de belangrijkste instructies in SQL Data Warehouse. Zorg ervoor dat deze grondig inzicht. Zie de [CTAS documentatie](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer tips voor ontwikkelaars de [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

