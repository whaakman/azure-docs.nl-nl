---
title: Een validatietest uitvoeren in Azure Stack | Microsoft Docs
description: Klik hier voor meer informatie over het verzamelen van logboekbestanden voor diagnostische gegevens in Azure Stack.
services: azure-stack
author: mattbriggs
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 10/15/2018
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 3f4dc6e4136d8d2e3eb1ca5e822306aae2217e3b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340848"
---
# <a name="run-a-validation-test-for-azure-stack"></a>Uitvoeren van een validatietest voor Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*
 
U kunt de status van uw Azure Stack kunt valideren. Wanneer u een probleem hebt, neem dan contact op met klantondersteuning van Microsoft voor Services. Ondersteuning wordt gevraagd om uit te voeren **Test AzureStack** van het beheerknooppunt. De validatietest worden geïsoleerd van de fout. Ondersteuning kan vervolgens gedetailleerde logboeken analyseren, richt u op het gebied waar de fout is opgetreden en werken met u bij het oplossen van het probleem.

## <a name="run-test-azurestack"></a>Run Test-AzureStack

Wanneer u een probleem hebt, neem contact op met klantondersteuning van Microsoft voor Services en voer **Voer Test-AzureStack**.

1. Hebt u een probleem.
2. Neem contact op met Microsoft Customer Services Support.
3. Voer **Test AzureStack** van het eindpunt dat bevoegde.
    1. Het eindpunt van de bevoegde toegang. Zie voor instructies [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md). 
    2. Op de ASDK, moet u zich aanmelden bij de management host als **AzureStack\CloudAdmin**.  
    Op een geïntegreerd systeem moet u het IP-adres gebruiken voor het bevoegdheden--eindpunt voor het beheer die u van de leverancier van de OEM-hardware.
    3. Open PowerShell als beheerder.
    4. Uitvoeren: `Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint`
    5. Uitvoeren: `Test-AzureStack`
