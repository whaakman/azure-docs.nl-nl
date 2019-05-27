---
title: Voorbereiden voor migratie van Azure Monitor-klassieke waarschuwingen door bij te werken uw logic apps en runbooks
description: Informatie over het wijzigen van uw webhooks, logic apps en runbooks om voor te bereiden voor migratie op vrijwillige basis.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: bdbd45c2b10dec8f1c0a85110747a470e818dbf9
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66015613"
---
# <a name="prepare-your-logic-apps-and-runbooks-for-migration-of-classic-alert-rules"></a>Uw logic apps en runbooks voorbereiden voor migratie van klassiek waarschuwingsregels

Als [eerder aangekondigd](monitoring-classic-retirement.md), klassieke waarschuwingen in Azure Monitor worden stopgezet in September 2019 (oorspronkelijk juli 2019 is). Een hulpprogramma voor migratie is beschikbaar in de Azure-portal voor klanten die gebruik van klassieke waarschuwingsregels en willen voor het activeren van de migratie zelf.

> [!NOTE]
> Vanwege vertragingen in het uitrollen van hulpprogramma voor migratie, is de vervaldatum voor de migratie van de klassieke waarschuwingen zijn uitgebreid ter 31 augustus 2019 vanaf de oorspronkelijk aangekondigde datum van 30 juni 2019.

Als u kiest voor het migreren van de klassieke waarschuwingsregels vrijwillig voor nieuwe regels voor waarschuwingen, er rekening mee dat er enkele verschillen tussen de twee systemen zijn. In dit artikel wordt uitgelegd die verschillen en hoe u kunt voorbereiden voor de wijziging.

## <a name="api-changes"></a>API-wijzigingen

De API's die bij het maken en beheren van klassieke waarschuwingsregels (`microsoft.insights/alertrules`) verschillen van de API's die nieuwe metrische waarschuwingen maken en beheren (`microsoft.insights/metricalerts`). Als via een programma maken en beheren van klassieke waarschuwingsregels vandaag, werkt u uw implementatiescripts om te werken met de nieuwe API's.

De volgende tabel bevat een verwijzing naar de programmatische interfaces voor zowel klassieke als nieuwe waarschuwingen:

|         |Klassieke waarschuwingen  |Nieuwe metrische waarschuwingen |
|---------|---------|---------|
|REST-API     | [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](https://docs.microsoft.com/rest/api/monitor/metricalerts)       |
|Azure-CLI     | [AZ monitor voor waarschuwing](https://docs.microsoft.com/cli/azure/monitor/alert?view=azure-cli-latest)        | [AZ monitor metrics waarschuwing](https://docs.microsoft.com/cli/azure/monitor/metrics/alert?view=azure-cli-latest)        |
|PowerShell      | [Naslaginformatie](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule)       |  [Naslaginformatie](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrulev2)    |
| Azure Resource Manager-sjabloon | [Voor klassieke waarschuwingen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-enable-template)|[Voor nieuwe metrische waarschuwingen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates)|

## <a name="notification-payload-changes"></a>Melding nettolading wijzigt

De indeling van de melding nettolading is enigszins tussen [klassieke waarschuwingsregels](alerts-webhooks.md) en [nieuwe metrische waarschuwingen](alerts-metric-near-real-time.md#payload-schema). Als u een webhook, logische app of runbook-acties die worden geactiveerd door regels voor klassieke waarschuwingen hebt, moet u deze meldingseindpunten voor het accepteren van de indeling van de nettolading van de nieuwe metrische waarschuwingen bijwerken.

Gebruik de volgende tabel de velden van de webhook-nettolading van de klassieke-indeling naar de nieuwe indeling toe te wijzen:

|  |Klassieke waarschuwingen  |Nieuwe metrische waarschuwingen |
|---------|---------|---------|
|De waarschuwing is geactiveerd of opgelost?    | **status**       | **data.status** |
|Contextuele informatie over de waarschuwing     | **context**        | **data.context**        |
|Tijdstempel die de waarschuwing is geactiveerd of opgelost     | **context.timestamp**       | **data.context.timestamp**        |
| Waarschuwingsregel-ID | **context.id** | **data.context.id** |
| Naam waarschuwingsregel | **context.name** | **data.context.name** |
| Beschrijving van de waarschuwingsregel | **context.description** | **data.context.description** |
| Waarschuwingsregel voorwaarde | **context.condition** | **data.context.condition** |
| Naam van de meetwaarde | **context.condition.metricName** | **data.context.condition.allOf[0].metricName** |
| Tijdverzameling (hoe de metrische gegevens over de evaluatie-venster wordt geaggregeerd)| **data.context.condition.timeAggregation** | **data.context.condition.timeAggregation** |
| De evaluatieperiode voor | **context.condition.windowSize** | **data.context.condition.windowSize** |
| Operator (hoe de samengevoegde metrische waarde wordt vergeleken met de drempelwaarde) | **context.condition.operator** | **data.context.condition.operator** |
| Drempelwaarde | **context.condition.threshold** | **data.context.condition.allOf[0].threshold** |
| Metrische waarde | **context.condition.metricValue** | **data.context.condition.allOf[0].metricValue** |
| Abonnements-id | **context.subscriptionId** | **data.context.subscriptionId** |
| Resourcegroep van de betrokken resource | **context.resourceGroup** | **data.context.resourceGroup** |
| Naam van de betrokken resource | **context.resourceName** | **data.context.resourceName** |
| Type van de betrokken resource | **context.resourceType** | **data.context.resourceType** |
| Resource-ID van de betreffende resource | **context.resourceId** | **data.context.resourceId** |
| Directe koppeling naar de pagina overzicht van portal-resource | **context.portalLink** | **data.context.portalLink** |
| De nettolading van de aangepaste velden die moeten worden doorgegeven aan de webhook of de logische app | **Eigenschappen** | **data.properties** |

De nettoladingen zijn vergelijkbaar, zoals u kunt zien. De volgende sectie biedt:

- Meer informatie over het wijzigen van logische apps om te werken met de nieuwe indeling.
- Een voorbeeld van het runbook dat de nettolading van de melding voor nieuwe waarschuwingen worden geparseerd.

## <a name="modify-a-logic-app-to-receive-a-metric-alert-notification"></a>Wijzigen van een logische app voor het ontvangen van een metrische waarschuwing

Als u logische apps met klassieke waarschuwingen, moet u uw code logica-app voor het parseren van de nettolading van de nieuwe metrische waarschuwingen wijzigen. Volg deze stappen:

1. Maak een nieuwe logische app.

1. Gebruik de sjabloon 'Azure Monitor--metrische waarschuwing Handler'. Deze sjabloon bevat een **HTTP-aanvraag** trigger met het bijbehorende schema gedefinieerd.

    ![logica-app-sjabloon](media/alerts-migration/logic-app-template.png "metrische waarschuwing sjabloon")

1. Een actie voor het hosten van uw verwerkingslogica toevoegen.

## <a name="use-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Een automation-runbook dat een metrische waarschuwing melding ontvangt gebruiken

Het volgende voorbeeld wordt PowerShell die moet worden gebruikt in uw runbook. Deze code kan de nettoladingen voor zowel klassieke waarschuwingsregels voor metrische gegevens en nieuwe waarschuwingsregels voor metrische gegevens worden geparseerd.

```PowerShell
## Example PowerShell code to use in a runbook to handle parsing of both classic and new metric alerts.

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

    # Determine whether the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new metric alert schema.
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic metric alert schema.
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is neither a classic metric alert nor a new metric alert.
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    # Parse fields related to resource affected.
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

Zie voor een compleet voorbeeld van een runbook waarmee een virtuele machine wordt gestopt wanneer een waarschuwing wordt geactiveerd, de [documentatie over Azure Automation](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook).

## <a name="partner-integration-via-webhooks"></a>Partnerintegratie via webhooks

De meeste van [onze partners die zijn geïntegreerd met klassieke waarschuwingen](https://docs.microsoft.com/azure/azure-monitor/platform/partners) al ondersteuning voor nieuwere metrische waarschuwingen via hun integraties. Bekende integraties gebruikt die al met nieuwe metrische waarschuwingen werken zijn:

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Signl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Als u een partnerintegratie die niet wordt vermeld hier gebruikt, controleert u of met de integratie-provider dat de integratie met nieuwe metrische waarschuwingen werkt.

## <a name="next-steps"></a>Volgende stappen

- [Het gebruik van het hulpprogramma voor migratie](alerts-using-migration-tool.md)
- [Informatie over de werking van het hulpprogramma voor migratie](alerts-understand-migration.md)
