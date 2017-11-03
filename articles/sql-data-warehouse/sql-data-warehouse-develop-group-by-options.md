---
title: Groeperen op opties in SQL Data Warehouse | Microsoft Docs
description: Tips voor het implementeren van de groep door de opties in Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f95a1e43-768f-4b7b-8a10-8a0509d0c871
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: da71cb834c13da5d0f5690f471efc6c696163f30
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a>Groeperen op opties in SQL Data Warehouse
De [GROUP BY] [ GROUP BY] component wordt gebruikt voor een overzicht set rijen van cumulatieve gegevens. Er wordt ook een aantal opties die de functionaliteit die moet worden besteed uitbreiden rond als ze niet rechtstreeks worden ondersteund door Azure SQL Data Warehouse.

Deze opties zijn

* GROUP BY met UPDATEPAKKET
* GROUPING SETS
* GROUP BY met kubus

## <a name="rollup-and-grouping-sets-options"></a>Rollup en grouping sets-opties
De eenvoudigste optie hier is het gebruik `UNION ALL` in plaats daarvan de rollup uitvoeren in plaats van te vertrouwen op de expliciete syntaxis. Het resultaat is precies hetzelfde

Hieronder volgt een voorbeeld van een groep met behulp van de instructie de `ROLLUP` optie:

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

Met behulp van UPDATEPAKKET hebben we de volgende aggregaties aangevraagd:

* Land en regio
* Land
* Eindtotaal

Dit u wilt gebruiken vervangen `UNION ALL`; aggregaties vereist expliciet om dezelfde resultaten weer te geven:

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

Voor GROUPING SETS alle we moeten doen is dezelfde principal vast maar UNION ALL secties voor de aggregatie niveaus die we wilt zien alleen maken

## <a name="cube-options"></a>Kubusopties voor de
Het is mogelijk een groep door met de kubus maken met de UNION ALL benadering. Het probleem is de code kan al gauw omslachtig en onhandig. Als oplossing hiervoor kunt u deze meer geavanceerde benadering.

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

![][1]

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

Ten slotte kunt we de resultaten terugkeren door gewoon lezen van de tijdelijke tabel #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

De code wordt door de code in de secties splitsen en het genereren van een samenvoegartikel constructie beter beheerbaar en bruikbaar.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [overzicht voor ontwikkelaars][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
