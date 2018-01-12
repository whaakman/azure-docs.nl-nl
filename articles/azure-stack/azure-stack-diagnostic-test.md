---
title: Een validatietest uitvoeren in Azure-Stack | Microsoft Docs
description: Het verzamelen van logboekbestanden voor diagnostische gegevens in Azure-Stack
services: azure-stack
author: mattbriggs
manager: femila
cloud: azure-stack
ms.assetid: D44641CB-BF3C-46FE-BCF1-D7F7E1D01AFA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: mabrigg
ms.openlocfilehash: 53ef19628b40c4a008143c867c9e7867ac91854d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="run-a-validation-test-for-azure-stack"></a>Een validatietest uitvoeren voor Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*
 
U kunt de status van uw Azure-Stack valideren. Wanneer u een probleem hebt, moet u contact op met klantondersteuning van Microsoft voor Services. Ondersteuning wordt u gevraagd te testen AzureStack uitvoeren vanaf het beheerknooppunt. De validatietest isoleert de fout. Ondersteuning kunt analyseren van gedetailleerde logboekgegevens, ligt de nadruk op het gebied waar de fout is opgetreden en werken met u bij het oplossen van het probleem.

## <a name="run-test-azurestack"></a>Test AzureStack uitvoeren

Wanneer u een probleem hebt, neem contact op met de klantondersteuning van Microsoft voor Services en voer vervolgens **Voer Test-AzureStack**.

1. Er is een probleem.
2. Neem contact op met Microsoft Customer Services Support.
3. Voer **Test AzureStack** vanaf het bevoegde eindpunt.
    1. Toegang tot de bevoegde eindpunt. Zie voor instructies [met behulp van de bevoegde eindpunt in Azure-Stack](azure-stack-privileged-endpoint.md). 
    2. Meld u aan als **AzureStack\CloudAdmin** op de host management.
    3. Open PowerShell als beheerder.
    4. Uitvoeren:`Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint`
    5. Uitvoeren:`Test-AzureStack`
