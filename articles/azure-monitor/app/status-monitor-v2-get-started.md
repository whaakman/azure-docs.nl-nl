---
title: Azure Status Monitor v2 aan de slag | Microsoft Docs
description: Een snelstartgids voor Status Monitor v2. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
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
ms.openlocfilehash: 3dcd50c3aa516f2af40c1e28a36a8039773e069c
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255059"
---
# <a name="getting-started-with-status-monitor-v2"></a>Aan de slag met Status Monitor v2

Dit document bevat de opdrachten in de quickstart om te werken voor de meeste omgevingen wordt verwacht. Deze instructies zijn afhankelijk van de PowerShell Gallery voor het distribueren van updates. Deze opdrachten ondersteuning bieden voor de PowerShell `-Proxy` parameter.

Bekijk onze [gedetailleerde instructies](status-monitor-v2-detailed-instructions.md) -pagina voor een uitleg van deze opdrachten, de instructies voor het aanpassen en oplossen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="download--install-via-powershell-gallery"></a>Downloaden en installeren via de PowerShell Gallery

### <a name="install-prerequisites"></a>Vereiste onderdelen installeren
PowerShell als Administrator uitvoeren
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
Afsluiten van PowerShell

### <a name="install-status-monitor-v2"></a>Installatiestatus van het bewaken van v2
PowerShell als Administrator uitvoeren
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>Bewaking inschakelen
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download--install-manually-offline-option"></a>Downloaden en handmatig installeren (offline optie)
### <a name="manual-download"></a>Handmatig worden gedownload
Handmatig downloaden de nieuwste versie van de Module op basis van: https://www.powershellgallery.com/packages/Az.ApplicationMonitor

### <a name="unzip-and-install-status-monitor-v2"></a>Pak deze uit en installeer Status Monitor v2
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.2.1-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>Bewaking inschakelen
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
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

- Controleer de [gedetailleerde instructies](status-monitor-v2-detailed-instructions.md) voor een uitleg van de opdrachten in deze handleiding.
- Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
