---
title: PowerShell installeren en configureren voor Azure Stack-Quick-Start | Microsoft Docs
description: Meer informatie over het installeren en configureren van PowerShell voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: mabrigg
ms.openlocfilehash: db253c921354ea132dc6b043dcb6f0b96cdbfd88
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368168"
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>Aan de slag met PowerShell in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In deze snelstartgids helpt u bij het installeren en configureren van een Azure Stack-omgeving met PowerShell. Het script waarmee we in dit artikel is afgestemd op de **Azure Stack-operators** alleen.

In dit artikel is een verkorte versie van de stappen die worden beschreven in de [PowerShell installeren]( azure-stack-powershell-install.md), [hulpprogramma's downloaden]( azure-stack-powershell-download.md), en [voordeAzureStack-operatorsPowerShell-omgevingconfigureren]( azure-stack-powershell-configure-admin.md) artikelen. Met behulp van de scripts in dit artikel, kunt u PowerShell instellen voor Azure Stack-omgevingen die zijn geïmplementeerd met Azure Active Directory of Active Directory Federation Services (AD FS).  

## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>PowerShell instellen voor implementaties op basis van Azure Active Directory  

Meld u aan uw Azure Stack Development Kit, of een externe Windows-client als u via VPN-verbinding verbonden bent. Open een PowerShell ISE-sessie met verhoogde bevoegdheden en voer de volgende scripts.

Zorg ervoor dat u het bijwerken van de **TenantName**, **ArmEndpoint**, en **GraphAudience** variabelen die nodig zijn voor de configuratie van uw omgeving:

```PowerShell  
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy.
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following commands:
Get-Module -ListAvailable -Name Azure* | Uninstall-Module
Get-Module Azs.* -ListAvailable | Uninstall-Module -force

# Install PowerShell for Azure Stack.
Install-Module -Name AzureRm.BootStrapper -Force
```

Laden van de API-profiel en de beheerder van de module voor uw versie van Azure Stack.

  - Azure Stack 1808 of hoger.

  ```PowerShell  
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
    Install-Module -Name AzureStack -RequiredVersion 1.5.0
  ```

  - Azure Stack 1807 of een eerdere versie.

  ```PowerShell  
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.4.0
  ```

  - Azure Stack 1804 of een eerdere versie.

  ```PowerShell  
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.2.11
  ```

Download de Azure Stack-tools en maak verbinding.

```PowerShell  
# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip

expand-archive master.zip -DestinationPath . -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId -AADTenantName $TenantName -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>PowerShell instellen voor AD FS-implementaties

U kunt het volgende script gebruiken als u Azure Stack wanneer verbonden met internet. Maar als u Azure Stack zonder verbinding met internet, gebruiken de [manier van PowerShell verbroken](azure-stack-powershell-install.md) en de cmdlets voor het configureren van PowerShell blijft hetzelfde zoals wordt weergegeven in dit script. Meld u aan uw Azure Stack Development Kit, of een externe Windows-client als u via VPN-verbinding verbonden bent. Open een PowerShell ISE-sessie met verhoogde bevoegdheden en voer het volgende script. Zorg ervoor dat u het bijwerken van de **ArmEndpoint** en **GraphAudience** variabelen die nodig zijn voor de configuratie van uw omgeving:

```PowerShell  

# Set the module repository and the execution policy.
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module -Name AzureRm.BootStrapper -Force
```

Laden van de API-profiel en de beheerder van de module voor uw versie van Azure Stack.

  - Azure Stack 1808 of hoger.

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ````

  - Azure Stack 1807 of een eerdere versie.

    > [!Note]  
    Een upgrade van de 1.2.11 versie, Zie de [Migratiehandleiding](https://aka.ms/azspowershellmigration).

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.4.0
    ````

  - Azure Stack 1804 of een eerdere versie.

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.3.0
    ````

Download de Azure Stack-tools en maak verbinding.

```PowerShell  
# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
invoke-webrequest `
https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip

expand-archive master.zip -DestinationPath . -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId -ADFS -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu u PowerShell hebt geconfigureerd, kunt u de configuratie testen door een resourcegroep te maken:

```PowerShell  
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

> [!note]  
> Als u een resourcegroep, moet u een resourcegroep in uw abonnement hebt. Zie voor meer informatie over abonnementen [Plan, aanbieding, quota en abonnementen overzicht](azure-stack-plan-offer-quota-overview.md)

Nadat de resourcegroep is gemaakt, de **Inrichtingsstatus** eigenschap is ingesteld op **geslaagd**.

## <a name="next-steps"></a>Volgende stappen

 - [CLI installeren en configureren](azure-stack-connect-cli.md)
 - [Sjablonen ontwikkelen](user/azure-stack-develop-templates.md)
