---
title: Azure Status Monitor v2 gedetailleerde instructies | Microsoft Docs
description: Gedetailleerde instructies voor het aan de slag met Status Monitor v2. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET web-apps die on-premises, in virtuele machines, of op Azure worden gehost.
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
ms.openlocfilehash: d0960c749d74903acc778c0f21d5c49f380195ae
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734186"
---
# <a name="status-monitor-v2-detailed-instructions"></a>Status Monitor v2: Gedetailleerde instructies

Dit artikel wordt beschreven hoe moet worden vrijgegeven met de PowerShell Gallery en downloaden de ApplicationMonitor-module.
Hierin worden de meest voorkomende parameters die u moet aan de slag.
Dit omvat ook handmatig instructies in het geval u geen toegang tot internet hebt.

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="get-an-instrumentation-key"></a>Een instrumentatiesleutel ophalen

Als u wilt beginnen, moet u een instrumentatiesleutel. Zie voor meer informatie, [maken van een Application Insights-resource](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-admin-with-an-elevated-execution-policy"></a>PowerShell uitvoeren als beheerder met een beleid voor uitvoering met verhoogde bevoegdheden

**Uitvoeren als beheerder**

PowerShell moet beheerdersrechten wijzigingen aanbrengen in uw computer.

**Uitvoeringsbeleid**
- Beschrijving: PowerShell-scripts uitgevoerd is standaard uitgeschakeld. Het is raadzaam dat RemoteSigned scripts voor alleen het huidige bereik.
- Referentie: [Over Uitvoeringsbeleidsregels](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) en [Set-ExecutionPolicy](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
).
- Opdracht: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`.
- Optionele parameter:
    - `-Force`. Omzeilt de bevestiging wordt gevraagd.

**Voorbeeld van fouten**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>Vereisten voor PowerShell

Controle uitvoeren op uw exemplaar van PowerShell door uit te voeren de `$PSVersionTable` opdracht.
Met deze opdracht wordt de volgende uitvoer gegenereerd:


```
Name                           Value
----                           -----
PSVersion                      5.1.17763.316
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.17763.316
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Deze instructies zijn geschreven en getest op een computer met Windows 10 en de hierboven vermelde versies.

## <a name="prerequisites-for-powershell-gallery"></a>Vereisten voor de PowerShell Gallery

Deze stappen wordt de server voorbereiden op modules downloaden vanuit PowerShell Gallery.

> [!NOTE] 
> PowerShell Gallery wordt ondersteund op Windows 10, Windows Server 2016 en PowerShell 6.
> Zie voor meer informatie over eerdere versies [PowerShellGet installeren](https://docs.microsoft.com/powershell/gallery/installing-psget).


1. Voer PowerShell uit als beheerder met een beleid voor uitvoering met verhoogde bevoegdheden.
2. Installeer de provider van NuGet-pakket.
    - Beschrijving: U moet deze provider om te communiceren met op basis van een NuGet-opslagplaatsen, zoals PowerShell Gallery.
    - Referentie: [Install-PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6).
    - Opdracht: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`.
    - Optionele parameters:
        - `-Proxy`. Hiermee geeft u een proxyserver voor de aanvraag.
        - `-Force`. Omzeilt de bevestiging wordt gevraagd.
    
    U ontvangt dit bericht als NuGet niet is ingesteld:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. PowerShell Gallery configureren als een vertrouwde opslagplaats.
    - Beschrijving: PowerShell Gallery is standaard een niet-vertrouwde opslagplaats.
    - Referentie: [Set-PSRepository](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6).
    - Opdracht: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`.
    - Optionele parameter:
        - `-Proxy`. Hiermee geeft u een proxyserver voor de aanvraag.

    U ontvangt dit bericht als PowerShell Gallery niet-vertrouwd:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    U kunt deze wijziging bevestigen en alle PSRepositories controleren door het uitvoeren van de `Get-PSRepository` opdracht.

4. Installeer de nieuwste versie van PowerShellGet.
    - Beschrijving: Deze module bevat de hulpprogramma's gebruikt om op te halen van andere modules vanuit PowerShell Gallery. Versie 1.0.0.1 wordt geleverd met Windows 10 en Windows Server. Versie 1.6.0 of hoger vereist. Uitvoeren om te bepalen welke versie is geïnstalleerd, de `Get-Command -Module PowerShellGet` opdracht.
    - Referentie: [PowerShellGet installeren](https://docs.microsoft.com/powershell/gallery/installing-psget).
    - Opdracht: `Install-Module -Name PowerShellGet`.
    - Optionele parameters:
        - `-Proxy`. Hiermee geeft u een proxyserver voor de aanvraag.
        - `-Force`. De waarschuwing 'al geïnstalleerd' omzeilt en installeert u de meest recente versie.

    U ontvangt deze fout als u niet de nieuwste versie van PowerShellGet:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. Start opnieuw op PowerShell. U kunt de nieuwe versie niet laden in de huidige sessie. Nieuwe PowerShell-sessies wordt de meest recente versie van PowerShellGet geladen.

## <a name="download-and-install-the-module-via-powershell-gallery"></a>Download en installeer de module via PowerShell Gallery

Deze stappen wordt de Az.ApplicationMonitor-module gedownload vanuit PowerShell Gallery.

1. Zorg ervoor dat aan alle vereisten voor de PowerShell Gallery wordt voldaan.
2. Voer PowerShell uit als beheerder met een beleid voor uitvoering met verhoogde bevoegdheden.
3. Installeer de Az.ApplicationMonitor-module.
    - Referentie: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6).
    - Opdracht: `Install-Module -Name Az.ApplicationMonitor`.
    - Optionele parameters:
        - `-Proxy`. Hiermee geeft u een proxyserver voor de aanvraag.
        - `-AllowPrerelease`. Kan installatie van alpha- en Bètaservices releases.
        - `-AcceptLicense`. De vraag 'Licentie accepteren' omzeilt
        - `-Force`. De waarschuwing "Niet-vertrouwde opslagplaats" omzeilt.

## <a name="download-and-install-the-module-manually-offline-option"></a>Download en installeer de module handmatig (offline optie)

Als voor een bepaalde reden niet kan u verbinding met de PowerShell-module maken, kunt u handmatig downloaden en installeren van de module Az.ApplicationMonitor.

### <a name="manually-download-the-latest-nupkg-file"></a>Handmatig de meest recente nupkg-bestand downloaden

1. Ga naar https://www.powershellgallery.com/packages/Az.ApplicationMonitor.
2. Selecteer de meest recente versie van het bestand in de **versiegeschiedenis** tabel.
3. Onder **installatieopties**, selecteer **handmatig downloaden**.

### <a name="option-1-install-into-a-powershell-modules-directory"></a>Optie 1: In de directory van een PowerShell-modules installeren
De handmatig gedownloade PowerShell-module installeren in een PowerShell-map zodat deze kunnen worden gedetecteerd door de PowerShell-sessies.
Zie voor meer informatie, [een PowerShell-Module installeren](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module).


#### <a name="unzip-nupkg-as-a-zip-file-by-using-expand-archive-v1010"></a>Pak nupkg als een zip-bestand uit met behulp van uit te breiden-archief (v1.0.1.0)

- Beschrijving: De basisversie van Microsoft.PowerShell.Archive (v1.0.1.0) kan niet nupkg bestanden uitpakken. Naam van het bestand met de extensie .zip.
- Referentie: [Expand-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6).
- Opdracht:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.3.0-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-by-using-expand-archive-v1100"></a>Pak nupkg uit met behulp van uit te breiden-archief (v1.1.0.0)

- Beschrijving: Gebruik een actuele versie van uit te breiden-archief bestanden uit te pakken nupkg zonder te hoeven wijzigen van de extensie.
- Referentie: [Vouw archief](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) en [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0).
- Opdracht:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-nupkg-manually"></a>Optie 2: Uitpakken en nupkg handmatig importeren
De handmatig gedownloade PowerShell-module installeren in een PowerShell-map zodat deze kunnen worden gedetecteerd door de PowerShell-sessies.
Zie voor meer informatie, [een PowerShell-Module installeren](https://docs.mircrosoft.com/powershell/developer/module/installing-a-powershell-module).

Als u de module in een andere adreslijst die installeert, moet u handmatig de module importeren met behulp van [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6).

> [!IMPORTANT] 
> DLL-bestanden worden geïnstalleerd via de relatieve paden.
> De inhoud van het pakket in uw directory beoogde runtime Store en bevestigt u dat machtigingen voor lezen, maar niet schrijven toestaan.

1. De extensie wijzigen in '.zip' en pak de inhoud van het pakket in uw beoogde installatiemap.
2. Het bestandspad van Az.ApplicationMonitor.psd1 vinden.
3. Voer PowerShell uit als beheerder met een beleid voor uitvoering met verhoogde bevoegdheden.
4. Laden van de module met behulp van de `Import-Module Az.ApplicationMonitor.psd1` opdracht.
    

## <a name="route-traffic-through-a-proxy"></a>Verkeer routeren via een proxy

Wanneer u een computer op het intranet van uw persoonlijke bewaken, moet u voor het routeren van HTTP-verkeer via een proxy.

Ondersteuning voor de PowerShell-opdrachten voor het downloaden en installeren van Az.ApplicationMonitor vanuit de PowerShell Gallery een `-Proxy` parameter.
Bekijk de voorgaande instructies bij het schrijven van scripts voor uw installatie.

De Application Insights SDK moet de telemetrie van uw app naar Microsoft verzenden. Het is raadzaam om de proxy-instellingen te configureren voor uw app in het web.config-bestand. Zie voor meer informatie, [Application Insights-Veelgestelde vragen: Proxy-passthrough](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough).


## <a name="enable-monitoring"></a>Bewaking inschakelen

Gebruik de `Enable-ApplicationInsightsMonitoring` opdracht bewaking wilt inschakelen.

Zie de [API-verwijzing](status-monitor-v2-api-enable-monitoring.md) voor een gedetailleerde beschrijving van het gebruik van deze cmdlet.



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
