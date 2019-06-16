---
title: Overzicht van Azure Status Monitor v2 | Microsoft Docs
description: Een overzicht van Status Monitor v2. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET web-apps die on-premises, in virtuele machines, of op Azure worden gehost.
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
ms.openlocfilehash: 2126408222433e6339723dc2da0d2611bb234fe8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734164"
---
# <a name="status-monitor-v2"></a>Status Monitor v2

Status Monitor v2 is een PowerShell module gepubliceerd naar de [PowerShell Gallery](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).
Azure Backup vervangt u [Status Monitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now).
De module biedt zonder code instrumentatie van .NET web-apps die worden gehost in IIS.
Telemetrie wordt verzonden naar de Azure-portal, waar u kunt [monitor](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) uw app.

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="powershell-gallery"></a>PowerShell Gallery

De PowerShell Gallery bevindt zich hier: https://www.powershellgallery.com/packages/Az.ApplicationMonitor.


## <a name="instructions"></a>Instructies
- Zie de [aan de slag instructies](status-monitor-v2-get-started.md) om op te halen van een begin met beknopte codevoorbeelden.
- Zie de [gedetailleerde instructies](status-monitor-v2-detailed-instructions.md) voor gedetailleerde informatie over hoe u aan de slag.

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

  *Ja*. Er zijn meerdere manieren voor het downloaden van Status Monitor v2. Als uw computer toegang tot internet heeft, kunt u onboarding naar de PowerShell Gallery met behulp van `-Proxy` parameters.
U kunt ook handmatig de module downloaden en een installeren op uw computer of rechtstreeks gebruiken.
Elk van deze opties wordt beschreven in de [gedetailleerde instructies](status-monitor-v2-detailed-instructions.md).
  
- Hoe kan ik controleren of de activering is geslaagd?

   Er is geen cmdlet om te controleren of activering is voltooid.
Raden wij aan u [Live Metrics](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) om snel te bepalen als uw app telemetrie verzendt.

   U kunt ook [Log Analytics](../log-query/get-started-portal.md) om alle cloudrollen die momenteel verzenden telemetrie weer te geven:
   ```Kusto
   union * | summarize count() by cloud_RoleName, cloud_RoleInstance
   ```

## <a name="next-steps"></a>Volgende stappen

Uw telemetrie weergeven:

* [Verken de metrische gegevens](../../azure-monitor/app/metrics-explorer.md) prestaties controleren en gebruik.
* [Doorzoek gebeurtenissen en logboeken](../../azure-monitor/app/diagnostic-search.md) om problemen te diagnosticeren.
* [Analytics gebruiken](../../azure-monitor/app/analytics.md) voor meer geavanceerde query's.
* [Maak dashboards](../../azure-monitor/app/overview-dashboard.md).

Meer telemetrie toevoegen:

* [Maak webtests](monitor-web-app-availability.md) om ervoor te zorgen dat uw site actief blijft.
* [Voeg telemetrie van de webclient](../../azure-monitor/app/javascript.md) om te zien welke uitzonderingen webpaginacode en om in te schakelen trace-aanroepen.
* [Voeg de Application Insights SDK toe aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen.

