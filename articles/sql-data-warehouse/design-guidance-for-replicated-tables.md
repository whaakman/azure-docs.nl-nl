---
title: Richtlijnen voor gerepliceerde tabellen - Azure SQL Data Warehouse ontwerpen | Microsoft Docs
description: Aanbevelingen voor het ontwerpen van gerepliceerde tabellen in uw Azure SQL Data Warehouse-schema.
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/23/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 575b3c5710d744e99c6e02439577a362eb17c67e
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Richtlijnen voor het gebruik van gerepliceerde tabellen in Azure SQL Data Warehouse ontwerpen
Dit artikel bevat aanbevelingen voor het ontwerpen van gerepliceerde tabellen in uw SQL Data Warehouse-schema. Gebruik deze aanbevelingen voor het verbeteren van prestaties van query's door gegevens verplaatsing en query complexiteit te verminderen.

> [!NOTE]
> De gerepliceerde tabel-functie is momenteel in de openbare preview. Sommige gedragingen nog worden gewijzigd.
> 

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bekend bent met gegevensdistributie en data movement concepten in SQL Data Warehouse.  Zie voor meer informatie de [architectuur](massively-parallel-processing-mpp-architecture.md) artikel. 

Als onderdeel van tabelontwerp begrijpen zo veel mogelijk over uw gegevens en hoe de gegevens wordt opgevraagd.  Neem bijvoorbeeld deze vragen:

- Hoe groot is de tabel?   
- Hoe vaak wordt de tabel vernieuwd   
- Heb ik feiten- en dimensietabellen tabellen in een datawarehouse?   

## <a name="what-is-a-replicated-table"></a>Wat is een gerepliceerde tabel?
Een gerepliceerde tabel heeft een volledige kopie van de tabel die toegankelijk is op elk rekenknooppunt. Bezig met het repliceren van een tabel verwijdert de behoefte om over te dragen van gegevens tussen rekenknooppunten voordat een join of aggregatie. Aangezien de tabel meerdere exemplaren heeft, gerepliceerde tabellen werken het beste als de grootte van de tabel minder dan 2 GB gecomprimeerd is.

Het volgende diagram toont een gerepliceerde tabel die toegankelijk is op elk rekenknooppunt. In SQL Data Warehouse, wordt de gerepliceerde tabel volledig gekopieerd naar een distributiedatabase op elk rekenknooppunt. 

![Gerepliceerde tabel](media/guidance-for-using-replicated-tables/replicated-table.png "gerepliceerde tabel")  

Gerepliceerde tabellen werk geschikt voor kleine dimensietabellen in een sterschema. Dimensietabellen zijn meestal met een grootte die het mogelijk voor het opslaan en onderhouden van meerdere exemplaren maakt. Dimensies opslaan beschrijvende gegevens die traag, zoals de naam van de klant en adres en productgegevens wijzigingen. De langzaam veranderende aard van de gegevens leidt tot de gerepliceerde tabel minder opnieuw worden opgebouwd. 

Overweeg het gebruik van een gerepliceerde tabel wanneer:

- De grootte van de tabel op schijf is minder dan 2 GB vereist, ongeacht het aantal rijen. Als u wilt de grootte van een tabel vinden, kunt u de [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) opdracht: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- De tabel wordt gebruikt in samenvoegingen die anders worden verplaatsing van gegevens moeten. Bijvoorbeeld vereist een join op tabellen hash gedistribueerd gegevensverplaatsing wanneer de kolommen die niet gelijk zijn aan dezelfde kolom van het distributiepunt. Als een van de tabellen hash gedistribueerd klein is, kunt u een gerepliceerde tabel. Een join op een round robin-tabel vereist de verplaatsing van gegevens. U kunt het beste gerepliceerde tabellen in plaats van round robin-tabellen in de meeste gevallen gebruikt. 


Houd rekening met het converteren van een bestaande gedistribueerde tabel naar een gerepliceerde tabel wanneer:

- Query-gebruik data movement-bewerkingen die de gegevens in de rekenknooppunten uitgezonden plannen. De BroadcastMoveOperation kostbaar en vertraagt queryprestaties. Als u wilt gegevens movement-bewerkingen in queryplannen bekijkt, gebruik [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).
 
Gerepliceerde tabellen kunnen niet resulteert in de beste queryprestaties wanneer:

