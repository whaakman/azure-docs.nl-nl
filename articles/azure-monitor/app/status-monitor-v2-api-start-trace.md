---
title: 'Azure Status Monitor v2 API-verwijzing: Tracering starten | Microsoft Docs'
description: Status Monitor v2 API-verwijzing. Tracering starten. Verzamel ETW-logboeken van Status Monitor en Application Insights-SDK.
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
ms.openlocfilehash: 186324e6a93f90fd04323074e5dc883e3e271725
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478990"
---
# <a name="status-monitor-v2-api-start-applicationinsightsmonitoringtrace-v031-alpha"></a>Status Monitor v2 API: Start-ApplicationInsightsMonitoringTrace (v0.3.1-alpha)

Dit artikel wordt beschreven voor een cmdlet die deel uitmaakt van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="description"></a>Description

Verzamelt [ETW-gebeurtenissen](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) koppelen van de zonder code runtime. Deze cmdlet is een alternatief voor het uitvoeren [voorbeeld](https://github.com/microsoft/perfview).

Verzamelde gebeurtenissen worden afgedrukt aan de console in realtime en opgeslagen als een ETL-bestand. De ETL-bestand voor uitvoer kan worden geopend door [voorbeeld](https://github.com/microsoft/perfview) voor verder onderzoek.

Deze cmdlet wordt uitgevoerd totdat het duur van de time-out (standaard 5 minuten) is bereikt of handmatig is gestopt (`Ctrl + C`).

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.

## <a name="examples"></a>Voorbeelden

### <a name="how-to-collect-events"></a>Over het verzamelen van gebeurtenissen

Normaal gesproken vragen wij verzamelen van gebeurtenissen om te onderzoeken waarom uw toepassing is niet wordt geïmplementeerd.

De zonder code koppelen runtime ETW-gebeurtenissen worden verzonden wanneer IIS wordt gestart en wanneer uw toepassing wordt gestart.

Voor het verzamelen van deze gebeurtenissen:
1. Voer in een cmd-console met beheerdersbevoegdheden, `iisreset /stop` uitschakelen IIS en alle web-apps.
2. Voert u deze cmdlet
3. Voer in een cmd-console met beheerdersbevoegdheden, `iisreset /start` naar IIS wordt gestart.
4. Probeer om te bladeren naar uw app.
5. Nadat uw app geladen is, u kunt handmatig voorkomen dat deze (`Ctrl + C`) of wacht totdat de time-out.

### <a name="what-events-to-collect"></a>Welke gebeurtenissen u wilt verzamelen

U hebt drie opties voor het verzamelen van gebeurtenissen:
1. Gebruik de schakeloptie `-CollectSdkEvents` voor het verzamelen van gebeurtenissen die afkomstig zijn van de Application Insights-SDK.
2. Gebruik de schakeloptie `-CollectRedfieldEvents` voor het verzamelen van gebeurtenissen die zijn gegenereerd door de Status Monitor en de Redfield-Runtime. Deze logboeken nuttig zijn bij het vaststellen van IIS en starten van de toepassing.
3. Beide switches gebruiken voor het verzamelen van beide typen gebeurtenissen.
4. Standaard, als u de schakeloptie niet is opgegeven dat beide typen gebeurtenissen worden verzameld.


## <a name="parameters"></a>Parameters

### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**Optioneel.** Gebruik deze parameter om in te stellen hoe lang dit script moet verzamelen van gebeurtenissen. Standaard is dit 5 minuten.

### <a name="-logdirectory"></a>-LogDirectory
**Optioneel.** Gebruik deze switch om in te stellen van de map met de uitvoer van de ETL-bestand. Standaard wordt dit bestand in de directory PowerShell-Modules worden gemaakt. Het volledige pad wordt weergegeven tijdens het uitvoeren van script.


### <a name="-collectsdkevents"></a>-CollectSdkEvents
**Optioneel.** Gebruik deze switch voor het verzamelen van gebeurtenissen van Application Insights-SDK.

### <a name="-collectredfieldevents"></a>-CollectRedfieldEvents
**Optioneel.** Gebruik deze switch voor het verzamelen van gebeurtenissen van Status Monitor en de Redfield-runtime.

### <a name="-verbose"></a>-Verbose
**Algemene parameter.** Gebruik deze switch naar gedetailleerde logboeken uitvoer.



## <a name="output"></a>Output


### <a name="example-of-application-startup-logs"></a>Voorbeeld van de toepassingslogboeken voor opstarten
```
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```


## <a name="next-steps"></a>Volgende stappen

Andere problemen oplossen:

- Lees hier meer probleemoplossingsstappen: https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-troubleshoot
- Controleer de [API-verwijzing](status-monitor-v2-overview.md#powershell-api-reference) voor meer informatie over parameters die u hebt gemist.
- Als u meer hulp nodig hebt, kunt u contact met ons op [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).



 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
 - [Ophalen van de configuratie](status-monitor-v2-api-get-config.md) om te bevestigen dat de instellingen correct zijn vastgelegd.
 - [De status ophalen](status-monitor-v2-api-get-status.md) te inspecteren bewaking.
