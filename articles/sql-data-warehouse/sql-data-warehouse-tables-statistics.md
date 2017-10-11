---
title: Het beheren van statistieken over tabellen in SQL Data Warehouse | Microsoft Docs
description: Aan de slag met statistieken over tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 1d5ded69e394643ddfc3de0c6d30dbd30c8e848f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Het beheren van statistieken over tabellen in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Overzicht][Overview]
> * [Gegevenstypen][Data Types]
> * [Distribueren][Distribute]
> * [Index][Index]
> * [Partitie][Partition]
> * [Statistieken][Statistics]
> * [Tijdelijke][Temporary]
> 
> 

Hoe meer SQL Data Warehouse weet over uw gegevens, hoe sneller een query uitgevoerd op uw gegevens kunnen worden uitgevoerd.  De manier waarop u SQL Data Warehouse zien over uw gegevens is door het verzamelen van statistieken over uw gegevens.  Statistieken over voor uw gegevens is een van de belangrijkste dingen die u doen kunt om uw query's optimaliseren.  Statistieken helpen bij het maken van de optimale planning van uw query's van SQL Data Warehouse.  Dit is omdat SQL Data Warehouse queryoptimalisatie optimalisatie van de kosten die zijn gebaseerd.  Dat wil zeggen, vergelijkt de kosten van verschillende queryplannen en kiest vervolgens het plan met de laagste kosten, moet het plan dat de snelste wordt uitgevoerd.

Statistieken kunnen worden gemaakt op één kolom, meerdere kolommen of op een index van een tabel.  Statistieken worden opgeslagen in een histogram die het bereik en selectiviteit van waarden vastgelegd.  Dit is met name van belang wanneer het optimalisatieprogramma moet evalueren JOINs, GROUP BY, HAVING en de component WHERE in een query.  Bijvoorbeeld als het optimalisatieprogramma maakt een schatting van dat de datum die u hebt gefilterd in uw query retourneert 1 rij, kunnen ervoor kiezen een heel andere plannen dan als deze maakt een schatting datum die u hebt geselecteerd wordt 1 miljoen rijen worden geretourneerd.  Tijdens het maken van statistieken is zeer belangrijk, is het belangrijk dat statistieken *nauwkeurig* overeenstemming met de huidige status van de tabel.  Up-to-date statistieken met zorgt ervoor dat het een goed plan is geselecteerd door de optimaliseren.  De abonnementen die zijn gemaakt door het optimalisatieprogramma zijn alleen nuttig als de statistieken voor uw gegevens.

Het proces van het maken en bijwerken van statistieken is op dit moment een handmatig proces, maar het is zeer eenvoudig doen.  Dit is in tegenstelling tot SQL Server die automatisch wordt gemaakt en statistieken over één kolommen en indexen bijgewerkt.  U kunt het beheer van de statistieken aanzienlijk voor uw gegevens automatiseren met behulp van de onderstaande informatie. 

## <a name="getting-started-with-statistics"></a>Aan de slag met statistieken
 Steekproef statistieken maken voor elke kolom is een eenvoudige manier om te beginnen met statistieken.  Aangezien is het belangrijk statistieken om up-to-date te houden, mogelijk uw statistieken bijwerken dagelijks of na elke load voorzichtig zijn. Het maken en bijwerken van statistieken kan ten koste gaan van prestaties en kosten.  Als het te lang duurt om al uw statistieken bij te houden, kunt u overwegen selectiever te zijn in welke kolommen statistieken hebben of voor welke kolommen de statistieken regelmatig moeten worden bijgewerkt.  U wilt bijwerken datumkolommen dagelijks nieuwe waarden kunnen worden toegevoegd in plaats van na elke load. Opnieuw, krijgt u optimaal wilt profiteren doordat de statistieken voor kolommen die zijn betrokken in JOINs, GROUP BY, HAVING en de component WHERE.  Als u een tabel met een groot aantal kolommen die alleen worden gebruikt in de component SELECT hebt, statistieken voor deze kolommen niet kan helpen en uitgaven iets meer werk alleen de kolommen waarin statistieken helpt, identificeren inkorten de tijd voor het onderhouden van uw statistieken.

