---
title: Indexeren van tabellen in SQL Data Warehouse | Microsoft Azure
description: Aan de slag met de tabel in Azure SQL Data Warehouse te indexeren.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 3e617674-7b62-43ab-9ca2-3f40c41d5a88
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: 672270536a7405e617edbcf5ec0e6eff68be7fde
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>Indexeren van tabellen in SQL Data Warehouse
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

SQL Data Warehouse biedt verschillende opties voor indexeren inclusief [geclusterde kolomopslagindexen][clustered columnstore indexes], [geclusterde indexen en niet-geclusterde indexen][clustered indexes and nonclustered indexes].  Bovendien biedt ook een geen indexoptie ook wel bekend als [heap][heap].  In dit artikel bevat informatie over de voordelen van elk indextype evenals tips voor het ophalen van de meeste prestaties buiten uw indexen. Zie [maken tabelsyntaxis] [ create table syntax] voor meer informatie over het maken van een tabel in SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Geclusterde columnstore-indexen
SQL Data Warehouse maakt standaard een geclusterde columnstore-index wanneer er geen indexopties voor zijn opgegeven in een tabel. De geclusterde columnstore-tabellen bieden zowel het hoogste niveau van de compressie van gegevens als de beste queryprestaties.  Geclusterde columnstore-tabellen wordt doorgaans leveren betere prestaties dan geclusterde index of heap-tabellen en zijn doorgaans de beste keuze voor grote tabellen.  Daarom is de geclusterde columnstore de beste plaats wordt gestart wanneer u niet zeker bent van het indexeren van uw tabel.  

Voor het maken van een geclusterde columnstore-tabel gewoon GECLUSTERDE COLUMNSTORE-INDEX in de component WITH, of laat u de component WITH uitgeschakeld:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Er zijn enkele scenario's waar de geclusterde columnstore niet mogelijk een goede optie:

* Columnstore-tabellen ondersteunen geen varchar(max), nvarchar(max) en varbinary(max).  Overweeg in plaats daarvan heap of geclusterde index.
* Columnstore-tabellen zijn mogelijk minder efficiënt voor tijdelijke gegevens.  U kunt heap en kan zelfs tijdelijke tabellen.
* Kleine tabellen met minder dan 100 miljoen rijen.  Houd rekening met heap tabellen.

## <a name="heap-tables"></a>Heap tabellen
Wanneer u tijdelijk gegevens naar SQL Data Warehouse laadt, kan een heap-tabel het proces versnellen.  Dit is omdat laadpogingen heaps zijn sneller dan met index-tabellen en in sommige gevallen is die het volgende lezen uit cache kan worden gedaan.  Als u alleen gegevens laadt om ze klaar te zetten voordat u meer transformaties uitvoert, kan het laden van de tabel naar een heap-tabel veel sneller zijn dan het downloaden van de gegevens naar een geclusterde columnstore-tabel. Bovendien laden van gegevens naar een [tijdelijke tabel] [ Temporary] ook veel sneller dan het laden van een tabel in de permanente opslag wordt geladen.  

Voor kleine opzoektabellen, kleiner dan 100 miljoen rijen, zinvol vaak heap tabellen zijn.  Cluster columnstore-tabellen begint met het bereiken van optimale compressie zodra er meer dan 100 miljoen rijen.

Een heap als tabel wilt maken, moet u gewoon HEAP opgeven in de component WITH:

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
Geclusterde indexen kunnen geclusterde columnstore-tabellen beter wanneer één rij moet snel worden opgehaald.  Voor query's waarbij één of heel weinig rij lookup vereist voor de prestaties met extreme snelheid wordt, kunt u een clusterindex of niet-geclusterde index van de secundaire.  Het nadeel van een geclusterde index is dat er alleen query's die het gebruik van een maximaal selectief filter voor de geclusterde indexkolom profiteert.  Ter verbetering van de filter van andere kolommen, kan een niet-geclusterde index naar andere kolommen worden toegevoegd.  Echter, wordt elke index die is toegevoegd aan een tabel ruimte en de verwerkingstijd toegevoegd aan belasting.

