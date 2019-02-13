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
ms.date: 02/11/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: d5b2b1a73a15874072caacd7549c1c328e7edb26
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116555"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>Verbinding maken met Azure Stack met PowerShell als operator

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt de Azure Stack voor het gebruik van PowerShell voor het beheren van resources, zoals het maken van aanbiedingen, plannen, quota's en waarschuwingen kunt configureren. Dit onderwerp helpt u bij het configureren van de operator-omgeving.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten worden uitgevoerd vanuit de [development kit](./asdk/asdk-connect.md#connect-with-rdp) of vanuit een Windows-externe client als u bent [verbonden met de ASDK via VPN](./asdk/asdk-connect.md#connect-with-vpn). 

 - Installeer [Azure Stack-compatibel is met Azure PowerShell-modules](azure-stack-powershell-install.md).  
 - In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .  

## <a name="connect-with-azure-ad"></a>Verbinding maken met Azure AD

De Azure Stack-operator-omgeving configureren met PowerShell. Voer een van de volgende scripts: Vervang de Tenantnaam van de Azure Active Directory (Azure AD) en waarden van Azure Resource Manager-eindpunt met de configuratie van uw eigen omgeving. <!-- GraphAudience endpoint -->

```PowerShell  
    # Register an Azure Resource Manager environment that targets your Azure Stack instance. Get your Azure Resource Manager endpoint value from your service provider.
Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external"

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

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackAdmin"
  ```

> [!Note]  
> AD FS biedt alleen ondersteuning voor interactieve verificatie met gebruikers-id's. Als u een referentieobject is vereist, moet u een service-principal (SPN) gebruiken. Zie voor meer informatie over het instellen van een service-principal met Azure Stack en AD FS als uw identity management-service [beheren service-principal voor AD FS](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat u alles integratiemethode PowerShell gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om te maken van een resourcegroep met de naam **MyResourceGroup**.

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen

 - [Sjablonen ontwikkelen voor Azure Stack](user/azure-stack-develop-templates.md)
 - [Sjablonen implementeren met PowerShell](user/azure-stack-deploy-template-powershell.md)
