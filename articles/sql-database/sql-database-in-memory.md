---
title: "Azure SQL Database In het geheugen technologieën | Microsoft Docs"
description: "Azure SQL Database In het geheugen technologieën verbeteren aanzienlijk de prestaties van transactionele en analytics werkbelastingen."
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: develop databases
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: jodebrui
ms.openlocfilehash: 23b313a473b93ba0eab7fc4cf97a5d26bfa31505
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>Prestaties optimaliseren door technologieën voor In-Memory in SQL-Database

In-Memory technologieën in Azure SQL Database gebruikt, kunt u met verschillende workloads voor verbeterde prestaties bereiken: transactionele (online transactionele verwerking (OLTP)), analytics (online analytical processing (OLAP)), en gemengde (hybride transactie/analytische verwerking (HTAP)). Vanwege de efficiënter query en transactieverwerking kunnen In-Memory-technologieën u ook om kosten te beperken. U normaal gesproken hoeft niet bijwerken van de prijscategorie van de database voor betere prestaties. In sommige gevallen u zelfs mogelijk de prijscategorie verminderen tijdens steeds prestatieverbeteringen met technologieën In het geheugen.

Hier vindt u twee voorbeelden van hoe In het geheugen OLTP geholpen prestaties aanzienlijk verbeteren:

