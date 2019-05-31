---
title: Azure Status Monitor v2 gedetailleerde instructies | Microsoft Docs
description: Gedetailleerde instructies voor het aan de slag met Status Monitor v2. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
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
ms.openlocfilehash: 6eca2b47c2362f34415db8b4f335f3089babc58b
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255883"
---
# <a name="status-monitor-v2-detailed-instructions"></a>Status Monitor v2 gedetailleerde instructies

Deze informatie hoe document moet worden vrijgegeven met de PowerShell Gallery en downloaden de ApplicationMonitor-Module. Gedocumenteerde van de meest voorkomende parameters die nodig zijn om te beginnen.
We hebben ook een handmatige instructies opgenomen in het geval dat toegang tot internet is niet beschikbaar.

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="instrumentation-key"></a>Instrumentatiesleutel

Als u wilt beginnen, moet u een instrumentatiesleutel hebben. Lees voor meer informatie, [maken van een Application Insights-resource](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-administrator-with-an-elevated-execution-policy"></a>PowerShell als beheerder met een beleid voor uitvoering met verhoogde bevoegdheden uitvoeren

**Als Administrator uitvoeren**: 
- Beschrijving: PowerShell moet de beheerdersmachtigingen wijzigingen aanbrengen in uw computer.

**Het uitvoeringsbeleid**:
- Beschrijving: Standaard worden PowerShell-scripts uitgevoerd, uitgeschakeld. U wordt aangeraden waardoor RemoteSigned scripts alleen het huidige bereik.
- Referentie: [Over Uitvoeringsbeleidsregels](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) en [Set-ExecutionPolicy](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
)
- Cmd: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`
- Optionele Parameters:
    - `-Force` Deze wordt bevestiging wordt gevraagd overgeslagen.

**Voorbeeld van de fouten:**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>Vereisten voor PowerShell

Controle uitvoeren op uw huidige versie van PowerShell met de opdracht: `$PSVersionTable`.
De opdracht wordt de volgende uitvoer gegenereerd:


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

Deze instructies zijn geschreven en getest op een Windows 10-computer met de hierboven vermelde versies.

## <a name="prerequisites-for-powershell-gallery"></a>Vereisten voor de PowerShell Gallery

Deze stappen wordt de server voorbereiden op modules downloaden van de PowerShell Gallery.

> [!NOTE] 
> Ondersteuning voor PowerShell Gallery is beschikbaar op Windows 10, Windows Server 2016 en PowerShell 6. Raadpleeg dit document voor oudere versies: [PowerShellGet installeren](https://docs.microsoft.com/powershell/gallery/installing-psget)


1. PowerShell als Administrator uitvoeren met een beleid voor uitvoering met verhoogde bevoegdheden.
2. NuGet-pakket-provider 
    - Beschrijving: Deze provider is vereist om te communiceren met op basis van een NuGet-opslagplaatsen, zoals PowerShellGallery
    - Referentie: [Install-PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6)
    - Cmd: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`
    - Optionele Parameters:
        - `-Proxy` Hiermee geeft u een proxyserver voor de aanvraag.
        - `-Force` Deze wordt bevestiging wordt gevraagd overgeslagen. 
    
    U ontvangt dit bericht als NuGet niet is ingesteld:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. Vertrouwde opslagplaatsen
    - Beschrijving: PowerShellGallery is standaard een niet-vertrouwde opslagplaats.
    - Referentie: [Set-PSRepository](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6)
    - Cmd: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`
    - Optionele Parameters:
        - `-Proxy` Hiermee geeft u een proxyserver voor de aanvraag.

    U ontvangt dit bericht als PowerShell Gallery niet-vertrouwd:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    - Kan deze wijziging bevestigen en alle PSRepositories controleren door het uitvoeren van de cmd: `Get-PSRepository`

4. PowerShellGet-versie 
    - Beschrijving: Deze module bevat de hulpprogramma's gebruikt om op te halen van andere modules vanuit PowerShell Gallery. V1.0.0.1 wordt geleverd met Windows 10 en Windows Server. Minimale versie vereist is v1.6.0. Om te controleren welke versie is geïnstalleerd, voert u de opdracht uit: `Get-Command -Module PowerShellGet`
    - Referentie: [PowerShellGet installeren](https://docs.microsoft.com/powershell/gallery/installing-psget)
    - Cmd: `Install-Module -Name PowerShellGet`
    - Optionele Parameters:
        - `-Proxy` Hiermee geeft u een proxyserver voor de aanvraag.
        - `-Force` Dit wordt de waarschuwing 'al geïnstalleerd' negeren en installeer de nieuwste versie.

    U ontvangt deze fout als u de nieuwste versie van PowerShellGet niet gebruikt:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. Start opnieuw op PowerShell. Het is niet mogelijk om te laden van de nieuwe versie in de huidige sessie. Een nieuwe Powershell-sessies moet de meest recente PowerShellGet geladen.

## <a name="download--install-via-powershell-gallery"></a>Downloaden en installeren via de PowerShell Gallery

Deze stappen wordt de Az.ApplicationMonitor-module gedownload vanuit PowerShell Gallery.

1. Vereisten voor de PowerShell Gallery zijn vereist.
2. PowerShell als Administrator uitvoeren met een beleid voor uitvoering met verhoogde bevoegdheden.
3. De Az.ApplicationMonitor-Module installeren
    - Referentie: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6)
    - Cmd: `Install-Module -Name Az.ApplicationMonitor`
    - Optionele Parameters:
        - `-Proxy` Hiermee geeft u een proxyserver voor de aanvraag.
        - `-AllowPrerelease` Hierdoor kunnen alpha- en Bètaservices versies installeren.
        - `-AcceptLicense` Hiermee wordt de prompt 'Licentie accepteren' overgeslagen
        - `-Force` Hiermee wordt de waarschuwing "Niet-vertrouwde opslagplaats" genegeerd

## <a name="download--install-manually-offline-option"></a>Downloaden en handmatig installeren (offline optie)

Als voor een bepaalde reden niet kan u verbinding met de PowerShell-Module maakt, kunt u handmatig downloaden en installeren van de module Az.ApplicationMonitor.

### <a name="manually-download-the-latest-nupkg"></a>De meest recente nupkg handmatig downloaden

1. Navigeer naar: https://www.powershellgallery.com/packages/Az.ApplicationMonitor
2. Selecteer de meest recente versie van de versiegeschiedenis.
3. Zoek 'installatieopties' en selecteer 'Handmatig downloaden'.

### <a name="option-1-install-into-powershell-modules-directory"></a>Optie 1: in de directory van PowerShell-modules installeren
Installeer de handmatig gedownloade PowerShell-Module naar een PowerShell-map, zodat deze gedetecteerd door de PowerShell-sessies worden kan.
Zie voor meer informatie: [Een PowerShell-Module installeren](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module)


#### <a name="unzip-nupkg-as-zip-using-expand-archive-v1010"></a>Pak deze uit nupkg als zip met behulp van uit te breiden-archief (v1.0.1.0)

- Beschrijving: De basisversie van Microsoft.PowerShell.Archive (v1.0.1.0) kan niet nupkg bestanden uitpakken. Naam van het bestand met de extensie .zip '-'.
- Referentie: [Expand-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6)
- Cmd: 

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-using-expand-archive-v1100"></a>Pak nupkg uit te breiden-archief (v1.1.0.0) gebruiken.

- Beschrijving: Gebruik een actuele versie van uit te breiden-archief uitpakken nupkgs zonder de naam van de extensie. 
- Referentie: [Vouw archief](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) en [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0)
- Cmd:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-manually"></a>Optie 2: uitpakken en handmatig importeren
Installeer de handmatig gedownloade PowerShell-Module naar een PowerShell-map, zodat deze gedetecteerd door de PowerShell-sessies worden kan.
Zie voor meer informatie: [Een PowerShell-Module installeren](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module)

Als in een andere adreslijst die installeert, moet u handmatig importeren van de module met behulp [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6)

> [!IMPORTANT] 
> Installatie wordt DLL-bestanden geïnstalleerd via de relatieve paden. De inhoud van dit pakket in uw directory beoogde runtime Store en bevestigt u dat machtigingen voor lezen, maar niet schrijven toestaan.

1. De extensie wijzigen in '.zip' en uitpakken van de inhoud van pakket in uw beoogde installatiemap.
2. Het pad naar "Az.ApplicationMonitor.psd1" zoeken.
3. PowerShell als Administrator uitvoeren met een beleid voor uitvoering met verhoogde bevoegdheden. 
4. Laden van de module met de opdracht: `Import-Module Az.ApplicationMonitor.psd1`.
    

## <a name="proxy"></a>Proxy

Bij het bewaken van een virtuele machine op een particulier intranet, wordt het nodig zijn voor het routeren van http-verkeer via een proxy zijn.

De PowerShell-opdrachten om te downloaden en installeren van de Az.ApplicationMonitor vanuit de PowerShell Gallery bieden ondersteuning voor een `-Proxy` parameter.
Controleer de bovenstaande instructies wanneer uw installatiescripts te schrijven.

De Application Insights SDK moet van uw toepassing om uitzonderingstelemetrie te verzenden naar Microsoft. Het is raadzaam om de proxy-instellingen voor uw toepassing configureren in uw web.config. Zie [Application Insights Veelgestelde vragen over: Proxy-Passthrough](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough) voor meer informatie.


## <a name="enable-monitoring"></a>Bewaking inschakelen 

Cmd: `Enable-ApplicationInsightsMonitoring`

Bekijk onze [API-verwijzing](status-monitor-v2-api-enable-monitoring.md) voor een gedetailleerde beschrijving van het gebruik van deze cmdlet. 



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
