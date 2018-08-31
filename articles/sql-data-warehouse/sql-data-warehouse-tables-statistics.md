---
title: Het maken, bijwerken van statistieken - Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen en voorbeelden voor het maken en bijwerken van statistieken van de query-optimalisatie voor tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 05/09/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 1a7ea00e8bdf4fa1a22dd765e5108dce72e2d380
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307459"
---
# <a name="creating-updating-statistics-on-tables-in-azure-sql-data-warehouse"></a>Het maken, bijwerken van statistieken op tabellen in Azure SQL Data Warehouse
Aanbevelingen en voorbeelden voor het maken en bijwerken van statistieken van de query-optimalisatie voor tabellen in Azure SQL Data Warehouse.

## <a name="why-use-statistics"></a>Waarom gebruik maken van statistieken?
Hoe Azure SQL Data Warehouse weet over uw gegevens, des te sneller query's op deze kunnen worden uitgevoerd. Verzamelen van gegevens over uw gegevens en deze vervolgens geladen in SQL Data Warehouse is een van de meest belangrijke dingen die u doen kunt voor het optimaliseren van uw query's. Dit komt doordat het queryoptimalisatieprogramma SQL Data Warehouse een optimaliseren op basis van kosten is. Het vergelijkt de kosten van verschillende queryplannen en kiest vervolgens het plan met de laagste kosten, dat in de meeste gevallen het plan is dat het snelst wordt uitgevoerd. Bijvoorbeeld, als het optimalisatieprogramma zijn datumfiltering in de query retourneert één rij schattingen, kunt het een ander abonnement dan als het maakt een schatting van die de geselecteerde datum wordt 1 miljoen rijen geretourneerd.

## <a name="automatic-creation-of-statistics"></a>Automatisch maken van statistieken
Bij het maken van de automatische statistieken-optie is ingeschakeld, AUTO_CREATE_STATISTICS, SQL Data Warehouse binnenkomende query's van gebruikers waar één kolom statistieken worden gemaakt voor kolommen die niet over statistieken beschikken worden geanalyseerd. Het queryoptimalisatieprogramma maakt statistieken op afzonderlijke kolommen in de query predikaat of join-voorwaarde voor het verbeteren van de kardinaliteit schattingen voor het queryplan. Automatisch maken van statistieken is momenteel standaard ingeschakeld.

U kunt controleren of uw datawarehouse deze geconfigureerd heeft met de volgende opdracht:

```sql
SELECT name, is_auto_create_stats_on 
FROM sys.databases
```
Als uw datawarehouse heeft geen AUTO_CREATE_STATISTICS geconfigureerd, raden we u deze eigenschap inschakelen door het uitvoeren van de volgende opdracht uit:

```sql
ALTER DATABASE <yourdatawarehousename> 
SET AUTO_CREATE_STATISTICS ON
```
De volgende instructies wordt automatisch maken van statistieken geactiveerd: selecteert, INSERT-selecteren, CTAS, UPDATE, DELETE en uitleg wanneer met een join of de aanwezigheid van een predicaat is gedetecteerd. 

> [!NOTE]
> Automatisch maken van statistieken worden niet gemaakt voor tijdelijke of externe tabellen.
> 

Automatisch maken van statistieken gegenereerd synchroon, zodat u mogelijk een lichte slechtere queryprestaties in rekening als uw kolommen nog geen statistieken worden gemaakt. Het maken van statistieken, duurt een paar seconden op één kolom, afhankelijk van de grootte van de tabel. Om te voorkomen dat het meten van prestatievermindering, met name in benchmarks voor prestaties, moet u ervoor zorgen dat statistieken eerst gemaakt met de benchmark-werkbelasting wordt uitgevoerd voordat de profilering van het systeem.

> [!NOTE]
> Het maken van statistieken ook worden geregistreerd in [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=aps-pdw-2016) in de context van een andere gebruiker.
> 

Wanneer automatische statistieken zijn gemaakt, nemen ze de vorm: _WA_Sys_< 8 cijfers kolom-id in een hexadecimale waarde > _ < 8 cijfers tabel-id in een hexadecimale waarde >. Vindt u statistieken die al zijn gemaakt door het uitvoeren van de [DBCC SHOW_STATISTICS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?view=sql-server-2017) opdracht:

```sql
DBCC SHOW_STATISTICS (<tablename>, <targetname>)
```
Het eerste argument is een tabel met de statistieken om weer te geven. Dit kan een externe tabel niet. Het tweede argument is de naam van de doelindex, de statistieken of de kolom waarvoor om statistische gegevens weer te geven.



## <a name="updating-statistics"></a>Bijwerken van statistieken

Een aanbevolen procedure is het bijwerken van statistieken op datumkolommen elke dag nieuwe datums worden toegevoegd. Elke keer nieuwe rijen worden geladen in het datawarehouse, worden nieuwe load datums of transactiedatums toegevoegd. Deze de verdeling van de gegevens wijzigen en de statistieken niet actueel te maken. Daarentegen statistieken op een land/regio-kolom in een tabel van de klant mogelijk nooit moeten worden bijgewerkt, omdat de verdeling van waarden in het algemeen niet wijzigen. Ervan uitgaande dat de distributie is constant tussen klanten, wordt nieuwe rijen toe te voegen aan de variant van de tabel niet de verdeling van de gegevens wijzigen. Echter, als uw datawarehouse alleen één land/regio bevat en u in de gegevens van een nieuw land/regio brengt, wat resulteert in gegevens uit meerdere landen worden opgeslagen, moet u bijwerken van statistieken voor de kolom land/regio.

Hier volgen de aanbevelingen statistieken bij te werken:

|||
|-|-|
| **Frequentie van updates van statistieken**  | Conservatief: dagelijks <br></br> Na het laden van of uw gegevens te transformeren |
| **Steekproeven** |  Minder dan 1 miljard rijen, gebruiken standaard steekproeven (20 procent) <br></br> Met meer dan 1 miljard rijen is statistieken voor een bereik 2 procent goed |

Een van de eerste vragen wanneer u problemen met een query is, **"Zijn de statistieken bijgewerkt?"**

Deze vraag is niet een die door de leeftijd van de gegevens kunnen worden beantwoord. Een object up-to-date statistieken mogelijk oude als er geen belangrijke wijzigingen in de onderliggende gegevens zijn. Wanneer het aantal rijen aanzienlijk is gewijzigd, of er een belangrijke wijziging in de distributie van de waarden voor een kolom is, *vervolgens* is het tijd om het bijwerken van statistieken.

Omdat er geen dynamische Beheerweergave om te bepalen of de gegevens in de tabel is gewijzigd sinds de laatste tijd statistische gegevens zijn bijgewerkt, kan weet de leeftijd van uw statistieken bieden met deel van de afbeelding.  U kunt de volgende query uit om te bepalen van de laatste keer dat de statistieken zijn bijgewerkt voor elke tabel.  

> [!NOTE]
> Houd er rekening mee dat als er een aanzienlijke wijzigingen in de distributie van de waarden voor een kolom, moet u bijwerken statistieken, ongeacht de laatste keer dat ze zijn bijgewerkt.  
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

**Datum van kolommen** in een datawarehouse, bijvoorbeeld meestal regelmatig moeten worden statistieken updates. Elke keer nieuwe rijen worden geladen in het datawarehouse, worden nieuwe load datums of transactiedatums toegevoegd. Deze de verdeling van de gegevens wijzigen en de statistieken niet actueel te maken.  Daarentegen statistieken op een kolom geslacht in een tabel van de klant mogelijk nooit moeten worden bijgewerkt. Ervan uitgaande dat de distributie is constant tussen klanten, wordt nieuwe rijen toe te voegen aan de variant van de tabel niet de verdeling van de gegevens wijzigen. Echter, als uw datawarehouse slechts één geslacht en een nieuwe vereiste resultaten in meerdere geslachten bevat, moet u bijwerken van statistieken voor de kolom geslacht.

Zie voor meer informatie, algemene richtlijnen voor [statistieken](/sql/relational-databases/statistics/statistics).

## <a name="implementing-statistics-management"></a>Statistieken management implementeren
Het is vaak een goed idee om uit te breiden van uw gegevens laden-proces om ervoor te zorgen dat de statistieken worden bijgewerkt aan het einde van de belasting. Het laden van gegevens is als tabellen vaakst van de grootte en/of de verdeling van waarden wijzigen. Daarom is dit een logische plaats voor het implementeren van bepaalde processen.

