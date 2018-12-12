---
title: Verbinding maken met Azure Stack met PowerShell als operator | Microsoft Docs
description: Meer informatie over het verbinding maken met Azure Stack met PowerShell als operator
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
ms.date: 12/07/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 1f9d5325522f8ec40af99059651a00f6cdc0e8e0
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53089614"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>Verbinding maken met Azure Stack met PowerShell als operator

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt de Azure Stack voor het gebruik van PowerShell voor het beheren van resources, zoals het maken van aanbiedingen, plannen, quota's en waarschuwingen kunt configureren. Dit onderwerp helpt u bij het configureren van de operator-omgeving.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten worden uitgevoerd vanuit de [development kit](./asdk/asdk-connect.md#connect-with-rdp) of vanuit een Windows-externe client als u [verbonden met de ASDK via VPN](./asdk/asdk-connect.md#connect-with-vpn). 

 - Installeer [Azure Stack-compatibel is met Azure PowerShell-modules](azure-stack-powershell-install.md).  
 - In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .  

## <a name="connect-with-azure-ad"></a>Verbinding maken met Azure AD

De Azure Stack-operator-omgeving configureren met PowerShell. Voer een van de volgende scripts: de Tenantnaam van de Azure Active Directory (Azure AD) en Azure Resource Manager-eindpunt waarden vervangen door de configuratie van uw eigen omgeving. <!-- GraphAudience endpoint -->

```PowerShell  
    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackAdmin").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>Verbinding maken met AD FS

Verbinding maken met de Azure Stack-operator-omgeving met PowerShell gebruiken met Azure Active Directory Federated Services (Azure AD FS). Voor Azure Stack development kit, deze Azure Resource Manager-eindpunt is ingesteld op `https://adminmanagement.local.azurestack.external`. Om het Azure Resource Manager-eindpunt voor Azure Stack-geïntegreerde systemen, neem contact op met uw serviceprovider.

<!-- GraphAudience endpoint -->

  ```PowerShell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
  Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external"

  $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackAdmin").ActiveDirectoryAuthority.TrimEnd('/')
  $tenantId = (invoke-restmethod "$($AuthEndpoint)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

  # Sign in to your environment

  $cred = get-credential

  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $tenantId `
    -Credential $cred
  ```



## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat u alles integratiemethode PowerShell gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om te maken van een resourcegroep met de naam **MyResourceGroup**.

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen

 - [Sjablonen ontwikkelen voor Azure Stack](user/azure-stack-develop-templates.md)
 - [Sjablonen implementeren met PowerShell](user/azure-stack-deploy-template-powershell.md)