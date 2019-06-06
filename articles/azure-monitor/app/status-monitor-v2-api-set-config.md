---
title: 'Azure Status Monitor v2 API-verwijzing: Configuratie instellen | Microsoft Docs'
description: Status Monitor v2 API-verwijzing. Set-ApplicationInsightsMonitoringConfig. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET web-apps die on-premises, in virtuele machines, of op Azure worden gehost.
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
ms.openlocfilehash: 562ce8a4267370be9b049e3b56f213f82deb89c0
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734990"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>Status Monitor v2 API: Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Dit document beschrijft een cmdlet die deel uitmaakt van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="description"></a>Description

Hiermee stelt u het configuratiebestand zonder een volledige installatie.
Start IIS opnieuw om uw wijzigingen te laten treden.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.


## <a name="examples"></a>Voorbeelden

### <a name="example-with-a-single-instrumentation-key"></a>Voorbeeld met een enkele instrumentatiesleutel
In dit voorbeeld wordt een enkele instrumentatiesleutel op alle apps op de huidige computer worden toegewezen.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Voorbeeld van een sleutel instrumentatie-kaart
In dit voorbeeld:
- `MachineFilter` komt overeen met de huidige computer met behulp van de `'.*'` jokertekens.
- `AppFilter='WebAppExclude'` biedt een `null` instrumentatiesleutel. Kan de opgegeven app wordt niet geïmplementeerd.
- `AppFilter='WebAppOne'` de opgegeven app een unieke instrumentatiesleutel toegewezen.
- `AppFilter='WebAppTwo'` de opgegeven app een unieke instrumentatiesleutel toegewezen.
- Ten slotte `AppFilter` gebruikt ook de `'.*'` jokerteken zodat deze overeenkomt met alle web-apps die zijn niet overeenkomt met de eerdere regels en toewijzen van een standaard-instrumentatiesleutel.
- Spaties zijn toegevoegd voor de leesbaarheid.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parameters

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Vereist.** Gebruik deze parameter om op te geven van een enkele instrumentatiesleutel voor gebruik door alle apps op de doelcomputer.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Vereist.** Gebruik deze parameter om op te geven meerdere instrumentatiesleutels en een toewijzing van de instrumentatiesleutels die worden gebruikt door elke app.
U kunt een script voor één installatie voor meerdere computers maken door in te stellen `MachineFilter`.

> [!IMPORTANT]
> Apps overeen met de regels in de volgorde die de regels worden verstrekt. Daarom moet u de meest specifieke regels eerst opgeven en de meest algemene regels laatste.

#### <a name="schema"></a>Schema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** is een vereiste C# reguliere expressie van de computer of VM-naam.
    - '. *' komt overeen met alle
    - "Computernaam" komt overeen met alleen computers met de opgegeven naam.
- **AppFilter** is een vereiste C# reguliere expressie van de computer of VM-naam.
    - '. *' komt overeen met alle
    - 'ApplicationName' komt overeen met de IIS-alleen apps met de opgegeven naam.
- **InstrumentationKey** is vereist voor het inschakelen van bewaking van de apps die overeenkomen met de voorgaande twee filters.
    - Laat deze waarde leeg als u definiëren van regels wilt voor het uitsluiten van bewaking.


### <a name="-verbose"></a>-Verbose
**Algemene parameter.** Gebruik deze switch om gedetailleerde logboeken weer te geven.


## <a name="output"></a>Uitvoer

Standaard geen uitvoer.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Voorbeeld van de uitgebreide uitvoer van het instellen van het configuratiebestand via - InstrumentationKey

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Voorbeeld van de uitgebreide uitvoer van het instellen van het configuratiebestand via - InstrumentationKeyMap

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

## <a name="next-steps"></a>Volgende stappen

  Uw telemetrie weergeven:
 - [Verken de metrische gegevens](../../azure-monitor/app/metrics-explorer.md) prestaties controleren en gebruik.
- [Doorzoek gebeurtenissen en logboeken](../../azure-monitor/app/diagnostic-search.md) om problemen te diagnosticeren.
- [Analytics gebruiken](../../azure-monitor/app/analytics.md) voor meer geavanceerde query's.
- [Maak dashboards](../../azure-monitor/app/overview-dashboard.md).
 
 Meer telemetrie toevoegen:
 - [Maak webtests](monitor-web-app-availability.md) om ervoor te zorgen dat uw site actief blijft.
- [Voeg telemetrie van de webclient](../../azure-monitor/app/javascript.md) om te zien welke uitzonderingen webpaginacode en om in te schakelen trace-aanroepen.
- [Voeg de Application Insights SDK toe aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen
 
 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
 - [Ophalen van de configuratie](status-monitor-v2-api-get-config.md) om te bevestigen dat de instellingen correct zijn vastgelegd.
 - [De status ophalen](status-monitor-v2-api-get-status.md) te inspecteren bewaking.