De volgende principes die worden geleverd voor het bijwerken van uw statistieken tijdens het laadproces:

* Zorg ervoor dat elke tabel geladen ten minste één statistieken-object bijgewerkt. Hiermee wordt de informatie over de grootte (aantal rijen en aantal pagina's) van tabel als onderdeel van de update van de statistieken bijgewerkt.
* Richt u op kolommen die deel uitmaken van de EU JOIN, GROUP BY, ORDER BY en DISTINCT.
* Houd rekening met bijwerken van kolommen 'oplopende sleutel' zoals transactie datums vaker, omdat deze waarden niet worden opgenomen in het histogram statistieken.
* Houd rekening met statische distributiekolommen minder vaak wordt bijgewerkt.
* Onthoud dat elk object statistiek is bijgewerkt in de reeks. Eenvoudig implementeren `UPDATE STATISTICS <TABLE_NAME>` is niet altijd ideaal zijn, vooral bij brede tabellen met veel objecten van statistieken.

Zie voor meer informatie, [Kardinaliteitsschatting](/sql/relational-databases/performance/cardinality-estimation-sql-server).

## <a name="examples-create-statistics"></a>Voorbeelden: Statistieken maken
Deze voorbeelden laten zien hoe u verschillende opties voor het maken van statistieken. De opties die u voor elke kolom gebruiken, is afhankelijk van de kenmerken van uw gegevens en hoe u de kolom wordt gebruikt in query's.

### <a name="create-single-column-statistics-with-default-options"></a>Statistieken voor één kolom maken met standaardopties
Voor het maken van statistieken op een kolom, moet u gewoon een naam voor het object statistieken en de naam van de kolom bevatten.

Deze syntaxis gebruikt al de standaardopties. Voorbeelden van SQL Data Warehouse standaard **20 procent** van de tabel bij het maken van statistieken.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="create-single-column-statistics-by-examining-every-row"></a>Statistieken voor één kolom maken door te controleren van elke rij
De samplefrequentie voor de standaardwaarde van 20 procent is voldoende voor de meeste situaties. U kunt echter de samplingfrequentie aanpassen.

Als u wilt de volledige tabel voorbeeld, gebruik de volgende syntaxis:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Statistieken voor één kolom maken door te geven van de grootte van de steekproef
U kunt ook de grootte van de steekproef opgeven als een percentage:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="create-single-column-statistics-on-only-some-of-the-rows"></a>Statistieken voor één kolom maken in slechts enkele van de rijen
U kunt ook statistieken maken voor een gedeelte van de rijen in de tabel. Dit is een gefilterde statistieken genoemd.

U kunt bijvoorbeeld gefilterde statistieken gebruiken wanneer u van plan bent om op te vragen van een specifieke partitie van een grote gepartitioneerde tabel. Als u statistieken maakt op basis van alleen de waarden van de partitie, wordt de nauwkeurigheid van de statistieken verbeteren en daarom de queryprestaties te verbeteren.

In dit voorbeeld worden statistieken in een bereik met waarden. De waarden kunnen eenvoudig worden gedefinieerd zodat deze overeenkomt met het bereik van waarden in een partitie.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Voor het queryoptimalisatieprogramma om te overwegen gefilterde statistieken wanneer deze de gedistribueerd queryplan kiest, moet de query in de definitie van het object statistieken passen. Met behulp van het vorige voorbeeld van de query waar component moet waarden tussen 2000101 en 20001231 col1 op te geven.
> 
> 

### <a name="create-single-column-statistics-with-all-the-options"></a>Statistieken voor één kolom maken met alle opties
U kunt ook de opties samen combineren. Het volgende voorbeeld wordt een gefilterde statistieken-object met een aangepaste samplegrootte:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Zie voor de volledige verwijzing [CREATE STATISTICS](/sql/t-sql/statements/create-statistics-transact-sql).

### <a name="create-multi-column-statistics"></a>Meerdere kolommen statistieken maken
Voor het maken van een object met meerdere kolommen statistieken, gewoon met de eerdere voorbeelden, maar meer kolommen opgeven.

> [!NOTE]
> Het histogram, wordt gebruikt voor het aantal rijen in het queryresultaat schatten, is alleen beschikbaar voor de eerste kolom die worden vermeld in de objectdefinitie van statistieken.
> 
> 

In dit voorbeeld wordt het histogram is op *product\_categorie*. Statistieken voor cross-kolom worden berekend op *product\_categorie* en *product\_sub_category*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Omdat er een correlatie tussen *product\_categorie* en *product\_sub\_categorie*, een object met meerdere kolommen statistieken kan nuttig zijn als deze kolommen zijn toegankelijk op hetzelfde moment.

### <a name="create-statistics-on-all-columns-in-a-table"></a>Statistieken maken voor alle kolommen in een tabel
Eén manier om statistieken te maken is het uitgeven van CREATE STATISTICS opdrachten na het maken van de tabel:

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

### <a name="use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>Een opgeslagen procedure gebruiken om statistieken te maken voor alle kolommen in een database
SQL Data Warehouse heeft geen een in het systeem opgeslagen procedure die gelijk is aan sp_create_stats in SQL Server. Deze opgeslagen procedure maakt een object van de statistieken één kolom voor elke kolom van de database die nog geen statistieken.

Het volgende voorbeeld ziet u een aan de slag met het ontwerpen van uw databases. U kunt deze aanpassen aan uw behoeften:

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type IS NULL
BEGIN
    SET @create_type = 1;
END;

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
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+CONVERT(varchar(4),@sample_pct)+' PERCENT' AS VARCHAR(8000))
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

