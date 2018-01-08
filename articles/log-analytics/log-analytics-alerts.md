---
title: Understanding alerts in Azure Log Analytics | Microsoft Docs
description: Waarschuwingen in logboekanalyse kunnen belangrijke informatie in de OMS-opslagplaats te identificeren en proactief zullen u informeren over problemen of acties uit om te proberen op te lossen ze aanroepen.  In dit artikel beschrijft de verschillende soorten waarschuwingsregels en hoe ze worden gedefinieerd.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/05/2018
ms.author: bwren
ms.openlocfilehash: 07e8312d5e113eeb9016dcc832b1cf66f8001c5f
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="understanding-alerts-in-log-analytics"></a>Informatie over waarschuwingen in Log Analytics

Waarschuwingen in logboekanalyse identificeren belangrijke informatie in de opslagplaats Log Analytics.  In dit artikel worden enkele van de ontwerpbeslissingen die moeten worden gemaakt op basis van de frequentie van de verzameling van de gegevens van de query, willekeurige vertragingen bij gegevensopname mogelijk veroorzaakt door een netwerklatentie of verwerkingscapaciteit en het doorvoeren van de gegevens in het logboek wordt beschreven Analytics-opslagplaats.  Ook vindt u informatie over hoe waarschuwingsregels in logboekanalyse werk en worden de verschillen tussen verschillende soorten waarschuwingsregels beschreven.

Voor het proces van het maken van regels voor waarschuwingen, Zie de volgende artikelen:

- Maken van regels voor waarschuwingen met [Azure-portal](log-analytics-alerts-creating.md)
- Maken van regels voor waarschuwingen met [Resource Manager-sjabloon](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Maken van regels voor waarschuwingen met [REST-API](log-analytics-api-alerts.md)

## <a name="considerations"></a>Overwegingen

Details over de frequentie van de verzameling gegevens voor verschillende oplossingen en gegevenstype zijn beschikbaar in de [verzameling Gegevensdetails](log-analytics-add-solutions.md#data-collection-details) van het overzicht van oplossingen artikel. Zoals beschreven in dit artikel, frequentie van de verzameling kan worden als incidentele als om de zeven dagen aan *op melding*. Het is belangrijk om te begrijpen en de frequentie van de verzameling gegevens overwegen voordat u kunt een waarschuwing instellen. 

- De frequentie van de verzameling wordt bepaald hoe vaak de OMS-agent op de machines gegevens verzenden met logboekanalyse. Bijvoorbeeld, als de frequentie van de verzameling is 10 minuten en er zijn geen andere vertragingen in het systeem, vervolgens tijdstempels van verzonden gegevens mogelijk overal tussen nul en 10 minuten oude voordat deze is toegevoegd aan de opslagplaats en doorzoekbaar in logboekanalyse is.

- Voordat een waarschuwing kan worden geactiveerd, moet de gegevens worden geschreven naar de opslagplaats zodat deze beschikbaar zijn wanneer een query uitgevoerd. Vanwege de latentie die hierboven worden beschreven, is de frequentie van de verzameling niet hetzelfde zijn als de tijd die de gegevens beschikbaar voor query's zijn. Bijvoorbeeld, terwijl de gegevens zijn mogelijk nauwkeurig elke 10 minuten worden verzameld, zijn de gegevens beschikbaar in de gegevensopslagplaats met onregelmatige intervallen. Gegevens die worden verzameld op nul, 10 en 20 minuten intervallen mogelijk operatie, beschikbaar voor zoekopdrachten om 25 28 en 35 minuten respectievelijk of met een ander onregelmatige interval beïnvloed door opname latentie. Het ergste geval voor deze vertragingen wordt beschreven in de [SLA voor logboekanalyse](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1), die een vertraging die zijn geïntroduceerd in de verzameling frequentie of netwerk latentie tussen de computer en Log Analytics-service niet bevat.


## <a name="alert-rules"></a>Waarschuwingsregels

Waarschuwingen worden gemaakt door regels voor waarschuwingen die automatisch logboek zoekopdrachten met regelmatige tussenpozen worden uitgevoerd.  Een waarschuwing record wordt gemaakt als de resultaten van de zoekopdracht logboek aan bepaalde criteria voldoen.  De regel kan een of meer acties voor het proactief zullen u informeren over de waarschuwing of een ander proces aanroepen vervolgens automatisch uitgevoerd.  Verschillende soorten waarschuwingsregels gebruiken verschillende logica voor deze analyses.

![Waarschuwingen in Log Analytics](media/log-analytics-alerts/overview.png)

Omdat er een verwachte latentie met de opname van logboekgegevens, kan de absolute tijd tussen het indexeren van gegevens en wanneer het beschikbaar om te zoeken is onvoorspelbaar zijn.  De near-realtime-beschikbaarheid van de verzamelde gegevens moet tijdens het definiëren van regels voor waarschuwingen rekening worden gehouden.    

Er is een compromis tussen de betrouwbaarheid van waarschuwingen en de reactiesnelheid van waarschuwingen. U kunt kiezen voor het configureren van Waarschuwingsparameters om te minimaliseren false waarschuwingen en meldingen ontbreken of u kunt Waarschuwingsparameters snel reageren op de voorwaarden die worden bewaakt, maar soms false of gemiste waarschuwingen genereren.

Waarschuwingsregels worden gedefinieerd door de volgende details:

- **Logboek zoeken**.  De query die wordt uitgevoerd telkens als de waarschuwingsregel wordt geactiveerd.  De records geretourneerd door deze query wordt gebruikt om te bepalen of een waarschuwing wordt gemaakt.
- **Tijdvenster**.  Hiermee geeft u het tijdsbereik voor de query.  De query retourneert alleen de records die zijn gemaakt binnen dit bereik van de huidige tijd.  Dit kan een waarde tussen 5 minuten en 24 uur zijn. Het bereik moet breed genoeg voor redelijke vertragingen in de opname. Het tijdvenster moet twee keer de lengte van de langste vertraging die u wilt verwerken.<br> Bijvoorbeeld als u waarschuwingen voor 30 minuten vertragingen betrouwbaar wilt, moet klikt u vervolgens het bereik één uur.  

    Er zijn twee symptomen die kan treedt op als het tijdsbereik te klein is.

    - **Ontbrekende waarschuwingen**. Stel dat u de opname vertraging is 60 minuten soms, maar de meeste gevallen is vijftien minuten.  Als de periode is ingesteld op 30 minuten wordt er een waarschuwing gemist wanneer de vertraging 60 minuten, is omdat de gegevens niet beschikbaar voor zoeken zijn wanneer de waarschuwing query wordt uitgevoerd. 
   
        >[!NOTE]
        >Probeert te achterhalen waarom de waarschuwing is gemist is onmogelijk. Bijvoorbeeld, in het geval is de gegevens worden geschreven naar de opslagplaats 60 minuten nadat de waarschuwing query is uitgevoerd. Als dit de volgende dag opgevallen is dat een waarschuwing is gemist en de volgende dag dat de query wordt uitgevoerd via het juiste interval, de zoekcriteria logboek overeenkomen met het resultaat. Het lijkt dat de waarschuwing moet zijn gestart. De waarschuwing is in feite is niet geactiveerd omdat de gegevens nog niet beschikbaar was wanneer de waarschuwing query is uitgevoerd. 
        >
 
    - **ONWAAR waarschuwingen**. Soms Waarschuwingsquery zijn ontworpen om de afwezigheid van gebeurtenissen te identificeren. Een voorbeeld hiervan wordt gedetecteerd wanneer een virtuele machine door te zoeken naar gemiste heartbeats offline is. Als hierboven, als de heartbeat is niet beschikbaar voor zoeken in het venster tijdstip waarschuwing vervolgens een waarschuwing wordt gegenereerd omdat de heartbeat-gegevens nog niet was doorzoekbare en daarom ontbreekt. Dit is hetzelfde resultaat als de virtuele machine daadwerkelijk offline is en er geen heartbeat-gegevens die worden gegenereerd door het is. De query uit de volgende dag over de juiste tijdvenster wordt weergegeven dat er heartbeats zijn en waarschuwingen is mislukt. In feite was de heartbeats van de niet nog beschikbaar voor zoeken omdat de waarschuwing tijdvenster te klein is ingesteld.  

- **Frequentie**.  Hiermee geeft u op hoe vaak de query moet worden uitgevoerd en kan worden gebruikt om waarschuwingen te laten meer voor de normale aanvraag reageren. De waarde kan tussen vijf minuten en 24 uur en moet gelijk zijn aan of kleiner is dan de waarschuwing tijdvenster.  Als de waarde groter dan de periode is, risico u records wordt overgeslagen.<br>Als het doel is betrouwbaar zijn voor een vertraging tot 30 minuten en de normale vertraging is 10 minuten, het tijdvenster moet één uur en de frequentiewaarde moet 10 minuten. Dit zou een waarschuwing met gegevens met een vertraging van 10 minuten opname tussen 10 en 20 minuten wanneer gegevens van de waarschuwing is gegenereerd.<br>Om te voorkomen dat meerdere waarschuwingen voor dezelfde gegevens maken omdat het tijdvenster te breed is de [waarschuwingen onderdrukt](log-analytics-tutorial-response.md#create-alerts) optie kan worden gebruikt voor het onderdrukken van waarschuwingen voor ten minste zolang het tijdvenster.
  
- **Drempelwaarde**.  De resultaten van de zoekopdracht logboek worden geëvalueerd om te bepalen of een waarschuwing moet worden gemaakt.  De drempelwaarde is verschillend voor de verschillende typen regels voor waarschuwingen.

Elke waarschuwingsregel in Log Analytics is een van twee typen.  Elk van deze typen is beschreven in de volgende secties.

- **[Aantal resultaten](#number-of-results-alert-rules)**. Één waarschuwing gemaakt wanneer het aantal records geretourneerd door de zoekopdracht logboek groter zijn dan een opgegeven getal.
- **[Metrische meting](#metric-measurement-alert-rules)**.  De waarschuwing is gemaakt voor elk object in de resultaten van de zoekopdracht logboek met waarden die groter zijn dan de opgegeven drempelwaarde.

De verschillen tussen waarschuwingsregel typen zijn als volgt.

- **Aantal resultaten** waarschuwingsregel maakt altijd één waarschuwing even **metrische meting** waarschuwingsregel maakt een waarschuwing voor elk object dat de drempelwaarde overschrijdt.
- **Aantal resultaten** waarschuwingsregels genereren een waarschuwing als de drempelwaarde één keer wordt overschreden. **Metrische meting** waarschuwingsregels een waarschuwing kunnen maken wanneer de drempel wordt overschreden een bepaald aantal keren gedurende een bepaald tijdsinterval.

## <a name="number-of-results-alert-rules"></a>Aantal resultaten waarschuwingsregels
**Aantal resultaten** waarschuwingsregels maken van één waarschuwing wanneer het aantal records dat wordt geretourneerd door de query de opgegeven drempelwaarde overschrijdt.

### <a name="threshold"></a>Drempelwaarde
De drempelwaarde voor een **aantal resultaten** waarschuwingsregel gewoon groter is dan of kleiner is dan een bepaalde waarde.  Als het aantal records dat wordt geretourneerd door de zoekopdracht logboek aan deze criteria voldoen, wordt een waarschuwing gemaakt.

### <a name="scenarios"></a>Scenario's

#### <a name="events"></a>Gebeurtenissen
Dit type waarschuwingsregel is ideaal voor het werken met gebeurtenissen, zoals Windows-gebeurtenislogboeken, Syslog, en aangepaste logboeken.  U wilt maken van een waarschuwing wanneer een bepaalde foutgebeurtenis wordt gemaakt, of wanneer er meerdere foutgebeurtenissen worden gemaakt binnen een bepaalde periode.

Voor waarschuwing bij één gebeurtenis, stelt u het aantal resultaten op groter dan 0 en de frequentie en tijdvenster 5 minuten.  Die de query wordt uitgevoerd elke 5 minuten en controleer of het exemplaar van een enkelvoudige gebeurtenis die is gemaakt sinds de laatste keer dat de query werd uitgevoerd.  Een langer frequentie mogelijk vertraagd de tijd tussen de gebeurtenis die wordt verzameld en de waarschuwing wordt gemaakt.

Sommige toepassingen kunnen zich aanmelden voor een incidentele fout die mag niet per se een melding kan genereren.  De toepassing kan bijvoorbeeld het proces opnieuw starten die de foutgebeurtenis gemaakt en slaagt de volgende keer.  U kan in dit geval niet wilt laten maken van de waarschuwing, tenzij meerdere gebeurtenissen worden gemaakt binnen een bepaalde periode.  

In sommige gevallen wilt u mogelijk een waarschuwing in het ontbreken van een gebeurtenis maken.  Bijvoorbeeld, een proces kan zich aanmelden reguliere gebeurtenissen om aan te geven dat deze correct werkt.  Als deze niet een van deze gebeurtenissen binnen een bepaalde periode, moet een waarschuwing worden gemaakt.  In dit geval stelt u de drempelwaarde op **minder dan 1**.

#### <a name="performance-alerts"></a>Waarschuwingen over toepassingsprestaties
[Prestatiegegevens](log-analytics-data-sources-performance-counters.md) wordt opgeslagen als records in de OMS-opslagplaats die vergelijkbaar is met gebeurtenissen.  Als u een waarschuwing wilt wanneer een prestatiemeteritem een bepaalde drempelwaarde overschrijdt, moet deze drempelwaarde worden opgenomen in de query.

Als u wilt een waarschuwing geven wanneer de processor wordt uitgevoerd bijvoorbeeld 90%, zou u een query als volgt met de drempelwaarde voor de waarschuwingsregel **groter dan 0**.

    Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90

Als u een waarschuwing geven wilt wanneer de processor gemiddeld meer dan 90% voor een bepaalde periode, gebruikt u een query met de [opdracht meten](log-analytics-search-reference.md#commands) als volgt met de drempelwaarde voor de waarschuwingsregel **groter dan 0**.

    Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query's in het volgende wijzigen wilt:`Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90`
> `Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | summarize avg(CounterValue) by Computer | where CounterValue>90`


## <a name="metric-measurement-alert-rules"></a>Waarschuwingsregels metrische meting

>[!NOTE]
> Waarschuwingsregels metrische meting zijn momenteel in de openbare preview.

**Metrische meting** waarschuwingsregels maakt een waarschuwing voor elk object in een query met een waarde die een opgegeven drempelwaarde overschrijdt.  Ze hebben de volgende duidelijke verschillen van **aantal resultaten** waarschuwing regels.

#### <a name="log-search"></a>Zoekopdrachten in logboeken
Tijdens het gebruik van een query voor een **aantal resultaten** waarschuwing sluiten, zijn er specifieke vereisten de query voor een waarschuwingsregel metrische meting.  Het omvat een [opdracht meten](log-analytics-search-reference.md#commands) voor het groeperen van de resultaten op een bepaald veld. Met deze opdracht moet de volgende elementen bevatten.

- **Statistische functie**.  Bepaalt de berekening die wordt uitgevoerd en mogelijk een numeriek veld kunnen worden geaggregeerd.  Bijvoorbeeld: **count()** retourneert het aantal records in de query **avg(CounterValue)** wordt het gemiddelde van het veld tegenwaarde retourneren over het interval.
- **Veld groeperen**.  Een record met een cumulatieve waarde voor elk exemplaar van dit veld wordt gemaakt en een waarschuwing worden gegenereerd voor elk.  Bijvoorbeeld, als u wilt dat er waarschuwingen gegenereerd voor elke computer, gebruikt u **door Computer**.   
- **Interval**.  Hiermee definieert u de tijdsinterval waarover de gegevens worden samengevoegd.  Bijvoorbeeld, als u hebt opgegeven **5 minuten**, een record voor elk exemplaar van het groepsveld om de 5 minuten gedurende de periode die is opgegeven voor de waarschuwing geaggregeerd zouden worden gemaakt.

#### <a name="threshold"></a>Drempelwaarde
De drempelwaarde voor metrische meting waarschuwingsregels is gedefinieerd door een cumulatieve waarde en een aantal schendingen.  Als alle gegevenspunten in de zoekopdracht logboek deze waarde overschrijdt, heeft deze beschouwd als een schending.  Als het aantal schendingen in voor elk object in de resultaten van de opgegeven waarde overschrijdt, wordt een waarschuwing gemaakt voor dat object.

#### <a name="example"></a>Voorbeeld
Overweeg een scenario waarin het gewenste een waarschuwing als een computer processorgebruik van 90% driemaal gedurende 30 minuten overschreden.  U zou een waarschuwingsregel maken met de volgende details.  

**Query:** Type = Perf ObjectName = Processor CounterName = "% processortijd" | meten avg(CounterValue) per Computer Interval 5 minuten<br>
**Tijdvenster:** 30 minuten<br>
**Waarschuwing frequentie:** 5 minuten<br>
**Cumulatieve waarde:** groter is dan 90<br>
**Waarschuwing activeren op basis van:** totaal kiezen oplossingen groter is dan 5<br>

De query zou een gemiddelde waarde voor elke computer maken om de 5 minuten.  Deze query worden uitgevoerd om de 5 minuten voor gegevens die worden verzameld in de vorige 30 minuten.  Voorbeeldgegevens worden hieronder weergegeven voor de drie computers.

![Voorbeeld-queryresultaten](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

In dit voorbeeld zou er afzonderlijke waarschuwingen voor srv02 en srv03 worden gemaakt omdat ze de drempelwaarde 90% 3 maal gedurende de periode geschonden.  Als de **Trigger waarschuwing op basis van:** zijn gewijzigd in **opeenvolgend** en vervolgens een waarschuwing alleen voor srv03 zou worden gemaakt omdat deze de drempelwaarde voor 3 opeenvolgende steekproeven geschonden.

## <a name="alert-records"></a>Waarschuwing records
Waarschuwing-records gemaakt door regels voor waarschuwingen in logboekanalyse hebben een **Type** van **waarschuwing** en een **SourceSystem** van **OMS**.  Ze hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*Een waarschuwing* |
| SourceSystem |*OMS* |
| *Object*  | [Metrische maateenheids waarschuwingen](#metric-measurement-alert-rules) heeft een eigenschap voor het groepsveld.  Bijvoorbeeld, als de zoekopdracht logboek groepen op de Computer, hebben de waarschuwing record met een veld van de Computer met de naam van de computer als de waarde.
| AlertName |De naam van de waarschuwing. |
| AlertSeverity |Ernst van de waarschuwing. |
| LinkToSearchResults |Koppelen aan logboekanalyse logboek zoeken waarmee de records geretourneerd van de query die de waarschuwing wordt gemaakt. |
| Query’s uitvoeren |De tekst van de query die is uitgevoerd. |
| QueryExecutionEndTime |Einde van het tijdsbereik voor de query. |
| QueryExecutionStartTime |Begin van het tijdsbereik voor de query. |
| ThresholdOperator | De operator die werd gebruikt door de waarschuwingsregel. |
| ThresholdValue | De waarde die is gebruikt door de waarschuwingsregel. |
| TimeGenerated |De datum en tijd waarop de waarschuwing is gemaakt. |

Er zijn andere soorten waarschuwing records gemaakt door de [waarschuwing beheeroplossing](log-analytics-solution-alert-management.md) en door [Power BI exporteert](log-analytics-powerbi.md).  Deze alle hebben een **Type** van **waarschuwing** maar elkaar worden onderscheiden door hun **SourceSystem**.


## <a name="next-steps"></a>Volgende stappen
* Installeer de [waarschuwing beheeroplossing](log-analytics-solution-alert-management.md) voor het analyseren van waarschuwingen die zijn gemaakt in logboekanalyse samen met de waarschuwingen die worden verzameld van System Center Operations Manager.
* Lees meer over [Meld zoekopdrachten](log-analytics-log-searches.md) kunnen die waarschuwingen genereren.
* Een stapsgewijze Kennismaking voltooien [configureren van een webook](log-analytics-alerts-webhooks.md) met een waarschuwingsregel.  
* Meer informatie over het schrijven van [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) oplossen van problemen die worden aangeduid met waarschuwingen.
