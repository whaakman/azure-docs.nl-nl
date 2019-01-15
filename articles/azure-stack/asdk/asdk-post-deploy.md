---
title: Post-implementatieconfiguraties voor de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Hierin wordt beschreven aanbevolen configuratiewijzigingen na de installatie van de Azure Stack Development Kit (ASDK).
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
ms.date: 10/10/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 756d07562503fa94ea2ea737a5f610d8bf3101aa
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54260359"
---
# <a name="post-asdk-installation-configuration-tasks"></a>Na de installatie van de ASDK configuratietaken

Na [installeren van de Azure Stack Development Kit (ASDK)](asdk-install.md), moet u enkele aanbevolen na de installatie configuratie wijzigingen terwijl u aangemeld als AzureStack\AzureStackAdmin op de hostcomputer ASDK aanbrengen. 

## <a name="install-azure-stack-powershell"></a>PowerShell voor Azure Stack installeren

Azure Stack compatibele Azure PowerShell-modules zijn vereist voor het werken met Azure Stack.

PowerShell-opdrachten voor Azure Stack zijn geïnstalleerd via de PowerShell Gallery. Voor het registreren van de opslagplaats PSGallery, open een PowerShell-sessie met verhoogde bevoegdheden en voer de volgende opdracht uit:

``` Powershell
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

U kunt de API-versieprofielen gebruiken om op te geven van de Azure Stack-compatibele AzureRM-modules.  API-versieprofielen bieden een manier voor het beheren van versieverschillen tussen Azure en Azure Stack. Een API-versie-profiel is een set van AzureRM PowerShell-modules met specifieke API-versies. De **AzureRM.Bootstrapper** -module die is beschikbaar via de PowerShell Gallery biedt PowerShell-cmdlets die nodig zijn voor het werken met API-versieprofielen.

U kunt de meest recente PowerShell voor Azure Stack-module installeren met of zonder internetverbinding met de hostcomputer ASDK:

> [!IMPORTANT]
> Voordat u de vereiste versie installeert, zorg ervoor dat u [verwijderen van een bestaande Azure PowerShell-modules](../azure-stack-powershell-install.md#3-uninstall-existing-versions-of-the-azure-stack-powershell-modules).

- **Met een internetverbinding** van de hostcomputer ASDK. Voer de volgende PowerShell-script voor het installeren van deze modules op uw development kit-installatie:

  - Azure Stack 1808 of hoger:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.5.0.
    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

  - Azure Stack 1807 of eerder:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    
    # Install Azure Stack Module Version 1.4.0.
    Install-Module -Name AzureStack -RequiredVersion 1.4.0
    ```

  - Azure Stack 1803 of eerder:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
      Install-Module -Name AzureRm.BootStrapper

      # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
      Use-AzureRmProfile -Profile 2017-03-09-profile -Force

      # Install Azure Stack Module Version 1.2.11
      Install-Module -Name AzureStack -RequiredVersion 1.2.11 
    ```

  Als de installatie geslaagd is, wordt de azurerm-module en AzureStack-modules worden weergegeven in de uitvoer.

- **Zonder internetverbinding** van de hostcomputer ASDK. In een niet-verbonden scenario, moet u eerst de PowerShell-modules downloaden op een computer die verbinding heeft met internet met behulp van de volgende PowerShell-opdrachten:

  ```PowerShell
  $Path = "<Path that is used to save the packages>"

  # AzureRM for 1808 requires 2.3.0, for prior versions use 1.2.11
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
  
  # AzureStack requires 1.5.0 for version 1808, 1.4.0 for versions after 1803, and 1.2.11 for versions before 1803
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
  ```

  Vervolgens kopieert u de gedownloade pakketten op de computer ASDK en meld u aan de locatie als de standaard-opslagplaats en de azurerm-module en AzureStack-modules installeren vanaf deze opslagplaats:

    ```PowerShell  
    $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
    $RepoName = "MyNuGetSource"

    Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

    Install-Module AzureRM -Repository $RepoName

    Install-Module AzureStack -Repository $RepoName
    ```

## <a name="download-the-azure-stack-tools"></a>De hulpprogramma's voor Azure Stack downloaden

[AzureStack-hulpprogramma's](https://github.com/Azure/AzureStack-Tools) is een GitHub-opslagplaats die als host fungeert voor PowerShell-modules voor het beheren en implementeren van resources met Azure Stack. Als u deze hulpprogramma's, de GitHub-opslagplaats klonen of downloaden van de map AzureStack-hulpprogramma's voor het volgende script uit te voeren:

  ```PowerShell
  # Change directory to the root directory.
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  invoke-webrequest `
    https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  expand-archive master.zip -DestinationPath . -Force

  # Change to the tools directory.
  cd AzureStack-Tools-master
  ```