Een lijst van de geclusterde index maakt, moet u gewoon GECLUSTERDE INDEX opgeven in de component WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Om toe te voegen een niet-geclusterde index in een tabel, gebruikt u gewoon de volgende syntaxis:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Geclusterde columnstore-indexen optimaliseren
De geclusterde columnstore-tabellen zijn ingedeeld in de gegevens in segmenten.  Een hoge segment goede is van cruciaal belang bij het bereiken van optimale query-prestaties van een columnstore-tabel.  Segment kwaliteit kan worden bepaald door het aantal rijen in een rijgroep gecomprimeerde.  De kwaliteit van het segment is optimale wanneer er ten minste 100K rijen per rij gecomprimeerde groeperen en krijgen in prestaties als het aantal rijen per rij benadering voor de groep 1.048.576 rijen, dit is de meeste rijen die een rijgroep kan bevatten.

De onderstaande weergave kan worden gemaakt en gebruikt op uw systeem berekenen gemiddelde rijen per rij groeperen en een columnstore-indexen suboptimale cluster identificeren.  De laatste kolom in deze weergave wordt gegenereerd als SQL-instructie die kan worden gebruikt om de indexen opnieuw samenstellen.

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

Nu dat u de weergave hebt gemaakt, kunt u deze query om te identificeren tabellen met Rijgroepen met minder dan 100K rijen uitvoeren.  U kunt natuurlijk de drempelwaarde van 100 kB verhogen als u meer optimale segment kwaliteit zoekt. 

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Zodra u de query die u beginnen kunt met de gegevens kijken en analyseren van uw resultaten hebt uitgevoerd. Deze tabel wordt uitgelegd wat u in uw groep rij analyse wilt zoeken.

| Kolom | Deze gegevens gebruiken |
| --- | --- |
| [table_partition_count] |Als de tabel is gepartitioneerd, mogelijk om te zien hogere Open rijgroep telt u verwacht. Elke partitie in de distributie kan in theorie hebben een open rijgroep worden gekoppeld. Dit factor in uw analyse. Een kleine tabel die is is gepartitioneerd kan worden geoptimaliseerd door het verwijderen van de partitionering helemaal omdat dit zou leiden tot betere compressie. |
| [row_count_total] |Totale aantal rijen voor de tabel. Bijvoorbeeld, kunt u deze waarde voor het percentage van de rijen in de status van de gecomprimeerde berekenen. |
| [row_count_per_distribution_MAX] |Als alle rijen gelijkmatig worden verdeeld deze waarde wordt het doelaantal rijen per distributiepunt. Deze waarde met de compressed_rowgroup_count vergelijken. |
| [COMPRESSED_rowgroup_rows] |Totale aantal rijen in de columnstore-indeling voor de tabel. |
| [COMPRESSED_rowgroup_rows_AVG] |Als het gemiddelde aantal rijen aanzienlijk minder zijn dan het maximale aantal rijen voor een rijgroep is, klikt u vervolgens overwegen CTAS of ALTER INDEX REBUILD op de gegevens te comprimeren |
| [COMPRESSED_rowgroup_count] |Het aantal Rijgroepen columnstore-indeling. Als dit aantal zeer hoog ten opzichte van de tabel is is een indicatie dat de columnstore-dichtheid laag is. |
| [COMPRESSED_rowgroup_rows_DELETED] |Rijen zijn logisch in columnstore-indeling verwijderd. Het aantal hoge ten opzichte van de tabelgrootte van de, dan kunt u de partitie opnieuw te maken of opnieuw opbouwen van de index Hiermee verwijdert u deze fysiek. |
| [COMPRESSED_rowgroup_rows_MIN] |Gebruik deze in combinatie met de gemiddelde en maximale kolommen om te begrijpen van het bereik van waarden voor de Rijgroepen in uw columnstore. Een lage waarde boven de drempelwaarde laden (102,400 per partitie uitgelijnd distributie) wordt voorgesteld dat optimalisaties beschikbaar in de gegevens zijn geladen zijn |
| [COMPRESSED_rowgroup_rows_MAX] |Hierboven |
| [OPEN_rowgroup_count] |Open Rijgroepen is normaal. Een verwachten redelijkerwijs één OPEN rijgroep per tabeldistributie (60). Overmatige cijfers voorgesteld meerdere partities het laden van gegevens. Controleer de partitionering strategie om te controleren of deze klopt. |
| [OPEN_rowgroup_rows] |Elke rijgroep kan deze als maximaal 1.048.576 rijen hebben. Deze waarde wordt gebruikt om te zien hoe vol de open Rijgroepen momenteel |
| [OPEN_rowgroup_rows_MIN] |Groepen openen aangeven dat er gegevens worden de nieuwe wordt geladen in de tabel of dat de vorige belasting overgeslagen resterende rijen in deze rijgroep. Gebruik de minimum, maximum, AVG kolommen om te zien hoeveel gegevens is zat in OPEN rij groepen. Voor kleine tabellen kan 100% van alle gegevens zijn! In dat geval kan ALTER INDEX REBUILD om te zorgen dat de gegevens naar columnstore. |
| [OPEN_rowgroup_rows_MAX] |Hierboven |
| [OPEN_rowgroup_rows_AVG] |Hierboven |
| [CLOSED_rowgroup_rows] |Bekijk de rijen van de groep gesloten rij als een controle. |
| [CLOSED_rowgroup_count] |Het aantal gesloten Rijgroepen moet laag als een helemaal zijn zichtbaar zijn. Gesloten Rijgroepen kunnen worden geconverteerd naar gecomprimeerde Rijgroepen met de ALTER INDEX... REORGANISATIE opdracht. Dit is echter niet normaal gesproken vereist. Gesloten groepen worden automatisch geconverteerd naar columnstore Rijgroepen door het proces van achtergrond 'tuple mover'. |
| [CLOSED_rowgroup_rows_MIN] |Gesloten Rijgroepen moeten een zeer hoge opvulling snelheid hebben. Als de frequentie van de opvulling voor een rijgroep gesloten laag is, zijn verdere analyse van de columnstore is vereist. |
| [CLOSED_rowgroup_rows_MAX] |Hierboven |
| [CLOSED_rowgroup_rows_AVG] |Hierboven |
| [Rebuild_Index_SQL] |SQL opnieuw samenstellen columnstore-index voor een tabel |

