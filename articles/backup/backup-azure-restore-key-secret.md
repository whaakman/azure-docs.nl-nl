---
title: Key Vault-sleutel en -geheim voor versleutelde virtuele machines met behulp van Azure back-up terugzetten
description: Meer informatie over het herstellen van Key Vault-sleutel en geheim in Azure Backup met behulp van PowerShell
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85c284c2a0e49d7fa20c0ec342878ffdf9d47387
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54261159"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Key Vault-sleutel en -geheim voor versleutelde virtuele machines met behulp van Azure back-up terugzetten
In dit artikel vertelt over het gebruik van Azure VM Backup voor herstel van versleutelde virtuele machines van Azure uitvoeren als uw sleutel en -geheim niet bestaan in de key vault. Deze stappen kunnen ook worden gebruikt als u wilt behouden een afzonderlijk exemplaar van de sleutel (Key-versleutelingssleutel) en -geheim (BitLocker-versleutelingssleutel) voor de herstelde virtuele machine.

## <a name="prerequisites"></a>Vereisten
* **Back-up van versleutelde virtuele machines** - versleutelde virtuele machines in Azure back-ups maken met Azure Backup. Raadpleeg het artikel [beheren van back-up en herstel van virtuele Azure-machines met behulp van PowerShell](backup-azure-vms-automation.md) voor meer informatie over hoe u back-up van versleutelde virtuele machines van Azure.
* **Configureren van Azure Key Vault** : Zorg ervoor dat die key vault waarnaar sleutels en geheimen moeten worden hersteld, is al aanwezig. Raadpleeg het artikel [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md) voor meer informatie over key vault-beheer.
* **Herstellen schijf** -Zorg ervoor dat u de hersteltaak voor het herstellen van schijven voor het gebruik van versleutelde VM hebt geactiveerd [PowerShell stappen](backup-azure-vms-automation.md#restore-an-azure-vm). Dit is omdat deze taak wordt een JSON-bestand gegenereerd in uw opslagaccount met sleutels en geheimen voor de versleutelde VM kunnen worden hersteld.

## <a name="get-key-and-secret-from-azure-backup"></a>Sleutel en -geheim ophalen uit Azure Backup

> [!NOTE]
> Zodra de schijf is hersteld voor de versleutelde VM, zorg ervoor dat:
> * $details is gevuld met details van schijf herstellen, zoals vermeld in [PowerShell de stappen in de herstelopdracht de sectie schijven](backup-azure-vms-automation.md#restore-an-azure-vm)
> * Virtuele machine moet worden gemaakt van herstelde schijven alleen **nadat de sleutel en -geheim voor key vault is hersteld**.
>
>

Query uitvoeren op de herstelde schijf-eigenschappen voor de taakdetails.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Stel de context van de Azure-opslag en JSON-configuratiebestand met de sleutel en geheime gegevens voor versleutelde VM herstellen.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Sleutel herstellen
Wanneer het JSON-bestand is gegenereerd in het doelpad zoals hierboven vermeld, sleutel blob-bestand genereren vanuit de JSON en voer deze voor het herstellen van de belangrijkste cmdlet om de sleutel (KEK) terug in de key vault.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Herstellen van geheim

Gebruik de JSON-bestand gegenereerd hierboven voor het ophalen van de geheime naam en waarde en hieraan geheime cmdlet om het geheim (BEK) terug in de key vault instellen. Deze cmdlets gebruiken als uw **virtuele machine is versleuteld met behulp van (bek) en KEK**.

**Deze cmdlets gebruiken als uw Windows-VM is versleuteld met behulp van (bek) en KEK-sleutel.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

**Deze cmdlets gebruiken als uw Linux-VM is versleuteld met behulp van (bek) en KEK-sleutel.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'LinuxPassPhraseFileName';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Gebruik de JSON-bestand gegenereerd hierboven voor het ophalen van de geheime naam en waarde en hieraan geheime cmdlet om het geheim (BEK) terug in de key vault instellen. Deze cmdlets gebruiken als uw **virtuele machine is versleuteld met behulp van de BEK** alleen.

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
  ```

> [!NOTE]
> * Waarde van de $secretname kunnen worden verkregen door te verwijzen naar de uitvoer van $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl en tekst na geheimen / bijvoorbeeld de URL van accountsleutelgeheim uitvoer is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 en geheime naam B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * Waarde van de tag DiskEncryptionKeyFileName is gelijk aan de geheime naam.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Virtuele machine maken van de herstelde schijf
Als u hebt back-ups van versleutelde VM met behulp van Azure VM Backup, genoemde de PowerShell-cmdlets hierboven help dat u herstellen sleutel en geheime terug naar de key vault. Nadat deze is teruggezet, Raadpleeg het artikel [beheren van back-up en herstel van virtuele Azure-machines met behulp van PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) om te maken van versleutelde virtuele machines van de herstelde schijf, sleutel en geheim.

## <a name="legacy-approach"></a>Verouderde benadering
De methode bovengenoemde zou moeten werken voor de herstelpunten. De oudere aanpak van sleutel en geheime informatie ophalen van herstelpunt, is ongeldig voor de herstelpunten die ouder zijn dan op 11 juli 2017 voor virtuele machines die zijn versleuteld met behulp van (bek) en KEK-sleutel. Zodra schijf hersteltaak voltooid voor het gebruik van versleutelde VM is [PowerShell stappen](backup-azure-vms-automation.md#restore-an-azure-vm), zorg ervoor dat $rp is gevuld met een geldige waarde.

### <a name="restore-key"></a>Sleutel herstellen
De volgende cmdlets gebruiken voor de sleutel (KEK)-gegevens ophalen uit herstelpunt en voer deze voor het herstellen van de belangrijkste cmdlet om het terug in de key vault.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Herstellen van geheim
De volgende cmdlets gebruiken voor het geheim (BEK)-gegevens ophalen uit herstelpunt en voer deze om in te stellen geheime cmdlet plaatsen terug in de key vault.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> * Waarde voor $secretname kan worden verkregen door te verwijzen naar de uitvoer van $rp1. KeyAndSecretDetails.SecretUrl en tekst met na geheimen / bijvoorbeeld geheim van de uitvoer-URL is https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 en geheime naam B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> * Waarde van de tag DiskEncryptionKeyFileName is gelijk aan de geheime naam.
> * Waarde voor DiskEncryptionKeyEncryptionKeyURL kan worden verkregen vanuit key vault na het herstellen van de sleutels terug en het gebruik van [Get-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultkey) cmdlet
>
>

## <a name="next-steps"></a>Volgende stappen
Na het herstellen van sleutel en geheime terug tot key vault, Raadpleeg het artikel [beheren van back-up en herstel van virtuele Azure-machines met behulp van PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) om te maken van versleutelde virtuele machines van de herstelde schijf, sleutel en geheim.
