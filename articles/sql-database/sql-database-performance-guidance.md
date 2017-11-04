---
title: Azure SQL Database-prestaties afstemmen richtlijnen | Microsoft Docs
description: Meer informatie over het gebruik van aanbevelingen om Azure SQL Database-query-prestaties te verbeteren.
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: dd8d95fa-24b2-4233-b3f1-8e8952a7a22b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 02/09/2017
ms.author: carlrab
ms.openlocfilehash: 5dc245a29a9106156c207ed7394f8bb289db729e
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="tuning-performance-in-azure-sql-database"></a>Afstemming van de prestaties in Azure SQL Database

Azure SQL Database biedt [aanbevelingen](sql-database-advisor.md) dat u gebruiken kunt voor het verbeteren van de prestaties van uw database, of kunt u gebruiken om Azure SQL Database [automatisch aanpassen aan uw toepassing](sql-database-automatic-tuning.md) en wijzigingen die worden de prestaties van uw werkbelasting toepassen.

In dat u geen toepasselijke aanbevelingen, en u nog steeds prestatieproblemen, je kunt de volgende methoden gebruiken voor betere prestaties:
1. Verhogen [Servicelagen](sql-database-service-tiers.md) en bieden meer bronnen met uw database.
2. Uw toepassing afstemmen en enkele aanbevolen procedures die u kunnen de prestaties verbeteren van toepassing. 
3. De database optimaliseren door indexen en query's efficiënter werken met gegevens te wijzigen.

Dit zijn handmatige methodes omdat u nodig hebt om te bepalen wat [Servicelagen](sql-database-service-tiers.md) kiest u of u moet voor het herschrijven van code toepassing of de database en implementeren van de wijzigingen.

## <a name="increasing-performance-tier-of-your-database"></a>Laag van de toenemende prestaties van uw database