- De tabel heeft frequente invoegen, bijwerken en verwijderen van bewerkingen. Taal (DML) bestandsbewerkingen van deze gegevens vereist opnieuw opbouwen van de gerepliceerde tabel. Opnieuw opbouwen vaak kan leiden tot lagere prestaties.
- Het datawarehouse wordt vaak geschaald. Het aantal rekenknooppunten, leidt ertoe dat opnieuw opbouwen schalen van een datawarehouse worden gewijzigd.
- De tabel heeft een groot aantal kolommen, maar gegevensbewerkingen gewoonlijk toegang tot een klein aantal kolommen. In dit scenario wordt in plaats van de gehele tabel repliceren mogelijk effectiever op hash distribueren van de tabel en maak vervolgens een index in de veelgebruikte kolommen. Als u een query moet de verplaatsing van gegevens, verplaatst SQL Data Warehouse alleen gegevens in de gevraagde kolommen. 



## <a name="use-replicated-tables-with-simple-query-predicates"></a>Gerepliceerde tabellen gebruiken met eenvoudige query predicaten
Voordat u wilt distribueren of een tabel niet repliceren, moet u over de typen query's die u van plan bent om uit te voeren op de tabel. Indien mogelijk

- Gerepliceerde tabellen voor query's met eenvoudige query predicaten, zoals gelijkheid of ongelijk gebruiken.
- Gebruik gedistribueerde tabellen voor query's met predicaten voor complexe query's, zoals ACHTIGE of geen wilt.

CPU-intensief query's werken het beste als het werk verdeeld over alle van de rekenknooppunten. Bijvoorbeeld, sneller query's die berekeningen uitgevoerd op elke rij van een tabel op gedistribueerde tabellen dan gerepliceerde tabellen. Omdat een gerepliceerde tabel volledig op elk rekenknooppunt is opgeslagen, wordt een CPU-intensief query op een gerepliceerde tabel uitgevoerd op de gehele tabel op elk rekenknooppunt. De extra berekening kan queryprestaties kan afnemen.

Deze query heeft bijvoorbeeld een complexe predikaat.  Sneller wanneer leverancier een gedistribueerde tabel in plaats van een gerepliceerde tabel. In dit voorbeeld kan leverancier hash gedistribueerd of round robin gedistribueerd.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a>Bestaande round robin tabellen omzetten in gerepliceerde tabellen
Als u al round robin-tabellen, wordt u aangeraden deze te converteren naar gerepliceerde tabellen als ze voldoen aan met de criteria die in dit artikel wordt beschreven. Gerepliceerde tabellen de prestaties verbeteren van round robin-tabellen, omdat ze niet meer hoeft te verplaatsen van gegevens.  Een tabel round robin vereist altijd gegevensverplaatsing joins. 

In dit voorbeeld wordt [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) te wijzigen van de tabel DimSalesTerritory in een gerepliceerde tabel. In dit voorbeeld werkt ongeacht of DimSalesTerritory hash gedistribueerd of round robin.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Voorbeeld van de query-prestaties round-robin versus gerepliceerd 

Een gerepliceerde tabel vereist geen een verplaatsing van gegevens voor samenvoegingen omdat de gehele tabel al aanwezig op elk rekenknooppunt is. Als de dimensietabellen round robin gedistribueerd, kopieert een join de dimensietabel volledig naar elk rekenknooppunt. Als u wilt de gegevens verplaatst, bevat het queryplan een BroadcastMoveOperation aangeroepen bewerking. Dit type verplaatsing van gegevens wordt vertraagd prestaties van query's en met behulp van gerepliceerde tabellen wordt geëlimineerd. Als u wilt weergeven van query plan stappen, gebruiken de [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) catalogusweergave systeem. 

Bijvoorbeeld, in de volgende query op de AdventureWorks-schema, de ` FactInternetSales` tabel is hash worden verdeeld. De `DimDate` en `DimSalesTerritory` tabellen zijn kleinere dimensietabellen. Deze query retourneert de totale verkoop in Noord-Amerika voor het fiscale jaar 2004:
 
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
We opnieuw gemaakt `DimDate` en `DimSalesTerritory` round robin-tabellen. De query bleek daardoor het volgende queryplan waarvoor meerdere uitzending bewerkingen verplaatsen: 
 
