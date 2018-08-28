---
title: Gegevensverschil problemen oplossen met behulp van Azure Data Lake Tools voor Visual Studio
description: Mogelijke oplossingen voor gegevensverschil problemen oplossen met behulp van Azure Data Lake Tools voor Visual Studio.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 12/16/2016
ms.openlocfilehash: b3079a7f2e71e26164d96cf167b67f1a60f7a23b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046470"
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Gegevensverschil problemen oplossen met behulp van Azure Data Lake Tools voor Visual Studio

## <a name="what-is-data-skew"></a>Wat is data scheeftrekken?

Kort gezegd, is gegevensverschil een te veel weergegeven waarde. Stel dat u 50 belasting onderzoekers om te controleren van de belastingaangiften, een onderzoeker voor elke status van de Verenigde Staten hebt toegewezen. De onderzoeker Wyoming, heeft omdat de populatie er klein is, weinig te doen. In Californië, maar wordt de onderzoeker opgeslagen bezet vanwege de grote populatie van de status.
    ![Voorbeeld van de probleem gegevensverschil](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

In ons scenario, is de gegevens ongelijkmatig verdeeld over alle btw-onderzoekers, dat betekent dat sommige onderzoekers meer dan anderen moeten samenwerken. In uw eigen baan, moet u vaak situaties, zoals in dit voorbeeld BTW-onderzoeker ervaren. Meer technische voorwaarden één hoekpunt nog veel meer gegevens dan de collega's worden opgehaald, een situatie waarin het hoekpunt werken meer dan de andere en die uiteindelijk een hele taak vertraagt. Wat is slechter, mislukt deze taak mogelijk, omdat hoekpunten kunnen hebben, bijvoorbeeld een beperking 5 uur runtime en een beperking 6 GB geheugen.

## <a name="resolving-data-skew-problems"></a>Gegevensverschil problemen oplossen

Met behulp van Azure Data Lake Tools voor Visual Studio kunt u vaststellen of uw taak een gegevensverschil probleem is. Als er een probleem bestaat, kunt u deze kunt oplossen door te proberen de oplossingen die in deze sectie.

## <a name="solution-1-improve-table-partitioning"></a>Oplossing 1: Verbeteren tabellen partitioneren

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a>Optie 1: De waarde van de asymmetrische sleutel vooraf filteren

Als het heeft geen invloed op uw bedrijfslogica, kunt u de waarden van de hogere frequentie vooraf filteren. Bijvoorbeeld, als er een groot aantal 000-000-000 in kolom GUID, raadzaam niet om samen te voegen die waarde. Voordat u samen te voegen kunt u ' WHERE GUID! = "000-000-000" "voor het filteren van de waarde van de hoge frequentie.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>Optie 2: Kies een andere partitie of distributie-sleutel

In het voorgaande voorbeeld, als u wilt dat alleen voor het controleren van de werkbelasting van de btw-controle over het land, kunt u verbeteren de verdeling van de gegevens door te selecteren van het ID-nummer als de sleutel. Verzamelen van een andere partitie of distributiesleutel soms beter kunt verdelen de gegevens, maar u moet ervoor zorgen dat deze optie heeft geen invloed op uw bedrijfslogica. Bijvoorbeeld, voor het berekenen van de som van de belasting voor elke status, kunt u aan te wijzen _status_ als de partitiesleutel. Als u dit probleem zich blijft voordoen, probeert u optie 3.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>Optie 3: Meer sleutels voor partitie of distributiepunt toevoegen

In plaats van alleen _status_ als een partitiesleutel, kunt u meer dan één sleutel gebruiken voor het partitioneren van. Bijvoorbeeld, kunt u toevoegen _postcode_ als een aanvullende partitiesleutel om te verminderen van de grootte van gegevens-partities en meer gelijkmatig verdelen van de gegevens.

### <a name="option-4-use-round-robin-distribution"></a>Optie 4: Gebruik van round robin-distributie

Als u niet een geschikte sleutel voor de partitie en distributie vinden, kunt u proberen te gebruiken van round robin-distributie. Round robin-distributie worden alle rijen gelijk behandeld en willekeurig, worden de tegels in de bijbehorende buckets. De gegevens gelijkmatig verdeeld, maar u verliest deze locatie-informatie, een nadeel van die prestaties van de taak voor bepaalde bewerkingen kan ook worden verminderd. Bovendien, als u cumulatie-instellingen voor de asymmetrische sleutel toch doet, wordt het probleem gegevensverschil behouden. Voor meer informatie over round-robin-distributie, Zie de sectie distributies van U-SQL-tabel in [CREATE TABLE (U-SQL): het maken van een tabel met Schema](https://msdn.microsoft.com/library/mt706196.aspx#dis_sch).

## <a name="solution-2-improve-the-query-plan"></a>Oplossing 2: Het queryplan verbeteren

### <a name="option-1-use-the-create-statistics-statement"></a>Optie 1: Gebruik de instructie CREATE STATISTICS

U-SQL biedt de instructie CREATE STATISTICS voor tabellen. Deze instructie geeft meer informatie naar het queryoptimalisatieprogramma over de gegevenskenmerken, zoals de verdeling van de waarde, die zijn opgeslagen in een tabel. Voor de meeste query's, wordt het queryoptimalisatieprogramma al de benodigde statistieken voor een hoge kwaliteit queryplan gegenereerd. In sommige gevallen moet u mogelijk de queryprestaties verbeteren door het maken van aanvullende statistieken met CREATE STATISTICS of door het wijzigen van het ontwerp van de query. Zie voor meer informatie de [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/library/azure/mt771898.aspx) pagina.

Codevoorbeeld:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>Statistische gegevens wordt niet automatisch bijgewerkt. Als u de gegevens in een tabel bijwerken zonder opnieuw maken van de statistieken, kunt de prestaties van query's weigeren.

### <a name="option-2-use-skewfactor"></a>Optie 2: SKEWFACTOR gebruiken

Als u wilt de som van de belasting voor elke status, moet u de GROUP BY staat, een benadering die niet voorkomen het probleem gegevensverschil dat-gebruiken. U kunt echter een hint gegevens opgeven in de query voor het identificeren van gegevensverschil-sleutels, zodat het optimalisatieprogramma een uitvoeringsplan voor u voorbereiden kunt.

Meestal kunt u de parameter 0,5 als 1, wat betekent dat niet veel scheeftrekken en 1 betekenis zware scheeftrekken 0,5 instellen. Omdat de hint is van invloed op uitvoeringsplan optimalisatie voor de huidige instructie en alle downstream-instructies, moet u de hint toevoegen voordat u de kans vervormd key-wise aggregatie.

    SKEWFACTOR (columns) = x

    Provides a hint that the given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

Codevoorbeeld:

    //Add a SKEWFACTOR hint.
    @Impressions =
        SELECT * FROM
        searchDM.SML.PageView(@start, @end) AS PageView
        OPTION(SKEWFACTOR(Query)=0.5)
        ;

    //Query 1 for key: Query, ClientId
    @Sessions =
        SELECT
            ClientId,
            Query,
            SUM(PageClicks) AS Clicks
        FROM
            @Impressions
        GROUP BY
            Query, ClientId
        ;

    //Query 2 for Key: Query
    @Display =
        SELECT * FROM @Sessions
            INNER JOIN @Campaigns
                ON @Sessions.Query == @Campaigns.Query
        ;   

### <a name="option-3-use-rowcount"></a>Optie 3: ROWCOUNT gebruiken  
Naast SKEWFACTOR, kunt voor specifieke ongelijkmatig sleutel join gevallen, als u weet dat de andere is toegevoegd aan rijenset klein is, u zien het optimalisatieprogramma door toe te voegen een ROWCOUNT-hint in de U-SQL-instructie voor samenvoegen. Op deze manier kunt optimaliseren een broadcast join-strategie om prestaties te verbeteren. Let erop dat het gegevensverschil probleem niet wordt opgelost door het aantal rijen, maar deze extra hulp kan bieden.

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

Codevoorbeeld:

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information to determine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-the-user-defined-reducer-and-combiner"></a>Oplossing 3: De gebruiker gedefinieerde reducer en combiner verbeteren

U kunt een door de gebruiker gedefinieerde operator logische gecompliceerd zijn getroffen soms schrijven en een goed geschreven reducer en combiner een gegevensverschil probleem in sommige gevallen mogelijk beperken.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>Optie 1: Een recursieve reducer, indien mogelijk gebruiken

Een door de gebruiker gedefinieerde reducer wordt standaard uitgevoerd in de modus voor niet-recursieve, wat betekent dat de hoeveelheid werk voor een sleutel wordt gedistribueerd naar een hoekpunt. Maar als uw gegevens is vervormd, de enorme gegevenssets kan worden verwerkt in een hoekpunt en gedurende langere tijd worden uitgevoerd.

U kunt een kenmerk in de code voor het definiëren van reducer om uit te voeren in de modus voor recursieve toevoegen voor betere prestaties. De enorme gegevenssets kan vervolgens worden gedistribueerd naar meerdere hoekpunten en parallel uit te voeren die de taak wordt versneld.

Als u een niet-recursieve reducer recursieve, moet u om ervoor te zorgen dat de algoritme associatieve is. Bijvoorbeeld, de som is associatieve en de mediaan is niet. U moet ook om ervoor te zorgen dat de invoer en uitvoer voor reducer hetzelfde schema blijven.

Kenmerk van recursieve reducer:

    [SqlUserDefinedReducer(IsRecursive = true)]

Codevoorbeeld:

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>Optie 2: Beveiliging op rijniveau combiner modus gebruiken, indien mogelijk

Net als bij de ROWCOUNT-hint voor specifieke ongelijkmatig sleutel join gevallen, combiner modus wil enorme ongelijkmatig-sleutel-waarde ingesteld op meerdere hoekpunten verdelen zodat het werk kan gelijktijdig worden uitgevoerd. Gegevensverschil problemen kan niet worden omgezet door combiner modus, maar aanvullende hulp voor sets met enorme ongelijkmatig-sleutel-waarde kunnen worden aangeboden.

Standaard is de modus combiner volledig, wat betekent dat de rij naar links en rechts rij kunnen niet worden gescheiden. De beveiligingsmodus instellen als links/rechts/Inner join voor beveiliging op rijniveau kunt. Het systeem scheidt het bijbehorende rij wordt ingesteld en worden ze verdeeld over meerdere hoekpunten die parallel worden uitgevoerd. Voordat u de modus combiner configureert, zijn echter zorgvuldig om ervoor te zorgen dat de bijbehorende rij sets kunnen worden gescheiden.

Het volgende voorbeeld toont een rijenset gescheiden links. Elke rij van de uitvoer is afhankelijk van één invoer rij aan de linkerkant en mogelijk afhankelijk is van alle rijen vanaf de rechterkant met dezelfde sleutelwaarde. Als u de modus combiner links instelt, wordt het systeem worden gescheiden van de enorme links-rij instellen in kleine netwerken en toegewezen aan meerdere hoekpunten.

![Afbeelding van combiner-modus](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Als u de verkeerde combiner modus instelt, wordt de combinatie is minder efficiënt en de resultaten mogelijk onjuist.

Kenmerken van combiner modus:

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all the input rows from left and right with the same key value.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Elke rij uitvoer afhankelijk is van één invoer rij vanaf de linkerkant (en mogelijk alle rijen vanaf de rechterkant met dezelfde sleutelwaarde).

- qlUserDefinedCombiner(Mode=CombinerMode.Right): elke rij van de uitvoer is afhankelijk van één invoer rij van het recht (en mogelijk alle rijen vanaf de linkerkant met dezelfde sleutelwaarde).

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Elke rij uitvoer afhankelijk is van één invoer rij van de links en rechts met dezelfde waarde.

Codevoorbeeld:

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
