---
title: Azure Status Monitor v2 oplossen van problemen en bekende problemen | Microsoft Docs
description: De bekende problemen van Status Monitor v2 en het oplossen van voorbeelden. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
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
ms.openlocfilehash: d640206fd72b4eb89afe5ed1750627823bca9637
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415878"
---
# <a name="troubleshooting-status-monitor-v2"></a>Status van probleemoplossing v2 bewaken

Wanneer u de bewaking inschakelt, u ondervindt mogelijk problemen die verhinderen het verzamelen van gegevens dat. Dit document geeft een lijst van alle bekende problemen en probleemoplossing voorbeelden.
Als u een probleem hier niet vermeld tegenkomt, u mogelijk contact met ons opnemen [hier](https://github.com/Microsoft/ApplicationInsights-Home/issues).


> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="known-issues"></a>Bekende problemen

### <a name="conflicting-dlls-in-an-applications-bin-directory"></a>Conflicterende dll-bestanden in de bin-map van een toepassing

Als een van deze DLL-bestanden aanwezig in de bin-map zijn, mislukken bewaking.

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Sommige van deze DLL-bestanden zijn opgenomen in Visual Studio application standaardsjablonen, zelfs als ze niet worden gebruikt door uw toepassing.
Symptomen gedrag kan worden weergegeven met behulp van hulpprogramma's voor probleemoplossing:

- Voorbeeld:
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- iisreset + app laden (geen TELEMETRIE). Verder onderzoek Sysinternals (Handle.exe en ListDLLs.exe)
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>Conflict met gedeelde IIS-configuratie

Als u een cluster van webservers, u mogelijk gebruikt een [gedeelde configuratie](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211). We kunnen niet automatisch onze HttpModule invoeren in deze gedeelde configuratie. Elke webserver moet eerst de opdracht inschakelen voor het installeren van de DLL-bestand in de GAC uitvoeren.

Na het uitvoeren van de opdracht inschakelen 
1. Blader naar de gedeelde configuratiemap en zoek uw `applicationHost.config` bestand.
2. Deze regel toevoegen aan uw configuratie in de sectie modules:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
    


## <a name="troubleshooting"></a>Probleemoplossing
    
### <a name="troubleshooting-powershell"></a>Oplossen van problemen met PowerShell

#### <a name="how-to-inspect-what-modules-are-available"></a>Hoe om te controleren welke modules zijn beschikbaar?
U kunt de geïnstalleerde Modules met de opdracht controleren: `Get-Module -ListAvailable`

#### <a name="how-to-import-a-module-into-the-current-session"></a>Het importeren van een module in de huidige sessie?
Als de Module niet in een PowerShell-sessie geladen is, kunnen handmatig laden met behulp van de opdracht: `Import-Module <path to psd1>`


### <a name="troubleshooting-the-status-monitor-v2-module"></a>Het oplossen van de Status Monitor v2-module

#### <a name="how-to-review-what-commands-are-available-in-the-status-monitor-v2-module"></a>Hoe om te controleren welke opdrachten zijn beschikbaar in de Status Monitor v2-module?
- Voer de opdracht uit: `Get-Command -Module Az.ApplicationMonitor` om op te halen van de beschikbare opdrachten:

    ```
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Disable-ApplicationInsightsMonitoring              0.2.1      Az.ApplicationMonitor
    Cmdlet          Disable-InstrumentationEngine                      0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-ApplicationInsightsMonitoring               0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-InstrumentationEngine                       0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.2.1      Az.ApplicationMonitor
    Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    ```

#### <a name="what-is-the-current-version-of-the-status-monitor-v2-module"></a>Wat is de huidige versie van de v2-module van Status Monitor?
- Voer de opdracht uit: `Get-ApplicationInsightsMonitoringStatus` om een uitvoer van informatie over deze module te verkrijgen:
    - PowerShell-moduleversie
    - Application Insights-SDK-versie
    - Paden van PowerShell-module
    
Bekijk onze [API-verwijzing](status-monitor-v2-api-get-status.md) voor een gedetailleerde beschrijving van het gebruik van deze cmdlet.



### <a name="troubleshooting-running-processes"></a>Het oplossen van actieve processen

U kunt het proces op de functionele machine om te controleren of alle dll's zijn geladen kunt inspecteren.
Als de controle werkt, moeten ten minste 12 dll's worden geladen.

- Cmd: `Get-ApplicationInsightsMonitoringStatus -InspectProcess`

Bekijk onze [API-verwijzing](status-monitor-v2-api-get-status.md) voor een gedetailleerde beschrijving van het gebruik van deze cmdlet.


### <a name="collect-etw-logs-with-perfview"></a>Verzamelen van ETW-logboeken met voorbeeld

#### <a name="setup"></a>Installatie

1. PerfView.exe en PerfView64.exe van downloaden https://github.com/Microsoft/perfview/releases
2. Launch PerfView64.exe
3. Vouw 'Geavanceerde opties'
4. Schakel het selectievakje:
    - Postcode
    - Samenvoegen
    - Verzameling van .NET-symbool
5. Stel aanvullende Providers: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Logboeken verzamelen

1. Voer in een cmd-console met beheerdersbevoegdheden, `iisreset /stop` uitschakelen IIS en alle web-apps.
2. In het voorbeeld, klikt u op 'Verzameling starten'
3. Voer in een cmd-console met beheerdersbevoegdheden, `iisreset /start` naar IIS wordt gestart.
4. Probeer om te bladeren naar uw app.
5. Nadat uw app geladen is, gaat u terug naar voorbeeld en klikt u op 'Verzameling stoppen'



## <a name="next-steps"></a>Volgende stappen

- Bekijk onze [API-verwijzing](status-monitor-v2-overview.md#powershell-api-reference) te vinden van een parameter die u hebt gemist.
- Als u een probleem hier niet vermeld tegenkomt, u mogelijk contact met ons opnemen [hier](https://github.com/Microsoft/ApplicationInsights-Home/issues).
