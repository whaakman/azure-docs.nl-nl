---
title: Distributie van tabellen in SQL Data Warehouse | Microsoft Docs
description: Aan de slag met het distribueren van tabellen in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: d0e12bf821a81826a20b8db84e76c48fa60ad9b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a>Distributie van tabellen in SQL Data Warehouse
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

SQL Data Warehouse is een gedistribueerd MPP-databasesysteem (Massively Parallel Processing).  Door gegevens- en processorcapaciteit over meerdere knooppunten te verdelen, kan SQL Data Warehouse grote schaalbaarheid bieden, veel groter dan enkel ander systeem.  Bepaalt hoe u uw gegevens in uw SQL Data Warehouse distribueren, is een van de belangrijkste factoren bij het bereiken van optimale prestaties.   Verplaatsing van gegevens voor de sleutel voor optimale prestaties is het minimaliseren en de sleutel voor het beperken van gegevensverplaatsing selecteert op zijn beurt de strategie voor de juiste distributiepunten.

## <a name="understanding-data-movement"></a>Understanding gegevensverplaatsing
In een MPP-systeem, wordt de gegevens van elke tabel verdeeld over verschillende onderliggende databases.  De meest geoptimaliseerde query's op een MPP-systeem kunnen eenvoudig worden doorgegeven via moet worden uitgevoerd op de afzonderlijke gedistribueerde databases zonder interactie tussen de andere databases.  Stel dat u hebt een database met verkoopgegevens die twee tabellen, verkoop en klanten bevat.  Als u een query die u moet uw verkoop tabel toevoegen aan de tabel van uw klant hebt en u uw verkoop- en de klantentabellen up door klantnummer deelt, plaatsen van elke klant in een aparte database, kan geen query's die lid van de verkoop- en kunnen worden opgelost binnen elke database met geen kennis van de andere databases.  Daarentegen als u uw verkoopgegevens gedeeld door het nummer en uw klantgegevens door klantnummer, klikt u vervolgens een bepaalde database geen de bijbehorende gegevens voor elke klant en dus als u uw verkoopgegevens toevoegen aan uw klantgegevens wilt, moet u de gegevens voor elke klant van de andere databases ophalen.  In dit voorbeeld tweede moet verplaatsing van gegevens de om klantgegevens te verplaatsen naar de verkoopgegevens plaatsvinden, zodat de twee tabellen kunnen worden gekoppeld.  

Verplaatsing van gegevens niet altijd slecht, soms is het nodig zijn voor het oplossen van een query.  Maar nadat deze extra stap kan worden vermeden, natuurlijk uw query wordt sneller worden uitgevoerd.  Verplaatsing van gegevens ontstaat meestal als tabellen zijn gekoppeld of aggregaties worden uitgevoerd.  Vaak moet u beide doen, dus terwijl u mogelijk optimaliseren voor een scenario, zoals een join, u nog steeds gegevensverplaatsing moet kunt u voor het scenario, zoals een aggregatie oplossen.  De truc uitzoeken wat minder werk is.  In de meeste gevallen is distributie van grote feitentabellen op een algemeen lid kolom de meest effectieve methode voor het beperken van de meeste gegevensverplaatsing.  Distributie van gegevens op de join-kolommen is een meer algemene methode om te beperken van gegevensverplaatsing dan het distribueren van gegevens voor kolommen die zijn betrokken bij een aggregatie.

## <a name="select-distribution-method"></a>De methode distributiepunten selecteren
Uw gegevens worden in SQL Data Warehouse achter de schermen worden opgedeeld in 60 databases.  Elke individuele database wordt aangeduid als een **distributie**.  Wanneer gegevens in elke tabel is geladen, heeft de SQL Data Warehouse te weten hoe u uw gegevens delen tussen deze 60 verdelingen.  

De verdeling wordt gedefinieerd op het tabelniveau van de en er zijn momenteel twee mogelijkheden:

1. **Round robin** die gegevens distribueren gelijkmatig maar willekeurig.
2. **Hash-gedistribueerde** die gegevens op basis van het hash-waarden uit één kolom distribueert

Standaard, wanneer u een distributiemethode gegevens niet definieert de tabel worden gedistribueerd met behulp van de **round-robin** distributiemethode.  Echter, naarmate u meer geavanceerde in uw implementatie, wilt u Overweeg het gebruik van **hash gedistribueerd** tabellen minimaliseren verplaatsing van gegevens die op zijn beurt optimaliseert query-prestaties.

