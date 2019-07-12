---
title: 'Azure Status Monitor v2 API-verwijzing: Configuratie ophalen | Microsoft Docs'
description: Status Monitor v2 API-verwijzing. Get-ApplicationInsightsMonitoringConfig. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET web-apps die on-premises, in virtuele machines, of op Azure worden gehost.
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
ms.openlocfilehash: 7eaaaa2dd7b22d138ea2f0a52d0bf0a1b2eab026
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807096"
---
# <a name="status-monitor-v2-api-get-applicationinsightsmonitoringconfig-v040-alpha"></a>Status Monitor v2 API: Get-ApplicationInsightsMonitoringConfig (v0.4.0-alpha)

Dit artikel wordt beschreven voor een cmdlet die deel uitmaakt van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="description"></a>Description

Het configuratiebestand opgehaald en worden de waarden in de console afgedrukt.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.

## <a name="examples"></a>Voorbeelden

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

## <a name="parameters"></a>Parameters

Er is geen vereiste parameters.

## <a name="output"></a>Output


#### <a name="example-output-from-reading-the-config-file"></a>Voorbeeld van uitvoer de configuratiebestand te lezen

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```

## <a name="next-steps"></a>Volgende stappen

  Uw telemetrie weergeven:
 - [Verken de metrische gegevens](../../azure-monitor/app/metrics-explorer.md) prestaties controleren en gebruik.
- [Doorzoek gebeurtenissen en logboeken](../../azure-monitor/app/diagnostic-search.md) om problemen te diagnosticeren.
- Gebruik [analytics](../../azure-monitor/app/analytics.md) voor meer geavanceerde query's.
- [Maak dashboards](../../azure-monitor/app/overview-dashboard.md).
 
 Meer telemetrie toevoegen:
 - [Maak webtests](monitor-web-app-availability.md) om ervoor te zorgen dat uw site actief blijft.
- [Voeg telemetrie van de webclient](../../azure-monitor/app/javascript.md) om te zien welke uitzonderingen webpaginacode en om in te schakelen trace-aanroepen.
- [Voeg de Application Insights SDK toe aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen.
 
 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
 - Wijzigingen aanbrengen in de configuratie met behulp van de [Set config](status-monitor-v2-api-set-config.md) cmdlet.