Roep de procedure voor het maken van statistieken voor alle kolommen in de tabel met de standaardinstellingen.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 1, NULL;
```
Voor het maken van statistieken voor alle kolommen in de tabel met behulp van een fullscan, moet u deze procedure aanroept:

```sql
EXEC [dbo].[prc_sqldw_create_stats] 2, NULL;
```
Voer Voorbeeldstatistieken maken voor alle kolommen in de tabel, 3 en het percentage van het voorbeeld.  Deze procedures maakt gebruik van een samplefrequentie 20 procent.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 3, 20;
```


Steekproef statistieken over alle kolommen te maken. 

## <a name="examples-update-statistics"></a>Voorbeelden: Update statistics
Voor het bijwerken van statistieken, kunt u het volgende doen:

- Een object van de statistieken worden bijgewerkt. Geef de naam van de statistieken-object dat u wilt bijwerken.
- Alle objecten voor statistieken in een tabel bijwerken. Geef de naam van de tabel in plaats van een specifieke statistieken-object.

### <a name="update-one-specific-statistics-object"></a>Object voor een specifieke statistieken bijwerken
Gebruik de volgende syntaxis om bij te werken van een specifieke statistieken-object:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Door het bijwerken van statistieken voor specifieke objecten, kunt u de tijd en resources die nodig zijn voor het beheren van statistieken minimaliseren. Hiervoor moeten enkele beschouwd kiezen van de beste objecten statistieken bij te werken.

