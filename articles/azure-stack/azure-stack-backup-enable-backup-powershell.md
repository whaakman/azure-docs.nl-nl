---
title: Back-up inschakelen voor Azure Stack met PowerShell | Microsoft Docs
description: De infrastructuur voor Backup-Service met Windows PowerShell inschakelen zodat Azure Stack kan worden hersteld als er een storing optreedt.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 08/16/2018
ms.openlocfilehash: 10d7303c4323305e177cf006b9a259a817dc695e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247473"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Back-up inschakelen voor Azure Stack met PowerShell

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

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
| $password       | Type de **wachtwoord** voor de gebruiker. |
| $sharepath      | Typ het pad naar de **back-up van opslaglocatie**. U moet een Universal Naming Convention (UNC)-tekenreeks voor het pad naar een bestandsshare die wordt gehost op een afzonderlijk apparaat gebruiken. Een UNC-tekenreeks geeft de locatie van bronnen zoals gedeelde bestanden of apparaten. Voor beschikbaarheid van de back-upgegevens, moet het apparaat zich in een andere locatie. |
| $frequencyInHours | De frequentie in uren wordt bepaald hoe vaak back-ups worden gemaakt. De standaardwaarde is 12. Scheduler biedt ondersteuning voor maximaal 12 en een minimum van 4.|
| $retentionPeriodInDays | De bewaarperiode in dagen bepaalt het aantal dagen van de back-ups, blijven behouden in de externe locatie. De standaardwaarde is 7. Scheduler biedt ondersteuning voor maximaal 14 en een minimum van 2. Back-ups ouder is dan de retentieperiode automatisch verwijderd van de externe locatie.|
|     |     |

   ```powershell
    # Example username:
    $username = "domain\backupadmin"
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
   
    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    
    # The encryption key is generated using the New-AzsEncryptionKeyBase64 cmdlet provided in Azure Stack PowerShell.
    # Make sure to store your encryption key in a secure location after it is generated.
    $Encryptionkey = New-AzsEncryptionKeyBase64
    $key = ConvertTo-SecureString -String ($Encryptionkey) -AsPlainText -Force

    Set-AzsBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Back-instellingen bevestigen

In dezelfde PowerShell-sessie, voer de volgende opdrachten:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName
   ```

Het resultaat ziet er als de volgende voorbeelduitvoer:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
   ```

## <a name="update-backup-settings"></a>Back-instellingen bijwerken
In dezelfde PowerShell-sessie, kunt u de standaardwaarden voor de bewaarperiode en frequentie voor back-ups bijwerken. 

   ```powershell
    #Set the backup frequency and retention period values.
    $frequencyInHours = 10
    $retentionPeriodInDays = 5

    Set-AzsBackupShare -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

Het resultaat ziet er als de volgende voorbeelduitvoer:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

## <a name="next-steps"></a>Volgende stappen

 - Meer informatie over het uitvoeren van een back-up, Zie [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md ).  
 - Meer informatie over het controleren of uw back-up is uitgevoerd, Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md ).
