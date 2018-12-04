---
title: Azure Key Vault beheerd opslagaccount - PowerShell-versie
description: De functie voor het account van beheerde opslag biedt een seemless-integratie tussen Azure Key Vault en Azure storage-account.
ms.topic: conceptual
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 11/28/2018
ms.openlocfilehash: 7effcc82a737fd2914f06a2c475cece94adc84f3
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52841792"
---
# <a name="azure-key-vault-managed-storage-account---powershell"></a>Azure Key Vault beheerd opslagaccount - PowerShell

> [!NOTE]
> [Azure storage-integratie met Azure Active Directory (Azure AD) is nu in preview](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Het is raadzaam om met behulp van Azure AD voor verificatie en autorisatie, waarmee u OAuth2-tokens gebaseerde toegang tot Azure-opslag, net als Azure Key Vault. Hiermee kunt u naar:
> - Verifiëren van uw clienttoepassing met behulp van een toepassings- of -identiteit, in plaats van de referenties van het opslagaccount. 
> - Gebruik een [Azure AD beheerde identiteit](/azure/active-directory/managed-identities-azure-resources/) bij het uitvoeren op Azure. Beheerde identiteiten verwijderen de noodzaak voor clientverificatie samen en opslaan van referenties in of met uw toepassing.
> - Gebruik rollen gebaseerd toegangsbeheer (RBAC) voor het beheren van autorisatie, die ook wordt ondersteund door Key Vault.

Een [Azure storage-account](/azure/storage/storage-create-storage-account) maakt gebruik van een referentie die uit een accountnaam en een sleutel bestaat. De sleutel is gegenereerd, en fungeert als een 'wachtwoord' in plaats van een cryptografische sleutel. Key Vault deze opslagaccountsleutels kunt beheren door op te slaan als [Key Vault-geheimen](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). 

## <a name="overview"></a>Overzicht

De Key Vault beheerde functie voert verschillende functies voor beheer namens uw storage-account:

- Een lijst met (synchronisatie) sleutels met een Azure storage-account.
- Hiermee wordt opnieuw gegenereerd (gedraaid) periodiek de sleutels.
- Hiermee beheert u de sleutels voor storage-accounts en klassieke opslagaccounts.
- Sleutelwaarden worden nooit geretourneerd in antwoord op de oproepende functie.

Wanneer u de belangrijkste functie van beheerde opslag account gebruikt:

- **Alleen toestaan Key Vault voor het beheren van uw opslagaccountsleutels.** Niet proberen ze om zelf te beheren, zoals u leiden tot met de processen van de Sleutelkluis problemen zult.
- **Opslagaccountsleutels worden beheerd door meer dan een Key Vault-object niet toestaan**.
- **Niet handmatig uw storage-accountsleutels opnieuw genereren**. Het is raadzaam dat u ze via Key Vault genereren.

Het volgende voorbeeld ziet u hoe u Key Vault voor het beheren van uw storage-accountsleutels toestaan.

## <a name="authorize-key-vault-to-access-to-your-storage-account"></a>Key Vault voor de toegang tot uw storage-account toestaan

> [!IMPORTANT]
> Een Azure AD-tenant biedt elke geregistreerde toepassing met een  **[service-principal](/azure/active-directory/develop/developer-glossary#service-principal-object)**, die fungeert als de identiteit van de toepassing. Toepassings-ID van de service-principal wordt gebruikt wanneer u deze machtiging voor toegang tot andere Azure-resources via op rollen gebaseerd toegangsbeheer (RBAC). Omdat de Key Vault is een Microsoft-toepassing, het vooraf geregistreerd in alle Azure AD-tenants onder dezelfde toepassings-ID, binnen elk Azure-cloud:
> - Toepassings-ID in Azure government-cloud Azure AD-tenants gebruiken `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Azure AD-tenants in de openbare cloud van Azure en alle andere toepassings-ID gebruiken `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

Voordat u Key Vault openen en beheren van de sleutels van uw storage-account, moet u de toegang tot uw storage-account machtigen. De Key Vault-toepassing vereist machtigingen voor *lijst* en *opnieuw genereren* sleutels voor uw storage-account. Deze machtigingen worden ingeschakeld via de ingebouwde RBAC-rol [Storage Account Key-Operatorrol Service](/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role). 

Deze rol toewijzen aan de Key Vault-service-principal bereik naar uw opslagaccount te beperken met behulp van de volgende stappen uit. Zorg ervoor dat de `$resourceGroupName`, `$storageAccountName`, `$storageAccountKey`, en `$keyVaultName` variabelen voordat u het script uitvoeren:

```azurepowershell-interactive
# TODO: Update with the resource group where your storage account resides, your storage account name, the name of your active storage account key, and your Key Vault instance name
$resourceGroupName = "rgContoso"
$storageAccountName = "sacontoso"
$storageAccountKey = "key1"
$keyVaultName = "kvContoso"
$keyVaultSpAppId = "cfa8b339-82a2-471a-a3c9-0fc0be7a4093" # See "IMPORTANT" block above for information on Key Vault Application IDs

# Authenticate your PowerShell session with Azure AD, for use with Azure Resource Manager cmdlets
$azureProfile = Connect-AzureRmAccount

# Get a reference to your Azure storage account
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName

# Assign RBAC role "Storage Account Key Operator Service Role" to Key Vault, limiting the access scope to your storage account. For a classic storage account, use "Classic Storage Account Key Operator Service Role." 
New-AzureRmRoleAssignment -ApplicationId $keyVaultSpAppId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storageAccount.Id
```

Na de roltoewijzing is gelukt ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld:

```console
RoleAssignmentId   : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso/providers/Microsoft.Authorization/roleAssignments/189cblll-12fb-406e-8699-4eef8b2b9ecz
Scope              : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
DisplayName        : Azure Key Vault
SignInName         :
RoleDefinitionName : storage account Key Operator Service Role
RoleDefinitionId   : 81a9662b-bebf-436f-a333-f67b29880f12
ObjectId           : 93c27d83-f79b-4cb2-8dd4-4aa716542e74
ObjectType         : ServicePrincipal
CanDelegate        : False
```

Als u Key Vault is al toegevoegd aan de rol in uw storage-account, ontvangt u een *"de roltoewijzing is al bestaat."* Fout bij. U kunt ook de roltoewijzing controleren met behulp van pagina 'Access control (IAM)' van het opslagaccount in Azure portal.  

## <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Uw machtigingen voor het account van gebruiker voor beheerde storage-accounts geven

>[!TIP] 
> Net als Azure AD biedt een **service-principal** voor de identiteit van een toepassing, een **UPN** is opgegeven voor de identiteit van een gebruiker. De principal gebruiker kan vervolgens worden opgegeven machtiging voor toegang tot Key Vault, via de toegangsmachtigingen voor Key Vault.

Met behulp van dezelfde PowerShell-sessie, het toegangsbeleid van Key Vault voor beheerde storage-accounts bijwerken. Deze stap geldt de machtigingen van de storage-account aan uw gebruikersaccount, ervoor te zorgen dat u toegang hebt tot de functies van beheerde opslag account: 

```azurepowershell-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $azureProfile.Context.Account.Id -PermissionsToStorage get,list,delete,set,update,regeneratekey,recover,backup,restore,purge
```

Houd er rekening mee dat machtigingen voor storage-accounts zijn niet beschikbaar op de pagina storage account 'Toegangsbeleid' in de Azure-portal.

## <a name="add-a-managed-storage-account-to-your-key-vault-instance"></a>Een beheerd opslagaccount toevoegen aan uw Key Vault-instantie

Met behulp van dezelfde PowerShell-sessie, een beheerde storage-account maken in uw Key Vault-instantie. De `-DisableAutoRegenerateKey` schakeloptie geeft u niet als u wilt genereren van sleutels voor de opslagaccount.

```azurepowershell-interactive
# Add your storage account to your Key Vault's managed storage accounts
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -DisableAutoRegenerateKey
```

Na een geslaagde toevoeging van de storage-account met geen sleutel opnieuw genereren ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : False
Regeneration Period : 90.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

### <a name="enable-key-regeneration"></a>Inschakelen van de sleutel opnieuw genereren

Als u Key Vault opnieuw genereren van sleutels van uw opslagaccount regelmatig wilt, kunt u een periode voor opnieuw regenereren kunt instellen. In het volgende voorbeeld stellen we een periode opnieuw genereren van drie dagen. Key Vault wordt na drie dagen 'key1' opnieuw genereren en de actieve sleutel uit 'key2' naar 'key1' wisselen.

```azurepowershell-interactive
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -RegenerationPeriod $regenPeriod
```

Na een geslaagde toevoeging van de storage-account met de sleutel opnieuw genereren ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : True
Regeneration Period : 3.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde sleutel voorbeelden van storage-account](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
- [Informatie over sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md)
- [Key Vault PowerShell-referentie](/powershell/module/azurerm.keyvault/)
