---
title: Post een bericht implementatieconfiguraties voor Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Beschrijft de aanbevolen configuratiewijzigingen aanbrengen na de installatie van Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: ec5947bc68ba95a7b1e1588c444f4b28a7435f1c
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801534"
---
# <a name="post-asdk-installation-configuration-tasks"></a>Na de installatie van de ASDK configuratietaken

Na [Azure Stack Development Kit (ASDK) installeren](asdk-install.md), moet u een enkele aanbevolen na de installatie-configuratie wijzigen.

## <a name="install-azure-stack-powershell"></a>PowerShell voor Azure Stack installeren

Azure Stack compatibele Azure PowerShell-modules zijn vereist voor het werken met Azure-Stack.

PowerShell-opdrachten voor Azure-Stack zijn geïnstalleerd via de PowerShell-galerie. Voor het registreren van de opslagplaats PSGallery, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht:

``` Powershell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

U kunt profielen voor API-versie opgeven Azure Stack compatibel AzureRM modules.  API-versie-profielen bieden een manier voor het beheren van versieverschillen tussen Azure en Azure-Stack. Een API-versie-profiel is een set van AzureRM PowerShell-modules met specifieke API-versies. De **AzureRM.Bootstrapper** module die is beschikbaar via de PowerShell-galerie biedt PowerShell-cmdlets die nodig zijn voor het werken met profielen voor API-versie.

U kunt de nieuwste Azure Stack PowerShell-module installeren met of zonder internetverbinding op de computer ASDK:

> [!IMPORTANT]
> Voordat u de vereiste versie installeert, moet u [verwijdert geen bestaande Azure PowerShell-modules](.\.\azure-stack-powershell-install.md#uninstall-existing-versions-of-powershell).

- **Met een internetverbinding** van de hostcomputer ASDK. Voer de volgende PowerShell-script voor het installeren van deze modules op uw development kit installatie:

  ``` PowerShell
  # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
  Install-Module `
    -Name AzureRm.BootStrapper

  # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
  Use-AzureRmProfile `
    -Profile 2017-03-09-profile -Force

  # Install Azure Stack Module Version 1.3.0. If running a pre-1804 version of Azure Stack, change the -RequiredVersion value to 1.2.11.
  Install-Module -Name AzureStack -RequiredVersion 1.3.0 

  ```

  Als de installatie geslaagd is, worden de modules AzureRM en AzureStack weergegeven in de uitvoer.

- **Zonder internetverbinding** van de hostcomputer ASDK. In een niet-verbonden scenario moet u eerst de PowerShell-modules downloaden voor een machine die verbinding heeft met internet met behulp van de volgende PowerShell-opdrachten:

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
  # Install Azure Stack Module Version 1.3.0. If running a pre-1804 version of Azure Stack, change the -RequiredVersion value to 1.2.11.  
    -RequiredVersion 1.3.0
  ```

  Vervolgens de gedownloade pakketten kopiëren naar de computer ASDK en registreren van de locatie als de standaard-opslagplaats en installeren van de modules AzureRM en AzureStack uit deze opslagplaats:

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

## <a name="download-the-azure-stack-tools"></a>Download de Azure-Stack-tools

