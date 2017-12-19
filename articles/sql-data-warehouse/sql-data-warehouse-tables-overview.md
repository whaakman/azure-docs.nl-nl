---
title: Overzicht van tabellen in SQL Data Warehouse | Microsoft Docs
description: Aan de slag met Azure SQL Data Warehouse-tabellen.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 2114d9ad-c113-43da-859f-419d72604bdf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/14/2017
ms.author: barbkess
ms.openlocfilehash: 46f7d2ea19a88e65b2d039fdf36d1619c4d74020
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="overview-of-tables-in-sql-data-warehouse"></a>Overzicht van tabellen in SQL Data Warehouse
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

Aan de slag met het maken van tabellen in SQL Data Warehouse is eenvoudig.  Het basic [CREATE TABLE] [ CREATE TABLE] syntaxis volgt de algemene syntaxis hoogstwaarschijnlijk al bekend bent met werken met andere databases.  Een tabel te maken, moet u gewoon naam van de tabel, naam van de kolommen en gegevenstypen voor elke kolom definiëren.  Als u hebt de tabellen in andere databases maakt, moet dit zeer bekend eruitzien.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Het bovenstaande voorbeeld maakt een tabel met de naam klanten met twee kolommen, voornaam en achternaam.  Elke kolom is gedefinieerd met het gegevenstype van VARCHAR(25) Hiermee beperkt u de gegevens naar 25 tekens.  Deze fundamentele kenmerken van een tabel, evenals andere, zijn grotendeels hetzelfde als andere databases.  Gegevenstypen zijn gedefinieerd voor elke kolom en de integriteit garanderen van uw gegevens.  Indexen kunnen worden toegevoegd aan de prestaties verbeteren door i/o's.  Partities kan worden toegevoegd aan de prestaties verbeteren als u nodig hebt om gegevens te wijzigen.

[Naam van] [ RENAME] een tabel met SQL Data Warehouse uitziet:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Gedistribueerde tabellen
Een nieuw fundamentele kenmerk geïntroduceerd door gedistribueerde systemen SQL Data Warehouse is de **distributie kolom**.  De distributie-kolom is te veel wat het klinkt als.  Dit is de kolom die hoe verspreiden of verdelen bepaalt, uw gegevens achter de schermen.  Wanneer u een tabel maken zonder op te geven van de kolom van het distributiepunt, de tabel wordt automatisch gedistribueerd met behulp van **round-robin**.  Round-robin tabellen kunnen zijn voldoende in sommige scenario's, kan definiëren distributiekolommen aanzienlijk minder verplaatsing van gegevens tijdens de query's, waardoor de prestaties optimaliseren.  In situaties wanneer er een kleine hoeveelheid gegevens in een tabel, kiezen voor het maken van de tabel met de **repliceren** Distributietype gegevens worden gekopieerd naar elk rekenknooppunt en slaat de verplaatsing van gegevens tijdens het uitvoeren van een query. Zie [distribueren van een tabel] [ Distribute] voor meer informatie over het selecteren van een distributie-kolom.

## <a name="indexing-and-partitioning-tables"></a>Indexeren en tabellen te partitioneren
Als u bij het gebruik van SQL Data Warehouse worden geavanceerde en om prestaties te optimaliseren, moet u voor meer informatie over het ontwerp van de tabel.  Zie voor meer informatie de artikelen op [tabel gegevenstypen][Data Types], [distribueren van een tabel][Distribute], [indexeren van een tabel] [ Index] en [partitioneren van een tabel][Partition].

## <a name="table-statistics"></a>Tabelstatistieken
Statistieken zijn een zeer belangrijk voor het ophalen van de beste prestaties buiten uw SQL Data Warehouse.  Omdat SQL Data Warehouse nog niet automatisch maken en bijwerken van statistieken voor u, zoals u mogelijk zijn zou verwachten in Azure SQL Database, het artikel lezen op [statistieken] [ Statistics] mogelijk een van de belangrijkste artikelen lezen om ervoor te zorgen dat u de beste prestaties van uw query.

