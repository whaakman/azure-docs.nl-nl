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
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 1cb4b1a7cfd72ea302676244a53af58e77215aa9
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235251"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Azure Stack-validatie met PowerShell automatiseren 

Validatie uit als een service (VaaS) biedt de mogelijkheid voor het automatiseren van het uitvoeren van tests met behulp van de **LaunchVaaSTests.ps1** script.

U kunt PowerShell gebruiken voor de volgende werkstroom:

- Testronde werkstroom

Met dit script omvat de vier onderdelen van een werkstroom:

- Vereiste onderdelen installeert.
- Installeert en de lokale agent te starten.
- Hiermee start u een categorie van tests, zoals integratie, functionele, betrouwbaarheid.
- Rapporten elke test doorgeven resultaat voor het bewaken en rapporteren genereren van het bestand.

## <a name="launch-the-test-pass-workflow"></a>Starten van de werkstroom test-pass

1. Open een verhoogde PowerShell-prompt.

2. Voer het volgende script voor het downloaden van het automatiseringsscript:

    ````PowerShell  
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://vaastestpacksprodeastus.blob.core.windows.net/packages/Microsoft.VaaS.Scripts.3.0.0.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ````

3. Voer het volgende script met de waarden:

    ````PowerShell  
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>"  -AsPlainText -Force)
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<ServiceAdminUser>", (ConvertTo-SecureString "<ServiceAdminPassword>" -AsPlainText -Force)
    $TenantAdminCreds = New-Object System.Management.Automation.PSCredential "<TenantAdminUser>", (ConvertTo-SecureString "<TenantAdminPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
        -VaaSAccountTenantId <VaaSAccountTenantId> `
        -VaaSSolutionName <VaaSSolutionName> `
        -VaaSTestPassName <VaaSTestPassName> `
        -VaaSTestCategories Integration,Functional `
        -MaxScriptWaitTimeInHours 12 `
        -ServiceAdminCreds $ServiceAdminCreds `
    ````

    **Parameters**

    | Parameter | Beschrijving |
    | --- | --- |
    | VaaSUserld | Uw VaaS gebruikers-ID. | 
    | VaaSUserPassword | Uw wachtwoord VaaS. |
    | ServiceAdminUser | Uw Azure Stack-account voor service-beheerder.  |
    | ServiceAdminPassword | Het wachtwoord van de Azure Stack-service.  |
    | TenantAdminUser | De beheerder voor de primaire tenant.  |
    | TenantAdminPassword | Het wachtwoord voor de primaire tenant.  |
    | FunctionalCategory| Integratie, functionele, of. Betrouwbaarheid. Als u meerdere waarden gebruikt, scheidt u elke waarde door een komma.  |

4. Bekijk de resultaten van uw test. Zie voor meer informatie over het lezen van de testresultaten [bewaken tests](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Volgende stappen

 - Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
 - Zie voor meer informatie over PowerShell in Azure Stack, de [Azure Stack-Module](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) verwijzing.