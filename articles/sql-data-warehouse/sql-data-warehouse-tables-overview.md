---
title: Het ontwerpen van tabellen - Azure SQL Data Warehouse | Microsoft Docs
description: Inleiding tot het ontwerpen van tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/15/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 1073e1b4ad38c4b05c9195cf4ea16ade7416fbce
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58133404"
---
# <a name="designing-tables-in-azure-sql-data-warehouse"></a>Het ontwerpen van tabellen in Azure SQL Data Warehouse

Meer informatie over belangrijke concepten voor het ontwerpen van tabellen in Azure SQL Data Warehouse. 

## <a name="determine-table-category"></a>Tabelcategorie bepalen 

Een [sterschema](https://en.wikipedia.org/wiki/Star_schema) ziet u de gegevens in tabellen feiten- en dimensietabellen. Sommige tabellen worden gebruikt voor de integratie- of faseringsomgevingen gegevens voordat deze wordt verplaatst naar een tabel met het feit of de dimensie. Bij het ontwerpen van een tabel, moet u bepalen of de gegevens van de tabel in een feit, dimensie of integratie tabel behoort. Dit besluit informeert de structuur van de juiste en de distributie. 

- **Feitentabellen** kwantitatieve gegevens bevatten die worden doorgaans gegenereerd in een transactionele systeem en vervolgens in het datawarehouse worden geladen. Bijvoorbeeld, een winkel verkoop genereert elke dag en vervolgens de gegevens worden geladen in een datawarehouse-feitentabel voor analyse.

- **Dimensietabellen** kenmerkgegevens bevatten die kunnen worden gewijzigd, maar meestal niet vaak worden gewijzigd. De naam en het adres van de klant worden bijvoorbeeld opgeslagen in een dimensietabel en bijgewerkt alleen wanneer de klant profiel wordt gewijzigd. Als u wilt de grootte van een grote feitentabel minimaliseren, hoeft de naam en het adres van de klant te worden in elke rij van een feitentabel. In plaats daarvan de feitentabel en de dimensietabel kunnen delen met een klant-ID. Een query kan deelnemen aan de twee tabellen om te koppelen van het profiel en de transacties van een klant. 

- **Integratie van tabellen** bieden een plaats voor het integreren of staging-gegevens. U kunt een integratie-tabel als een normale tabel, een externe tabel of een tijdelijke tabel maken. U kunt bijvoorbeeld gegevens naar een faseringstabel te laden, transformaties uitvoeren op de gegevens in fasering en vervolgens de gegevens in een productietabel invoegen.

## <a name="schema-and-table-names"></a>Schema- en tabelnamen
Schema's zijn een goede manier om tabellen, samen op dezelfde manier gebruikt.  Als u meerdere databases van een on-premises-oplossing naar SQL Data Warehouse migreert, werkt het het beste voor het migreren van alle van de dimensie-, feiten- en integratie van tabellen tot één schema in SQL Data Warehouse. Bijvoorbeeld, kunt u opslaan alle tabellen in de [WideWorldImportersDW](/sql/sample/world-wide-importers/database-catalog-wwi-olap) voorbeeld van datawarehouse binnen één schema met de naam wwi. De volgende code maakt een [gebruiker gedefinieerde schema](/sql/t-sql/statements/create-schema-transact-sql) wwi genoemd.

```sql
CREATE SCHEMA wwi;
```

Als u wilt weergeven van de organisatie van de tabellen in SQL Data Warehouse, kunt u feitelijke dimensie en int kunt gebruiken als voorvoegsel voor de tabelnamen van de. De volgende tabel ziet u enkele van de schema- en tabelnamen voor WideWorldImportersDW.  

| WideWorldImportersDW-tabel  | Tabeltype | SQL Data Warehouse |
|:-----|:-----|:------|:-----|
| Plaats | Dimensie | wwi.DimCity |
| Bestellen | Feit | wwi.FactOrder |


## <a name="table-persistence"></a>Persistentie van de tabel 

Tabellen opslaan van gegevens definitief in Azure Storage, tijdelijk in Azure Storage, of in een gegevensarchief externe naar datawarehouse.

### <a name="regular-table"></a>Gewone tabellen

Een gewone tabellen worden gegevens opgeslagen in Azure Storage als onderdeel van het datawarehouse. De tabel en de gegevens behouden, ongeacht of een sessie geopend is.  In dit voorbeeld maakt een normale tabel met twee kolommen. 

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Tijdelijke tabel
Een tijdelijke tabel is alleen beschikbaar voor de duur van de sessie. U kunt een tijdelijke tabel gebruiken om te voorkomen dat andere gebruikers tijdelijke resultaten zien en verminderen de noodzaak om op te schonen.  Tijdelijke tabellen gebruikmaken van lokale opslag te nemen voor snelle prestaties.  Zie voor meer informatie, [tijdelijke tabellen](sql-data-warehouse-tables-temporary.md).

### <a name="external-table"></a>Externe tabel
Een externe tabel verwijst naar gegevens die zich bevinden in Azure Storage-blob of Azure Data Lake Store. Wanneer gebruikt in combinatie met de instructie CREATE TABLE AS SELECT, importeert selecteren in een externe tabel gegevens in SQL Data Warehouse. Externe tabellen zijn daarom nuttig voor het laden van gegevens. Zie voor een zelfstudie laden [gebruik PolyBase om gegevens te laden vanuit Azure blob storage](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="data-types"></a>Gegevenstypen
SQL Data Warehouse ondersteunt de meest gebruikte gegevenstypen. Zie voor een lijst van de ondersteunde gegevenstypen, [gegevenstypen in CREATE TABLE verwijzing](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) in de instructie CREATE TABLE. Zie voor instructies over het gebruik van gegevenstypen [gegevenstypen](sql-data-warehouse-tables-data-types.md).

## <a name="distributed-tables"></a>Gedistribueerde tabellen
Een fundamenteel onderdeel van SQL Data Warehouse is de manier kunnen worden opgeslagen en worden toegepast op tabellen met [distributies](massively-parallel-processing-mpp-architecture.md#distributions).  SQL Data Warehouse ondersteunt drie methoden voor het distribueren van gegevens, round robin (standaard), hash- en gerepliceerd.

### <a name="hash-distributed-tables"></a>Hash-gedistribueerde tabellen
Een tabel distribueert rijen op basis van de waarde in de kolom van het distributiepunt. Een tabel is ontworpen voor het bereiken van hoge prestaties voor query's op grote tabellen. Er zijn diverse factoren om te overwegen bij het kiezen van een distributiekolom. 

Zie voor meer informatie, [ontwerprichtlijnen voor gedistribueerde tabellen](sql-data-warehouse-tables-distribute.md).

### <a name="replicated-tables"></a>Gerepliceerde tabellen
Een gerepliceerde tabel heeft een volledige kopie van de tabel beschikbaar op elk rekenknooppunt. Query's uitvoeren snel op gerepliceerde tabellen omdat joins in gerepliceerde tabellen niet te verplaatsen van gegevens hoeven. Replicatie vereist extra opslagruimte, maar dan wel en is het niet praktisch is voor grote tabellen. 

Zie voor meer informatie, [ontwerprichtlijnen voor gerepliceerde tabellen](design-guidance-for-replicated-tables.md).

### <a name="round-robin-tables"></a>Round robin-tabellen
Een round robin-tabel wordt rijen gelijkmatig verspreid over alle distributies. De rijen worden willekeurig gedistribueerd. Het laden van gegevens in een round robin-tabel is snel.  Query's vereisen echter meer verplaatsing van gegevens dan de andere distributiemethoden. 

Zie voor meer informatie, [ontwerprichtlijnen voor gedistribueerde tabellen](sql-data-warehouse-tables-distribute.md).

### <a name="common-distribution-methods-for-tables"></a>Algemene methoden van verkeersdistributie voor tabellen
De tabelcategorie is vaak bepaalt van welke optie te kiezen voor het distribueren van de tabel. 

| Tabelcategorie | Aanbevolen distributieoptie |
|:---------------|:--------------------|
| Feit           | Hash-distributiepunt gebruikt met geclusterde columnstore-index. Prestaties verbeteren door twee hash-tabellen zijn gekoppeld op dezelfde kolom van het distributiepunt. |
| Dimensie      | Gebruik voor kleinere tabellen zijn gerepliceerd. Als tabellen te groot om op te slaan op elk rekenknooppunt zijn, gebruikt u de hash-gedistribueerd. |
| Faseren        | Round robin gebruiken voor de tijdelijke tabel. De belasting met CTAS wordt snel. Zodra de gegevens zich in de faseringstabel, gebruikt u invoegen... Selecteer de om gegevens te verplaatsen naar Productietabellen. |

## <a name="table-partitions"></a>Tabelpartities
Een gepartitioneerde tabel wordt opgeslagen en bewerkingen worden uitgevoerd op de tabelrijen op basis van gegevensbereiken. Een tabel kan bijvoorbeeld worden gepartitioneerd op dag, maand of jaar. U kunt de prestaties van query's via partitie elimineren, waardoor een scan query om gegevens in een partitie te verbeteren. U kunt ook de gegevens via het overschakelen van de partitie onderhouden. Omdat de gegevens in SQL Data Warehouse is al gedistribueerd, kunnen er zijn te veel partities prestaties van query's vertragen. Zie voor meer informatie, [richtlijnen partitioneren](sql-data-warehouse-tables-partition.md).  Wanneer partitie schakelt over naar de tabel partities die zijn niet leeg zijn, overweeg het gebruik van de optie TRUNCATE_TARGET in uw [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) instructie als de bestaande gegevens worden afgekapt. De onderstaande code schakelopties in de getransformeerde dagelijkse gegevens in de SalesFact overschrijft alle bestaande gegevens. 

```sql
ALTER TABLE SalesFact_DailyFinalLoad SWITCH PARTITION 256 TO SalesFact PARTITION 256 WITH (TRUNCATE_TARGET = ON);  
```

## <a name="columnstore-indexes"></a>Columnstore-indexen
Standaard wordt een tabel in SQL Data Warehouse opgeslagen als een geclusterde columnstore-index. Deze vorm van de opslag van gegevens bereikt hoge gegevenscompressie en prestaties van query's op grote tabellen.  De geclusterde columnstore-index is meestal de beste keuze, maar in sommige gevallen een geclusterde index of een heap is de juiste opslagstructuur.  Een heap-tabel is vooral handig voor het laden van tijdelijke gegevens, zoals een tijdelijke tabel die wordt omgezet in een definitieve tabel.

Zie voor een lijst met functies columnstore [wat is er nieuw voor columnstore-indexen](/sql/relational-databases/indexes/columnstore-indexes-what-s-new). Zie voor betere prestaties voor columnstore-index [rijgroep kwaliteit voor columnstore-indexen optimaliseren](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

## <a name="statistics"></a>statistieken
Het queryoptimalisatieprogramma maakt gebruik van statistieken op kolomniveau bij het maken van het plan voor het uitvoeren van een query. Ter verbetering van prestaties van query's, is het belangrijk dat u hebt statistieken op afzonderlijke kolommen, met name kolommen in de query wordt gebruikt. [Het maken van statistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistics) gebeurt automatisch.  Echter wordt statistieken bij te werken niet automatisch gevraagd. Statistieken bijwerken nadat een groot aantal rijen zijn toegevoegd of gewijzigd. Bijvoorbeeld: statistieken worden bijgewerkt wanneer een belasting. Zie voor meer informatie, [statistieken richtlijnen](sql-data-warehouse-tables-statistics.md).

## <a name="commands-for-creating-tables"></a>Opdrachten voor het maken van tabellen
U kunt een tabel maken als een nieuwe, lege tabel. U kunt ook maken en vullen van een tabel met de resultaten van een select-instructie. Hier volgen de T-SQL-opdrachten voor het maken van een tabel.

| T-SQL-instructie | Description |
|:----------------|:------------|
| [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) | Hiermee maakt u een lege tabel met het definiëren van de kolommen in de tabel en opties. |
| [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql) | Hiermee maakt u een externe tabel. De definitie van de tabel wordt opgeslagen in SQL Data Warehouse. Gegevens in de tabel is opgeslagen in Azure Blob storage of Azure Data Lake Store. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) | Hiermee wordt een nieuwe tabel met de resultaten van een instructie select gevuld. De tabel kolommen en gegevenstypen zijn gebaseerd op de resultaten van de select-instructie. Om gegevens te importeren, kunt deze instructie in een externe tabel selecteren. |
| [CREATE EXTERNAL TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql) | Hiermee maakt u een nieuwe externe tabel door de resultaten van een select-instructie te exporteren naar een externe locatie.  De locatie is Azure Blob storage of Azure Data Lake Store. |

## <a name="aligning-source-data-with-the-data-warehouse"></a>Het uitlijnen van gegevens met het datawarehouse

Datawarehouse-tabellen zijn gevuld met het laden van gegevens uit een andere gegevensbron. Om uit te voeren een geslaagde load, moeten het nummer en gegevenstypen van de kolommen in de brongegevens uitgelijnd met de definitie van de tabel in het datawarehouse. Ophalen van de gegevens zodat deze kan de lastigste onderdeel van het ontwerpen van uw tabellen zijn. 

Als gegevens afkomstig is van meerdere gegevensarchieven, kunt u de gegevens overzetten naar het datawarehouse en opslaan in een tabel integratie. Zodra de gegevens zich bevinden in de tabel integratie, kunt u de kracht van SQL Data Warehouse transformatiebewerkingen uit te voeren. Zodra de gegevens is voorbereid, kunt u deze kunt invoegen in de Productietabellen.

## <a name="unsupported-table-features"></a>Niet-ondersteunde tabelfuncties
SQL Data Warehouse ondersteunt veel, maar niet alle van de tabelfuncties die worden aangeboden door andere databases.  De volgende lijst bevat enkele van de tabelfuncties die niet worden ondersteund in SQL Data Warehouse.

- Primaire sleutel, refererende sleutels, unieke en controleer de [tabelbeperkingen](/sql/t-sql/statements/alter-table-table-constraint-transact-sql)

- [Berekende kolommen](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql)
- [Geïndexeerde weergaven](/sql/relational-databases/views/create-indexed-views)
- [Takenreeks](/sql/t-sql/statements/create-sequence-transact-sql)
- [Sparse-kolommen](/sql/relational-databases/tables/use-sparse-columns)
- Vervangend sleutels. Implementeren met [identiteit](sql-data-warehouse-tables-identity.md).
- [Synoniemen](/sql/t-sql/statements/create-synonym-transact-sql)
- [Triggers](/sql/t-sql/statements/create-trigger-transact-sql)
- [Unieke indexen](/sql/t-sql/statements/create-index-transact-sql)
- [User-Defined Types](/sql/relational-databases/native-client/features/using-user-defined-types)

## <a name="table-size-queries"></a>Tabel grootte query 's
Een eenvoudige manier om ruimte en de rijen die worden gebruikt door een tabel in elk van de 60 distributies te identificeren, is met [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql).

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Echter met behulp van DBCC-opdrachten kan worden helemaal te beperken.  Dynamische beheerweergave (DMV's) weergeven meer details dan DBCC-opdrachten. Beginnen met het maken van deze weergave.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Tabel schijfruimte beschikbaar is

Deze query retourneert de rijen en ruimte door tabel.  Hiermee kunt u zien welke tabellen worden de grootste tabellen en of ze round-robin wordt gerepliceerd zijn, of hash - verdeeld.  De query bevat de distributiekolom voor hash-gedistribueerde tabellen.  

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tabelruimte door Distributietype

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabelruimte door indextype

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Distributie schijfruimte beschikbaar is

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Volgende stappen
Nadat de tabellen voor uw datawarehouse is gemaakt, wordt de volgende stap is om gegevens te laden in de tabel.  Zie voor een zelfstudie laden [het laden van gegevens naar SQL Data Warehouse](load-data-wideworldimportersdw.md).
