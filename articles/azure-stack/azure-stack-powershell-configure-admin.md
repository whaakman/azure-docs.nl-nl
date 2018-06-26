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
ms.date: 06/22/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 74a5a9408a78dd0da12fb3f8ed721774030cc438
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749858"
---
# <a name="configure-the-azure-stack-powershell-environment"></a>De Azure-Stack PowerShell-omgeving configureren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de Azure-Stack als PowerShell wilt gebruiken voor het beheren van resources, zoals het maken van aanbiedingen, plannen, quota en waarschuwingen configureren. Dit onderwerp helpt u bij het configureren van de operator-omgeving.

## <a name="prerequisites"></a>Vereisten

Voer de volgende vereisten een van de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). 

 - Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  
 - Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configureren van de operator-omgeving en aanmelden bij Azure Stack

Hiermee configureert u de Azure-Stack operator-omgeving met PowerShell. Voer een van de volgende scripts: de Azure AD-tenantName, GraphAudience eindpunt en ArmEndpoint waarden vervangen door de configuratie van uw eigen omgeving.

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

Nu dat u alles hebt constateert PowerShell gebruiken voor het maken van resources in Azure-Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht voor het maken van een resourcegroep met de naam **MyResourceGroup**.

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen

 - [Sjablonen voor Azure-Stack ontwikkelen](user/azure-stack-develop-templates.md)
 - [Sjablonen implementeren met PowerShell](user/azure-stack-deploy-template-powershell.md)
