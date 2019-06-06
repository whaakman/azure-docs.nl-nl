---
title: Oplossen van waarschuwingen in Azure Monitor | Microsoft Docs
description: Veelvoorkomende problemen, fouten en oplossingen voor waarschuwingsregels in Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 03a6ea45577b4a4bf57501b1834f91438feb4e2b
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66477874"
---
# <a name="troubleshoot-log-alerts-in-azure-monitor"></a>Oplossen van waarschuwingen in Azure Monitor  

Dit artikel laat u over het oplossen van veelvoorkomende problemen die kunnen ontstaan tijdens het instellen van waarschuwingen in Azure Monitor. Het biedt ook oplossingen voor veelvoorkomende problemen met de functie of de configuratie van waarschuwingen. 

De term *waarschuwingen voor activiteitenlogboeken* beschrijven regels die worden gestart op basis van een query voor in een [Azure Log Analytics-werkruimte](../learn/tutorial-viewdata.md) of in [Azure Application Insights](../../azure-monitor/app/analytics.md). Meer informatie over functies, -terminologie en -typen in [waarschuwingen voor activiteitenlogboeken in Azure Monitor](../platform/alerts-unified-log.md).

> [!NOTE]
> In dit artikel geen rekening gehouden met gevallen waarbij de Azure-portal ziet u een waarschuwingsregel geactiveerd en een melding wordt niet uitgevoerd met een bijbehorende actiegroep. Zie de details in voor dergelijke gevallen [maken en beheren van actiegroepen in Azure portal](../platform/action-groups.md).

## <a name="log-alert-didnt-fire"></a>Waarschuwing is niet gestart

Hier zijn enkele veelvoorkomende redenen waarom de status voor een geconfigureerde [waarschuwingsregel in Azure Monitor](../platform/alerts-log.md) wordt niet weergegeven [als *geactiveerd* wanneer verwacht](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Gegevens opnemen tijd voor logboeken

Een waarschuwing wordt periodiek uitgevoerd voor uw query op basis van [Log Analytics](../learn/tutorial-viewdata.md) of [Application Insights](../../azure-monitor/app/analytics.md). Omdat Azure Monitor vele terabytes aan gegevens van duizenden klanten uit verschillende bronnen over de hele wereld verwerkt, is de service is vatbaar voor verschillende vertragingen. Zie voor meer informatie, [gegevensopname tijd in de logboeken van Azure Monitor](../platform/data-ingestion-time.md).

Als u wilt verhelpen vertragingen, het systeem moet wachten en probeert de Waarschuwingsquery meerdere keren opnieuw als er dat nog niet de benodigde gegevens worden opgenomen. Het systeem heeft een exponentieel toenemende wachttijd instellen. De waarschuwing wordt geactiveerd nadat de gegevens beschikbaar, zijn zodat de vertraging wordt mogelijk veroorzaakt door trage opname van gegevens aan het logboek. 

### <a name="incorrect-time-period-configured"></a>Periode onjuist geconfigureerd

Zoals beschreven in het artikel op [terminologie voor logboekwaarschuwingen](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types), de periode die is vermeld in de configuratie van de Hiermee geeft u het tijdsbereik voor de query. De query retourneert alleen de records die zijn gemaakt binnen dit bereik. 

De periode beperkt de gegevens opgehaald voor een query voor om misbruik te voorkomen, en deze willekeurige opdracht van de tijd heeft (zoals **geleden**) gebruikt in een logboekquery. Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd op 13:15 uur, worden alleen de records die zijn gemaakt tussen 12:15 uur en 13:15 uur gebruikt voor de logboekquery. Als de logboekquery gebruikmaakt van een opdracht tijd als **geleden (1d)** , de query nog steeds alleen gegevens tussen 12:15 uur en 13:15 uur worden gebruikt omdat de periode is ingesteld op dat het interval.

Controleer of de periode in de configuratie overeenkomt met uw query. Voor het voorbeeld hierboven, als de logboekquery gebruikt **geleden (1d)** met de groene markering, de periode moet worden ingesteld op 24 uur of 1440 minuten (zoals aangegeven in rood). Deze instelling zorgt ervoor dat de query wordt uitgevoerd zoals bedoeld.

![Periode](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Optie is ingesteld waarschuwingen onderdrukken

Zoals beschreven in stap 8 van het artikel op [het maken van een waarschuwingsregel in de Azure-portal](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal), waarschuwingen bieden een **waarschuwingen onderdrukken** optie om te activeren en melding onderdrukken voor een ingestelde van tijd. Als gevolg hiervan, denkt u dat er een waarschuwing is niet gestart. In feite het geactiveerd, maar is onderdrukt.  

![Waarschuwingen onderdrukken](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>De waarschuwingsregel meting van metrische gegevens is onjuist

*Waarschuwingen voor meting van metrische gegevens* zijn van een subtype van waarschuwingen die speciale mogelijkheden en een beperkte waarschuwing query-syntaxis zijn. Een regel voor een waarschuwing voor een meting van metrische gegevens is vereist voor de query uitvoeren om te worden van een metrische tijdreeks. Dat wil zeggen, is de uitvoer een tabel met afzonderlijke, gelijke grootte perioden, samen met de bijbehorende geaggregeerde waarden. 

U kunt extra variabelen hebben in de tabel naast **AggregatedValue**. Deze variabelen kunnen worden gebruikt om de tabel te sorteren. 

Stel bijvoorbeeld dat een regel voor een waarschuwing voor een meting van metrische gegevens is geconfigureerd als:

- Query van `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- periode van 6 uur
- drempelwaarde van 50
- waarschuwingslogica van drie achtereenvolgende schendingen
- **Cumulatieve bij** gekozen als **$table**

Omdat de opdracht bevat **samenvatten... door** en biedt twee variabelen (**timestamp** en **$table**), het systeem kiest **$table** voor **cumulatieve bij** . Het systeem sorteert de resultaattabel door de **$table** veld, zoals wordt weergegeven in de volgende schermafbeelding. Wordt gekeken naar de meerdere **AggregatedValue** exemplaren voor elk tabeltype (zoals **availabilityResults**) om te zien of er drie of meer achtereenvolgende schendingen zijn.

![De uitvoering van de meting van metrische gegevens query met meerdere waarden](media/alert-log-troubleshoot/LogMMQuery.png)

Omdat **cumulatieve bij** is gedefinieerd in het **$table**, de gegevens gesorteerd op een **$table** kolom (zoals aangegeven in rood). En we groep en zoek naar typen van de **cumulatieve bij** veld. 

Bijvoorbeeld: voor **$table**, waarden voor **availabilityResults** worden beschouwd als een diagram/entiteit (aangeduid in oranje). In deze entiteit/waarde diagram, wordt de waarschuwing service controleert voor drie opeenvolgende inbreuken op (zoals aangegeven in het groen). De inbreuk op een waarschuwing voor de tabelwaarde **availabilityResults**. 

Op dezelfde manier als drie achtereenvolgende schendingen voor een andere waarde van gebeurt **$table**, een andere waarschuwing wordt geactiveerd voor hetzelfde te doen. De waarschuwing service worden automatisch de waarden in één diagram/entiteit (aangeduid in oranje) gesorteerd op tijd.

Stel nu dat de regel voor de waarschuwing voor een meting van metrische gegevens is gewijzigd en de query is `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)`. De rest van de configuratie van de bleef gelijk zijn aan voordat u, met inbegrip van de waarschuwing logica voor drie opeenvolgende schendingen. De **cumulatieve bij** optie is in dit geval **timestamp** standaard. Slechts één waarde is opgegeven in de query voor **samenvatten... door** (dat wil zeggen, **timestamp**). Als het vorige voorbeeld zou de uitvoer aan het einde van de uitvoering van zoals wordt geïllustreerd als volgt.

   ![De uitvoering van de meting van metrische gegevens query met enkelvoudige waarde](media/alert-log-troubleshoot/LogMMtimestamp.png)

Omdat **cumulatieve bij** is gedefinieerd in het **timestamp**, de gegevens gesorteerd op de **timestamp** kolom (zoals aangegeven in rood). En we groeperen op **timestamp**. Bijvoorbeeld, waarden voor `2018-10-17T06:00:00Z` worden beschouwd als een diagram/entiteit (aangeduid in oranje). In deze entiteit/waarde plot, vindt de waarschuwings-service geen achtereenvolgende schendingen (omdat elke **timestamp** waarde heeft slechts één vermelding). De waarschuwing wordt, zodat nooit geactiveerd. In dat geval moet moet de gebruiker een van:

- Een dummy-variabele of een bestaande variabele toevoegen (zoals **$table**) correct sorteren met behulp van de **cumulatieve bij** veld.
- Configureren van de waarschuwingsregel voor het gebruik van waarschuwingslogica op basis van **totaal inbreuk** in plaats daarvan.

## <a name="log-alert-fired-unnecessarily"></a>Waarschuwing geactiveerd onnodig

Een geconfigureerde [waarschuwingsregel in Azure Monitor](../platform/alerts-log.md) mogelijk onverwacht worden geactiveerd wanneer u deze bekijken in [Azure-waarschuwingen](../platform/alerts-managing-alert-states.md). De volgende secties worden enkele veelvoorkomende redenen waarom.

### <a name="alert-triggered-by-partial-data"></a>Waarschuwing geactiveerd door gegevens gedeeltelijk

Log Analytics en Application Insights zijn afhankelijk van vertragingen van gegevensopname en -verwerking. Wanneer u een logboekwaarschuwingsquery uitvoert, u wellicht dat er geen gegevens beschikbaar is of slechts enkele gegevens beschikbaar is. Zie voor meer informatie, [gegevensopname keer aanmelden met Azure Monitor](../platform/data-ingestion-time.md).

Afhankelijk van de configuratie van de waarschuwingsregel kan ontstekingsfout optreden als er geen gegevens of gegevens gedeeltelijk in Logboeken op het moment van uitvoering van de waarschuwing. In dergelijke gevallen adviseren we u de Waarschuwingsquery of de configuratie wijzigen. 

Bijvoorbeeld, als u de waarschuwingsregel wordt geactiveerd wanneer het aantal resultaten van een analytics-query kleiner dan 5 is configureert, wordt de waarschuwing geactiveerd wanneer er geen data (geen record) of een gedeeltelijke resultaten (één record is). Maar na de vertraging opname van gegevens, dezelfde query met de volledige gegevens een resultaat van 10 records kan bevatten.

### <a name="alert-query-output-is-misunderstood"></a>Waarschuwing query-uitvoer is verkeerd begrepen

U opgeven de logica voor logboekwaarschuwingen in een analytics-query. De analytics-query kunt gebruiken verschillende wiskundige functies en big data. De waarschuwing service voert uw query met intervallen opgegeven met de gegevens voor een opgegeven periode. De waarschuwing service aanbrengt subtiele wijzigingen in de query op basis van het Waarschuwingstype. U vindt deze wijziging in de **Query moet worden uitgevoerd** sectie op de **signaallogica configureren** scherm:

![Query moet worden uitgevoerd](media/alert-log-troubleshoot/LogAlertPreview.png)

De **Query moet worden uitgevoerd** vak is wat de waarschuwing log-service wordt uitgevoerd. Als u weten wat de Waarschuwingsquery uitvoer kan zijn wilt voordat u de waarschuwing hebt gemaakt, kunt u uitvoeren de opgegeven query en de periode via de [analyseportal](../log-query/portals.md) of de [Tekstanalyse-API](https://docs.microsoft.com/rest/api/loganalytics/).

## <a name="log-alert-was-disabled"></a>Waarschuwing is uitgeschakeld

De volgende secties worden enkele redenen waarom Azure Monitor uitschakelen de [waarschuwingsregel](../platform/alerts-log.md).

### <a name="resource-where-the-alert-was-created-no-longer-exists"></a>Bron waar de waarschuwing is gemaakt niet meer bestaat

Meld u waarschuwingsregels die zijn gemaakt in Azure Monitor-doel van een specifieke bron, zoals een Azure Log Analytics-werkruimte, een Azure Application Insights-app en een Azure-resource. De waarschuwing log-service wordt vervolgens uitgevoerd voor een analytics-query die is opgegeven in de regel voor het opgegeven doel. Maar nadat de regel is gemaakt, gebruikers vaak gaat u naar van Azure, verwijderen of verplaatsen binnen Azure--het doel van de waarschuwingsregel. Omdat het doel van de waarschuwingsregel niet meer geldig is, mislukt de uitvoering van de regel.

In dergelijke gevallen worden Azure Monitor schakelt de waarschuwing en zorgt ervoor dat u niet in rekening onnodig gebracht bent wanneer de regel kan niet continu wordt uitgevoerd voor aanzienlijke periode (zoals een week). U vindt de precieze tijd wanneer Azure Monitor uitgeschakeld voor de waarschuwing via [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md). Een gebeurtenis in Azure-activiteitenlogboek wordt toegevoegd wanneer Azure Monitor de waarschuwingsregel schakelt.

Het volgende voorbeeld van de gebeurtenis in Azure-activiteitenlogboek is voor een waarschuwingsregel die is uitgeschakeld vanwege een continue fout.

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

### <a name="query-used-in-a-log-alert-is-not-valid"></a>Query die wordt gebruikt in een waarschuwing is niet geldig

Elke logboekwaarschuwingen in Azure Monitor is gemaakt als onderdeel van de configuratie moet een analytics-query die de waarschuwing service wordt periodiek uitgevoerd opgeven. De analytics-query mogelijk de juiste syntaxis op het moment van de regel voor het maken of bijwerken. Maar soms gedurende een bepaalde periode, de query die is opgegeven in de waarschuwingsregel kunt ontwikkelen syntaxisproblemen en ervoor zorgen dat de uitvoering van de regel wilt uitvoeren. Er zijn enkele veelvoorkomende redenen waarom een analytics-query die is opgegeven in een waarschuwingsregel met fouten ontwikkelen kunt:

- De query is geschreven naar [uitvoeren voor meerdere resources](../log-query/cross-workspace-query.md). En een of meer van de opgegeven resources niet meer bestaat.
- [Waarschuwing voor een meting van metrische gegevens type](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules) geconfigureerd heeft een waarschuwing query voldoet niet aan de normen syntaxis
- Er is geen gegevensstroom aan het analytics-platform. De [uitvoeren van query's biedt een fout](https://dev.loganalytics.io/documentation/Using-the-API/Errors) omdat er geen gegevens voor de opgegeven query.
- Wijzigingen in [querytaal](https://docs.microsoft.com/azure/kusto/query/) een herziene indeling voor opdrachten en functies bevatten. De opgegeven eerder in een waarschuwingsregel query is dus niet meer geldig.

[Azure Advisor](../../advisor/advisor-overview.md) waarschuwt u over dit probleem. Een aanbeveling is toegevoegd voor de specifieke waarschuwingsregel op Azure Advisor, onder de categorie van hoge beschikbaarheid met normale impact en een beschrijving van 'Herstellen de waarschuwingsregel voor de controle.' Als een waarschuwing query in de waarschuwingsregel is niet hersteld na Azure Advisor is een aanbeveling voor de zeven dagen, wordt Azure Monitor de waarschuwing uitschakelen en zorg ervoor dat u niet in rekening onnodig gebracht bent wanneer de regel kan niet continu wordt uitgevoerd voor een aanzienlijke periode ( Als een week).

U vindt de precieze tijd wanneer Azure Monitor de waarschuwingsregel uitgeschakeld door te zoeken naar een gebeurtenis in [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure](../platform/alerts-unified-log.md).
- Meer informatie over [Application Insights](../../azure-monitor/app/analytics.md).
- Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md).
