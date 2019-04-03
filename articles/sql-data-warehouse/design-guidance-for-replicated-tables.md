---
title: Ontwerprichtlijnen voor gerepliceerde tabellen - Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen voor het ontwerpen gerepliceerde tabellen in uw Azure SQL Data Warehouse-schema. 
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/19/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: acea42f7f4ab986e9828000ab7cfc9e302ed92a3
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885448"
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Ontwerprichtlijnen voor het gebruik van gerepliceerde tabellen in Azure SQL Data Warehouse
In dit artikel biedt aanbevelingen voor het ontwerpen van gerepliceerde tabellen in uw SQL Data Warehouse-schema. Gebruik deze aanbevelingen om queryprestaties te verbeteren door minder gegevens verplaatsen en query complex.

> [!VIDEO https://www.youtube.com/embed/1VS_F37GI9U]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bekend bent met de verdeling van de gegevens en gegevens verplaatsingen concepten in SQL Data Warehouse.  Zie voor meer informatie de [architectuur](massively-parallel-processing-mpp-architecture.md) artikel. 

Als onderdeel van het tabelontwerp, krijg inzicht in zo veel mogelijk over uw gegevens en hoe de gegevens in query wordt uitgevoerd.  Bijvoorbeeld, kunt u overwegen deze vragen:

- Hoe groot is de tabel?   
- Hoe vaak wordt de tabel vernieuwd?   
- Heb ik feiten- en dimensietabellen tabellen in een datawarehouse?   

## <a name="what-is-a-replicated-table"></a>Wat is een gerepliceerde tabel?
Een gerepliceerde tabel heeft een volledige kopie van de tabel die toegankelijk is op elk knooppunt. Een tabel te repliceren Hiermee verwijdert u de noodzaak om over te dragen van gegevens tussen rekenknooppunten voordat een join of aggregatie. Omdat de tabel meerdere exemplaren heeft, gerepliceerde tabellen werken het beste wanneer de grootte van de tabel minder dan 2 GB gecomprimeerd is.  2 GB is niet een vaste limiet.  Als de gegevens statisch is en niet wordt gewijzigd, kunt u grotere tabellen kunt repliceren.

Het volgende diagram toont een gerepliceerde tabel die toegankelijk is op elk knooppunt. In SQL Data Warehouse, wordt de gerepliceerde tabel volledig gekopieerd naar een distributiedatabase op elk knooppunt. 

![Gerepliceerde tabel](media/guidance-for-using-replicated-tables/replicated-table.png "gerepliceerde tabel")  

Gerepliceerde tabellen werken goed voor dimensietabellen in een stervormig schema. Dimensietabellen worden doorgaans gekoppeld aan de feitentabellen die anders dan de dimensietabel worden gedistribueerd.  Dimensies zijn meestal met een grootte die het mogelijk voor het opslaan en onderhouden van meerdere kopieën maakt. Dimensies beschrijvende worden gegevens opgeslagen die traag, zoals de naam van de klant en adres en productdetails wordt gewijzigd. De langzaam veranderende aard van de gegevens leidt tot minder onderhoud van de gerepliceerde tabel. 

Overweeg het gebruik van een gerepliceerde tabel wanneer:

- De grootte van de tabel op de schijf is minder dan 2 GB, ongeacht het aantal rijen. Als u de grootte van een tabel zoekt, kunt u de [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) opdracht: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- De tabel wordt gebruikt in joins die anders verplaatsing van gegevens vereist. Bij het samenvoegen van tabellen die niet worden gedistribueerd op dezelfde kolom, zoals een tabel met hash gedistribueerd naar een round robin-tabel worden verplaatsing van gegevens is vereist voor het voltooien van de query.  Als een van de tabellen klein is, kunt u overwegen een gerepliceerde tabel. U wordt aangeraden de gerepliceerde tabellen gebruiken in plaats van round robin-tabellen in de meeste gevallen. Voor bewerkingen voor gegevensverplaatsing in queryplannen bekijkt, gebruik [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).  De BroadcastMoveOperation is de verplaatsing van gegevens die kan worden verwijderd met behulp van een gerepliceerde tabel.  
 
Gerepliceerde tabellen kunnen niet de beste queryprestaties opleveren wanneer:

- De tabel is vaak invoegen, bijwerken en verwijderen van bewerkingen. Deze gegevens manipuleren taal (DML)-bewerkingen moeten samenstellen van de gerepliceerde tabel. Opnieuw opbouwen van vaak kan leiden tot tragere prestaties.
- Het datawarehouse wordt vaak geschaald. Een datawarehouse schalen, wijzigt het aantal rekenknooppunten, waarop worden in rekening gebracht opnieuw opbouwen van de gerepliceerde tabel.
- De tabel heeft een groot aantal kolommen, maar gegevensbewerkingen hebben doorgaans toegang tot slechts een beperkt aantal kolommen. In dit scenario, in plaats van het repliceren van de volledige tabel, is het mogelijk beter om te distribueren in de tabel en vervolgens een index maken voor veelgebruikte kolommen. Wanneer een query is vereist voor verplaatsing van gegevens, worden in SQL Data Warehouse alleen gegevens voor de aangevraagde kolommen verplaatst. 

## <a name="use-replicated-tables-with-simple-query-predicates"></a>Gerepliceerde tabellen gebruiken met eenvoudige query predicaten
Voordat u ervoor kiest om te distribueren of een tabel niet repliceren, moet u over de typen query's die u van plan bent om uit te voeren op de tabel. Indien mogelijk

- Gerepliceerde tabellen voor query's met een eenvoudige query predikaten, zoals gelijkheid of ongelijkheid gebruiken.
- Gedistribueerde tabellen gebruiken voor query's met predicaten van complexe query's, zoals LIKE of niet graag werken.

CPU-intensieve query's het beste presteren wanneer het werk wordt verdeeld over alle van de rekenknooppunten. Bijvoorbeeld, sneller query's die berekeningen kunt op elke rij van een tabel uitvoeren op gedistribueerde tabellen dan gerepliceerde tabellen. Omdat een gerepliceerde tabel zijn opgeslagen in het volledige op elk knooppunt, een CPU-intensieve query op basis van een gerepliceerde tabel wordt uitgevoerd op de hele tabel op elk rekenknooppunt. De extra berekening kan de prestaties van query's vertragen.

Deze query heeft bijvoorbeeld een complexe predicaat.  Het wordt sneller uitgevoerd wanneer de gegevens zich in een tabel in plaats van een gerepliceerde tabel. In dit voorbeeld wordt door de gegevens kunnen worden gedistribueerd middels round-robin.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a>Converteren van bestaande round robin-tabellen naar gerepliceerde tabellen
Als u al round robin-tabellen hebt, wordt u aangeraden deze te converteren naar gerepliceerde tabellen als ze voldoen aan de criteria die in dit artikel worden beschreven. Gerepliceerde tabellen de prestaties verbeteren van round robin-tabellen, omdat ze de noodzaak van verplaatsing van gegevens elimineert.  Een round robin-tabel is altijd vereist voor verplaatsing van gegevens voor joins. 

In dit voorbeeld wordt [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) wijzigen van de tabel DimSalesTerritory in een gerepliceerde tabel. In dit voorbeeld werkt ongeacht of DimSalesTerritory hash gedistribueerde of round robin.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Voorbeeld van de query-prestaties voor round robin versus gerepliceerd 

Een gerepliceerde tabel is een verplaatsing van gegevens niet vereist voor joins, omdat de hele tabel al aanwezig op elk knooppunt is. Als de dimensietabellen gedistribueerd middels round-robin, kopieert een join de dimensietabel volledig naar elk knooppunt. Het queryplan bevat voor het verplaatsen van de gegevens, een bewerking met de naam BroadcastMoveOperation. Dit type verplaatsing van gegevens wordt vertraagd prestaties van query's en verwijderd met behulp van gerepliceerde tabellen. Als u wilt weergeven querystappen plan, gebruiken de [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) catalogusweergave system. 

Bijvoorbeeld, in de volgende query uit op basis van de AdventureWorks-schema, de `FactInternetSales` tabel is hash worden verdeeld. De `DimDate` en `DimSalesTerritory` tabellen zijn kleiner dimensietabellen. Deze query retourneert de totale verkoop in Noord-Amerika voor boekjaar 2004:

```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
We opnieuw gemaakt `DimDate` en `DimSalesTerritory` als round robin-tabellen. Als gevolg hiervan het volgende queryplan, die meerdere uitzending bewerkingen verplaatsen is gebleken dat met de query: 
 
![Round robin queryplan](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

We opnieuw gemaakt `DimDate` en `DimSalesTerritory` als gerepliceerde tabellen en de query opnieuw uitgevoerd. Het resulterende queryplan veel korter is en niet hebben een uitzendt verplaatst.

![Queryplan gerepliceerd](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Prestatie-overwegingen voor het wijzigen van gerepliceerde tabellen
SQL Data Warehouse implementeert een gerepliceerde tabel door het onderhouden van een master-versie van de tabel. De master-versie worden gekopieerd naar een distributiedatabase op elk knooppunt. Wanneer er een wijziging is, wordt in SQL Data Warehouse eerst de hoofdtabel bijgewerkt. Vervolgens wordt de tabellen op elk knooppunt. Opnieuw opbouwen van een gerepliceerde tabel bevat de tabel kopiëren naar elk rekenknooppunt en het bouwen van de indexen.  Een gerepliceerde tabel op een DW400 heeft bijvoorbeeld 5 kopieën van de gegevens.  Een origineel exemplaar en een volledige kopie op elk knooppunt.  Alle gegevens worden opgeslagen in distributiedatabases. SQL Data Warehouse maakt gebruik van dit model voor de ondersteuning van sneller gegevens wijziging instructies en flexibele vergroten / verkleinen. 

Opnieuw worden opgebouwd zijn na vereist:
- Gegevens worden geladen of is gewijzigd
- Het datawarehouse wordt geschaald naar een ander niveau
- Definitie van de tabel wordt bijgewerkt

Opnieuw op te bouwen, zijn niet vereist is nadat:
- De bewerking onderbreken
- Bewerking hervatten

Het opnieuw bouwen niet is gebeurd onmiddellijk nadat de gegevens worden gewijzigd. Het opnieuw bouwen wordt in plaats daarvan de eerste keer dat een query uit de tabel geselecteerd geactiveerd.  Leest de query die het opnieuw bouwen geactiveerd direct van de hoofdversie van de tabel terwijl de gegevens asynchroon aan elk knooppunt worden gekopieerd. Totdat het kopiëren van gegevens voltooid is, blijft volgende query's gebruiken van de hoofdversie van de tabel.  Als een activiteit op basis van de gerepliceerde tabel die ervoor zorgt een andere opnieuw maken gebeurt dat, het kopiëren van gegevens is ongeldig gemaakt en de select-instructie van de volgende gegevens moeten worden gekopieerd opnieuw wordt geactiveerd. 

### <a name="use-indexes-conservatively"></a>Indexen dan gebruiken
Standaardprocedures indexering van toepassing op gerepliceerde tabellen. SQL Data Warehouse wordt elke index gerepliceerde tabel als onderdeel van het opnieuw bouwen. Gebruik alleen indexen wanneer de prestatieverbetering belangrijker is dan de kosten van de indexen opnieuw samenstellen.  
 
### <a name="batch-data-loads"></a>Laden van de batch-gegevens
Bij het laden van gegevens in gerepliceerde tabellen, probeer het opnieuw op te bouwen door belastingen samen batchverwerking minimaliseren. Alle batch belastingen uitvoeren voordat u select-instructies uitvoert.

Dit patroon load zijn bijvoorbeeld gegevens worden geladen uit vier bronnen en roept vier opnieuw op te bouwen. 

- Laden van bron 1.
- SELECT-instructie triggers opnieuw opbouwen van 1.
- Laden uit bron 2.
- SELECT-instructie triggers opnieuw opbouwen van 2.
- Laden van bron 3.
- SELECT-instructie triggers opnieuw opbouwen van 3.
- Laden van bron 4.
- SELECT-instructie triggers opnieuw 4.

Bijvoorbeeld: dit patroon load gegevens worden geladen uit vier bronnen, maar alleen roept een opnieuw maken.

- Laden van bron 1.
- Laden uit bron 2.
- Laden van bron 3.
- Laden van bron 4.
- SELECT-instructie triggers opnieuw opbouwen.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Na het laden van een batch opnieuw opbouwen van een gerepliceerde tabel
Om ervoor te zorgen consistent query uitvoertijd, houd rekening met de build van de gerepliceerde tabellen na het laden van een batch te forceren. De eerste query wordt anders nog steeds gebruiken voor gegevensverplaatsing om uit te voeren van de query. 

Deze query gebruikt de [sys.pdw_replicated_table_cache_state](/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV om de gerepliceerde tabellen die zijn gewijzigd, maar niet opnieuw opgebouwd weer te geven.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
Voer de volgende instructie voor elke tabel in de bovenstaande uitvoer voor het activeren van een opnieuw is gebouwd. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Volgende stappen 
Gebruik een van deze instructies voor het maken van een gerepliceerde tabel:

- [CREATE TABLE (Azure SQL Data Warehouse)](/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [CREATE TABLE AS SELECT (Azure SQL Data Warehouse)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Zie voor een overzicht van gedistribueerde tabellen [gedistribueerde tabellen](sql-data-warehouse-tables-distribute.md).



