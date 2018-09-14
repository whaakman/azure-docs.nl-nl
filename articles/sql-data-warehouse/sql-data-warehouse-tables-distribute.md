---
title: Gedistribueerde tabellen ontwerprichtlijnen - Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen voor het ontwerpen van gedistribueerde hash- en round robin gedistribueerde tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 36db91cd7c4dad3c28c0c110ee837ca6d1284959
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45575375"
---
# <a name="guidance-for-designing-distributed-tables-in-azure-sql-data-warehouse"></a>Richtlijnen voor het ontwerpen van gedistribueerde tabellen in Azure SQL Data Warehouse
Aanbevelingen voor het ontwerpen van gedistribueerde hash- en round robin gedistribueerde tabellen in Azure SQL Data Warehouse.

In dit artikel wordt ervan uitgegaan dat u bekend bent met de verdeling van de gegevens en gegevens verplaatsingen concepten in SQL Data Warehouse.  Zie voor meer informatie, [Azure SQL Data Warehouse - architectuur van zeer (Massively Parallel Processing)](massively-parallel-processing-mpp-architecture.md). 

## <a name="what-is-a-distributed-table"></a>Wat is een distributietabel?
Een distributietabel wordt weergegeven als één tabel, maar de rijen in 60 distributies zijn opgeslagen. De rijen worden gedistribueerd met een hash- of round robin-algoritme.  

**Hash-gedistribueerde tabellen** verbeteren de prestaties van query's op grote feitentabellen en de focus van dit artikel zijn. **Round robin-tabellen** zijn nuttig voor het verbeteren van laadsnelheid. Deze ontwerpkeuzes hebben een aanzienlijke invloed op het verbeteren van de query en het laden van de prestaties.

Een andere optie voor tabelopslag is een kleine tabel gerepliceerd naar de rekenknooppunten. Zie voor meer informatie, [ontwerprichtlijnen voor gerepliceerde tabellen](design-guidance-for-replicated-tables.md). Als u wilt snel kunt u kiezen tussen de drie opties, Zie gedistribueerde tabellen in de [tabellen overzicht](sql-data-warehouse-tables-overview.md). 

Als onderdeel van het tabelontwerp, krijg inzicht in zo veel mogelijk over uw gegevens en hoe de gegevens in query wordt uitgevoerd.  Bijvoorbeeld, kunt u overwegen deze vragen:

- Hoe groot is de tabel?   
- Hoe vaak wordt de tabel vernieuwd?   
- Heb ik feiten- en dimensietabellen tabellen in een datawarehouse?   


