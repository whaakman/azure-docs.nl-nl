---
title: 'Status van Azure Monitor v2 API-verwijzing: Configuratie instellen | Microsoft Docs'
description: Status Monitor v2 API-verwijzing Set-ApplicationInsightsMonitoringConfig. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
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
ms.openlocfilehash: 971fea76a23859f32437a1698b6d3094113737a1
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255101"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>Status Monitor v2 API: Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Dit document beschrijft een cmdlet die wordt geleverd als een lid van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Description

Het configuratiebestand instellen zonder het herhalende volledig opnieuw moet installeren. Start IIS opnieuw om uw wijzigingen te laten treden.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.


## <a name="examples"></a>Voorbeelden

### <a name="example-with-single-instrumentation-key"></a>Voorbeeld met één instrumentatiesleutel
In dit voorbeeld wordt een enkele instrumentatiesleutel op alle toepassingen op de huidige machine worden toegewezen.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-instrumentation-key-map"></a>Voorbeeld van belangrijke instrumentatie-kaart
In dit voorbeeld 
- `MachineFilter` komt overeen met de huidige machine met de `'.*'` jokertekens.
- `AppFilter='WebAppExclude'` biedt een `null` InstrumentationKey. Kan deze app wordt niet geïmplementeerd.
- `AppFilter='WebAppOne'` wordt toegewezen deze specifieke app een unieke instrumentatiesleutel.
- `AppFilter='WebAppTwo'` wordt deze specifieke app een unieke instrumentatiesleutel ook toewijzen.
- Ten slotte `AppFilter` gebruikt ook de `'.*'` jokerteken zodat deze overeenkomt met alle andere WebApps niet overeenkomt met de eerdere regels en een standaard-instrumentatiesleutel wordt toegewezen.
- Ruimten toegevoegd voor de leesbaarheid alleen.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parameters 

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Vereist.** Gebruik deze parameter om op te geven van een enkele iKey voor gebruik door alle toepassingen op de doelcomputer.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Vereist.** Gebruik deze parameter om op te geven meerdere ikeys en een toewijzing van welke apps u wilt gebruiken, welke ikey. U kunt een script voor één installatie voor meerdere machines maken door in te stellen de MachineFilter. 

> [!IMPORTANT] 
> Toepassingen overeen met de regels in de volgorde waarin ze zijn opgegeven. Daarom moet u de meest specifieke regels eerst opgeven en de meest algemene regels laatste.

#### <a name="schema"></a>Schema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** is een vereiste c#-regex van de naam van de computer of virtuele machine.
    - '. *' komt overeen met alle
    - "Computernaam" komt overeen met alleen computers met de exacte naam.
- **AppFilter** is een vereiste c#-regex van de naam van de computer of virtuele machine.
    - '. *' komt overeen met alle
    - 'ApplicationName' komt overeen met alleen IIS-toepassingen met de exacte naam.
- **InstrumentationKey** is vereist voor het inschakelen van bewaking van de toepassingen die overeenkomen met de bovenstaande twee filters.
    - Laat deze waarde leeg als u definiëren van regels wilt voor het uitsluiten van bewaking


### <a name="-verbose"></a>-Verbose
**Common Parameter.** Gebruik deze switch naar gedetailleerde logboeken uitvoer.


## <a name="output"></a>Uitvoer

Er is geen uitvoer standaard.

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
 - [Verken de metrische gegevens](../../azure-monitor/app/metrics-explorer.md) om de prestaties en het gebruik te bewaken
- [Doorzoek gebeurtenissen en logboeken](../../azure-monitor/app/diagnostic-search.md) om problemen te diagnosticeren
- [Gebruik analyses](../../azure-monitor/app/analytics.md) voor meer geavanceerde query's
- [Maak dashboards](../../azure-monitor/app/overview-dashboard.md)
 
 Meer telemetrie toevoegen:
 - [Maak webtests](monitor-web-app-availability.md) om ervoor te zorgen dat uw site actief blijft.
- [Voeg telemetrie van de webclient](../../azure-monitor/app/javascript.md) zien welke uitzonderingen webpaginacode en traceringsaanroepen in te voegen.
- [Application Insights SDK toevoegen aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen
 
 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
 - [Ophalen van de configuratie](status-monitor-v2-api-get-config.md) om te bevestigen dat de instellingen correct zijn vastgelegd.
 - [De status ophalen](status-monitor-v2-api-get-status.md) te inspecteren bewaking.