- Met behulp van de In-geheugen OLTP [Quorum bedrijfsoplossingen kon worden dubbele van hun werkbelasting en verbetert de dtu's met 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU betekent *dtu*, en bevat een mesurement van resourceverbruik.
- De volgende video toont aanzienlijke verbetering in het verbruik van met een werklast voorbeeld: [In het geheugen OLTP in Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - Zie voor meer informatie het blogbericht: [In het geheugen OLTP in Azure SQL Database-blogberichten](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

In het geheugen technologieën zijn beschikbaar in alle databases in de laag Premium, met inbegrip van databases in Premium elastische pools.

De volgende video wordt uitgelegd mogelijke prestatieverbeteringen met technologieën in Azure SQL Database In het geheugen. Houd er rekening mee dat de prestatieverbetering te bereiken die u altijd ziet afhankelijk is van meerdere factoren, met inbegrip van de aard van de werkbelasting en de gegevens, toegangstijden van de database, enzovoort.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Azure SQL-Database heeft de volgende In het geheugen-technologieën:

- *In het geheugen OLTP* verhoogt de doorvoer en vermindert de latentie voor de transactieverwerking. Scenario's die van de In-geheugen OLTP profiteren zijn: hoge gegevensdoorvoer transactieverwerking zoals handelspartners en games, gegevensopname uit gebeurtenissen of IoT-apparaten, opslaan in cache laden van gegevens en de tijdelijke tabel en de variabele scenario's voor een tabel.
- *Geclusterde columnstore-indexen* (maximaal 10 keer) voor de opslag van kooldioxide verminderen en verbeterde prestaties voor query's voor rapportage en analyse. U kunt deze met feitentabellen in de datamarts meer gegevens in uw database passen en de prestaties verbeteren. Bovendien kunt u deze met historische gegevens in de operationele database te archiveren en kunnen maximaal 10 keer meer gegevens opvragen.
- *Niet-geclusterde columnstore-indexen* voor HTAP helpt u bij de realtime inzicht in uw bedrijf via het opvragen van de operationele database rechtstreeks, zonder de noodzaak om uit te voeren een dure uitpakken, transformeren, en load (ETL)-proces en wacht tot het datawarehouse worden ingevuld. Niet-geclusterde columnstore-indexen uitvoering van zeer snel van analysequery's op de OLTP-database terwijl vermindert de gevolgen voor de operationele werkbelasting.
- U kunt ook de combinatie van een tabel geoptimaliseerd voor geheugen met een columnstore-index hebben. Deze combinatie kunt u zeer snel transactieverwerking, uitvoeren en *gelijktijdig* analysequery's zeer snel worden uitgevoerd op dezelfde gegevens.

Columnstore-indexen zowel In-geheugen OLTP hebben sinds onderdeel van het product van de SQL Server 2012 en 2014, respectievelijk. Azure SQL Database en SQL Server delen de dezelfde implementatie van de technologieën In het geheugen. Voortaan kunt zijn nieuwe mogelijkheden voor deze technologieën uitgebracht in Azure SQL Database eerst voordat ze worden vrijgegeven in SQL Server.

Dit onderwerp wordt beschreven aspecten van de In-geheugen OLTP en columnstore-indexen die specifiek voor Azure SQL Database zijn en tevens voorbeelden:
- Op de opslag- en maximale grootte ervan ziet u de impact van dergelijke technologieën.
- Hier ziet u hoe het verkeer van de databases die gebruikmaken van deze technologieën tussen de verschillende Prijscategorieën beheren.
- Hier ziet u twee voorbeelden die het gebruik van de In-geheugen OLTP, evenals de columnstore-indexen in Azure SQL Database aangeven.

Zie de volgende bronnen voor meer informatie.

Gedetailleerde informatie over de technologieën:

- [In het geheugen OLTP-overzicht en gebruiksscenario's](https://msdn.microsoft.com/library/mt774593.aspx) (inclusief verwijzingen naar casestudy's van klanten en informatie aan de slag)
- [Documentatie voor In-geheugen OLTP](http://msdn.microsoft.com/library/dn133186.aspx)
- [Handleiding voor Columnstore-indexen](https://msdn.microsoft.com/library/gg492088.aspx)
- Hybride transactionele/analytische verwerking (HTAP), ook wel bekend als [operationele realtime-analyses](https://msdn.microsoft.com/library/dn817827.aspx)

Een snelle introductie op In het geheugen OLTP: [snel starten 1: In het geheugen OLTP-technologieën voor sneller T-SQL-prestaties](http://msdn.microsoft.com/library/mt694156.aspx) (een ander artikel om u te helpen aan de slag)

Gedetailleerde video's over de technologieën:

- [In het geheugen OLTP in Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (bevat een demonstratie van de prestatievoordelen en stappen voor deze resultaten reproduceren zelf)
- [In het geheugen OLTP-video's: Wat is en wanneer en hoe u aan deze gebruiken](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [Columnstore-Index: In het geheugen Analytics video's van Ignite 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>De grootte van opslag en gegevens

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Grootte en opslag cap voor In-geheugen OLTP

In het geheugen OLTP bevat geheugen geoptimaliseerde tabellen die worden gebruikt voor het opslaan van gebruikersgegevens. Deze tabellen zijn vereist voor het geheugen. Aangezien u geheugen rechtstreeks in de SQL Database-service beheert, hebben we het concept van een quotum voor gebruikersgegevens. Dit idee wordt aangeduid als *In het geheugen OLTP-opslag*.

Elke prijscategorie en elke elastische pool prijscategorie ondersteunde zelfstandige-database bevat een bepaalde hoeveelheid In het geheugen OLTP-opslag. Op het moment van schrijven krijgt u een gigabyte van opslag van elke 125 database transactie-eenheden (dtu's) of transactie-eenheden voor de elastische database (edtu's). Zie voor meer informatie [limieten](sql-database-resource-limits.md).

De volgende items meetellen voor uw opslagruimte In het geheugen OLTP cap:

- Rijen van de actieve gebruiker gegevens in tabellen geoptimaliseerd voor geheugen en tabelvariabelen. Houd er rekening mee dat de oude rij versies niet voor het kapje meetellen.
- Indexen op tabellen geoptimaliseerd voor geheugen.
- Operationele overhead van ALTER TABLE-bewerkingen.

Als u het kapje raakt, er een foutbericht out van quota en bent u niet langer kunnen invoegen of bijwerken van gegevens. Om deze fout verhelpen, gegevens verwijderen of vergroot de prijscategorie van de database of de groep van toepassingen.

Zie voor meer informatie over het gebruik van de opslag In het geheugen OLTP controleren en waarschuwingen te configureren als u het kapje bijna raakt [Monitor In-Memory opslag](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Over elastische pools

De In-Memory OLTP-opslag wordt gedeeld met elastische pools voor alle databases in de groep. Daarom kan het gebruik in één database mogelijk andere databases beïnvloeden. Er zijn twee oplossingen voor deze:

- Configureer een Max-aantal edtu's voor databases die lager is dan het aantal eDTU voor de groep als geheel. Dit maximum hoofdletters in een database in de groep, de grootte bereikt die overeenkomt met het aantal eDTU's van het opslaggebruik In-Memory OLTP.
- Configureer een minimum-aantal edtu's die groter is dan 0. Dit minimum zorgt ervoor dat elke database in de groep is de hoeveelheid beschikbaar geheugen OLTP-opslag die overeenkomt met de geconfigureerde Min-eDTU.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>De gegevensgrootte van en opslag voor de columnstore-indexen

Columnstore-indexen zijn niet vereist voor het geheugen. De enige initiaal van de grootte van de indexen is daarom maximale totale grootte van de database die wordt beschreven in de [SQL Database Servicelagen](sql-database-service-tiers.md) artikel.

Wanneer u een geclusterde columnstore-indexen gebruikt, worden kolommen compressie wordt gebruikt voor de opslag van de basistabel. Deze compressie kan de voetafdruk van de opslag van uw gebruikersgegevens, wat betekent dat u meer gegevens in de database past aanzienlijk verminderen. En de compressie kan worden verhoogd met [kolommen archivering compressie](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression). De hoeveelheid compressie die u kunt bereiken, is afhankelijk van de aard van de gegevens, maar 10 keer de compressie is niet ongewoon.

Als u een database met een maximale grootte van 1 terabyte (TB hebt) en u 10 keer de compressie bereiken met columnstore-indexen, kunt u een totaal van 10 TB gebruikersgegevens plaatsen in de database.

Wanneer u niet-geclusterde columnstore-indexen gebruikt, wordt de basistabel nog steeds in de indeling van de traditionele rowstore opgeslagen. De opslagbesparing zijn daarom niet zo groot als met geclusterde columnstore-indexen. Als u een aantal niet-geclusterde indexen traditionele met een enkele columnstore-index vervangt, kunt u echter nog steeds in de voetafdruk van de opslag voor de tabel een algehele bespaard zien.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>Verplaatsen van databases die gebruikmaken van technologieën tussen Prijscategorieën In het geheugen

Er zijn nooit eventuele incompatibiliteiten of andere problemen bij het upgraden naar een hogere prijscategorie zoals van standaard naar Premium. De beschikbare functies en bronnen alleen verhogen.

Maar de prijscategorie downgraden kunt negatieve invloed hebben op uw database. De impact is vooral duidelijk wanneer u van Premium naar Standard of Basic downgraden wanneer uw database In het geheugen OLTP-objecten bevat. Tabellen geoptimaliseerd voor geheugen en columnstore-indexen zijn niet beschikbaar na de downgrade (zelfs als ze zichtbaar blijft). Dezelfde overwegingen van toepassing wanneer u de prijscategorie van een elastische pool te verlagen of verplaatsen van een database met technologieën In het geheugen, in een standaard of Basic elastische pool.

### <a name="in-memory-oltp"></a>OLTP in het geheugen

*Downgraden naar Basic/standaard*: In het geheugen OLTP-databases in de categorie Standard of Basic wordt niet ondersteund. Bovendien is het niet mogelijk om een database die met In-geheugen OLTP-objecten naar de prijscategorie Standard of Basic te verplaatsen.

Er is een programmatische manier om te begrijpen of een bepaalde database In het geheugen OLTP ondersteunt. U kunt de volgende Transact-SQL-query uitvoeren:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Als de query retourneert **1**, In het geheugen OLTP wordt ondersteund in deze database.

Voordat u de database naar de standaard en eenvoudige downgraden, verwijdert u alle tabellen geoptimaliseerd voor geheugen en tabeltypen, evenals alle modules met systeemeigen compilatie T-SQL. De volgende query's identificeren alle objecten die worden verwijderd moeten voordat u een database kan worden verlaagd naar standaard en eenvoudige:

```
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

*Met een lagere Premium tier downgraden*: gegevens in tabellen geoptimaliseerd voor geheugen moeten passen binnen de In-Memory OLTP-opslag die is gekoppeld aan de prijscategorie van de database of in de elastische groep beschikbaar is. Als u probeert te verlagen de prijscategorie of de database verplaatsen naar een groep die beschikt niet over voldoende beschikbare In het geheugen OLTP-opslag, de bewerking is mislukt.

### <a name="columnstore-indexes"></a>Columnstore-indexen

*Downgraden naar basis of standaard*: Columnstore-indexen worden alleen op de prijscategorie Premium en niet op de lagen Standard- of Basic ondersteund. Wanneer u de database naar de Standard- of Basic gedowngraded, columnstore-index niet meer beschikbaar. Het systeem onderhoudt columnstore-index, maar deze nooit maakt gebruik van de index. Als u een later terug naar Premium upgrade, kan columnstore-index onmiddellijk opnieuw worden gebruikt.

Als u hebt een **geclusterde** columnstore-index in de hele tabel niet meer beschikbaar is na een downgrade van laag. Daarom raden we aan dat u alle neerzetten *geclusterde* columnstore-indexen voordat u de database onder de laag Premium downgraden.

*Met een lagere Premium tier downgraden*: deze downgrade is geslaagd als de gehele database past binnen de maximale grootte voor het doel prijscategorie of binnen de beschikbare opslagruimte in de elastische groep. Er zijn geen specifieke gevolgen van de columnstore-indexen.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Installeer het In-Memory OLTP-voorbeeld

U kunt de voorbeelddatabase van AdventureWorksLT maken met een paar muisklikken in de [Azure-portal](https://portal.azure.com/). Klik vervolgens in deze sectie wordt uitgelegd hoe u uw database AdventureWorksLT met In-geheugen OLTP-objecten aanvullen en prestatievoordelen demonstreren.

Zie voor een meer eenvoudig, maar meer aantrekkelijk prestaties demo voor In-geheugen OLTP:

- De versie: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Broncode: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Installatiestappen

1. In de [Azure-portal](https://portal.azure.com/), een Premium-database maken op een server. Stel de **bron** met de voorbeelddatabase van AdventureWorksLT. Zie voor gedetailleerde instructies [uw eerste Azure SQL-database maken](sql-database-get-started-portal.md).

2. Verbinding maken met de database met SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Kopieer de [In het geheugen OLTP Transact-SQL-script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) naar het Klembord. De T-SQL-script maakt de benodigde In-Memory-objecten in de voorbeelddatabase van AdventureWorksLT die u in stap 1 hebt gemaakt.

4. De T-SQL-script in SSMS plakken en vervolgens het script wordt uitgevoerd. De `MEMORY_OPTIMIZED = ON` component CREATE TABLE-instructies zijn essentieel. Bijvoorbeeld:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fout 40536


Als u de fout 40536 krijgt wanneer u de T-SQL-script uitvoert, voert u de volgende T-SQL-script om te controleren of de database In het geheugen ondersteunt:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Een resultaat van **0** betekent dat In het geheugen wordt niet ondersteund, en **1** betekent dat wordt ondersteund. Zorg ervoor dat de database zich in de Premium servicecategorie voor diagnose van het probleem.


#### <a name="about-the-created-memory-optimized-items"></a>Over de gemaakte geoptimaliseerd voor geheugen items

**Tabellen**: het voorbeeld bevat de volgende geheugen geoptimaliseerde tabellen:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


U kunt inspecteren tabellen geoptimaliseerd voor geheugen via de **Objectverkenner** in SSMS. Met de rechtermuisknop op **tabellen** > **Filter** > **filterinstellingen** > **Is geoptimaliseerd voor geheugen**. De waarde gelijk is aan 1.


Of u kunt de catalogusweergaven, zoals een query:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Systeemeigen, gecompileerde opgeslagen procedure**: U kunt SalesLT.usp_InsertSalesOrder_inmem inspecteren via een weergave catalogus query:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a>De voorbeeld-OLTP-werkbelasting uitvoeren

Het enige verschil tussen de volgende twee *opgeslagen procedures* is dat de eerste procedure maakt gebruik van versies van de tabellen geoptimaliseerd voor geheugen, terwijl de tweede procedure gebruikt u de gewone tabellen voor op schijf:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


In deze sectie ziet u het gebruik van de handige **ostress.exe** hulpprogramma voor het uitvoeren van de twee opgeslagen procedures op stress niveaus. U kunt vergelijken hoe lang het duurt voordat de twee stress wordt uitgevoerd om te voltooien.


Wanneer u ostress.exe uitvoert, wordt u aangeraden dat u parameterwaarden die zijn ontworpen voor het volgende doorgeeft:

- Uitvoeren van een groot aantal gelijktijdige verbindingen met behulp - n100.
- Hebt u elke lus verbinding honderden tijdstippen, door met de parameter - r500.


Echter, als u wilt beginnen met veel kleiner waarden zoals - n10 en -r50 om ervoor te zorgen dat alles werkt.


### <a name="script-for-ostressexe"></a>Script voor ostress.exe


Deze sectie vindt de T-SQL-script dat is ingesloten in onze ostress.exe vanaf de opdrachtregel. Het script maakt gebruik van items die zijn gemaakt door de T-SQL-script dat u eerder hebt geïnstalleerd.


Het volgende script voegt een verkoop volgorde voorbeeld met vijf artikelen in de volgende geoptimaliseerd voor geheugen *tabellen*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Om de *_ondisk* versie van het voorgaande T-SQL-script voor ostress.exe, vervangt u beide exemplaren van de *_inmem* subtekenreeks met *_ondisk*. Deze vervangingen van invloed op de namen van tabellen en opgeslagen procedures.


### <a name="install-rml-utilities-and-ostress"></a>Hulpprogramma's voor RML en ostress installeren


In het ideale geval zou u van plan bent om uit te voeren ostress.exe op Azure een virtuele machine (VM). Maakt u een [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) in dezelfde Azure geografische regio waarin uw AdventureWorksLT-database zich bevindt. Maar u kunt in plaats daarvan ostress.exe op uw laptop uitvoeren.


Op de virtuele machine of op wat u host kiezen, installeert u de's Replay Markup Language (RML). De hulpprogramma's voor bevatten ostress.exe.

Zie voor meer informatie:
- De bespreking van de ostress.exe in [voorbeelddatabase voor In-geheugen OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
- [De voorbeelddatabase voor In-geheugen OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
- De [blog voor het installeren van ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Voer de *_inmem* eerst stresstest voor de werkbelasting


U kunt een *RML Cmd vragen* venster onze ostress.exe opdrachtregel uitvoeren. De opdrachtregelparameters direct ostress naar:

- 100 verbindingen gelijktijdig worden uitgevoerd (-n100).
- Elke verbinding heeft de T-SQL-script 50 keer uitvoeren (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


De voorgaande ostress.exe opdrachtregel uitvoeren:


1. Opnieuw instellen van de inhoud van de gegevens database door de volgende opdracht in SSMS om alle gegevens die door een eerder uitgevoerde wordt ingevoegd te verwijderen:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. De tekst van de voorgaande ostress.exe vanaf de opdrachtregel naar het Klembord kopiëren.

3. Vervang de `<placeholders>` voor de parameters -S - u. -P -d met de juiste echte waarden.

4. De bewerkte opdrachtregel uitvoeren in een venster RML Cmd.


#### <a name="result-is-a-duration"></a>Resultaat is een duur


Wanneer ostress.exe is voltooid, worden de duur van de uitvoeren als de laatste regel van uitvoer in het venster RML Cmd geschreven. Bijvoorbeeld, een kortere testuitvoering geduurd ongeveer 1,5 minuten:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Opnieuw instellen en bewerken voor *_ondisk*, vervolgens opnieuw uitvoeren


Nadat u het resultaat van hebt de *_inmem* uitvoeren, voert u de volgende stappen voor het *_ondisk* uitvoeren:


1. De database opnieuw instellen door de volgende opdracht in SSMS om alle gegevens te verwijderen dat is ingevoegd door de eerder werd uitgevoerd:
```
EXECUTE Demo.usp_DemoReset;
```

2. Bewerken van de opdrachtregel ostress.exe vervangen van alle *_inmem* met *_ondisk*.

3. Ostress.exe voor de tweede keer opnieuw, en het resultaat van de duur te leggen.

4. De database (voor voorzichtig te verwijderen welke een grote hoeveelheid testgegevens kunnen worden) opnieuw instellen.


#### <a name="expected-comparison-results"></a>Van de verwachte vergelijkingsresultaten

Onze tests In het geheugen is gebleken dat prestaties verbeterd door **negen keer** voor deze eenvoudig werkbelasting, met ostress uitgevoerd op een virtuele machine van Azure in dezelfde Azure-regio als de database.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a>2. Het voorbeeld In-Memory Analytics installeren


In deze sectie maakt vergelijken u de resultaten van de i/o en statistieken wanneer u een columnstore-index ten opzichte van een traditionele b-tree-index.


Voor realtime analyses op een OLTP-werkbelasting is het vaak het beste een niet-geclusterde columnstore-index te gebruiken. Zie voor meer informatie [Columnstore-indexen beschreven](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>De columnstore analytics test voorbereiden


1. Gebruik de Azure portal een database te maken van nieuwe AdventureWorksLT van het voorbeeld.
 - Gebruikt u de exacte naam.
 - Kies een Premium servicecategorie.

2. Kopieer de [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) naar het Klembord.
 - De T-SQL-script maakt de benodigde In-Memory-objecten in de voorbeelddatabase van AdventureWorksLT die u in stap 1 hebt gemaakt.
 - Het script maakt de dimensietabel en twee feitentabellen. De feitentabellen worden ingevuld met 3.5 miljoen rijen.
 - Het script kan 15 minuten duren.

3. De T-SQL-script in SSMS plakken en vervolgens het script wordt uitgevoerd. De **COLUMNSTORE** -sleutelwoord in de **CREATE INDEX** instructie is van cruciaal belang, zoals in:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. AdventureWorksLT ingesteld op compatibiliteitsniveau 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Niveau 130 is niet direct gerelateerd aan functies In het geheugen. Maar niveau 130 biedt doorgaans sneller queryprestaties dan 120.


#### <a name="key-tables-and-columnstore-indexes"></a>Belangrijkste tabellen en columnstore-indexen


- dbo. FactResellerSalesXL_CCI is een tabel die een geclusterde columnstore-index die beschikt over geavanceerde compressie op de *gegevens* niveau.

- dbo. FactResellerSalesXL_PageCompressed is een tabel met een equivalent reguliere geclusterde index die is gecomprimeerd alleen op de *pagina* niveau.


#### <a name="key-queries-to-compare-the-columnstore-index"></a>Sleutel query's vergelijken van de columnstore-index


Er zijn [verschillende typen van de T-SQL-query die u kunt uitvoeren](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) om te zien voor verbeterde prestaties. In stap 2 van de T-SQL-script, moet u aandacht schenken aan deze combinatie van query's. Verschillen slechts op één regel:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Een geclusterde columnstore-index wordt in de FactResellerSalesXL\_CCI tabel.

Het volgende fragment van de T-SQL-script worden statistieken voor i/o- en tijd voor de query van elke tabel afgedrukt.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

In een database met de prijscategorie P2, kunt u ongeveer negen keer de prestatieverbetering voor deze query met behulp van de geclusterde columnstore-index vergeleken met de traditionele index verwachten. Met P15, kunt u ongeveer 57 maal de prestatieverbetering verwachten met behulp van de columnstore-index.



## <a name="next-steps"></a>Volgende stappen

- [1 voor snel starten: In het geheugen OLTP-technologieën voor betere prestaties van de T-SQL](http://msdn.microsoft.com/library/mt694156.aspx)

- [Gebruik In het geheugen OLTP in een bestaande Azure SQL-toepassing](sql-database-in-memory-oltp-migration.md)

- [Opslag van de monitor In-geheugen OLTP](sql-database-in-memory-oltp-monitoring.md) voor In-geheugen OLTP


## <a name="additional-resources"></a>Aanvullende bronnen

#### <a name="deeper-information"></a>Meer gedetailleerde informatie

- [Meer informatie over hoe Quorum wordt verdubbeld sleutel database werkbelasting tijdens DTU verlagen door 70% met In-geheugen OLTP in SQL-Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [In het geheugen OLTP in Azure SQL Database-blogbericht](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Meer informatie over In-geheugen OLTP](http://msdn.microsoft.com/library/dn133186.aspx)

- [Meer informatie over de columnstore-indexen](https://msdn.microsoft.com/library/gg492088.aspx)

- [Meer informatie over realtime operationele analytics](http://msdn.microsoft.com/library/dn817827.aspx)

- Zie [algemene patronen van de werkbelasting en overwegingen bij migratie](http://msdn.microsoft.com/library/dn673538.aspx) (waarin wordt beschreven werkbelasting patronen In het geheugen OLTP waarin vaak aanzienlijke prestatieverbeteringen biedt)

#### <a name="application-design"></a>Het ontwerp van toepassing

- [In het geheugen OLTP (optimalisatie In het geheugen)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Gebruik In het geheugen OLTP in een bestaande Azure SQL-toepassing](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Hulpprogramma's

- [Azure Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx)
