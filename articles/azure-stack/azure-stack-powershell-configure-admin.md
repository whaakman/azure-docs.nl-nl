---
title: De Azure-Stack PowerShell-omgeving configureren | Microsoft Docs
description: Informatie over het configureren van de Azure-Stack PowerShell-omgeving.
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
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 86608ef8b3623682bd10498605f8b7b62c377ff1
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="configure-the-azure-stack-powershell-environment"></a>De Azure-Stack PowerShell-omgeving configureren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de Azure-Stack als PowerShell wilt gebruiken voor het beheren van resources, zoals het maken van aanbiedingen, plannen, quota en waarschuwingen configureren. Dit onderwerp helpt u bij het configureren van de operator-omgeving.

## <a name="prerequisites"></a>Vereisten

Voer de volgende vereisten een van de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

 - Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  
 - Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configureren van de operator-omgeving en aanmelden bij Azure Stack

Hiermee configureert u de Azure-Stack operator-omgeving met PowerShell. Op basis van het type van de implementatie van Azure AD of AD FS, voer een van de volgende scripts: de Azure AD-tenantName, GraphAudience eindpunt en ArmEndpoint waarden vervangen door de configuratie van uw eigen omgeving.

### <a name="azure-active-directory-azure-ad-based-deployments"></a>Azure Active Directory (Azure AD) op basis van implementaties

````PowerShell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````


### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Implementaties op basis van Active Directory Federation Services (AD FS)

````PowerShell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat u alles hebt constateert we PowerShell gebruiken om resources in Azure stapel te maken. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen
 - [Sjablonen voor Azure-Stack ontwikkelen](user/azure-stack-develop-templates.md)
 - [Sjablonen implementeren met PowerShell](user/azure-stack-deploy-template-powershell.md)