4. Als het rapport mislukken alle tests uitvoeren: `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>` de cmdlet haalt de logboeken van de Test-AzureStack. Zie voor meer informatie over diagnostische logboeken [diagnostische hulpprogramma's van Azure-Stack](azure-stack-diagnostics.md).
5. Verzenden van de **SeedRing** logboeken aan de klantenservice van Microsoft voor Services. Services klantondersteuning van Microsoft werkt samen met u het probleem op te lossen.

## <a name="reference-for-test-azurestack"></a>Naslaginformatie voor Test-AzureStack

Deze sectie bevat een overzicht van de cmdlet Test-AzureStack en een overzicht van het rapport.

### <a name="test-azurestack"></a>Test AzureStack

Valideert de status van de Azure-Stack. De cmdlet meldt de status van uw Azure-Stack hardware en software. Ondersteuningsmedewerkers kunt dit rapport in minder tijd om op te lossen ondersteuningsaanvragen Azure-Stack gebruiken.

> [!Note]  
> Test AzureStack detecteert mogelijk fouten die zijn niet cloud-uitval waardoor zoals één schijf of een knooppuntfout één fysieke host is mislukt.

#### <a name="syntax"></a>Syntaxis

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parameters

| Parameter               | Waarde           | Vereist | Standaard |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | PSCredential    | Nee       | DE WAARDE FALSE   |
| DoNotDeployTenantVm     | SwitchParameter | Nee       | DE WAARDE FALSE   |
| AdminCredential         | PSCredential    | Nee       | N.v.t.      |
| StorageConnectionString | Tekenreeks          | Nee       | N.v.t.      |
| Lijst                    | SwitchParameter | Nee       | DE WAARDE FALSE   |
| Negeren                  | Tekenreeks          | Nee       | N.v.t.      |
| Opnemen                 | Tekenreeks          | Nee       | N.v.t.      |

De cmdlet Test-AzureStack ondersteunt de algemene parameters: uitgebreid, foutopsporing, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable en OutVariable. Zie voor meer informatie [over algemene Parameters](http://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Voorbeelden van Test AzureStack

De volgende voorbeelden wordt ervan uitgegaan dat u bent aangemeld als **CloudAdmin** en de toegang tot de bevoegde eindpunt (PEP). Zie voor instructies [met behulp van de bevoegde eindpunt in Azure-Stack](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Test AzureStack interactief worden uitgevoerd zonder cloud scenario 's

In een sessie PEP uitvoeren:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
      Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Voer Test AzureStack met cloud-scenario 's

Test AzureStack kunt u cloud-scenario's uitvoeren op uw Azure-Stack. Deze scenario's omvatten:

 - Maken van resourcegroepen
 - Plannen maken
 - Aanbiedingen maken
 - Storage-accounts maken
 - Een virtuele machine maken
 - Blob-bewerkingen met behulp van het opslagaccount dat is gemaakt in de Testscenario uitvoeren
 - Wachtrij-bewerkingen met behulp van het opslagaccount dat is gemaakt in de Testscenario uitvoeren
 - Tabelbewerkingen met behulp van het opslagaccount dat is gemaakt in de Testscenario uitvoeren

De cloud-scenario's waarvoor cloud beheerdersreferenties zijn vereist. 
> [!Note]  
> U kunt de cloud-scenario's met behulp van Active Directory Federated Services (AD FS)-referenties niet uitvoeren. De **Test AzureStack** cmdlet is alleen toegankelijk via de PEP. Maar de PEP biedt geen ondersteuning voor AD FS-referenties.

Typ de gebruikersnaam van de cloud-beheerder in UPN-indeling serviceadmin@contoso.onmicrosoft.com (AAD). Typ desgevraagd het wachtwoord voor het beheerdersaccount van de cloud.

In een sessie PEP uitvoeren:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Test AzureStack worden uitgevoerd zonder cloud scenario 's

In een sessie PEP uitvoeren:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Lijst met beschikbare Testscenario's:

In een sessie PEP uitvoeren:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Een opgegeven test uitvoeren

In een sessie PEP uitvoeren:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

Specifieke tests uitsluiten:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="validation-test"></a>Validatietest

De volgende tabel geeft een overzicht van de validatietests uitvoeren door Test AzureStack.

| Naam                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Azure Stack Cloud samenvatting van de infrastructuur die als host fungeert                                                                                  |
| Overzicht van Stack van Azure Storage-Services                                                                                              |
| Overzicht van Azure Stack infrastructuur rollen exemplaar                                                                                  |
| Azure Stack Cloud die als host fungeert voor het gebruik van de infrastructuur                                                                              |
| Capaciteit van de Azure-Stack-infrastructuur                                                                                               |
| De Stack van de Azure Portal en API-overzicht                                                                                                |
| Azure stapelen samenvatting van Azure Resource Manager-certificaat                                                                                               |
| Infrastructuur beheercontroller, netwerkcontroller Storage-services en bevoorrechte eindpunt infrastructuur functies          |
| Infrastructuur beheercontroller, netwerkcontroller Storage-services en bevoorrechte eindpunt infrastructuur Rolinstanties |
| Overzicht van Azure Stack infrastructuur rollen                                                                                           |
| Stack Azure Cloud Service Fabric-Services                                                                                         |
| Prestaties van Azure Stack infrastructuur rol exemplaar                                                                              |
| Overzicht van Azure Stack Cloud Host prestaties                                                                                        |
| Samenvatting van de Stack van Azure Service Resource-verbruik                                                                                  |
| Azure-Stack Scale Unit kritieke gebeurtenissen (laatste 8 uur)                                                                             |
| Overzicht van Azure Stack Storage Services fysieke schijven                                                                               |

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over Azure-Stack diagnostische hulpprogramma's en probleem logboekregistratie, [ diagnostische hulpprogramma's van Azure-Stack](azure-stack-diagnostics.md).
 - Zie voor meer informatie over het oplossen van problemen, [Microsoft Azure-Stack probleemoplossing](azure-stack-troubleshooting.md)