### <a name="hash-distributed"></a>Hash gedistribueerd
Een tabel met hash gedistribueerde tabelrijen verdeeld over de Compute-knooppunten met behulp van een deterministische hash-functie waarmee elke rij aan een [distributie](massively-parallel-processing-mpp-architecture.md#distributions). 

![Gedistribueerde tabel](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "gedistribueerde tabel")  

Aangezien altijd hash-waarden identiek zijn aan de dezelfde distributie, beschikt de datawarehouse over ingebouwde kennis van de rij-locaties. SQL Data Warehouse maakt gebruik van deze kennis om te beperken van gegevensverplaatsing tijdens query's, wat zorgt voor betere prestaties van query's. 

Hash-gedistribueerde tabellen werken goed bij grote feitentabellen in een stervormig schema. Ze kunnen zeer groot aantal rijen hebben en behoud van hoge prestaties. Er zijn natuurlijk sommige ontwerpoverwegingen die u helpen bij het ophalen van de prestaties van die het gedistribueerde systeem is ontworpen om te bieden. Het kiezen van een goede distributiekolom is een dergelijke overweging die in dit artikel wordt beschreven. 

Overweeg het gebruik van een hash-gedistribueerd tabel wanneer:

- De grootte van de tabel op de schijf is groter dan 2 GB.
- De tabel is vaak invoegen, bijwerken en verwijderen van bewerkingen. 

### <a name="round-robin-distributed"></a>Gedistribueerd middels round-robin
Een gedistribueerde round robin-tabel wordt rijen gelijkmatig verspreid over alle distributies. De toewijzing van de rijen aan distributies wordt een willekeurige. In tegenstelling tot hash gedistribueerde tabellen zijn rijen met gelijke waarden niet noodzakelijkerwijs worden toegewezen aan de dezelfde distributie. 

Als gevolg hiervan moet het systeem soms om aan te roepen een verplaatsing van gegevens beter te organiseren uw gegevens voordat deze een query kunt oplossen.  Deze extra stap kan uw query's vertragen. Bijvoorbeeld, moet lid van een round robin-tabel meestal reshuffling de rijen die een treffer prestaties is.

Overweeg het gebruik van de round robin-distributie voor de tabel in de volgende scenario's:

- Wanneer aan de slag als een eenvoudige beginpunt omdat het is de standaard
- Als er geen sleutel voor de hand liggende lid te worden is
- Als er geen goede kandidaatkolom voor hash-distributie van de tabel
- Als de tabel een gemeenschappelijke join-sleutel niet met andere tabellen deelt
- Als de join minder belangrijk dan andere joins in de query is
- Als de tabel is een tijdelijke faseringstabel

De zelfstudie [Load New York over taxi's in gegevens naar Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md#load-the-data-into-your-data-warehouse) geeft een voorbeeld van het laden van gegevens naar een tijdelijke tabel round robin.


## <a name="choosing-a-distribution-column"></a>Het kiezen van een distributiekolom
Een gedistribueerde hash-tabel heeft een distributiekolom die de hashsleutel. De volgende code maakt bijvoorbeeld een tabel met hash worden verdeeld met ProductKey als de distributiekolom.

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
``` 

Het kiezen van een distributiekolom is een belangrijke beslissing omdat de waarden in deze kolom wordt bepaald hoe de rijen worden gedistribueerd. De beste keuze is afhankelijk van diverse factoren en normaal gesproken omvat het gebruik van systeembronnen. Echter, als u niet de aanbevolen kolom de eerste keer kiest, kunt u [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) opnieuw maken in de tabel met een ander distributiepunt-kolom. 

### <a name="choose-a-distribution-column-that-does-not-require-updates"></a>Kies een distributiekolom waarvoor updates
U kunt een distributiekolom niet bijwerken, tenzij u de rij verwijderen en voegt u een nieuwe rij met de bijgewerkte waarden. Selecteer daarom een kolom door statische waarden. 

### <a name="choose-a-distribution-column-with-data-that-distributes-evenly"></a>Kies een distributiekolom met gegevens die worden gelijkmatig verdeeld

Voor de beste prestaties moeten alle van de distributies ongeveer een hetzelfde aantal rijen hebben. Wanneer een of meer distributies een onevenredige aantal rijen zijn, worden bepaalde distributies hun gedeelte van een parallelle query vóór andere voltooid. Omdat de query kan niet worden voltooid als alle distributies verwerking hebt voltooid, is elke query alleen zo snel de traagste distributie.

- Gegevensverschil betekent dat de gegevens niet gelijkmatig is verdeeld over de distributies
- Verwerking scheeftrekken betekent dat bepaalde distributies langer dan andere gebruikers duurt bij het uitvoeren van parallelle query's. Dit kan gebeuren wanneer de gegevens is vervormd.
  
Als u wilt de parallelle verwerking in balans brengen, selecteert u een distributiekolom die:

- **Aantal unieke waarden bevat.** De kolom kan sommige dubbele waarden bevatten. Echter worden alle rijen met dezelfde waarde toegewezen aan de dezelfde distributie. Aangezien er 60 distributies, moet de kolom ten minste 60 unieke waarden bevatten.  Het aantal unieke waarden is meestal veel hoger.
- **Geen null-waarden of is slechts een paar null-waarden.** Voor een extreme voorbeeld, dat als alle waarden in de kolom NULL zijn, alle rijen zijn toegewezen aan de dezelfde distributie. Als gevolg hiervan, verwerking van query's naar één distributie is vervormd en profiteren niet van de parallelle verwerking. 
- **Is niet een datumkolom**. Alle gegevens voor de datum die dezelfde terechtkomt in de dezelfde distributie. Als meerdere gebruikers zijn gefilterd op dezelfde dag, doen slechts 1 van de 60 distributies al het werk van verwerking. 

### <a name="choose-a-distribution-column-that-minimizes-data-movement"></a>Kies een distributiekolom die minimaliseert de verplaatsing van gegevens

Als u de juiste query-query's kan gegevens verplaatsen van één knooppunt naar een andere. Verplaatsing van gegevens gebeurt vaak wanneer query's op gedistribueerde tabellen samenvoegingen en aggregaties hebt. Voor het kiezen van een distributiekolom waarmee de verplaatsing van gegevens beperken, is een van de belangrijkste strategieën voor het optimaliseren van prestaties van uw SQL Data Warehouse.

Om te beperken van gegevensverplaatsing, selecteert u een distributiekolom die:

- Wordt gebruikt in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER`, en `HAVING` van de EU. Wanneer twee grote feitentabellen vaak joins hebben, verbetert de prestaties van query's wanneer u beide tabellen op een van de join-kolommen distribueert.  Wanneer een tabel niet in joins gebruikt wordt, houd rekening met het distribueren van de tabel op een kolom die vaak wordt de `GROUP BY` component.
- Is *niet* gebruikt in `WHERE` van de EU. Dit kan de query niet uitvoeren op alle distributies beperken. 
- Is *niet* een datumkolom. WHERE-componenten zijn vaak van filteren op datum.  Als dit gebeurt, kan de verwerking op slechts een paar distributies uitvoeren.

### <a name="what-to-do-when-none-of-the-columns-are-a-good-distribution-column"></a>Wat te doen wanneer geen van de kolommen een goede distributiekolom zijn

Als geen van de kolommen voldoende afzonderlijke waarden voor een distributiekolom hebt, kunt u een nieuwe kolom maken als een samenstelling van een of meer waarden. Om te voorkomen verplaatsing van gegevens tijdens het uitvoeren van query's, de samengestelde distributiekolom als een join-kolom in query's te gebruiken.

Wanneer u een gedistribueerde hash-tabel ontwerpt, wordt de volgende stap is om gegevens te laden in de tabel.  Zie voor hulp bij het laden, [Laadoverzicht](sql-data-warehouse-overview-load.md). 

## <a name="how-to-tell-if-your-distribution-column-is-a-good-choice"></a>Hoe kunt u zien als uw distributiekolom een goede keuze is
Controleer nadat gegevens zijn geladen in een tabel met hash worden verdeeld, hoe gelijkmatig de rijen worden verdeeld over de 60 distributies. Het aantal rijen per distributie kunnen tot 10% zonder een merkbare invloed op de prestaties variëren. 

### <a name="determine-if-the-table-has-data-skew"></a>Bepalen of de tabel heeft gegevens is een gegevensverschil
Een snelle manier om te controleren of er gegevensverschil is het gebruik van [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql). De volgende SQL-code retourneert het aantal rijen die zijn opgeslagen in elk van de 60 distributies. Voor balans tussen prestaties moeten de rijen in een gedistribueerde gelijkmatig worden verdeeld over alle distributies.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Om te bepalen welke tabellen u meer dan 10% gegevensverschil hebben:

1. Maken van de weergave dbo.vTableSizes die wordt weergegeven in de [tabellen overzicht](sql-data-warehouse-tables-overview.md#table-size-queries) artikel.  
2. Voer de volgende query uit:

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="check-query-plans-for-data-movement"></a>Query-plannen voor verplaatsing van gegevens controleren
Een goede distributiekolom kunt samenvoegingen en aggregaties dat minimale gegevensverplaatsing. Dit is van invloed op de manier waarop joins tekstberichten worden geschreven. Als u de minimale gegevensverplaatsing voor een join op twee hash gedistribueerde tabellen, moet een van de join-kolommen de distributiekolom.  Wanneer twee hash gedistribueerde tabellen voor een distributiekolom van hetzelfde gegevenstype, vereist de join verplaatsing van gegevens niet. Samenvoegingen kunnen extra kolommen gebruiken zonder dat er verplaatsing van gegevens.

Om te voorkomen verplaatsing van gegevens tijdens een join:

- De tabellen die betrokken zijn in de join moeten met hash worden verdeeld op **één** van de kolommen die deel uitmaken van de join.
- De gegevenstypen van de join-kolommen moeten overeenkomen met tussen beide tabellen.
- De kolommen moeten worden samengevoegd met een operator is gelijk aan.
- Het jointype is niet mogelijk een `CROSS JOIN`.

Als u wilt zien als verplaatsing van gegevens is sprake van query's, kunt u het queryplan kijken.  


## <a name="resolve-a-distribution-column-problem"></a>Distributie kolom problemen op te lossen
Het is niet nodig om op te lossen scheeftrekken van alle gevallen van gegevens. Distribueren van gegevens is een kwestie van het zoeken naar de juiste balans tussen het minimaliseren van gegevensverschil en gegevensverplaatsing. Het is niet altijd mogelijk om te minimaliseren gegevensverschil zowel verplaatsing van gegevens. Soms kan het voordeel van de minimale gegevensverplaatsing met opwegen tegen de gevolgen van gegevens is een gegevensverschil.

Om te bepalen als u gegevens moet worden omgezet scheeftrekken in een tabel, dient u te begrijpen zo veel mogelijk over de gegevensvolumes en query's in uw werkbelasting. U kunt de stappen in de [querybewaking](sql-data-warehouse-manage-monitor.md) artikel voor het bewaken van de impact van scheeftrekken op de prestaties van query's. Zoeken om precies hoe lang het duurt voordat een grote query's om uit te voeren in een afzonderlijke distributie.

Nadat u de distributiekolom in een bestaande tabel niet wijzigen, is de gebruikelijke manier om op te lossen gegevensverschil opnieuw maken in de tabel met een ander distributiepunt-kolom.  

### <a name="re-create-the-table-with-a-new-distribution-column"></a>Opnieuw maken in de tabel met een nieuwe distributiekolom
In dit voorbeeld wordt [CREATE TABLE AS SELECT](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?view=aps-pdw-2016-au7) opnieuw maken van een tabel met een andere hash-distributie-kolom.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

## <a name="next-steps"></a>Volgende stappen

Gebruik een van deze instructies voor het maken van een distributietabel:

- [TABEL (Azure SQL datawarehouse) maken](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [TABLE AS SELECT (Azure SQL datawarehouse maken](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)


