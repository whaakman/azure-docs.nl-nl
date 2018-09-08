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
ms.date: 09/07/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: e6e1ffdf4384080649a769b2fdf6877ea744ec51
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162169"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>Verbinding maken met Azure Stack met PowerShell als operator

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt de Azure Stack voor het gebruik van PowerShell voor het beheren van resources, zoals het maken van aanbiedingen, plannen, quota's en waarschuwingen kunt configureren. Dit onderwerp helpt u bij het configureren van de operator-omgeving.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten worden uitgevoerd vanuit de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden zijn via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). 

 - Installeer [Azure Stack-compatibel is met Azure PowerShell-modules](azure-stack-powershell-install.md).  
 - In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>De operator-omgeving configureren en aanmelden bij Azure Stack

De Azure Stack-operator-omgeving configureren met PowerShell. Voer een van de volgende scripts: de Tenantnaam van Azure AD, GraphAudience eindpunt en ArmEndpoint waarden vervangen door de configuratie van uw eigen omgeving.

````PowerShell  
    # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
    # To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Admin Resource Manager endpoint for your environment>"

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
````

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat u alles integratiemethode PowerShell gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om te maken van een resourcegroep met de naam **MyResourceGroup**.

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen

 - [Sjablonen ontwikkelen voor Azure Stack](user/azure-stack-develop-templates.md)
 - [Sjablonen implementeren met PowerShell](user/azure-stack-deploy-template-powershell.md)
