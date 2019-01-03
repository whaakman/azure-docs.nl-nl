---
title: Voorbeeld van Azure SQL Database In het geheugen | Microsoft Docs
description: Probeer Azure SQL Database In-Memory technologieën met OLTP en columnstore-voorbeeld.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 51cf04509608435117e0368b25952a58f7fc3557
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53609640"
---
# <a name="in-memory-sample"></a>In-Memory-voorbeeld

In-Memory-technologieën in Azure SQL Database kunnen u de prestaties van uw toepassing te verbeteren en kosten van de database mogelijk te reduceren. U kunt met behulp van In-Memory-technologieën in Azure SQL Database, verbeterde prestaties met verschillende workloads bereiken.

In dit artikel ziet u twee voorbeelden die laten zien van het gebruik van In-Memory OLTP, evenals de columnstore-indexen in Azure SQL Database.

Zie voor meer informatie:
- [Overzicht van in-Memory OLTP en gebruiksscenario's](https://msdn.microsoft.com/library/mt774593.aspx) (met inbegrip van verwijzingen naar casestudy's en informatie aan de slag)
- [Documentatie voor In-Memory OLTP](https://msdn.microsoft.com/library/dn133186.aspx)
- [Gids Columnstore-indexen](https://msdn.microsoft.com/library/gg492088.aspx)
- Hybride transactionele/analytische verwerking (HTAP), ook wel bekend als [real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)

<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Het voorbeeld In-Memory OLTP installeren

U kunt de AdventureWorksLT-voorbeelddatabase maken met een paar muisklikken in de [Azure-portal](https://portal.azure.com/). Vervolgens in deze sectie wordt uitgelegd hoe u de AdventureWorksLT-database met In-Memory OLTP-objecten te verrijken en prestatievoordelen demonstreren.

Voor een meer eenvoudig, maar visueel aantrekkelijker prestaties demo voor In-Memory OLTP, Zie:

- Release: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Broncode: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Installatiestappen

1. In de [Azure-portal](https://portal.azure.com/), maakt u een Premium en bedrijfskritiek-database op een server. Stel de **bron** met de AdventureWorksLT-voorbeelddatabase. Zie voor gedetailleerde instructies [uw eerste Azure SQL-database maken](sql-database-get-started-portal.md).

2. Verbinding maken met de database met SQL Server Management Studio [(SSMS.exe)](https://msdn.microsoft.com/library/mt238290.aspx).

3. Kopieer de [In-Memory OLTP Transact-SQL-script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) naar het Klembord. De T-SQL-script maakt u de objecten die nodig zijn In-Memory in de AdventureWorksLT-voorbeelddatabase die u in stap 1 hebt gemaakt.

4. Plak de T-SQL-script in SSMS en vervolgens het script wordt uitgevoerd. De `MEMORY_OPTIMIZED = ON` component CREATE TABLE-instructies zijn essentieel. Bijvoorbeeld:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fout 40536


Als u fout 40536 krijgt wanneer u de T-SQL-script uitvoert, voert u de volgende T-SQL-script om te controleren of de database ondersteuning biedt voor In-Memory uit:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Een resultaat van **0** betekent dat In het geheugen wordt niet ondersteund, en **1** betekent dat het wordt ondersteund. Als u wilt het probleem vaststellen, zorg ervoor dat de database op de Premium-servicelaag.


#### <a name="about-the-created-memory-optimized-items"></a>Over de gemaakte geoptimaliseerd voor geheugen items

**Tabellen**: Het voorbeeld bevat de volgende tabellen geoptimaliseerd voor geheugen:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


U kunt inspecteren tabellen geoptimaliseerd voor geheugen via de **Objectverkenner** in SSMS. Met de rechtermuisknop op **tabellen** > **Filter** > **filterinstellingen** > **Is geoptimaliseerd voor geheugen**. De waarde gelijk is aan 1.


Of u kunt de catalogusweergaven, zoals een query:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Systeemeigen, gecompileerde, opgeslagen procedure**: U kunt SalesLT.usp_InsertSalesOrder_inmem inspecteren via een query catalogus weergeven:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a>Het voorbeeld OLTP-werkbelasting uitvoeren

Het enige verschil tussen de volgende twee *opgeslagen procedures* is dat de eerste procedure maakt gebruik van versies van de tabellen geoptimaliseerd voor geheugen, terwijl de tweede procedure worden de regelmatig op de schijf-tabellen gebruikt:

- SalesLT **.** usp_InsertSalesOrder **_inmem**
- SalesLT **.** usp_InsertSalesOrder **_ondisk**


In deze sectie ziet u hoe u de handige **ostress.exe** hulpprogramma voor het uitvoeren van de twee opgeslagen procedures op stress niveaus. U kunt vergelijken hoe lang het duurt voor de twee stress wordt uitgevoerd om te voltooien.


Wanneer u ostress.exe uitvoert, wordt u aangeraden dat geeft u de parameterwaarden die zijn ontworpen voor het volgende:

- Een groot aantal gelijktijdige verbindingen, worden uitgevoerd via - n100.
- Op elke lus verbinding honderden keren, hebben met de parameter - r500.


Echter, als u wilt beginnen met veel kleiner waarden zoals - n10 en -r50 om ervoor te zorgen dat alles werkt.


### <a name="script-for-ostressexe"></a>Script voor ostress.exe


Deze sectie vindt u de T-SQL-script dat is ingesloten in onze ostress.exe vanaf de opdrachtregel. Het script maakt gebruik van items die zijn gemaakt door de T-SQL-script dat u eerder hebt geïnstalleerd.


Het volgende script wordt een voorbeeld van verkooporderformulier met vijf regelitems ingevoegd in de volgende geoptimaliseerd voor geheugen *tabellen*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Om de *_ondisk* versie van de voorgaande T-SQL-script voor ostress.exe, vervangt u beide exemplaren van de *_inmem* subtekenreeks met *_ondisk*. Deze vervangingen van invloed zijn op de namen van tabellen en opgeslagen procedures.


### <a name="install-rml-utilities-and-ostress"></a>RML hulpprogramma's installeren en `ostress`


In het ideale geval zou u van plan bent om uit te voeren ostress.exe op een Azure-machine (VM). U maakt een [virtuele Azure-machine](https://azure.microsoft.com/documentation/services/virtual-machines/) in dezelfde Azure geografische regio waarin uw AdventureWorksLT-database zich bevindt. Maar u kunt in plaats daarvan ostress.exe op uw laptop uitvoeren.


Op de virtuele machine of op elke host die u hebt Kies, installeert u de's opnieuw afspelen Markup Language (RML). De hulpprogramma's omvatten ostress.exe.

Zie voor meer informatie:
- De discussie ostress.exe in [voorbeelddatabase voor In-Memory OLTP](https://msdn.microsoft.com/library/mt465764.aspx).
- [Voorbeeld van een Database voor In-Memory OLTP](https://msdn.microsoft.com/library/mt465764.aspx).
- De [blog voor het installeren van ostress.exe](https://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Voer de *_inmem* eerst stresstest voor de werkbelasting


U kunt een *RML Cmd vragen* venster om uit te voeren van onze ostress.exe vanaf de opdrachtregel. De opdrachtregelparameters direct `ostress` aan:

- 100 verbindingen gelijktijdig worden uitgevoerd (-n100).
- Elke verbinding heeft de T-SQL-script 50 keer uitvoeren (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


De voorgaande ostress.exe vanaf de opdrachtregel uitvoeren:


1. Opnieuw instellen van de inhoud van de gegevens database door het uitvoeren van de volgende opdracht in SSMS, om alle gegevens die door een eerdere uitvoeringen is ingevoegd te verwijderen:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Kopieer de tekst van de voorgaande ostress.exe vanaf de opdrachtregel naar het Klembord.

3. Vervang de `<placeholders>` voor de parameters -S - U -P -d met de juiste echte waarden.

4. De bewerkte opdrachtregel uitvoeren in een venster RML Cmd.


#### <a name="result-is-a-duration"></a>Resultaat is een duur


Wanneer `ostress.exe` is voltooid, wordt de uitvoeringsduur als de laatste regel van de uitvoer geschreven in het venster RML Cmd. Bijvoorbeeld, een kortere testuitvoering heeft geduurd van ongeveer 1,5 minuten:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Opnieuw instellen en bewerken voor *_ondisk*, klikt u vervolgens opnieuw uitvoeren


Nadat u het resultaat van hebt de *_inmem* uitvoeren, voert u de volgende stappen uit voor de *_ondisk* uitvoeren:


1. Opnieuw instellen van de database met de volgende opdracht in SSMS om alle gegevens te verwijderen die door de vorige uitvoering is ingevoegd:
```
EXECUTE Demo.usp_DemoReset;
```

2. Bewerken van de opdrachtregel ostress.exe vervangen van alle *_inmem* met *_ondisk*.

3. Ostress.exe voor de tweede keer opnieuw, en het resultaat duur vastleggen.

4. Nogmaals, de database (voor het verantwoorde manier te verwijderen van wat is een grote hoeveelheid testgegevens) opnieuw instellen.


#### <a name="expected-comparison-results"></a>Van de verwachte vergelijkingsresultaten

Onze In-Memory-tests heeft aangetoond dat prestaties verbeterd door **negen keer** voor deze eenvoudig werkbelasting met `ostress` die worden uitgevoerd op een Azure-VM in hetzelfde Azure-regio als de database.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a>2. Het voorbeeld In-Memory analyse installeren


In deze sectie maakt vergelijken u de i/o- en statistieken van de resultaten als u een columnstore-index ten opzichte van een traditionele b-tree-index.


Realtime analyses over een OLTP-werkbelasting is het vaak het beste een niet-geclusterde columnstore-index te gebruiken. Zie voor meer informatie, [Columnstore-indexen beschreven](https://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Voorbereiden van de columnstore-analytics-test


1. De Azure portal gebruiken voor het maken van een nieuwe AdventureWorksLT-database van het voorbeeld.
 - Gebruikt deze naam.
 - Kies een Premium-servicelaag.

2. Kopieer de [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) naar het Klembord.
 - De T-SQL-script maakt u de objecten die nodig zijn In-Memory in de AdventureWorksLT-voorbeelddatabase die u in stap 1 hebt gemaakt.
 - Het script maakt u de dimensietabel en twee feitentabellen. De feitentabellen worden ingevuld met 3,5 miljoen rijen.
 - Het script kan 15 minuten duren.

3. Plak de T-SQL-script in SSMS en vervolgens het script wordt uitgevoerd. De **COLUMNSTORE** sleutelwoord in de **CREATE INDEX** instructie is van cruciaal belang, zoals in:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. AdventureWorksLT ingesteld op compatibiliteitsniveau 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Level 130 is niet direct gerelateerd aan In-Memory-functies. Maar level 130 biedt doorgaans sneller query dan 120.


#### <a name="key-tables-and-columnstore-indexes"></a>Belangrijkste tabellen en columnstore-indexen


- dbo. FactResellerSalesXL_CCI is een tabel met een geclusterde columnstore-index, die beschikt over geavanceerde compressie op het *gegevens* niveau.

- dbo. FactResellerSalesXL_PageCompressed is een tabel met een gelijkwaardige reguliere geclusterde index, die wordt gecomprimeerd alleen op de *pagina* niveau.


#### <a name="key-queries-to-compare-the-columnstore-index"></a>Belangrijke query's om te vergelijken van de columnstore-index


Er zijn [verschillende typen van T-SQL-query's die u kunt uitvoeren](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) om te zien van verbeterde prestaties. In stap 2 van de T-SQL-script, Let op deze combinatie van query's. Deze verschillen alleen op één regel:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Een geclusterde columnstore-index is in de FactResellerSalesXL\_CCI tabel.

Het volgende fragment van T-SQL-script af te drukken statistieken voor i/o- en -tijd voor de query van elke tabel.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

In een database met de prijscategorie P2, kunt u verwachten dat ongeveer negen keer de prestatieverbetering voor deze query met behulp van de geclusterde columnstore-index in vergelijking met de traditionele index. Met P15, kunt u verwachten dat ongeveer 57 maal de prestatieverbetering te bereiken met behulp van de columnstore-index.



## <a name="next-steps"></a>Volgende stappen

- [Snelstartgids 1: In-Memory OLTP-technologieën voor snellere prestaties met T-SQL](https://msdn.microsoft.com/library/mt694156.aspx)

- [Gebruik In-Memory OLTP in een bestaande Azure SQL-toepassing](sql-database-in-memory-oltp-migration.md)

- [Monitor In-Memory OLTP-opslag](sql-database-in-memory-oltp-monitoring.md) voor In-Memory OLTP


## <a name="additional-resources"></a>Aanvullende resources

#### <a name="deeper-information"></a>Meer informatie

- [Informatie over hoe Quorum verdubbelt de belangrijkste databaseworkload terwijl u tegelijkertijd DTU terugdringt met 70% met In-Memory OLTP in SQL-Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [In-Memory OLTP in Azure SQL Database-blogbericht](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Meer informatie over In-Memory OLTP](https://msdn.microsoft.com/library/dn133186.aspx)

- [Meer informatie over de columnstore-indexen](https://msdn.microsoft.com/library/gg492088.aspx)

- [Meer informatie over real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)

- Zie [algemene patronen van de werkbelasting en overwegingen bij migraties](https://msdn.microsoft.com/library/dn673538.aspx) (welke optie beschrijft werkbelasting patronen waar In-Memory OLTP vaak aanzienlijke prestatievoordelen biedt)

#### <a name="application-design"></a>Het ontwerp van toepassing

- [In-Memory OLTP (optimalisatie In het geheugen)](https://msdn.microsoft.com/library/dn133186.aspx)

- [Gebruik In-Memory OLTP in een bestaande Azure SQL-toepassing](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Hulpprogramma's

- [Azure Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