4. Als een test mislukt, worden uitgevoerd: `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>` de cmdlet haalt de logboeken van Test-AzureStack. Zie voor meer informatie over diagnostische logboeken [diagnostische hulpprogramma's voor Azure Stack](azure-stack-diagnostics.md).
5. Verzendt de **SeedRing** logboeken aan de klantenservice van Microsoft voor Services. Klantondersteuning van Microsoft voor Services werkt samen met u het probleem op te lossen.

## <a name="reference-for-test-azurestack"></a>Naslaginformatie voor Test-AzureStack

In deze sectie bevat een overzicht van de cmdlet Test-AzureStack en een overzicht van het rapport.

### <a name="test-azurestack"></a>Test-AzureStack

Hiermee valideert u de status van Azure Stack. De cmdlet rapporteert de status van uw Azure Stack-hardware en software. Ondersteuningsmedewerkers kunt dit rapport gebruiken om de tijd om op te lossen kwesties van Azure Stack te verkorten.

> [!Note]  
> **Test-AzureStack** kan detecteren van fouten die niet tot uitval van de cloud leidt zijn, zoals één schijf of een knooppuntfout één fysieke host is mislukt.

#### <a name="syntax"></a>Syntaxis

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parameters

| Parameter               | Waarde           | Vereist | Standaard |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | Reeks    | Nee       | DE WAARDE FALSE   |
| DoNotDeployTenantVm     | SwitchParameter | Nee       | DE WAARDE FALSE   |
| AdminCredential         | PSCredential    | Nee       | N.v.t.      |
| Lijst                    | SwitchParameter | Nee       | DE WAARDE FALSE   |
| Negeren                  | Reeks          | Nee       | N.v.t.      |
| Opnemen                 | Reeks          | Nee       | N.v.t.      |
| BackupSharePath         | Reeks          | Nee       | N.v.t.      |
| BackupShareCredential   | PSCredential    | Nee       | N.v.t.      |


De cmdlet Test-AzureStack de gangbare parameters ondersteund: uitgebreid, foutopsporing, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable en OutVariable. Zie voor meer informatie, [over algemene Parameters](http://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Voorbeelden van Test-AzureStack

De volgende voorbeelden wordt ervan uitgegaan dat u bent aangemeld als **CloudAdmin** en de toegang tot het eindpunt van de bevoegde (PEP). Zie voor instructies [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Test-AzureStack interactief worden uitgevoerd zonder cloud-scenario 's

Voer in een sessie PEP:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Test-AzureStack uitvoeren met cloud-scenario 's

U kunt **Test AzureStack** cloudscenario's uitvoeren voor uw Azure Stack. Deze scenario's omvatten:

 - Het maken van resourcegroepen
 - Het maken van abonnementen
 - Het maken van aanbiedingen
 - Storage-accounts maken
 - Het maken van een virtuele machine
 - Blob-bewerkingen met behulp van de storage-account hebt gemaakt in de Testscenario uitvoeren
 - Wachtrijbewerkingen met behulp van de storage-account hebt gemaakt in de Testscenario uitvoeren
 - Tabelbewerkingen met behulp van de storage-account hebt gemaakt in de Testscenario uitvoeren

De cloud-scenario's waarvoor cloud beheerdersreferenties zijn vereist. 
> [!Note]  
> U kunt de cloud-scenario's met behulp van Active Directory Federated Services (AD FS)-referenties niet uitvoeren. De **Test AzureStack** cmdlet is alleen toegankelijk via de PEP. Maar de PEP biedt geen ondersteuning voor AD FS-referenties.

Typ de gebruikersnaam van de cloud-beheerder in de indeling van de UPN serviceadmin@contoso.onmicrosoft.com (Azure AD). Wanneer u hierom wordt gevraagd, typt u het wachtwoord aan het beheerdersaccount van de cloud.

Voer in een sessie PEP:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Test-AzureStack zonder cloud-scenario's worden uitgevoerd

Voer in een sessie PEP:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Lijst met beschikbare Testscenario's:

Voer in een sessie PEP:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Een opgegeven test uitvoeren

Voer in een sessie PEP:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

Uitsluiten van specifieke tests:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="run-test-azurestack-to-test-infrastructure-backup-settings"></a>Test-AzureStack als u wilt testen van back-instellingen van de infrastructuur worden uitgevoerd

Voordat u back-up van de infrastructuur configureert, kunt u het pad van de back-upshare testen en referenties met behulp van de **AzsBackupShareAccessibility** testen.

Voer in een sessie PEP:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
````
U kunt uitvoeren na het configureren van back-up, AzsBackupShareAccessibility voor het valideren van de share is toegankelijk vanuit de ERCS, vanuit een PEP sessie uitvoeren:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility
````

Als u wilt testen van nieuwe referenties met de geconfigureerde back-upshare, vanuit een sessie PEP uitvoeren:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential <PSCredential for backup share>
````

### <a name="validation-test"></a>Validatietest

De volgende tabel geeft een overzicht van de validatietests uitvoeren door **Test AzureStack**.

| Naam                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Azure Stack-Cloud die als host fungeert voor infrastructuur-overzicht                                                                                  |
| Overzicht van Azure Stack Storage-Services                                                                                              |
| Overzicht rollen exemplaar van Azure Stack-infrastructuur                                                                                  |
| Azure Stack-Cloud die als host fungeert voor het gebruik van de infrastructuur                                                                              |
| Azure Stack-infrastructuurcapaciteit                                                                                               |
| Azure Stack-Portal en API-overzicht                                                                                                |
| Azure Stack-samenvatting van Azure Resource Manager-certificaat                                                                                               |
| Infrastructuur Baseboard management controller, netwerkcontroller, Storage-services en bevoorrechte eindpunt infrastructuur rollen          |
| Infrastructuur Baseboard management controller, netwerkcontroller, Storage-services en bevoorrechte eindpunt instanties van de infrastructuur |
| Overzicht van Azure Stack-infrastructuur rollen                                                                                           |
| Azure Stack-Cloud Service Fabric-Services                                                                                         |
| Prestaties van Azure Stack-infrastructuur rol instanties                                                                              |
| Overzicht van Azure Stack Cloud Host prestaties                                                                                        |
| Samenvatting van de Azure Stack-Service Resource-verbruik                                                                                  |
| Azure Stack Scale Unit kritieke gebeurtenissen (afgelopen 8 uur)                                                                             |
| Overzicht van Azure Stack Storage Services fysieke schijven                                                                               |
|Overzicht van Azure Stack back-Upshare toegankelijkheid                                                                                     |

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over Azure Stack diagnostische hulpprogramma's en probleem logboekregistratie, [ diagnostische hulpprogramma's voor Azure Stack](azure-stack-diagnostics.md).
 - Zie voor meer informatie over het oplossen van problemen, [Microsoft Azure Stack oplossen](azure-stack-troubleshooting.md)
