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
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 280a811e943c2e81a96875e3c8ba8efdb86fbf2a
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004822"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Back-up inschakelen voor Azure Stack met PowerShell

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Schakel de infrastructuur voor Backup-Service met Windows PowerShell, Neem dus even periodieke back-ups van:
 - Interne service en de hoofdmap identiteitscertificaat
 - Gebruikersplannen, aanbiedingen en abonnementen
 - COMPUTE, opslag en netwerkquota
 - Gebruiker Key vault-geheimen
 - Gebruiker RBAC-rollen en beleid
 - Storage-accounts voor gebruiker

U kunt toegang tot de PowerShell-cmdlets voor back-up inschakelen, back-up starten en het verkrijgen van de back-upinformatie via het beheereindpunt operator.

## <a name="prepare-powershell-environment"></a>PowerShell-omgeving voorbereiden

Zie voor instructies over het configureren van de PowerShell-omgeving [PowerShell installeren voor Azure Stack ](azure-stack-powershell-install.md). Als u wilt aanmelden bij Azure Stack, Zie [configureert u de operator-omgeving en aanmelden bij Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>De back-sleutel voor delen, referenties en versleuteling om in te schakelen van back-up opgeven

In dezelfde PowerShell-sessie, bewerkt u het volgende PowerShell-script door toe te voegen van de variabelen voor uw omgeving. Voer het bijgewerkte script voor de back-sleutel voor delen, referenties en versleuteling aan de infrastructuur voor back-upservice.

| Variabele        | Description   |
|---              |---                                        |
| $username       | Type de **gebruikersnaam** met behulp van het domein en gebruikersnaam voor de gedeelde locatie met voldoende toegangsrechten voor bestanden lezen en schrijven. Bijvoorbeeld `Contoso\backupshareuser`. |
| $password       | Type de **wachtwoord** voor de gebruiker. |
| $sharepath      | Typ het pad naar de **back-up van opslaglocatie**. U moet een Universal Naming Convention (UNC)-tekenreeks voor het pad naar een bestandsshare die wordt gehost op een afzonderlijk apparaat gebruiken. Een UNC-tekenreeks geeft de locatie van bronnen zoals gedeelde bestanden of apparaten. Voor beschikbaarheid van de back-upgegevens, moet het apparaat zich in een andere locatie. |
| $frequencyInHours | De frequentie in uren wordt bepaald hoe vaak back-ups worden gemaakt. De standaardwaarde is 12. Scheduler biedt ondersteuning voor maximaal 12 en een minimum van 4.|
| $retentionPeriodInDays | De bewaarperiode in dagen bepaalt het aantal dagen van de back-ups, blijven behouden in de externe locatie. De standaardwaarde is 7. Scheduler biedt ondersteuning voor maximaal 14 en een minimum van 2. Back-ups ouder is dan de retentieperiode automatisch verwijderd van de externe locatie.|
| $encryptioncertpath | Het pad naar het certificaat van de versleuteling Hiermee geeft u het pad naar de. CER-bestand met de openbare sleutel die wordt gebruikt voor gegevensversleuteling. |
|     |     |

```powershell
    # Example username:
    $username = "domain\backupadmin"
 
    # Example share path:
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    $password = Read-Host -Prompt ("Password for: " + $username) -AsSecureString

    # Create a self-signed certificate using New-SelfSignedCertificate, export the public key portion and save it locally.

    $cert = New-SelfSignedCertificate `
        -DnsName "www.contoso.com" `
        -CertStoreLocation "cert:\LocalMachine\My" 

    New-Item -Path "C:\" -Name "Certs" -ItemType "Directory" 

    #make sure to export the PFX format of the certificate with the public and private keys and then delete the certifcate from the local certificate store of the machine where you created the certificate
    
    Export-Certificate `
        -Cert $cert `
        -FilePath c:\certs\AzSIBCCert.cer 

    # Set the backup settings with the name, password, share, and CER certificate file.
    Set-AzsBackupConfiguration -BackupShare $sharepath -Username $username -Password $password -EncryptionCertPath "c:\temp\cert.cer"
```
   
##  <a name="confirm-backup-settings"></a>Back-instellingen bevestigen

In dezelfde PowerShell-sessie, voer de volgende opdrachten:

   ```powershell
    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName
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

    Set-AzsBackupConfiguration -BackupFrequencyInHours $frequencyInHours -BackupRetentionPeriodInDays $retentionPeriodInDays

    Get-AzsBackupConfiguration | Select-Object -Property Path, UserName, AvailableCapacity, BackupFrequencyInHours, BackupRetentionPeriodInDays
   ```

Het resultaat ziet er als de volgende voorbeelduitvoer:

   ```powershell
    Path                        : \\serverIP\AzsBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
    BackupFrequencyInHours      : 10
    BackupRetentionPeriodInDays : 5
   ```

###<a name="azure-stack-powershell"></a>PowerShell voor Azure Stack 
De PowerShell-cmdlet infrastructuur back-up configureren is Set-AzsBackupConfiguration. In eerdere versies is de cmdlet Set-AzsBackupShare. Deze cmdlet is vereist voor het leveren van een certificaat. Als back-up van de infrastructuur is geconfigureerd met een versleutelingssleutel gemaakt, kan de sleutel voor het bijwerken of weergeven van de eigenschap. U moet versie 1.6 van de Admin PowerShell te gebruiken. 

Als back-up van de infrastructuur is geconfigureerd voordat u bijwerkt naar 1901, kunt u versie 1.6 van de Admin PowerShell instellen en weergeven van de versleutelingssleutel. Versie 1.6 kunt u bij te werken van de versleutelingssleutel naar een certificaatbestand.
Raadpleeg [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md) voor meer informatie over het installeren van de juiste versie van de module. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het uitvoeren van een back-up, Zie [maakt u een Back-up van Azure Stack](azure-stack-backup-back-up-azure-stack.md)

Meer informatie over het controleren of uw back-up is uitgevoerd, Zie [bevestigen back-up is voltooid in de beheerportal](azure-stack-backup-back-up-azure-stack.md)
