---
title: Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving | Microsoft Docs
description: Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: F4ED2238-AAF2-4930-AA7F-7C140311E10F
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/15/2018
ms.author: mabrigg
ms.reviewer: Balsu.G
ms.openlocfilehash: 2655b682d35dd1879c649ed58d524ecd80808896
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="configure-the-azure-stack-users-powershell-environment"></a>Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Volg de instructies in dit artikel voor het configureren van de PowerShell-omgeving voor een Azure-Stack-gebruiker.
Nadat u de omgeving hebt geconfigureerd, kunt u PowerShell gebruiken om Azure-Stack-resources te beheren. U kunt bijvoorbeeld PowerShell gebruiken om u te abonneren op aanbiedingen, virtuele machines maken en implementeren van Azure Resource Manager-sjablonen.

>[!NOTE]
>In dit artikel is afgestemd voor Azure-Stack gebruiker omgevingen. Als u PowerShell instellen voor de cloud operator-omgeving wilt, raadpleegt u de [configureren van de Azure-Stack-operator PowerShell-omgeving](../azure-stack-powershell-configure-admin.md) artikel.

## <a name="prerequisites"></a>Vereisten

U kunt deze vereisten van de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).
* Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).

## <a name="configure-the-user-environment-and-sign-in-to-azure-stack"></a>Configureer de gebruikersomgeving en aanmelden bij Azure Stack

Op basis van het type van uw Azure-Stack-implementatie (Azure AD of AD FS), voer een van de volgende scripts voor het configureren van PowerShell voor Azure-Stack.

Zorg ervoor dat u de volgende scriptvariabelen vervangen door waarden van de configuratie van uw Azure-Stack:

* AAD tenantName
* GraphAudience eindpunt
* ArmEndpoint

### <a name="azure-active-directory-aad-based-deployments"></a>Implementaties op basis van Azure Active Directory (AAD)

  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAudience endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackUser"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantID
   ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Implementaties op basis van Active Directory Federation Services (AD FS)

  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAudience endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackUser"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantID
  ```

## <a name="register-resource-providers"></a>Resourceproviders registreren

Resourceproviders worden niet automatisch worden geregistreerd voor de nieuwe gebruikersabonnementen waaraan geen resources geïmplementeerd via de portal. Het volgende script uit te voeren, kunt u expliciet een resourceprovider registreren:

```powershell
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Wanneer u klaar instellen, kunt u de connectiviteit testen met behulp van PowerShell om resources te maken in Azure-Stack. Als een test een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Voer de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)
* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
