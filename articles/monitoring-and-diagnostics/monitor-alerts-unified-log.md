---
title: Logboek waarschuwingen in Azure Monitor - waarschuwingen (Preview) | Microsoft Docs
description: Trigger e-mailberichten, meldingen, worden URL's van websites (webhooks) of automation aanroepen wanneer de complexe query's door u opgegeven voorwaarden wordt voldaan voor Azure waarschuwingen (Preview).
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: vinagara
ms.openlocfilehash: 438776e7f0885dbdb0d66ccdd18d854e14beb299
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="log-alerts-in-azure-monitor---alerts-preview"></a>Logboek waarschuwingen in Azure Monitor - waarschuwingen (Preview)
Dit artikel vindt u informatie over hoe waarschuwingsregels in Analytics query's voor werk in Azure waarschuwingen (Preview) en worden de verschillen tussen verschillende soorten logboek waarschuwingsregels beschreven.

Momenteel Azure waarschuwingen (Preview), ondersteunt Meld u waarschuwingen op query's van [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) en [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events).

> [!WARNING]

> Logboek waarschuwing in Azure waarschuwingen (Preview) ondersteunt momenteel geen cross-werkruimte of cross-app-query's.

Ook gebruikers hun query's in Analytics platform van keuze in Azure kunnen verbeteren en vervolgens *importeren voor gebruik in waarschuwingen (Preview) door het opslaan van de query*. Volgende stappen uitvoeren:
- Voor Application Insights: Ga naar Analytics-portal, query's en de resultaten te valideren. Sla met unieke naam in *gedeelde query's*.
- Voor logboekanalyse: Ga naar logboek zoeken, query's en de resultaten te valideren. Gebruik vervolgens opslaan met de unieke naam in elke categorie.

