---
title: Sleutelkluis-sleutel en geheime herstellen voor versleutelde virtuele machines maken met Azure Backup | Microsoft Docs
description: Informatie over het herstellen van de Sleutelkluis-sleutel en geheime in Azure back-up met behulp van PowerShell
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 45214083-d5fc-4eb3-a367-0239dc59e0f6
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f2db3449187d655248b13198b268841052570626
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Sleutelkluis-sleutel en geheime voor versleutelde VM's met Azure back-up herstellen
In dit artikel wordt gesproken over het gebruik van Azure VM-back-up voor herstel uitvoeren van versleutelde Azure virtuele machines, als uw sleutel en geheime bestaan niet in de sleutelkluis. Deze stappen kunnen ook worden gebruikt als u wilt een afzonderlijk exemplaar van de sleutel (coderingssleutel Key) en geheim (sleutel BitLocker-versleuteling) voor de herstelde virtuele machine te onderhouden.

## <a name="prerequisites"></a>Vereisten
* **Back-up van virtuele machines versleuteld** - gecodeerde virtuele machines in Azure back-ups zijn met Azure Backup. Raadpleeg het artikel [back-up en herstel van virtuele Azure-machines met behulp van PowerShell beheren](backup-azure-vms-automation.md) voor meer informatie over het back-up van versleutelde Azure VM's.
* **Azure Sleutelkluis configureren** : Zorg ervoor dat sleutelkluis waaraan sleutels en geheimen moeten worden hersteld, is al aanwezig. Raadpleeg het artikel [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md) voor meer informatie over het beheer van de sleutelkluis.
* **Herstellen schijf** -Zorg ervoor dat u de hersteltaak voor het herstellen van schijven voor het gebruik van versleutelde VM hebt geactiveerd [PowerShell stappen](backup-azure-vms-automation.md#restore-an-azure-vm). Dit is omdat deze taak een JSON-bestand in uw opslagaccount met sleutels en geheimen voor de versleutelde virtuele machine genereert moet worden hersteld.

## <a name="get-key-and-secret-from-azure-backup"></a>Sleutel en geheime ophalen uit Azure Backup

> [!NOTE]
> Als de schijf voor de versleutelde virtuele machine is hersteld, zorgt ervoor dat:
> 1. $details is gevuld met restore schijf taakdetails, zoals vermeld in [PowerShell stappen voor het terugzetten van de sectie schijven](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. VM moet worden gemaakt van herstelde schijven alleen **nadat sleutel en geheime sleutelkluis weer**.
>
>

De schijfeigenschappen van de herstelde voor de taakdetails opvragen.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

De context van de Azure-opslag instellen en JSON-configuratiebestand met de sleutel en geheime details voor versleutelde virtuele machine herstellen.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Sleutel herstellen
Zodra het JSON-bestand wordt gegenereerd in het doelpad bovengenoemde, key blob-bestand van de JSON genereren en voer deze voor het herstellen van de belangrijkste cmdlet om de sleutel (KEK-sleutel) terug in de sleutelkluis.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Geheim herstellen
Gebruik het JSON-bestand die hierboven worden gegenereerd voor het ophalen van de geheime naam en waarde en voer deze in te stellen geheime cmdlet om het geheim (BEK) terug in de sleutelkluis. **Deze cmdlets gebruiken als uw VM is versleuteld met behulp van BEK en KEK-sleutel.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Als uw virtuele machine wordt **versleuteld met BEK alleen**, geheime blob-bestand van de JSON genereren en voer deze voor het herstellen van de geheime cmdlet om het geheim (BEK) terug in de sleutelkluis.

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. Waarde voor $secretname kan worden verkregen door te verwijzen naar de uitvoer van $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl en tekst na geheimen / uitvoer geheime URL is bijvoorbeeld https://keyvaultname.vault.azure.net/secrets/ B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 en de geheime naam is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Waarde van de tag DiskEncryptionKeyFileName is hetzelfde als de geheime naam.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Virtuele machine van de herstelde schijf maken
Als u reservekopie versleutelde VM die gebruikmaakt van Azure VM-back-up hebt gemaakt, bovengenoemde de PowerShell-cmdlets help dat u sleutel en geheime terug naar de sleutelkluis herstellen. Nadat deze is teruggezet, Raadpleeg het artikel [back-up en herstel van virtuele Azure-machines met behulp van PowerShell beheren](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) versleutelde virtuele machines van de herstelde schijf, sleutel en geheime maken.

## <a name="legacy-approach"></a>Verouderde benadering
De hierboven genoemde benadering zou voor de herstelpunten werken. De oudere benadering van sleutel en geheime informatie ophalen van herstelpunt, is ongeldig voor de herstelpunten die ouder zijn dan 11 juli 2017 voor virtuele machines die zijn versleuteld met BEK en KEK-sleutel. Zodra de hersteltaak voor de schijf is voltooid voor het gebruik van versleutelde VM [PowerShell stappen](backup-azure-vms-automation.md#restore-an-azure-vm), zorg ervoor dat $rp is gevuld met een geldige waarde.

### <a name="restore-key"></a>Sleutel herstellen
Gebruik de volgende cmdlets sleutelinformatie (KEK-sleutel) van herstelpunt verkrijgen en voer deze voor het herstellen van sleutel-cmdlet om te plaatsen in de sleutelkluis.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Geheim herstellen
Gebruik de volgende cmdlets uit te halen geheime gegevens van (BEK) herstelpunt en voer deze om in te stellen geheime cmdlet terug in de sleutelkluis te plaatsen.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. Waarde voor $secretname kan worden verkregen door te verwijzen naar de uitvoer van $rp1. KeyAndSecretDetails.SecretUrl en het gebruik van tekst na geheimen / uitvoer geheime URL is bijvoorbeeld https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 en geheime naam is B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Waarde van de tag DiskEncryptionKeyFileName is hetzelfde als de geheime naam.
> 3. Waarde voor DiskEncryptionKeyEncryptionKeyURL kan worden verkregen van de sleutelkluis na het herstellen van de sleutels terug en het gebruik van [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) cmdlet
>
>

## <a name="next-steps"></a>Volgende stappen
Na het herstellen van sleutel en geheime terug naar sleutelkluis, Raadpleeg het artikel [back-up en herstel van virtuele Azure-machines met behulp van PowerShell beheren](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) versleutelde virtuele machines van de herstelde schijf, sleutel en geheime maken.
