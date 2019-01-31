---
title: Met behulp van de groep met de opties in Azure SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van de groep met opties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 69a76ae9d6f355fe401b438ec2ab89d6606ba46c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463477"
---
# <a name="group-by-options-in-sql-data-warehouse"></a>Groeperen op opties in SQL Data Warehouse
Tips voor het implementeren van de groep met opties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.

## <a name="what-does-group-by-do"></a>Wat doet GROUP BY?

De [GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql) T-SQL-component aggregeert gegevens naar een samenvatting set rijen. Heeft enkele mogelijkheden die SQL Data Warehouse biedt geen ondersteuning voor GROUP BY. Deze opties staat er tijdelijke oplossingen.

Deze opties zijn

* GROUP BY met UPDATEPAKKET
* GROUPING SETS
* GROUP BY zonder kubus

## <a name="rollup-and-grouping-sets-options"></a>Hiermee stelt u opties Rollup en grouping
De eenvoudigste optie hier is het gebruik in plaats daarvan UNION ALL om uit te voeren van het updatepakket in plaats van afhankelijk zijn van de expliciete syntaxis. Het resultaat is precies hetzelfde

Het volgende voorbeeld met behulp van de GROUP BY-instructie met de optie ROLLUP:
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

Met behulp van UPDATEPAKKET, vraagt het vorige voorbeeld de volgende aggregaties:

* Land en regio
* Land/regio
* Eindtotaal

U kunt UPDATEPAKKET vervangen en de dezelfde resultaten worden geretourneerd, UNION ALL gebruiken en de vereiste aggregaties expliciet op te geven:

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

Als u wilt vervangen GROUPING SETS, de voorbeeld-principe van toepassing. U hoeft te maken van UNION ALL secties voor de aggregatieniveaus die u wilt zien.

## <a name="cube-options"></a>Kubusopties voor
Het is mogelijk een groep door met-kubus maken met behulp van de UNION ALL benadering. Het probleem is dat de code korte tijd uitgegroeid omslachtig en onhandig tot kunt. Als oplossing hiervoor, kunt u dit meer geavanceerde benadering gebruiken.

We gebruiken het bovenstaande voorbeeld.

De eerste stap is het definiëren van de 'kubus' waarin alle niveaus van aggregatie die we willen maken. Het is belangrijk te nemen van de CROSS JOIN van de twee afgeleide tabellen. Hiermee wordt alle niveaus gegenereerd voor ons. De rest van de code is echt er voor de opmaak.

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

De derde stap is het doorlopen van de kubus van uitvoeren van de aggregatie van kolommen. De query wordt eenmaal uitvoeren voor elke rij in de tijdelijke tabel #Cube en de resultaten opgeslagen in de tijdelijke tabel #Results

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

Ten slotte kunt u de resultaten terugkeren door gewoon uit de tijdelijke tabel #Results lezen

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Door de code opgesplitst in secties en konstruktor cyklu wordt gegenereerd, wordt de code beter worden beheerd en onderhouden.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkelaars [overzicht voor ontwikkelaars](sql-data-warehouse-overview-develop.md).