Klik wanneer [maken van een waarschuwing in een logboek in waarschuwingen (Preview)](monitor-alerts-unified-usage.md), ziet u de opgeslagen query vermeld als signaaltype **logboek (opgeslagen Query)**; zoals geïllustreerd in onderstaand voorbeeld: ![opgeslagen Query die zijn geïmporteerd op waarschuwingen](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Met behulp van **logboek (opgeslagen Query)** resulteert in een import op waarschuwingen. Eventuele wijzigingen die zijn uitgevoerd na in Analytics wordt daarom niet worden reflecteert in opgeslagen waarschuwingsregels en vice versa.

## <a name="log-alert-rules"></a>Meld u regels voor waarschuwingen

Waarschuwingen worden gemaakt met Azure-waarschuwingen (Preview) automatisch logboek query's uitvoeren met regelmatige tussenpozen.  Als de resultaten van de query logboek aan bepaalde criteria voldoen, wordt een waarschuwing record gemaakt. Een of meer acties voor het proactief zullen u informeren over de waarschuwing of een ander proces, zoals het verzenden van gegevens voor het gebruik van de externe toepassing aanroepen kan vervolgens automatisch uitgevoerd door de regel [json gebaseerde webhook](monitor-alerts-unified-log-webhook.md)met [actiegroepen](monitoring-action-groups.md). Verschillende soorten waarschuwingsregels gebruiken verschillende logica voor deze analyses.

Waarschuwingsregels worden gedefinieerd door de volgende details:

- **Meld u Query**.  De query die wordt uitgevoerd telkens als de waarschuwingsregel wordt geactiveerd.  De records geretourneerd door deze query worden gebruikt om te bepalen of een waarschuwing wordt gemaakt.
- **Tijdvenster**.  Hiermee geeft u het tijdsbereik voor de query.  De query retourneert alleen de records die zijn gemaakt binnen dit bereik van de huidige tijd.  Tijdvenster is een waarde tussen 5 minuten en 1440 minuten of 24 uur. Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd om 1:15 uur, wordt alleen de records die zijn gemaakt tussen 12:15 uur en 1:15 PM geretourneerd.
- **Frequentie**.  Hiermee geeft u op hoe vaak de query moet worden uitgevoerd. Is een waarde tussen 5 minuten en 24 uur. Moet gelijk zijn aan of kleiner is dan de periode.  Als de waarde groter dan de periode is, risico u records wordt overgeslagen.<br>Neem bijvoorbeeld een tijdvenster van 30 minuten en een frequentie van 60 minuten.  Als de query wordt uitgevoerd om 1:00 uur, wordt er records tussen 12:30 en 13:00 uur.  De volgende keer dat de query werd uitgevoerd is 2:00 wanneer er records tussen 1:30 en 2:00 zou geretourneerd.  Alle records gemaakt tussen de 1:00 en 1:30 zou nooit worden geëvalueerd.
- **Drempelwaarde**.  De resultaten van de zoekopdracht logboek worden geëvalueerd om te bepalen of een waarschuwing moet worden gemaakt.  De drempelwaarde is verschillend voor de verschillende typen regels voor waarschuwingen.

Elke waarschuwingsregel in Log Analytics is een van twee typen.  Elk van deze typen is beschreven in de volgende secties.

- **[Aantal resultaten](#number-of-results-alert-rules)**. Één waarschuwing gemaakt wanneer het aantal records geretourneerd door de zoekopdracht logboek groter zijn dan een opgegeven getal.
- **[Metrische meting](#metric-measurement-alert-rules)**.  De waarschuwing is gemaakt voor elk object in de resultaten van de zoekopdracht logboek met waarden die groter zijn dan de opgegeven drempelwaarde.

De verschillen tussen waarschuwingsregel typen zijn als volgt.

- ** Aantal resultaten waarschuwingsregels maakt altijd één waarschuwing even **metrische meting** waarschuwingsregel maakt een waarschuwing voor elk object dat de drempelwaarde overschrijdt.
- **Aantal resultaten** waarschuwingsregels genereren een waarschuwing als de drempelwaarde één keer wordt overschreden. **Metrische meting** waarschuwingsregels een waarschuwing kunnen maken wanneer de drempel wordt overschreden een bepaald aantal keren gedurende een bepaald tijdsinterval.

## <a name="number-of-results-alert-rules"></a>Aantal resultaten waarschuwingsregels
**Aantal resultaten** waarschuwingsregels maken van één waarschuwing wanneer het aantal records dat wordt geretourneerd door de query de opgegeven drempelwaarde overschrijdt. Dit type van de waarschuwingsregel is ideaal voor het werken met gebeurtenissen, zoals Windows-gebeurtenislogboeken, Syslog WebApp-antwoord en aangepaste logboeken.  U wilt maken van een waarschuwing wanneer een bepaalde foutgebeurtenis wordt gemaakt, of wanneer er meerdere foutgebeurtenissen worden gemaakt binnen een bepaalde periode.

**Drempelwaarde**: de drempelwaarde voor een ** aantal resultaten waarschuwingsregels is groter dan of kleiner is dan een bepaalde waarde.  Als het aantal records dat wordt geretourneerd door de zoekopdracht logboek aan deze criteria voldoen, wordt een waarschuwing gemaakt.

Waarschuw op één gebeurtenis, het aantal resultaten ingesteld op groter dan 0 en Controleer op het exemplaar van een enkelvoudige gebeurtenis die is gemaakt sinds de laatste keer dat de query werd uitgevoerd. Sommige toepassingen kunnen zich aanmelden voor een incidentele fout die mag niet per se een melding kan genereren.  De toepassing kan bijvoorbeeld het proces opnieuw starten die de foutgebeurtenis gemaakt en slaagt de volgende keer.  U kan in dit geval niet wilt laten maken van de waarschuwing, tenzij meerdere gebeurtenissen worden gemaakt binnen een bepaalde periode.  

In sommige gevallen wilt u mogelijk een waarschuwing in het ontbreken van een gebeurtenis maken.  Bijvoorbeeld, een proces kan zich aanmelden reguliere gebeurtenissen om aan te geven dat deze correct werkt.  Als deze niet een van deze gebeurtenissen binnen een bepaalde periode, moet een waarschuwing worden gemaakt.  In dit geval stelt u de drempelwaarde op **minder dan 1**.

### <a name="example"></a>Voorbeeld
Neem bijvoorbeeld een scenario waarin u wilt weten wanneer uw web gebaseerde App biedt een antwoord aan gebruikers met code 500 (dat wil) interne serverfout. U zou een waarschuwingsregel maken met de volgende details:  
**Query:** aanvragen | waar resultCode == '500'<br>
**Tijdvenster:** 30 minuten<br>
**Waarschuwing frequentie:** vijf minuten<br>
**De waarde voor drempel:** geweldige dan 0<br>

Waarschuwing zou voert u de query om de 5 minuten met 30 minuten aan gegevens - gezocht naar een record waarop resultaatcode 500 is. Als een dergelijke record wordt gevonden, wordt deze geactiveerd de waarschuwing en de trigger de actie die is geconfigureerd.

## <a name="metric-measurement-alert-rules"></a>Waarschuwingsregels metrische meting

**Metrische meting** waarschuwingsregels maakt een waarschuwing voor elk object in een query met een waarde die een opgegeven drempelwaarde overschrijdt.  Ze hebben de volgende duidelijke verschillen van **aantal resultaten** waarschuwing regels.

**Statistische functie**: bepaalt de berekening die wordt uitgevoerd en mogelijk een numeriek veld kunnen worden geaggregeerd.  Bijvoorbeeld: **count()** retourneert het aantal records in de query **avg(CounterValue)** retourneert het gemiddelde van het veld tegenwaarde over het interval.

> [!NOTE]

> Statistische functie in de query moet met de naam/aangeroepen: AggregatedValue en geef een numerieke waarde. 


**Veld groeperen**: een record met een cumulatieve waarde voor elk exemplaar van dit veld wordt gemaakt en een waarschuwing worden gegenereerd voor elk.  Bijvoorbeeld, als u wilt dat er waarschuwingen gegenereerd voor elke computer, gebruikt u **door Computer**   

> [!NOTE]

> Metrische meting regels voor waarschuwingen die zijn gebaseerd op de Application Insights, kunt u het veld voor het groeperen van de gegevens opgeven. Gebruik hiervoor de **cumulatieve op** optie in de definitie van de regel.   

**Interval**: definieert de tijdsinterval waarover de gegevens worden samengevoegd.  Bijvoorbeeld, als u hebt opgegeven **vijf minuten**, een record voor elk exemplaar van het groepsveld om de 5 minuten gedurende de periode die is opgegeven voor de waarschuwing geaggregeerd zouden worden gemaakt.
> [!NOTE]
> Functie van de opslaglocatie moet worden gebruikt in query. Ook als ongelijke tijdsintervallen voor tijdvenster worden geproduceerd door het gebruik van de functie van de Bin - wordt waarschuwing in plaats daarvan gebruikt u bin_at functie om te controleren of dat er is een vast punt

**Drempelwaarde**: de drempelwaarde voor waarschuwingsregels metrische meting wordt gedefinieerd door een cumulatieve waarde en een aantal schendingen.  Als alle gegevenspunten in de zoekopdracht logboek deze waarde overschrijdt, heeft deze beschouwd als een schending.  Als het aantal schendingen in voor elk object in de resultaten van de opgegeven waarde overschrijdt, wordt een waarschuwing gemaakt voor dat object.

#### <a name="example"></a>Voorbeeld
Overweeg een scenario waarin het gewenste een waarschuwing als een computer processorgebruik van 90% driemaal gedurende 30 minuten overschreden.  U zou een waarschuwingsregel maken met de volgende details:  

**Query:** Perf | waar ObjectName == 'Processor' en CounterName == "% processortijd" | samenvatten AggregatedValue = avg(CounterValue) door bin (TimeGenerated, 5 m), Computer<br>
**Tijdvenster:** 30 minuten<br>
**Waarschuwing frequentie:** vijf minuten<br>
**Cumulatieve waarde:** geweldige dan 90<br>
**Waarschuwing activeren op basis van:** totaal kiezen oplossingen groter is dan 5<br>

De query maakt een gemiddelde waarde voor elke computer met een interval van 5 minuten.  Deze query worden uitgevoerd om de 5 minuten voor gegevens die worden verzameld in de vorige 30 minuten.  Voorbeeldgegevens worden hieronder weergegeven voor de drie computers.

![Voorbeeld-queryresultaten](../log-analytics/media/log-analytics-alerts/metrics-measurement-sample-graph.png)

In dit voorbeeld zou er afzonderlijke waarschuwingen voor srv02 en srv03 worden gemaakt omdat ze de drempelwaarde 90% 3 maal gedurende de periode geschonden.  Als de **Trigger waarschuwing op basis van:** zijn gewijzigd in **opeenvolgend** en vervolgens een waarschuwing alleen voor srv03 zou worden gemaakt omdat deze de drempelwaarde voor de drie opeenvolgende steekproeven geschonden.


## <a name="next-steps"></a>Volgende stappen
* Begrijpen [webhookacties voor logboek-waarschuwingen](monitor-alerts-unified-log-webhook.md)
* [Een overzicht van Azure-waarschuwingen (Preview)](monitoring-overview-unified-alerts.md)
* Meer informatie over [met behulp van Azure waarschuwingen (preview)](monitor-alerts-unified-usage.md)
* Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md)
* Meer informatie over [logboekanalyse](../log-analytics/log-analytics-overview.md).    