## <a name="causes-of-poor-columnstore-index-quality"></a>Oorzaken van slechte columnstore-index kwaliteit
Als u tabellen hebt geïdentificeerd met slechte segment kwaliteit, wilt u de hoofdoorzaak te identificeren.  Hieronder vindt u enkele veelvoorkomende oorzaken van slechte segment kwaliteit:

1. Geheugendruk wanneer de index is samengesteld.
2. Groot aantal DML-bewerkingen
3. Kleine of load-bewerkingen doorsijpelen
4. Te veel partities

Deze factoren kunnen leiden tot een columnstore-index hebben aanzienlijk kleiner is dan de optimale 1 miljoen rijen per rij.  Ze kunnen ook leiden tot rijen aan de groep van de rij delta in plaats van een rijgroep gecomprimeerde gaan. 

### <a name="memory-pressure-when-index-was-built"></a>Geheugendruk wanneer de index is samengesteld.
Het aantal rijen per rijgroep gecomprimeerde rechtstreeks verband houden met de breedte van de rij en de hoeveelheid geheugen die beschikbaar is voor het verwerken van de rijgroep.  Wanneer rijen naar columnstore-tabellen worden geschreven onder geheugendruk, kan dit ten koste gaan van de kwaliteit van columnstore-segmenten.  Daarom is de aanbevolen procedure om te geven van de sessie die is schrijven naar de columnstore-index tabellen toegang tot zoveel mogelijk geheugen.  Omdat er een compromis tussen het geheugen en een gelijktijdigheid van taken, de instructies op de juiste geheugentoewijzing is afhankelijk van de gegevens in elke rij van de tabel, de gegevens datawarehouse eenheden zijn toegewezen aan uw systeem en het aantal sleuven gelijktijdigheid van taken die u kunt geven tot de sessie die schrijven van gegevens naar de tabel is.  Als een best practice is het raadzaam met xlargerc als u van DW300 gebruikmaakt of minder largerc starten als u DW400 DW600 en mediumrc als u van DW1000 gebruikmaakt en hoger.

