---
title: Richtlijnen voor gedistribueerde tabellen - Azure SQL Data Warehouse ontwerpen | Microsoft Docs
description: Aanbevelingen voor het ontwerpen van hash-gedistribueerd en round-robin tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 01/18/2018
ms.author: barbkess
ms.openlocfilehash: 692d92f2e45e04a4eb284b43797b5b468cd9ec1b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="guidance-for-designing-distributed-tables-in-azure-sql-data-warehouse"></a>Richtlijnen voor het ontwerpen van tabellen in Azure SQL Data Warehouse gedistribueerd

Dit artikel bevat aanbevelingen voor het ontwerpen van gedistribueerde tabellen in Azure SQL Data Warehouse. Tabellen hash gedistribueerde query's sneller op grote feitentabellen en zijn gericht aan dit artikel. Round robin-tabellen zijn nuttig voor het laden van de snelheid te verbeteren. Deze ontwerpkeuzes hebben een aanzienlijke gevolgen voor de verbetering van de query en laden van de prestaties.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bekend bent met gegevensdistributie en data movement concepten in SQL Data Warehouse.  Zie voor meer informatie de [architectuur](massively-parallel-processing-mpp-architecture.md) artikel. 

Als onderdeel van tabelontwerp begrijpen zo veel mogelijk over uw gegevens en hoe de gegevens wordt opgevraagd.  Neem bijvoorbeeld deze vragen:

- Hoe groot is de tabel?   
- Hoe vaak wordt de tabel vernieuwd   
- Heb ik feiten- en dimensietabellen tabellen in een datawarehouse?   

## <a name="what-is-a-distributed-table"></a>Wat is een gedistribueerde tabel?
Een gedistribueerde tabel wordt weergegeven als één tabel, maar de rijen daadwerkelijk over 60 distributies worden opgeslagen. De rijen worden gedistribueerd met een hash of round robin-algoritme. 

Een andere tabel opslagoptie is een kleine tabel repliceren op de rekenknooppunten. Zie voor meer informatie [richtlijnen voor gerepliceerde tabellen ontwerpen](design-guidance-for-replicated-tables.md). Om snel tussen de drie opties te selecteren, Zie gedistribueerde tabellen in de [tabellen overzicht](sql-data-warehouse-tables-overview.md). 


