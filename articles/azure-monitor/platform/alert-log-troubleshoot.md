---
title: Het oplossen van waarschuwingen in Azure Monitor | Microsoft Docs
description: Veelvoorkomende problemen, fouten en omzetting voor waarschuwingsregels in Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 0c7189f1d43a114532b30b0c1aabe6f7cd4402d8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60775947"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Waarschuwingen voor het oplossen van problemen in Azure Monitor  

## <a name="overview"></a>Overzicht

Dit artikel laat u over het oplossen van problemen die regelmatig voorkomen bij het instellen van waarschuwingen in Azure Monitor. Het biedt ook oplossingen voor antwoorden op veelgestelde vragen met betrekking tot de functionaliteit of de configuratie van waarschuwingen. 

De term **Logboekwaarschuwingen** beschrijving van waarschuwingen die worden gestart op basis van een query voor in een [Log Analytics-werkruimte](../learn/tutorial-viewdata.md) of [Application Insights](../../azure-monitor/app/analytics.md). Meer informatie over functies, -terminologie en -typen in [Logboekwaarschuwingen - overzicht](../platform/alerts-unified-log.md).

> [!NOTE]
> In dit artikel geen rekening gehouden met de aanvragen als de Azure-portal wordt weergegeven en Waarschuwing regel geactiveerd en een melding die wordt uitgevoerd door een bijbehorende actie groep(en). Voor dergelijke gevallen raadpleegt u de details in het artikel op [actiegroepen](../platform/action-groups.md).

## <a name="log-alert-didnt-fire"></a>Waarschuwing is niet gestart

