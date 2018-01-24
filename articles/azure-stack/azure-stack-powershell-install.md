---
title: Installeer PowerShell voor Azure Stack | Microsoft Docs
description: Informatie over het installeren van PowerShell voor Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 0CDD8B9B-EC32-4453-918A-D0A606C7BC10
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2018
ms.author: mabrigg
ms.openlocfilehash: 18a697813fbeb11be7a599359285f824ed804216
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="install-powershell-for-azure-stack"></a>Installeer PowerShell voor Azure Stack  

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Stack compatibele Azure PowerShell-modules zijn vereist voor het werken met Azure-Stack. In deze handleiding doorlopen we de stappen die nodig zijn voor het installeren van PowerShell voor Azure-Stack. U kunt de stappen in dit artikel vanuit de Azure-Stack Development Kit of vanaf een externe Windows-client als u via VPN-verbinding verbonden bent gebruiken.

In dit artikel bevat gedetailleerde instructies voor het installeren van PowerShell voor Azure-Stack. Echter, als u wilt snel installeren en configureren van PowerShell, kunt u het script dat is opgegeven in de [leren werken met PowerShell](azure-stack-powershell-configure-quickstart.md) onderwerp. 

> [!NOTE]
> De volgende stappen moet PowerShell 5.0. U kunt uw versie controleren door $PSVersionTable.PSVersion uitvoeren en het vergelijken van de ' ' hoofdversie.

PowerShell-opdrachten voor Azure-Stack zijn geïnstalleerd via de PowerShell-galerie. Voor het registreren van de opslagplaats PSGallery, open een PowerShell-sessie met verhoogde bevoegdheden van de development kit of van een externe Windows-client als u bent via VPN-verbinding verbonden en voer de volgende opdracht:

```powershell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

## <a name="uninstall-existing-versions-of-powershell"></a>Verwijder bestaande versies van PowerShell

Voordat u de vereiste versie installeert, moet u eventuele bestaande Azure PowerShell-modules verwijderen. U kunt ze verwijderen met behulp van een van de volgende twee methoden:

* Voor het verwijderen van de bestaande PowerShell-modules, meld u op de development kit, of op de externe Windows-client als u van plan bent een VPN-verbinding tot stand brengen. Sluit alle actieve sessies in PowerShell en voer de volgende opdracht: 

   ```powershell
   Get-Module -ListAvailable | where-Object {$_.Name -like “Azure*”} | Uninstall-Module
   ```

* Meld u op de development kit, of op de externe Windows-client als u van plan bent een VPN-verbinding tot stand brengen. De mappen die met 'Azure beginnen' verwijderen uit de `C:\Program Files\WindowsPowerShell\Modules` en `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` mappen. Deze mappen verwijdert, worden eventuele bestaande PowerShell-modules van de 'AzureStackAdmin' en 'global' gebruiker bereiken. 

De volgende secties beschrijven de stappen voor het installeren van PowerShell voor Azure-Stack. PowerShell kan worden geïnstalleerd op Azure-Stack die wordt gebruikt in verbonden, gedeeltelijk is verbonden, of in een scenario met niet-verbonden. 

## <a name="install-powershell-in-a-connected-scenario-with-internet-connectivity"></a>Installeer PowerShell in een scenario met verbonden (met de verbinding met internet)

Azure Stack compatibel AzureRM-modules zijn geïnstalleerd via de API-versie profielen. Azure Stack vereist de **2017-03-09-profiel** profiel API-versie die beschikbaar is door de module AzureRM.Bootstrapper installeren. Raadpleeg voor meer informatie over profielen voor API-versie en de cmdlets die is geleverd door ze naar de [API-versie Profielbeheer](azure-stack-version-profiles.md). Naast de modules AzureRM, moet u ook de Azure-Stack-specifieke PowerShell-modules installeren. Voer het volgende PowerShell-script voor deze modules installeren op uw ontwikkelwerkstation:

> [!IMPORTANT]
> De versie van de PowerShell-module AzureRM 1.2.11 wordt geleverd met een lijst met wijzigingen op te splitsen. Upgrade van de 1.2.10 versie, Zie de Migratiehandleiding voor op [https://aka.ms/azspowershellmigration](https://aka.ms/azspowershellmigration).

  ```powershell
  # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
  Install-Module `
    -Name AzureRm.BootStrapper

  # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
  Use-AzureRmProfile `
    -Profile 2017-03-09-profile -Force

  Install-Module `
    -Name AzureStack `
    -RequiredVersion 1.2.11
  ```

Bevestig de installatie door de volgende opdracht uitvoeren:

  ```powershell
  Get-Module `
    -ListAvailable | where-Object {$_.Name -like “Azure*”}
  ```
  Als de installatie geslaagd is, worden de modules AzureRM en AzureStack weergegeven in de uitvoer.

## <a name="install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Installeer PowerShell in een niet-verbonden of een gedeeltelijk verbonden scenario (met beperkte verbinding met internet)

In een niet-verbonden scenario moet u eerst de PowerShell-modules voor een machine die verbinding heeft met internet te downloaden en ze vervolgens overbrengen naar de Azure-Stack Development Kit voor installatie.

> [!IMPORTANT]
> De versie van de PowerShell-module AzureRM 1.2.11 wordt geleverd met een lijst met wijzigingen op te splitsen. Upgrade van de 1.2.10 versie, Zie de Migratiehandleiding voor op [https://aka.ms/azspowershellmigration](https://aka.ms/azspowershellmigration).

1. Meld u bij een computer waarop u beschikt over een internetverbinding en gebruik het volgende script downloaden de AzureRM en AzureStack pakketten naar de lokale computer:

   ```powershell
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
     -RequiredVersion 1.2.11 
   ```

2. Kopieer het gedownloade pakketten via een USB-apparaat.

3. Aanmelden bij de development kit en kopieer de pakketten van het USB-apparaat naar een locatie op de development kit. 

4. U moet nu registreren van deze locatie als de standaard-opslagplaats en installeren van de modules AzureRM en AzureStack uit deze opslagplaats:

   ```powershell
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

## <a name="next-steps"></a>Volgende stappen

* [Azure-Stack-hulpprogramma's vanuit GitHub downloaden](azure-stack-powershell-download.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](user/azure-stack-powershell-configure-user.md)  
* [De Azure-Stack-operator PowerShell-omgeving configureren](azure-stack-powershell-configure-admin.md) 
* [Profielen voor API-versie in Azure-Stack beheren](azure-stack-version-profiles.md)  
