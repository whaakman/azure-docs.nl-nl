---
title: Powershell gebruiken voor meldingen instellen in Application Insights | Microsoft Docs
description: Configuratie van Application Insights om op te halen van e-mailberichten over metrische wijzigingen automatiseren.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: mbullwin
ms.openlocfilehash: b90a540afd1c2815db8f5a99ee210ce21ea4d874
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>PowerShell gebruiken om waarschuwingen in te stellen in Application Insights
U kunt de configuratie van automatiseren [waarschuwingen](app-insights-alerts.md) in [Application Insights](app-insights-overview.md).

Bovendien kunt u [webhooks voor het automatiseren van uw reactie op een waarschuwing instellen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

> [!NOTE]
> Als u maken van resources en waarschuwingen op hetzelfde moment wilt, overweeg dan [met een Azure Resource Manager-sjabloon](app-insights-powershell.md).
>
>

## <a name="one-time-setup"></a>Eenmalige instelling
Als u dit nog niet hebt PowerShell met uw Azure-abonnement voordat gebruikt:

De Azure Powershell-module installeren op de computer waar u de scripts worden uitgevoerd.

* Installeer [Microsoft Web Platform Installer (v5 of hoger)](http://www.microsoft.com/web/downloads/platform.aspx).
* Voor het installeren van Microsoft Azure Powershell gebruiken

## <a name="connect-to-azure"></a>Verbinding maken met Azure
Open Azure PowerShell en [verbinding maken met uw abonnement](/powershell/azure/overview):

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a>Ontvang waarschuwingen
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Waarschuwing toevoegen
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a>Voorbeeld 1
Stuur mij een e-mail als reactie op HTTP-aanvragen meer dan 5 minuten, een gemiddelde waarde van de server langzamer dan 1 seconde is. Mijn Application Insights-resource IceCreamWebApp wordt genoemd en is in de resourcegroep Fabrikam. Ik ben de eigenaar van het Azure-abonnement.

De GUID is de abonnement-ID (niet de instrumentatiesleutel van de toepassing).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Voorbeeld 2
Ik heb een toepassing die ik gebruik [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) voor het rapporteren van een waarde met de naam 'salesPerHour'. Verzend dat een e-mail naar Mijn collega's als 'salesPerHour' onder de 100, gemiddeld meer dan 24 uur.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Dezelfde regel kan worden gebruikt voor de metriek gerapporteerd met behulp van de [meting parameter](app-insights-api-custom-events-metrics.md#properties) van bijhouden van een andere aanroep zoals TrackEvent of trackPageView.

## <a name="metric-names"></a>De namen van de metrische gegevens
| Metrische naam | De schermnaam van het | Beschrijving |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Browseruitzonderingen |Het aantal niet-onderschepte uitzonderingen in de browser. |
| `basicExceptionServer.count` |Serveruitzonderingen |Aantal niet-verwerkte uitzonderingen die door de app |
| `clientPerformance.clientProcess.value` |Verwerkingstijd van de client |Tijd tussen ontvangst van de laatste byte van een document totdat de DOM is geladen. Asynchrone aanvragen kunnen nog worden verwerkt. |
| `clientPerformance.networkConnection.value` |Paginalaadtijd netwerk verbinding maken |Duur van de browser verbinding maken met het netwerk. Kan niet 0 zijn als in de cache. |
| `clientPerformance.receiveRequest.value` |Ontvangende reactietijd |Tijd tussen de browser aanvraag verzenden om te beginnen met een reactie ontvangen. |
| `clientPerformance.sendRequest.value` |Aanvraagtijd verzenden |Tijd die door de browser om aanvraag te verzenden. |
| `clientPerformance.total.value` |Laadtijd van browserpagina |Tijd vanaf gebruikersaanvraag totdat DOM, opmaakmodellen, scripts en afbeeldingen zijn geladen. |
| `performanceCounter.available_bytes.value` |Beschikbaar geheugen |Fysiek geheugen die direct beschikbaar is voor een proces of voor systeemgebruik. |
| `performanceCounter.io_data_bytes_per_sec.value` |Verwerkingssnelheid van i/o |Totaal aantal bytes per seconde gelezen en geschreven naar bestanden, netwerk en apparaten. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |snelheid van de uitzondering |Uitzonderingen per seconde. |
| `performanceCounter.percentage_processor_time.value` |Proces CPU |Het percentage verstreken tijd van alle procesthreads die worden gebruikt door de processor uitvoering instructies voor het toepassingsproces. |
| `performanceCounter.percentage_processor_total.value` |Tijd van processor |Het percentage tijd dat de processor aan niet-inactieve threads spendeert. |
| `performanceCounter.process_private_bytes.value` |Eigen bytes van proces |Geheugen exclusief toegewezen aan de bewaakte toepassing processen. |
| `performanceCounter.request_execution_time.value` |Uitvoeringstijd voor ASP.NET-aanvraag |Uitvoeringstijd van de meest recente aanvraag. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET-aanvragen in wachtrij voor uitvoering |Lengte van de wachtrij voor toepassingsaanvragen. |
| `performanceCounter.requests_per_sec.value` |Snelheid van aanvragen voor ASP.NET |Verhouding van alle aanvragen naar de toepassing per seconde van ASP.NET. |
| `remoteDependencyFailed.durationMetric.count` |Afhankelijkheid fouten |Het aantal mislukte aanroepen van de servertoepassing naar externe bronnen. |
| `request.duration` |Serverreactietijd |Tijd tussen ontvangst van een HTTP-aanvraag en het voltooien van de reactie wordt verzonden. |
| `request.rate` |Snelheid van aanvragen voor |De snelheid van alle aanvragen aan de toepassing per seconde. |
| `requestFailed.count` |Mislukte aanvragen |Aantal HTTP-aanvragen dat heeft geresulteerd in een responscode > = 400 |
| `view.count` |Paginaweergaven |Het aantal aanvragen van clientgebruikers voor webpagina's. Synthetisch verkeer is gefilterd. |
| {uw aangepaste metrische naam} |{De naam van de meetwaarde} |De waarde van de metrische gegevens die zijn gerapporteerd door [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) of in de [metingen parameter van een aanroep van bijhouden](app-insights-api-custom-events-metrics.md#properties). |

De metrische gegevens worden verzonden door verschillende telemetrie modules:

| Metrische groep | Module collector |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>weergeven |[Browser JavaScript](app-insights-javascript.md) |
| performanceCounter |[Prestaties](app-insights-configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Afhankelijkheid](app-insights-configuration-with-applicationinsights-config.md) |
| aanvraag,<br/>requestFailed |[Serveraanvraag](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Webhooks.
U kunt [automatiseren uw reactie op een waarschuwing](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure belt een webadres van uw keuze wanneer een waarschuwing wordt gegenereerd.

## <a name="see-also"></a>Zie ook
* [Script voor het configureren van Application Insights](app-insights-powershell-script-create-resource.md)
* [Application Insights en test webbronnen van sjablonen maken](app-insights-powershell.md)
* [Microsoft Azure Diagnostics koppeling naar Application Insights automatiseren](app-insights-powershell-azure-diagnostics.md)
* [Uw reactie op een waarschuwing te automatiseren](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
