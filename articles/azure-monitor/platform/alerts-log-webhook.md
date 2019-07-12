---
title: Webhookacties voor logboekwaarschuwingen in Azure-waarschuwingen
description: In dit artikel wordt beschreven hoe u een waarschuwingsregel maken met behulp van de Log Analytics-werkruimte of Application Insights, hoe de waarschuwing gegevens als een HTTP-webhook en de details van de andere aanpassingen die mogelijk zijn gepusht.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 6aa007c621e76cb0c188a7dab6279fd9e387b2b3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705192"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Webhookacties voor waarschuwingsregels
Wanneer een [waarschuwing is gemaakt in Azure](alerts-log.md), hebt u de optie [configureren met behulp van actiegroepen](action-groups.md) een of meer acties uit te voeren. In dit artikel wordt beschreven welke verschillende webhookacties die beschikbaar zijn en laat zien hoe u een aangepaste JSON-indeling webhook configureren.

> [!NOTE]
> Ook kunt u de [algemene waarschuwing schema](https://aka.ms/commonAlertSchemaDocs) voor de webhook-integraties. Het algemene waarschuwing schema biedt het voordeel dat één uitbreidbaar en geïntegreerde waarschuwing nettolading voor alle waarschuwingen services in Azure Monitor. [Meer informatie over de algemene waarschuwing schemadefinities.](https://aka.ms/commonAlertSchemaDefinitions)

## <a name="webhook-actions"></a>Webhookacties

Met webhookacties, kunt u een extern proces via één HTTP POST-aanvraag aanroepen. De service met de naam moet ondersteuning van webhooks en bepalen hoe u een nettolading die wordt ontvangen.

Webhookacties vereisen de eigenschappen in de volgende tabel.

| Eigenschap | Description |
|:--- |:--- |
| **Webhook URL** |De URL van de webhook. |
| **Aangepaste JSON-nettolading** |De aangepaste nettolading om te verzenden met de webhook wanneer deze optie is gekozen tijdens het maken van waarschuwingen. Zie voor meer informatie, [waarschuwingen beheren](alerts-log.md).|

> [!NOTE]
> De **weergave Webhook** knop naast de **inclusief aangepaste JSON-payload voor webhook** optie voor de waarschuwing wordt weergegeven de nettolading van de voorbeeld-webhook voor de aanpassingen die is opgegeven. Geen daadwerkelijke gegevens bevat, maar is van het JSON-schema dat wordt gebruikt voor waarschuwingen. 

Webhooks omvatten een URL en een nettolading opgemaakt in JSON die de gegevens naar de externe service verzonden. De nettolading bevat standaard de waarden in de volgende tabel. U kunt deze nettolading vervangen door een aangepaste classificatie van uw eigen. In dat geval gebruikt u de variabelen in de tabel voor elk van de parameters om op te nemen van de waarden in de aangepaste nettolading.


| Parameter | Variabele | Description |
|:--- |:--- |:--- |
| *AlertRuleName* |#alertrulename |De naam van de waarschuwingsregel. |
| *Ernst* |#severity |Ernst instellen voor de geactiveerde waarschuwing. |
| *AlertThresholdOperator* |#thresholdoperator |Drempelwaarde voor operator voor de waarschuwingsregel maakt gebruik van groter dan of kleiner dan. |
| *AlertThresholdValue* |#thresholdvalue |Drempelwaarde voor de waarschuwingsregel. |
| *LinkToSearchResults* |#linktosearchresults |Koppeling naar de Analytics-portal waarmee de records van de query die de waarschuwing hebt gemaakt. |
| *ResultCount* |#searchresultcount |Het aantal records in de lijst met zoekresultaten. |
| *Eindtijd van Search Interval* |#searchintervalendtimeutc |Eindtijd van de query in UTC, met de indeling mm/dd/jjjj uu: mm: ss AM/PM. |
| *Interval voor zoeken* |#searchinterval |Tijdvenster voor de waarschuwingsregel, met de indeling uu: mm:. |
| *Search Interval StartTime* |#searchintervalstarttimeutc |Begintijd voor de query in UTC, met de indeling mm/dd/jjjj uu: mm: ss AM/PM. 
| *SearchQuery* |#searchquery |Zoekquery voor de logboekbestanden die worden gebruikt door de waarschuwingsregel. |
| *Zoekresultaten* |"IncludeSearchResults": true|Records die door de query wordt geretourneerd als een JSON-tabel, beperkt tot de eerste 1000 records als "IncludeSearchResults": true wordt toegevoegd aan de definitie van een aangepaste JSON-webhook als een eigenschap op het hoogste niveau. |
| *Waarschuwingstype*| #alerttype | Het type van de waarschuwingsregel geconfigureerd als [meting van metrische gegevens](alerts-unified-log.md#metric-measurement-alert-rules) of [aantal resultaten](alerts-unified-log.md#number-of-results-alert-rules).|
| *WorkspaceID* |#workspaceid |ID van uw Log Analytics-werkruimte. |
| *Toepassings-ID* |#applicationid |ID van uw Application Insights app. |
| *Subscription ID* |#subscriptionid |ID van uw Azure-abonnement gebruikt. 

> [!NOTE]
> *LinkToSearchResults* wordt doorgegeven parameters, zoals *SearchQuery*, *zoeken Interval StartTime*, en *zoeken Interval eindtijd* in de URL naar Azure Portal voor weergave in de sectie Analytics. De Azure-portal heeft een URI-limiet van ongeveer 2000 tekens. De portal wordt *niet* openen van koppelingen in waarschuwingen als de parameterwaarden voor de limiet overschrijdt. U kunt details van resultaten weergeven in de Analytics-portal handmatig invoeren. Of u kunt de [Application Insights Analytics REST-API](https://dev.applicationinsights.io/documentation/Using-the-API) of de [Log Analytics REST API](/rest/api/loganalytics/) voor het ophalen van resultaten via een programma. 

Bijvoorbeeld, kunt u de volgende aangepaste nettolading met één parameter met de naam opgeven *tekst*. De service die webhook aanroept wordt verwacht dat deze parameter.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Deze voorbeeld-nettolading wordt omgezet naar ongeveer als volgt wanneer deze wordt verzonden naar de webhook:

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Omdat alle variabelen in een aangepaste webhook moeten worden opgegeven in een JSON-behuizing, zoals "#searchinterval," de resulterende webhook heeft ook variabele gegevens binnen bijlagen, zoals "00: 05:00."

Zorg ervoor dat als u wilt opnemen zoekresultaten in een aangepaste payload, **IncludeSearchResults** is ingesteld als een eigenschap op het hoogste niveau in de JSON-nettolading. 

## <a name="sample-payloads"></a>Voorbeeld-nettoladingen
Deze sectie wordt beschreven voorbeeld nettoladingen voor webhooks voor waarschuwingen. De voorbeeld-nettoladingen bevatten voorbeelden wanneer de nettolading van de standard- en wanneer het aangepaste is.

### <a name="standard-webhook-for-log-alerts"></a>Standard webhook voor logboekwaarschuwingen 
Deze voorbeelden hebt een dummy-nettolading met slechts twee kolommen en twee rijen.

#### <a name="log-alert-for-log-analytics"></a>Waarschuwing voor Log Analytics
De voorbeeldnettolading van het volgende is voor een standaard webhookactie *zonder een aangepaste JSON-optie* die wordt gebruikt voor waarschuwingen op basis van Log Analytics:

```json
{
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": "log alert rule",
    "Severity": "Warning",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
 }
 ```

> [!NOTE]
> De waarde van het veld 'Prioriteit' kan worden gewijzigd als u hebt [overgeschakeld van uw voorkeur API](alerts-log-api-switch.md) voor waarschuwingen in Log Analytics.


#### <a name="log-alert-for-application-insights"></a>Waarschuwing voor Application Insights
De voorbeeldnettolading van het volgende is voor een standaard webhook *zonder een aangepaste JSON-optie* wanneer deze wordt gebruikt voor waarschuwingen op basis van Application Insights:
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"requests | where resultCode == \"500\"",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "3",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
    "AlertType": "Number of results"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Waarschuwing met aangepaste JSON-nettolading
Bijvoorbeeld, voor het maken van een aangepaste payload met alleen de naam van de waarschuwing en de lijst met zoekresultaten, kunt u het volgende: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

De voorbeeldnettolading van het volgende is voor een aangepaste webhookactie voor elke waarschuwing:
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](alerts-unified-log.md).
- Begrijpen hoe u [waarschuwingen in Azure beheren](alerts-log.md).
- Maken en beheren van [actiegroepen in Azure](action-groups.md).
- Meer informatie over [Application Insights](../../azure-monitor/app/analytics.md).
- Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md). 

