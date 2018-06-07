---
title: Meld u waarschuwingen in de Azure-Monitor - waarschuwingen | Microsoft Docs
description: Trigger e-mailberichten, meldingen, worden URL's van websites (webhooks) of automation aanroepen wanneer de analytische door u opgegeven queryvoorwaarden wordt voldaan voor Azure-waarschuwingen.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 175e512d0bdaa84d5251f4bbdb09aed3aed436f9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638718"
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Logboek waarschuwingen in Azure Monitor - waarschuwingen 
Dit artikel vindt u details van logboek-waarschuwingen zijn een van de typen waarschuwingen die worden ondersteund in de nieuwe [Azure waarschuwingen](monitoring-overview-unified-alerts.md) en toestaan dat gebruikers van Azure analytics platform gebruiken als basis voor het waarschuwen... Raadpleeg voor meer informatie van metriek waarschuwingen met Logboeken [bijna realtime metriek waarschuwingen](monitoring-near-real-time-metric-alerts.md)


Logboek waarschuwing bestaat uit regels van het logboek zoeken gemaakt voor [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events)


## <a name="log-search-alert-rule---definition-and-types"></a>Logboek zoeken waarschuwingsregel - definitie en typen

Logboek zoeken regels worden gemaakt met Azure-waarschuwingen automatisch opgegeven logboek query's uitvoeren met regelmatige tussenpozen.  Als de resultaten van de query logboek aan bepaalde criteria voldoen, wordt een waarschuwing record gemaakt. Een of meer acties met kan vervolgens automatisch uitgevoerd door de regel [actiegroepen](monitoring-action-groups.md). 