## <a name="multi-column-statistics"></a>Statistieken met meerdere kolommen
Naast het maken van statistieken voor één kolommen, merkt u dat uw query's van meerdere kolomstatistieken profiteren.  Statistieken met meerdere kolommen worden statistieken gemaakt op een lijst met kolommen.  Ze bevatten één kolomstatistieken voor de eerste kolom in de lijst, plus informatie cross-kolom correlatie dichtheden aangeroepen.  Bijvoorbeeld, als er een tabel die samengevoegd op twee kolommen, wellicht dat SQL Data Warehouse beter het plan optimaliseren kunt wordt begrepen dat de relatie tussen de twee kolommen.   Statistieken met meerdere kolommen kunnen query's sneller voor bepaalde bewerkingen zoals joins samengestelde en gegroepeerd.

## <a name="updating-statistics"></a>Bijwerken van statistieken
Bijwerken van statistieken is een belangrijk onderdeel van uw database management routine.  Wanneer de distributie van gegevens in de database wordt gewijzigd, moeten de statistieken worden bijgewerkt.  Verouderde statistieken zal leiden tot suboptimale queryprestaties.

Een aanbevolen procedure is het bijwerken van statistieken over datumkolommen elke dag als nieuwe datums worden toegevoegd.  Elke keer nieuwe rijen in het datawarehouse zijn geladen, worden nieuwe load datums of transactiedatums toegevoegd. Deze de verdeling van de gegevens wijzigen en de statistieken verouderd wilt maken. Als u daarentegen statistieken over een land-kolom in een tabel van de klant mogelijk nooit moeten worden bijgewerkt, zoals de verdeling van waarden in het algemeen niet wijzigen. Ervan uitgaande dat de distributie constant tussen klanten, eens toevoegen van nieuwe rijen aan de tabel variatie niet te wijzigen van de gegevensdistributie. Echter als uw datawarehouse alleen een bepaald land bevat en u Importeer gegevens vanuit een nieuw land, moet wat resulteert in gegevens uit meerdere landen worden opgeslagen, zeker u statistieken voor de kolom land moeten worden bijgewerkt.

Een van de eerste vragen stellen bij het oplossen van een query is 'Are de statistieken bijgewerkt?'

Deze vraag is niet een die door de leeftijd van de gegevens kunnen worden beantwoord. Een object up-to-date statistieken kan worden oude als er geen belangrijke wijzigingen in de onderliggende gegevens zijn. Wanneer het aantal rijen aanzienlijk is gewijzigd of er is een belangrijke wijziging in de distributie van waarden voor een bepaalde kolom *vervolgens* is het tijd om het bijwerken van statistieken.  

Ter referentie: **SQL Server** (geen SQL Data Warehouse) automatisch bijgewerkt statistieken voor deze situaties:

* Als er geen rijen in de tabel wanneer u rijen toevoegt, krijgt u een automatische update van statistieken
* Wanneer u meer dan 500 rijen toevoegen aan een tabel die beginnen met minder dan 500 rijen (bijvoorbeeld aan begin u hebt 499 en 500 rijen vervolgens toe te voegen aan een totaal van 999 rijen), krijgt u een automatische update 
* Als u meer dan 500 rijen die u 500 extra rijen + 20% van de grootte van de tabel toevoegen moet voordat u een automatische update op de statistieken ziet klaar

Omdat er geen DMV om te bepalen of de gegevens in de tabel is gewijzigd sinds de laatste tijd statistieken zijn bijgewerkt, kunt weet de leeftijd van de statistieken u kennismaken met onderdeel van de afbeelding.  U kunt de volgende query om te bepalen van de laatste keer dat de statistieken waar bijgewerkt op elke tabel.  

