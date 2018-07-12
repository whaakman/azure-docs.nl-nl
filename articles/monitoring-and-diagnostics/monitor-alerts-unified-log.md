---
title: Waarschuwingen in Azure Monitor
description: Trigger e-mailberichten, meldingen, aanroepen websites URL's (webhooks) of automatisering van wanneer de analytische query door u opgegeven voorwaarden wordt voldaan voor Azure-waarschuwingen.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: f36f05789424cfd3213525dd501333f852a0d9c2
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971717"
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Waarschuwingen in Azure Monitor - waarschuwingen 
Dit artikel vindt u details van waarschuwingen zijn een van de typen waarschuwingen die worden ondersteund in de nieuwe [Azure-waarschuwingen](monitoring-overview-unified-alerts.md) en gebruikers van Azure-platform voor streaminganalyse gebruiken als basis voor waarschuwingen... Raadpleeg voor meer informatie over metrische waarschuwingen met behulp van Logboeken, [bijna realtime metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md)


Waarschuwing bestaat uit regels voor zoeken in logboeken die zijn gemaakt voor [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events)


## <a name="log-search-alert-rule---definition-and-types"></a>Search waarschuwingsregel - definitie en typen

Log search regels worden gemaakt door Azure-waarschuwingen automatisch opgegeven logboeken-query's uitvoeren met regelmatige intervallen.  Als de resultaten van de logboekquery aan bepaalde criteria voldoen, wordt een waarschuwingsrecord gemaakt. De regel kan een of meer acties waarbij gebruik wordt vervolgens automatisch uitvoeren [actiegroepen](monitoring-action-groups.md). 

