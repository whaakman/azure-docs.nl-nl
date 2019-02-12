---
title: Installeren van PowerShell voor Azure Stack | Microsoft Docs
description: Informatie over het installeren van PowerShell voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/08/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 72c4bfc4151e8120f8641c743c0da3896bd71fe1
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55995027"
---
# <a name="install-powershell-for-azure-stack"></a>PowerShell voor Azure Stack installeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als u wilt werken met uw cloud, moet u Azure Stack-compatibele PowerShell-modules installeren. Compatibiliteit wordt ingeschakeld via een functie, genaamd *API profielen*.

API-profielen bieden een manier voor het beheren van versieverschillen tussen Azure en Azure Stack. Een API-versie-profiel is een set met Azure Resource Manager PowerShell-modules met specifieke API-versies. Elk cloudplatform bevat een set ondersteunde API-versieprofielen. Azure Stack ondersteunt bijvoorbeeld de versie van een specifiek profiel zoals **2018-03-01-hybride**. Wanneer u een profiel installeert, worden de Azure Resource Manager PowerShell-modules die met het opgegeven profiel overeenkomen geïnstalleerd.

U kunt Azure Stack compatibel PowerShell-modules op Internet verbonden, gedeeltelijk verbonden of niet verbonden scenario's kunt installeren. Dit artikel helpt bij de gedetailleerde instructies voor het installeren van PowerShell voor Azure Stack voor deze scenario's.

## <a name="1-verify-your-prerequisites"></a>1. Controleer of uw vereisten

Voordat u aan de slag met Azure Stack en PowerShell, hebt u de volgende vereisten:

- **PowerShell-versie 5.0** uitvoeren om te controleren of uw versie, **$PSVersionTable.PSVersion** en vergelijkt u de **belangrijke** versie. Als u geen PowerShell 5.0 hebt, volgt u de [Windows PowerShell installeren](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]
  > PowerShell 5.0 vereist een Windows-machine.

- **Voer Powershell uit in een opdrachtprompt met verhoogde bevoegdheid**.
  U moet PowerShell ook uitvoeren met administratorbevoegdheden.

