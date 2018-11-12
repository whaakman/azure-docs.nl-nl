---
title: Waarschuwingen voor het oplossen van problemen in Azure Monitor
description: Veelvoorkomende problemen, fouten en resolutie voor logboek waarschuwingsregels in Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 5572c80879584e7f6df650263ae455a134ee4088
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283594"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Waarschuwingen voor het oplossen van problemen in Azure Monitor  

## <a name="overview"></a>Overzicht
Dit artikel laat dat u problemen die regelmatig voorkomen bij het instellen van waarschuwingen in Azure monitor verwerken. En oplossing voor antwoorden op veelgestelde vragen met betrekking tot de functionaliteit of de configuratie van waarschuwingen bieden. De term **Logboekwaarschuwingen** om te beschrijven van waarschuwingen met waar signaal aangepaste query op basis van [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-analytics.md). Meer informatie over functies, -terminologie en typen van [Logboekwaarschuwingen - overzicht](monitor-alerts-unified-log.md).

> [!NOTE]
> In dit artikel niet na te gaan wanneer de waarschuwingsregel wordt weergegeven als geactiveerd in Azure portal en een melding via de bijbehorende actie groep(en). Voor dergelijke gevallen raadpleegt u de details in het artikel op [actiegroepen](monitoring-action-groups.md).


## <a name="log-alert-didnt-fire"></a>Waarschuwing is niet gestart

Gedetailleerde volgende worden enkele veelvoorkomende redenen waarom een geconfigureerde [waarschuwingsregel in Azure Monitor](alert-log.md) niet ophalen geactiveerd wanneer deze wordt bekeken [Azure-waarschuwingen](monitoring-alerts-managing-alert-states.md), wanneer u verwacht dat het worden geactiveerd. 

### <a name="data-ingestion-time-for-logs"></a>Gegevens opnemen tijd voor logboeken
Meld u waarschuwingen werkt periodiek uitvoeren klantgeleverde query op basis van [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-analytics.md). Beide worden aangestuurd door de kracht van Analytics, die grote hoeveelheden gegevens worden verwerkt en biedt functionaliteit op hetzelfde. Als de service Log Analytics omvat het verwerken van vele terabytes aan gegevens van duizenden klanten uit verschillende bronnen over de hele wereld - de service deze vatbaar voor vertraging is. Zie voor meer informatie, [gegevensopname tijd in Log Analytics](../log-analytics/log-analytics-data-ingestion-time.md).

Te strijden tegen de vertraging voor het opnemen van gegevens die zich in Log Analytics of Application Insights-logboekbestanden voordoen kan; Waarschuwing voor moet wachten en probeert om opnieuw na enige tijd wanneer er gegevens nog niet is opgenomen voor de waarschuwingen periode gevonden. Waarschuwingen voor logboeken is er een exponentieel toenemende wacht tijd ingesteld, zodat deze Zorg ervoor dat we wachttijd die nodig zijn voor gegevens die door Log Analytics worden opgenomen. Dus als de logboeken die zijn opgevraagd met de waarschuwingsregel worden beïnvloed door opname vertragingen, wordt klikt u vervolgens waarschuwing geactiveerd nadat de gegevens beschikbaar in Log Analytics na opname en na de exponentiële tijdsverschil zijn vanwege log alert-service opnieuw wordt geprobeerd meerdere keren in de tussentijd .

