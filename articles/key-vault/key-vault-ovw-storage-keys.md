---
ms.assetid: ''
title: Azure Key Vault-Opslagaccountsleutels
description: Opslagaccountsleutels bieden een seemless integratie tussen Azure Key Vault en toegang tot de sleutel op basis van Azure Storage-Account.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: 80601ed30785af37346f801b7e4f1c90e897b3cd
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888062"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Opslagaccountsleutels

[!NOTE] [Azure storage biedt nu ondersteuning voor AAD-autorisatie](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Het is raadzaam om met behulp van Azure Active Directory voor verificatie en autorisatie voor opslag als gebruikers geen zorgen te hoeven maken over het draaien van hun sleutels Storage-Account hebt. 

- Azure Key Vault beheert sleutels van een Azure Storage-Account (ASA).
    - Azure Key Vault kan intern (sync) sleutels met een Azure Storage-Account weergeven.    
    - Azure Key Vault wordt opnieuw gegenereerd (gedraaid) periodiek de sleutels.
    - Sleutelwaarden worden nooit geretourneerd in antwoord op de oproepende functie.
    - Azure Key Vault beheert sleutels van Opslagaccounts en klassieke Storage-Accounts.

<a name="prerequisites"></a>Vereisten
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI installeren   
2. [Een Opslagaccount maken](https://azure.microsoft.com/services/storage/)
    - Volg de stappen in deze [document](https://docs.microsoft.com/azure/storage/) om een opslagaccount te maken  
    - **Richtlijnen voor de naamgeving:** namen van Opslagaccounts moet tussen 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters.        
      
<a name="step-by-step-instructions"></a>Voor stap door stap-instructies
-------------------------

1. Haal de resource-ID van het Azure-Opslagaccount dat u wilt beheren.
    a. Als we een storage-account maken 
    ```
    az storage account show -n storageaccountname (Copy ID out of the result of this command)
    ```
2. Haal de resource-ID van het Azure-Opslagaccount dat u wilt beheren.
    ```
    az storage account show -n storageaccountname (Take ID out of this)
    ```
3. Aanvraag-ID van Azure Key Vault de service principal ophalen 
    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
4. Opslag sleutel Operator-rol toewijzen aan Azure Key Vault-identiteit
    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id hhjkh --scope idofthestorageaccount
    ```
5. Maken van een Key Vault beheerde Storage-Account.     <br /><br />
   Onderstaande opdracht vraagt u Key Vault voor de sleutel opnieuw genereren om de 90 dagen.
   Onderstaande opdracht vraagt u Key Vault voor uw opslagtoegangssleutels regelmatig opnieuw genereren met een punt opnieuw genereren. Hieronder, zijn we een periode van 90 dagen voor opnieuw regenereren instellen. Na 90 dagen wordt Key Vault opnieuw genereren 'key1' en de actieve sleutel uit 'key2' naar 'key1' wisselen.
   ### <a name="key-regeneration"></a>Sleutel opnieuw genereren
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-generate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    In het geval de gebruiker het storage-account hebt gemaakt en heeft geen machtigingen voor de storage-account, stel de volgende stappen uit de machtigingen voor uw account om ervoor te zorgen dat u alle opslagmachtigingen voor de in de Key Vault kunt beheren.
    [!NOTE] In het geval dat de gebruiker heeft geen machtigingen voor de storage-account We eerst de object-id van de gebruiker krijgen

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover purge restore set setsas update
    ```

### <a name="relevant-powershell-cmdlets"></a>Relevante Powershell-cmdlets

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>Zie ook

- [Informatie over sleutels, geheimen en certificaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-teamblog](https://blogs.technet.microsoft.com/kv/)