[AzureStack-Tools](https://github.com/Azure/AzureStack-Tools) is een GitHub-opslagplaats die als host fungeert voor PowerShell-modules voor het beheren en implementeren van resources op Azure-Stack. U kunt deze hulpprogramma's, de GitHub-opslagplaats klonen of downloaden van de map AzureStack-hulpprogramma's voor het volgende script uit te voeren:

  ```PowerShell
  # Change directory to the root directory. 
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  invoke-webrequest `
    https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  expand-archive master.zip `
    -DestinationPath . `
    -Force

  # Change to the tools directory.
  cd AzureStack-Tools-master
  ```

## <a name="validate-the-asdk-installation"></a>De installatie ASDK valideren
Om ervoor te zorgen dat de ASDK-implementatie gelukt is, kunt u de cmdlet Test-AzureStack met de volgende stappen:

1. Meld u aan als AzureStack\AzureStackAdmin op de hostcomputer ASDK.
2. Open PowerShell als beheerder (niet PowerShell ISE).
3. Uitvoeren: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Uitvoeren: `Test-AzureStack`

De tests duren een paar minuten. Als de installatie voltooid is, de uitvoer ziet er ongeveer als volgt:

![test-azurestack](media/asdk-post-deploy/test-azurestack.png)

Als er een fout is, volg de stappen voor probleemoplossing voor hulp.

## <a name="activate-the-administrator-and-tenant-portals"></a>Activeren van de portals beheerder- en tenantverkeer
Na de implementaties die gebruikmaken van Azure AD, moet u zowel de Azure-Stack-beheerder- en tenantverkeer de portals activeren. Deze activering instemt geven de Stack van Azure portal en Azure Resource Manager de juiste machtigingen (die wordt weergegeven op de pagina toestemming) voor alle gebruikers van de map.

- Voor de beheerdersportal, gaat u naar https://adminportal.local.azurestack.external/guest/signup, lees de informatie en klik vervolgens op **accepteren**. Nadat u hebt geaccepteerd, kunt u servicebeheerders die niet ook tenantbeheerders directory zijn toevoegen.

- Voor de tenantportal, gaat u naar https://portal.local.azurestack.external/guest/signup, lees de informatie en klik vervolgens op **accepteren**. Nadat u hebt geaccepteerd, kunnen gebruikers in de map kunnen aanmelden bij de tenantportal. 

> [!NOTE] 
> Als de portals zijn niet geactiveerd, kan de beheerder van de directory aanmelden en gebruik van de portals. Als een andere gebruiker zich aanmeldt, zien ze een foutbericht dat aangeeft dat de beheerder geen aan andere gebruikers machtigingen heeft. Als de beheerder systeemeigen niet tot de map behoort die voor Azure-Stack is geregistreerd, moet de Azure-Stack-map worden toegevoegd aan de activerings-URL. Bijvoorbeeld, als Azure-Stack is geregistreerd voor fabrikam.onmicrosoft.com en de gebruiker met beheerdersrechten is admin@contoso.com, gaat u naar https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com voor het activeren van de portal. 

## <a name="reset-the-password-expiration-policy"></a>Het vervalbeleid voor wachtwoord opnieuw instellen 
Om ervoor te zorgen dat het wachtwoord voor de development kit host voordat uw evaluatieversie is afgelopen periode niet verloopt, door deze stappen uit te voeren nadat u de ASDK hebt geïmplementeerd.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Het vervalbeleid voor wachtwoord wijzigen vanuit Powershell:
Voer de opdracht vanaf een verhoogde Powershell-console:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Het vervalbeleid voor wachtwoorden handmatig wijzigen:
1. Open op de host van de kit ontwikkeling **Group Policy Management** (GPMC. MMC) en navigeer naar **Group Policy Management** – **Forest: azurestack.local** – **domeinen** – **azurestack.local**.
2. Met de rechtermuisknop op **standaarddomeinbeleid** en klik op **bewerken**.
3. In de Editor voor Groepsbeleidsbeheer, gaat u naar **Computerconfiguratie** – **beleid** – **Windows-instellingen** – **beveiligingsinstellingen**– **Beleidsregels van account** – **wachtwoordbeleid**.
4. Dubbelklik in het rechterdeelvenster op **Maximum wachtwoord leeftijd**.
5. In de **Maximum wachtwoord leeftijd eigenschappen** wijziging van het dialoogvenster de **wachtwoord verloopt over** van waarde naar **180**, en klik vervolgens op **OK**.

![Console Groepsbeleidsbeheer](media/asdk-post-deploy/gpmc.png)


## <a name="next-steps"></a>Volgende stappen
[De ASDK registreren bij Azure](asdk-register.md)
