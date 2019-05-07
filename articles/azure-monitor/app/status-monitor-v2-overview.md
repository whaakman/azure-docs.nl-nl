---
title: Overzicht van Azure Status Monitor v2 | Microsoft Docs
description: Een overzicht van Status Monitor v2. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 77c2cd9e0eac6717d91a73a6fc033dcaa2390444
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145029"
---
# <a name="status-monitor-v2"></a>Status Monitor v2

Status Monitor v2 is een PowerShell-Module die is gepubliceerd naar de [PowerShellGallery](https://www.powershellgallery.com/packages/Az.ApplicationMonitor) en is de vervanging voor [Status Monitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now). Deze module biedt code zonder instrumentatie van .NET-webtoepassingen die worden gehost in IIS.
Telemetrie wordt verzonden naar de Azure-portal kunt u [monitor](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) uw toepassing.

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="powershell-gallery"></a>PowerShell Gallery

https://www.powershellgallery.com/packages/Az.ApplicationMonitor


## <a name="instructions"></a>Instructies
- Bekijk onze [aan de slag instructies](status-monitor-v2-get-started.md) om te beginnen met beknopte codevoorbeelden.
- Bekijk onze [gedetailleerde instructies](status-monitor-v2-detailed-instructions.md) voor gedetailleerde informatie over hoe u aan de slag.

## <a name="powershell-api-reference"></a>PowerShell-API-verwijzing
- [Disable-ApplicationInsightsMonitoring](status-monitor-v2-api-disable-monitoring.md)
- [Disable-InstrumentationEngine](status-monitor-v2-api-disable-instrumentation-engine.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Enable-InstrumentationEngine](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)

## <a name="troubleshooting"></a>Problemen oplossen
- [Problemen oplossen](status-monitor-v2-troubleshoot.md)
- [Bekende problemen](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>Veelgestelde vragen

- Status Monitor v2 biedt ondersteuning voor installaties van de proxy?

  **Ja**. U hebt meerdere opties voor het downloaden van Status Monitor v2. Als uw computer toegang tot internet heeft, kunt u onboarding voor het gebruik van de PowerShell Gallery `-Proxy` parameters. Deze module kunt u ook handmatig downloaden en u deze installeren op uw computer of de module rechtstreeks gebruiken. Elk van deze opties wordt beschreven in onze [gedetailleerde instructies](status-monitor-v2-detailed-instructions.md).
  
- Het controleren van de activering is voltooid?

   Er zijn geen een cmdlet om te controleren dat activering is geslaagd. Wordt u aangeraden [Live Metrics](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) snel in acht nemen als uw toepassing ons telemetriegegevens verzendt.

## <a name="next-steps"></a>Volgende stappen

Uw telemetrie weergeven:

* [Verken de metrische gegevens](../../azure-monitor/app/metrics-explorer.md) om de prestaties en het gebruik te bewaken
* [Doorzoek gebeurtenissen en logboeken](../../azure-monitor/app/diagnostic-search.md) om problemen te diagnosticeren
* [Gebruik analyses](../../azure-monitor/app/analytics.md) voor meer geavanceerde query's
* [Maak dashboards](../../azure-monitor/app/app-insights-dashboards.md)

Meer telemetrie toevoegen:

* [Maak webtests](monitor-web-app-availability.md) om ervoor te zorgen dat uw site actief blijft.
* [Voeg telemetrie van de webclient](../../azure-monitor/app/javascript.md) zien welke uitzonderingen webpaginacode en traceringsaanroepen in te voegen.
* [Application Insights SDK toevoegen aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen

