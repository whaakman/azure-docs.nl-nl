---
title: Back-up inschakelen voor Azure Stack met PowerShell | Microsoft Docs
description: De infrastructuur voor Backup-Service met Windows PowerShell inschakelen zodat Azure Stack kan worden hersteld als er een storing optreedt.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/10/2018
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 76a24e7096cbc2a9bcea8bf68e2b333345dbff68
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242951"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Back-up inschakelen voor Azure Stack met PowerShell

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Schakel de infrastructuur voor Backup-Service met Windows PowerShell, Neem dus even periodieke back-ups van:
 - Interne service en de hoofdmap identiteitscertificaat
 - Gebruikersplannen, aanbiedingen en abonnementen
 - Keyvault-geheimen
 - Gebruiker RBAC-rollen en beleid

U kunt toegang tot de PowerShell-cmdlets voor back-up inschakelen, back-up starten en het verkrijgen van de back-upinformatie via het beheereindpunt operator.

## <a name="prepare-powershell-environment"></a>PowerShell-omgeving voorbereiden

Zie voor instructies over het configureren van de PowerShell-omgeving [PowerShell installeren voor Azure Stack ](azure-stack-powershell-install.md). Als u wilt aanmelden bij Azure Stack, Zie [configureert u de operator-omgeving en aanmelden bij Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>De back-sleutel voor delen, referenties en versleuteling om in te schakelen van back-up opgeven

In dezelfde PowerShell-sessie, bewerkt u het volgende PowerShell-script door toe te voegen van de variabelen voor uw omgeving. Voer het bijgewerkte script voor de back-sleutel voor delen, referenties en versleuteling aan de infrastructuur voor back-upservice.

| Variabele        | Beschrijving   |
|---              |---                                        |
| $username       | Type de **gebruikersnaam** met behulp van het domein en gebruikersnaam voor de gedeelde locatie met voldoende toegangsrechten voor bestanden lezen en schrijven. Bijvoorbeeld `Contoso\backupshareuser`. |
| $key            | Type de **versleutelingssleutel** gebruikt voor het versleutelen van elke back-up. |
| $password       | Type de **wachtwoord** voor de gebruiker. |
| $sharepath      | Typ het pad naar de **back-up van opslaglocatie**. U moet een Universal Naming Convention (UNC)-tekenreeks voor het pad naar een bestandsshare die wordt gehost op een afzonderlijk apparaat gebruiken. Een UNC-tekenreeks geeft de locatie van bronnen zoals gedeelde bestanden of apparaten. Voor beschikbaarheid van de back-upgegevens, moet het apparaat zich in een andere locatie. |

   ```powershell
    $username = "domain\backupadmin"
   
    $Secure = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    $Encrypted = ConvertFrom-SecureString -SecureString $Secure
    $password = ConvertTo-SecureString -String $Encrypted
    
    $BackupEncryptionKeyBase64 = ""
    $tempEncryptionKeyString = ""
    foreach($i in 1..64) { $tempEncryptionKeyString += -join ((65..90) + (97..122) | Get-Random | % {[char]$_}) }
    $tempEncryptionKeyBytes = [System.Text.Encoding]::UTF8.GetBytes($tempEncryptionKeyString)
    $BackupEncryptionKeyBase64 = [System.Convert]::ToBase64String($tempEncryptionKeyBytes)
    $BackupEncryptionKeyBase64
    
    $Securekey = ConvertTo-SecureString -String $BackupEncryptionKeyBase64 -AsPlainText -Force
    $Encryptedkey = ConvertFrom-SecureString -SecureString $Securekey
    $key = ConvertTo-SecureString -String $Encryptedkey
    
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    Set-AzSBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Back-instellingen bevestigen

In dezelfde PowerShell-sessie, voer de volgende opdrachten:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity
   ```

Het resultaat ziet er als de volgende uitvoer:

   ```powershell
    Path                        : \\serverIP\AzSBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
   ```

## <a name="next-steps"></a>Volgende stappen

 - Meer informatie over het uitvoeren van een back-up, Zie [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md ).  
 - Meer informatie over het controleren of uw back-up is uitgevoerd, Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md ).