Logboek zoeken regels zijn gedefinieerd door de volgende details:
- **Meld u Query**.  De query die wordt uitgevoerd telkens als de waarschuwingsregel wordt geactiveerd.  De records geretourneerd door deze query worden gebruikt om te bepalen of een waarschuwing wordt gemaakt. *Azure Application Insights* query kan ook betekenen dat [tussen verschillende toepassingen aanroepen](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), mits de gebruiker heeft de toegangsrechten voor de externe toepassingen. 

    > [!IMPORTANT]
    > Relatietype van [cross-toepassing-query voor Application Insights](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) is in de preview - de functionaliteit beperkt gebruik met 2 of meer apps en gebruikerservaring kan worden gewijzigd. Informatie over het gebruik van [cross-werkruimte query](https://dev.loganalytics.io/oms/documentation/3-Using-the-API/CrossResourceQuery) en [cross-resource-query voor logboekanalyse](../log-analytics/log-analytics-cross-workspace-search.md) is momenteel **niet ondersteund** in waarschuwingen van Azure.

- **Periode**.  Hiermee geeft u het tijdsbereik voor de query. De query retourneert alleen de records die zijn gemaakt binnen dit tijdsbereik. Periode Hiermee beperkt u de gegevens die zijn opgehaald voor logboek-query om te voorkomen dat misbruik en gevolg willekeurige opdracht van de tijd (zoals geleden) in logboek query gebruikt. <br>*Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd om 1:15 uur, wordt alleen records gemaakt tussen 12:15 uur en 1:15 uur geretourneerd logboek query uit te voeren. Nu als de query logboek gebruikt, zoals opdracht geleden tijd (7d), de logboek-query worden uitgevoerd alleen voor gegevens die tussen 12:15 uur en 1:15 PM - alsof er voor alleen de afgelopen 60 minuten gegevens. En niet voor de zeven dagen aan gegevens, zoals opgegeven in het logboek-query.*
- **Frequentie**.  Hiermee geeft u op hoe vaak de query moet worden uitgevoerd. Is een waarde tussen 5 minuten en 24 uur. Moet gelijk zijn aan of kleiner is dan de periode.  Als de waarde groter dan de periode is, risico u records wordt overgeslagen.<br>*Neem bijvoorbeeld een periode van 30 minuten en een frequentie van 60 minuten.  Als de query wordt uitgevoerd om 1:00 uur, wordt er records tussen 12:30 en 13:00 uur.  De volgende keer dat de query werd uitgevoerd is 2:00 wanneer er records tussen 1:30 en 2:00 zou geretourneerd.  Alle records gemaakt tussen de 1:00 en 1:30 zou nooit worden geëvalueerd.*
- **Drempelwaarde**.  De resultaten van de zoekopdracht logboek worden geëvalueerd om te bepalen of een waarschuwing moet worden gemaakt.  De drempelwaarde is verschillend voor de verschillende soorten logboek zoeken met regels voor waarschuwingen.

Logboek zoeken regels worden voor [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events), zijn twee soorten. Elk van deze typen is beschreven in de volgende secties.

- **[Aantal resultaten](#number-of-results-alert-rules)**. Één waarschuwing gemaakt wanneer het aantal records geretourneerd door de zoekopdracht logboek groter zijn dan een opgegeven getal.
- **[Metrische meting](#metric-measurement-alert-rules)**.  De waarschuwing is gemaakt voor elk object in de resultaten van de zoekopdracht logboek met waarden die groter zijn dan de opgegeven drempelwaarde.

De verschillen tussen waarschuwingsregel typen zijn als volgt.

- *Aantal resultaten* waarschuwingsregels maakt altijd één waarschuwing, even *metrische meting* waarschuwingsregel maakt een waarschuwing voor elk object dat de drempelwaarde overschrijdt.
- *Aantal resultaten* waarschuwingsregels genereren een waarschuwing als de drempelwaarde één keer wordt overschreden. *Metrische meting* waarschuwingsregels een waarschuwing kunnen maken wanneer de drempel wordt overschreden een bepaald aantal keren gedurende een bepaald tijdsinterval.

### <a name="number-of-results-alert-rules"></a>Aantal resultaten waarschuwingsregels
**Aantal resultaten** waarschuwingsregels maken van één waarschuwing wanneer het aantal records dat wordt geretourneerd door de query de opgegeven drempelwaarde overschrijdt. Dit type van de waarschuwingsregel is ideaal voor het werken met gebeurtenissen, zoals Windows-gebeurtenislogboeken, Syslog WebApp-antwoord en aangepaste logboeken.  U wilt maken van een waarschuwing wanneer een bepaalde foutgebeurtenis wordt gemaakt, of wanneer er meerdere foutgebeurtenissen worden gemaakt binnen een bepaalde periode.

**Drempelwaarde**: de drempelwaarde voor een aantal resultaten waarschuwingsregels is groter dan of kleiner is dan een bepaalde waarde.  Als het aantal records dat wordt geretourneerd door de zoekopdracht logboek aan deze criteria voldoen, wordt een waarschuwing gemaakt.

Waarschuw op één gebeurtenis, het aantal resultaten ingesteld op groter dan 0 en Controleer op het exemplaar van een enkelvoudige gebeurtenis die is gemaakt sinds de laatste keer dat de query werd uitgevoerd. Sommige toepassingen kunnen zich aanmelden voor een incidentele fout die mag niet per se een melding kan genereren.  De toepassing kan bijvoorbeeld het proces opnieuw starten die de foutgebeurtenis gemaakt en slaagt de volgende keer.  In dit geval u mogelijk niet wilt maken van de waarschuwing als er meerdere gebeurtenissen worden gemaakt binnen een bepaalde periode.  

In sommige gevallen wilt u mogelijk een waarschuwing in het ontbreken van een gebeurtenis maken.  Bijvoorbeeld, een proces kan zich aanmelden reguliere gebeurtenissen om aan te geven dat deze correct werkt.  Als deze niet een van deze gebeurtenissen binnen een bepaalde periode, moet een waarschuwing worden gemaakt.  In dit geval stelt u de drempelwaarde op **minder dan 1**.

#### <a name="example"></a>Voorbeeld
Neem bijvoorbeeld een scenario waarin u wilt weten wanneer uw web gebaseerde App biedt een antwoord aan gebruikers met code 500 (dat wil) interne serverfout. U zou een waarschuwingsregel maken met de volgende details:  
- **Query:** aanvragen | waar resultCode == '500'<br>
- **Periode:** 30 minuten<br>
- **Waarschuwing frequentie:** vijf minuten<br>
- **De waarde voor drempel:** geweldige dan 0<br>

Waarschuwing zou voert u de query om de 5 minuten met 30 minuten aan gegevens - gezocht naar een record waarop resultaatcode 500 is. Als een dergelijke record wordt gevonden, wordt de waarschuwing wordt geactiveerd en activeert de actie die is geconfigureerd.

### <a name="metric-measurement-alert-rules"></a>Waarschuwingsregels metrische meting

- **Metrische meting** waarschuwingsregels maakt een waarschuwing voor elk object in een query met een waarde die een opgegeven drempelwaarde overschrijdt.  Ze hebben de volgende duidelijke verschillen van **aantal resultaten** waarschuwing regels.
- **Statistische functie**: bepaalt de berekening die wordt uitgevoerd en mogelijk een numeriek veld kunnen worden geaggregeerd.  Bijvoorbeeld: **count()** retourneert het aantal records in de query **avg(CounterValue)** retourneert het gemiddelde van het veld tegenwaarde over het interval. Statistische functie in de query moet met de naam/aangeroepen: AggregatedValue en geef een numerieke waarde. 
- **Veld groeperen**: een record met een cumulatieve waarde voor elk exemplaar van dit veld wordt gemaakt en een waarschuwing worden gegenereerd voor elk.  Bijvoorbeeld, als u wilt dat er waarschuwingen gegenereerd voor elke computer, gebruikt u **door Computer** 

    > [!NOTE]
    > Metrische meting regels voor waarschuwingen die zijn gebaseerd op de Application Insights, kunt u het veld voor het groeperen van de gegevens opgeven. Gebruik hiervoor de **cumulatieve op** optie in de definitie van de regel.   
    
- **Interval**: definieert de tijdsinterval waarover de gegevens worden samengevoegd.  Bijvoorbeeld, als u hebt opgegeven **vijf minuten**, een record voor elk exemplaar van het groepsveld geaggregeerd met een interval van 5 minuten gedurende de periode die is opgegeven voor de waarschuwing moet worden gemaakt.

    > [!NOTE]
    > Functie van de opslaglocatie moet worden gebruikt in query interval opgeven. Aangezien bin() leiden ongelijke tijdsintervallen tot kan - wordt waarschuwing automatisch geconverteerd bin opdracht naar bin_at opdracht met de juiste tijd tijdens runtime, om ervoor te zorgen resultaten met een vaste point
    
- **Drempelwaarde**: de drempelwaarde voor waarschuwingsregels metrische meting wordt gedefinieerd door een cumulatieve waarde en een aantal schendingen.  Als alle gegevenspunten in de zoekopdracht logboek deze waarde overschrijdt, heeft deze beschouwd als een schending.  Als het aantal schendingen in voor elk object in de resultaten van de opgegeven waarde overschrijdt, wordt een waarschuwing gemaakt voor dat object.

#### <a name="example"></a>Voorbeeld
Overweeg een scenario waarin het gewenste een waarschuwing als een computer processorgebruik van 90% driemaal gedurende 30 minuten overschreden.  U zou een waarschuwingsregel maken met de volgende details:  

- **Query:** Perf | waar ObjectName == 'Processor' en CounterName == "% processortijd" | samenvatten AggregatedValue = avg(CounterValue) door bin (TimeGenerated, 5 m), Computer<br>
- **Periode:** 30 minuten<br>
- **Waarschuwing frequentie:** vijf minuten<br>
- **Cumulatieve waarde:** geweldige dan 90<br>
- **Waarschuwing activeren op basis van:** totaal kiezen oplossingen groter is dan 2<br>

De query maakt een gemiddelde waarde voor elke computer met een interval van 5 minuten.  Deze query worden uitgevoerd om de 5 minuten voor gegevens die worden verzameld in de vorige 30 minuten.  Voorbeeldgegevens worden hieronder weergegeven voor de drie computers.

![Voorbeeld-queryresultaten](./media/monitor-alerts-unified/metrics-measurement-sample-graph.png)

In dit voorbeeld zou er afzonderlijke waarschuwingen voor srv02 en srv03 worden gemaakt omdat ze de drempelwaarde 90% 3 maal gedurende de periode geschonden.  Als de **Trigger waarschuwing op basis van:** zijn gewijzigd in **opeenvolgend** en vervolgens een waarschuwing alleen voor srv03 zou worden gemaakt omdat deze de drempelwaarde voor de drie opeenvolgende steekproeven geschonden.


## <a name="log-search-alert-rule---creation-and-modification"></a>Logboek zoeken waarschuwingsregel - maken en wijzigen

Logboek waarschuwing, evenals de waaruit het is opgebouwd logboek zoeken waarschuwingsregel kan worden bekeken, gemaakt of gewijzigd van:
- Azure Portal
- REST API's (inclusief via PowerShell)
- Azure Resource Manager-sjablonen

### <a name="azure-portal"></a>Azure Portal
Sinds de introductie van de [nieuwe waarschuwingen van Azure](monitoring-overview-unified-alerts.md), nu gebruikers alle typen waarschuwingen in Azure portal vanaf één locatie en vergelijkbare stappen beheren kunnen. Meer informatie over [met behulp van de nieuwe Azure-waarschuwingen](monitor-alerts-unified-usage.md).

Ook gebruikers hun query's in Analytics platform van keuze in Azure kunnen verbeteren en vervolgens *importeren voor gebruik in waarschuwingen door op te slaan van de query*. Volgende stappen uitvoeren:
- *Voor Application Insights*: Ga naar Analytics-portal, query's en de resultaten te valideren. Sla met unieke naam in *gedeelde query's*.
- *Voor logboekanalyse*: Ga naar logboek zoeken, query's en de resultaten te valideren. Gebruik vervolgens opslaan met de unieke naam in elke categorie.

Klik wanneer [maken van een waarschuwing in een logboek in waarschuwingen ](monitor-alerts-unified-usage.md), ziet u de opgeslagen query vermeld als signaaltype **logboek (opgeslagen Query)**; zoals geïllustreerd in onderstaand voorbeeld: ![opgeslagen Query die zijn geïmporteerd op waarschuwingen](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Met behulp van **logboek (opgeslagen Query)** resulteert in een import op waarschuwingen. Eventuele wijzigingen die zijn uitgevoerd na in Analytics wordt daarom niet worden reflecteert in logboek zoeken waarschuwingsregels en vice versa.

### <a name="rest-apis"></a>REST-API’s
API's zijn voorzien van logboek waarschuwingen RESTful zijn en toegankelijk zijn via de REST-API van Azure Resource Manager. Daarom kan worden benaderd via PowerShell, evenals andere opties voor het benutten van de API's.

Voor meer informatie en voorbeelden van het gebruik van de REST-API, kunt u verwijzen naar:
- [Meld u Analytics waarschuwing REST-API](../log-analytics/log-analytics-api-alerts.md) : als u wilt maken en beheren van logboek zoeken waarschuwingsregels voor Azure Log Analytics
- [Azure Monitor gepland Query regels REST-API](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) : als u wilt maken en beheren van logboek zoeken waarschuwingsregels voor Azure Application Insights

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
Gebruikers kunnen ook gebruiken voor de flexibiliteit door [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) maken en bijwerken van resources - maken of bijwerken van logboek-waarschuwingen.

Voor meer informatie en voorbeelden van het gebruik van Resource Manager-sjablonen, kunt u verwijzen naar:
- [Opgeslagen zoekopdracht en waarschuwingen management](monitor-alerts-unified-log-template.md#managing-log-alert-on-log-analytics) voor logboek-waarschuwingen op basis van de Azure Log Analytics
- [Geplande queryregel](monitor-alerts-unified-log-template.md#managing-log-alert-on-application-insights) voor logboek-waarschuwingen op basis van Azure Application Insights
 

## <a name="next-steps"></a>Volgende stappen
* Begrijpen [waarschuwingen aanmelden voor Azure](monitor-alerts-unified-log-webhook.md).
* Meer informatie over de nieuwe [Azure waarschuwingen](monitoring-overview-unified-alerts.md).
* Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md).
* Meer informatie over [logboekanalyse](../log-analytics/log-analytics-overview.md).    
