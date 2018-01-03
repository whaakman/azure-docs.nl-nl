---
title: Understanding alerts in Azure Log Analytics | Microsoft Docs
description: Waarschuwingen in logboekanalyse kunnen belangrijke informatie in de OMS-opslagplaats te identificeren en proactief zullen u informeren over problemen of acties uit om te proberen op te lossen ze aanroepen.  In dit artikel beschrijft de verschillende soorten waarschuwingsregels en hoe ze worden gedefinieerd.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/13/2017
ms.author: bwren
ms.openlocfilehash: a0897113660f764cb23239b066bc93c479a9a553
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="understanding-alerts-in-log-analytics"></a>Informatie over waarschuwingen in Log Analytics

Waarschuwingen in logboekanalyse identificeren belangrijke informatie in de opslagplaats Log Analytics.  Dit artikel vindt u informatie over hoe waarschuwingsregels in logboekanalyse werk en worden de verschillen tussen verschillende soorten waarschuwingsregels beschreven.

Voor het proces van het maken van regels voor waarschuwingen, Zie de volgende artikelen:

- Maken van regels voor waarschuwingen met [Azure-portal](log-analytics-alerts-creating.md)
- Maken van regels voor waarschuwingen met [Resource Manager-sjabloon](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Maken van regels voor waarschuwingen met [REST-API](log-analytics-api-alerts.md)


## <a name="alert-rules"></a>Waarschuwingsregels

Waarschuwingen worden gemaakt door regels voor waarschuwingen die automatisch logboek zoekopdrachten met regelmatige tussenpozen worden uitgevoerd.  Een waarschuwing record wordt gemaakt als de resultaten van de zoekopdracht logboek aan bepaalde criteria voldoen.  De regel kan een of meer acties voor het proactief zullen u informeren over de waarschuwing of een ander proces aanroepen vervolgens automatisch uitgevoerd.  Verschillende soorten waarschuwingsregels gebruiken verschillende logica voor deze analyses.

![Waarschuwingen in Log Analytics](media/log-analytics-alerts/overview.png)

Waarschuwingsregels worden gedefinieerd door de volgende details:

- **Logboek zoeken**.  De query die wordt uitgevoerd telkens als de waarschuwingsregel wordt geactiveerd.  De records geretourneerd door deze query wordt gebruikt om te bepalen of een waarschuwing wordt gemaakt.
- **Tijdvenster**.  Hiermee geeft u het tijdsbereik voor de query.  De query retourneert alleen de records die zijn gemaakt binnen dit bereik van de huidige tijd.  Dit kan een waarde tussen 5 minuten en 24 uur zijn. Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd om 1:15 uur, wordt alleen de records die zijn gemaakt tussen 12:15 uur en 1:15 PM geretourneerd.
- **Frequentie**.  Hiermee geeft u op hoe vaak de query moet worden uitgevoerd. Is een waarde tussen 5 minuten en 24 uur. Moet gelijk zijn aan of kleiner is dan de periode.  Als de waarde groter dan de periode is, risico u records wordt overgeslagen.<br>Neem bijvoorbeeld een tijdvenster van 30 minuten en een frequentie van 60 minuten.  Als de query wordt uitgevoerd om 1:00 uur, wordt er records tussen 12:30 en 13:00 uur.  De volgende keer dat de query werd uitgevoerd is 2:00 wanneer er records tussen 1:30 en 2:00 zou geretourneerd.  Alle records gemaakt tussen de 1:00 en 1:30 zou nooit worden geëvalueerd.
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

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90

    

Als u een waarschuwing geven wilt wanneer de processor gemiddeld meer dan 90% voor een bepaalde periode, zou u een query als volgt gebruiken met de drempelwaarde voor de waarschuwingsregel **groter dan 0**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | where CounterValue>90 | summarize avg(CounterValue) by Computer

    
>[!NOTE]
> Als uw werkruimte is nog niet zijn bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en vervolgens de bovenstaande query's wijzigen in het volgende bij de laatste met behulp van wilt de [opdracht meten](log-analytics-search-reference.md#commands):`Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90`
> `Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90`


## <a name="metric-measurement-alert-rules"></a>Waarschuwingsregels metrische meting

>[!NOTE]
> Waarschuwingsregels metrische meting zijn momenteel in de openbare preview.

**Metrische meting** waarschuwingsregels maakt een waarschuwing voor elk object in een query met een waarde die een opgegeven drempelwaarde overschrijdt.  Ze hebben de volgende duidelijke verschillen van **aantal resultaten** waarschuwing regels.

#### <a name="log-search"></a>Zoekopdrachten in logboeken
Tijdens het gebruik van een query voor een **aantal resultaten** waarschuwing sluiten, zijn er specifieke vereisten de query voor een waarschuwingsregel metrische meting.  Het omvat een [opdracht meten](log-analytics-search-reference.md#commands) voor het groeperen van de resultaten op een bepaald veld. Met deze opdracht moet de volgende elementen bevatten.

- **Statistische functie**.  Bepaalt de berekening die wordt uitgevoerd en mogelijk een numeriek veld kunnen worden geaggregeerd.  Bijvoorbeeld: **count()** retourneert het aantal records in de query **avg(CounterValue)** wordt het gemiddelde van het veld tegenwaarde retourneren over het interval.
- **Veld groeperen**.  Een record met een cumulatieve waarde voor elk exemplaar van dit veld wordt gemaakt en een waarschuwing worden gegenereerd voor elk.  Bijvoorbeeld, als u wilt dat er waarschuwingen gegenereerd voor elke computer, gebruikt u **door Computer**.   
- **Interval**.  Hiermee definieert u de tijdsinterval waarover de gegevens worden samengevoegd.  Bijvoorbeeld, als u hebt opgegeven **5minutes**, een record voor elk exemplaar van het groepsveld om de 5 minuten gedurende de periode die is opgegeven voor de waarschuwing geaggregeerd zouden worden gemaakt.

#### <a name="threshold"></a>Drempelwaarde
De drempelwaarde voor metrische meting waarschuwingsregels is gedefinieerd door een cumulatieve waarde en een aantal schendingen.  Als alle gegevenspunten in de zoekopdracht logboek deze waarde overschrijdt, heeft deze beschouwd als een schending.  Als het aantal schendingen in voor elk object in de resultaten van de opgegeven waarde overschrijdt, wordt een waarschuwing gemaakt voor dat object.

#### <a name="example"></a>Voorbeeld
Overweeg een scenario waarin het gewenste een waarschuwing als een computer processorgebruik van 90% driemaal gedurende 30 minuten overschreden.  U zou een waarschuwingsregel maken met de volgende details.  

**Query:** Perf | waar ObjectName == 'Processor' en CounterName == "% processortijd" | samenvatten AggregatedValue = avg(CounterValue) door bin (TimeGenerated, 5 m), Computer<br>
**Tijdvenster:** 30 minuten<br>
**Waarschuwing frequentie:** 5 minuten<br>
**Cumulatieve waarde:** groter is dan 90<br>
**Waarschuwing activeren op basis van:** totaal kiezen oplossingen groter is dan 2<br>

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
* Een stapsgewijze Kennismaking voltooien [configureren van een webhook](log-analytics-alerts-webhooks.md) met een waarschuwingsregel.  
* Meer informatie over het schrijven van [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) oplossen van problemen die worden aangeduid met waarschuwingen.
