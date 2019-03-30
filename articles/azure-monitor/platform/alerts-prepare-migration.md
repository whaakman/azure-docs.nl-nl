---
title: Voorbereiden voor migratie van Azure Monitor-klassieke waarschuwingen door bij te werken uw logic apps en runbooks
description: Informatie over het wijzigen van de webhook, logische app en runbooks om voor te bereiden voor migratie op vrijwillige basis.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 3c47404826d5055d4a82d4842523f790fb11f000
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632102"
---
# <a name="prepare-your-logic-apps-and-run-books-for-classic-alert-rules-migration"></a>Voorbereiden van uw logische apps en boeken voor klassieke waarschuwingsregels migratie uitvoeren

Als [eerder aangekondigd](monitoring-classic-retirement.md), klassieke waarschuwingen in Azure Monitor worden stopgezet in juli 2019. Het hulpprogramma voor migratie voor het activeren van migratie vrijwillig is beschikbaar in Azure portal en is beschikbaar voor klanten die gebruikmaken van klassieke waarschuwingsregels.

Als u ervoor uw klassieke waarschuwingsregels vrijwillig migreren naar nieuwe regels voor waarschuwingen kiest, zijn er enkele verschillen tussen de twee systemen die u moet rekening houden met. In dit artikel begeleidt u stapsgewijs door van de verschillen tussen de twee systemen en hoe u kunt voorbereiden voor de wijziging.

## <a name="api-changes"></a>API-wijzigingen

De API's gebruikt voor het maken/beheren van klassieke waarschuwingsregels (`microsoft.insights/alertrules`) verschillen van de API's gebruikt voor het maken/beheren van nieuwe metrische waarschuwingen (`microsoft.insights/metricalerts`). Als u via een programma maken / klassieke waarschuwingsregels vandaag beheren, werkt u uw implementatiescripts om te werken met nieuwe API's.

De volgende tabel bevat een verwijzing naar programmatische interfaces voor zowel klassieke als nieuwe waarschuwingen.