Azure SQL Database biedt vier [Servicelagen](sql-database-service-tiers.md) die u kunt kiezen uit: Basic, Standard, Premium en Premium RS (prestaties wordt gemeten in database-eenheden voor doorvoer, of [dtu's](sql-database-what-is-a-dtu.md). Elke servicelaag isoleert uitsluitend de resources in uw SQL-database kunt gebruiken, en wordt gegarandeerd dat voorspelbare prestaties voor dit serviceniveau. In dit artikel bieden we hulp kunt u de servicetier voor uw toepassing kiezen. Dat kunt u uw toepassing Haal het meeste uit Azure SQL Database afstemmen manieren ook besproken.

> [!NOTE]
> Dit artikel is gericht op prestaties richtlijnen voor individuele databases in Azure SQL Database. Zie voor instructies prestaties gerelateerd aan elastische pools [prijs- en Prestatieoverwegingen voor elastische pools](sql-database-elastic-pool-guidance.md). Merk echter op dat u kunt veel afstemmen aanbevelingen in dit artikel van toepassing op databases in een elastische pool en vergelijkbare prestatievoordelen ophalen.
> 

* **Basic**: Basic de service tier aanbiedingen goede prestaties voorspelbaarheid voor elke database, uur ruim uur. In een Basic-database ondersteuning voldoende bronnen zijn voor goede prestaties in een kleine database die niet meerdere, gelijktijdige aanvragen. Er zijn typische gebruiksvoorbeelden wanneer u Basic servicelaag wilt gebruiken:
  * **U alleen aan de slag met Azure SQL Database**. Toepassingen die zich in ontwikkeling vaak hoeft geen hoge prestaties. Basic-databases zijn een ideale omgeving voor databaseontwikkeling of tests, tegen een lage prijs.
  * **U hebt een database met één gebruiker**. Toepassingen die meestal een enkele gebruiker aan een database koppelt hebt geen hoge gelijktijdigheid van taken en prestaties van de vereisten. Deze toepassingen zijn kandidaten voor de laag Basic-service.
* **Standaard**: de standaard servicelaag biedt verbeterde prestaties, voorspelbaarheid en biedt goede prestaties voor databases die meerdere gelijktijdige aanvragen, zoals werkgroep en webtoepassingen. Wanneer u een database van de laag Standard service kiest, u kunt het formaat van uw databasetoepassing op basis van voorspelbare prestaties, minuut gedurende een minuut.
  * **Uw database heeft meerdere gelijktijdige aanvragen**. Toepassingen die meer dan één gebruiker op een tijdstip meestal moeten hogere prestaties. Werkgroep- of toepassingen die hebben laag tot gemiddeld i/o-verkeer vereisten ondersteunen meerdere gelijktijdige query's zijn bijvoorbeeld geschikt voor het serviceniveau Standard.
* **Premium**: de Premium servicecategorie voorziet in voorspelbare prestaties tweede via ten tweede voor elke database Premium. Wanneer u de Premium servicecategorie kiest, kunt u het formaat van uw databasetoepassing op basis van de piekbelasting voor die database. Het plan verwijdert gevallen in welke prestaties van de variantie kleine query's naar het duurt langer dan verwacht in latentie gevoelig bewerkingen kan veroorzaken. Dit model kan aanzienlijk te vereenvoudigen, de ontwikkelings- en validatie cycli voor toepassingen die u wilt maken van sterke instructies over de resourcebehoeften piek, prestaties van de variantie of latentie van query. De meeste Premium-laag service gebruiksvoorbeelden hebben een of meer van deze kenmerken:
  * **Hoge piekbelasting**. Een speciale, hoge prestaties verificatieniveau is vereist door een toepassing die is vereist aanzienlijk CPU, geheugen of input/output (I/O) om de bewerkingen te voltooien. Een databasebewerking bekend is dat het aantal CPU-kernen gebruiken voor een langere periode is bijvoorbeeld geschikt is voor de Premium servicecategorie.
  * **Veel gelijktijdige aanvragen**. Sommige databasetoepassingen-service is veel gelijktijdige aanvragen, bijvoorbeeld wanneer voor een website die een intensief verkeer. Basis en standaard Servicelagen beperkt het aantal gelijktijdige aanvragen per database. Toepassingen waarvoor meer verbindingen moet om een juiste reserveringsgrootte voor het afhandelen van het maximum aantal benodigde aanvragen te kiezen.
  * **Lage latentie**. Sommige toepassingen moeten een reactie van de database in de minimale tijd te garanderen. Als een specifieke opgeslagen procedure als onderdeel van een bredere klant-bewerking wordt aangeroepen, is u wellicht een vereiste om een return-aanroep die in 99 procent van de tijd niet meer dan 20 milliseconden. Dit type toepassing profiteert van de Premium servicecategorie om ervoor te zorgen dat de vereiste rekenkracht beschikbaar is.
* **Premium-RS**: de Premium-RS laag is ontworpen voor i/o-intensieve werkbelastingen die niet wordt gegarandeerd dat de hoogste beschikbaarheid nodig hebt. Voorbeelden omvatten het testen van werklasten met hoge prestaties of een analytische werkbelasting de database is waar geen opslagsysteem.

Het serviceniveau dat u nodig hebt voor de SQL-database, is afhankelijk van de vereisten voor het laden van piek voor elke resourcedimensie. Sommige toepassingen een trivial bedrag van één resource gebruiken, maar belangrijke vereisten voor andere bronnen.

### <a name="service-tier-capabilities-and-limits"></a>Service tier mogelijkheden en beperkingen

Op elke servicelaag instellen u het prestatieniveau, zodat u de flexibiliteit om betaalt alleen voor de capaciteit die u nodig hebt. U kunt [aanpassen capaciteit](sql-database-service-tiers.md), omhoog of omlaag, omdat wijzigingen van de werkbelasting. Als de werkbelasting van uw database tijdens de back-naar-school winkelwagen seizoen hoog is, kunt u het prestatieniveau voor de database verhogen voor een bepaalde periode, juli tot en met September. U kunt deze beperken wanneer uw seizoen piek wordt beëindigd. U betaalt door het optimaliseren van uw cloudomgeving voor de seizoensgebonden van uw bedrijf, kunt u minimaliseren. Dit model ook geschikt is voor software product release cycli. Een testteam mogelijk capaciteit toewijzen terwijl deze wordt uitgevoerd test en vervolgens die capaciteit vrijgeven wanneer ze klaar bent met testen. In een model van de aanvraag capaciteit betaalt u voor capaciteit terwijl u nodig hebt en besparen op toegewijde bronnen die u mogelijk zelden gebruikt.

### <a name="why-service-tiers"></a>Waarom Servicelagen?
Hoewel elke werkbelasting database verschillen kan, is het doel van Servicelagen voor prestaties voorspelbaarheid op verschillende prestatieniveaus. Klanten met grootschalige database resourcevereisten kunnen in een meer specifieke computeromgeving werken.

## <a name="tune-your-application"></a>Uw toepassing optimaliseren
Bij traditionele on-premises SQL Server is het proces van het eerste capaciteitsplanning vaak gescheiden van het proces van het uitvoeren van een toepassing in productie. Hardware- en licenties eerst zijn aangeschaft en prestatieafstemming later wordt uitgevoerd. Wanneer u Azure SQL Database gebruikt, is het een goed idee om het proces van het uitvoeren van een toepassing en het afstemmen interweave. Met het model van de capaciteit op aanvraag betaalt, kunt u uw toepassing gebruik van de minimaal benodigde nu in plaats van overmatige inrichting op hardware gebaseerd op wat van toekomstige groeiplannen voor een toepassing die vaak onjuist resources afstemmen. Sommige klanten kunnen ervoor kiezen niet af te stemmen, een toepassing, maar in plaats daarvan overprovision hardwarebronnen. Deze aanpak is mogelijk een goed idee als u niet wilt wijzigen van een sleutel toepassing gedurende een periode van bezet. Maar afstemming van een toepassing kunt minimaliseren resourcevereisten en maandelijkse rekeningen te verlagen wanneer u de servicecategorieën in Azure SQL Database gebruikt.

### <a name="application-characteristics"></a>Kenmerken van toepassingen
Hoewel Azure SQL Database Servicelagen zijn ontworpen voor betere prestaties stabiliteit en voorspelbaarheid voor een toepassing, kunt enkele aanbevolen procedures u uw toepassing om beter te profiteren van de bronnen op een prestatieniveau van de te optimaliseren. Hoewel veel toepassingen aanzienlijke prestatieverbeteringen hebben gewoon door het overschakelen naar een hoger prestatieniveau of servicelaag, bepaalde toepassingen moeten verder afstemmen om te profiteren van een hoger niveau van de service. Voor betere prestaties kunt u eventueel extra toepassing afstemmen voor toepassingen die de volgende kenmerken:

* **Toepassingen met trage prestaties vanwege 'chatty' gedrag**. Chatty toepassingen maken overmatige gegevenstoegangbewerkingen die gevoelig voor netwerklatentie zijn. Mogelijk moet u deze soorten toepassingen minder gegevenstoegangbewerkingen om de SQL-database te wijzigen. Bijvoorbeeld, u mogelijk de toepassingsprestaties verbeteren door met behulp van technieken zoals batchverwerking ad-hocquery's of het verplaatsen van de query's naar de opgeslagen procedures. Zie voor meer informatie [Batch-query's](#batch-queries).
* **Databases met een intensieve werkbelasting dat kan niet worden ondersteund door een volledige één machine**. Databases die groter is dan de resources van het hoogste niveau van de Premium-prestaties kunnen profiteren van het uitbreiden van de werkbelasting. Zie voor meer informatie [tussen meerdere databases sharding](#cross-database-sharding) en [functionele partitioneren](#functional-partitioning).
* **Toepassingen die suboptimale query's hebben**. Toepassingen, met name voor die in de gegevenstoegangslaag die query's zijn niet goed zijn afgestemd niet nuttig een hoger prestatieniveau. Het gaat hierbij om query's die niet over een WHERE-component, indexen ontbrekende of verouderde statistieken. Deze toepassingen profiteren van de prestaties afstemmen standaard query-technieken. Zie voor meer informatie [ontbrekende indexen](#identifying-and-adding-missing-indexes) en [Query afstemmen en coderingstips](#query-tuning-and-hinting).
* **Toepassingen die suboptimale gegevens hebben toegang tot ontwerp**. Toepassingen die intrinsieke gegevens gelijktijdigheid toegangsproblemen, bijvoorbeeld deadlocking, hebben mogelijk niet profiteren van een hoger prestatieniveau. Houd rekening met retouren op basis van de Azure SQL Database verminderen door in het cachegeheugen gegevens op de client met de service Azure Caching of een andere caching-technologie. Zie [toepassing laag caching](#application-tier-caching).

## <a name="tune-your-database"></a>Uw database afstemmen
In dit gedeelte kijken we enkele technieken die u gebruiken kunt voor Azure SQL Database om toegang te krijgen tot de beste prestaties voor uw toepassing en voer deze uit op het laagste niveau van de mogelijke prestaties afstemmen. Enkele van de volgende manieren traditionele SQL Server best practices afstemmen overeenkomen, maar sommige zijn specifiek voor Azure SQL Database. In sommige gevallen kunt u de verbruikte resources voor een database vinden gebieden verder afstemmen en uitbreiden van de traditionele SQL Server-technieken om te werken in Azure SQL Database onderzoeken.

### <a name="identify-performance-issues-using-azure-portal"></a>Identificeren van prestatieproblemen met Azure portal
De volgende hulpprogramma's in de Azure portal kunt u analyseren en oplossen van prestatieproblemen met uw SQL-database:

* [Inzicht in queryprestaties](sql-database-query-performance.md)
* [SQL Database Advisor](sql-database-advisor.md)

De Azure-portal bevat meer informatie over beide van deze hulpprogramma's en hoe ze te gebruiken. Efficiënt opsporen en corrigeren van problemen, wordt u aangeraden eerst de hulpprogramma's in de Azure-portal te proberen. Het is raadzaam dat u de handmatige methoden die we vervolgens bespreken ontbrekende indexen en query afstemmen, in bijzondere gevallen afstemmen.

Meer informatie over het identificeren van problemen in Azure SQL Database op [prestatiebewaking](sql-database-single-database-monitor.md) artikel.

### <a name="identifying-and-adding-missing-indexes"></a>Te identificeren en ontbrekende indexen toe te voegen
Er is een veelvoorkomend probleem in de prestaties van de OLTP-database gekoppeld aan het ontwerp van de fysieke database. Vaak worden databaseschema ontworpen en getest op schaal (ofwel in load of gegevensvolume) verzonden. De prestaties van een queryplan kan helaas aanvaardbaar op kleine schaal maar aanzienlijk verslechteren onder productieniveau gegevensvolumes. De meest voorkomende oorzaak van dit probleem is het ontbreken van de juiste indexen om te voldoen aan de filters of andere beperkingen in een query. Vaak ontbrekende indexen manifesten als een tabel als kan worden volstaan met een index seek scannen.

In dit voorbeeld gebruikt de geselecteerde queryplan een scan wanneer seek zou voldoende:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Het queryplan met ontbrekende indexen](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL Database kunt u zoeken naar en fix algemene ontbreken index voorwaarden. DMV's die zijn ingebouwd in Azure SQL Database bekijken querycompilaties waarin een index aanzienlijk sneller de geschatte kosten voor het uitvoeren van een query. Tijdens het uitvoeren van query houdt SQL-Database hoe vaak elke queryplan wordt uitgevoerd, en houdt de geschatte kloof tussen het queryplan wordt uitgevoerd en de imagined waar die index bestaat. U kunt deze DMV's snel raden welke wijzigingen in het ontwerp van de fysieke database totale kosten van de werkbelasting voor een database en de werkelijke werkbelasting kunnen worden verbeterd.

Deze query kunt u mogelijke ontbrekende indexen evalueren:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

In dit voorbeeld is de query heeft geresulteerd in deze suggestie:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Nadat deze gemaakt, haalt die dezelfde SELECT-instructie in een ander abonnement, dat een seek gebruikt in plaats van een scan, en het plan vervolgens efficiënter uitgevoerd:

![Een queryplan met de gecorrigeerde indexen](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Het belangrijkste inzicht is dat de i/o-capaciteit van een gedeelde, standaard-systeem beperkter dan die van een speciale server-machine. Er is een premium op voor het minimaliseren van onnodige i/o om te kunnen maximaal profiteren van het systeem in de DTU van elke prestatieniveau van de Azure SQL Database servicecategorieën. Ontwerp van de juiste fysieke database keuzen kunnen aanzienlijk verbeteren de latentie voor afzonderlijke query's, de doorvoer van gelijktijdige aanvragen verwerkt per schaaleenheid verbeteren en de kosten die zijn vereist om te voldoen aan de query te minimaliseren. Zie voor meer informatie over de ontbrekende index DMV's [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Query afstemmen en coderingstips
Queryoptimalisatie in Azure SQL Database is vergelijkbaar met de traditionele SQL Server-queryoptimalisatie. De meeste van aanbevolen procedures voor het afstemmen van query's en kennis van de redenering model beperkingen voor queryoptimalisatie ook van toepassing op Azure SQL Database. Als u query's in Azure SQL Database afstemmen, krijgt u mogelijk extra voordeel cumulatieve resourcebehoeften verminderen. Uw toepassing mogelijk om uit te voeren, tegen lagere kosten dan een ruiseffect gelijkwaardige omdat deze kan worden uitgevoerd op een lager prestatieniveau.

Een voorbeeld die vaak worden uitgevoerd in SQL Server en dat geldt ook voor Azure SQL Database is hoe queryoptimalisatie 'bithandtekeningen' parameters. Tijdens de compilatie evalueert queryoptimalisatie de huidige waarde van een parameter om te bepalen of dit meer optimale queryplan kan genereren. Hoewel deze strategie vaak tot aanzienlijk sneller dan een plan dat is gecompileerd zonder bekende parameterwaarden queryplan leiden kan, momenteel het werkt dat zowel in SQL Server en in Azure SQL Database. Soms is de parameter niet sniff, en soms de parameter is sniff, maar het gegenereerde plan suboptimale voor de volledige set van parameterwaarden in een werkbelasting. Microsoft levert queryhints (richtlijnen) zodat u kunt meer doelbewust doel opgeven en het standaardgedrag van de parameter het bewaken van netwerken negeren. Als u hints gebruikt, kunt u vaak gevallen waarin het standaardgedrag voor SQL Server of Azure SQL Database voor een specifieke klant werkbelasting imperfecte oplossen.

Het volgende voorbeeld laat zien hoe een plan dat suboptimale voor prestaties en resourcevereisten is in de queryprocessor kan worden gegenereerd. Dit voorbeeld ziet ook als u een hint query gebruikt, u query uitvoeren tijd- en vereisten voor de SQL-database verkleinen kunt:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

De Setupcode maakt een tabel die Gegevensdistributie is vervormd. Het optimale queryplan verschilt op basis van welke parameter is geselecteerd. Helaas Format niet het plan cachegedrag altijd de query op basis van de meest voorkomende waarde van parameter. Het is dus mogelijk voor een suboptimale plan worden in de cache opgeslagen en gebruikt voor meerdere waarden, zelfs wanneer een ander schema is mogelijk een betere keuze voor plan gemiddeld. Het queryplan maakt vervolgens twee opgeslagen procedures die identiek zijn, behalve dat een een hint speciale query heeft.

**Bijvoorbeeld: deel 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Bijvoorbeeld: deel 2**

(We raden u ten minste 10 minuten voordat u begint met deel 2 van het voorbeeld, zodat de resultaten uniek is in de resulterende telemetriegegevens zijn.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Elk deel van dit voorbeeld probeert uit te voeren van een instructie insert geparameteriseerde 1000 keer (voor het genereren van een voldoende belasting moet worden gebruikt als een testgegevensset). Bij het uitvoeren van opgeslagen procedures, onderzoekt de queryprocessor de waarde van de parameter die wordt doorgegeven aan de procedure bij de eerste compileren (parameter ' bekijken'). De processor voor het resulterende plan-cache en wordt gebruikt voor latere aanroepen, zelfs als de waarde van parameter verschilt. De optimale planning kan niet worden gebruikt in alle gevallen. Soms moet u een handleiding voor het optimaliseren om te selecteren van een plan dat is beter geschikt voor het gemiddelde geval in plaats van de specifieke aanvraag uit wanneer de query voor het eerst is gecompileerd. In dit voorbeeld genereert het oorspronkelijke plan een plan 'scan', die alle rijen om elke waarde die overeenkomt met de parameter leest:

![Query afstemmen met behulp van een scan plannen](./media/sql-database-performance-guidance/query_tuning_1.png)

Aangezien we de procedure hebt uitgevoerd met behulp van de waarde 1, het resulterende plan is optimaal is voor de waarde 1 maar suboptimale voor alle andere waarden in de tabel. Het resultaat waarschijnlijk niet wat u zou willen zijn als u kiest elk plan willekeurig, omdat het plan traag meer en meer bronnen worden gebruikt.

Als u de test met uitvoert `SET STATISTICS IO` ingesteld op `ON`, het werk logische scan in dit voorbeeld wordt uitgevoerd op de achtergrond. U kunt zien dat er 1,148 leesbewerkingen gedaan door het plan (dit is inefficiënt als de gemiddelde wordt slechts één rij retourneren) zijn:

![Query afstemmen met behulp van een logische scan](./media/sql-database-performance-guidance/query_tuning_2.png)

Het tweede gedeelte van het voorbeeld maakt gebruik van een query-hint zien of de optimalisatie van een specifieke waarde gebruiken tijdens de compilatie. In dit geval wordt het zorgt ervoor dat de queryprocessor voor het negeren van de waarde die is doorgegeven als parameter, en in plaats daarvan te mogen nemen `UNKNOWN`. Dit verwijst naar een waarde die de gemiddelde frequentie in de tabel is (tijdverschil worden genegeerd). Het resulterende plan is een seek op basis van een plan dat is sneller en gebruikt minder bronnen, Gemiddeld, dan het plan in dit voorbeeld, deel 1:

![Query afstemmen met behulp van een query-hint](./media/sql-database-performance-guidance/query_tuning_3.png)

Ziet u het effect in de **sys.resource_stats** tabel (Er is een vertraging vanaf het moment dat u de test en wanneer de gegevens in de tabel gevuld uitvoeren). Voor dit voorbeeld, deel 1 uitgevoerd tijdens de periode 22:25:00 en deel 2 om 22:35:00 uur uitgevoerd. De eerdere periode meer bronnen in dat tijdvenster hoger dan de (vanwege plan efficiëntieverbeteringen) gebruikt.

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Prestatieafstemming voorbeeld queryresultaten](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> Hoewel het volume in dit voorbeeld opzettelijk klein is, kan het effect van suboptimale parameters aanzienlijke vooral op database groter worden. Het verschil in uitzonderlijke gevallen mag tussen seconden voor snelle gevallen en uur trage gevallen.
> 
> 

U kunt controleren **sys.resource_stats** om te bepalen of de bron voor een test meer of minder resources dan een andere test gebruikt. Als u gegevens vergelijken, scheidt u de timing van tests zodat ze niet in hetzelfde venster 5 minuten in de **sys.resource_stats** weergeven. Het doel van de oefening is om te beperken van de totale hoeveelheid resources die worden gebruikt en niet naar de piek-resources te minimaliseren. In het algemeen minder een stuk code voor latentie optimaliseren ook brongebruik. Zorg ervoor dat de wijzigingen die u in een toepassing aanbrengt nodig zijn en de wijzigingen niet negatieve invloed op de gebruikerservaring voor iemand die queryhints in de toepassing gebruikmaken mogelijk.

Als een werklast een set heeft met herhaalde query's, is vaak het handig om te leggen en valideren van de optimalisatie van uw keuzes plan omdat deze de de minimale grootte eenheid die is vereist voor het hosten van de database. Nadat u deze valideren klikken zo nu en dan de abonnementen kunt u ervoor zorgen dat ze hebben niet is gedegradeerd. U kunt meer lezen over [hints (Transact-SQL) query](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Meerdere databases sharding
Omdat Azure SQL Database wordt uitgevoerd op basishardware, zijn de capaciteitslimieten voor één database lager dan bij traditionele on-premises installatie van SQL Server. Sommige klanten gebruik sharding technieken databasebewerkingen verspreiden over meerdere databases als de bewerkingen niet binnen de grenzen van een individuele database in Azure SQL Database passen. De meeste klanten die sharding technieken in Azure SQL Database gebruiken wordt hun gegevens op één dimensie verdelen over meerdere databases. Voor deze benadering moet u begrijpen dat OLTP-toepassingen vaak transacties die van toepassing op slechts één rij of rijen in het schema een kleine groep uitvoeren.

> [!NOTE]
> SQL-Database biedt nu een om te helpen bij sharding-bibliotheek. Zie voor meer informatie [overzicht van clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md).
> 
> 

Bijvoorbeeld, als een database heeft de naam van de klant, volgorde en de details van de order (zoals de traditionele voorbeeld Northwind-database die wordt geleverd met SQL Server), u kan deze gegevens splitsen in meerdere databases door te groeperen met de verwante order en gedetailleerde informatie over volgorde van de klant. U kunt garanderen dat de gegevens van de klant in één database blijft. De toepassing zou verschillende klanten verdelen over databases en effectief de belasting verspreid over meerdere databases. Klanten niet alleen kunnen voorkomen dat u de maximale limiet met sharding, maar Azure SQL Database ook werkbelastingen die aanzienlijk groter dan de grenzen van de verschillende prestatieniveaus zijn, zolang elke afzonderlijke database binnen de DTU past kan verwerken.

Hoewel sharding van de database niet de capaciteit van de cumulatieve bron voor een oplossing te verminderen, is het zeer effectief te ondersteunen van zeer grote oplossingen die worden verdeeld over meerdere databases. Elke database kunt uitvoeren op een andere prestatieniveau voor de ondersteuning van zeer grote 'effectieve' databases met hoge benodigde middelen.

### <a name="functional-partitioning"></a>Functionele partities
SQL Server-gebruikers combineren vaak veel functies in één database. Bijvoorbeeld, als een toepassing logica heeft voor het beheren van de inventaris voor een winkel, wellicht dat de database logica die is gekoppeld aan Voorraadbeheer, inkoop, opgeslagen procedures en geïndexeerde of gerealiseerde weergaven die het einde van de maand rapportage beheren. Op deze manier kunt eenvoudiger te beheren van de database voor bewerkingen zoals back-up, maar ook moet u het formaat van de hardware voor het afhandelen van de piekbelasting over alle functies van een toepassing.

Als u een uitbreidbare architectuur in Azure SQL Database gebruikt, is het een goed idee om op te splitsen verschillende functies van een toepassing in andere databases. Met deze techniek kunnen schaalt elke toepassing onafhankelijk van elkaar. Als een toepassing veelgebruikte wordt (en de belasting van de database verhoogt), kan de beheerder kan onafhankelijk prestatieniveaus voor elke functie kiezen in de toepassing. De bereikt, klikt u met deze architectuur kan een toepassing niet groter zijn dan een machine één product verwerken kan, omdat de belasting wordt verdeeld over meerdere machines.

### <a name="batch-queries"></a>Batch-query 's
Voor toepassingen die toegang gegevens tot met behulp van hoog volume, regelmatig, ad hoc query wordt uitgevoerd, een aanzienlijke hoeveelheid reactietijd is besteed aan netwerkcommunicatie tussen de toepassingslaag en de Azure SQL Database-laag. Zelfs als de toepassing en de Azure SQL Database in hetzelfde datacenter zijn, kan de netwerklatentie tussen de twee worden vergroot door een groot aantal gegevens toegangsbewerkingen. Om te beperken van het netwerk round reizen voor de data access-bewerkingen, overweeg het gebruik van de optie in een batch moet ofwel de ad-hocquery's of voor het compileren van als opgeslagen procedures. Als u de ad-hocquery's batch, kunt u meerdere query's verzenden als één grote batch in een enkele reis naar Azure SQL Database. Als u ad-hocquery's in een opgeslagen procedure compileert, kunt u hetzelfde resultaat kan bereiken, alsof u deze batch. Met behulp van een opgeslagen procedure kunt u ook het voordeel van het verhogen van de kans op de queryplannen in Azure SQL Database in cache opslaan zodat u de opgeslagen procedure opnieuw kunt gebruiken.

Sommige toepassingen zijn schrijven-intensief. U kunt de totale i/o-belasting van een database soms verminderen door rekening te houden hoe batch schrijfbewerkingen. Dit is vaak net zo eenvoudig als expliciete transacties via in plaats van automatisch doorvoeren transacties in de opgeslagen procedures en ad-hoc batches. Zie voor een evaluatie van verschillende technieken die u kunt gebruiken, [batchverwerking technieken voor toepassingen in Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimenteer met de werkbelasting van uw eigen vinden van het juiste model voor batchverwerking. Zorg dat u begrijpt dat een model mogelijk enigszins verschillen transactionele consistentie wordt gegarandeerd. Zoeken naar de juiste werkbelasting die gebruik van bronnen minimaliseert, moet zoeken naar de juiste combinatie van de consistentie en prestaties-en nadelen.

### <a name="application-tier-caching"></a>Toepassingslaag caching
Sommige databasetoepassingen hebben lezen zware workloads. Opslaan in cache lagen mogelijk de belasting van de database te verminderen en het prestatieniveau vereist ter ondersteuning van een database met behulp van Azure SQL Database mogelijk verkleint. Met [Azure Redis-Cache](https://azure.microsoft.com/services/cache/), als u een werkbelasting lezen zware hebt, kunt u de gegevens eenmaal lezen (of bijvoorbeeld eenmaal per toepassingslaag machine, afhankelijk van hoe deze is geconfigureerd), en die gegevens buiten uw SQL-database op te slaan. Dit is een manier om te beperken van de database laden (CPU en i/o-lezen), maar er is een effect op transactionele consistentie omdat de gegevens worden gelezen uit de cache is mogelijk niet gesynchroniseerd met de gegevens in de database. Hoewel in veel toepassingen een zekere mate van inconsistentie acceptabel is, die geldt niet voor alle werkbelastingen. U moet eventuele toepassingsvereisten volledig begrijpen voordat u een toepassing lagen in het cachegeheugen strategie implementeert.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over Servicelagen [SQL Database-opties en prestaties](sql-database-service-tiers.md)
* Zie voor meer informatie over elastische pools [wat is er een Azure elastische groep?](sql-database-elastic-pool.md)
* Zie voor meer informatie over prestaties en elastische pools [wanneer te overwegen een elastische pool](sql-database-elastic-pool-guidance.md)

