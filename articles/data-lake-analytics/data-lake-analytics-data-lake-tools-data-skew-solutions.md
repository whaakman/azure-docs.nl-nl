---
title: Gegevens tijdverschil problemen oplossen met behulp van Azure Data Lake Tools voor Visual Studio | Microsoft Docs
description: Het oplossen van problemen die gegevens tijdverschil mogelijke oplossingen met behulp van Azure Data Lake Tools voor Visual Studio.
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/16/2016
ms.author: yanacai
ms.openlocfilehash: 9b284ef33be4b935569fc368d81ddf040b2c2b7d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Gegevens tijdverschil problemen oplossen met behulp van Azure Data Lake Tools voor Visual Studio

## <a name="what-is-data-skew"></a>Wat is er gegevens leiden tot onjuiste?

Kort gezegd, is gegevens tijdverschil een overmatig vertegenwoordigde waarde. Stel dat u 50 btw-onderzoekers btw berekent, één examinator voor elke status VS controleren hebt toegewezen. Wyoming name op toezien, heeft omdat er in de populatie klein is, weinig van doen. In Californië, echter wordt name op toezien opgeslagen erg druk is vanwege de grote bevolking van de status.
    ![Voorbeeld van gegevens tijdverschil probleem](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

In ons scenario, is de gegevens ongelijkmatig verdeeld over alle btw-onderzoekers, wat betekent dat sommige onderzoekers meer dan andere moeten werken. In uw eigen baan, moet u vaak situaties zoals in het voorbeeld BTW-examinator hier ervaren. In meer technische voorwaarden één hoekpunt ontvangt veel meer gegevens dan de peers, een situatie waarin het hoekpunt werken meer dan de andere en die uiteindelijk vertraagt een volledige-taak. Wat is er slechter, kan de taak mislukken, omdat de hoekpunten, bijvoorbeeld een beperking 5 uur runtime en een beperking 6 GB geheugen.

## <a name="resolving-data-skew-problems"></a>Het oplossen van problemen met gegevens tijdverschil

Met behulp van Azure Data Lake Tools voor Visual Studio kunt u vaststellen of de taak een probleem gegevens tijdverschil heeft. Als er een probleem is, kunt u deze kunt oplossen door de oplossingen in deze sectie.

## <a name="solution-1-improve-table-partitioning"></a>Oplossing 1: Verbeteren Tabelpartities

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a>Optie 1: De waarde van de asymmetrische sleutel van tevoren filteren

Als het heeft geen invloed op uw bedrijfslogica, kunt u de waarden van de hogere frequentie van tevoren filteren. Bijvoorbeeld, als er een groot aantal 000 000 000 in kolom GUID, raadzaam niet om samen te voegen die waarde. Voordat u cumulatieve, kunt u ' WHERE GUID! '000 000 000' = ' voor het filteren van de hoge frequentie-waarde.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>Optie 2: Kies een andere partitie of distributie-sleutel

In het voorgaande voorbeeld, als u wilt dat alleen voor het controleren van de werkbelasting btw-controle over het land, kunt u verbeteren de gegevensdistributie door het id-nummer als uw sleutel. Verzamelen van een andere partitie of distributiesleutel soms beter kunt verdelen de gegevens, maar u moet ervoor zorgen dat deze keuze niet van invloed op uw bedrijfslogica. Voor de instantie voor het berekenen van de som van de belasting voor elke status kunt u aanwijzen _status_ als de partitiesleutel. Als u dit probleem zich voordoet, probeert u optie 3 blijven.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>Optie 3: Meer partitie of distributie sleutels toevoegen

In plaats van alleen _status_ als een partitiesleutel, kunt u meer dan één sleutel gebruiken voor het partitioneren. Overweeg bijvoorbeeld _postcode_ als een extra partitiesleutel verminderen van de grootte van gegevens partities en de gegevens beter verdelen.

### <a name="option-4-use-round-robin-distribution"></a>Optie 4: Round-robin distributie gebruiken

Als u niet een geschikte sleutel voor partitie en distributie vinden, kunt u proberen om te gebruiken van round-robin distributie. Round-robin distributie worden alle rijen gelijk behandeld en worden ze willekeurig in bijbehorende buckets geplaatst. De gegevens opgehaald gelijkmatig worden verdeeld, maar een nadeel van het ook zo de prestaties van de taak voor bepaalde bewerkingen plaats informatie verliest. Bovendien, als u aggregatie voor de asymmetrische sleutel toch doet, bewaard het probleem gegevens tijdverschil. Voor meer informatie over round-robin distributie, Zie de sectie distributies van U-SQL-tabel in [CREATE TABLE (U-SQL): het maken van een tabel met Schema](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).

## <a name="solution-2-improve-the-query-plan"></a>Oplossing 2: Het queryplan verbeteren

### <a name="option-1-use-the-create-statistics-statement"></a>Optie 1: De instructie CREATE STATISTICS gebruiken

U-SQL biedt de instructie CREATE STATISTICS voor tabellen. Deze instructie biedt meer informatie naar de queryoptimalisatie over de gegevens kenmerken, zoals distributie waarde, die zijn opgeslagen in een tabel. Voor de meeste query's genereert de queryoptimalisatie al de benodigde statistieken voor een hoge kwaliteit queryplan. Soms moet u mogelijk queryprestaties verbeteren door het maken van aanvullende statistieken met CREATE STATISTICS of door het ontwerp van de query wijzigen. Zie voor meer informatie de [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) pagina.

Codevoorbeeld:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>Statistische gegevens wordt niet automatisch bijgewerkt. Als u de gegevens in een tabel bijwerken zonder de statistieken worden opnieuw gemaakt, kan de queryprestaties weigeren.

### <a name="option-2-use-skewfactor"></a>Optie 2: SKEWFACTOR gebruiken

Als u optellen van de belasting voor elke status wilt, moet u GROEPEREN op status, een benadering die niet voorkomen het probleem gegevens tijdverschil dat gebruiken. U kunt echter een hint gegevens opgeven in uw query voor het identificeren van gegevens tijdverschil-sleutels, zodat de optimalisatie van een uitvoeringsplan voor u voorbereiden kunt.

Meestal kunt u de parameter instellen als 0,5 en 1, met 0,5, wat betekent dat niet veel zware tijdverschil scheeftrekken en 1 betekenis. Omdat de hint gevolgen heeft voor uitvoering plan optimalisatie voor de huidige instructie en alle downstream-instructies, zorg er dan voor dat de hint toevoegen voordat u de mogelijkheid vervormd key-wise aggregatie.

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
Naast SKEWFACTOR, kunt specifieke vervormd sleutel join gevallen, als u weet dat de andere gekoppelde rijenset klein is, u zien het optimalisatieprogramma door toe te voegen een ROWCOUNT-hint in de U-SQL-instructie voor samenvoegen. Op deze manier kunt optimaliseren een broadcast-join-strategie om prestaties te verbeteren. Let erop dat ROWCOUNT niet de gegevens tijdverschil probleem is opgelost, maar aanvullende hulp kunnen worden aangeboden.

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

U kunt een door de gebruiker gedefinieerde operator om te gaan met een complex proces logica soms schrijven en een goed geschreven reducer en combiner mogelijk een probleem gegevens tijdverschil in sommige gevallen beperken.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>Optie 1: Een reducer recursieve indien mogelijk gebruiken

Een door de gebruiker gedefinieerde reducer wordt standaard uitgevoerd in de modus voor niet-recursieve, wat betekent dat de hoeveelheid werk voor een sleutel die wordt gedistribueerd in een enkel hoekpunt. Maar als u uw gegevens is vervormd, de grote gegevenssets mogelijk verwerkt in een één hoekpunt en lange tijd worden uitgevoerd.

Om prestaties te verbeteren, kunt u een kenmerk toevoegen in uw code voor het definiëren van reducer in recursieve modus uit te voeren. De grote gegevenssets kunnen vervolgens worden gedistribueerd naar meerdere hoekpunten en parallel, die uw taak sneller worden uitgevoerd.

Als u een niet-recursieve reducer recursieve, moet u ervoor zorgen dat uw algoritme associatieve. Bijvoorbeeld, de som associatieve is en de mediaan is niet. U moet ook om ervoor te zorgen dat de invoer en uitvoer voor reducer, hetzelfde schema behoudt.

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

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>Optie 2: Rijniveau combiner modus gebruiken, indien mogelijk

Net als bij de ROWCOUNT-hint voor specifieke vervormd sleutel join gevallen, combiner modus probeert te grote vervormd-sleutelwaarde sets op meerdere hoekpunten distribueren, zodat het werk gelijktijdig kan worden uitgevoerd. Gegevens tijdverschil problemen kan niet worden omgezet in combiner modus, maar aanvullende hulp voor grote vervormd-sleutelwaarde sets kunnen worden aangeboden.

Standaard is de modus combiner volledig, wat betekent dat de rij links en rechts rij kunnen niet worden gescheiden. De modus instellen als rechts-links/interne kunt rijniveau join. Het systeem scheidt de bijbehorende rij sets en ze worden verdeeld over meerdere hoekpunten die parallel worden uitgevoerd. Voordat u de modus combiner configureren, let er om ervoor te zorgen dat de bijbehorende rij sets kunnen worden gescheiden.

Het volgende voorbeeld ziet u een rijenset gescheiden links. Elke rij van de uitvoer is afhankelijk van één rij invoer vanaf de linkerkant en mogelijk afhankelijk is van alle rijen uit de rechts met dezelfde sleutelwaarde. Als u de modus combiner als links instelt, wordt het systeem scheidt de enorme links-rijenset in kleine netwerken en ze worden toegewezen aan meerdere hoekpunten.

![Afbeelding van combiner-modus](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Als u de verkeerde combiner modus instelt, wordt de combinatie is minder efficiënt en de resultaten mogelijk onjuist.

De kenmerken van combiner modus:

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all the input rows from left and right with the same key value.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Elke rij van de uitvoer hangt af van één rij invoer van links (en mogelijk alle rijen uit de rechts met dezelfde sleutelwaarde).

- qlUserDefinedCombiner(Mode=CombinerMode.Right): elke rij van de uitvoer is afhankelijk van één rij invoer van het recht (en mogelijk alle rijen uit de linkerkant met dezelfde sleutelwaarde).

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Elke rij van de uitvoer hangt af van één rij invoer vanaf de linkerkant en het recht met dezelfde waarde.

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
