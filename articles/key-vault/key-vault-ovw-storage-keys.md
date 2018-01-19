---
ms.assetid: 
title: De Opslagaccountsleutels voor Azure Sleutelkluis
description: Een seemless integratie tussen Azure Key Vault en toegang tot de sleutel op basis bieden toegangscodes voor opslag voor Azure Storage-Account.
ms.topic: article
services: key-vault
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: 6ebac5fc90e259b19e0a4103a732754384232a44
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
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
 
```powershell
//create an Azure Storage Account using a connection string containing an account name and a storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Na Opslagsleutels voor Azure Sleutelkluis 

```powershell
//Make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Get secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  

-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get a SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If your SAS token is about to expire, Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Handleiding voor ontwikkelaars

- Kan alleen Sleutelkluis voor het beheren van uw sleutels ASA. Probeer niet zelf beheren, wordt u processen van de Sleutelkluis verstoren. 
- ASA sleutels die worden beheerd door meer dan één Sleutelkluis-object is niet toegestaan. 
- Als u moet handmatig opnieuw genereren van uw sleutels ASA, raden wij u ze via Sleutelkluis genereren. 

## <a name="getting-started"></a>Aan de slag

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Instellingen voor op rollen gebaseerde machtigingen voor toegangsbeheer (RBAC)

De toepassings-id van Azure Sleutelkluis moet machtigingen voor *lijst* en *genereren* sleutels voor een opslagaccount. Stelt u deze machtigingen met behulp van de volgende stappen uit:

- Object-id van Azure Sleutelkluis identiteit ophalen: 

    `Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093`

- Opslag sleutel Operator rol toewijzen aan Azure Key Vault Identity: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Voor een klassieke accounttype, stelt u de rolparameter te *"Klassiek Storage Account sleutel Service rol Operator."*

## <a name="working-example"></a>Voorbeeld van de werkende

Het volgende voorbeeld toont een Sleutelkluis maken beheerd Azure Storage-Account en de bijbehorende definities van de Shared Access Signature (SAS).

### <a name="assumptions"></a>Veronderstellingen

De volgende instructies zijn givens voor dit voorbeeld werken.

- De opslagbron bevindt zich op: */subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1*

- De naam van de sleutelkluis is: *yourtest1*

### <a name="get-a-service-principal"></a>Ophalen van een service-principal

```powershell
$yourKeyVaultServicePrincipalId = (Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

De uitvoer van de voorgaande opdracht bevat uw ServicePrincipal, bellen we om *yourKeyVaultServicePrincipalId*. 

### <a name="set-permissions"></a>Machtigingen instellen

Zorg ervoor dat u hebt uw opslagmachtigingen zijn ingesteld op *alle*. U kunt youruserPrincipalId ophalen en machtigingen instellen voor de kluis die met de volgende opdrachten.

```powershell
$youruserPrincipalId = (Get-AzureRmADUser -SearchString "your user principal name").Id
```
Nu uw naam te zoeken en ophalen van het gerelateerde object-id die u gebruiken wilt bij het instellen van machtigingen op de kluis.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourtest1' -ObjectId $youruserPrincipalId -PermissionsToStorage all
```

### <a name="allow-access"></a>Toegang toestaan

U moet de Sleutelkluis-Servicetoegang geven tot de storage-accounts voordat u een beheerde storage-account en SAS-definities kunt maken.

```powershell
New-AzureRmRoleAssignment -ObjectId $yourKeyVaultServicePrincipalId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '/subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1'
```

### <a name="create-storage-account"></a>Een opslagaccount maken

Maak nu een Opslagaccount beheerd en twee SAS-definities. De account-SAS biedt toegang tot de blob-service met andere machtigingen.

```powershell
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="regeneration"></a>Opnieuw genereren

Instellen van de periode van het opnieuw genereren met de volgende opdrachten.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Set SAS definities

De SAS-definities in de Sleutelkluis voor uw beheerde storage-account instellen.

```powershell
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourtest1  -AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName yourtest1  -AccountName msak01 -Name blobsas2 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List,Write
```

### <a name="get-token"></a>Token ophalen

De bijbehorende SAS-tokens verkrijgen en aanroepen naar de opslag.

```powershell
$sasToken1 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas1).SecretValueText
$sasToken2 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas2).SecretValueText
```

### <a name="create-storage"></a>Opslag maken

U ziet dat u probeert te openen met *$sastoken1* mislukt, maar die we toegang hebben met *$sastoken2*. 

```powershell
$context1 = New-AzureStorageContext -SasToken $sasToken1 -StorageAccountName yourtest1
$context2 = New-AzureStorageContext -SasToken $sasToken2 -StorageAccountName yourtest1
Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt"  -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt"  -Context $context2
```

### <a name="example-summary"></a>Voorbeeld van de samenvatting

Bent u toegang tot de opslag van inhoud met de SAS-token die schrijftoegang heeft blob.

### <a name="relevant-powershell-cmdlets"></a>Relevante Powershell-cmdlets

- [Get-AzureKeyVaultManagedStorageAccount ](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount?view=azurermps-4.3.1)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)

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
