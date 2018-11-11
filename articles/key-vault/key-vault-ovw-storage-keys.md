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
ms.openlocfilehash: e62c599d82f488bf1fc30ce503c271084c5ae59d
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300216"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Opslagaccountsleutels

> [!NOTE]
> [Azure storage biedt nu ondersteuning voor AAD-autorisatie](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Het is raadzaam om met behulp van Azure Active Directory voor verificatie en autorisatie voor opslag als gebruikers geen zorgen te hoeven maken over het draaien van hun sleutels Storage-Account hebt.

- Azure Key Vault beheert sleutels van een Azure Storage-Account (ASA).
    - Azure Key Vault kan intern (sync) sleutels met een Azure Storage-Account weergeven.    
    - Azure Key Vault wordt opnieuw gegenereerd (gedraaid) periodiek de sleutels.
    - Sleutelwaarden worden nooit geretourneerd in antwoord op de oproepende functie.
    - Azure Key Vault beheert sleutels van Opslagaccounts en klassieke Storage-Accounts.

<a name="prerequisites"></a>Vereisten
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI installeren   
2. [Een Opslagaccount maken](https://azure.microsoft.com/services/storage/)
    - Volg de stappen in dit [document](https://docs.microsoft.com/azure/storage/) om een opslagaccount te maken  
    - **Richtlijnen voor de naamgeving:** namen van Opslagaccounts moet tussen 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Voor stap door stapsgewijze instructies over het gebruik van Key Vault voor het beheren van Opslagaccountsleutels
--------------------------------------------------------------------------------
In de onderstaande instructies volgen, zijn we Key Vault toewijzen als een service operatormachtigingen hebben voor uw storage-account

> [!NOTE]
> . Houd er rekening mee dat zodra u hebt ingesteld met Azure Key Vault beheerde opslag accountsleutels ze moet **geen** meer behalve gewijzigd via Key Vault. Storage-accountsleutels betekent dat Key Vault beheren de opslagaccountsleutel draaien beheerd

1. Na het maken van een storage-account de volgende opdracht om op te halen van de resource-ID van het opslagaccount dat wilt u beheren

    ```
    az storage account show -n storageaccountname (Copy ID field out of the result of this command)
    ```
    
2. Aanvraag-ID van Azure Key Vault de service principal ophalen 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Opslag sleutel Operator-rol toewijzen aan Azure Key Vault-identiteit

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ApplicationIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Maken van een Key Vault beheerde Storage-Account.     <br /><br />
   Hieronder, zijn we een periode van 90 dagen voor opnieuw regenereren instellen. Na 90 dagen wordt Key Vault opnieuw genereren 'key1' en de actieve sleutel uit 'key2' naar 'key1' wisselen.
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-regenerate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    In het geval de gebruiker het storage-account hebt gemaakt en heeft geen machtigingen voor de storage-account, stel de volgende stappen uit de machtigingen voor uw account om ervoor te zorgen dat u alle opslagmachtigingen voor de in de Key Vault kunt beheren.
 > [!NOTE] 
    In het geval de gebruiker beschikt niet over machtigingen voor het opslagaccount, krijgen we eerst de Object-Id van de gebruiker

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```
### <a name="relavant-azure-cli-cmdlets"></a>Relavant Azure CLI-cmdlets
- [Opslag-Cmdlets van Azure CLI](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

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
