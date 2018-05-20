---
title: Installeer PowerShell voor Azure Stack | Microsoft Docs
description: Informatie over het installeren van PowerShell voor Azure-Stack.
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
ms.date: 5/17/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: cb672c8e378db80707db1b0cf77a3196e36b1eb5
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="install-powershell-for-azure-stack"></a>Installeer PowerShell voor Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Stack compatibele Azure PowerShell-modules zijn vereist voor het werken met Azure-Stack. In deze handleiding doorlopen we de stappen die nodig zijn voor het installeren van PowerShell voor Azure-Stack.

In dit artikel bevat gedetailleerde instructies voor het installeren van PowerShell voor Azure-Stack.

> [!Note]
> De volgende stappen moet PowerShell 5.0. $PSVersionTable.PSVersion uitvoeren en vergelijken te controleren of uw versie, de **belangrijke** versie.

PowerShell-opdrachten voor Azure-Stack zijn geïnstalleerd via de PowerShell-galerie. U kunt de volgende procedure om te valideren als PSGallery is geregistreerd als een opslagplaats, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht:

```PowerShell  
Get-PSRepository -Name "PSGallery"
```

Als de opslagplaats is niet geregistreerd, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht:

```PowerShell  
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
> [!Note]  
> Deze stap is vereist voor toegang tot Internet. 

## <a name="uninstall-existing-versions-of-powershell"></a>Verwijder bestaande versies van PowerShell

Voordat u de vereiste versie installeert, moet u geen eerder geïnstalleerde Azure Stack PowerShell-modules verwijderen. U kunt ze verwijderen met behulp van een van de volgende twee methoden:

 - Voor het verwijderen van de bestaande PowerShell-modules, sluit alle actieve sessies in PowerShell en voer de volgende opdracht:

  ```PowerShell
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ```

 - De mappen die met 'Azure beginnen' verwijderen uit de `C:\Program Files\WindowsPowerShell\Modules` en `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` mappen. Deze mappen verwijdert, worden eventuele bestaande PowerShell-modules.

De volgende secties beschrijven de stappen voor het installeren van PowerShell voor Azure-Stack. PowerShell kan worden geïnstalleerd op Azure-Stack die wordt gebruikt in verbonden, gedeeltelijk is verbonden, of in een scenario met niet-verbonden.

## <a name="install-powershell-in-a-connected-scenario-with-internet-connectivity"></a>Installeer PowerShell in een scenario met verbonden (met de verbinding met Internet)

Azure Stack compatibel AzureRM-modules zijn geïnstalleerd via de API-versie profielen. Azure Stack vereist de **2017-03-09-profiel** profiel API-versie die beschikbaar is door de module AzureRM.Bootstrapper installeren. Raadpleeg voor meer informatie over profielen voor API-versie en de cmdlets die is geleverd door ze naar de [API-versie Profielbeheer](user/azure-stack-version-profiles.md). Naast de modules AzureRM, moet u ook de Azure-Stack-specifieke PowerShell-modules installeren. Voer het volgende PowerShell-script voor deze modules installeren op uw ontwikkelwerkstation:

  ```PowerShell  
# Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
Install-Module -Name AzureRm.BootStrapper 

# Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

# Install Module Version 1.3.0 if Azure Stack is running 1804 at a minimum 
Install-Module -Name AzureStack -RequiredVersion 1.3.0 

# Install Module Version 1.2.11 if Azure Stack is running a lower version then 1804 
Install-Module -Name AzureStack -RequiredVersion 1.2.11 
  ```

Bevestig de installatie door de volgende opdracht uitvoeren:

```PowerShell  
Get-Module -ListAvailable | where-Object {$_.Name -like "Azs*"}
```

Als de installatie geslaagd is, worden de modules AzureRM en AzureStack weergegeven in de uitvoer.

## <a name="install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Installeer PowerShell in een niet-verbonden of een gedeeltelijk verbonden scenario (met beperkte verbinding met Internet)

In een niet-verbonden scenario moet u eerst de PowerShell-modules voor een machine die verbinding heeft met Internet te downloaden en ze vervolgens overbrengen naar de Azure-Stack Development Kit voor installatie.

> [!IMPORTANT]  
> De versie van de Azure Stack 1.3.0 PowerShell-module is voorzien van een lijst met wijzigingen op te splitsen. Upgrade van de 1.2.11 versie, Zie de [Migratiehandleiding](https://aka.ms/azspowershellmigration).

1. Meld u bij een computer waarop u beschikt over een internetverbinding en gebruik het volgende script downloaden de AzureRM en AzureStack pakketten naar de lokale computer:

   ```PowerShell  
   $Path = "<Path that is used to save the packages>"

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureRM `
     -Path $Path `
     -Force `
     -RequiredVersion 1.2.11

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureStack `
     -Path $Path `
     -Force `
     -RequiredVersion 1.3.0 
   ```

  > [!Important]  
  > Als u geen Azure-Stack met update 1804 of hoger, wijzig de **requiredversion** parameterwaarde naar `1.2.11`. 

2. Kopieer het gedownloade pakketten via een USB-apparaat.

3. Meld u aan met het werkstation en kopieer de pakketten van het USB-apparaat naar een locatie op het werkstation.

4. U moet nu registreren van deze locatie als de standaard-opslagplaats en installeren van de modules AzureRM en AzureStack uit deze opslagplaats:

   ```PowerShell
   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository `
     -Name $RepoName `
     -SourceLocation $SourceLocation `
     -InstallationPolicy Trusted

   Install-Module AzureRM `
     -Repository $RepoName

   Install-Module AzureStack `
     -Repository $RepoName 
   ```

## <a name="configure-powershell-to-use-a-proxy-server"></a>PowerShell voor het gebruik van een proxyserver configureren

In scenario's waarvoor een proxyserver voor toegang tot het internet, moet u eerst de PowerShell voor het gebruik van een bestaande proxyserver configureren.

1. Open een opdrachtprompt PowerShell.
2. Voer de volgende opdrachten uit:

````PowerShell  
  #To use Windows credentials for proxy authentication
  [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

  #Alternatively, to prompt for separate credentials that can be used for #proxy authentication

  [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
````

## <a name="next-steps"></a>Volgende stappen

 - [Azure-Stack-hulpprogramma's vanuit GitHub downloaden](azure-stack-powershell-download.md)
 - [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](user/azure-stack-powershell-configure-user.md)  
 - [De Azure-Stack-operator PowerShell-omgeving configureren](azure-stack-powershell-configure-admin.md) 
 - [Profielen voor API-versie in Azure-Stack beheren](user/azure-stack-version-profiles.md)  
