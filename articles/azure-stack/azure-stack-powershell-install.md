---
title: Installeren van PowerShell voor Azure Stack | Microsoft Docs
description: Informatie over het installeren van PowerShell voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 08/10/2018
ms.author: sethm
ms.reviewer: thoroet
ms.openlocfilehash: 2619f959dbefba84ea1a4d5aa974055998b78b5a
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42059169"
---
# <a name="install-powershell-for-azure-stack"></a>PowerShell voor Azure Stack installeren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U moet Azure Stack compatibel PowerShell-modules om te werken met uw cloud te installeren. Compatibiliteit wordt ingeschakeld via een functie, genaamd *API profielen*.

API-profielen bieden een manier voor het beheren van versieverschillen tussen Azure en Azure Stack. Een API-versie-profiel is een set met Azure Resource Manager PowerShell-modules met specifieke API-versies. Elk cloudplatform bevat een set ondersteunde API-versieprofielen. Bijvoorbeeld, Azure Stack biedt ondersteuning voor een specifieke datum Profielversie zoals **2017-03-09-profiel**, en biedt ondersteuning voor Azure de **nieuwste** API-versie-profiel. Wanneer u een profiel installeert, worden de Azure Resource Manager PowerShell-modules die met het opgegeven profiel overeenkomen geïnstalleerd.

U kunt Azure Stack-compatibele PowerShell-modules installeren in Internet verbonden zijn, gedeeltelijk verbonden, of in een niet-verbonden scenario. In dit artikel helpen we u bij de gedetailleerde instructies voor het installeren van PowerShell voor Azure Stack voor deze scenario's.

## <a name="1-verify-your-prerequisites"></a>1. Controleer of uw vereisten

Voordat u uw get aan de slag met Azure Stack en PowerShell, moet u beschikken over een paar vereisten.

- **PowerShell-versie 5.0**  
Om te controleren of uw versie, $PSVersionTable.PSVersion uitvoeren en vergelijkt u de **belangrijke** versie. Als u geen PowerShell 5.0 hebt, volgt u de [koppeling](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell) om te upgraden naar PowerShell 5.0.

  > [!Note]  
  > PowerShell 5.0 vereist een Windows-machine.

- **Powershell een opdrachtprompt met verhoogde bevoegdheden uitvoeren**  
  U moet mogelijk om PowerShell met beheerdersbevoegdheden.

- **PowerShell Gallery-toegang**  
  U moet toegang tot de [PowerShell Gallery](https://www.powershellgallery.com). De galerie is de centrale opslagplaats voor PowerShell-inhoud. De **PowerShellGet** -module bevat cmdlets voor het detecteren, installeren, bijwerken en PowerShell-artefacten, zoals modules, DSC-resources, rolmogelijkheden en scripts vanaf de PowerShell Gallery en andere persoonlijke publiceren opslagplaatsen. Als u PowerShell in een niet-verbonden scenario gebruikt, moet u resources ophalen van een virtuele machine met een verbinding met Internet en op te slaan op een locatie die toegankelijk is voor uw niet-verbonden machine.


<!-- Nuget? -->

## <a name="2-validate-if-the-powershell-gallery-is-accessible"></a>2. Controleren of de PowerShell Gallery beschikbaar is

Valideren of PSGallery is geregistreerd als een opslagplaats.

> [!Note]  
> Deze stap is vereist voor toegang tot Internet. 

Open een verhoogde PowerShell-prompt en voer de volgende cmdlets:

````PowerShell  
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
````

Als de opslagplaats is niet geregistreerd, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht uit:

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Verwijder bestaande versies van de Azure Stack-PowerShell-modules

Voordat u de vereiste versie installeert, zorg ervoor dat alle eerder geïnstalleerde Azure Stack AzureRM PowerShell-modules te verwijderen. U kunt ze verwijderen met behulp van een van de volgende twee methoden:

1. Voor het verwijderen van de bestaande AzureRM PowerShell-modules, sluit alle actieve PowerShell-sessies en voer de volgende cmdlets:

  ````PowerShell  
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ````

2. Verwijder de mappen die met beginnen `Azure` uit de `C:\Program Files\WindowsPowerShell\Modules` en `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` mappen. Deze mappen verwijdert, worden alle bestaande PowerShell-modules.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Verbonden: Installeer PowerShell voor Azure Stack met verbinding met Internet

Azure Stack is vereist de **2017-03-09-profiel** profiel API-versie die beschikbaar door het installeren van is de **AzureRM.Bootstrapper** module. Naast de AzureRM-modules, moet u ook de Azure Stack-specifieke PowerShell-modules installeren. 

Voer het volgende PowerShell-script voor het installeren van deze modules op uw ontwikkelwerkstation:

  - **Versie 1.4.0** (Azure Stack 1804 of hoger)

    ```PowerShell  
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
    Install-Module -Name AzureRm.BootStrapper 

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

    # Install Module Version 1.4.0 if Azure Stack is running 1804 at a minimum 
    Install-Module -Name AzureStack -RequiredVersion 1.4.0
    ```

- **Versie 1.2.11** (vóór 1804)

    ```PowerShell  
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
    Install-Module -Name AzureRm.BootStrapper 

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

    # Install Module Version 1.2.11 if Azure Stack is running a lower version than 1804 
    Install-Module -Name AzureStack -RequiredVersion 1.2.11 
    ```

Bevestig de installatie door het uitvoeren van de volgende opdracht uit:

```PowerShell  
Get-Module -ListAvailable | where-Object {$_.Name -like "Azs*"}
```

Als de installatie geslaagd is, wordt de azurerm-module en AzureStack-modules worden weergegeven in de uitvoer.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Verbinding verbroken: Installeer PowerShell zonder internetverbinding

In een niet-verbonden scenario, moet u eerst de PowerShell-modules downloaden op een computer die verbinding heeft met Internet en deze vervolgens overbrengen naar de Azure Stack Development Kit voor installatie.

Aanmelden bij een computer waarop u over een internetverbinding beschikt en de volgende scripts gebruiken voor het downloaden van de Azure Resource Manager en AzureStack pakketten naar uw lokale computer, afhankelijk van uw versie van Azure Stack.


  - **Versie 1.3.0** (Azure Stack 1804 of hoger)
  
    > [!Note]  
    Een upgrade van de 1.2.11 versie, Zie de [Migratiehandleiding](https://aka.ms/azspowershellmigration).

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.4.0
    ````

  - **Versie 1.2.11** (vóór 1804)

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"

      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    ````

2. Kopieer het gedownloade pakketten via een USB-apparaat.

3. Meld u aan met het werkstation en kopieer de pakketten van het USB-apparaat naar een locatie op het werkstation.

4. Nu moet u deze locatie als de standaard-opslagplaats registreren en de azurerm-module en AzureStack-modules installeren vanuit deze opslagplaats:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module AzureRM -Repository $RepoName

   Install-Module AzureStack -Repository $RepoName 
   ```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. PowerShell voor het gebruik van een proxyserver configureren

In scenario's waarin een proxyserver voor toegang tot het Internet, moet u eerst de PowerShell voor het gebruik van een bestaande proxyserver configureren.

1. Open een verhoogde PowerShell-prompt.
2. Voer de volgende opdrachten uit:

````PowerShell  
  #To use Windows credentials for proxy authentication
  [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

  #Alternatively, to prompt for separate credentials that can be used for #proxy authentication

  [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
````

## <a name="next-steps"></a>Volgende stappen

 - [Hulpprogramma's voor Azure Stack downloaden vanuit GitHub](azure-stack-powershell-download.md)
 - [PowerShell-omgeving van de Azure Stack-gebruiker configureren](user/azure-stack-powershell-configure-user.md)  
 - [De Azure Stack-operators PowerShell-omgeving configureren](azure-stack-powershell-configure-admin.md) 
 - [Beheren van API-versieprofielen in Azure Stack](user/azure-stack-version-profiles.md)  
