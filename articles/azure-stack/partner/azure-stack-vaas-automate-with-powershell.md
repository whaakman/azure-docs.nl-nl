---
title: Azure Stack-validatie met PowerShell automatiseren | Microsoft Docs
description: U kunt Azure Stack-validatie met PowerShell automatiseren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 39ed9ee9dab7f2ec97d2fb6a0148db333648b227
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481464"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Azure Stack-validatie met PowerShell automatiseren

Validatie uit als een Service (VaaS) biedt de mogelijkheid voor het automatiseren van het uitvoeren van tests met behulp van de **LaunchVaaSTests.ps1** script.

> [!NOTE]  
> Automation is alleen beschikbaar voor de werkstroom testronde. De validatie van het pakket en validatie van de oplossing werkstromen worden alleen ondersteund via de portal VaaS.

Met dit script kan worden gebruikt voor:

> [!div class="checklist"]
> * Vereiste onderdelen installeren
> * Installeren en starten van de lokale agent
> * Een categorie van tests, zoals starten *integratie*, *functionele*, *betrouwbaarheid*
> * De resultaten van rapport

## <a name="launch-the-test-pass-workflow"></a>De werkstroom testronde starten

1. Open een verhoogde PowerShell-prompt.

2. Voer het volgende script voor het downloaden van het automatiseringsscript:

    ```powershell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Voer het volgende script met de juiste parameterwaarden:

    ```powershell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Parameters**

    | Parameter | Description |
    | --- | --- |
    | VaaSUserId | Uw VaaS gebruikers-ID. |
    | VaaSUserPassword | Uw wachtwoord VaaS. |
    | VaaSAccountTenantId | Uw tenant VaaS GUID. |
    | VaaSSolutionName | De naam van de VaaS-oplossing waarmee het doorgeven van de test wordt uitgevoerd. |
    | VaaSTestPassName | De naam van de test VaaS doorgeven werkstroom te maken. |
    | VaaSTestCategories | `Integration`, `Functional`, or. `Reliability`. Als u meerdere waarden gebruikt, scheidt u elke waarde door een komma.  |
    | ServiceAdminUserName | Uw Azure Stack-account voor service-beheerder.  |
    | ServiceAdminPassword | Het wachtwoord van de Azure Stack-service.  |
    | TenantAdminUserName | De beheerder voor de primaire tenant.  |
    | TenantAdminPassword | Het wachtwoord voor de primaire tenant.  |
    | CloudAdminUserName | De gebruikersnaam van de cloud-beheerder.  |
    | CloudAdminPassword | Het wachtwoord voor de beheerder van de cloud.  |

4. Bekijk de resultaten van uw test. Zie voor andere opties [bewaken en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Volgende stappen

Bekijk de meest recente modules voor meer informatie over PowerShell in Azure Stack.

> [!div class="nextstepaction"]
> [Azure Stack-Module](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.6.0)
