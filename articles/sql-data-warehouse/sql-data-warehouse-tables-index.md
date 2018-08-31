---
title: Indexeren van tabellen in Azure SQL Data Warehouse | Microsoft Azure
description: Aanbevelingen en voorbeelden voor het indexeren van tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: d709acfe378583a21b72971f465e4b5d73818bcd
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307725"
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>Indexeren van tabellen in SQL Data Warehouse
Aanbevelingen en voorbeelden voor het indexeren van tabellen in Azure SQL Data Warehouse.

## <a name="what-are-index-choices"></a>Wat zijn de index-opties?

SQL Data Warehouse biedt verschillende opties voor indexeren met inbegrip van [geclusterde columnstore-indexen](/sql/relational-databases/indexes/columnstore-indexes-overview), [geclusterde indexen en niet-geclusterde indexen](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described), en een niet-index ook wel bekend als optie [heap ](/sql/relational-databases/indexes/heaps-tables-without-clustered-indexes).  

Zie voor informatie over het maken van een tabel met een index die de [CREATE TABLE (Azure SQL Data Warehouse)](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) documentatie.

## <a name="clustered-columnstore-indexes"></a>Geclusterde columnstore-indexen
SQL Data Warehouse maakt standaard een geclusterde columnstore-index wanneer er geen indexopties zijn opgegeven in een tabel. Geclusterde columnstore-tabellen bieden zowel het hoogste niveau van de compressie van gegevens als de meest algemene prestaties van query's.  Geclusterde columnstore-tabellen worden de geclusterde index of heap-tabellen in het algemeen beter en hebben meestal de beste keuze voor grote tabellen.  Geclusterde columnstore is daarom de beste plek om te starten wanneer u niet zeker bent van uw tabel indexeren.  

Voor het maken van een geclusterde columnstore-tabel, gewoon GECLUSTERDE COLUMNSTORE-INDEX opgeven in de WITH-clausule of laat de WITH-clausule uit:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Er zijn enkele scenario's waarbij geclusterde columnstore niet mogelijk een goede optie:

- Columnstore-tabellen ondersteunen geen varchar(max), nvarchar(max) en varbinary(max). Overweeg in plaats daarvan heap of geclusterde index.
- Columnstore-tabellen is mogelijk minder efficiënt voor tijdelijke gegevens. Houd rekening met heap en kan zelfs tijdelijke tabellen.
- Kleine tabellen met minder dan 100 miljoen rijen. Kunt u overwegen een heap-tabellen.

## <a name="heap-tables"></a>Heap-tabellen
Wanneer u tijdelijk gegevens in SQL Data Warehouse terechtkomen zijn, merkt u misschien dat het proces versnellen met behulp van een heap-tabel maakt. Dit is omdat loads naar heaps sneller dan indextabellen en in sommige gevallen is die het volgende lezen uit cache kan worden gedaan.  Als u het laden van gegevens alleen als u wilt deze fase voordat u meer transformaties uitvoert, is het laden van de tabel aan de heap-tabel veel sneller dan het laden van de gegevens naar een geclusterde columnstore-tabel. Bovendien het laden van gegevens naar een [tijdelijke tabel](sql-data-warehouse-tables-temporary.md) wordt geladen sneller dan het laden van een tabel in de permanente opslag.  

Voor kleine opzoektabellen, minder dan 100 miljoen rijen, zinvol vaak heap-tabellen zijn.  Cluster columnstore-tabellen beginnen met het bereiken van optimale compressie zodra er meer dan 100 miljoen rijen.

Geef HEAP in de WITH-component voor het maken van een heap-tabel:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Geclusterde en niet-geclusterde indexen
Geclusterde indexen kunnen leveren betere prestaties dan geclusterde columnstore-tabellen wanneer één rij moet snel worden opgehaald. Voor query's waarbij één of een enkele rij opzoekfunctie vereist op prestaties met extreem snelle is, houd rekening met een clusterindex of niet-geclusterde index van de secundaire. Het nadeel van een geclusterde index is dat alleen query's die baat hebben die een zeer selectief filter voor de geclusterde index-kolom gebruiken. Ter verbetering van filteren op andere kolommen kan niet-geclusterde index worden toegevoegd aan andere kolommen. Elke index die wordt toegevoegd aan een tabel toegevoegd, ruimte en de verwerkingstijd te laden.

