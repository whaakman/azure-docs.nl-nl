---
title: Inleiding in de tabel-ontwerp - Azure SQL Data Warehouse | Microsoft Docs
description: Inleiding tot het ontwerpen van tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 01/18/2018
ms.author: barbkess
ms.openlocfilehash: 5c163880a7508d69bce0019cc5379bca8c704d59
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="introduction-to-designing-tables-in-azure-sql-data-warehouse"></a>Inleiding tot het ontwerpen van tabellen in Azure SQL Data Warehouse

Meer informatie over belangrijke concepten voor het ontwerpen van tabellen in Azure SQL Data Warehouse. 

## <a name="determining-table-category"></a>Tabelcategorie bepalen 

Een [sterschema](https://en.wikipedia.org/wiki/Star_schema) gegevens zijn gerangschikt in feiten- en dimensietabellen tabellen. Sommige tabellen worden gebruikt voor integratie of staging-gegevens voordat deze wordt verplaatst naar een tabel met het feit of de dimensie. Bij het ontwerpen van een tabel, moet u bepalen of gegevens in de tabel in een feitelijke dimensie of een integratie tabel hoort. Deze beslissing informeert het juiste tabelstructuur en distribueren. 

- **Feitentabellen** kwantitatieve gegevens bevatten die zijn vaak in een transactionele systeem gegenereerd en vervolgens in het datawarehouse geladen. Bijvoorbeeld, een detailhandel verkoop genereert elke dag en vervolgens de gegevens laadt in een datawarehouse-feitentabel voor analyse.

- **Dimensie tabellen** kenmerkgegevens bevatten die kan worden gewijzigd, maar gewoonlijk niet vaak veranderen. De naam en het adres van de klant worden bijvoorbeeld opgeslagen in een dimensietabel en alleen wanneer de klant verandert het profiel bijgewerkt. Als u wilt de grootte van een grote feitentabel minimaliseren, hoeft de naam en het adres van de klant niet te worden in elke rij van een feitentabel. In plaats daarvan de feitentabel en de dimensietabel kunnen een klant-id delen. Een query kunt deelnemen aan de twee tabellen om te koppelen van een klant-profiel en transacties. 

- **Integratie tabellen** bieden een plaats voor het integreren of staging-gegevens. U kunt een integratie tabel als een gewone tabel, een externe tabel of een tijdelijke tabel maken. Bijvoorbeeld, kunt u gegevens naar een tijdelijke tabel laden, transformaties uitvoeren van de gegevens in fasering en vervolgens de gegevens in een productietabel invoegen.

## <a name="schema-and-table-names"></a>Schema- en tabelnamen
In SQL Data Warehouse is een datawarehouse een type van de database. Alle tabellen in het datawarehouse bevinden zich in dezelfde database.  U kan niet deelnemen aan tabellen in meerdere datawarehouses. Dit gedrag verschilt van de SQL Server, die ondersteuning biedt voor meerdere databases joins. 

In een SQL Server-database, kan u feitelijke dimensie, gebruiken of integreren voor de schemanamen. Als u een SQL Server-database met SQL Data Warehouse migreert, werkt het het beste alle feitelijke dimensie en integratie-tabellen te migreren naar één schema in SQL Data Warehouse. U kan bijvoorbeeld opslaan van alle tabellen in de [WideWorldImportersDW](/sql/sample/world-wide-importers/database-catalog-wwi-olap) voorbeeld datawarehouse binnen één schema wwi aangeroepen. De volgende code maakt een [gebruiker gedefinieerde schema](/sql/t-sql/statements/create-schema-transact-sql) wwi aangeroepen.

```sql
CREATE SCHEMA wwi;
```

Als de organisatie van de tabellen in SQL Data Warehouse weergeven, kunt u feitelijke dimensie en int gebruiken als voorvoegsels zodat de tabelnamen. De volgende tabel bevat enkele van de schema- en tabelnamen voor WideWorldImportersDW. De namen in SQL Server met de namen in SQL Data Warehouse worden vergeleken. 

| WideWorldImportersDW tabel  | Tabeltype | SQL Server | SQL Data Warehouse |
|:-----|:-----|:------|
| Plaats | Dimensie | Dimension.City | wwi.DimCity |
| Volgorde | Fact | Fact.Order | wwi.FactOrder |


## <a name="table-persistence"></a>Persistentie van de tabel 

Tabellen gegevensopslag permanent in Azure Storage, tijdelijk in Azure Storage, of in een datawarehouse externe gegevensarchief.

### <a name="regular-table"></a>Gewone tabellen

Een gewone tabellen worden de gegevens in Azure Storage als onderdeel van het datawarehouse opgeslagen. De tabel en de gegevens behouden blijven ongeacht of een sessie geopend is.  In dit voorbeeld maakt een gewone tabel met twee kolommen. 

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Tijdelijke tabel
Een tijdelijke tabel is alleen beschikbaar voor de duur van de sessie. U kunt een tijdelijke tabel gebruiken om te voorkomen dat andere maakt gebruik van tijdelijke resultaten en de noodzaak voor opschoning verminderen.  Omdat de tijdelijke tabellen ook gebruikmaken van lokale opslag, bieden ze sneller voor bepaalde bewerkingen.  Zie voor meer informatie [tijdelijke tabellen](sql-data-warehouse-tables-temporary.md).

### <a name="external-table"></a>Externe tabel
Een externe tabel verwijst naar gegevens die zich bevinden in Azure Storage-blob of Azure Data Lake Store. Wanneer gebruikt in combinatie met de instructie CREATE TABLE AS SELECT, importeert selecteren in een externe tabel gegevens in SQL Data Warehouse. Externe tabellen zijn daarom nuttig voor het laden van gegevens. Zie voor een zelfstudie laden [gebruik PolyBase om gegevens te laden uit Azure blob storage](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="data-types"></a>Gegevenstypen
SQL Data Warehouse ondersteunt gebruikte de meest gegevenstypen. Zie voor een lijst van de ondersteunde gegevenstypen [gegevenstypen in CREATE TABLE verwijzing](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) in de instructie CREATE TABLE. Voor het minimaliseren van de grootte van gegevenstypen kunt u queryprestaties verbeteren. Zie voor instructies over het gebruik van gegevenstypen [gegevenstypen](sql-data-warehouse-tables-data-types.md).

## <a name="distributed-tables"></a>Gedistribueerde tabellen
Een fundamenteel onderdeel van SQL Data Warehouse is de manier kunnen worden opgeslagen en tabellen niet bewerken via 60 [distributies](massively-parallel-processing-mpp-architecture.md#distributions).  De tabellen worden gedistribueerd met behulp van een methode round robin, hash of replicatie.

### <a name="hash-distributed-tables"></a>Gedistribueerd hash tabellen
De hash-distributie distribueert rijen op basis van de waarde in de kolom van het distributiepunt. De tabel hash gedistribueerd is ontworpen om hoge prestaties voor query-verbindingen in grote tabellen bereiken. Er zijn verschillende factoren die invloed hebben op de keuze van de distributie-kolom. 

Zie voor meer informatie [richtlijnen voor gedistribueerde tabellen ontwerpen](sql-data-warehouse-tables-distribute.md).

### <a name="replicated-tables"></a>Gerepliceerde tabellen
Een gerepliceerde tabel heeft een volledige kopie van de tabel die beschikbaar is op elk rekenknooppunt. Query's uitvoeren snel op gerepliceerde tabellen omdat joins in gerepliceerde tabellen geen verplaatsing van gegevens vereisen. Replicatie extra opslag vereist, en is het niet praktisch voor grote tabellen. 

Zie voor meer informatie [richtlijnen voor gerepliceerde tabellen ontwerpen](design-guidance-for-replicated-tables.md).

### <a name="round-robin-tables"></a>Round robin tabellen
Een tabel round robin worden tabelrijen gelijkmatig over alle distributies verdeeld. De rijen worden willekeurig gedistribueerd. Laden van gegevens in een tabel round robin is snel.  Query's vereisen echter meer gegevensverplaatsing dan de andere distributiemethoden. 

Zie voor meer informatie [richtlijnen voor gedistribueerde tabellen ontwerpen](sql-data-warehouse-tables-distribute.md).


### <a name="common-distribution-methods-for-tables"></a>Algemene methoden van verkeersdistributie voor tabellen
De tabelcategorie wordt vaak bepaalt welke optie te kiezen voor het distribueren van de tabel. 

| Tabelcategorie | Optie aanbevolen distributie |
|:---------------|:--------------------|
| Fact           | Hash-distributiepunt gebruiken met geclusterde columnstore-index. Prestaties verbeteren door twee hashtabellen zijn gekoppeld op dezelfde kolom van het distributiepunt. |
| Dimensie      | Gebruik voor kleinere tabellen zijn gerepliceerd. Als tabellen te groot om op te slaan op elk rekenknooppunt zijn, gebruikt u de hash is gedistribueerd. |
| Faseren        | Round robin gebruiken voor de faseringstabel. De werklast met CTAS is snel. Nadat de gegevens in de faseringstabel is, gebruikt u INSERT... Selecteer de gegevens worden verplaatst naar een productie-tabellen. |

## <a name="table-partitions"></a>Tabelpartities
Een gepartitioneerde tabel slaat en bewerkingen worden uitgevoerd op de tabelrijen volgens gegevensbereiken. Een tabel kan bijvoorbeeld worden gepartitioneerd per dag, maand of jaar. U kunt de prestaties van query's via partitie elimineren, waardoor een query worden gescand om gegevens binnen een partitie te verbeteren. U kunt ook de gegevens via het overschakelen van de partitie onderhouden. Omdat de gegevens in SQL Data Warehouse al is gedistribueerd, kunnen de partities te veel queryprestaties kunnen afnemen. Zie voor meer informatie [richtlijnen partitioneren](sql-data-warehouse-tables-partition.md).

## <a name="columnstore-indexes"></a>Columnstore-indexen
Standaard wordt een tabel in SQL Data Warehouse opgeslagen als een geclusterde columnstore-index. Deze vorm van gegevensopslag realiseert hoge gegevenscompressie en prestaties van query's op grote tabellen.  De geclusterde columnstore-index is meestal de beste keuze, maar in sommige gevallen een geclusterde index of een heap is de juiste opslagstructuur.

Zie voor een lijst met functies columnstore [wat is er nieuw voor columnstore-indexen](/sql/relational-databases/indexes/columnstore-indexes-what-s-new). Columnstore-index om prestaties te verbeteren, Zie [rijgroep kwaliteit voor columnstore-indexen optimaliseren](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

## <a name="statistics"></a>statistieken
De queryoptimizer maakt gebruik van statistieken op kolomniveau bij het maken van het plan voor een query uit te voeren. Ter verbetering van de prestaties van query's, is het belangrijk dat u statistieken maakt voor afzonderlijke kolommen, met name de kolommen in de query wordt gebruikt. Maken en bijwerken van statistieken gebeurt niet automatisch. [Maken van statistieken](/sql/t-sql/statements/create-statistics-transact-sql) na het maken van een tabel. Statistieken bijwerken nadat u een groot aantal rijen zijn toegevoegd of gewijzigd. Bijvoorbeeld: statistieken bijwerken na een belasting. Zie voor meer informatie [statistieken richtlijnen](sql-data-warehouse-tables-statistics.md).

## <a name="commands-for-creating-tables"></a>Opdrachten voor het maken van tabellen
Als een nieuwe lege tabel kunt u een tabel. U kunt ook maken en vullen van een tabel met de resultaten van een select-instructie. Hieronder vindt u de T-SQL-opdrachten voor het maken van een tabel.

| T-SQL-instructie | Beschrijving |
|:----------------|:------------|
| [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) | Maakt een lege tabel met het definiëren van de tabelkolommen en opties. |
| [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql) | Maakt een externe tabel. De definitie van de tabel wordt opgeslagen in SQL Data Warehouse. Gegevens in de tabel wordt opgeslagen in Azure Blob storage of Azure Data Lake Store. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) | Hiermee wordt een nieuwe tabel met de resultaten van een instructie select gevuld. De tabel kolommen en gegevenstypen zijn gebaseerd op de resultaten van de select-instructie. Om gegevens te importeren, kunt deze instructie van een externe tabel selecteren. |
| [EXTERNAL TABLE AS SELECT MAKEN](/sql/t-sql/statements/create-external-table-as-select-transact-sql) | Maakt een nieuwe externe tabel door de resultaten van een select-instructie te exporteren naar een externe locatie.  De locatie is Azure Blob storage of Azure Data Lake Store. |

## <a name="aligning-source-data-with-the-data-warehouse"></a>Het uitlijnen van brongegevens bij het datawarehouse

Datawarehouse-tabellen zijn gevuld met het laden van gegevens uit een andere gegevensbron. Als u wilt uitvoeren van een geslaagde belasting, het aantal en de gegevenstypen van de kolommen in de brongegevens uitgelijnd met de definitie van de tabel in het datawarehouse. Ophalen van de gegevens op één lijn mogelijk de lastigste onderdeel van het ontwerpen van uw tabellen. 

Als gegevens afkomstig is van meerdere gegevensarchieven, kunt u laat de gegevens in het datawarehouse en opslaan in een tabel van de integratie. Nadat de gegevens zich in de tabel integratie, kunt u de kracht van SQL Data Warehouse transformatiebewerkingen uit te voeren. Als de gegevens is voorbereid, kunt u deze kunt invoegen in productie tabellen.

## <a name="unsupported-table-features"></a>Niet-ondersteunde tabelfuncties
SQL Data Warehouse biedt ondersteuning voor veel, maar niet alle tabelfuncties die worden aangeboden door andere databases.  De volgende lijst bevat enkele van de tabelfuncties die worden niet ondersteund in SQL Data Warehouse.

- Primaire sleutel, refererende sleutels, unieke en controleer de [tabelbeperkingen](/sql/t-sql/statements/alter-table-table-constraint-transact-sql)

- [Berekende kolommen](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql)
- [Geïndexeerde weergaven](/sql/relational-databases/views/create-indexed-views)
- [Reeks](/sql/t-sql/statements/create-sequence-transact-sql)
- [Sparse kolommen](/sql/relational-databases/tables/use-sparse-columns)
- [Vervangend sleutels](). Implementeren met [identiteit](sql-data-warehouse-tables-identity.md).
- [Synonyms](/sql/t-sql/statements/create-synonym-transact-sql)
- [Triggers](/sql/t-sql/statements/create-trigger-transact-sql)
- [Unieke indexen](/sql/t-sql/statements/create-index-transact-sql)
- [Gebruiker gedefinieerde typen](/sql/relational-databases/native-client/features/using-user-defined-types)

## <a name="table-size-queries"></a>Tabel grootte query 's
Er is een eenvoudige manier om ruimte en de rijen die worden gebruikt door een tabel in elk van de 60 distributies te identificeren met DBCC PDW_SHOWSPACEUSED [DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Echter kan met behulp van DBCC-opdrachten worden behoorlijk beperken.  Dynamische beheerweergaven (DMV's) weergeven meer details dan DBCC-opdrachten. Begint met het maken van deze weergave.

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

### <a name="table-space-summary"></a>Tabelruimte samenvatting

Deze query retourneert de rijen en ruimte door tabel.  Hiermee kunt u zien welke tabellen worden de grootste tabellen en of ze round-robin wordt gerepliceerd zijn, of hash - verdeeld.  De query geeft voor gedistribueerde hash tabellen, de distributie-kolom.  

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

### <a name="distribution-space-summary"></a>Distributie ruimte samenvatting

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
Na het maken van de tabellen voor het datawarehouse van is de volgende stap om gegevens te laden in de tabel.  Zie voor een zelfstudie laden [laden van gegevens uit Azure blob storage met PolyBase](load-data-from-azure-blob-storage-using-polybase.md).