![Round robin queryplan](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

We opnieuw gemaakt `DimDate` en `DimSalesTerritory` als in de gerepliceerde tabellen en de query opnieuw is gestart. Het resulterende queryplan veel korter is en niet hebben een uitzendt verplaatst.

![Queryplan gerepliceerd](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Prestatie-overwegingen voor het wijzigen van gerepliceerde tabellen
SQL Data Warehouse implementeert een gerepliceerde tabel door het onderhouden van een hoofdversie van de tabel. De hoofdversie gekopieerd naar een distributiedatabase op elk rekenknooppunt. Wanneer er een wijziging is, wordt in SQL Data Warehouse eerst de hoofdtabel bijgewerkt. Klik hiervoor opnieuw opbouwen van de tabellen op elk rekenknooppunt. Opnieuw opbouwen van een gerepliceerde tabel bevat de tabel kopiëren naar elk rekenknooppunt en de indexen opnieuw samenstellen.

Opnieuw worden opgebouwd zijn vereist is nadat:
- Gegevens worden geladen of is gewijzigd
- Het datawarehouse wordt geschaald naar een andere [service niveau](performance-tiers.md#service-levels)
- Definitie van de tabel wordt bijgewerkt

Opnieuw worden opgebouwd zijn niet vereist is nadat:
- Onderbreken is
- Bewerking hervatten

Het opnieuw bouwen gebeurt niet onmiddellijk nadat de gegevens worden gewijzigd. Het opnieuw bouwen wordt in plaats daarvan de eerste keer dat een query wordt geselecteerd in de tabel geactiveerd.  Zijn de stappen voor het bouwen van de gerepliceerde tabel binnen de eerste select-instructie uit de tabel.  Omdat het opnieuw bouwen binnen de query wordt uitgevoerd, kan impact op de eerste instructie select verwaarloosbaar zijn afhankelijk van de grootte van de tabel zijn.  Als er meerdere gerepliceerde tabellen zijn betrokken die opnieuw opbouwen moeten, wordt elke kopie als de stappen in de instructie opnieuw opeenvolgend opgebouwd.  Als u wilt behouden van gegevens consistentie tijdens het opnieuw opbouwen van de gerepliceerde tabel een exclusieve vergrendeling meegenomen bij de tabel.  De vergrendeling voorkomt alle toegang tot de tabel voor de duur van het opnieuw bouwen. 

### <a name="use-indexes-conservatively"></a>Indexen conservatieve gebruiken
Standaardprocedures indexering van toepassing op gerepliceerde tabellen. SQL Data Warehouse wordt elke gerepliceerde tabelindex als onderdeel van het opnieuw bouwen. Gebruik alleen indexen wanneer de prestatieverbetering belangrijker is dan de kosten van de indexen opnieuw opbouwen.  
 
### <a name="batch-data-loads"></a>Batch gegevens geladen
Wanneer gegevens in gerepliceerde tabellen te laden, probeer het opnieuw worden opgebouwd minimaliseren door batchverwerking belasting samen. De batch belasting uitvoeren voordat u de select-instructies.

Bijvoorbeeld, dit patroon load gegevens worden geladen uit vier bronnen en roept vier opnieuw worden opgebouwd. 

- Laden van bron 1.
- SELECT-instructie triggers opnieuw opbouwen van 1.
- Laden van bron 2.
- SELECT-instructie triggers opnieuw worden opgebouwd 2.
- Laden van bron 3.
- SELECT-instructie triggers opnieuw opbouwen van 3.
- Laden van bron 4.
- SELECT-instructie triggers 4 opnieuw worden opgebouwd.

Bijvoorbeeld, dit patroon load gegevens worden geladen uit vier bronnen, maar roept slechts één opnieuw opbouwen.

- Laden van bron 1.
- Laden van bron 2.
- Laden van bron 3.
- Laden van bron 4.
- SELECT-instructie triggers opnieuw worden opgebouwd.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Opnieuw samenstellen van een gerepliceerde tabel na een batch-belasting
Om ervoor te zorgen uitvoeringstijden consistente query, wordt u aangeraden forceren van een vernieuwing van de gerepliceerde tabellen na een batch-belasting. Anders wordt moet de eerste query wachten tot de tabellen wilt vernieuwen, waaronder de indexen opnieuw opbouwen. Afhankelijk van de grootte en een aantal gerepliceerde tabellen die van invloed op zijn de prestatie-invloed verwaarloosbaar.  

Deze query gebruikt de [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV voor een lijst met de gerepliceerde tabellen die zijn gewijzigd, maar niet opnieuw opgebouwd.

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
 
Als u wilt afdwingen opnieuw opbouwen, voer de volgende instructie voor elke tabel in de bovenstaande uitvoer. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Volgende stappen 
Gebruik een van deze instructies voor het maken van een gerepliceerde tabel:

- [TABEL (Azure SQL datawarehouse) maken](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [TABLE AS SELECT (Azure SQL datawarehouse) maken](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Zie voor een overzicht van gedistribueerde tabellen [tabellen gedistribueerd](sql-data-warehouse-tables-distribute.md).



