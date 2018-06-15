---
title: Groep wordt gebruikt door de opties in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van de groep door de opties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 0548983df23b158385783ac777b23268b5ac7d01
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526043"
---
# <a name="group-by-options-in-sql-data-warehouse"></a>Groeperen op opties in SQL Data Warehouse
Tips voor het implementeren van de groep door de opties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="what-does-group-by-do"></a>Wat is GROUP BY?

De [GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql) T-SQL-component maakt een aggregatie van gegevens naar een samenvatting set rijen. GROUP BY bevat een aantal opties die biedt geen ondersteuning voor SQL Data Warehouse. Deze opties staat er tijdelijke oplossingen.

Deze opties zijn

* GROUP BY met UPDATEPAKKET
* GROUPING SETS
* GROUP BY met kubus

## <a name="rollup-and-grouping-sets-options"></a>Rollup en grouping sets-opties
De eenvoudigste optie hier is het gebruik in plaats daarvan UNION ALL om uit te voeren van het updatepakket in plaats van te vertrouwen op de expliciete syntaxis. Het resultaat is precies hetzelfde

Het volgende voorbeeld met de GROUP BY-instructie met de ROLLUP-optie:
```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Met behulp van de ROLLUP-het voorgaande voorbeeld de volgende aggregaties aanvragen:

* Land en regio
* Land
* Eindtotaal

U kunt voor het UPDATEPAKKET vervangen en dezelfde resultaten retourneren, UNION ALL gebruiken en de vereiste aggregaties expliciet op te geven:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Als u wilt vervangen GROUPING SETS, het principe voorbeeld is van toepassing. U hoeft alleen te maken van UNION ALL secties voor de aggregatie niveaus die u wilt zien.

## <a name="cube-options"></a>Kubusopties voor de
Het is mogelijk een groep door met de kubus maken met de UNION ALL benadering. Het probleem is de code kan al gauw omslachtig en onhandig. U kunt deze meer geavanceerde benadering gebruiken als oplossing hiervoor kunt.

We gaan gebruiken in het bovenstaande voorbeeld.

De eerste stap is voor het definiëren van de 'kubus' waarin alle niveaus van aggregatie die we willen maken. Het is belangrijk te nemen van de CROSS JOIN van de twee afgeleide tabellen. Hiermee wordt alle niveaus gegenereerd voor ons. De rest van de code is echt er voor opmaak.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Hieronder ziet u de resultaten van de CTAS:

![Groeperen op kubus](media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png)

De tweede stap is om op te geven van een doeltabel voor het opslaan van tijdelijke resultaten:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

De derde stap is het doorlopen van onze kubus van kolommen uitvoeren van de aggregatie. De query wordt eenmaal uitvoeren voor elke rij in de tijdelijke tabel #Cube en de resultaten op te slaan in de tijdelijke tabel #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Ten slotte kunt u de resultaten terugkeren door gewoon lezen van de tijdelijke tabel #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Door de code in de secties splitsen en een samenvoegartikel constructie wordt gegenereerd, wordt de code te beheren en meer bruikbaar.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

