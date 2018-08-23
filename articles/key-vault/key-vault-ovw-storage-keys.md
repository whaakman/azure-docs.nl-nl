---
ms.assetid: ''
title: Azure Key Vault-Opslagaccountsleutels
description: Opslagaccountsleutels bieden een seemless integratie tussen Azure Key Vault en toegang tot de sleutel op basis van Azure Storage-Account.
ms.topic: article
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 08/21/2017
ms.openlocfilehash: 0112d48647c031845bc89ccebfcdd40954c59f14
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42061412"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Opslagaccountsleutels

Voordat u Azure Key Vault Opslagaccountsleutels moest ontwikkelaars beheren hun eigen sleutels voor Azure Storage-Account (ASA) en deze handmatig of via een externe automatisering draaien. Nu Key Vault-Opslagaccountsleutels worden geïmplementeerd als [Key Vault-geheimen](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) voor verificatie met een Azure Storage-Account.

De belangrijke functie van Azure Storage-Account (ASA) beheert geheime rotatie voor u. Deze ook niet meer nodig voor uw rechtstreeks contact met een sleutel ASA door het aanbieden van Shared Access Signatures (SAS) als een methode.

Zie voor meer algemene informatie over Azure Storage-Accounts [over Azure storage-accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Interfaces ondersteunen

U vindt een volledige lijst en koppelingen naar onze interfaces programmeren en uitvoeren van scripts in de [Ontwikkelaarshandleiding voor Key Vault](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>Wat Key Vault wordt beheerd

Key Vault worden enkele interne beheerfuncties uitgevoerd namens, als u beheerde Opslagaccountsleutels gebruiken.

- Azure Key Vault beheert sleutels van een Azure Storage-Account (ASA).
    - Azure Key Vault kan intern (sync) sleutels met een Azure Storage-Account weergeven.
    - Azure Key Vault wordt opnieuw gegenereerd (gedraaid) periodiek de sleutels.
    - Sleutelwaarden worden nooit geretourneerd in antwoord op de oproepende functie.
    - Azure Key Vault beheert sleutels van Opslagaccounts en klassieke Storage-Accounts.
- Azure Key Vault kunt u, de eigenaar van het object-kluis, SAS (Shared Access Signature, account of service-SAS)-definities maken.
    - De waarde van de SAS, gemaakt met behulp van SAS-definitie wordt geretourneerd als een geheim via de REST-URI-pad. Zie voor meer informatie, de bewerkingen van de SAS-definitie in de [Azure Key Vault REST API-verwijzing](/rest/api/keyvault).

## <a name="naming-guidance"></a>Richtlijnen voor de naamgeving

- Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.
- De naam van een SAS moet 1-102 tekens lang dat alleen 0-9, a-z, A-Z.

## <a name="developer-experience"></a>Ervaring voor ontwikkelaars

### <a name="before-azure-key-vault-storage-keys"></a>Voordat u Azure Key Vault-Opslagsleutels

Ontwikkelaars hoeven te doen van de volgende procedures met een sleutel van het opslagaccount gebruikt toegang te krijgen tot Azure storage.
1. Store-verbindingsreeks of SAS-token in Azure App service-toepassingsinstellingen of een andere opslag.
1. Haal bij het opstarten van de toepassing, de verbindingsreeks of SAS-token.
1. Maak [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) om te communiceren met opslag.

```cs
// The Connection string is being fetched from App Service application settings
var connectionStringOrSasToken = CloudConfigurationManager.GetSetting("StorageConnectionString");
var storageAccount = CloudStorageAccount.Parse(connectionStringOrSasToken);
var blobClient = storageAccount.CreateCloudBlobClient();
 ```

### <a name="after-azure-key-vault-storage-keys"></a>Nadat Storage-sleutels voor Azure Key Vault

Ontwikkelaars maken een [KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient) en gebruikmaken van die voor de SAS-token voor de opslag. Daarna maken ze [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) met dit token.

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

 ### <a name="developer-guidance"></a>Richtlijnen voor ontwikkelaars

- Alleen toestaan Key Vault om uw ASA-sleutels te beheren. Probeer niet zelf beheren, zal leiden tot problemen met Key Vault van processen.
- ASA-sleutels worden beheerd door meer dan een Key Vault-object is niet toegestaan.
- Als u de ASA-sleutels handmatig opnieuw te genereren wilt, raden wij u ze via Key Vault genereren.

## <a name="getting-started"></a>Aan de slag

### <a name="give-key-vault-access-to-your-storage-account"></a>Key Vault toegang geven tot uw Storage-Account 

Key Vault is net als veel toepassingen geregistreerd bij Azure AD om te kunnen OAuth gebruiken voor toegang tot andere services. Tijdens de registratie, [een service-principal](/azure/active-directory/develop/app-objects-and-service-principals) object is gemaakt, die wordt gebruikt voor de identiteit van de toepassing tijdens de uitvoering. De service-principal wordt ook gebruikt voor het autoriseren van de identiteit van de toepassing via op rollen gebaseerd toegangsbeheer (RBAC) toegang krijgen tot een andere resource.

De identiteit van de Azure Key Vault-servicetoepassing moet toegangsmachtigingen voor *lijst* en *opnieuw genereren* sleutels voor uw storage-account. Instellen van deze machtigingen met behulp van de volgende stappen uit:

```powershell
# Get the resource ID of the Azure Storage Account you want to manage.
# Below, we are fetching a storage account using Azure Resource Manager
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get Application ID of Azure Key Vault's service principal
$servicePrincipal = Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093

# Assign Storage Key Operator role to Azure Key Vault Identity
New-AzureRmRoleAssignment -ObjectId $servicePrincipal.Id -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storage.Id
```

    >[!NOTE]
    > For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role."*

## <a name="working-example"></a>Werkvoorbeeld van

Het volgende voorbeeld ziet u het maken van een Key Vault beheerde Azure Storage-Account en de bijbehorende SAS-definities.

### <a name="prerequisite"></a>Vereiste

Zorg ervoor dat u hebt [instellen voor machtigingen voor op rollen gebaseerd toegangsbeheer (RBAC)](#setup-for-role-based-access-control-rbac-permissions).

### <a name="setup"></a>Instellen

```powershell
# This is the name of our Key Vault
$keyVaultName = "mykeyVault"

# Fetching all the storage account object, of the ASA we want to manage with KeyVault
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure KeyVault Identity service principal
$servicePrincipalId = $(Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Vervolgens stelt de machtigingen voor **uw account** om ervoor te zorgen dat u alle opslagmachtigingen voor de in de Key Vault kunt beheren. In het onderstaande voorbeeld onze Azure-account is _developer@contoso.com_.

```powershell
# Searching our Azure Active Directory for our account's ObjectId
$userPrincipalId = $(Get-AzureRmADUser -SearchString "developer@contoso.com").Id

# We use the ObjectId we found to setting permissions on the vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToStorage all
```

### <a name="create-a-key-vault-managed-storage-account"></a>Maken van een Key Vault beheerde Storage-Account

Nu maakt u een beheerd Opslagaccount in Azure Key Vault en een toegangssleutel van uw storage-account gebruiken om te maken van de SAS-tokens.
- `-ActiveKeyName` maakt gebruik van 'key2' voor het genereren van de SAS-tokens.
- `-AccountName` wordt gebruikt voor het identificeren van uw beheerde storage-account. Hieronder, gebruiken we de naam van opslagaccount aan Houd het eenvoudig, maar kan elke naam zijn.
- `-DisableAutoRegenerateKey` Hiermee geeft u de opslagaccountsleutels niet opnieuw te genereren.

```powershell
# Adds your storage account to be managed by Key Vault and will use the access key, key2
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storage.StorageAccountName -AccountResourceId $storage.Id -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="key-regeneration"></a>Sleutel opnieuw genereren

Als u Key Vault opnieuw genereren van uw opslagtoegangssleutels periodiek wilt, kunt u een periode voor opnieuw regenereren kunt instellen. Hieronder, zijn we een periode voor opnieuw regenereren van 3 dagen instellen. Key Vault wordt na 3 dagen, 'key1' opnieuw genereren en de actieve sleutel uit 'key2' naar 'key1' wisselen.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
$accountName = $storage.StorageAccountName

Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $accountName -AccountResourceId $storage.Id -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>De SAS-definities instellen

De account-SAS biedt toegang tot de blob-service met andere machtigingen.
De SAS-definities in Key Vault instellen voor uw beheerde storage-account.
- `-AccountName` is de naam van de beheerde opslagaccount in Key Vault.
- `-Name` is de id voor de SAS-token in uw opslag.
- `-ValidityPeriod` Hiermee stelt u de vervaldatum voor de gegenereerde SAS-token.

```powershell
$validityPeriod = [System.Timespan]::FromDays(1)
$readSasName = "readBlobSas"
$writeSasName = "writeBlobSas"

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName $keyVaultName -AccountName $accountName -Name $readSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName $keyVaultName -AccountName $accountName -Name $writeSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List,Write
```

### <a name="get-sas-tokens"></a>SAS-tokens ophalen

De bijbehorende SAS-tokens verkrijgen en aanroepen naar de opslag. `-SecretName` is opgesteld met de invoer van de `AccountName` en `Name` parameters als u uitgevoerd [Set AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition).

```powershell
$readSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$readSasName").SecretValueText
$writeSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$writeSasName").SecretValueText
```

### <a name="create-storage"></a>Opslag maken

U ziet dat u probeert te openen met *$readSasToken* mislukt, maar dat kan met toegang tot *$writeSasToken*.

```powershell
$context1 = New-AzureStorageContext -SasToken $readSasToken -StorageAccountName $storage.StorageAccountName
$context2 = New-AzureStorageContext -SasToken $writeSasToken -StorageAccountName $storage.StorageAccountName

# Ensure the txt file in command exists in local path mentioned
Set-AzureStorageBlobContent -Container containertest1 -File "./abc.txt" -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "./file.txt" -Context $context2
```

Bent u wel toegang krijgen tot de inhoud met de SAS-token met schrijftoegang heeft upopslagblob.

### <a name="relevant-powershell-cmdlets"></a>Relevante Powershell-cmdlets

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="storage-account-onboarding"></a>Onboarding voor Storage-account

Voorbeeld: als de eigenaar van een Key Vault-object is die u een opslagobject van het account aan uw Azure Key Vault voor onboarding een storage-account toevoegen.

Tijdens de voorbereiding, Key Vault nodig heeft om te controleren of de identiteit van het onboarding-account machtigingen heeft voor *lijst* en *opnieuw genereren* opslagsleutels. Als u wilt controleren of deze machtigingen, Key Vault haalt u een OBO (op namens van)-token van de verificatieservice, doelgroep ingesteld op Azure Resource Manager en maakt een *lijst* sleutel aanroep naar de Azure Storage-service. Als de *lijst* aanroep is mislukt, de Key Vault maken van databaseobject is mislukt met een HTTP-statuscode *verboden*. De sleutels die worden vermeld op deze manier met uw sleutelkluis entiteit opslag in cache zijn opgeslagen.

Key Vault moet verifiëren dat de identiteit heeft *opnieuw genereren* machtigingen voordat deze eigenaar worden van uw sleutels opnieuw genereren. Om te controleren dat de identiteit, met behulp van OBO token, evenals de Key Vault eerste partij identiteit beschikt over deze machtigingen:

- Key Vault geeft een lijst van RBAC-machtigingen op de resource van het opslagaccount.
- Key Vault valideert de reactie via reguliere expressie die overeenkomt met van acties en niet-acties.

Sommige ondersteunende voorbeelden op [Key Vault - voorbeelden van Storage Account sleutels wordt beheerd](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=).

Als de identiteit heeft geen *opnieuw genereren* machtigingen of als het eerste partij identiteit van de Sleutelkluis geen *lijst* of *opnieuw genereren* machtiging en vervolgens het voorbereidingsproces aanvraag mislukt een juiste foutcode en een bericht retourneren.

Het token OBO werkt alleen als u eigen, native clienttoepassingen van PowerShell of CLI.

## <a name="other-applications"></a>Andere toepassingen

- SAS-tokens, gebouwd met behulp van Key Vault-opslagaccountsleutels, bieden nog meer gecontroleerde toegang tot Azure storage-account. Zie voor meer informatie, [voor gedeelde toegangshandtekeningen](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Zie ook

- [Informatie over sleutels, geheimen en certificaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-teamblog](https://blogs.technet.microsoft.com/kv/)
