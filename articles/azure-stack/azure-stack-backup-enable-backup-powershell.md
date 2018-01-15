---
title: Back-up inschakelen voor Azure Stack met PowerShell | Microsoft Docs
description: De back-up-Service van de infrastructuur met Windows PowerShell inschakelen zodat Azure-Stack kan worden hersteld als er een fout.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 7DFEFEBE-D6B7-4BE0-ADC1-1C01FB7E81A6
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: cbec6242fb4e185c9801a93fc2c4b35721269c2f
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Back-up inschakelen voor Azure Stack met PowerShell

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De back-up-Service van de infrastructuur met Windows PowerShell inschakelen zodat Azure-Stack kan worden hersteld als er een fout. U kunt de PowerShell-cmdlets voor het inschakelen van de back-up, back-up starten en het verkrijgen van de back-upgegevens via het eindpunt van de management operator openen.

## <a name="download-azure-stack-tools"></a>Azure Stack-hulpprogramma's downloaden

Installeren en geconfigureerde PowerShell voor Azure-Stack en de Azure-Stack-hulpprogramma's. Zie [leren werken met PowerShell in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

##  <a name="load-the-connect-and-infrastructure-modules"></a>Laden van de modules Connect en infrastructuur

Open Windows PowerShell met een opdrachtprompt en voer de volgende opdrachten:

   ```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1
    
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
   ```

##  <a name="setup-rm-environment-and-log-into-the-operator-management-endpoint"></a>Setup-Rm-omgeving en de logboekbestanden in de operator eindpunt

Bewerk in dezelfde PowerShell-sessie het volgende PowerShell-script door de variabelen voor uw omgeving toe te voegen. Voer het bijgewerkte script voor het instellen van de RM-omgeving en meld u aan bij de operator eindpunt.

| Variabele    | Beschrijving |
|---          |---          |
| $TenantName | Naam Azure Active Directory-tenant. |
| Operator-accountnaam        | De naam van uw Azure-Stack operator-account. |
| Azure Resource Manager-eindpunt | URL naar de Azure Resource Manager. |

   ```powershell
   # Specify Azure Active Directory tenant name
    $TenantName = "contoso.onmicrosoft.com"
    
    # Set the module repository and the execution policy
    Set-PSRepository `
      -Name "PSGallery" `
      -InstallationPolicy Trusted
    
    Set-ExecutionPolicy RemoteSigned `
      -force
    
    # Configure the Azure Stack operator’s PowerShell environment.
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint "https://adminmanagement.seattle.contoso.com"
    
    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience "https://graph.windows.net/"
    
    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName $TenantName `
      -EnvironmentName AzureStackAdmin
    
    # Sign-in to the operator's console.
    Login-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID 
    
   ```
## <a name="generate-a-new-encryption-key"></a>Een nieuwe versleutelingssleutel genereren

Voer de volgende opdrachten in dezelfde PowerShell-sessie:

   ```powershell
   $encryptionkey = New-EncryptionKeyBase64
   ```

> [!Warning]  
> U moet de AzureStack-hulpprogramma's gebruiken om de sleutel te genereren.

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>De back-sleutel voor de share, referenties en versleuteling zodat back-up opgeven

Bewerk in dezelfde PowerShell-sessie het volgende PowerShell-script door de variabelen voor uw omgeving toe te voegen. Voer het script bijgewerkt als de back-share, referenties en versleuteling sleutel wilt maken voor de Backup-Service-infrastructuur.

| Variabele        | Beschrijving   |
|---              |---                                        |
| $username       | Typ de **gebruikersnaam** met behulp van het domein en gebruikersnaam voor de locatie van de gedeelde schijf. Bijvoorbeeld `Contoso\administrator`. |
| $password       | Typ de **wachtwoord** voor de gebruiker. |
| $sharepath      | Typ het pad naar de **opslaglocatie back-up**. U moet een tekenreeks Universal Naming Convention (UNC) gebruiken voor het pad naar een bestandsshare die wordt gehost op een afzonderlijk apparaat. Een UNC-tekenreeks bevat de locatie van resources, zoals gedeelde bestanden of apparaten. Beschikbaarheid van de back-upgegevens, zodat moet het apparaat op een andere locatie. |

   ```powershell
    $username = "domain\backupoadmin"
    $password = "password"
    $credential = New-Object System.Management.Automation.PSCredential($username, ($password| ConvertTo-SecureString -asPlainText -Force))  
    $location = Get-AzsLocation
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
    
    Set-AzSBackupShare -Location $location.Name -Path $sharepath -UserName $credential.UserName -Password $credential.GetNetworkCredential().password -EncryptionKey $encryptionkey
   ```
   
##  <a name="confirm-backup-settings"></a>Back-instellingen bevestigen

Voer de volgende opdrachten in dezelfde PowerShell-sessie:

   ```powershell
   Get-AzsBackupLocation | Select-Object -Property Path, UserName, Password | ConvertTo-Json 
   ```

Het resultaat moet eruitzien als in de volgende JSON-uitvoer:

   ```json
      {
    "ExternalStoreDefault":  {
        "Path":  "\\\\serverIP\\AzSBackupStore\\contoso.com\\seattle",
        "UserName":  "domain\backupoadmin",
        "Password":  null
       }
   } 
   ```

## <a name="next-steps"></a>Volgende stappen

 - Informatie over het uitvoeren van een back-up, Zie [Back-up van Azure-Stack](azure-stack-backup-back-up-azure-stack.md ).  
- Informatie over het controleren of de back-up is uitgevoerd, Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md ).
