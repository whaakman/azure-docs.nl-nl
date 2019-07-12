---
title: Azure Status Monitor v2 oplossen van problemen en bekende problemen | Microsoft Docs
description: De bekende problemen van Status Monitor v2 en het oplossen van voorbeelden. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET web-apps die on-premises, in virtuele machines, of op Azure worden gehost.
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
ms.openlocfilehash: df59766ce38ac81568570cd6544ee28808ff8249
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807015"
---
# <a name="troubleshooting-status-monitor-v2"></a>Status van probleemoplossing v2 bewaken

Wanneer u bewaking hebt ingeschakeld, ervaren kunt u problemen die verhinderen het verzamelen van gegevens dat.
In dit artikel geeft een lijst van alle bekende problemen en probleemoplossing voorbeelden.
Als u tijdens het een probleem dat hier niet wordt vermeld, kunt u contact met ons op [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).


> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="known-issues"></a>Bekende problemen

### <a name="conflicting-dlls-in-an-apps-bin-directory"></a>Conflicterende dll-bestanden in de bin-map van een app

Als een van deze DLL-bestanden aanwezig in de bin-map zijn, mislukken bewaking:

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Sommige van deze DLL-bestanden zijn opgenomen in de Visual Studio-sjablonen voor standaard-app, zelfs als ze niet worden gebruikt door uw app.
U kunt hulpprogramma's voor probleemoplossing gebruiken om te zien van de symptomen gedrag:

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

- IISReset en app laden (zonder telemetrie). Verder onderzoek Sysinternals (Handle.exe en ListDLLs.exe):
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>Conflict met gedeelde IIS-configuratie

Als u een cluster van webservers, u mogelijk gebruikt een [gedeelde configuratie](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211).
De module kan niet worden opgenomen in deze gedeelde configuratie.
Voer de opdracht inschakelen op elke webserver voor het installeren van het DLL-bestand in de GAC van elke server.

Nadat u de opdracht inschakelen uitgevoerd, voert u deze stappen uit:
1. Ga naar de gedeelde configuratiemap en zoek het bestand applicationHost.config.
2. Deze regel toevoegen aan de modules-sectie van uw configuratie:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
## <a name="troubleshooting"></a>Problemen oplossen
    
### <a name="troubleshooting-powershell"></a>Oplossen van problemen met PowerShell

#### <a name="determine-which-modules-are-available"></a>Bepalen welke modules beschikbaar zijn
U kunt de `Get-Module -ListAvailable` opdracht uit om te bepalen welke modules zijn geïnstalleerd.

#### <a name="import-a-module-into-the-current-session"></a>Een module importeren in de huidige sessie
Als een module niet in een PowerShell-sessie geladen is, kunt u handmatig deze laden met behulp van de `Import-Module <path to psd1>` opdracht.


### <a name="troubleshooting-the-status-monitor-v2-module"></a>Het oplossen van de Status Monitor v2-module

#### <a name="list-the-commands-available-in-the-status-monitor-v2-module"></a>Lijst met de opdrachten die beschikbaar zijn in de Status Monitor v2-module
Voer de opdracht `Get-Command -Module Az.ApplicationMonitor` om op te halen van de beschikbare opdrachten:

```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Disable-ApplicationInsightsMonitoring              0.4.0      Az.ApplicationMonitor
Cmdlet          Disable-InstrumentationEngine                      0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-ApplicationInsightsMonitoring               0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-InstrumentationEngine                       0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.4.0      Az.ApplicationMonitor
Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Start-ApplicationInsightsMonitoringTrace           0.4.0      Az.ApplicationMonitor
```

#### <a name="determine-the-current-version-of-the-status-monitor-v2-module"></a>Bepalen van de huidige versie van de Status Monitor v2-module
Voer de `Get-ApplicationInsightsMonitoringStatus` opdracht voor het weergeven van de volgende informatie over de module:
   - PowerShell-moduleversie
   - Application Insights-SDK-versie
   - Paden van de PowerShell-module
    
Controleer de [API-verwijzing](status-monitor-v2-api-get-status.md) voor een gedetailleerde beschrijving van het gebruik van deze cmdlet.


### <a name="troubleshooting-running-processes"></a>Het oplossen van actieve processen

U kunt de processen die op de functionele computer om te bepalen of alle dll's zijn geladen kunt inspecteren.
Als de controle werkt, moeten ten minste 12 dll's worden geladen.

Gebruik de `Get-ApplicationInsightsMonitoringStatus -InspectProcess` opdracht om te controleren van de dll-bestanden.

Controleer de [API-verwijzing](status-monitor-v2-api-get-status.md) voor een gedetailleerde beschrijving van het gebruik van deze cmdlet.


### <a name="collect-etw-logs-by-using-perfview"></a>ETW-logboeken verzamelen met behulp van voorbeeld

#### <a name="setup"></a>Instellen

1. Download PerfView.exe en PerfView64.exe van [GitHub](https://github.com/Microsoft/perfview/releases).
2. Start PerfView64.exe.
3. Vouw **geavanceerde opties**.
4. Schakel deze selectievakjes uit:
    - **Zip**
    - **Merge**
    - **Verzameling van .NET-symbool**
5. Stel deze in **aanvullende Providers**: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Verzamelen van Logboeken

1. Voer in de opdrachtconsole van een met beheerdersbevoegdheden, de `iisreset /stop` opdracht voor het uitschakelen van IIS en alle web-apps.
2. Selecteer in het voorbeeld, **verzamelen starten**.
3. Voer in de opdrachtconsole van een met beheerdersbevoegdheden, de `iisreset /start` opdracht waarmee IIS wordt gestart.
4. Probeer om te bladeren naar uw app.
5. Nadat uw app is geladen, keert u terug naar voorbeeld en selecteer **verzameling stoppen**.



## <a name="next-steps"></a>Volgende stappen

- Controleer de [API-verwijzing](status-monitor-v2-overview.md#powershell-api-reference) voor meer informatie over parameters die u hebt gemist.
- Als u tijdens het een probleem dat hier niet wordt vermeld, kunt u contact met ons op [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).