### <a name="high-volume-of-dml-operations"></a>Groot aantal DML-bewerkingen
Een groot aantal DML-bewerkingen die bijwerken en verwijderen van rijen kunt inefficiëntie door in de columnstore introduceren. Dit is vooral van toepassing wanneer het merendeel van de rijen in de rijgroep van een zijn gewijzigd.

* Verwijderen van een rij uit een rijgroep gecomprimeerde alleen logisch, wordt de rij gemarkeerd als verwijderd. De rij is pas in de rijgroep gecomprimeerde de partitie of de tabel opnieuw wordt opgebouwd.
* Invoegen van een rij wordt de rij wordt toegevoegd aan een interne rowstore tabel met de naam van een rij delta-groep. De ingevoegde rij is niet geconverteerd naar columnstore totdat de rij delta-groep vol is en is gemarkeerd als gesloten. Rijgroepen worden gesloten nadat deze de maximale capaciteit van 1.048.576 rijen bereiken. 
* Bijwerken van een rij in de columnstore-indeling wordt verwerkt als een logische verwijderen en vervolgens een INSERT-bewerking. De ingevoegde rij kan worden opgeslagen in de delta-store.

Update batch verwerkt en voeg de bewerkingen die groter is dan de drempelwaarde voor bulksgewijs van 102.400 rijen per partitie uitgelijnde distributie rechtstreeks naar de columnstore-indeling worden geschreven. Echter, ervan uitgaande dat een gelijkmatige verdeling, moet u worden meer dan 6.144 miljoen rijen in één bewerking hiervoor wijzigen. Als het aantal rijen voor een bepaalde partitie uitgelijnd is distributie minder dan 102,400, en vervolgens de rijen naar de delta-store gaat en er blijven totdat voldoende rijen zijn ingevoegd of gewijzigd om aan te sluiten van de rijgroep of de index is opnieuw opgebouwd.

### <a name="small-or-trickle-load-operations"></a>Kleine of load-bewerkingen doorsijpelen
Kleine wordt geladen dat stroom in SQL Data Warehouse worden soms ook wel doorsijpelen laadt. Ze vertegenwoordigen meestal een nabije constante stream met gegevens die door het systeem wordt ingenomen. Omdat deze stroom in de buurt continue wordt is het volume van de rijen echter niet bijzonder groot. De gegevens zijn vaak aanzienlijk onder de drempelwaarde die is vereist voor een directe werklast voor columnstore-indeling.

In deze gevallen is het vaak beter om de gegevens eerst terechtkomen in Azure blob-opslag en laat deze verzamelen voordat ze worden geladen. Deze techniek is vaak bekend als *micro batchverwerking*.

### <a name="too-many-partitions"></a>Te veel partities
Een andere factor is de impact van partitioneren op uw geclusterde columnstore-tabellen.  Voordat het partitioneren verdeelt SQL Data Warehouse al uw gegevens in 60 databases.  Partitioneren van verdeelt uw gegevens verder.  Als partitioneren van uw gegevens, moet u rekening houden met die **elke** partitie moet ten minste 1 miljoen rijen profiteren van een geclusterde columnstore-index.  Als u uw tabel in 100 partities partitioneren, wordt de tabel ten minste 6 miljard rijen moet profiteren van een geclusterde columnstore-index (60 distributies * 100 partities * 1 miljoen rijen). Als uw 100 partitietabel geen 6 miljard rijen Verminder het aantal partities of gebruik in plaats hiervan een heap-tabel.

