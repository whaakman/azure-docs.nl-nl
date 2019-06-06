---
title: 'Azure Status Monitor v2 API-verwijzing: Schakel de bewaking | Microsoft Docs'
description: Status Monitor v2 API-verwijzing. Enable-ApplicationInsightsMonitoring. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET web-apps die on-premises, in virtuele machines, of op Azure worden gehost.
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
ms.openlocfilehash: e87bfad11eee5b86d35e6b4f2846b094c467e0ef
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734168"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v021-alpha"></a>Status Monitor v2 API: Enable-ApplicationInsightsMonitoring (v0.2.1-alfa)

Dit artikel wordt beschreven voor een cmdlet die deel uitmaakt van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="description"></a>Description

Zonder code kunt koppelen aan bewaking van IIS-apps op een doelcomputer.

Deze cmdlet wordt de IIS-applicationHost.config wijzigen en bepaalde registersleutels instellen.
Het maakt ook een bestand applicationinsights.ikey.config, waarin de instrumentatiesleutel die worden gebruikt door elke app.
IIS kan de RedfieldModule bij het opstarten, die de Application Insights-SDK wordt worden invoeren in toepassingen zoals het starten van de toepassingen wordt geladen.
Start IIS opnieuw om uw wijzigingen te laten treden.

Nadat u bewaking hebt ingeschakeld, raden wij aan dat u [Live Metrics](live-stream.md) om snel te controleren als uw app ons telemetriegegevens verzendt.


> [!NOTE] 
> - Als u wilt beginnen, moet u een instrumentatiesleutel. Zie voor meer informatie, [een resource maken](create-new-resource.md#copy-the-instrumentation-key).
> - Deze cmdlet is vereist dat u lees en accepteer van onze licentie en privacy-instructie.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen en een beleid voor uitvoering met verhoogde bevoegdheden vereist. Zie voor meer informatie, [PowerShell uitvoeren als beheerder met een beleid voor uitvoering met verhoogde bevoegdheden](status-monitor-v2-detailed-instructions.md#run-powershell-as-admin-with-an-elevated-execution-policy).

## <a name="examples"></a>Voorbeelden

### <a name="example-with-a-single-instrumentation-key"></a>Voorbeeld met een enkele instrumentatiesleutel
In dit voorbeeld worden alle apps op de huidige computer een enkel instrumentatiesleutel toegewezen.

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
    - "Computernaam" komt overeen met alleen computers met de exacte naam die is opgegeven.
- **AppFilter** is een vereiste C# reguliere expressie van de computer of VM-naam.
    - '. *' komt overeen met alle
    - 'ApplicationName' wordt alleen IIS apps overeen met de exacte naam die is opgegeven.
- **InstrumentationKey** is vereist voor het inschakelen van bewaking van apps die overeenkomen met de voorgaande twee filters.
    - Laat deze waarde leeg als u definiëren van regels wilt voor het uitsluiten van bewaking.


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Optioneel.** Gebruik deze switch zodat de instrumentatie-engine voor het verzamelen van gebeurtenissen en -berichten over wat tijdens het uitvoeren van een beheerd proces gebeurt er. Deze gebeurtenissen en -berichten zijn resultaatcodes afhankelijkheid, HTTP-termen en SQL-opdrachttekst.

De engine instrumentation overhead toegevoegd en is standaard uitgeschakeld.

### <a name="-acceptlicense"></a>-AcceptLicense
**Optioneel.** Gebruik deze switch te accepteren van de licentie- en privacy-instructie in ' headless '-installaties.

### <a name="-verbose"></a>-Verbose
**Algemene parameter.** Gebruik deze switch om gedetailleerde logboeken weer te geven.

### <a name="-whatif"></a>-WhatIf 
**Algemene parameter.** Gebruik deze schakeloptie om te testen en valideren van de invoerparameters zonder daadwerkelijk inschakelen van de controle.

## <a name="output"></a>Uitvoer


#### <a name="example-output-from-a-successful-enablement"></a>Van voorbeelduitvoer van een geslaagde inschakelen

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
Installing GAC module 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.0\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll'
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
Found GAC module Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z_1'
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'enable'
Configuring IIS Environment for codeless attach...
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
Updating app pool permissions...
Successfully enabled Application Insights Status Monitor
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
- [Voeg de Application Insights SDK toe aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen.
 
 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
 - [Ophalen van de configuratie](status-monitor-v2-api-get-config.md) om te bevestigen dat de instellingen correct zijn vastgelegd.
 - [De status ophalen](status-monitor-v2-api-get-status.md) te inspecteren bewaking.
