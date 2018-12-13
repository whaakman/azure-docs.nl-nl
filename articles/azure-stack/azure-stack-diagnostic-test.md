---
title: Gebruik het hulpprogramma voor het valideren van Azure Stack | Microsoft Docs
description: Klik hier voor meer informatie over het verzamelen van logboekbestanden voor diagnostische gegevens in Azure Stack.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 12/03/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: f754242d0cf7ee30572b21a3f4daf6fd2c0f63ff
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275904"
---
# <a name="validate-azure-stack-system-state"></a>Valideren van de systeemstatus van de Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als Azure Stack-operators is het essentieel de mogelijkheid om te weten de gezondheid en status van uw systeem op aanvraag. Het hulpprogramma voor het valideren van Azure Stack (**Test AzureStack**) is een PowerShell-cmdlet u kunt een reeks tests uitvoeren op uw systeem voor het identificeren van fouten, indien aanwezig. U wordt meestal gevraagd dit hulpprogramma uitvoeren via de [bevoegde eindpunt (PEP)](azure-stack-privileged-endpoint.md) wanneer u Neem contact op met Microsoft klantenondersteuning Services (CSS) met een probleem. Met de status van de gehele systeem en de statusinformatie bij de hand, kunt CSS verzamelen en analyseren van gedetailleerde logboeken, richt u op het gebied waar de fout is opgetreden en samen met u het probleem op te lossen.

## <a name="running-the-validation-tool-and-accessing-results"></a>Uitvoeren van het hulpprogramma voor het valideren en toegang tot resultaten

Zoals eerder gezegd, wordt het hulpprogramma voor het valideren via de PEP uitgevoerd. Elke test retourneert een **geslaagd/mislukt** status in het PowerShell-venster. Bovendien wordt een gedetailleerde HTML-rapport gemaakt die later kunnen worden geopend tijdens [verzamelen](azure-stack-diagnostics.md). Hier volgt een overzicht van de end-to-end-proces voor validatietests: 

1. Toegang tot het eindpunt van de bevoegde (PEP). Voer de volgende opdrachten een PEP-sessie tot stand brengen:

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > Voor toegang tot de PEP op een hostcomputer ASDK, AzS-ERCS01 voor - ComputerName te gebruiken.

