---
title: Antwoorden op waarschuwingen in de Azure Log Analytics | Microsoft Docs
description: Waarschuwingen in logboekanalyse kunnen belangrijke informatie in uw Azure-werkruimte te identificeren en proactief zullen u informeren over problemen of acties uit om te proberen op te lossen ze aanroepen.  In dit artikel beschrijft het maken van een waarschuwingsregel en details van de verschillende acties die ze kunnen ondernemen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2018
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 717adf1b19b9de8542ec507df3a01b187d0df8a5
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31521061"
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a>Acties toevoegen aan de regels voor waarschuwingen in Log Analytics

> [!NOTE]
> Waarschuwingen in logboekanalyse zijn [wordt uitgebreid naar Azure](../monitoring-and-diagnostics/monitoring-alerts-extend.md).  Waarschuwingen in Azure gebruik [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md) hun om acties te definiëren in plaats van de informatie in dit artikel.


Wanneer een [waarschuwing is gemaakt in logboekanalyse](log-analytics-alerts.md), hebt u de optie [configureren van de waarschuwingsregel](log-analytics-alerts.md) een of meer acties uit te voeren.  In dit artikel beschrijft de verschillende acties die beschikbaar zijn en meer informatie over het configureren van elk type.

| Bewerking | Beschrijving |
|:--|:--|
| [E-mail](#email-actions) | Verzend een e-mailbericht met de details van de waarschuwing naar een of meer geadresseerden. |
| [Webhook](#webhook-actions) | Een extern proces via één HTTP POST-aanvraag worden aangeroepen. |
| [Runbook](#runbook-actions) | Starten van een runbook in Azure Automation. |


## <a name="email-actions"></a>E-acties
E-acties verzend een e-mailbericht met de details van de waarschuwing naar een of meer geadresseerden.  U kunt het onderwerp van het e-mailbericht opgeven, maar de inhoud ervan is een standaardindeling die wordt samengesteld door Log Analytics.  Dit omvat samenvattende informatie zoals de naam van de waarschuwing naast de details van maximaal tien records geretourneerd door het logboek zoeken.  Het bevat ook een koppeling naar een zoekopdracht logboek logboekanalyse die de volledige set van records uit deze query retourneert.   De afzender van het e-mailbericht is *Microsoft Operations Management Suite-Team &lt; noreply@oms.microsoft.com &gt;* . 

E-bewerkingen moet de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Onderwerp |Onderwerpen in het e-mailbericht.  U kunt de hoofdtekst van het e-mailbericht niet wijzigen. |
| Geadresseerden |De adressen van alle e-mailgeadresseerden.  Als u meer dan één adres opgeeft, scheidt u de adressen van elkaar met een puntkomma (;). |


## <a name="webhook-actions"></a>Webhookacties

Webhookacties kunnen u een extern proces via één HTTP POST-aanvraag worden aangeroepen.  De service die wordt aangeroepen moet webhooks ondersteunen en bepalen hoe het maakt gebruik van elke nettolading van het ontvangt.  U kunt ook een REST-API die niet specifiek webhooks ondersteunt, zolang de aanvraag heeft een indeling die werkt met de API aanroepen.  Voorbeelden van het gebruik van een webhook in reactie op een waarschuwing een bericht verzendt [Slack](http://slack.com) of het maken van een incident in [PagerDuty](http://pagerduty.com/).  Een volledige procedure voor het maken van een waarschuwingsregel met een webhook aanroepen Slack is beschikbaar op [Webhooks in logboekanalyse waarschuwingen](log-analytics-alerts-webhooks.md).

Webhookacties vereisen de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Webhook-URL |De URL van de webhook. |
| Aangepaste JSON-nettolading |Aangepaste nettolading met de webhook moeten worden verzonden.  Zie hieronder voor meer informatie. |


Webhooks omvatten een URL en een nettolading opgemaakt in JSON is de gegevens naar de externe service verzonden.  De nettolading bevat standaard de waarden in de volgende tabel.  U kunt deze nettolading vervangen door een aangepaste van uzelf.  In dat geval kunt u de variabelen in de tabel voor elk van de parameters voor hun waarde bevatten in de aangepaste nettolading.


| Parameter | Variabele | Beschrijving |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Naam van de waarschuwingsregel. |
| AlertThresholdOperator |#thresholdoperator |Drempelwaarde voor de operator voor de waarschuwingsregel.  *Groter dan* of *minder dan*. |
| AlertThresholdValue |#thresholdvalue |Drempelwaarde voor de waarschuwingsregel. |
| LinkToSearchResults |#linktosearchresults |Koppelen aan logboekanalyse logboek zoeken waarmee de records geretourneerd van de query die de waarschuwing wordt gemaakt. |
| ResultCount |#searchresultcount |Het aantal records in de zoekresultaten. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |De eindtijd voor de query in UTC-notatie. |
| SearchIntervalInSeconds |#searchinterval |Tijdvenster voor de waarschuwingsregel. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Begintijd voor de query in UTC-notatie. |
| SearchQuery |#searchquery |Logboek zoekquery gebruikt door de waarschuwingsregel. |
| Zoekresultaten |Zie hieronder |Records geretourneerd door de query in JSON-indeling.  Beperkt tot de eerste 5000 records. |
| WorkspaceID |#workspaceid |ID van de werkruimte voor logboekanalyse. |

Bijvoorbeeld, kunt u opgeven de volgende aangepaste nettolading met een enkele parameter aangeroepen *tekst*.  De service die deze webhook aanroepen, zou deze parameter worden verwacht.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

De nettolading van dit voorbeeld zou worden omgezet naar ongeveer het volgende wanneer u naar de webhook verzonden.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Als u wilt opnemen zoekresultaten in een aangepaste nettolading, voeg de volgende regel als eigenschap op het hoogste niveau in de json-nettolading.  

    "IncludeSearchResults":true

Bijvoorbeeld: voor het maken van een aangepaste nettolading met alleen de naam van de waarschuwing en de zoekresultaten, u kunt de volgende. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


U kunt een compleet voorbeeld van een waarschuwingsregel maken met een webhook om een externe service op te starten doorlopen [maken van een actie waarschuwing webhook in logboekanalyse bericht te verzenden naar Slack](log-analytics-alerts-webhooks.md).


## <a name="runbook-actions"></a>Runbook-acties
Runbook-acties starten van een runbook in Azure Automation.  Om dit type actie gebruiken, hebt u de [Automation-oplossing](log-analytics-add-solutions.md) geïnstalleerd en geconfigureerd in de werkruimte voor logboekanalyse.  U kunt kiezen uit de runbooks in het automation-account die u hebt geconfigureerd in de Automation-oplossing.

Runbook-bewerkingen moet de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:---|
| Runbook | Een Runbook dat u starten wilt wanneer een waarschuwing wordt gemaakt. |
| Voer op | Geef **Azure** voor het uitvoeren van het runbook in de cloud.  Geef **hybride worker** voor het uitvoeren van het runbook op een agent met [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) geïnstalleerd.  |

Runbook-acties, start het runbook met behulp van een [webhook](../automation/automation-webhooks.md).  Wanneer u de waarschuwingsregel maakt, wordt automatisch een nieuwe webhook voor het runbook gemaakt met de naam **OMS waarschuwing herstel** gevolgd door een GUID.  

U kan niet rechtstreeks vullen met de parameters van het runbook, maar de [$WebhookData parameter](../automation/automation-webhooks.md) bevat details van de waarschuwing, met inbegrip van de resultaten van de zoekopdracht logboek waarvan deze is gemaakt.  Het runbook moet definiëren **$WebhookData** als parameter voor deze toegang krijgt tot de eigenschappen van de waarschuwing.  Gegevens van de waarschuwing is beschikbaar in json-indeling in één eigenschap aangeroepen **SearchResult** (voor runbook acties en webhookacties met standaard nettolading) of **zoekresultaten** (webhookacties met aangepaste nettolading inclusief **IncludeSearchResults ': true**) in de **RequestBody** eigenschap van **$WebhookData**.  Dit heeft met de eigenschappen in de volgende tabel.

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de nettolading van de runbook is gewijzigd.  Details van de indeling zijn te vinden in [Azure Log Analytics REST API](https://aka.ms/loganalyticsapiresponse).  U ziet een voorbeeld in [voorbeelden](#sample-payload) hieronder.  

| Knooppunt | Beschrijving |
|:--- |:--- |
| id |Pad en de GUID van de zoekopdracht. |
| __metadata |Informatie over de waarschuwing waaronder het aantal records en status van de zoekresultaten. |
| waarde |Afzonderlijke vermelding voor elke record in de zoekresultaten.  De details van de vermelding overeen met de eigenschappen en waarden van de record. |

De volgende runbooks zou bijvoorbeeld uitpakken van de records geretourneerd door het logboek zoeken en toewijzen van andere eigenschappen op basis van het type van elke record.  Denk eraan dat het runbook met het converteren van begint **RequestBody** van json zodanig dat deze kan worden gewerkt met als een object in PowerShell.

>[!NOTE]
> Maakt gebruik van dit runbook **SearchResult** die is de eigenschap die de resultaten voor runbook-activiteiten en webhookacties met standaard nettolading bevat.  Als het runbook is aangeroepen vanuit een webhook antwoord aan de hand van een aangepaste nettolading, moet u deze eigenschap te wijzigen **zoekresultaten**.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody

    # Get all metadata properties    
    $AlertRuleName = $RequestBody.AlertRuleName
    $AlertThresholdOperator = $RequestBody.AlertThresholdOperator
    $AlertThresholdValue = $RequestBody.AlertThresholdValue
    $AlertDescription = $RequestBody.Description
    $LinktoSearchResults =$RequestBody.LinkToSearchResults
    $ResultCount =$RequestBody.ResultCount
    $Severity = $RequestBody.Severity
    $SearchQuery = $RequestBody.SearchQuery
    $WorkspaceID = $RequestBody.WorkspaceId
    $SearchWindowStartTime = $RequestBody.SearchIntervalStartTimeUtc
    $SearchWindowEndTime = $RequestBody.SearchIntervalEndtimeUtc
    $SearchWindowInterval = $RequestBody.SearchIntervalInSeconds

    # Get detailed search results
    if($RequestBody.SearchResult -ne $null)
    {
        $SearchResultRows    = $RequestBody.SearchResult.tables[0].rows 
        $SearchResultColumns = $RequestBody.SearchResult.tables[0].columns;

        foreach ($SearchResultRow in $SearchResultRows)
        {   
            $Column = 0
            $Record = New-Object –TypeName PSObject 
        
            foreach ($SearchResultColumn in $SearchResultColumns)
            {
                $Name = $SearchResultColumn.name
                $ColumnValue = $SearchResultRow[$Column]
                $Record | Add-Member –MemberType NoteProperty –Name $name –Value $ColumnValue -Force
                        
                $Column++
            }

            # Include code to work with the record. 
            # For example $Record.Computer to get the computer property from the record.
            
        }
    }



## <a name="sample-payload"></a>Voorbeeld-nettolading
Deze sectie vindt voorbeeld nettolading voor webhook en runbook-acties.

### <a name="webhook-actions"></a>Webhookacties
In dit voorbeeld wordt **SearchResult** die is de eigenschap die de resultaten voor webhookacties met standaard nettolading bevat.  Als u de webhook gebruikt een aangepaste nettolading met zoekresultaten, zijn deze eigenschap **zoekresultaten**.

Hier volgt een voorbeeld-nettolading voor een webhook-actie.

    {
    "WorkspaceId": "workspaceID",
    "AlertRuleName": "WebhookAlert",
    "SearchQuery": "Usage",
    "SearchResult": {
        "tables": [
        {
            "name": "PrimaryResult",
            "columns": [
            {
                "name": "TenantId",
                "type": "string"
            },
            {
                "name": "Computer",
                "type": "string"
            },
            {
                "name": "TimeGenerated",
                "type": "datetime"
            },
            {
                "name": "SourceSystem",
                "type": "string"
            },
            {
                "name": "StartTime",
                "type": "datetime"
            },
            {
                "name": "EndTime",
                "type": "datetime"
            },
            {
                "name": "ResourceUri",
                "type": "string"
            },
            {
                "name": "LinkedResourceUri",
                "type": "string"
            },
            {
                "name": "DataType",
                "type": "string"
            },
            {
                "name": "Solution",
                "type": "string"
            },
            {
                "name": "BatchesWithinSla",
                "type": "long"
            },
            {
                "name": "BatchesOutsideSla",
                "type": "long"
            },
            {
                "name": "BatchesCapped",
                "type": "long"
            },
            {
                "name": "TotalBatches",
                "type": "long"
            },
            {
                "name": "AvgLatencyInSeconds",
                "type": "real"
            },
            {
                "name": "Quantity",
                "type": "real"
            },
            {
                "name": "QuantityUnit",
                "type": "string"
            },
            {
                "name": "IsBillable",
                "type": "bool"
            },
            {
                "name": "MeterId",
                "type": "string"
            },
            {
                "name": "LinkedMeterId",
                "type": "string"
            },
            {
                "name": "Type",
                "type": "string"
            }
            ],
            "rows": [
            [
                "workspaceID",
                "-",
                "2017-09-26T13:59:59Z",
                "OMS",
                "2017-09-26T13:00:00Z",
                "2017-09-26T13:59:59Z",
                "/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.operationalinsights/workspaces/workspace-workspaceID",
                null,
                "Operation",
                "LogManagement",
                8,
                0,
                0,
                8,
                0,
                0.002502,
                "MBytes",
                false,
                "a4e29a95-5b4c-408b-80e3-113f9410566e",
                "00000000-0000-0000-0000-000000000000",
                "Usage"
            ]
            ]
        }
        ]
    },
    "SearchIntervalStartTimeUtc": "2017-09-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2017-09-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 1,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2017-09-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }


### <a name="runbooks"></a>Runbooks

Hier volgt een voorbeeld-nettolading voor een runbook-actie.

    {
    "WorkspaceId": "workspaceID",
    "AlertRuleName": "AutomationAlert",
    "SearchQuery": "Usage",
    "SearchResult": {
        "tables": [
        {
            "name": "PrimaryResult",
            "columns": [
            {
                "name": "TenantId",
                "type": "string"
            },
            {
                "name": "Computer",
                "type": "string"
            },
            {
                "name": "TimeGenerated",
                "type": "datetime"
            },
            {
                "name": "SourceSystem",
                "type": "string"
            },
            {
                "name": "StartTime",
                "type": "datetime"
            },
            {
                "name": "EndTime",
                "type": "datetime"
            },
            {
                "name": "ResourceUri",
                "type": "string"
            },
            {
                "name": "LinkedResourceUri",
                "type": "string"
            },
            {
                "name": "DataType",
                "type": "string"
            },
            {
                "name": "Solution",
                "type": "string"
            },
            {
                "name": "BatchesWithinSla",
                "type": "long"
            },
            {
                "name": "BatchesOutsideSla",
                "type": "long"
            },
            {
                "name": "BatchesCapped",
                "type": "long"
            },
            {
                "name": "TotalBatches",
                "type": "long"
            },
            {
                "name": "AvgLatencyInSeconds",
                "type": "real"
            },
            {
                "name": "Quantity",
                "type": "real"
            },
            {
                "name": "QuantityUnit",
                "type": "string"
            },
            {
                "name": "IsBillable",
                "type": "bool"
            },
            {
                "name": "MeterId",
                "type": "string"
            },
            {
                "name": "LinkedMeterId",
                "type": "string"
            },
            {
                "name": "Type",
                "type": "string"
            }
            ],
            "rows": [
            [
                "861bd466-5400-44be-9552-5ba40823c3aa",
                "-",
                "2017-09-26T13:59:59Z",
                "OMS",
                "2017-09-26T13:00:00Z",
                "2017-09-26T13:59:59Z",
            "/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.operationalinsights/workspaces/workspace-861bd466-5400-44be-9552-5ba40823c3aa",
                null,
                "Operation",
                "LogManagement",
                8,
                0,
                0,
                8,
                0,
                0.002502,
                "MBytes",
                false,
                "a4e29a95-5b4c-408b-80e3-113f9410566e",
                "00000000-0000-0000-0000-000000000000",
                "Usage"
            ]
            ]
        }
        ]
    },
    "SearchIntervalStartTimeUtc": "2017-09-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2017-09-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 1,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2017-09-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Critical"
    }



## <a name="next-steps"></a>Volgende stappen
- Een stapsgewijze Kennismaking voltooien [configureren van een webook](log-analytics-alerts-webhooks.md) met een waarschuwingsregel.  
- Meer informatie over het schrijven van [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) oplossen van problemen die worden aangeduid met waarschuwingen.