Een geclusterde index als tabel wilt maken, moet u gewoon GECLUSTERDE INDEX opgeven in de WITH-clausule:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Om toe te voegen een niet-geclusterde index op een tabel, gebruikt u gewoon de volgende syntaxis:

```SQL
CREATE INDEX zipCodeIndex ON myTable (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Geclusterde columnstore-indexen optimaliseren
Geclusterde columnstore-tabellen zijn ingedeeld in de gegevens in segmenten.  Met hoge segmentkwaliteit is van essentieel belang voor het bereiken van optimale queryprestaties op een columnstore-tabel.  Segmentkwaliteit kan worden bepaald door het aantal rijen in een gecomprimeerde Rijengroep.  Segmentkwaliteit is optimale wanneer er ten minste 100K rijen per gecomprimeerde Rijgroepen groep en toegang in de prestaties als het aantal rijen per rij benadering voor de groep 1.048.576 rijen, dit is de meeste rijen die een rijgroep kan bevatten.

De onderstaande weergave kan worden gemaakt en die op uw systeem wordt gebruikt voor het berekenen van de gemiddelde rijen per rij groeperen en identificeren van alle suboptimale geclusterde columnstore-indexen.  De laatste kolom in deze weergave wordt een SQL-instructie die kan worden gebruikt om uw indexen opnieuw gegenereerd.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,    COUNT(DISTINCT rg.[partition_number])                    AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,    CEILING    ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Nu dat u de weergave hebt gemaakt, kunt u deze query voor het identificeren van tabellen met Rijgroepen met minder dan 100K rijen uitvoeren. U kunt natuurlijk de drempelwaarde van 100 kB verhogen als u meer optimale segmentkwaliteit zoekt. 

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Nadat u de query die u beginnen kunt met de gegevens kijken en analyseer de resultaten hebt uitgevoerd. Deze tabel wordt uitgelegd wat u zoekt in uw groep rij analyse.

| Kolom | Het gebruik van deze gegevens |
| --- | --- |
| [table_partition_count] |Als de tabel is gepartitioneerd, mag u verwachten dat om te zien van de groep met hogere Open rijen wordt geteld. Elke partitie in de distributie kan in theorie een open rij-groep die is gekoppeld aan deze hebben. Houd hier rekening in uw analyse. Een kleine tabel die is zijn gepartitioneerd kan worden geoptimaliseerd door het verwijderen van de partities kan worden overgeslagen als dit zou leiden tot betere compressie. |
| [row_count_total] |Totale aantal rijen voor de tabel. Bijvoorbeeld, kunt u deze waarde om percentage van de rijen in de gecomprimeerde toestand te berekenen. |
| [row_count_per_distribution_MAX] |Als alle rijen gelijkmatig worden gedistribueerd deze waarde wordt het beoogd aantal rijen per distributiepunt. Deze waarde met de compressed_rowgroup_count vergelijken. |
| [COMPRESSED_rowgroup_rows] |Totale aantal rijen in de columnstore-indeling voor de tabel. |
| [COMPRESSED_rowgroup_rows_AVG] |Als het gemiddelde aantal rijen aanzienlijk minder zijn dan het maximum aantal rijen voor een Rijengroep is, overweeg dan om de gegevens comprimeren met behulp van CTAS of ALTER INDEX REBUILD |
| [COMPRESSED_rowgroup_count] |Het aantal Rijgroepen in columnstore-indeling. Als dit aantal zeer hoog ten opzichte van de tabel is is een indicator dat de columnstore-dichtheid laag is. |
| [COMPRESSED_rowgroup_rows_DELETED] |Rijen zijn logisch in columnstore-indeling verwijderd. Als het nummer van de hoge ten opzichte van de tabelgrootte, kunt u de partitie opnieuw te maken of opnieuw opbouwen van de index Hiermee verwijdert u deze fysiek. |
| [COMPRESSED_rowgroup_rows_MIN] |Gebruik deze in combinatie met de gemiddelde en maximale kolommen om te begrijpen van het bereik van waarden voor de Rijgroepen in uw columnstore. Een lage waarde boven de drempelwaarde voor het laden (102,400 per partitie uitgelijnd distributie) kan erop wijzen dat optimalisaties beschikbaar in het laden van gegevens zijn |
| [COMPRESSED_rowgroup_rows_MAX] |Als hierboven |
| [OPEN_rowgroup_count] |Open Rijgroepen is normaal. Een zou groep van een OPEN rijen per tabeldistributie (60) redelijkerwijs verwachten. Overmatige getallen voorstellen over meerdere partities het laden van gegevens. Controleer de strategie voor partitioneren om te controleren of het is goed |
| [OPEN_rowgroup_rows] |Elke Rijengroep kan als maximaal 1.048.576 rijen in deze hebben. Gebruik deze waarde om te zien hoe vol de open Rijgroepen zijn momenteel |
| [OPEN_rowgroup_rows_MIN] |Open groepen geven aan dat gegevens is een nieuwe wordt geladen in de tabel of dat de vorige belasting overgeslagen rijen resterend in deze rijgroep. Gebruik de MIN, MAX, Gem. kolommen om te zien hoeveel gegevens in OPEN is za rij groepen. Voor kleine tabellen kan het 100% van alle gegevens zijn! In welk geval ALTER INDEX REBUILD om af te dwingen van de gegevens naar columnstore. |
| [OPEN_rowgroup_rows_MAX] |Als hierboven |
| [OPEN_rowgroup_rows_AVG] |Als hierboven |
| [CLOSED_rowgroup_rows] |Bekijk de rijen van de groep gesloten rij als een controle. |
| [CLOSED_rowgroup_count] |Het aantal gesloten Rijgroepen moet laag als een helemaal worden gezien. Gesloten Rijgroepen kunnen worden geconverteerd naar gecomprimeerde Rijgroepen nodig met behulp van de INDEX wijzigen... Opdracht INDELEN. Dit is echter niet normaal gesproken vereist. Gesloten groepen worden automatisch geconverteerd naar columnstore Rijgroepen door het "tuple mover" achtergrondproces. |
| [CLOSED_rowgroup_rows_MIN] |Gesloten Rijgroepen moet een zeer hoge opvulling-tarief. Als de frequentie van de opvulling voor een gesloten rijgroep laag is, zijn verdere analyse van de columnstore is vereist. |
| [CLOSED_rowgroup_rows_MAX] |Als hierboven |
| [CLOSED_rowgroup_rows_AVG] |Als hierboven |
| [Rebuild_Index_SQL] |SQL opnieuw opbouwen van columnstore-index voor een tabel |

## <a name="causes-of-poor-columnstore-index-quality"></a>Oorzaken van slechte columnstore-index kwaliteit
Als u tabellen met slechte segmentkwaliteit hebt geïdentificeerd, wilt u de hoofdoorzaak identificeren.  Hieronder vindt u enkele veelvoorkomende oorzaken van slechte segmentkwaliteit:

1. Geheugendruk wanneer de index is gemaakt
2. Groot aantal DML-bewerkingen
3. Kleine of doorsijpelen load-bewerkingen
4. Te veel partities

Deze factoren kunnen leiden tot een columnstore-index hebben aanzienlijk kleiner is dan de optimale 1 miljoen rijen per rijgroep. Ze kunnen ook leiden tot rijen naar de delta-rij-groep in plaats van een gecomprimeerde Rijengroep. 

### <a name="memory-pressure-when-index-was-built"></a>Geheugendruk wanneer de index is gemaakt
Het aantal rijen per gecomprimeerde Rijgroepen groep zijn direct gerelateerd aan de breedte van de rij en de hoeveelheid geheugen die beschikbaar is voor het verwerken van de rijgroep.  Wanneer rijen naar columnstore-tabellen worden geschreven onder geheugendruk, kan dit ten koste gaan van de kwaliteit van columnstore-segmenten.  De aanbevolen procedure is daarom dat de sessie die wordt geschreven naar de columnstore-index tabellen toegang tot net zo veel geheugen mogelijk.  Omdat er een afweging tussen het geheugen en gelijktijdigheid van taken, de instructies op de juiste geheugentoewijzing is afhankelijk van de gegevens in elke rij van de tabel, de datawarehouse-eenheden toegewezen aan uw systeem en het aantal gelijktijdige plekken gebruiken die u kunt geven tot de sessie die schrijven van gegevens naar de tabel is.  Als een best practice kunt het beste beginnen met xlargerc als u DW300 of minder, largerc als u DW400 naar DW600 en mediumrc als u DW1000 en hoger.

### <a name="high-volume-of-dml-operations"></a>Groot aantal DML-bewerkingen
Een groot aantal DML-bewerkingen die bijwerken en verwijderen van rijen kunt inefficiëntie in de columnstore introduceren. Dit is vooral van toepassing als het merendeel van de rijen in een Rijengroep zijn gewijzigd.

- De rij verwijderen van een rij uit een groep gecomprimeerde Rijgroepen alleen logisch worden gemarkeerd als verwijderd. De rij blijft in de groep gecomprimeerde Rijgroepen totdat de partitie of de tabel is opnieuw opgebouwd.
- De rij invoegen van een rij aan een interne rowstore-tabel met de naam een delta-rij-groep worden toegevoegd. De ingevoegde rij is niet geconverteerd naar columnstore totdat de delta-rijgroep vol is en is gemarkeerd als gesloten. Rijgroepen worden gesloten zodra ze zich bij de maximale capaciteit van 1.048.576 rijen. 
- Bijwerken van een rij in de columnstore-indeling wordt verwerkt als een logische verwijderen en vervolgens een insert. De ingevoegde rij kan worden opgeslagen in de delta-store.

Batchgewijze update en insert-bewerkingen die groter zijn dan de drempelwaarde bulksgewijs van 102.400 rijen per distributie uitgelijnd Ga rechtstreeks naar de columnstore-indeling. Echter moet als een evenredige verdeling, u meer dan 6.144 miljoen rijen in één bewerking hiervoor wijzigt. Als het aantal rijen voor een bepaalde uitgelijnd distributie minder dan 102,400 is vervolgens gaat de rijen u naar de delta-store andstay er totdat voldoende rijen zijn ingevoegd of gewijzigd om aan te sluiten van de rijgroep of de index is opnieuw opgebouwd.

### <a name="small-or-trickle-load-operations"></a>Kleine of doorsijpelen load-bewerkingen
Kleine wordt geladen dat flow in SQL Data Warehouse zijn soms ook wel bekend als doorsijpelen geladen. Ze vertegenwoordigen meestal een bijna constante stream met gegevens die door het systeem wordt opgenomen. Als u deze stroom is in de buurt continue is het volume van de rijen echter niet bijzonder groot. De gegevens zijn vaak dan niet meer aanzienlijk onder de drempelwaarde die is vereist voor een directe laden naar columnstore-indeling.

In deze gevallen is het vaak beter de gegevens eerst worden neergezet in Azure blob-opslag en laat verzamelen voordat ze worden geladen. Deze techniek wordt vaak ook wel *micro-batching*.

### <a name="too-many-partitions"></a>Te veel partities
Een andere factor zijn de gevolgen van het partitioneren van de geclusterde columnstore-tabellen.  Voordat u partitioneren verdeelt SQL Data Warehouse uw gegevens al in 60 databases.  Uw gegevens partitioneren verder worden verdeeld.  Als u uw gegevens partitioneren, overweeg dan om die **elke** partitie moet ten minste 1 miljoen rijen om te profiteren van een geclusterde columnstore-index.  Als u uw tabel in 100 partities partitioneren, wordt de tabel ten minste 6 miljard rijen die u moet wilt profiteren van een geclusterde columnstore-index (60 distributies * 100 partities * 1 miljoen rijen). Als uw tabel 100 partities heeft geen 6 miljard rijen, het aantal partities verminderen of overwegen een heap-tabel in plaats daarvan.

Zodra uw tabellen met gegevens zijn geladen, volg de onderstaande stappen om te bepalen en opnieuw maken van tabellen met suboptimale geclusterde columnstore-indexen.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Opnieuw opbouwen van indexen voor het verbeteren van segmentkwaliteit
### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Stap 1: Bepalen of gebruikers dat gebruikmaakt van de juiste resourceklasse maken
Er is een snelle manier voor het verbeteren van segmentkwaliteit onmiddellijk de index opnieuw opbouwen.  De SQL-code die wordt geretourneerd door de bovenstaande weergave retourneert een instructie ALTER INDEX REBUILD, die kan worden gebruikt om uw indexen opnieuw samenstellen. Wanneer uw-indexen herbouwen, moet u dat u onvoldoende geheugen om de sessie die de index van opnieuw gebouwde toewijzen.  U doet dit door de resourceklasse van een gebruiker met machtigingen voor het opnieuw opbouwen van de index voor deze tabel aan de aanbevolen minimale te verhogen. De resourceklasse van de database-eigenaar-gebruiker kan niet worden gewijzigd, dus als u een gebruiker op het systeem niet hebt gemaakt, moet u dat eerst doen. De minimale aanbevolen resourceklasse is xlargerc als u DW300 of minder, largerc als u DW400 naar DW600 en mediumrc als u DW1000 en hoger.

Hieronder volgt een voorbeeld van hoe u meer geheugen toewijzen aan een gebruiker door hun resourceklasse. Als u wilt werken met resource-klassen, Zie [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Stap 2: Geclusterde columnstore-indexen opnieuw samenstellen met hogere resource klasse gebruiker
Meld u aan als de gebruiker uit stap 1 (bijvoorbeeld LoadUser), waarop nu met behulp van een hogere resourceklasse, en voer de instructies ALTER INDEX. Zorg ervoor dat deze gebruiker heeft de machtiging ALTER voor de tabellen waarbij de index wordt opnieuw opgebouwd. Deze voorbeelden laten zien hoe u de volledige columnstore-index opnieuw opbouwen of het opnieuw opbouwen van een enkele partitie. Bij grote tabellen is het meer praktische om op te bouwen indexen één partitie op een tijdstip.

U kunt ook in plaats van opnieuw opbouwen van de index, u kan de tabel kopiëren naar een nieuwe tabel [met behulp van CTAS](sql-data-warehouse-develop-ctas.md). Welke het beste is? Voor grote hoeveelheden gegevens, CTAS is doorgaans sneller dan [ALTER INDEX](/sql/t-sql/statements/alter-index-transact-sql). Voor kleinere hoeveelheden gegevens, ALTER INDEX is eenvoudiger te gebruiken en wordt niet hoeft te wisselen van de tabel. Zie **opnieuw opbouwen van indexen met CTAS en het wijzigen van de partitie** hieronder voor meer informatie over het opnieuw opbouwen van indexen met CTAS.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Opnieuw opbouwen van een index in SQL Data Warehouse is een offline bewerking.  Voor meer informatie over het opnieuw opbouwen van indexen, Zie de sectie ALTER INDEX REBUILD in [Columnstore-indexen defragmentatie](/sql/relational-databases/indexes/columnstore-indexes-defragmentation), en [ALTER INDEX](/sql/t-sql/statements/alter-index-transact-sql).

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Stap 3: Controleren segmentkwaliteit van geclusterde columnstore is verbeterd
Voer de query die geïdentificeerde tabel met slechte kwaliteit in segmenten en controleer of segmentkwaliteit is verbeterd.  Als het verbeteren van segmentkwaliteit is niet, kan het zijn dat de rijen in de tabel extra breed zijn.  Overweeg het gebruik van een hogere resourceklasse of DWU tijdens het opnieuw opbouwen van uw indexen.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Met CTAS en het wijzigen van de partitie-indexen herbouwen
In dit voorbeeld wordt de [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) instructie en partitie overschakelt naar een tabelpartitie opnieuw. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Zie voor meer informatie over het opnieuw maken met behulp van CTAS partities [gebruik van partities in SQL Data Warehouse](sql-data-warehouse-tables-partition.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwikkelen van tabellen [ontwikkelen tabellen](sql-data-warehouse-tables-overview.md).