## <a name="temporary-tables"></a>Tijdelijke tabellen
Tijdelijke tabellen zijn tabellen die alleen bestaan voor de duur van uw aanmelding en kunnen niet worden bekeken door andere gebruikers.  Tijdelijke tabellen mag een uitstekende manier om te voorkomen dat anderen tijdelijke resultaten en ook de noodzaak voor opschoning te verminderen.  Omdat de tijdelijke tabellen ook gebruikmaken van lokale opslag, bieden ze sneller voor bepaalde bewerkingen.  Zie de [tijdelijke tabel] [ Temporary] artikelen voor meer informatie over tijdelijke tabellen.

## <a name="external-tables"></a>Externe tabellen
Externe tabellen, ook wel bekend als Polybase tabellen zijn tabellen die kunnen worden opgevraagd van SQL Data Warehouse, maar punt naar externe gegevens uit SQL Data Warehouse.  Bijvoorbeeld, kunt u een externe tabel die verwijst naar de bestanden op Azure Blob Storage of Azure Data Lake Store.  Zie voor meer informatie over het maken en query uitvoeren op een externe tabel [gegevens laden met Polybase][Load data with Polybase].  

## <a name="unsupported-table-features"></a>Niet-ondersteunde tabelfuncties
SQL Data Warehouse bevat veel van dezelfde tabelfuncties die worden aangeboden door andere databases, maar er zijn bepaalde functies die nog niet ondersteund.  Hieronder ziet u een lijst met enkele van de tabelfuncties die nog niet ondersteund.

| Niet-ondersteunde functies |
| --- |
| Primaire sleutel, refererende sleutels, unieke- en controle [tabelbeperkingen][Table Constraints] |
| [Unieke indexen][Unique Indexes] |
| [Berekende kolommen][Computed Columns] |
| [Sparse kolommen][Sparse Columns] |
| [Gebruiker gedefinieerde typen][User-Defined Types] |
| [Reeks][Sequence] |
| [Triggers][Triggers] |
| [Geïndexeerde weergaven][Indexed Views] |
| [Synoniemen][Synonyms] |

## <a name="table-size-queries"></a>Tabel grootte query 's
Een eenvoudige manier om ruimte en de rijen die worden gebruikt door een tabel in elk van de 60 distributies te identificeren, is met [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Echter kan met behulp van DBCC-opdrachten worden behoorlijk beperken.  Dynamische beheerweergaven (DMV's) kunt u veel meer details, evenals bieden u veel meer controle over de queryresultaten.  Begint met het maken van deze weergave wordt naar worden verwezen door veel van onze voorbeelden in deze en andere artikelen.

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
Deze query retourneert de rijen en ruimte door tabel.  Het is een uitstekende query om te zien welke tabellen worden de grootste tabellen en of round-robin gerepliceerd of hash gedistribueerd.  Voor gedistribueerde hashtabellen ook ziet u de distributie-kolom.  In de meeste gevallen moet de grootste tabellen hash gedistribueerd met een geclusterde columnstore-index.

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
Zie voor meer informatie de artikelen op [tabel gegevenstypen][Data Types], [distribueren van een tabel][Distribute], [indexeren van een tabel][Index], [partitioneren van een tabel][Partition], [tabelstatistieken onderhouden] [ Statistics] en [tijdelijke tabellen][Temporary].  Zie voor meer informatie over best practices [aanbevolen procedures van SQL Data Warehouse][SQL Data Warehouse Best Practices].

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
[Load data with Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Table Constraints]: https://msdn.microsoft.com/library/ms188066.aspx
[Computed Columns]: https://msdn.microsoft.com/library/ms186241.aspx
[Sparse Columns]: https://msdn.microsoft.com/library/cc280604.aspx
[User-Defined Types]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequence]: https://msdn.microsoft.com/library/ff878091.aspx
[Triggers]: https://msdn.microsoft.com/library/ms189799.aspx
[Indexed Views]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyms]: https://msdn.microsoft.com/library/ms177544.aspx
[Unique Indexes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
