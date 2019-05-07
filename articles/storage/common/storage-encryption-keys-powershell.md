---
title: Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van PowerShell
description: Leer hoe u PowerShell gebruiken voor het configureren van de klant beheerde sleutels voor Azure Storage-versleuteling. Door de klant beheerde sleutels kunnen u maken, draaien, uitschakelen en besturingselementen voor toegang intrekken.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 0eeae1451e77d9000c87b1aff7ad73323e74f7ee
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154130"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-powershell"></a>Configureren van de klant beheerde sleutels voor Azure Storage-versleuteling van PowerShell

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

In dit artikel laat zien hoe het configureren van een key vault met de klant beheerde sleutels met behulp van PowerShell.

## <a name="assign-an-identity-to-the-storage-account"></a>Een identiteit toewijzen aan de storage-account

Als u wilt inschakelen door de klant beheerde sleutels voor uw storage-account, moet u eerst een beheerde identiteit voor het systeem toegewezen toewijzen aan de storage-account. U gebruikt deze beheerde identiteit de storage-account machtigingen voor toegang tot de sleutelkluis.

Als u wilt toewijzen van een beheerde identiteit met behulp van PowerShell, aanroepen [Set AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

Zie voor meer informatie over het configureren van beheerde identiteiten systeem toegewezen met PowerShell [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Een nieuwe sleutelkluis maken

Aanroepen voor het maken van een nieuwe sleutelkluis met behulp van PowerShell, [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault). De sleutelkluis die u gebruikt voor het opslaan van de klant beheerde sleutels voor Azure Storage-versleuteling moet twee sleutelbeveiliging instellingen ingeschakeld, hebben **voorlopig verwijderen** en **doen niet leegmaken**. 

Vergeet niet om de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden. 

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Het toegangsbeleid voor key vault configureren

Vervolgens configureert u het toegangsbeleid voor key vault zodat het storage-account heeft machtigingen om deze te openen. In deze stap maakt u de beheerde identiteit die u eerder hebt toegewezen aan de storage-account gebruikt.

Aanroepen om het toegangsbeleid voor key vault, [Set AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy). Vergeet niet dat de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden en voor het gebruik van de variabelen die zijn gedefinieerd in de vorige voorbeelden.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Een nieuwe sleutel maken

Maak vervolgens een nieuwe sleutel in de key vault. Aanroepen voor het maken van een nieuwe sleutel [toevoegen AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey). Vergeet niet dat de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden en voor het gebruik van de variabelen die zijn gedefinieerd in de vorige voorbeelden.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Versleuteling configureren met de klant beheerde sleutels

Azure Storage-versleuteling gebruikt standaard Microsoft beheerde sleutels. In deze stap configureert u uw Azure Storage-account voor het gebruik van de klant beheerde sleutels en geeft u de code om te koppelen aan de storage-account.

Bel [Set AzStorageAccount](/powershell/module/az.keyvault/set-azstorageaccount) om bij te werken instellingen voor codering van de storage-account. Vergeet niet dat de tijdelijke aanduiding voor waarden tussen vierkante haken vervangen door uw eigen waarden en voor het gebruik van de variabelen die zijn gedefinieerd in de vorige voorbeelden.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>De belangrijkste versie bijwerken

Wanneer u een nieuwe versie van een sleutel maakt, moet u het opslagaccount voor het gebruik van de nieuwe versie bijwerken. Eerst, aan te roepen [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) naar de meest recente versie van de sleutel. Roep vervolgens [Set AzStorageAccount](/powershell/module/az.keyvault/set-azstorageaccount) om bij te werken de versleutelingsinstellingen van de storage-account voor het gebruik van de nieuwe versie van de sleutel, zoals wordt weergegeven in de vorige sectie.

## <a name="next-steps"></a>Volgende stappen

- [Azure Storage-versleuteling voor data-at-rest](storage-service-encryption.md) 
- [Wat is Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