### <a name="update-all-statistics-on-a-table"></a>Alle statistische gegevens in een tabel bijwerken
Dit bevat een eenvoudige methode voor het bijwerken van de statistieken-objecten in een tabel:

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS dbo.table1;
```

Deze verklaring is eenvoudig te gebruiken. Let erop dat het bijwerken van *alle* statistieken voor de tabel en kan daarom meer werk dan nodig is uitgevoerd. Als de prestaties niet een probleem is, is dit de eenvoudigste en meest complete manier om te garanderen dat de statistieken bijgewerkt zijn.

> [!NOTE]
> Tijdens het bijwerken van alle statistische gegevens in een tabel, biedt SQL Data Warehouse een scan, voorbeeld van de tabel voor elk object statistieken. Als de tabel groot is en veel kolommen en veel statistieken bevat, kan het efficiënter om bij te werken van afzonderlijke statistieken op basis van behoeften zijn.
> 
> 

Voor een implementatie van een `UPDATE STATISTICS` procedure, Zie [tijdelijke tabellen](sql-data-warehouse-tables-temporary.md). De implementatie-methode is enigszins afwijken van de voorgaande `CREATE STATISTICS` procedure, maar het resultaat hetzelfde is.

Zie voor de volledige syntaxis [Update Statistics](/sql/t-sql/statements/update-statistics-transact-sql).

## <a name="statistics-metadata"></a>De metagegevens van statistieken
Er zijn verschillende systeemweergaven en functies die u gebruiken kunt om informatie over statistieken te vinden. U kunt bijvoorbeeld zien als een object statistieken mogelijk verouderd met behulp van de functie statistieken-datum om te zien wanneer statistieken laatst zijn gemaakt of bijgewerkt.

### <a name="catalog-views-for-statistics"></a>Weergaven voor statistieken van catalogus
Deze systeemweergaven bevatten informatie over statistieken:

| Catalogusweergave | Beschrijving |
|:--- |:--- |
| [sys.columns](/sql/relational-databases/system-catalog-views/sys-columns-transact-sql) |Een rij voor elke kolom. |
| [sys.objects](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Een rij voor elk object in de database. |
| [sys.schemas](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Een rij voor elk schema in de database. |
| [sys.Stats](/sql/relational-databases/system-catalog-views/sys-stats-transact-sql) |Een rij voor elk object statistieken. |
| [sys.stats_columns](/sql/relational-databases/system-catalog-views/sys-stats-columns-transact-sql) |Een rij voor elke kolom in het object statistieken. Koppelingen naar sys.columns. |
| [sys.tables](/sql/relational-databases/system-catalog-views/sys-tables-transact-sql) |Een rij voor elke tabel (inclusief externe tabellen). |
| [sys.table_types](/sql/relational-databases/system-catalog-views/sys-table-types-transact-sql) |Een rij voor elk gegevenstype. |

### <a name="system-functions-for-statistics"></a>Systeemfuncties voor statistieken
Deze systeemfuncties zijn nuttig voor het werken met statistieken:

| Systeemfunctie | Beschrijving |
|:--- |:--- |
| [STATS_DATE](/sql/t-sql/functions/stats-date-transact-sql) |De datum is die het object statistieken voor het laatst is bijgewerkt. |
| [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql) |Samenvatting niveau en gedetailleerde informatie over de distributie van waarden zoals begrepen door het object statistieken. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Functies en statistieken kolommen combineren in één weergave
In deze weergave zorgt voor kolommen die betrekking op statistieken hebben en resultaten van de functie STATS_DATE() samen.

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

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS() voorbeelden
DBCC SHOW_STATISTICS() bevat gegevens die zijn opgeslagen in een object statistieken. Deze gegevens zijn afkomstig uit drie delen:

- Koptekst
- Dichtheid vector
- Histogram

De koptekst van metagegevens over de statistieken. Het histogram geeft de distributie van de waarden in de eerste kolom van de sleutel van het object statistieken. De dichtheid vector metingen cross-kolom correlatie. SQL Data Warehouse berekend kardinaliteit schattingen met een van de gegevens in het object statistieken.

### <a name="show-header-density-and-histogram"></a>Koptekst, een dichtheid en histogram weergeven
Dit eenvoudige voorbeeld ziet u alle drie de gedeelten van een object statistieken:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Een of meer onderdelen van DBCC SHOW_STATISTICS() weergeven
Als u alleen geïnteresseerd bent in specifieke onderdelen weergeven, gebruikt u de `WITH` component en geeft u welke onderdelen die u wilt zien:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() verschillen
DBCC SHOW_STATISTICS() wordt meer strikt in SQL Data Warehouse ten opzichte van SQL Server geïmplementeerd:

- Niet-gedocumenteerde functies worden niet ondersteund.
- Stats_stream kan niet worden gebruikt.
- Kan geen join-resultaten voor specifieke subreeksen van statistische gegevens. Bijvoorbeeld: (STAT_HEADER JOIN DENSITY_VECTOR).
- NO_INFOMSGS kan niet worden ingesteld voor de onderdrukking van het bericht.
- Vierkante haken om de namen van statistieken kan niet worden gebruikt.
- Kan geen kolomnamen gebruiken om statistieken objecten te identificeren.
- Aangepaste foutpagina 2767 wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
Voor prestaties van query's verder verbeteren, Zie [uw workload controleren](sql-data-warehouse-manage-monitor.md)