> [!NOTE]
> Vergeet niet dat als er een aanzienlijke wijzigingen in de distributie van waarden voor een bepaalde kolom, moet u bijwerken statistieken ongeacht de laatste keer dat ze zijn bijgewerkt.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Datumkolommen in een datawarehouse bijvoorbeeld meestal regelmatig hoeft te worden statistieken updates. Elke keer nieuwe rijen in het datawarehouse zijn geladen, worden nieuwe load datums of transactiedatums toegevoegd. Deze de verdeling van de gegevens wijzigen en de statistieken verouderd wilt maken.  Als u daarentegen statistieken voor een kolom geslacht in een klantentabel mogelijk nooit moeten worden bijgewerkt. Ervan uitgaande dat de distributie constant tussen klanten, eens toevoegen van nieuwe rijen aan de tabel variatie niet te wijzigen van de gegevensdistributie. Echter als uw datawarehouse slechts één geslacht en een nieuwe vereiste resultaten in meerdere geslachten bevat moet beslist u statistieken voor de kolom geslacht moeten worden bijgewerkt.

Zie voor verdere uitleg [statistieken] [ Statistics] op MSDN.

## <a name="implementing-statistics-management"></a>Implementatie van statistieken management
Het is vaak een goed idee om uw gegevens laadproces om ervoor te zorgen dat de statistieken worden bijgewerkt aan het einde van de belasting uitbreiden. Het laden van gegevens is wanneer tabellen meest hun grootte en/of de distributie van de waarden wijzigen. Daarom is dit een logische plaats voor het implementeren van een aantal processen.

Sommige principes zijn hieronder beschikbaar voor het bijwerken van de statistieken tijdens het laadproces:

* Zorg ervoor dat elke geladen tabel ten minste één statistieken-object bijgewerkt. Hiermee wordt de informatie over de grootte (aantal rijen en aantal pagina's) van tabellen als onderdeel van de update van de statistieken bijgewerkt.
* Richt u op de kolommen die deel uitmaken van JOIN, GROUP BY, ORDER BY en DISTINCT-componenten
* Houd rekening met het bijwerken van kolommen 'oplopende sleutel' zoals transactie datums vaker als deze waarden niet in het histogram statistieken opgenomen worden.
* U kunt statische distributiekolommen minder vaak bijwerken.
* Vergeet niet dat elk object statistiek is bijgewerkt in de reeks. Gewoon implementeren `UPDATE STATISTICS <TABLE_NAME>` mogelijk niet ideaal - vooral voor grote tabellen met veel objecten zijn statistieken.

> [!NOTE]
> Raadpleeg de SQL Server 2014 kardinaliteitsschatting model technisch document voor meer informatie over [sleutel oplopende].
> 
> 

Zie voor verdere uitleg [Kardinaliteitsschatting] [ Cardinality Estimation] op MSDN.

## <a name="examples-create-statistics"></a>Voorbeelden: Statistieken maken
Deze voorbeelden laten zien hoe het gebruik van verschillende opties voor het maken van statistieken. De opties die u voor elke kolom gebruikt is afhankelijk van de kenmerken van uw gegevens en hoe u de kolom wordt gebruikt in query's.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Statistieken voor één kolom maken met de standaardopties
Verstrek voor statistieken maken voor een kolom, een naam voor het object statistieken en de naam van de kolom.

Deze syntaxis gebruikt alle standaardopties. SQL Data Warehouse voorbeelden standaard 20 procent van de tabel bij het maken van statistieken.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Maken van statistieken voor één kolom aan de hand van elke rij
De samplingfrequentie standaardwaarde van 20 procent is voldoende voor de meeste situaties. U kunt echter de samplingfrequentie aanpassen.

Gebruik de volgende syntaxis om een voorbeeld van de volledige tabel:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Maken van statistieken voor één kolom door te geven van de grootte van de steekproef
U kunt ook de grootte van de steekproef opgeven als een percentage:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D. Maken van statistieken voor één kolom op slechts een deel van de rijen
Een andere optie kunt u statistieken op een gedeelte van de rijen in de tabel. Dit is een gefilterde statistieken genoemd.

U kunt bijvoorbeeld gefilterde statistieken gebruiken als u van plan bent een query uitvoeren op een specifieke partitie van een grote gepartitioneerde tabel. Als u statistieken maakt op alleen de waarden van de partitie, de nauwkeurigheid van de statistieken wordt te verbeteren, en daarom queryprestaties verbeteren.

In dit voorbeeld maakt statistieken op een bereik met waarden. De waarden kunnen eenvoudig worden gedefinieerd zodat deze overeenkomt met het bereik van waarden in een partitie.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Voor queryoptimalisatie overwegen gefilterde statistieken wanneer deze het plan gedistribueerde query kiest, moet de query binnen de definitie van de statistieken object passen. Met behulp van het vorige voorbeeld van de query waarin component moet col1 waarden tussen 2000101 en 20001231 opgeven.
> 
> 

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Statistieken voor één kolom maken met alle opties
U kunt de opties natuurlijk samen combineren. Het volgende voorbeeld wordt een gefilterde statistieken-object gemaakt met een aangepaste samplegrootte:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Zie voor de volledige verwijzing [CREATE STATISTICS] [ CREATE STATISTICS] op MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Statistieken met meerdere kolommen maken
Voor het maken van een statistieken met meerdere kolommen gewoon gebruiken de eerdere voorbeelden, maar meer kolommen opgeven.

> [!NOTE]
> Het histogram, wordt gebruikt om het aantal rijen in het queryresultaat schatten, is alleen beschikbaar voor de eerste kolom die worden vermeld in de objectdefinitie van statistieken.
> 
> 

In dit voorbeeld wordt het histogram is op *product\_categorie*. Statistieken voor cross-kolom worden berekend op *product\_categorie* en *product\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Omdat er een correlatie tussen *product\_categorie* en *product\_sub\_categorie*, een stat met meerdere kolommen kan nuttig zijn als deze kolommen zijn toegankelijk op hetzelfde moment.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G. Statistieken maken voor alle kolommen in een tabel
Een manier om statistieken te maken is om problemen CREATE STATISTICS opdrachten na het maken van de tabel.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H. Een opgeslagen procedure gebruiken om statistieken te maken voor alle kolommen in een database
SQL Data Warehouse heeft geen een systeem opgeslagen procedure die gelijk is aan [sp_create_stats] [] in SQL Server. Deze opgeslagen procedure maakt een object van de statistieken één kolom voor elke kolom van de database die nog geen statistieken.

Dit helpt u aan de slag met het ontwerp van de database. U kunt deze aanpassen aan uw behoeften.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Voor het maken van statistieken voor alle kolommen in de tabel met deze procedure, kunt u de procedure te roepen.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Voorbeelden: update statistics
Voor het bijwerken van statistieken, kunt u het volgende doen:

1. Een object van de statistieken worden bijgewerkt. Geef de naam van de statistieken-object dat u wilt bijwerken.
2. Werk alle statistieken objecten in een tabel. Geef de naam van de tabel in plaats van één specifieke statistieken-object.

### <a name="a-update-one-specific-statistics-object"></a>A. Object voor een specifieke statistieken bijwerken
Gebruik de volgende syntaxis als u een object specifieke statistieken bij te werken:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Door het bijwerken van statistieken voor specifieke objecten, minimaliseert u de tijd en resources die vereist zijn voor het beheren van statistieken. Hiervoor moet voorzichtig, maar om de beste objecten statistieken bijwerken te kiezen.

### <a name="b-update-all-statistics-on-a-table"></a>B. Bijwerken van alle statistische gegevens in een tabel
Hier wordt een eenvoudige methode voor het bijwerken van de statistieken-objecten in een tabel.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS dbo.table1;
```

Deze instructie is eenvoudig te gebruiken. Vergeet niet dit alle statistische gegevens voor de tabel wordt bijgewerkt en meer werk dan noodzakelijk is daarom mogelijk uitvoeren. Als de prestaties niet een probleem is, is dit zeker de eenvoudigste en meest volledige manier om te garanderen statistieken zijn bijgewerkt.

> [!NOTE]
> Wanneer alle statistische gegevens in een tabel wordt bijgewerkt, wordt in SQL Data Warehouse een scan met het volgende voorbeeld de tabel voor elke statistieken bevat. Als de tabel groot is, heeft veel kolommen en meer statistische gegevens, mogelijk efficiënter afzonderlijke statistieken op basis van behoeften moeten worden bijgewerkt.
> 
> 

Voor een implementatie van een `UPDATE STATISTICS` Zie de procedure de [tijdelijke tabellen] [ Temporary] artikel. De implementatie-methode is iets anders dan bij de `CREATE STATISTICS` bovenstaande procedure, maar het eindresultaat is hetzelfde.

Zie voor de volledige syntaxis [Update Statistics] [ Update Statistics] op MSDN.

## <a name="statistics-metadata"></a>Statistieken metagegevens
Er zijn verschillende systeemweergave en functies die u gebruiken kunt om informatie over statistieken te vinden. U kunt bijvoorbeeld zien als een object statistieken mogelijk verouderd met behulp van de functie statistieken datum om te zien wanneer statistieken laatste zijn gemaakt of bijgewerkt.

### <a name="catalog-views-for-statistics"></a>Catalogusweergaven voor statistieken
Deze systeemweergaven bevatten informatie over statistieken:

| Catalogusweergave | Beschrijving |
|:--- |:--- |
| [sys.Columns][sys.columns] |Een rij voor elke kolom. |
| [sys.Objects][sys.objects] |Een rij voor elk object in de database. |
| [sys.schemas][sys.schemas] |Een rij voor elk schema in de database. |
| [sys.Stats][sys.stats] |Een rij voor elk object statistieken. |
| [sys.stats_columns][sys.stats_columns] |Een rij voor elke kolom in het object statistieken. Koppelingen naar sys.columns. |
| [sys.Tables][sys.tables] |Een rij voor elke tabel (inclusief externe tabellen). |
| [sys.table_types][sys.table_types] |Een rij voor elk gegevenstype. |

### <a name="system-functions-for-statistics"></a>Systeemfuncties voor statistieken
Deze systeemfuncties zijn nuttig voor het werken met statistieken:

| De systeemfunctie | Beschrijving |
|:--- |:--- |
| [STATS_DATE][STATS_DATE] |De datum is die het object statistieken voor het laatst is bijgewerkt. |
| [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] |Biedt samenvatting niveau en gedetailleerde informatie over de distributie van de waarden zoals begrepen door het object statistieken. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Functies en statistieken kolommen combineren in één weergave
Deze weergave biedt kolommen die betrekking op statistieken hebben en resultaat is van de functie samen [] van [STATS_DATE()].

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS()-voorbeelden
DBCC SHOW_STATISTICS() bevat gegevens die zijn opgeslagen in een object van statistieken. Deze gegevens zijn afkomstig uit drie delen.

1. Koptekst
2. Dichtheid Vector
3. Histogram

De header-metagegevens over de statistieken. Het histogram geeft de verdeling van waarden in de eerste kolom van de sleutel van het object statistieken. De dichtheid vector metingen cross-kolom correlatie. SQLDW berekent kardinaliteit maakt een schatting met een van de gegevens in de statistieken-object.

### <a name="show-header-density-and-histogram"></a>Koptekst, dichtheid en histogram weergeven
Dit eenvoudige voorbeeld toont alle drie onderdelen van een object statistieken.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Een of meer onderdelen van DBCC SHOW_STATISTICS(); weergeven
Als u alleen geïnteresseerd bent in specifieke onderdelen weer te geven, gebruikt u de `WITH` component en opgeven welke onderdelen die u wilt zien:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() verschillen
DBCC SHOW_STATISTICS() is meer strikt in SQL Data Warehouse vergeleken met SQL Server geïmplementeerd.

1. Niet-gedocumenteerde functies worden niet ondersteund
2. Stats_stream kan niet worden gebruikt.
3. Resultaten voor specifieke subreeksen van statistische gegevens bijvoorbeeld niet koppelen (STAT_HEADER JOIN DENSITY_VECTOR)
4. NO_INFOMSGS kan niet worden ingesteld voor bericht onderdrukken
5. Vierkante haken om de namen van statistieken kan niet worden gebruikt.
6. Kan geen kolomnamen gebruiken om statistieken objecten te identificeren
7. Aangepaste fout 2767 wordt niet ondersteund

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] op MSDN.  Zie voor meer informatie de artikelen op [tabel overzicht][Overview], [tabel gegevenstypen][Data Types], [distribueren van een tabel] [ Distribute], [Indexeren van een tabel][Index], [partitioneren van een tabel] [ Partition] en [Tijdelijke tabellen][Temporary].  Zie voor meer informatie over best practices [aanbevolen procedures van SQL Data Warehouse][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
