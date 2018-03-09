---
title: Configureren van de Azure-Stack-operator PowerShell-omgeving | Microsoft Docs
description: Informatie over het configureren van de Azure-Stack-operator PowerShell-omgeving.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 37D9CAC9-538B-4504-B51B-7336158D8A6B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: mabrigg
ms.openlocfilehash: 57aa5a1ccc45548c3e789b50c888f669df39d5f1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>De Azure-Stack-operator PowerShell-omgeving configureren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de Azure-Stack als PowerShell wilt gebruiken voor het beheren van resources, zoals het maken van aanbiedingen, plannen, quota en waarschuwingen configureren. Dit onderwerp helpt u bij het configureren van de operator-omgeving. Als u wilt PowerShell configureren voor de gebruikersomgeving, Zie [configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](user/azure-stack-powershell-configure-user.md) artikel.

## <a name="prerequisites"></a>Vereisten

Voer de volgende vereisten een van de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

* Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  
* Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configureren van de operator-omgeving en aanmelden bij Azure Stack

Hiermee configureert u de Azure-Stack operator-omgeving met PowerShell. Op basis van het type van de implementatie van Azure AD of AD FS, voer een van de volgende scripts: de Azure AD-tenantName, GraphAudience eindpunt en ArmEndpoint waarden vervangen door de configuratie van uw eigen omgeving.

### <a name="azure-active-directory-azure-ad-based-deployments"></a>Azure Active Directory (Azure AD) op basis van implementaties

````powershell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# Get the value of your Directory Tenant ID
$TenantID = Get-AzsDirectoryTenantId -AADTenantName "<mydirectorytenant>.onmicrosoft.com" -EnvironmentName AzureStackAdmin

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Login-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````


### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Implementaties op basis van Active Directory Federation Services (AD FS)

````powershell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# Get the value of your Directory Tenant ID
$TenantID = Get-AzsDirectoryTenantId -ADFS -EnvironmentName AzureStackAdmin

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Login-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat u alles hebt constateert we PowerShell gebruiken om resources in Azure stapel te maken. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen
* [Sjablonen voor Azure-Stack ontwikkelen](user/azure-stack-develop-templates.md)
* [Sjablonen implementeren met PowerShell](user/azure-stack-deploy-template-powershell.md)
