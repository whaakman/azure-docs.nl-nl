---
title: Webhookacties voor logboek-waarschuwingen in Azure waarschuwingen (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe naar een waarschuwingsregel logboek met behulp van log analytics of application insights, zendt de gegevens als HTTP-webhook en details van de andere aanpassingen mogelijk.
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/2/2018
ms.author: vinagara
ms.openlocfilehash: 5852eb099f6620656aa69e5085447c2a8b4e0c01
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="webhook-actions-for-log-alert-rules"></a>Webhookacties voor waarschuwingsregels logboek
Wanneer een [waarschuwing is gemaakt in Azure (Preview)](monitor-alerts-unified-usage.md), hebt u de optie [configureren met de actiegroepen](monitoring-action-groups.md) een of meer acties uit te voeren.  In dit artikel beschrijft de verschillende webhookacties die beschikbaar zijn en de details over het configureren van de aangepaste JSON-indeling webhook.


## <a name="webhook-actions"></a>Webhookacties

Webhookacties kunnen u een extern proces via één HTTP POST-aanvraag worden aangeroepen.  De service die wordt aangeroepen moet webhooks ondersteunen en bepalen hoe het maakt gebruik van elke nettolading van het ontvangt.   Voorbeelden van het gebruik van een webhook in reactie op een waarschuwing een bericht verzendt [Slack](http://slack.com) of het maken van een incident in [PagerDuty](http://pagerduty.com/).  

Webhookacties vereisen de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--- |:--- |
| Webhook-URL |De URL van de webhook. |
| Aangepaste JSON-nettolading |Aangepaste nettolading verzenden met de webhook wanneer deze optie is gekozen tijdens het maken van waarschuwingen. Details beschikbaar op [waarschuwingen met behulp van Azure-waarschuwingen (Preview) beheren](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Webhook-knop naast testen *aangepaste JSON-nettolading opnemen voor webhook* optie voor logboek waarschuwing, dummy-aanroep voor het testen van de webhook-URL wordt geactiveerd. Het bevat geen actuele gegevens en vertegenwoordiger van JSON-schema gebruikt voor logboek-waarschuwingen. 

Webhooks omvatten een URL en een nettolading opgemaakt in JSON is de gegevens naar de externe service verzonden.  De nettolading bevat standaard de waarden in de volgende tabel: U kunt deze nettolading vervangen door een aangepaste van uzelf.  In dat geval kunt u de variabelen in de tabel voor elk van de parameters voor hun waarde bevatten in de aangepaste nettolading.


| Parameter | Variabele | Beschrijving |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Naam van de waarschuwingsregel. |
| Ernst |#severity |Ernst instellen voor de waarschuwing gestarte logboek. |
| AlertThresholdOperator |#thresholdoperator |Drempelwaarde voor de operator voor de waarschuwingsregel.  *Groter dan* of *minder dan*. |
| AlertThresholdValue |#thresholdvalue |Drempelwaarde voor de waarschuwingsregel. |
| LinkToSearchResults |#linktosearchresults |Koppelen aan logboekanalyse logboek zoeken waarmee de records geretourneerd van de query die de waarschuwing wordt gemaakt. |
| ResultCount |#searchresultcount |Het aantal records in de zoekresultaten. |
| Eindtijd van Interval zoeken |#searchintervalendtimeutc |De eindtijd voor de query in UTC-notatie. |
| Interval voor zoeken |#searchinterval |Tijdvenster voor de waarschuwingsregel. |
| Zoeken Interval StartTime |#searchintervalstarttimeutc |Begintijd voor de query in UTC-notatie. 
| SearchQuery |#searchquery |Logboek zoekquery gebruikt door de waarschuwingsregel. |
| Zoekresultaten |'IncludeSearchResults': true|Records dat wordt geretourneerd door de query als een JSON-tabel, beperkt tot de eerste 1000 records; Als 'IncludeSearchResults': true is toegevoegd in de aangepaste JSON-webhook definitie als eigenschap op het hoogste niveau. |
| WorkspaceID |#workspaceid |ID van de werkruimte voor logboekanalyse (OMS). |
| Toepassings-id |#applicationid |ID van uw toepassing inzicht app. |
| Abonnements-id |#subscriptionid |ID van uw Azure-abonnement gebruikt met Application Insights. 


Bijvoorbeeld, kunt u opgeven de volgende aangepaste nettolading met een enkele parameter aangeroepen *tekst*.  De service die deze webhook aanroepen, zou deze parameter worden verwacht.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

De nettolading van dit voorbeeld zou worden omgezet naar ongeveer het volgende wanneer u naar de webhook verzonden.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Zorg ervoor dat zodanig zoekresultaten in een aangepaste nettolading **IncudeSearchResults** is ingesteld als eigenschap op het hoogste niveau in de json-nettolading. 

## <a name="sample-payloads"></a>Voorbeeld-nettoladingen
Deze sectie vindt u voorbeeld nettolading voor webhook voor logboek-meldingen, inclusief wanneer de nettolading standaard is en wanneer de aangepast.

> [!NOTE]
> Compatibiliteit met eerdere versies, zodat de nettolading van de standaard webhook voor waarschuwingen met behulp van Azure-logboekanalyse is hetzelfde als [OMS waarschuwing management](../log-analytics/log-analytics-alerts-creating.md). Maar voor logboek-waarschuwingen met [Application Insights](../application-insights/app-insights-analytics.md), de nettolading van de standaard webhook is gebaseerd op schema van de groep acties.

### <a name="standard-webhook-for-log-alerts"></a>Standaard Webhook voor logboek-waarschuwingen 
Beide voorbeelden hebben een dummy-nettolading met slechts twee kolommen en twee rijen aangegeven.

#### <a name="log-alert-for-azure-log-analytics"></a>Logboek-waarschuwing voor Azure-logboekanalyse
Hieronder volgt een voorbeeld-nettolading voor een actie standaard webhook *zonder aangepaste Json-optie* wordt gebruikt voor log analytics gebaseerde waarschuwingen.

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
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
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
    


#### <a name="log-alert-for-azure-application-insights"></a>Waarschuwing voor Azure Application Insights logboek
Hieronder volgt een voorbeeld-nettolading voor een standaard webhook *zonder aangepaste Json-optie* als gebruikt voor application insights gebaseerde logboek-waarschuwingen.
    

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
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Error"
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
    }
    }


#### <a name="log-alert-with-custom-json-payload"></a>Meld u waarschuwing met aangepaste JSON-nettolading
Bijvoorbeeld: voor het maken van een aangepaste nettolading met alleen de naam van de waarschuwing en de zoekresultaten, u kunt het volgende: 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }

Hier volgt een voorbeeld-nettolading voor een aangepaste webhook-actie voor een logboek-waarschuwing.
    

    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
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




## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [logboek waarschuwingen in de Azure-waarschuwingen (preview)](monitor-alerts-unified-log.md)
- Maken en beheren van [actiegroepen in Azure](monitoring-action-groups.md)
- Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md)
- Meer informatie over [logboekanalyse](../log-analytics/log-analytics-overview.md). 