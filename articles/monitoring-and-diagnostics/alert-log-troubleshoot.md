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
ms.openlocfilehash: e2326f56ad367f744bc7895bc8c4bfd6f32d0310
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52264876"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Waarschuwingen voor het oplossen van problemen in Azure Monitor  
## <a name="overview"></a>Overzicht
Dit artikel laat u over het oplossen van problemen die regelmatig voorkomen bij het instellen van waarschuwingen in Azure monitor. Het biedt ook oplossingen voor antwoorden op veelgestelde vragen met betrekking tot de functionaliteit of de configuratie van waarschuwingen. 

De term **Logboekwaarschuwingen** om te beschrijven van waarschuwingen die worden gestart op basis van een aangepaste query in [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-analytics.md). Meer informatie over functies, -terminologie en -typen in [Logboekwaarschuwingen - overzicht](monitor-alerts-unified-log.md).

> [!NOTE]
> In dit artikel geen rekening gehouden met de aanvragen als de Azure-portal wordt weergegeven en Waarschuwing regel geactiveerd en een melding die wordt uitgevoerd door een bijbehorende actie groep(en). Voor dergelijke gevallen raadpleegt u de details in het artikel op [actiegroepen](monitoring-action-groups.md).


## <a name="log-alert-didnt-fire"></a>Waarschuwing is niet gestart

Hier zijn enkele veelvoorkomende redenen waarom een geconfigureerde [waarschuwingsregel in Azure Monitor](alert-log.md) staat geen weergegeven [als *geactiveerd* wanneer verwacht](monitoring-alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Gegevens opnemen tijd voor logboeken
Waarschuwing wordt periodiek uitgevoerd voor de query op basis van [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-analytics.md). Omdat de Log Analytics verwerkt vele terabytes aan gegevens van duizenden klanten uit verschillende bronnen over de hele wereld, is de service is vatbaar voor verschillende vertraging. Zie voor meer informatie, [gegevensopname tijd in Log Analytics](../log-analytics/log-analytics-data-ingestion-time.md).

Als u wilt beperken gegevensopname vertraging, het systeem moet wachten en probeert de Waarschuwingsquery meerdere keren opnieuw als er dat nog niet de benodigde gegevens worden opgenomen. Het systeem heeft een exponentieel toenemende wachttijd instellen. Het logboek waarschuwing alleen triggers nadat de gegevens beschikbaar zijn, zodat ze vertraging kunnen worden veroorzaakt door trage logboekgegevens opnemen. 

### <a name="incorrect-time-period-configured"></a>Periode onjuist geconfigureerd
Zoals beschreven in het artikel op [terminologie voor logboekwaarschuwingen](monitor-alerts-unified-log.md#log-search-alert-rule---definition-and-types), de tijd die periode wordt vermeld in configuratie Hiermee geeft u het tijdsbereik voor de query. De query retourneert alleen de records die zijn gemaakt binnen dit tijdsbereik. Periode Hiermee beperkt u de gegevens opgehaald voor de query voor om misbruik te voorkomen en willekeurige opdracht van de tijd heeft (zoals geleden) gebruikt in logboekquery. 
*Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd op 13:15 uur, worden alleen de records die zijn gemaakt tussen 12:15 uur en 13:15 uur gebruikt voor de logboekquery. Als de logboekquery gebruikmaakt van een opdracht tijd als *geleden (1d)*, de query nog steeds alleen gegevens tussen 12:15 uur en 13:15 uur worden gebruikt omdat de periode is ingesteld op dat het interval.*

Daarom controle die periode in de configuratie komt overeen met uw query. Eerder aangegeven, als de logboekquery gebruikt bijvoorbeeld *geleden (1d)* zoals wordt weergegeven met groene markering, klikt u vervolgens de periode moet worden ingesteld op 24 uur of 1440 minuten (zoals aangegeven in rood), om te controleren of de query wordt uitgevoerd zoals bedoeld.

![Periode](./media/monitor-alerts-unified/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Optie is ingesteld waarschuwingen onderdrukken
Zoals beschreven in stap 8 van het artikel op [een waarschuwingsregel maken in Azure portal](alert-log.md#managing-log-alerts-from-the-azure-portal), waarschuwingen bieden een **waarschuwingen onderdrukken** optie voor het activeren en melding onderdrukken voor een geconfigureerde hoeveelheid de tijd. Als gevolg hiervan, mag u denkt dat een waarschuwing is niet gestart in werkelijkheid het heeft, maar is onderdrukt.  

![Waarschuwingen onderdrukken](./media/monitor-alerts-unified/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>De waarschuwingsregel meting van metrische gegevens is onjuist
**Waarschuwingen voor meting van metrische gegevens** zijn van een subtype van waarschuwingen, waarvoor speciale mogelijkheden en een beperkte waarschuwing query-syntaxis. Een waarschuwingsregel meting van metrische gegevens is vereist voor de query uitvoeren om te worden van een metrische tijdreeks; dat wil zeggen, grote een tabel met afzonderlijke even voor perioden, samen met de bijbehorende geaggregeerde waarden. Bovendien kunnen gebruikers kiezen om meer variabelen in de tabel naast AggregatedValue. Deze variabelen kunnen worden gebruikt om de tabel te sorteren. 

Stel bijvoorbeeld dat een waarschuwingsregel meting van metrische gegevens is geconfigureerd als:
- query is: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- periode van 6 uur
- drempelwaarde van 50
- waarschuwingslogica van drie achtereenvolgende schendingen
- Cumulatieve bij gekozen als $table

Omdat de opdracht bevat *... samenvatten op* en twee variabelen (timestamp & $table), het systeem kiest $table naar 'Cumulatieve na'. Sorteert de resultaattabel in het veld *$table* zoals hieronder wordt weergegeven en wordt gecontroleerd op de meerdere AggregatedValue voor elk tabeltype (zoals availabilityResults) als er achtereenvolgende schendingen van 3 of meer is.

![Metrische meting queryuitvoering met meerdere waarden](./media/monitor-alerts-unified/LogMMQuery.png)

Als ' totale "$table is: de gegevens gesorteerd op kolom $table (zoals in rood); we groep en zoek naar "Cumulatieve bij" veld typen (dat wil zeggen) $table – bijvoorbeeld: waarden voor de availabilityResults worden beschouwd als een diagram/entiteit (zoals gemarkeerd in oranje). In deze entiteit/waarde plot: waarschuwing service controleert op drie achtereenvolgende schendingen optreden (zoals weergegeven in groen) voor die waarschuwing wordt ophalen geactiveerd voor de tabelwaarde 'availabilityResults'. Op dezelfde manier als voor een andere waarde voor $table als zijn zichtbaar voor drie opeenvolgende inbreuken op - worden een andere waarschuwingsmeldingen geactiveerd voor hetzelfde; met waarschuwing service automatisch de waarden in één diagram/entiteit (zoals in oranje) op tijd te sorteren.

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
* Meer informatie over [Log Analytics](../log-analytics/log-analytics-overview.md). 