### <a name="hash-distributed"></a>Hash gedistribueerd
Een tabel met hash gedistribueerd tabelrijen over verdeeld de rekenknooppunten met een deterministische hash-functie voor elke rij toewijzen aan een [distributie](massively-parallel-processing-mpp-architecture.md#distributions). 

![Gedistribueerde tabel](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "gedistribueerde tabel")  

Aangezien altijd hash-waarden identiek aan de dezelfde distributie, het datawarehouse ingebouwde kennis heeft van de rij-locaties. SQL Data Warehouse maakt gebruik van deze kennis te minimaliseren verplaatsing van gegevens tijdens de query's, wat zorgt voor betere prestaties van query's. 

Gedistribueerd hash tabellen werken goed bij grote feitentabellen in een sterschema. Ze kunnen zeer groot aantal rijen hebben en hoge prestaties nog steeds bereiken. Er zijn natuurlijk sommige ontwerpoverwegingen die u helpen bij het ophalen van de prestaties die het gedistribueerde systeem is ontworpen om te bieden. Het kiezen van een goede distributie-kolom is een dergelijke overweging die in dit artikel wordt beschreven. 

Overweeg het gebruik van hash-gedistribueerd tabel wanneer:

- De grootte van de tabel op schijf is meer dan 2 GB.
- De tabel heeft frequente invoegen, bijwerken en verwijderen van bewerkingen. 

### <a name="round-robin-distributed"></a>Round robin gedistribueerd
Een gedistribueerde round robin-tabel worden tabelrijen gelijkmatig over alle distributies verdeeld. De toewijzing van rijen distributies wordt een willekeurige. In tegenstelling tot gedistribueerd hash tabellen, zijn rijen met gelijke waarden niet gegarandeerd worden toegewezen aan de dezelfde distributie. 

Als gevolg hiervan moet het systeem soms aanroepen van een verplaatsing van gegevens beter te organiseren uw gegevens voordat het oplossen van een query.  Deze extra stap kan uw query's vertragen. Bijvoorbeeld, meestal lid worden van een tabel round robin vereist reshuffling de rijen die een treffer prestaties.

Overweeg het gebruik van de distributie van round robin voor uw tabel in de volgende scenario's:

- Wanneer aan de slag als een eenvoudige beginpunt omdat het de standaardwaarde is
- Als er geen duidelijke die sleutel is
- Als er geen goede kandidaat kolom voor het distribueren van de tabel hash
- Als de tabel een gemeenschappelijke join-sleutel niet met andere tabellen delen
- Als de join minder belangrijk dan andere joins in de query is
- Als de tabel is een tijdelijke tabel fasering

De zelfstudie [laden van gegevens uit Azure Storage-blob](load-data-from-azure-blob-storage-using-polybase.md#load-the-data-into-your-data-warehouse) geeft een voorbeeld van het laden van gegevens naar een tijdelijke tabel round robin.


## <a name="choosing-a-distribution-column"></a>Een kolom distributie kiezen
Een gedistribueerde van hash-tabel bevat een distributie-kolom die het hash-sleutel is. De volgende code maakt bijvoorbeeld een tabel met hash gedistribueerd met ProductKey als de distributie-kolom.

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

Het kiezen van een distributie-kolom is een belangrijk ontwerpbeslissing aangezien de waarden in deze kolom bepalen hoe de rijen worden gedistribueerd. De beste keuze is afhankelijk van verschillende factoren en omvat het doorgaans voor-en nadelen. Echter, als u niet de aanbevolen kolom voor het eerst kiest, kunt u [maken tabel AS selecteren (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) opnieuw te maken in de tabel met een ander distributiepunt-kolom. 

### <a name="choose-a-distribution-column-that-does-not-require-updates"></a>Kies een distributie-kolom die geen updates vereisen
U kunt een distributie-kolom niet bijwerken, tenzij u de rij verwijderen en voegt u een nieuwe rij met de bijgewerkte waarden. Selecteer daarom een kolom met statische waarden. 

### <a name="choose-a-distribution-column-with-data-that-distributes-evenly"></a>Kies een distributie-kolom met gegevens die worden evenredig verdeeld

Voor de beste prestaties moeten alle van de verdelingen ongeveer een hetzelfde aantal rijen hebben. Wanneer een of meer verdelingen een onevenredige aantal rijen hebben, voltooien sommige distributies hun gedeelte van een parallelle query vóór andere. Omdat de query kan niet worden voltooid totdat alle distributies verwerking hebt, is elke query alleen even snel als de traagste distributie.

- Gegevens tijdverschil betekent dat de gegevens is niet gelijkmatig verdeeld over de distributies
- Verwerking scheeftrekken betekent dat een aantal distributies langer dan anderen duren bij het uitvoeren van parallelle query's. Dit kan gebeuren wanneer de gegevens is vervormd.
  
Als u wilt een balans vinden tussen de parallelle verwerking, selecteert u een distributie-kolom die:

- **Veel unieke waarden bevat.** De kolom kan een aantal dubbele waarden hebben. Echter worden alle rijen met dezelfde waarde toegewezen aan de dezelfde distributie. Aangezien er 60 distributies, moet ten minste 60 unieke waarden in de kolom hebben.  Het aantal unieke waarden is meestal veel groter.
- **Geen null-waarden, of alleen enkele Null-waarden heeft.** Voor een uiterst voorbeeld dat als alle waarden in de kolom NULL zijn, alle rijen zijn toegewezen aan de dezelfde distributie. Als gevolg hiervan verwerking van query's naar een distributie is vervormd en komt niet profiteren van parallelle verwerking. 
- **Is niet een datumkolom**. Alle gegevens voor de datum die hetzelfde bestand op de dezelfde distributie. Als meerdere gebruikers zijn gefilterd op dezelfde dag, doen alleen 1 van de 60 distributies al het werk van verwerking. 

### <a name="choose-a-distribution-column-that-minimizes-data-movement"></a>Kies een distributie-kolom die minimaliseert de verplaatsing van gegevens

Als u de juiste query-query's kan verplaatsen van gegevens van één rekenknooppunt naar een andere. Verplaatsing van gegevens gebeurt vaak wanneer query's samenvoegingen en aggregaties op gedistribueerde tabellen. Het kiezen van een distributie-kolom die helpt te beperken van gegevensverplaatsing is een van de belangrijkste strategieën voor het optimaliseren van de prestaties van uw SQL Data Warehouse.

Om te beperken van gegevensverplaatsing, selecteert u een distributie-kolom die:

- Wordt gebruikt in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER`, en `HAVING` componenten. Wanneer twee grote feitentabellen frequente joins, verbetert de prestaties van query's wanneer u beide tabellen op een van de join-kolommen distribueert.  Wanneer een tabel niet in joins gebruikt wordt, rekening houden met het distribueren van de tabel in een kolom die vaak in de `GROUP BY` component.
- Is *niet* gebruikt in `WHERE` componenten. Dit kan de query niet uitvoeren op alle distributies beperken. 
- Is *niet* een datumkolom. Component WHERE wordt vaak filteren op datum.  Als dit gebeurt, kan de verwerking op slechts een paar distributies uitvoeren.

### <a name="what-to-do-when-none-of-the-columns-are-a-good-distribution-column"></a>Wat te doen wanneer geen van de kolommen een goede distributie-kolom zijn

Als er geen goede kandidaat kolommen bestaan, klikt u vervolgens kunt u overwegen round robin als de distributiemethode.

Wanneer u een tabel met hash gedistribueerd ontwerpt, wordt de volgende stap is om gegevens te laden in de tabel.  Zie voor richtlijnen worden geladen, [laden overzicht](sql-data-warehouse-overview-load.md). 

## <a name="how-to-tell-if-your-distribution-column-is-a-good-choice"></a>Hoe kunt u zien als uw distributiepunt-kolom een goede keuze is
Controleer nadat gegevens zijn geladen in een tabel met hash is gedistribueerd, hoe gelijkmatig worden de rijen zijn verdeeld over de 60 distributies. Het aantal rijen per distributiepunt kunnen maximaal 10% zonder een merkbare invloed op prestaties variëren. 

### <a name="determine-if-the-table-has-data-skew"></a>Vaststellen of de tabel heeft leiden tot onjuiste gegevens
Een snelle manier om te controleren of er gegevens tijdverschil is het gebruik van [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql). De volgende SQL-code retourneert het aantal rijen die zijn opgeslagen in elk van de 60 distributies. Voor taakverdeling prestaties moeten de rijen in een gedistribueerde gelijkmatig worden verdeeld over alle verdelingen.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Identificeren welke tabellen meer dan 10% gegevens tijdverschil hebben:

1. Maken van de weergave dbo.vTableSizes die wordt weergegeven in de [tabellen overzicht](sql-data-warehouse-tables-overview.md#table-size-queries) artikel.  
2. Voer de volgende query:

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

### <a name="check-query-plans-for-data-movement"></a>Controleer de queryplannen voor gegevensverplaatsing
Een kolom goede distributie kunt samenvoegingen en aggregaties minimale gegevensverplaatsing hebben. Dit geldt voor de manier waarop joins worden geschreven. Als u de minimale gegevensverplaatsing voor een join twee tabellen hash is gedistribueerd, moet een van de join-kolommen de distributie-kolom.  Wanneer twee tabellen hash gedistribueerd voor een kolom van de distributie van hetzelfde gegevenstype, is de join verplaatsing van gegevens niet vereist. Joins kunnen extra kolommen gebruiken zonder de verplaatsing van gegevens.

Om te voorkomen dat verplaatsing van gegevens tijdens een join:

- De tabellen die betrokken zijn in de join moet hash gedistribueerd bij **één** van de kolommen die deel uitmaken van de join.
- De gegevenstypen van de join-kolommen moeten tussen beide tabellen overeenkomen.
- Moeten lid zijn van de kolommen met een operator equals.
- Het jointype is niet mogelijk een `CROSS JOIN`.

Als u wilt zien als verplaatsing van gegevens is sprake van query's, kunt u het queryplan bekijken.  


## <a name="resolve-a-distribution-column-problem"></a>Distributie kolom problemen op te lossen
Het is niet nodig om op te lossen dat leiden tot onjuiste alle gevallen van gegevens. Distributie van gegevens is een kwestie van het zoeken naar de juiste balans tussen gegevens scheeftrekken en gegevensverplaatsing minimaliseren. Het is niet altijd mogelijk gegevens tijdverschil zowel gegevensverplaatsing minimaliseren. Soms kan het voordeel dat de minimale gegevensverplaatsing opwegen tegen de impact van leiden tot onjuiste gegevens.

Om te beslissen of u gegevens moet oplossen scheeftrekken in een tabel, moet u begrijpen zo veel mogelijk over de gegevensvolumes en query's in uw workload. U kunt de stappen in de [Query bewaking](sql-data-warehouse-manage-monitor.md) artikel voor het bewaken van de impact van scheeftrekken op de prestaties van query's. Zoek in het bijzonder hoe lang het duurt grote query's op afzonderlijke distributies.

Nadat u de distributie-kolom aan een bestaande tabel niet wijzigen, is de gebruikelijke manier voor het oplossen van tijdverschil gegevens opnieuw te maken in de tabel met een ander distributiepunt-kolom.  

### <a name="re-create-the-table-with-a-new-distribution-column"></a>De tabel met een nieuwe kolom voor distributiepunten opnieuw maken
In dit voorbeeld wordt [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) opnieuw maken van een tabel met een andere hash-distributie-kolom.

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

Gebruik een van deze instructies voor het maken van een gedistribueerde tabel:

- [TABEL (Azure SQL datawarehouse) maken](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [TABLE AS SELECT (Azure SQL datawarehouse maken](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)