### <a name="round-robin-tables"></a>Round Robin tabellen
De methode Round-Robin van het distribueren van gegevens is zeer veel hoe het klinkt.  Als uw gegevens zijn geladen, wordt elke rij gewoon verzonden naar de volgende distributie.  Deze methode van het distribueren van de gegevens wordt altijd willekeurig de gegevens zeer gelijkmatig verdelen over alle van de verdelingen.  Er is geen uitgevoerd tijdens de round-robin die plaatst van uw gegevens sorteren.  Een round robin-distributie is wel een willekeurige hash om deze reden.  Met een round robin gedistribueerde tabel hoeft niet te begrijpen van de gegevens.  Round-Robin met tabellen kunnen daarom vaak goed laden doelen.

Standaard als er geen distributiemethode is gekozen, zal de distributiemethode round-robin worden gebruikt.  Round robin-tabellen zijn eenvoudig te gebruiken, omdat gegevens willekeurig in het systeem dit betekent dat het systeem biedt geen garantie van welke distributiepunten wordt gedistribueerd moet elke rij is echter op.  Als gevolg hiervan moet het systeem soms aanroepen van een verplaatsing van gegevens beter te organiseren uw gegevens voordat het oplossen van een query.  Deze extra stap kan uw query's vertragen.

Overweeg het gebruik van Round Robin distributie voor uw tabel in de volgende scenario's:

* Wanneer aan de slag als een eenvoudige beginpunt
* Als er geen duidelijke die sleutel is
* Als er geen goede kandidaat kolom voor het distribueren van de tabel hash
* Als de tabel een gemeenschappelijke join-sleutel niet met andere tabellen delen
* Als de join minder belangrijk dan andere joins in de query is
* Als de tabel is een tijdelijke tabel fasering

Beide voorbeelden maakt een tabel Round Robin:

```SQL
-- Round Robin created by default
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
;

-- Explicitly Created Round Robin Table
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> Tijdens een round-robin wordt het standaardtype van de tabel wordt expliciete in uw DDL wordt beschouwd als een aanbevolen procedure zo in dat het doel van de indeling van de tabel wissen voor anderen.
>
>

### <a name="hash-distributed-tables"></a>Hash-tabellen gedistribueerde
Met behulp van een **Hash gedistribueerd** -algoritme gebruiken voor het distribueren van uw tabellen kan de prestaties verbeteren voor veel scenario's door verplaatsing van gegevens op het moment dat de query.  Gedistribueerd hash-tabellen zijn tabellen die zijn verdeeld tussen de gedistribueerde databases met een hash-algoritme in één kolom die u selecteert.  De distributie-kolom is wat bepaalt hoe de gegevens over uw gedistribueerde databases worden verdeeld.  De hash-functie maakt gebruik van de kolom distributie rijen toewijzen aan distributies.  De hash-algoritme en de resulterende distributie is deterministisch.  Dat is heeft dezelfde waarde met hetzelfde gegevenstype altijd aan de dezelfde distributie.    

In dit voorbeeld maakt een tabel die wordt gedistribueerd bij-id:

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

## <a name="select-distribution-column"></a>De kolom distributiepunten selecteren
Als u zich **hash distribueren** een tabel, moet u een kolom voor één distributiepunt selecteren.  Wanneer u een distributie-kolom selecteert, zijn er drie belangrijke factoren.  

Selecteer één kolom die wordt:

1. Niet worden bijgewerkt
2. Verdelen gegevens, gegevens scheeftrekken voorkomen
3. Verplaatsing van gegevens beperken

### <a name="select-distribution-column-which-will-not-be-updated"></a>Distributiepunten selecteren kolom die wordt niet bijgewerkt
Distributiekolommen kunnen niet worden bijgewerkt, dus, selecteert u een kolom met statische waarden.  Als een kolom moet worden bijgewerkt, wordt dit doorgaans niet goed distributiepunt geschikt is.  Als er een aanvraag waarin u een distributie-kolom moet bijwerken, kunt u dit doen door eerst de rij te verwijderen en voegt vervolgens een nieuwe rij.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Distributiepunten selecteren kolom die gegevens wordt gelijkmatig verdelen
Omdat een gedistribueerd systeem snel als de traagste distributiepunt uitvoert, is het belangrijk dat u het werk gelijkmatig over de distributies verdelen om te kunnen bereiken taakverdeling worden uitgevoerd in het systeem.  De manier waarop die het werk is onderverdeeld in een gedistribueerde systeem is gebaseerd op waar de gegevens voor elke distributie woont.  Hierdoor is het belangrijk dat u selecteert u de kolom van de juiste distributiepunten voor het distribueren van de gegevens zodat elke distributie gelijk zijn werk heeft en is de dezelfde tijd om het gedeelte van het werk te voltooien.  Wanneer het werk goed is onderverdeeld in het systeem, worden de gegevens wordt verdeeld over de distributies.  Wanneer gegevens niet gelijkmatig wordt verdeeld, noemen we dit **gegevens tijdverschil**.  

Als u wilt gegevens gelijkmatig verdelen en te voorkomen dat gegevens scheeftrekken, overweeg het volgende bij het selecteren van de kolom distributie:

1. Selecteer een kolom die een groot aantal afzonderlijke waarden bevat.
2. Vermijd het distribueren van gegevens voor kolommen met een aantal afzonderlijke waarden.
3. Vermijd het distribueren van gegevens voor kolommen met een hoge frequentie van null-waarden.
4. Vermijd het distribueren van gegevens op datumkolommen.

Omdat elke waarde wordt opgedeeld naar 1 van 60 distributies, zodat zelfs distributiepunt wilt u Selecteer een kolom die maximaal uniek is en meer dan 60 unieke waarden bevat.  U kunt een aanvraag waarin een kolom alleen unieke waarden 40 heeft ter illustratie.  Als deze kolom is geselecteerd als de distributiesleutel, zou de gegevens voor die tabel neerzetten op 40 distributies maximaal 20-distributies die met geen gegevens en er is geen verwerking te doen.  Als u daarentegen er de 40 distributies meer werk te doen als de gegevens is meer dan 60 distributies gelijkmatig verdeeld.  Dit scenario is een voorbeeld van gegevens scheeftrekken.

In de MPP-systeem wacht elke stap query tot alle distributies hun aandeel van het werk te voltooien.  Als één distributiepunt doet meer werk dan de andere, klikt u vervolgens de bron van de andere distributies zijn in wezen verspild NET wachten op het distributiepunt bezet.  Wanneer het werk niet gelijkmatig verdeeld over alle distributies, noemen we dit **verwerking scheeftrekken**.  Verwerking tijdverschil zorgt ervoor dat de query's trager dan als de werkbelasting gelijkmatig kan worden verdeeld over de distributies.  Gegevens scheeftrekken zal leiden tot scheeftrekken verwerking.

Vermijd distribueren op maximaal kolom, zoals de null-waarden wordt land op het distributiepunt met dezelfde. Distribueren op een datumkolom kan verwerking tijdverschil ook veroorzaken omdat alle gegevens voor een gegeven datum komt dan uit op de dezelfde distributiepunten. Als meerdere gebruikers worden uitvoeren van query's alle filteren op dezelfde datum, en vervolgens alleen 1 van de 60 distributies doet al het werk sinds een bepaalde datum alleen worden op een distributiepunt. In dit scenario wordt de query's waarschijnlijk 60 keer langzamer dan als de gegevens gelijkmatig zijn verdeeld over alle van de verdelingen uitgevoerd.

Als er geen goede kandidaat kolommen bestaan, klikt u vervolgens kunt u overwegen round-robin als de distributiemethode.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Distributiepunten selecteren kolom verplaatsing van gegevens te minimaliseren
Verplaatsing van gegevens voor het minimaliseren van de kolom rechts distributiepunten te selecteren, is een van de belangrijkste strategieën voor het optimaliseren van de prestaties van uw SQL Data Warehouse.  Verplaatsing van gegevens ontstaat meestal als tabellen zijn gekoppeld of aggregaties worden uitgevoerd.  Kolommen die worden gebruikt `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` en `HAVING` componenten alle maken voor **goed** hash-kandidaten voor distributie.

Anderzijds kolommen in de `WHERE` component komen **niet** maken voor goede hash-kolom kandidaten omdat zij welke distributies deelnemen aan de query beperken, waardoor verwerking scheeftrekken.  Een goed voorbeeld van een kolom die mogelijk verleidelijk te distribueren op, maar vaak kan leiden tot deze scheeftrekken verwerking is een datumkolom.

In het algemeen hebt u twee grote feitentabellen vaak die betrokken zijn in een join, krijgt u de meeste prestaties door het distribueren van beide tabellen op een van de join-kolommen.  Als u een tabel die nooit is gekoppeld aan een andere grote feitentabel hebt, zoekt u naar kolommen die zijn vaak in de `GROUP BY` component.

Er zijn enkele belangrijke criteria waaraan moeten worden voldaan om te voorkomen dat de verplaatsing van gegevens tijdens een join:

1. De tabellen die betrokken zijn in de join moet hash gedistribueerd bij **één** van de kolommen die deel uitmaken van de join.
2. De gegevenstypen van de join-kolommen moeten tussen beide tabellen overeenkomen.
3. Moeten lid zijn van de kolommen met een operator equals.
4. Het jointype is niet mogelijk een `CROSS JOIN`.

## <a name="troubleshooting-data-skew"></a>Analytische gegevens scheeftrekken
Wanneer de gegevens in een tabel is gedistribueerd met behulp van de hash-distributiemethode is een kans dat een aantal distributies zal worden vervormd als u niet goed meer gegevens dan andere. Veel gegevens tijdverschil kan invloed hebben op queryprestaties omdat het uiteindelijke resultaat van een gedistribueerde query tot de langst lopende distributie wachten moet te voltooien. Afhankelijk van de mate van de gegevens tijdverschil moet u mogelijk aanpakken.

### <a name="identifying-skew"></a>Tijdverschil identificeren
Een eenvoudige manier naar een tabel die is vervormd is met `DBCC PDW_SHOWSPACEUSED`.  Dit is een zeer snelle en eenvoudige manier om te zien van het aantal rijen die zijn opgeslagen in elk van de 60 distributies van uw database.  Vergeet niet dat voor de meest evenwichtige prestaties, de rijen in een gedistribueerde moeten gelijkmatig worden verdeeld over alle verdelingen.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Als u een query uitvoeren op de dynamische beheerweergaven (DMV) van Azure SQL Data Warehouse kunt u een meer gedetailleerde analyse uitvoeren.  Als u wilt starten, maak de weergave [dbo.vTableSizes] [ dbo.vTableSizes] bekijken met behulp van de SQL van [tabel overzicht] [ Overview] artikel.  Zodra de weergave is gemaakt, kunt u deze query om te identificeren welke tabellen meer dan 10% gegevens tijdverschil hebben uitgevoerd.

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

### <a name="resolving-data-skew"></a>Het omzetten van gegevens scheeftrekken
Niet alle scheeftrekken is voldoende voor een oplossing heeft.  In sommige gevallen kan de prestaties van een tabel in sommige query's kan bieden, opwegen tegen de beschadiging van gegevens scheeftrekken.  Om te beslissen of u gegevens moet oplossen scheeftrekken in een tabel, moet u begrijpen zo veel mogelijk over de gegevensvolumes en query's in uw workload.   Een manier om te kijken naar de impact van tijdverschil is met de stappen in de [Query bewaking] [ Query Monitoring] artikel voor het bewaken van de impact van scheeftrekken op de prestaties van query's en specifiek de gevolgen voor hoe lang een query uitvoeren op de afzonderlijke distributies.

Distributie van gegevens is een kwestie van het zoeken naar de juiste balans tussen gegevens tijdverschil minimaliseren en de verplaatsing van gegevens voor het minimaliseren. Deze doelstellingen kunnen tegengestelde en soms wilt u gegevens scheeftrekken houden om te reduceren verplaatsing van gegevens. Bijvoorbeeld, wanneer de distributie-kolom vaak de gedeelde kolom in samenvoegingen en aggregaties is, zal u worden minimaliseren verplaatsing van gegevens. Het voordeel dat de minimale gegevensverplaatsing kan bieden, opwegen tegen de impact van leiden tot onjuiste gegevens.

De gebruikelijke manier om op te lossen gegevens tijdverschil is opnieuw te maken in de tabel met een ander distributiepunt-kolom. Omdat er geen manier om de kolom van het distributiepunt op een bestaande tabel, de manier wijzigen van de distributie van een tabel deze opnieuw met een [CTAS] [] te maken.  Hier vindt u twee voorbeelden van hoe u gegevens scheeftrekken oplossen:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Voorbeeld 1: De tabel met een nieuwe kolom voor distributiepunten opnieuw maken
[CTAS] [] wordt dit voorbeeld een tabel met een andere hash-distributie kolom opnieuw moet maken.

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

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Voorbeeld 2: De tabel die gebruikmaken van round robin distributie opnieuw maken
In dit voorbeeld gebruikt [CTAS] [] opnieuw maken van een tabel met round-robin in plaats van een hash-distributiepunt. Deze wijziging wordt zelfs gegevensdistributie koste toegenomen gegevensverplaatsing produceren.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
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
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tabelontwerp, de [distribueren][Distribute], [Index][Index], [partitie][Partition], [gegevenstypen][Data Types], [statistieken] [ Statistics] en [tijdelijke tabellen] [ Temporary] artikelen.

Zie voor een overzicht van best practices [aanbevolen procedures van SQL Data Warehouse][SQL Data Warehouse Best Practices].

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
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