2. Wanneer u zich in de PEP, uitvoeren: 

   ```powershell
   Test-AzureStack
   ```

   Raadpleeg de [Parameter overwegingen met betrekking tot](azure-stack-diagnostic-test.md#parameter-considerations) en [gebruik case voorbeelden](azure-stack-diagnostic-test.md#use-case-examples) secties voor meer informatie.

3. Als u een rapport test **MISLUKKEN**, uitvoeren:

   ```powershell
   Get-AzureStackLog -FilterByRole SeedRing -OutputSharePath "<path>" -OutputShareCredential $cred
   ```

   De cmdlet verzamelt logboeken die worden gegenereerd door Test-AzureStack. Zie voor meer informatie over diagnostische logboeken [diagnostische hulpprogramma's voor Azure Stack](azure-stack-diagnostics.md). U moet niet verzamelen van Logboeken of neem contact op met CSS als test rapport **waarschuwen**.

4. Als u krijgt de instructie om uit te voeren van het hulpprogramma voor het valideren van de CSS, vraagt de CSS-vertegenwoordiger om de logboeken die u hebt verzameld om door te gaan met het oplossen van uw probleem.

## <a name="tests-available"></a>Tests beschikbaar

Het hulpprogramma voor het valideren kunt u een reeks op systeemniveau tests uitvoeren en basic cloud-scenario's waarmee u inzicht aan de huidige status en Ga na problemen in uw systeem.

### <a name="cloud-infrastructure-tests"></a>Cloud-infrastructuur tests

Deze tests weinig impact op het niveau van een infrastructuur werken en u voorzien van informatie over de verschillende onderdelen van het systeem en functies. Op dit moment worden tests gegroepeerd in de volgende categorieën:

| Test-categorie                                        | Argument-opnemen en - negeren |
| :--------------------------------------------------- | :-------------------------------- |
| Samenvatting van de Azure Stack-meldingen                            | AzsAlertSummary                   |
| Overzicht van Azure Stack back-Upshare toegankelijkheid       | AzsBackupShareAccessibility       |
| Overzicht van Azure Stack besturingselement vlak                    | AzsControlPlane                   |
| Defender-overzicht van Azure Stack                         | AzsDefenderSummary                |
| Azure Stack samenvatting van de Firmware infrastructuur die als host fungeert  | AzsHostingInfraFWSummary          |
| Azure Stack-Cloud die als host fungeert voor infrastructuur-overzicht     | AzsHostingInfraSummary            |
| Azure Stack-Cloud die als host fungeert voor het gebruik van de infrastructuur | AzsHostingInfraUtilization        |
| Azure Stack-infrastructuurcapaciteit                  | AzsInfraCapacity                  |
| Prestaties van de Azure Stack-infrastructuur               | AzsInfraPerformance               |
| Overzicht van Azure Stack-infrastructuur-rollen              | AzsInfraRoleSummary               |
| Samenvatting van Azure Stack                           | AzsInfraUpdateSummary             |
| Azure Stack-Portal en API-overzicht                   | AzsPortalAPISummary               |
| Gebeurtenissen in Azure Stack Scale Unit virtuele machine                     | AzsScaleUnitEvents                |
| Azure Stack Scale Unit VM-Resources                  | AzsScaleUnitResources             |
| Azure Stack SDN Validatieoverzicht                   | AzsSDNValidation                  |
| Overzicht van Azure Stack-Service Fabric-rollen              | AzsSFRoleSummary                  |
| Overzicht van Azure Stack BMC                              | AzsStampBMCSummary                |
| Overzicht van Azure Stack Storage-Services                 | AzsStorageSvcsSummary             |
| Overzicht van Azure Stack SQL Store                        | AzsStoreSummary                   |
| Azure Stack VM-plaatsing samenvatting                     | AzsVmPlacement                    |

### <a name="cloud-scenario-tests"></a>Tests voor cloud-scenario

Naast de bovenstaande tests infrastructuur hebt u ook de mogelijkheid om uit te voeren cloud scenario tests om te controleren of functionaliteit tussen onderdelen van de infrastructuur. Cloud-beheerdersreferenties zijn vereist om uit te voeren van deze tests zoals ze betrekking hebben op resource-implementatie. 
    > [!NOTE]
    >
    > Currently you cannot run cloud scenario tests using Active Directory Federated Services (AD FS) credentials. 

De volgende cloud-scenario's zijn getest door het hulpprogramma voor het valideren:
- Resource is gemaakt   
- Plan maken              
- Aanbieding maken            
- Opslagaccount is gemaakt   
- Het maken van virtuele machines 
- BLOB storage-bewerking   
- De opslagbewerking wachtrij  
- De opslagbewerking tabel  

## <a name="parameter-considerations"></a>Overwegingen voor parameter

- De parameter **lijst** kan worden gebruikt om alle beschikbare test categorieën weer te geven.

- De parameters **opnemen** en **negeren** wilt opnemen of uitsluiten van test-categorieën kunnen worden gebruikt. Zie de [Tests beschikbaar](azure-stack-diagnostic-test.md#tests-available) sectie voor meer informatie over verkorte versie moet worden gebruikt met deze argumenten.

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- Een tenant die virtuele machine wordt geïmplementeerd als onderdeel van een test voor de cloudscenario. U kunt **DoNotDeployTenantVm** dit uitschakelen. 

- Moet u opgeven de **ServiceAdminCredential** parameter cloud scenario tests uitvoeren, zoals beschreven in de [gebruik case voorbeelden](azure-stack-diagnostic-test.md#use-case-examples) sectie.

- **BackupSharePath** en **BackupShareCredential** worden gebruikt bij het testen van back-instellingen van de infrastructuur, zoals wordt weergegeven in de [gebruik case voorbeelden](azure-stack-diagnostic-test.md#use-case-examples) sectie.

- Het hulpprogramma voor het valideren biedt ook ondersteuning voor algemene PowerShell-parameters: Uitgebreid, foutopsporing, ErrorAction, ErrorVariable, WarningAction, -WarningVariable, OutBuffer, -PipelineVariable, en OutVariable. Zie voor meer informatie, [over algemene Parameters](http://go.microsoft.com/fwlink/?LinkID=113216).  

## <a name="use-case-examples"></a>De aanvraag voorbeelden gebruiken

### <a name="run-validation-without-cloud-scenarios"></a>Validatie zonder cloudscenario's uitvoeren

Voer de validatie zonder de **ServiceAdminCredential** parameter om over te slaan van de actieve cloud scenario tests: 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>Validatie uitvoeren met cloud-scenario 's

Leveren van het hulpprogramma voor het valideren met de **ServiceAdminCredentials** parameter wordt standaard de cloud scenario tests uitgevoerd: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

Als u alleen cloud-scenario's worden uitgevoerd zonder dat de rest van de tests worden uitgevoerd wilt, kunt u de **opnemen** parameter om dit te doen: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

De gebruikersnaam van de cloud-beheerder moet worden ingevoerd in de UPN-indeling: serviceadmin@contoso.onmicrosoft.com (Azure AD). Wanneer u hierom wordt gevraagd, typt u het wachtwoord aan het beheerdersaccount van de cloud.

### <a name="run-validation-tool-to-test-system-readiness-before-installing-update-or-hotfix"></a>Hulpprogramma voor het valideren als u wilt testen system gereedheid voor het installeren van update of hotfix uitvoeren

Voordat u begint de installatie van een update of hotfix, moet u het hulpprogramma voor het valideren om te controleren of de status van uw Azure-Stack uitvoeren:

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>Hulpprogramma voor het valideren als u wilt testen van back-instellingen van de infrastructuur worden uitgevoerd

*Voordat u* infrastructuur back-up configureren, kunt u het pad van de back-upshare testen en referenties met behulp van de **AzsBackupShareAccessibility** testen: 

  ```powershell
  New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
  ```

*Na* back-up configureren, kunt u uitvoeren **AzsBackupShareAccessibility** voor het valideren van de share is toegankelijk vanuit de ERCS:

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

Als u wilt testen van nieuwe referenties met de geconfigureerde back-upshare, voert u de volgende uit: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Stack diagnostische hulpprogramma's en probleem logboekregistratie, [diagnostische hulpprogramma's voor Azure Stack](azure-stack-diagnostics.md).

Zie voor meer informatie over het oplossen van problemen, [het oplossen van Microsoft Azure Stack](azure-stack-troubleshooting.md).