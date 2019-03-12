---
title: Verbinding maken met Azure Stack met PowerShell als een gebruiker | Microsoft Docs
description: Stappen voor het verbinding maken met Azure Stack-exemplaar van de gebruiker.
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
ms.date: 03/04/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: 2012810e42c3a35e8e93f85402d92dae1151516b
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731539"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>Verbinding maken met Azure Stack met PowerShell als een gebruiker

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In dit artikel biedt u de stappen voor het verbinding maken met uw Azure Stack-exemplaar. U moet verbinding maken met Azure Stack-resources beheren met PowerShell. Bijvoorbeeld, kunt u PowerShell gebruiken om u te abonneren op aanbiedingen, virtuele machines maken en implementeren van Azure Resource Manager-sjablonen. Als u wilt uitvoeren van PowerShell-cmdlets.

U kunt instellen:
  - Zorg ervoor dat u de vereisten hebt.
  - Verbinding maken met Azure Active Directory (Azure AD) of Active Directory Federatieservices (AD FS). 
  - Registreren van resourceproviders.
  - Test de verbinding.

## <a name="prerequisites"></a>Vereisten

U kunt configureren dat deze vereisten van de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u [verbonden zijn via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Installeer [Azure Stack-compatibel is met Azure PowerShell-modules](azure-stack-powershell-install.md).
* In het [Azure Stack development Kit, worden de blobEndpoint](azure-stack-powershell-download.md) .

Zorg ervoor dat u de volgende scriptvariabelen vervangen door waarden van uw Azure Stack-configuratie:

- **Naam van een Azure AD-tenant**  
  De naam van uw Azure AD-tenant die is gebruikt voor het beheren van Azure Stack, bijvoorbeeld yourdirectory.onmicrosoft.com.
- **Azure Resource Manager-eindpunt**  
  Voor Azure Stack development kit, deze waarde is ingesteld op https://management.local.azurestack.external. Als u deze waarde voor geïntegreerde Azure Stack-systemen, neem contact op met uw serviceprovider.

## <a name="connect-with-azure-ad"></a>Verbinding maken met Azure AD

```PowerShell  
    Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"
    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackUser" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>Verbinding maken met AD FS

  ```PowerShell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackUser"
  ```

## <a name="register-resource-providers"></a>Registreren van resourceproviders

Resourceproviders worden niet automatisch geregistreerd voor de nieuwe gebruikersabonnementen waarvoor geen resources geïmplementeerd via de portal. U kunt expliciet een resourceprovider registreren met het volgende script is uitgevoerd:

```PowerShell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Wanneer u alles is ingesteld, kunt u de connectiviteit testen met behulp van PowerShell om resources te maken in Azure Stack. Maak een resourcegroep voor een toepassing en een virtuele machine toevoegen als een test. Voer de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Volgende stappen

<<<<<<< HEAD
- [Sjablonen ontwikkelen voor Azure Stack](azure-stack-develop-templates.md)
- [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)
 - [PowerShell-Module voor Azure Stack-verwijzing](https://docs.microsoft.com/en-us/powershell/azure/azure-stack/overview)
=======
- [Sjablonen ontwikkelen voor Azure Stack](azure-stack-develop-templates.md)  
- [Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)  
- [Azure Stack-Moduleverwijzing voorkomen](https://docs.microsoft.com/en-us/powershell/azure/azure-stack/overview)  
>>>>>>> ac8c5a2d00d4eb5b4ef65359c5349ddff1bf30cf
- Als u PowerShell instellen voor de operator cloudomgeving wilt, raadpleegt u de [configureren van de Azure Stack-operators PowerShell-omgeving](../azure-stack-powershell-configure-admin.md) artikel.