Zodra de tabellen met enkele gegevens zijn geladen, volg de onderstaande stappen om te bepalen en tabellen met columnstore-indexen suboptimale cluster opnieuw maken.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Opnieuw opbouwen van indexen segment kwaliteitsverbetering
### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Stap 1: Identificeren of gebruiker dat gebruikmaakt van de juiste bronklasse maken
Een snelle manier onmiddellijk kwaliteitsverbetering segment is de index opnieuw maken.  De SQL die is geretourneerd door de bovenstaande weergave resulteert in een instructie ALTER INDEX REBUILD die kan worden gebruikt om de indexen opnieuw samenstellen.  Wanneer uw indexen opnieuw opbouwen, zorg er dan voor dat voldoende geheugen aan de sessie die uw index wordt opnieuw toe te wijzen.  U doet dit door de bronklasse van een gebruiker met machtigingen voor het opnieuw samenstellen van de index voor deze tabel de aanbevolen minimale te verhogen.  De bronklasse van de database-eigenaar-gebruiker kan niet worden gewijzigd, dus als u een gebruiker op het systeem niet hebt gemaakt, moet u eerst te doen.  Het is raadzaam minimum is xlargerc als u van DW300 gebruikmaakt of minder largerc als u DW400 DW600 en mediumrc als u van DW1000 gebruikmaakt en hoger.

Hieronder volgt een voorbeeld van hoe meer geheugen toewijzen aan een gebruiker door te verhogen van de bronklasse.  Voor meer informatie over resource klassen en het maken van een nieuwe gebruiker vindt u in de [gelijktijdigheid en werkbelasting management] [ Concurrency] artikel.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Stap 2: De geclusterde columnstore-indexen opnieuw samenstellen met hogere resource klasse gebruiker
Meld u aan als de gebruiker uit stap 1 (bijvoorbeeld LoadUser), die wordt nu gebruikt een hogere bronklasse en voer de instructies ALTER INDEX.  Zorg ervoor dat deze gebruiker heeft de machtiging ALTER aan de tabellen waarbij de index wordt opnieuw opgebouwd.  Deze voorbeelden laten zien hoe u de volledige columnstore-index opnieuw worden opgebouwd of het opnieuw opbouwen van een enkele partitie. Op grote tabellen is het meer praktisch om op te bouwen indexen één partitie op een tijdstip.

U kunt ook in plaats van het opnieuw opbouwen van de index, u kan de tabel kopiëren naar een nieuwe tabel met [CTAS][CTAS].  Welke het beste is? Voor grote hoeveelheden gegevens, [CTAS] [ CTAS] is doorgaans sneller dan [ALTER INDEX][ALTER INDEX]. Voor kleinere volumes van gegevens, [ALTER INDEX] [ ALTER INDEX] gemakkelijker te gebruiken en won't, moet u wisselen uit de tabel.  Zie **opnieuw opbouwen van indexen met CTAS en partitie overschakelingen** hieronder voor meer informatie over het opnieuw opbouwen van indexen met CTAS.

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

Opnieuw opbouwen van een index in SQL Data Warehouse is een offline bewerking.  Voor meer informatie over het opnieuw opbouwen van indexen, Zie de sectie ALTER INDEX REBUILD in [Columnstore-indexen defragmentatie][Columnstore Indexes Defragmentation], en [ALTER INDEX] [ ALTER INDEX].

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Stap 3: Controleren of de kwaliteit van de geclusterde columnstore-segment is verbeterd
Voer de query welke geïdentificeerde tabel met slecht segmenteren kwaliteit en controleer of segment kwaliteit is verbeterd.  Als het segment kwaliteit kon niet worden verbeterd, kan het zijn dat de rijen in de tabel extra breed zijn.  Overweeg het gebruik van een hogere bronklasse of DWU wanneer uw indexen opnieuw opbouwen.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Opnieuw opbouwen van indexen met CTAS en partitie overschakelen
In dit voorbeeld wordt [CTAS] [ CTAS] en partitie overschakelen op te bouwen van een partitie van tabel. 

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

Voor meer informatie over het opnieuw maken met behulp van partities `CTAS`, Zie de [partitie] [ Partition] artikel.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie de artikelen op [tabel overzicht][Overview], [tabel gegevenstypen][Data Types], [distribueren van een tabel][Distribute], [partitioneren van een tabel][Partition], [tabelstatistieken onderhouden] [ Statistics] en [tijdelijke tabellen][Temporary].  Zie voor meer informatie over best practices, [aanbevolen procedures van SQL Data Warehouse][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[clustered indexes and nonclustered indexes]: https://msdn.microsoft.com/library/ms190457.aspx
[create table syntax]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indexes Defragmentation]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[clustered columnstore indexes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