## <a name="validate-the-asdk-installation"></a>De installatie ASDK valideren

Om ervoor te zorgen dat de ASDK-implementatie gelukt is, kunt u de cmdlet Test-AzureStack door de volgende stappen:

1. Meld u als AzureStack\AzureStackAdmin op de hostcomputer ASDK.
2. Open PowerShell als beheerder (niet PowerShell ISE).
3. Uitvoeren: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Uitvoeren: `Test-AzureStack`

De tests duren een paar minuten. Als de installatie voltooid is, de uitvoer ziet er ongeveer uit zoals:

![test-azurestack](media/asdk-post-deploy/test-azurestack.png)

Als er een fout is opgetreden, volgt u de stappen voor hulp.

## <a name="reset-the-password-expiration-policy"></a>Het vervalbeleid voor wachtwoord opnieuw instellen 

Deze stappen om ervoor te zorgen dat het wachtwoord voor de development kit host voordat u een einde van de evaluatie niet verloopt, nadat u de ASDK hebt geïmplementeerd.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Het vervalbeleid voor wachtwoord wijzigen vanuit Powershell

Voer de opdracht uit vanaf een verhoogde Powershell-console:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Het vervalbeleid voor wachtwoorden handmatig wijzigen

1. Open op de host development kit **Group Policy Management** (GPMC. MMC) en navigeer naar **Group Policy Management** – **Forest: azurestack.local** – **domeinen** – **azurestack.local**.
2. Met de rechtermuisknop op **standaarddomeinbeleid** en klikt u op **bewerken**.
3. In de Editor voor Groepsbeleidsbeheer, navigeer naar **Computerconfiguratie** – **beleid** – **Windows-instellingen** – **beveiligingsinstellingen**– **Accountbeleid** – **wachtwoordbeleid**.
4. Dubbelklik in het rechter deelvenster op **maximale wachtwoordduur**.
5. In de **maximale wachtwoordduur eigenschappen** wijzigen van het dialoogvenster de **wachtwoord verloopt over** waarde die moet worden **180**, en klik vervolgens op **OK**.

![Console Groepsbeleidsbeheer](media/asdk-post-deploy/gpmc.png)

## <a name="enable-multi-tenancy"></a>Multitenancy inschakelen

Voor implementaties met Azure AD, moet u [multitenancy inschakelen](../azure-stack-enable-multitenancy.md#enable-multi-tenancy) voor uw installatie ASDK.

> [!NOTE]  
> Als beheerder of gebruiker-accounts uit andere domeinen dan het account dat wordt gebruikt voor het registreren van Azure Stack worden gebruikt voor het aanmelden bij een Azure Stack-portal, de domeinnaam die wordt gebruikt voor het registreren van Azure Stack moet worden toegevoegd aan de portal url. Bijvoorbeeld, als Azure Stack is geregistreerd bij fabrikam.onmicrosoft.com en de account van de gebruiker zich aanmeldt is admin@contoso.com, de URL te gebruiken om aan te melden bij de gebruikersportal: https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

## <a name="next-steps"></a>Volgende stappen

[De ASDK registreren bij Azure](asdk-register.md)
