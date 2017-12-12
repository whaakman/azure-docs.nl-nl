---
title: Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving | Microsoft Docs
description: Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: F4ED2238-AAF2-4930-AA7F-7C140311E10F
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: mabrigg
ms.openlocfilehash: 0bd5b4a98fee7a5d914e53e49a9517f5d3682a88
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-the-azure-stack-users-powershell-environment"></a>Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving

U kunt uw Azure-Stack Development Kit PowerShell-omgeving configureren als een gebruiker met Azure-Stack. Nadat u hebt geconfigureerd, kunt u PowerShell gebruiken voor het beheren van Azure-Stack resources zoals op aanbiedingen abonneren maken van virtuele machines, het implementeren van Azure Resource Manager-sjablonen, enzovoort. In dit onderwerp als bereik voor het gebruik van de gebruiker omgevingen alleen, als u wilt PowerShell instellen voor de operator cloudomgeving naar verwijzen de [configureren van de Azure-Stack-operator PowerShell-omgeving](../azure-stack-powershell-configure-admin.md) artikel. 

## <a name="prerequisites"></a>Vereisten 

Voer de volgende vereisten een van de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  
* Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md). 

## <a name="configure-the-user-environment-and-sign-in-to-azure-stack"></a>Configureer de gebruikersomgeving en aanmelden bij Azure Stack

Op basis van het type implementatie (Azure AD of AD FS), voer een van de volgende scripts voor het configureren van PowerShell voor Azure-Stack (Zorg ervoor dat u de AAD-tenantName, GraphAudience eindpunt en ArmEndpoint waarden volgens de configuratie van uw omgeving):

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

Als het wordt uitgevoerd op een nieuwe gebruiker-abonnement dat geen resources geïmplementeerd via de portal, worden niet automatisch de resourceproviders geregistreerd. U moet ze expliciet registreren met behulp van het volgende script:

```powershell
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    } 
```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat we hebben een alles constateert we PowerShell gebruiken om resources in Azure stapel te maken. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen
* [Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)
* [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
