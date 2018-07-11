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
ms.date: 07/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: e2785b0beeab042d4b1ad9a9eb5f545dbb58b8b9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38487498"
---
# <a name="install-powershell-for-azure-stack"></a>PowerShell voor Azure Stack installeren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack compatibele Azure PowerShell-modules zijn vereist voor het werken met Azure Stack. In deze handleiding helpen we u bij de stappen die nodig zijn voor het installeren van PowerShell voor Azure Stack.

In dit artikel bevat gedetailleerde instructies voor het installeren van PowerShell voor Azure Stack.

> [!Note]  
> De volgende stappen moet PowerShell 5.0. Om te controleren of uw versie, $PSVersionTable.PSVersion uitvoeren en vergelijkt u de **belangrijke** versie.

PowerShell-opdrachten voor Azure Stack zijn geïnstalleerd via de PowerShell Gallery. U kunt de volgende procedure gebruiken om te valideren of PSGallery is geregistreerd als een opslagplaats, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht uit:

```PowerShell  
Get-PSRepository -Name "PSGallery"
```

Als de opslagplaats is niet geregistreerd, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht uit:

```PowerShell  
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
> [!Note]  
> Deze stap is vereist voor toegang tot Internet. 

## <a name="uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>Verwijder bestaande versies van de Azure Stack-PowerShell-modules

Voordat u de vereiste versie installeert, zorg ervoor dat alle eerder geïnstalleerde Azure Stack AzureRM PowerShell-modules te verwijderen. U kunt ze verwijderen met behulp van een van de volgende twee methoden:

 - Voor het verwijderen van de bestaande AzureRM PowerShell-modules, sluit u alle actieve PowerShell-sessies en voer de volgende opdracht uit:

  ```PowerShell
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ```

 - Verwijderen van alle mappen die met 'Azure beginnen' uit de `C:\Program Files\WindowsPowerShell\Modules` en `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` mappen. Deze mappen verwijdert, worden alle bestaande PowerShell-modules.

De volgende secties worden de stappen die nodig zijn voor het installeren van PowerShell voor Azure Stack. PowerShell kan worden geïnstalleerd op Azure Stack die wordt beheerd die zijn verbonden, gedeeltelijk verbonden of in een niet-verbonden scenario.

## <a name="install-the-azure-stack-powershell-modules-in-a-connected-scenario-with-internet-connectivity"></a>De Azure Stack-PowerShell-modules installeren in een scenario voor verbonden (met een internetverbinding)

Azure Stack compatibel AzureRM-modules zijn geïnstalleerd via de API-versieprofielen. Azure Stack is vereist de **2017-03-09-profiel** profiel API-versie die door het installeren van de module AzureRM.Bootstrapper beschikbaar is. Voor meer informatie over API-versieprofielen en de beschikbare deze cmdlets, raadpleegt u de [API-versieprofielen beheren](user/azure-stack-version-profiles.md). Naast de AzureRM-modules, moet u ook de Azure Stack-specifieke PowerShell-modules installeren. Voer het volgende PowerShell-script voor het installeren van deze modules op uw ontwikkelwerkstation:

  ```PowerShell  
# Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
Install-Module -Name AzureRm.BootStrapper 

# Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

# Install Module Version 1.3.0 if Azure Stack is running 1804 at a minimum 
Install-Module -Name AzureStack -RequiredVersion 1.3.0 

# Install Module Version 1.2.11 if Azure Stack is running a lower version than 1804 
Install-Module -Name AzureStack -RequiredVersion 1.2.11 
  ```

Om te bevestigen dat de installatie, moet u de volgende opdracht uitvoeren:

```PowerShell  
Get-Module -ListAvailable | where-Object {$_.Name -like "Azs*"}
```

Als de installatie geslaagd is, wordt de azurerm-module en AzureStack-modules worden weergegeven in de uitvoer.

## <a name="install-the-azure-stack-powershell-modules-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>De Azure Stack-PowerShell-modules installeren in een niet-verbonden of een gedeeltelijk verbonden scenario (met beperkte verbinding met Internet)

In een niet-verbonden scenario, moet u eerst de PowerShell-modules downloaden op een computer die verbinding heeft met Internet en deze vervolgens overbrengen naar de Azure Stack Development Kit voor installatie.

> [!IMPORTANT]  
> De versie van de Azure Stack 1.3.0 PowerShell-module wordt geleverd met een lijst met belangrijke wijzigingen. Een upgrade van de 1.2.11 versie, Zie de [Migratiehandleiding](https://aka.ms/azspowershellmigration).

1. Meld u aan bij een computer waar u over een internetverbinding beschikt en gebruik het volgende script om te downloaden van de AzureRM en AzureStack pakketten naar uw lokale computer:

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
  > Als u niet Azure Stack met update 1804 of hoger uitvoert, wijzigt u de **requiredversion** parameterwaarde `1.2.11`. 

2. Kopieer het gedownloade pakketten via een USB-apparaat.

3. Meld u aan met het werkstation en kopieer de pakketten van het USB-apparaat naar een locatie op het werkstation.

4. Nu moet u deze locatie als de standaard-opslagplaats registreren en de azurerm-module en AzureStack-modules installeren vanuit deze opslagplaats:

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

In scenario's waarin een proxyserver voor toegang tot het internet, moet u eerst de PowerShell voor het gebruik van een bestaande proxyserver configureren.

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