Hier zijn enkele veelvoorkomende redenen waarom een geconfigureerde [waarschuwingsregel in Azure Monitor](../platform/alerts-log.md) staat geen weergegeven [als *geactiveerd* wanneer verwacht](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Gegevens opnemen tijd voor logboeken

Waarschuwing wordt periodiek uitgevoerd voor de query op basis van [Log Analytics](../learn/tutorial-viewdata.md) of [Application Insights](../../azure-monitor/app/analytics.md). Omdat Azure Monitor vele terabytes aan gegevens van duizenden klanten uit verschillende bronnen over de hele wereld verwerkt, is de service is vatbaar voor verschillende vertraging. Zie voor meer informatie, [gegevensopname tijd in de logboeken van Azure Monitor](../platform/data-ingestion-time.md).

Als u wilt beperken gegevensopname vertraging, het systeem moet wachten en probeert de Waarschuwingsquery meerdere keren opnieuw als er dat nog niet de benodigde gegevens worden opgenomen. Het systeem heeft een exponentieel toenemende wachttijd instellen. Het logboek waarschuwing alleen triggers nadat de gegevens beschikbaar zijn, zodat ze vertraging kunnen worden veroorzaakt door trage logboekgegevens opnemen. 

### <a name="incorrect-time-period-configured"></a>Periode onjuist geconfigureerd

Zoals beschreven in het artikel op [terminologie voor logboekwaarschuwingen](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types), de tijd die periode wordt vermeld in configuratie Hiermee geeft u het tijdsbereik voor de query. De query retourneert alleen de records die zijn gemaakt binnen dit tijdsbereik. Periode Hiermee beperkt u de gegevens opgehaald voor de query voor om misbruik te voorkomen en willekeurige opdracht van de tijd heeft (zoals *geleden*) gebruikt in logboekquery. Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd op 13:15 uur, worden alleen de records die zijn gemaakt tussen 12:15 uur en 13:15 uur gebruikt voor de logboekquery. Als de logboekquery gebruikmaakt van een opdracht tijd als *geleden (1d)*, de query nog steeds alleen gegevens tussen 12:15 uur en 13:15 uur worden gebruikt omdat de periode is ingesteld op die interval.*

Daarom controle die periode in de configuratie komt overeen met uw query. Eerder aangegeven, als de logboekquery gebruikt bijvoorbeeld *geleden (1d)* zoals wordt weergegeven met groene markering, klikt u vervolgens de periode moet worden ingesteld op 24 uur of 1440 minuten (zoals aangegeven in rood), om te controleren of de query wordt uitgevoerd zoals bedoeld.

![Periode](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Optie is ingesteld waarschuwingen onderdrukken

Zoals beschreven in stap 8 van het artikel op [een waarschuwingsregel maken in Azure portal](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal), waarschuwingen bieden een **waarschuwingen onderdrukken** optie voor het activeren en melding onderdrukken voor een geconfigureerde hoeveelheid de tijd. Als gevolg hiervan, mag u denkt dat een waarschuwing is niet gestart in werkelijkheid het heeft, maar is onderdrukt.  

![Waarschuwingen onderdrukken](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>De waarschuwingsregel meting van metrische gegevens is onjuist

**Waarschuwingen voor meting van metrische gegevens** zijn van een subtype van waarschuwingen, waarvoor speciale mogelijkheden en een beperkte waarschuwing query-syntaxis. Een waarschuwingsregel meting van metrische gegevens is vereist voor de query uitvoeren om te worden van een metrische tijdreeks; dat wil zeggen, grote een tabel met afzonderlijke even voor perioden, samen met de bijbehorende geaggregeerde waarden. Bovendien kunnen gebruikers kiezen om meer variabelen in de tabel naast AggregatedValue. Deze variabelen kunnen worden gebruikt om de tabel te sorteren. 

Stel bijvoorbeeld dat een waarschuwingsregel meting van metrische gegevens is geconfigureerd als:

- query is: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- periode van 6 uur
- drempelwaarde van 50
- waarschuwingslogica van drie achtereenvolgende schendingen
- Cumulatieve bij gekozen als $table

Omdat de opdracht bevat *... samenvatten op* en twee variabelen (timestamp & $table), het systeem kiest $table naar *cumulatieve bij*. Sorteert de resultaattabel in het veld *$table* zoals hieronder wordt weergegeven en wordt gecontroleerd op de meerdere AggregatedValue voor elk tabeltype (zoals availabilityResults) als er achtereenvolgende schendingen van 3 of meer is.

![Metrische meting queryuitvoering met meerdere waarden](media/alert-log-troubleshoot/LogMMQuery.png)

Als *cumulatieve bij* is gedefinieerd in het *$table*, de gegevens gesorteerd op kolom $table (zoals in rood); en we groep en zoek naar typen *cumulatieve bij* veld (dat wil zeggen) $table voor Voorbeeld: waarden voor de availabilityResults worden beschouwd als een diagram/entiteit (zoals gemarkeerd in oranje). In deze waarde plot/entiteit waarschuwing service controleert op drie achtereenvolgende schendingen optreden (zoals weergegeven in groen) voor die waarschuwing wordt ophalen geactiveerd voor de tabelwaarde 'availabilityResults'. Op dezelfde manier als voor een andere waarde voor $table als zijn zichtbaar voor drie opeenvolgende inbreuken op - worden een andere waarschuwingsmeldingen geactiveerd voor hetzelfde; met waarschuwing service automatisch de waarden in één diagram/entiteit (zoals in oranje) op tijd te sorteren.

Stel meting van metrische gegevens waarschuwingsregel is gewijzigd en de query is `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)` met de rest van de configuratie van de resterende hetzelfde als voordat waarschuwingslogica voor drie opeenvolgende schendingen. De optie 'Samenvoegen op' in dit geval zal worden standaard: timestamp. Omdat er slechts één waarde is opgegeven in de query voor *... samenvatten op* (dat wil zeggen) *timestamp*; die vergelijkbaar is met het vorige voorbeeld, aan het einde van de uitvoering van de uitvoer zijn zou, zoals hieronder weergegeven.

   ![Metrische meting queryuitvoering met enkelvoudige waarde](media/alert-log-troubleshoot/LogMMtimestamp.png)

Als *cumulatieve bij* is gedefinieerd in timestamp, de gegevens gesorteerd op *timestamp* kolom (zoals in rood); en we groeperen op timestamp bijvoorbeeld: waarden voor `2018-10-17T06:00:00Z` wordt beschouwd als een diagram/entiteit (als een gemarkeerd in oranje). In deze waarde plot/entiteit vindt alert-service er geen achtereenvolgende schendingen voorkomende (zoals elke waarde timestamp slechts één vermelding heeft) en kan daarom waarschuwing wordt nooit ophalen geactiveerd. Daarom in een dergelijk geval moet gebruiker een van:

- Een dummy-variabele of een bestaande variabele (zoals $table) toevoegen aan het correct sorteren gedaan met behulp van "Cumulatieve bij" veld geconfigureerd
- (Of) opnieuw configureren van de waarschuwingsregel voor het gebruik van waarschuwingslogica op basis van *totaal inbreuk* in plaats daarvan op de juiste wijze

## <a name="log-alert-fired-unnecessarily"></a>Waarschuwing geactiveerd onnodig

Gedetailleerde volgende worden enkele veelvoorkomende redenen waarom een geconfigureerde [waarschuwingsregel in Azure Monitor](../platform/alerts-log.md) kan worden geactiveerd wanneer deze wordt bekeken [Azure-waarschuwingen](../platform/alerts-managing-alert-states.md), wanneer u niet verwacht dat het worden geactiveerd.

### <a name="alert-triggered-by-partial-data"></a>Waarschuwing geactiveerd door gegevens gedeeltelijk

Levert het vermogen voor Log Analytics en Application Insights Analytics zijn afhankelijk van vertragingen van gegevensopname en -verwerking; vanwege die op het moment wanneer opgegeven logboekwaarschuwingsquery wordt uitgevoerd - mogelijk zijn er een aanvraag van geen gegevens beschikbaar worden gesteld of alleen bepaalde gegevens beschikbaar worden gesteld. Zie voor meer informatie, [gegevensopname keer aanmelden met Azure Monitor](../platform/data-ingestion-time.md).

Afhankelijk van hoe de waarschuwingsregel is geconfigureerd, kunnen er verkeerd firing als er geen of gedeeltelijke gegevens in Logboeken op het moment van uitvoering van de waarschuwing. In dergelijke gevallen adviseren we u om de Waarschuwingsquery of de configuratie te wijzigen. 

Bijvoorbeeld, als de waarschuwingsregel is geconfigureerd om te activeren wanneer het aantal resultaten van een analytics-query lager is dan 5, klikt u vervolgens de waarschuwing wordt geactiveerd wanneer er geen gegevens (nul record) of een gedeeltelijke resultaten (één record). Na de vertraging opname van gegevens, kan dezelfde query met de volledige gegevens evenwel een resultaat van 10 records.

### <a name="alert-query-output-misunderstood"></a>Waarschuwing query-uitvoer verkeerd begrepen

U opgeven de logica voor logboekwaarschuwingen in een analytics-query. De analytics-query mag gebruiken verschillende wiskundige functies en big data.  De waarschuwingen service voert de query met intervallen opgegeven met de gegevens voor de opgegeven tijdsperiode. De waarschuwingen service maakt subtiele wijzigingen in de opgegeven query op basis van het Waarschuwingstype dat is gekozen. Deze wijziging kan worden weergegeven in de sectie 'Een Query om te worden uitgevoerd' in *signaallogica configureren* scherm, zoals hieronder wordt weergegeven: ![Query moet worden uitgevoerd](media/alert-log-troubleshoot/LogAlertPreview.png)

Wat wordt weergegeven in de **query moet worden uitgevoerd** vak is wat de waarschuwing log-service wordt uitgevoerd. U kunt uitvoeren met de opgegeven query, evenals de timespan via [analyseportal](../log-query/portals.md) of de [Tekstanalyse-API](https://docs.microsoft.com/rest/api/loganalytics/) als u weten wat de Waarschuwingsquery uitvoer mag zijn wilt voordat u daadwerkelijk de waarschuwing hebt gemaakt.

## <a name="log-alert-was-disabled"></a>Waarschuwing is uitgeschakeld

Hieronder volgen enkele oorzaken van dit [waarschuwingsregel in Azure Monitor](../platform/alerts-log.md) kan worden uitgeschakeld door Azure Monitor.

### <a name="resource-on-which-alert-was-created-no-longer-exists"></a>Er bestaat resource waarop waarschuwing niet meer is gemaakt

Meld u waarschuwingsregels die zijn gemaakt in Azure Monitor-doel van een specifieke bron, zoals een Azure Log Analytics-werkruimte, de Azure Application Insights-app en de Azure-resource. En voer de waarschuwing log-service vervolgens analysequery opgegeven in de regel voor het opgegeven doel. Maar na het maken van de regel, vaak gebruikers gaat u naar van Azure verwijderen of verplaatsen binnen Azure - het doel van de waarschuwingsregel. Als het doel van de waarschuwingsregel niet meer geldig is, mislukt de uitvoering van de regel.

In dergelijke gevallen worden Azure Monitor uitschakelen van de waarschuwing en controleer of klanten worden niet onnodig, gefactureerd wanneer de regel zelf is niet continu uitvoeren voor aanzienlijke periode, zoals een week. Gebruikers vindt u op de exacte tijd waarop de waarschuwingsregel is uitgeschakeld door Azure Monitor via [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md). Wanneer de waarschuwingsregel is uitgeschakeld door Azure, wordt een gebeurtenis in Azure-activiteitenlogboek toegevoegd in Azure-activiteitenlogboek.

Een voorbeeld van de gebeurtenis in Azure-activiteitenlogboek waarschuwingsregel is uitgeschakeld omdat er continu; Hieronder wordt weergegeven.

```json
{
    "caller": "Microsoft.Insights/ScheduledQueryRules",
    "channels": "Operation",
    "claims": {
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/ScheduledQueryRules"
    },
    "correlationId": "abcdefg-4d12-1234-4256-21233554aff",
    "description": "Alert: test-bad-alerts is disabled by the System due to : Alert has been failing consistently with the same exception for the past week",
    "eventDataId": "f123e07-bf45-1234-4565-123a123455b",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2019-03-22T04:18:22.8569543Z",
    "id": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "localizedValue": "Microsoft.Insights/ScheduledQueryRules/disable/action"
    },
    "resourceGroupName": "<Resource Group>",
    "resourceProviderName": {
        "value": "MICROSOFT.INSIGHTS",
        "localizedValue": "Microsoft Insights"
    },
    "resourceType": {
        "value": "MICROSOFT.INSIGHTS/scheduledqueryrules",
        "localizedValue": "MICROSOFT.INSIGHTS/scheduledqueryrules"
    },
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-03-22T04:18:22.8569543Z",
    "subscriptionId": "<SubscriptionId>",
    "properties": {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
        "subscriptionId": "<SubscriptionId>",
        "resourceGroup": "<ResourceGroup>",
        "eventDataId": "12e12345-12dd-1234-8e3e-12345b7a1234",
        "eventTimeStamp": "03/22/2019 04:18:22",
        "issueStartTime": "03/22/2019 04:18:22",
        "operationName": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "status": "Succeeded",
        "reason": "Alert has been failing consistently with the same exception for the past week"
    },
    "relatedEvents": []
}
```

### <a name="query-used-in-log-alert-is-not-valid"></a>Query die wordt gebruikt in een logboek waarschuwing is niet geldig

Elke logboekwaarschuwingen in Azure Monitor is gemaakt als onderdeel van de configuratie moet een analytics-query moet regelmatig worden uitgevoerd door de waarschuwing service opgeven. Terwijl de analytics-query mogelijk de juiste syntaxis op het moment van de regel voor het maken of bijwerken. Enkele voorbeelden van momenten gedurende een bepaalde periode, de query opgeven in het logboek waarschuwingsregel syntaxisproblemen ontwikkelen en ervoor zorgen dat de uitvoering van de regel voor het mislukken. Er zijn enkele veelvoorkomende redenen waarom opgegeven in een waarschuwingsregel voor log analytics-query met fouten ontwikkelen kunt:

- Query wordt geschreven naar [uitvoeren voor meerdere resources](../log-query/cross-workspace-query.md) en een of meer van de resources die zijn opgegeven, nu nog niet bestaan.
- Er is geen gegevensstroom aan het analytics-platform, omdat dat de [uitvoering van de query geeft een foutmelding](https://dev.loganalytics.io/documentation/Using-the-API/Errors) omdat er geen gegevens voor de opgegeven query.
- Wijzigingen in [querytaal](https://docs.microsoft.com/azure/kusto/query/) hebben plaatsgevonden in welke opdrachten en -functies hebben een herziene indeling. De eerder opgegeven query in de waarschuwingsregel is daarom niet meer geldig.

De gebruiker moet worden gewaarschuwd over dit gedrag eerst [Azure Advisor](../../advisor/advisor-overview.md). Een aanbeveling kan worden toegevoegd voor de specifieke waarschuwingsregel op Azure Advisor, onder de categorie van hoge beschikbaarheid met normale impact en een beschrijving zoals 'Uw waarschuwingsregel om ervoor te zorgen bewaking herstellen'. Als de Waarschuwingsquery in de opgegeven logboekwaarschuwingsregel is niet hersteld na zeven dagen van de aanbeveling bieden op Azure Advisor. Vervolgens wordt Azure Monitor uitschakelen van de waarschuwing en zorg ervoor dat klanten worden niet onnodig, gefactureerd wanneer de regel zelf is niet continu uitvoeren voor aanzienlijke periode, zoals een week.

Gebruikers vindt u op de exacte tijd waarop de waarschuwingsregel is uitgeschakeld door Azure Monitor via [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md). Als de waarschuwingsregel is uitgeschakeld door de Azure - wordt een gebeurtenis in Azure Activity Log toegevoegd in Azure-activiteitenlogboek.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](../platform/alerts-unified-log.md)
- Meer informatie over [Application Insights](../../azure-monitor/app/analytics.md)
- Meer informatie over [melden van query's](../log-query/log-query-overview.md)