Log search regels zijn gedefinieerd door de volgende gegevens:
- **Meld u Query**.  De query die wordt uitgevoerd telkens als de waarschuwingsregel wordt geactiveerd.  De records die zijn geretourneerd door deze query worden gebruikt om te bepalen of een waarschuwing wordt gemaakt. *Azure Application Insights* -query kan ook bevatten [verschillende toepassingen aanroepen](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), mits er volgens de gebruiker toegangsrechten voor de externe toepassingen heeft. 

    > [!IMPORTANT]
    > Ondersteuning van [cross-application-query voor Application Insights](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) is beschikbaar als Preview: de functionaliteit beperkt gebruiken met 2 of meer apps en gebruikerservaring is onderhevig aan wijzigingen. Gebruik van [cross-werkruimte query](https://dev.loganalytics.io/oms/documentation/3-Using-the-API/CrossResourceQuery) en [query van meerdere bronnen voor Log Analytics](../log-analytics/log-analytics-cross-workspace-search.md) is momenteel **niet ondersteund** in Azure-waarschuwingen.

- **Periode**.  Hiermee geeft u het tijdsbereik voor de query. De query retourneert alleen de records die zijn gemaakt binnen dit tijdsbereik. Periode Hiermee beperkt u de gegevens opgehaald voor de query voor om misbruik te voorkomen en willekeurige opdracht van de tijd heeft (zoals geleden) gebruikt in logboekquery. <br>*Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd op 13:15 uur, wordt alleen de records die zijn gemaakt tussen 12:15 uur en 13:15 uur geretourneerd logboekquery uit te voeren. Als de logboekquery gebruikt tijd opdracht zoals geleden nu (7d), de logboekquery wordt uitgevoerd alleen voor gegevens die tussen 12:15 uur en 1:15 PM - als gegevens bestaan alleen de afgelopen 60 minuten. En niet voor zeven dagen aan gegevens die zijn opgegeven in de query voor.*
- **Frequentie**.  Hiermee geeft u op hoe vaak de query moet worden uitgevoerd. Een waarde tussen 5 minuten en 24 uur kan zijn. Moet gelijk zijn aan of kleiner is dan de periode.  Als de waarde groter dan de periode is, riskeert u records worden overgeslagen.<br>*Neem bijvoorbeeld een periode van 30 minuten en een frequentie van 60 minuten.  Als de query wordt uitgevoerd om 1:00, wordt er records tussen 12:30 en 13:00 uur.  De volgende keer dat de query wordt uitgevoerd, is 2:00 wanneer hij records tussen 1:30 en 2:00 terugkeert.  Alle records gemaakt tussen 1:00 uur en 1:30 zouden nooit worden geëvalueerd.*
- **Drempelwaarde**.  De resultaten van zoeken in Logboeken worden geëvalueerd om te bepalen of een waarschuwing moet worden gemaakt.  De drempelwaarde is verschillend voor de verschillende typen waarschuwingsregels zoeken.

Log search regels worden voor [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events), zijn twee soorten. Elk van deze typen is beschreven in de volgende secties.

- **[Aantal resultaten](#number-of-results-alert-rules)**. Één waarschuwing gemaakt wanneer het aantal records geretourneerd door de zoeken in Logboeken groter zijn dan een opgegeven getal.
- **[Meting van metrische gegevens](#metric-measurement-alert-rules)**.  Waarschuwing gemaakt voor elk object in de resultaten van zoeken in Logboeken met waarden die groter zijn dan de opgegeven drempelwaarde.

De verschillen tussen waarschuwingsregel typen zijn als volgt.

- *Aantal resultaten* waarschuwingsregels maakt altijd één waarschuwing, even *meting van metrische gegevens* waarschuwingsregel maakt een waarschuwing voor elk object dat de drempelwaarde overschrijdt.
- *Aantal resultaten* waarschuwingsregels genereren een waarschuwing als de drempelwaarde voor één keer is overschreden. *Meting van metrische gegevens* waarschuwingsregels kunnen genereren een waarschuwing als de drempelwaarde wordt overschreden een bepaald aantal keren gedurende een bepaald tijdsinterval.

### <a name="number-of-results-alert-rules"></a>Aantal resultaten waarschuwingsregels
**Aantal resultaten** waarschuwingsregels één waarschuwing maken wanneer het aantal records dat wordt geretourneerd door de zoekopdracht langer zijn dan de opgegeven drempelwaarde. Dit type van de waarschuwingsregel is ideaal voor het werken met gebeurtenissen, zoals Windows-Logboeken, Syslog antwoord van de Web-App en aangepaste logboeken.  U wilt maken van een waarschuwing wanneer een bepaalde fout-gebeurtenis wordt gemaakt, of wanneer er meerdere gebeurtenissen op foutniveau worden gemaakt binnen een bepaalde periode.

**Drempelwaarde**: de drempel voor een aantal resultaten waarschuwingsregels is groter dan of kleiner is dan een bepaalde waarde.  Als het aantal records dat wordt geretourneerd door de zoeken in Logboeken aan deze criteria voldoen, wordt een waarschuwing gemaakt.

Om u te waarschuwen wanneer één gebeurtenis, het aantal resultaten ingesteld op groter dan 0 en controleren op het exemplaar van een gebeurtenis die is gemaakt sinds de laatste keer dat de query is uitgevoerd. Sommige toepassingen kunnen zich aanmelden voor een incidentele fout mag niet per se een waarschuwing geven.  De toepassing kan bijvoorbeeld proberen het proces dat de foutgebeurtenis is gemaakt en slaagt de volgende keer.  In dit geval kunt u niet te maken van de waarschuwing, tenzij er meerdere gebeurtenissen worden gemaakt binnen een bepaalde periode.  

In sommige gevallen kunt u een waarschuwing maken in de afwezigheid van een gebeurtenis.  Bijvoorbeeld, een proces kan zich aanmelden reguliere gebeurtenissen om aan te geven dat deze correct werkt.  Als dit niet een van deze gebeurtenissen zich binnen een bepaalde periode, moet klikt u vervolgens een waarschuwing worden gemaakt.  In dit geval stelt u de drempelwaarde op **minder dan 1**.

#### <a name="example"></a>Voorbeeld
U hebt een scenario waarin u wilt weten wanneer uw web-apps biedt een antwoord naar gebruikers met code 500 (dat wil zeggen) interne serverfout. U zou een waarschuwingsregel maken met de volgende details:  
- **Query:** aanvragen | waarbij resultCode == "500"<br>
- **Periode:** 30 minuten<br>
- **Waarschuwingsfrequentie:** vijf minuten<br>
- **De waarde voor drempel:** groter dan 0<br>

Vervolgens waarschuwing zou de query uitvoeren om de 5 minuten, 30 minuten van gegevens - om te controleren of voor elke record waarop resultaatcode is 500. Als een dergelijke record wordt gevonden, wordt de waarschuwing wordt geactiveerd en activeert de actie die is geconfigureerd.

### <a name="metric-measurement-alert-rules"></a>Waarschuwingsregels meting van metrische gegevens

- **Meting van metrische gegevens** waarschuwingsregels een waarschuwing maken voor elk object in een query met een waarde die groter is dan een opgegeven drempelwaarde.  Ze hebben de volgende toch duidelijke verschillen van **aantal resultaten** waarschuwingsregels.
- **Statistische functie**: Hiermee bepaalt u de berekening die wordt uitgevoerd en mogelijk een numeriek veld om samen te voegen.  Bijvoorbeeld, **count()** retourneert het aantal records in de query **avg(CounterValue)** retourneert het gemiddelde van het veld CounterValue over het interval. Statistische functie in de query moet worden met de naam/met de naam: AggregatedValue en geef een numerieke waarde. 
- **Veld groep**: een record met een geaggregeerde waarde voor elk exemplaar van dit veld is gemaakt en een waarschuwing worden gegenereerd voor elke.  Bijvoorbeeld, als u wilt dat er waarschuwingen gegenereerd voor elke computer, gebruikt u **door Computer** 

    > [!NOTE]
    > Voor de meting van metrische gegevens regels voor waarschuwingen die zijn gebaseerd op de Application Insights, kunt u het veld voor het groeperen van de gegevens. U doet dit door gebruik van de **cumulatieve op** optie in het regeldefinitie van de.   
    
- **Interval**: definieert het tijdsinterval op waarover de gegevens worden samengevoegd.  Bijvoorbeeld, als u hebt opgegeven **vijf minuten**, een record voor elk exemplaar van het groepsveld samengevoegd met intervallen van 5 minuten gedurende de periode die is opgegeven voor de waarschuwing moet worden gemaakt.

    > [!NOTE]
    > BIn-functie moet worden gebruikt in query opgeven voor interval. Als bin() tot ongelijk tijdsintervallen leiden kan - wordt waarschuwing automatisch geconverteerd bin-opdracht naar bin_at opdracht met de juiste tijd tijdens runtime, om ervoor te zorgen resultaten met een vaste punt
    
- **Drempelwaarde**: de drempelwaarde voor de meting van metrische gegevens waarschuwingsregels is gedefinieerd door een cumulatieve waarde en een aantal schendingen.  Als een gegevenspunt in de logboekzoekopdracht deze waarde overschrijdt, heeft deze beschouwd als een schending.  Als het aantal schendingen in voor een object in de resultaten de opgegeven waarde overschrijdt, wordt een waarschuwing gemaakt voor dat object.

#### <a name="example"></a>Voorbeeld
U hebt een scenario waarin u een waarschuwing wilt als een computer processorgebruik van 90% drie keer meer dan 30 minuten overschreden.  U zou een waarschuwingsregel maken met de volgende details:  

- **Query:** Perf | waarbij ObjectName == 'Processor' en CounterName == "% processortijd" | summarize AggregatedValue = avg(CounterValue) door bin (TimeGenerated, 5 min.), Computer<br>
- **Periode:** 30 minuten<br>
- **Waarschuwingsfrequentie:** vijf minuten<br>
- **Statistische waarde:** groter dan 90<br>
- **Waarschuwing activeren op basis van:** totaal kiezen oplossingen groter is dan 2<br>

De query maakt een gemiddelde waarde voor elke computer met 5 minuten durende intervallen.  Deze query worden uitgevoerd om de 5 minuten voor gegevens die worden verzameld in de vorige 30 minuten.  Hieronder ziet u voorbeeldgegevens voor de drie computers.

![De resultaten van de voorbeeld-query](./media/monitor-alerts-unified/metrics-measurement-sample-graph.png)

In dit voorbeeld zou afzonderlijke waarschuwingen voor srv02 en srv03 worden gemaakt omdat ze de drempelwaarde 90% 3 keer gedurende de periode geschonden.  Als de **waarschuwing activeren op basis van:** zijn gewijzigd in **opeenvolgend** en vervolgens een waarschuwing alleen voor srv03 zouden worden gemaakt omdat deze de drempelwaarde voor drie opeenvolgende steekproeven geschonden.


## <a name="log-search-alert-rule---creation-and-modification"></a>Search waarschuwingsregel - maken en wijzigen

Waarschuwing, evenals de waaruit het is opgebouwd logboekwaarschuwingsregel van de zoekopdracht kan worden weergegeven, gemaakt of gewijzigd van:
- Azure Portal
- REST-API's (zoals via PowerShell)
- Azure Resource Manager-sjablonen

### <a name="azure-portal"></a>Azure Portal
Sinds de introductie van de [nieuwe Azure-waarschuwingen](monitoring-overview-unified-alerts.md), nu gebruikers alle typen waarschuwingen in Azure portal vanaf één locatie en vergelijkbare stappen beheren kunnen. Meer informatie over [met behulp van de nieuwe Azure-waarschuwingen](monitor-alerts-unified-usage.md).

Ook gebruikers hun query's in Analytics platform van uw keuze in Azure kunnen verbeteren en vervolgens *importeren voor gebruik in waarschuwingen door op te slaan van de query*. De stappen te volgen:
- *Voor Application Insights*: Ga naar de analyseportal om query's en de resultaten te valideren. Sla met een unieke naam in *gedeelde query's*.
- *Voor Log Analytics*: Ga om te zoeken in Logboeken, query's en de resultaten te valideren. Gebruik vervolgens opslaan met de unieke naam in elke categorie.

Klik wanneer [het maken van een waarschuwing in waarschuwingen ](monitor-alerts-unified-usage.md), ziet u de opgeslagen query die is vermeld als signaaltype **logboek (opgeslagen Query)**, zoals wordt geïllustreerd in onderstaande voorbeeld: ![opgeslagen Query die worden geïmporteerd op waarschuwingen](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Met behulp van **logboek (opgeslagen Query)** resulteert in een import op waarschuwingen. Alle wijzigingen die zijn uitgevoerd na in Analytics wordt daarom niet worden bij reflectieve in waarschuwingsregels zoeken en vice versa.

### <a name="rest-apis"></a>REST-API’s
API's voorzien in logboek waarschuwingen RESTful zijn en zijn toegankelijk via de Azure Resource Manager REST API. Daarom zijn toegankelijk via PowerShell, evenals andere opties voor de API's.

Voor meer informatie en voorbeelden over het gebruik van REST-API, kunt u verwijzen naar:
- [Meld u Analytics-waarschuwing REST-API](../log-analytics/log-analytics-api-alerts.md) : als u wilt maken en beheren van waarschuwingsregels zoeken voor Azure Log Analytics
- [Azure Monitor gepland regels REST API-Query](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) : als u wilt maken en beheren van waarschuwingsregels zoeken voor Azure Application Insights

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
Gebruikers kunnen ook gebruiken voor de flexibiliteit die wordt geleverd door [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) maken en bijwerken van bronnen - voor het maken of bijwerken van waarschuwingen.

Voor meer informatie en voorbeelden over het gebruik van Resource Manager-sjablonen, kunt u verwijzen naar:
- [Opgeslagen zoekopdrachten en waarschuwingen](monitor-alerts-unified-log-template.md#managing-log-alert-on-log-analytics) voor waarschuwingen op basis van Azure Log Analytics
- [Geplande queryregel](monitor-alerts-unified-log-template.md#managing-log-alert-on-application-insights) voor waarschuwingen op basis van Azure Application Insights
 

## <a name="next-steps"></a>Volgende stappen
* Inzicht in [waarschuwingen voor activiteitenlogboeken in Azure](monitor-alerts-unified-log-webhook.md).
* Meer informatie over de nieuwe [Azure-waarschuwingen](monitoring-overview-unified-alerts.md).
* Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md).
* Meer informatie over [Log Analytics](../log-analytics/log-analytics-overview.md).    