### <a name="incorrect-time-period-configured"></a>Periode onjuist geconfigureerd
Zoals beschreven in het artikel op [terminologie voor logboekwaarschuwingen](monitor-alerts-unified-log.md#log-search-alert-rule---definition-and-types), periode vermeld in de configuratie van Hiermee geeft u het tijdsbereik voor de query. De query retourneert alleen de records die zijn gemaakt binnen dit tijdsbereik. Periode Hiermee beperkt u de gegevens opgehaald voor de query voor om misbruik te voorkomen en willekeurige opdracht van de tijd heeft (zoals geleden) gebruikt in logboekquery. 
*Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd op 13:15 uur, wordt alleen de records die zijn gemaakt tussen 12:15 uur en 13:15 uur geretourneerd logboekquery uit te voeren. Als de logboekquery gebruikt tijd opdracht zoals geleden (1d), de logboekquery wordt uitgevoerd alleen voor gegevens die tussen 12:15 uur en 1:15 PM - als gegevens bestaan alleen de afgelopen 60 minuten. En niet voor zeven dagen aan gegevens die zijn opgegeven in de query voor.*

Op basis van de querylogica van uw, Controleer als geschikte periode in de configuratie is opgegeven. Eerder aangegeven, bijvoorbeeld als het logboek query gebruikt geleden (1d), zoals wordt weergegeven met groene markering - en vervolgens de periode moet worden ingesteld op 24 uur of 1440 minuten (zoals aangegeven in het rood), om te controleren of de opgegeven query wordt uitgevoerd correct als beoogd.
    ![Periode](./media/monitor-alerts-unified/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Optie is ingesteld waarschuwingen onderdrukken
Zoals beschreven in stap 8 van het artikel op [een waarschuwingsregel maken in Azure portal](alert-log.md#managing-log-alerts-from-the-azure-portal), waarschuwing biedt een optie Automatische onderdrukking van de waarschuwingsregel configureren en te voorkomen dat meldingen/trigger voor de aangegeven hoeveelheid tijd. Onderdrukken waarschuwingen optie leidt ertoe dat waarschuwing uit te voeren tijdens actiegroep niet activeren voor de tijd die is opgegeven **waarschuwingen onderdrukken** optie en kan daarom gebruiker vinden er een waarschuwing is niet geactiveerd terwijl in werkelijkheid is onderdrukt zoals geconfigureerd .
    ![Waarschuwingen onderdrukken](./media/monitor-alerts-unified/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>De waarschuwingsregel meting van metrische gegevens is onjuist
Type van de meting van metrische gegevens van de waarschuwingsregel is subtype van waarschuwingen, die hebben speciale functies, maar die op zijn beurt de veiligheidsmaatregelen voor beperking van de waarschuwing query-syntaxis. Waarschuwing voor een meting van metrische gegevens regel wordt vereist dat de uitvoer van Waarschuwingsquery voor een metrische tijdreeks - een tabel met afzonderlijke gelijke grootte perioden, samen met de overeenkomende waarden van AggregatedValue berekend. Bovendien kunnen gebruikers kiezen dat in de tabel meer variabelen naast AggregatedValue als Computer, knooppunt, enz. met behulp van welke gegevens in de tabel kan worden gesorteerd.

Stel bijvoorbeeld dat logboekwaarschuwingen meting van metrische gegevens is geconfigureerd als:
- query is: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- periode van 6 uur
- drempelwaarde van 50
- waarschuwingslogica van drie achtereenvolgende schendingen
- Cumulatieve bij gekozen als $table

Nu samenvatten omdat in de opdracht, we hebben gebruikt... door en twee variabelen: timestamp & $table; Waarschuwing service, kiest u $table bij 'Cumulatieve na' - in feite de resultaattabel per veld sorteren: $table - zoals hieronder wordt weergegeven en de meerdere AggregatedValue voor elke tabel kijken Typ (zoals availabilityResults) om te zien als er achtereenvolgende schendingen van 3 of meer is.

   ![Metrische meting queryuitvoering met meerdere waarden](./media/monitor-alerts-unified/LogMMQuery.png)

Als ' totale "$table is: de gegevens gesorteerd op kolom $table (zoals in rood); we groep en zoek naar "Cumulatieve bij" veld typen (dat wil zeggen) $table – bijvoorbeeld: waarden voor de availabilityResults worden beschouwd als een diagram/entiteit (zoals gemarkeerd in oranje). In deze entiteit/waarde plot: waarschuwing service controleert op drie achtereenvolgende schendingen optreden (zoals weergegeven in groen) voor die waarschuwing wordt ophalen geactiveerd voor de tabelwaarde 'availabilityResults'. Op dezelfde manier als voor een andere waarde voor $table als drie achtereenvolgende schendingen worden gezien - wordt nog een waarschuwing geactiveerd voor hetzelfde; met waarschuwing service automatisch de waarden in één diagram/entiteit (zoals in oranje) op tijd te sorteren.

Stel meting van metrische gegevens waarschuwingsregel is gewijzigd en de query is `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)` met de rest van de configuratie van de resterende hetzelfde als voordat waarschuwingslogica voor drie opeenvolgende schendingen. De optie 'Samenvoegen op' in dit geval zal worden standaard: timestamp. Omdat er slechts één waarde is opgegeven in de query voor samenvatten... op de timestamp (dat wil zeggen); vergelijkbaar met het vorige voorbeeld, aan het einde van de uitvoering van de uitvoer zou zijn zoals hieronder weergegeven. 

   ![Metrische meting queryuitvoering met enkelvoudige waarde](./media/monitor-alerts-unified/LogMMtimestamp.png)

Als ' totale "tijdstempel is: de gegevens gesorteerd op timestamp-kolom (zoals in rood); en we op timestamp groeperen –: waarden voor `2018-10-17T06:00:00Z` worden beschouwd als een diagram/entiteit (zoals gemarkeerd in oranje). In deze entiteit/waarde kaartverbeteringen – vindt alert-service geen achtereenvolgende schendingen voorkomende (zoals elke waarde timestamp slechts één vermelding heeft) en kan daarom waarschuwing wordt nooit ophalen geactiveerd. Daarom in een dergelijk geval moet gebruiker een van:
- Een dummy-variabele of een bestaande variabele (zoals $table) toevoegen aan het correct sorteren gedaan met behulp van "Cumulatieve bij" veld geconfigureerd
- (Of) opnieuw configureren van de waarschuwingsregel voor het gebruik van waarschuwingslogica op basis van *totaal inbreuk* in plaats daarvan op de juiste wijze
 
## <a name="log-alert-fired-unnecessarily"></a>Waarschuwing geactiveerd onnodig
Gedetailleerde volgende worden enkele veelvoorkomende redenen waarom een geconfigureerde [waarschuwingsregel in Azure Monitor](alert-log.md) kan worden geactiveerd wanneer deze wordt bekeken [Azure-waarschuwingen](monitoring-alerts-managing-alert-states.md), wanneer u niet verwacht dat het worden geactiveerd.

### <a name="alert-triggered-by-partial-data"></a>Waarschuwing geactiveerd door gegevens gedeeltelijk
Levert het vermogen voor Log Analytics en Application Insights Analytics zijn afhankelijk van vertragingen van gegevensopname en -verwerking; vanwege die op het moment wanneer opgegeven logboekwaarschuwingsquery wordt uitgevoerd - mogelijk zijn er een aanvraag van geen gegevens beschikbaar worden gesteld of alleen bepaalde gegevens beschikbaar worden gesteld. Zie voor meer informatie, [gegevensopname tijd in Log Analytics](../log-analytics/log-analytics-data-ingestion-time.md).

Afhankelijk van hoe de waarschuwingsregel is geconfigureerd, kunnen er verkeerd firing als er geen of gedeeltelijke gegevens in Logboeken op het moment van uitvoering van de waarschuwing. In dergelijke gevallen is het raadzaam dat Waarschuwingsquery of configuratieversie is gewijzigd. *Bijvoorbeeld, als het logboek waarschuwingsregel is geconfigureerd om te activeren wanneer het aantal resultaten uit analytische query is kleiner dan 5; (bijvoorbeeld) Wanneer er geen gegevens (nul record) of een gedeeltelijke resultaten (één record) kunnen de waarschuwingsregel vervolgens wordt u geactiveerd. Waar-als u na de opname vertraging wanneer dezelfde query wordt uitgevoerd in Analytics de query met de volledige gegevens bieden mogelijk resultaat als 10 records.*

### <a name="alert-query-output-misunderstood"></a>Waarschuwing query-uitvoer verkeerd begrepen
Voor waarschuwingen voor wordt de logica voor waarschuwingen geboden door de gebruiker via de analytics-query. De opgegeven analytics-query kunt gebruiken voor verschillende Big Data en wiskundige functies te maken van specifieke constructies. De waarschuwingen-service wordt de klant opgegeven query uitvoeren met intervallen opgegeven met de gegevens voor een periode die zijn opgegeven. waarschuwingen van de service maakt subtiele wijzigingen in query opgegeven - op basis van het Waarschuwingstype gekozen en hetzelfde in de sectie 'Een Query om te worden uitgevoerd' in configureren signaal logische scherm worden gelogd zoals hieronder weergegeven: ![Query moet worden uitgevoerd](./media/monitor-alerts-unified/LogAlertPreview.png)
 
Wat wordt weergegeven in **query moet worden uitgevoerd** sectie is welke logboek alert-service wordt uitgevoerd; gebruiker kan uitvoeren voor de opgegeven query, evenals de timespan via [analyseportal](../log-analytics/log-analytics-log-search-portals.md) of [Tekstanalyse-API](https://docs.microsoft.com/rest/api/loganalytics/) -Als ze weten voordat u het maken van waarschuwingen willen, welke uitvoer Waarschuwingsquery mogelijk.
 
## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](monitor-alerts-unified-log.md)
* Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md)
* Meer informatie over [Log Analytics](../log-analytics/log-analytics-queries.md). 