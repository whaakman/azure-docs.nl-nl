---
title: Webhookacties voor logboekwaarschuwingen in Azure-waarschuwingen
description: Dit artikel wordt beschreven hoe naar een waarschuwingsregel met behulp van log analytics of application insights, gegevens worden gepusht als HTTP-webhook en details van de verschillende aanpassingen mogelijk.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: a592dc150d535df775a62c157f76f327b54240fd
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496085"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Webhookacties voor waarschuwingsregels
Wanneer een [waarschuwing is gemaakt in Azure](alerts-log.md), hebt u de optie [configureren met behulp van actiegroepen](action-groups.md) een of meer acties uit te voeren.  In dit artikel beschrijft de verschillende webhookacties die beschikbaar zijn en meer informatie over het configureren van de aangepaste JSON-indeling webhook.


## <a name="webhook-actions"></a>Webhookacties

Webhookacties kunnen u een extern proces via één HTTP POST-aanvraag aanroepen.  De service die wordt aangeroepen moet ondersteuning van webhooks en bepalen hoe het maakt gebruik van elke nettolading wordt ontvangen.    

Webhookacties vereisen de eigenschappen in de volgende tabel:

| Eigenschap | Description |
|:--- |:--- |
| Webhook-URL |De URL van de webhook. |
| Aangepaste JSON-nettolading |Aangepaste nettolading om te verzenden met de webhook wanneer deze optie is gekozen tijdens het maken van waarschuwingen. Details beschikbaar op [waarschuwingen beheren](alerts-log.md) |

> [!NOTE]
> Webhook-knop naast weergave *inclusief aangepaste JSON-payload voor webhook* optie voor melding logboek, webhook voorbeeldnettolading voor aanpassing van de opgegeven worden weergegeven. Deze bevat geen actuele gegevens en een vertegenwoordiger van JSON-schema voor Logboekwaarschuwingen gebruikt. 

Webhooks omvatten een URL en een nettolading opgemaakt in JSON die de gegevens zijn verzonden naar de externe service.  Standaard bevat de nettolading van de waarden in de volgende tabel:  U kunt deze nettolading vervangen door een aangepaste classificatie van uw eigen.  In dat geval kunt u de variabelen in de tabel voor elk van de parameters om op te nemen van de waarde in de aangepaste nettolading.


| Parameter | Variabele | Description |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |De naam van de waarschuwingsregel. |
| Severity |#severity |Ernst instellen voor de geactiveerde waarschuwing. |
| AlertThresholdOperator |#thresholdoperator |Drempelwaarde voor operator voor de waarschuwingsregel.  *Groter dan* of *minder dan*. |
| AlertThresholdValue |#thresholdvalue |Drempelwaarde voor de waarschuwingsregel. |
| LinkToSearchResults |#linktosearchresults |Koppeling naar de Analytics-portal waarmee de records van de query die de waarschuwing hebt gemaakt. |
| ResultCount |#searchresultcount |Het aantal records in de lijst met zoekresultaten. |
| Eindtijd van Search Interval |#searchintervalendtimeutc |Eindtijd van de query in UTC, format - mm/dd/jjjj uu: mm: ss AM/PM. |
| Interval voor zoeken |#searchinterval |Tijdvenster voor de waarschuwing sluiten, indeling - uu: mm:. |
| Search Interval StartTime |#searchintervalstarttimeutc |Begintijd voor de query in UTC, format - mm/dd/jjjj uu: mm: ss AM/PM... 
| SearchQuery |#searchquery |Zoekquery voor de logboekbestanden die worden gebruikt door de waarschuwingsregel. |
| Zoekresultaten |"IncludeSearchResults": true|Records die door de query wordt geretourneerd als een JSON-tabel, beperkt tot de eerste 1000 records. Als "IncludeSearchResults": true wordt toegevoegd aan de aangepaste JSON-webhook-definitie als een eigenschap op het hoogste niveau. |
| WorkspaceID |#workspaceid |ID van uw Log Analytics-werkruimte. |
| Toepassings-id |#applicationid |ID van uw Application Insights app. |
| Abonnements-id |#subscriptionid |ID van uw Azure-abonnement gebruikt met Application Insights. 

> [!NOTE]
> LinkToSearchResults doorgegeven parameters, zoals SearchQuery, Search Interval StartTime & zoeken Interval eindtijd in de URL naar Azure-portal voor weergave in de sectie Analytics. Azure-portal heeft URI hoe groot de limiet van ongeveer 2000 tekens en wordt *niet* koppeling openen in waarschuwingen, opgegeven als parameterwaarden de genoemde limiet overschrijdt. Gebruikers kunnen handmatig invoeren van details van resultaten weergeven in Analytics-portal of gebruik de [Application Insights Analytics REST-API](https://dev.applicationinsights.io/documentation/Using-the-API) of [Log Analytics REST API](/rest/api/loganalytics/) voor het ophalen van resultaten via een programma 

Bijvoorbeeld, kunt u de volgende aangepaste nettolading met één parameter met de naam opgeven *tekst*.  De service die webhook aanroept zou deze parameter wordt verwacht.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Deze nettolading voorbeeld zou worden omgezet naar ongeveer als volgt wanneer verzonden naar de webhook.

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Als u alle variabelen in een aangepaste webhook hebt opgegeven in de JSON-behuizing, zoals '#searchinterval', de resulterende webhook variabele gegevens binnen de behuizing, zoals ook heeft "00: 05:00 '.

Zorg ervoor dat als u wilt opnemen zoekresultaten in een aangepaste payload, **IncludeSearchResults** is ingesteld als een eigenschap op het hoogste niveau in de json-nettolading. 

## <a name="sample-payloads"></a>Voorbeeld-nettoladingen
In deze sectie wordt een voorbeeld van payload voor webhook voor Logboekwaarschuwingen, met inbegrip van wanneer de nettolading van standard is en wanneer de aangepaste.

### <a name="standard-webhook-for-log-alerts"></a>Standard Webhook voor Logboekwaarschuwingen 
Deze voorbeelden hebben een dummy-nettolading met slechts twee kolommen en twee rijen vermeld.

#### <a name="log-alert-for-azure-log-analytics"></a>Waarschuwing voor Azure-logboekanalyse
Hieronder volgt een voorbeeld-nettolading voor een standaard webhookactie *zonder aangepaste Json-optie* wordt gebruikt voor log analytics-waarschuwingen.

```json
{
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
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
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Warning"
 }
 ```

> [!NOTE]
> De veldwaarde ernst kan worden gewijzigd als u hebt [overgeschakeld van uw voorkeur API](alerts-log-api-switch.md) voor waarschuwingen in Log Analytics.


#### <a name="log-alert-for-azure-application-insights"></a>Waarschuwing voor Azure Application Insights
Hieronder volgt een voorbeeld-nettolading voor een standaard webhook *zonder aangepaste Json-optie* wanneer die wordt gebruikt voor application insights-waarschuwingen op basis van log-.
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
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
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "3",
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
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

Hieronder volgt een voorbeeld-nettolading voor een aangepaste webhookactie voor elke waarschuwing.
    
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
- Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](alerts-unified-log.md)
- Inzicht in [waarschuwingen in Azure beheren](alerts-log.md)
- Maken en beheren van [actiegroepen in Azure](action-groups.md)
- Meer informatie over [Application Insights](../../azure-monitor/app/analytics.md)
- Meer informatie over [Log Analytics](../../azure-monitor/log-query/log-query-overview.md). 

