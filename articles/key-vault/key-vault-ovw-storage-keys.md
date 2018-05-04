---
ms.assetid: ''
title: De Opslagaccountsleutels voor Azure Sleutelkluis
description: Een seemless integratie tussen Azure Key Vault en toegang tot de sleutel op basis bieden toegangscodes voor opslag voor Azure Storage-Account.
ms.topic: article
services: key-vault
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: 4f42a47a6d934bf0538efccbcf7f057fd28e2c03
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-key-vault-storage-account-keys"></a>De Opslagaccountsleutels voor Azure Sleutelkluis

Voordat u Azure Key Vault Opslagaccountsleutels moest ontwikkelaars hun eigen sleutels Azure Storage-Account (ASA) beheren en deze handmatig of via een externe automatisering draaien. Nu Sleutelkluis Opslagaccountsleutels worden geïmplementeerd als [Sleutelkluis geheimen](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) voor verificatie met een Azure Storage-Account.

De belangrijke functie van Azure Storage-Account (ASA) beheert geheime rotatie voor u. Ook verwijdert deze de behoefte aan uw rechtstreeks contact met een sleutel ASA door het aanbieden van Shared Access Signatures (SAS) als een methode.

Zie voor meer algemene informatie over Azure Storage-Accounts [over Azure storage-accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Interfaces ondersteunen

U vindt een volledig overzicht en koppelingen naar onze interfaces programmeren en uitvoeren van scripts in de [Sleutelkluis Developer Guide](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>Wat Sleutelkluis beheert

Sleutelkluis voert enkele interne beheerfuncties namens u wanneer u toegangscodes voor opslag worden beheerd.

- Azure Sleutelkluis beheert sleutels van een Azure Storage-Account (ASA).
    - Intern, kunt u Azure Sleutelkluis sleutels (sync) met een Azure Storage-Account weergeven.
    - Genereert Azure Key Vault (draait) periodiek de sleutels.
    - Sleutelwaarden worden nooit geretourneerd in antwoord op de aanroeper.
    - Azure Sleutelkluis beheert sleutels van zowel de Storage-Accounts en de klassieke Opslagaccounts.
- Azure Sleutelkluis kunt u de kluis/objecteigenaar, SAS (account of service-SAS)-definities maken.
    - De SAS-waarde, gemaakt met behulp van SAS-definitie geretourneerd als een geheim via de REST-URI-pad. Zie voor meer informatie [-account voor Azure Sleutelkluis opslagbewerkingen](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

## <a name="naming-guidance"></a>Richtlijnen voor de naamgeving

- Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.
- Een SAS-definitie-naam moet 1 102 tekens met alleen 0-9, a-z, A-Z.

## <a name="developer-experience"></a>Functionaliteit voor ontwikkelaars

### <a name="before-azure-key-vault-storage-keys"></a>Voordat u Azure Sleutelkluis Opslagsleutels

Ontwikkelaars hoeven te maken van de volgende procedures met de sleutel van een opslagaccount wordt gebruikt voor toegang tot Azure-opslag.
1. Sla de verbindingsreeks of SAS-token in Azure App service-instellingen voor toepassing of een andere opslag.
1. Haal bij het starten van de toepassing, de verbindingsreeks of SAS-token.
1. Maak [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) om te communiceren met opslag.

```cs
// The Connection string is being fetched from App Service application settings
var connectionStringOrSasToken = CloudConfigurationManager.GetSetting("StorageConnectionString");
var storageAccount = CloudStorageAccount.Parse(connectionStringOrSasToken);
var blobClient = storageAccount.CreateCloudBlobClient();
 ```

### <a name="after-azure-key-vault-storage-keys"></a>Na Opslagsleutels voor Azure Sleutelkluis

Ontwikkelaars maken een [KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient) en gebruikmaken van die voor de SAS-token voor de opslag. Daarna maken ze [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) met dat token.

```cs
// Create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();

// Use the blobClientWithSas
...

// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Handleiding voor ontwikkelaars

- Kan alleen Sleutelkluis voor het beheren van uw sleutels ASA. Probeer niet zelf beheren, wordt u processen van de Sleutelkluis verstoren.
- ASA sleutels die worden beheerd door meer dan één Sleutelkluis-object is niet toegestaan.
- Als u moet handmatig opnieuw genereren van uw sleutels ASA, raden wij u ze via Sleutelkluis genereren.

## <a name="getting-started"></a>Aan de slag

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Instellingen voor op rollen gebaseerde machtigingen voor toegangsbeheer (RBAC)

De toepassings-id van Azure Sleutelkluis moet machtigingen voor *lijst* en *genereren* sleutels voor een opslagaccount. Stelt u deze machtigingen met behulp van de volgende stappen uit:

```powershell
# Get the resource ID of the Azure Storage Account you want to manage.
# Below, we are fetching a storage account using Azure Resource Manager
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure Key Vault Identity
$servicePrincipal = Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093

# Assign Storage Key Operator role to Azure Key Vault Identity
New-AzureRmRoleAssignment -ObjectId $servicePrincipal.Id -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storage.Id
```

    >[!NOTE]
    > For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role."*

## <a name="working-example"></a>Voorbeeld van de werkende

Het volgende voorbeeld toont een Sleutelkluis maken beheerd Azure Storage-Account en de bijbehorende definities van de Shared Access Signature (SAS).

### <a name="prerequisite"></a>Vereiste

Zorg ervoor dat u hebt voltooid [instellen voor op rollen gebaseerde machtigingen voor toegangsbeheer (RBAC)](#setup-for-role-based-access-control-rbac-permissions).

### <a name="setup"></a>Instellen

```powershell
# This is the name of our Key Vault
$keyVaultName = "mykeyVault"

# Fetching all the storage account object, of the ASA we want to manage with KeyVault
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure KeyVault Identity service principal
$servicePrincipalId = $(Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Vervolgens stelt de machtigingen voor **uw account** om ervoor te zorgen dat u alle opslagmachtigingen van in de Sleutelkluis kunt beheren. In het onderstaande voorbeeld ons Azure-account is _developer@contoso.com_.

```powershell
# Searching our Azure Active Directory for our account's ObjectId
$userPrincipalId = $(Get-AzureRmADUser -SearchString "developer@contoso.com").Id

# We use the ObjectId we found to setting permissions on the vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToStorage all
```

### <a name="create-a-key-vault-managed-storage-account"></a>Een Sleutelkluis maken beheerd Storage-Account

Nu een Opslagaccount beheerd maken in Azure Sleutelkluis en een toegangssleutel van uw storage-account gebruiken voor het maken van de SAS-tokens.
- `-ActiveKeyName` maakt gebruik van 'key2' voor het genereren van de SAS-tokens.
- `-AccountName` wordt gebruikt voor het identificeren van uw beheerde storage-account. Hieronder, kan gebruiken we de opslagaccountnaam te houden eenvoudige maar het een willekeurige naam.
- `-DisableAutoRegenerateKey` Hiermee geeft u de opslagaccountsleutels niet opnieuw te genereren.

```powershell
# Adds your storage account to be managed by Key Vault and will use the access key, key2
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storage.StorageAccountName -AccountResourceId $storage.Id -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="key-regeneration"></a>Toegangssleutel wordt opnieuw gegenereerd

Als u de Sleutelkluis periodiek opnieuw genereren van uw opslagtoegangssleutels wilt, kunt u een periode wordt opnieuw gegenereerd kunt instellen. Hieronder instelt een periode opnieuw genereren van 3 dagen. Na 3 dagen, wordt Sleutelkluis genereren 'key1' en de actieve sleutel uit 'key2' naar 'key1' wisselen.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
$accountName = $storage.StorageAccountName

Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $accountName -AccountResourceId $storage.Id -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Set SAS definities

De account-SAS biedt toegang tot de blob-service met andere machtigingen.
De SAS-definities in de Sleutelkluis voor uw beheerde storage-account instellen.
- `-AccountName` is de naam van de beheerde opslagaccount in de Sleutelkluis.
- `-Name` is de id voor de SAS-token in uw opslag.
- `-ValidityPeriod` Hiermee stelt u de vervaldatum voor de gegenereerde SAS-token.

```powershell
$validityPeriod = [System.Timespan]::FromDays(1)
$readSasName = "readBlobSas"
$writeSasName = "writeBlobSas"

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName $keyVaultName -AccountName $accountName -Name $readSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName $keyVaultName -AccountName $accountName -Name $writeSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List,Write
```

### <a name="get-sas-tokens"></a>SAS-tokens verkrijgen

De bijbehorende SAS-tokens verkrijgen en aanroepen naar de opslag. `-SecretName` is opgesteld met de invoer van de `AccountName` en `Name` parameters op wanneer u uitgevoerd [Set AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition).

```powershell
$readSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$readSasName").SecretValueText
$writeSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$writeSasName").SecretValueText
```

### <a name="create-storage"></a>Opslag maken

U ziet dat u probeert te openen met *$readSasToken* mislukt, maar die we toegang hebben met *$writeSasToken*.

```powershell
$context1 = New-AzureStorageContext -SasToken $readSasToken -StorageAccountName $storage.StorageAccountName
$context2 = New-AzureStorageContext -SasToken $writeSasToken -StorageAccountName $storage.StorageAccountName

Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt" -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt" -Context $context2
```

Bent u toegang tot de opslag van inhoud met de SAS-token die schrijftoegang heeft blob.

### <a name="relevant-powershell-cmdlets"></a>Relevante Powershell-cmdlets

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="storage-account-onboarding"></a>Storage-account voorbereiden

Voorbeeld: als een object Sleutelkluis eigenaar die u een opslagobject van het account aan uw Azure Sleutelkluis vrijgeven een opslagaccount toevoegen.

Tijdens de voorbereiding, Sleutelkluis moet controleren of de identiteit van de voorbereiding-serviceaccount is gemachtigd om *lijst* en *genereren* opslagsleutels. Om te controleren of deze machtigingen, Sleutelkluis krijgt een OBO (op andere gebruikers van)-token van de verificatieservice, doelgroep instellen in Azure Resource Manager en maakt een *lijst* sleutel aanroep naar de Azure Storage-service. Als de *lijst* aanroep is mislukt, de Sleutelkluis maken van het object is mislukt met een HTTP-statuscode van *verboden*. De sleutels op deze manier worden vermeld in de cache opgeslagen met de opslag van uw sleutelkluis entiteit.

Sleutelkluis moet verifiëren dat de identiteit heeft *genereren* machtigingen voordat deze kan eigenaar van uw sleutels opnieuw genereren. Om te controleren dat de identiteit, via OBO-token, evenals de Sleutelkluis identiteit eigen beschikt over deze machtigingen:

- Sleutelkluis worden RBAC-machtigingen op de bron voor storage-account.
- Sleutelkluis valideert het antwoord via reguliere expressie die overeenkomt met van acties en niet-bewerkingen.

Ondersteunende voorbeelden op vinden [Sleutelkluis - Storage-Account sleutels voorbeelden beheerd](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Als de identiteit geen heeft *genereren* machtigingen of als het eerste partij identiteit van de Sleutelkluis geen *lijst* of *genereren* machtiging en vervolgens het voorbereidingsproces aanvragen mislukt retourneren van een juiste foutcode en het bericht.

Het token OBO werkt alleen wanneer u directe, native clienttoepassingen van PowerShell of CLI.

## <a name="other-applications"></a>Andere toepassingen

- SAS-tokens opgesteld met Sleutelkluis sleutels van opslagaccount, bieden nog meer beheerde toegang tot Azure storage-account. Zie voor meer informatie [gedeelde handtekeningen voor toegang](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Zie ook

- [Informatie over sleutels, geheimen en certificaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Sleutelkluis-teamblog](https://blogs.technet.microsoft.com/kv/)