|         |Klassieke waarschuwingen  |Nieuwe metrische waarschuwingen |
|---------|---------|---------|
|REST-API     | [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](https://docs.microsoft.com/rest/api/monitor/metricalerts)       |
|Azure-CLI     | [AZ monitor voor waarschuwing](https://docs.microsoft.com/cli/azure/monitor/alert?view=azure-cli-latest)        | [AZ monitor metrics waarschuwing](https://docs.microsoft.com/cli/azure/monitor/metrics/alert?view=azure-cli-latest)        |
|PowerShell      | [Naslaginformatie](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule)       |      |
| Azure Resource Manager-sjabloon | [Voor klassieke waarschuwingen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-enable-template)|[Voor nieuwe metrische waarschuwingen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates)|

## <a name="notification-payload-changes"></a>Melding nettolading wijzigt

De indeling van de melding nettolading is enigszins tussen [klassieke waarschuwingsregels](alerts-webhooks.md) en [nieuwe metrische waarschuwingen](alerts-metric-near-real-time.md#payload-schema). Als u een webhook, logische app hebt of runbook-acties die wordt geactiveerd door regels voor klassieke waarschuwingen, moet u deze meldingseindpunten voor het accepteren van de indeling van de nettolading van de nieuwe metrische waarschuwingen bijwerken.

De volgende tabel kunt u de velden tussen klassieke waarschuwingsregel webhookpayload en de nieuwe metrische waarschuwing webhookpayload toewijzen.

|  |Klassieke waarschuwingen  |Nieuwe metrische waarschuwingen |
|---------|---------|---------|
|De waarschuwing is geactiveerd of opgelost     | status       | data.status |
|Contextuele informatie over de waarschuwing     | context        | data.context        |
|Tijdstempel die de waarschuwing is geactiveerd of opgelost      | context.timestamp       | data.context.timestamp        |
| Waarschuwingsregel-ID | context.id | data.context.id |
| Naam waarschuwingsregel | context.name | Data.context.name |
| Beschrijving van de waarschuwingsregel | context.description | data.context.description |
| Waarschuwingsregel voorwaarde | context.condition | data.context.condition|
| Naam van de meetwaarde | context.condition.metricName| data.context.condition.allOf[0].metricName|
| Tijdverzameling (hoe de metrische gegevens over de evaluatie-venster wordt geaggregeerd)|data.context.condition.timeAggregation|data.context.condition.timeAggregation|
| De evaluatieperiode voor | context.condition.windowSize | data.context.condition.windowSize|
| Operator (hoe de samengevoegde metrische waarde wordt vergeleken met de drempelwaarde) | context.condition.operator | data.context.condition.operator|
| Drempelwaarde | context.condition.threshold| data.context.condition.allOf[0].threshold|
| Metrische waarde | context.condition.metricValue | data.context.condition.allOf[0].metricValue|
| Abonnements-id | context.subscriptionId | data.context.subscriptionId|
| Resourcegroep van de betrokken resource | context.resourceGroup | data.context.resourceGroup|
| Naam van de betrokken resource | context.resourceName | data.context.resourceName |
| Type van de betrokken resource | context.resourceType | data.context.resourceType |
|  Resource-ID van de betreffende resource | context.resourceId | data.context.resourceId |
| Een directe koppeling naar de pagina overzicht van portal-resource | context.portalLink | data.context.portalLink|
| De nettolading van de aangepaste velden die moeten worden doorgegeven aan een webhook of de logische app | properties |Data.Properties |

Zoals u ziet, zijn beide de nettoladingen zijn vergelijkbaar. Volgende sectie bevat informatie over de voorbeeld-logic apps en een voorbeeldrunbook parseren van de nettolading van de melding voor nieuwe waarschuwingen.

## <a name="using-a-logic-app-that-receives-a-metric-alert-notification"></a>Met behulp van een logische app die u een metrische waarschuwing ontvangt

Als u van logische apps met klassieke waarschuwingen gebruikmaakt, moet u uw logische app voor het parseren van de nettolading van de nieuwe metrische waarschuwingen wijzigen.

1. Maak een nieuwe logische app.

2. Gebruik de sjabloon 'Azure Monitor--metrische waarschuwing Handler'. Deze sjabloon bevat een **HTTP-aanvraag** trigger met het bijbehorende schema gedefinieerd

    ![logica-app-sjabloon](media/alerts-migration/logic-app-template.png "metrische waarschuwing sjabloon")

3. Een actie voor het hosten van uw verwerkingslogica toevoegen.

## <a name="using-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Met behulp van een automation-runbook dat een metrische waarschuwing melding ontvangt

Het volgende voorbeeld biedt een PowerShell-code die kan worden gebruikt in uw runbook die de nettoladingen voor zowel klassieke waarschuwingsregels voor metrische gegevens en nieuwe waarschuwingsregels voor metrische gegevens kan worden geparseerd.

```PS
## Sample PowerShell code to be used in a runbook to handle parsing of both classic and new metric alerts

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData.
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Identify if the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new Metric Alert schema
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic Metric Alert schema
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is not either a classic metric alert or a new metric alert
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    #parse fields related to resource affected
    $ResourceName = $AlertContext.resourceName
    $ResourceType = $AlertContext.resourceType
    $ResourceGroupName = $AlertContext.resourceGroupName
    $ResourceId = $AlertContext.resourceId
    $SubId = $AlertContext.subscriptionId

    ## Your logic to handle the alert here.
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}

```

Een volledig voorbeeld van een runbook waarmee een virtuele machine wordt gestopt wanneer een waarschuwing wordt geactiveerd [documentatie over Azure Automation](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook).

## <a name="partner-integration-via-webhooks"></a>Partnerintegratie via webhooks

De meeste van [onze partners die zijn geïntegreerd met klassieke waarschuwingen](https://docs.microsoft.com/azure/azure-monitor/platform/partners) al ondersteuning voor nieuwere metrische waarschuwingen via hun integraties. Bekende integraties gebruikt die al met nieuwe metrische waarschuwingen werken worden hieronder vermeld.

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Signl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Als u een partnerintegratie die niet wordt vermeld hier gebruikt, Controleer of met de provider van de integratie van de integratie werkt met nieuwe metrische waarschuwingen.

## <a name="next-steps"></a>Volgende stappen

- [Het gebruik van het hulpprogramma voor migratie](alerts-using-migration-tool.md)
- [Informatie over de werking van het hulpprogramma voor migratie](alerts-understand-migration.md)