- **Toegang tot de PowerShell Gallery** moet u toegang tot de [PowerShell Gallery](https://www.powershellgallery.com). De galerie is de centrale opslagplaats voor PowerShell-inhoud. De **PowerShellGet** -module bevat cmdlets voor het detecteren, installeren, bijwerken en PowerShell-artefacten, zoals modules, DSC-resources, rolmogelijkheden en scripts vanaf de PowerShell Gallery en andere persoonlijke publiceren opslagplaatsen. Als u PowerShell in een niet-verbonden scenario gebruikt, moet u resources ophalen van een virtuele machine met een verbinding met Internet en sla ze op een locatie die toegankelijk is voor uw niet-verbonden machine.


## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. Valideren van de PowerShell Gallery toegankelijkheid

Valideren of PSGallery is geregistreerd als een opslagplaats.

> [!Note]  
> Deze stap is vereist voor toegang tot Internet.

Open een verhoogde PowerShell-prompt en voer de volgende cmdlets:

```PowerShell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
```

Als de opslagplaats is niet geregistreerd, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht uit:

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Verwijder bestaande versies van de Azure Stack-PowerShell-modules

Voordat u de vereiste versie installeert, zorg ervoor dat alle eerder geïnstalleerde Azure Stack AzureRM PowerShell-modules te verwijderen. U kunt ze verwijderen met behulp van een van de volgende twee methoden:

1. Voor het verwijderen van de bestaande AzureRM PowerShell-modules, sluit alle actieve PowerShell-sessies en voer de volgende cmdlets:

    ```PowerShell
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose
    ```
    Als een fout optreedt, zoals 'de module is al in gebruik', sluit de PowerShell-sessies die gebruikmaken van de modules en het bovenstaande script opnieuw uitvoeren.

2. Verwijder de mappen die met beginnen `Azure` of `Azs.` uit de `C:\Program Files\WindowsPowerShell\Modules` en `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` mappen. Deze mappen verwijdert, worden alle bestaande PowerShell-modules.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Verbonden: PowerShell voor Azure Stack installeren met een internetverbinding

Azure Stack is vereist de **2018-03-01-hybride** profiel van de API-version voor Azure Stack-versie 1808 of hoger. Het profiel is beschikbaar door het installeren van de **AzureRM.Bootstrapper** module. Daarnaast kunnen moet de AzureRM-modules, u ook installeren de Azure Stack-specifieke PowerShell-modules. Het profiel voor API-versie en de Azure Stack-PowerShell-modules die u nodig hebt zal afhankelijk zijn van de versie van Azure Stack uw worden uitgevoerd.

Installatie heeft drie stappen:

1. PowerShell voor Azure Stack, afhankelijk van uw versie van Azure Stack installeren
2. Extra opslag-functies inschakelen
3. Controleer of de installatie van PowerShell

### <a name="install-azure-stack-powershell"></a>PowerShell voor Azure Stack installeren

Voer het volgende PowerShell-script voor het installeren van deze modules op uw ontwikkelwerkstation:

- Azure Stack 1901 of hoger.

    ```PowerShell
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Install-Module AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.0
    ```

    > [!Note]  
    > De Azure Stack-moduleversie 1.7.0 is een belangrijke wijziging. Voor het migreren van Azure Stack 1.6.0 raadpleegt u de [Migratiehandleiding](https://aka.ms/azspshmigration170).

- Azure Stack 1811 of een eerdere versie.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

- Azure Stack 1809 of een eerdere versie.

    ```PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

> [!Note]  
> Upgrade uitvoeren van Azure PowerShell via de **2017-03-09-profiel** naar **2018-03-01-hybride**, raadpleegt u de [Migratiehandleiding](https://github.com/azure/azure-powershell/blob/AzureRM/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

### <a name="enable-additional-storage-features"></a>Extra opslag-functies inschakelen

Maken gebruik van de extra opslag-functies (die worden vermeld in de sectie verbonden), download en installeer de volgende pakketten ook.

```PowerShell
# Install the Azure.Storage module version 4.5.0
Install-Module -Name Azure.Storage -RequiredVersion 4.5.0 -Force -AllowClobber

# Install the AzureRm.Storage module version 5.0.4
Install-Module -Name AzureRM.Storage -RequiredVersion 5.0.4 -Force -AllowClobber

# Remove incompatible storage module installed by AzureRM.Storage
Uninstall-Module Azure.Storage -RequiredVersion 4.6.1 -Force

# Load the modules explicitly specifying the versions
Import-Module -Name Azure.Storage -RequiredVersion 4.5.0
Import-Module -Name AzureRM.Storage -RequiredVersion 5.0.4
```

### <a name="confirm-the-installation-of-powershell"></a>Controleer of de installatie van PowerShell

Bevestig de installatie door het uitvoeren van de volgende opdracht uit:

```PowerShell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

Als de installatie geslaagd is, wordt de azurerm-module en AzureStack-modules worden weergegeven in de uitvoer.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Verbinding verbroken: PowerShell installeren zonder internetverbinding

In een niet-verbonden scenario, moet u eerst de PowerShell-modules downloaden op een computer die verbinding heeft met Internet en deze vervolgens overbrengen naar de Azure Stack Development Kit voor installatie.

Aanmelden bij een computer met een internetverbinding en de volgende scripts gebruiken voor het downloaden van de Azure Resource Manager en AzureStack pakketten, afhankelijk van uw versie van Azure Stack.

Installatie bestaat uit vier stappen:

1. PowerShell voor Azure Stack installeren op een computer verbonden
2. Extra opslag-functies inschakelen
3. De PowerShell-pakketten naar uw niet-verbonden werkstation transport
4. Controleer of de installatie van PowerShell


### <a name="install-azure-stack-powershell"></a>PowerShell voor Azure Stack installeren

- Azure Stack 1901 of hoger.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.4.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.0
    ```

    > [!Note]  
    > De Azure Stack-moduleversie 1.7.0 is een belangrijke wijziging. Voor het migreren van AzureStack 1.6.0 raadpleegt u de [Migratiehandleiding](https://github.com/Azure/azure-powershell/tree/AzureRM/documentation/migration-guides/Stack).


  - Azure Stack 1811 of een eerdere versie.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.6.0
    ```

  - Azure Stack 1809 of een eerdere versie.

    ```PowerShell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ```

### <a name="enable-additional-storage-features"></a>Extra opslag-functies inschakelen

Maken gebruik van de extra opslag-functies (die worden vermeld in de sectie verbonden), download en installeer de volgende pakketten ook.

```PowerShell
$Path = "<Path that is used to save the packages>"
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name Azure.Storage -Path $Path -Force -RequiredVersion 4.5.0
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRm.Storage -Path $Path -Force -RequiredVersion 5.0.4
```

### <a name="add-your-packages-to-your-workstation"></a>Voeg uw pakketten toe aan uw werkstation

1. Kopieer het gedownloade pakketten naar een USB-apparaat.

2. Aanmelden bij niet-verbonden werkstation en kopieer de pakketten van het USB-apparaat naar een locatie op het werkstation.

3. Nu registreren van deze locatie als de standaard-opslagplaats en de azurerm-module en AzureStack-modules installeren vanaf deze opslagplaats:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

### <a name="confirm-the-installation-of-powershell"></a>Controleer of de installatie van PowerShell

Bevestig de installatie door het uitvoeren van de volgende opdracht uit:

```PowerShell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. PowerShell voor het gebruik van een proxyserver configureren

In scenario's waarin een proxyserver voor toegang tot het Internet, moet u eerst PowerShell voor het gebruik van een bestaande proxyserver configureren:

1. Open een verhoogde PowerShell-prompt.
2. Voer de volgende opdrachten uit:

   ```PowerShell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Volgende stappen

 - [Hulpprogramma's voor Azure Stack downloaden vanuit GitHub](azure-stack-powershell-download.md)
 - [PowerShell-omgeving van de Azure Stack-gebruiker configureren](user/azure-stack-powershell-configure-user.md)
 - [De Azure Stack-operators PowerShell-omgeving configureren](azure-stack-powershell-configure-admin.md)
 - [Beheren van API-versieprofielen in Azure Stack](user/azure-stack-version-profiles.md)