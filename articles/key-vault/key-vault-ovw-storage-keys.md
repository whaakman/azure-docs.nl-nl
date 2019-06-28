---
title: Opslagaccountsleutels met Azure Key Vault en de Azure CLI beheren
description: Opslagaccountsleutels bieden naadloze integratie tussen Azure Key Vault en de sleutel gebaseerde toegang tot Azure storage-account.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 6ac054bc9750e4297080c4ab64030c9c6a5fb55a
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312838"
---
# <a name="manage-storage-account-keys-with-azure-key-vault-and-the-azure-cli"></a>Opslagaccountsleutels met Azure Key Vault en de Azure CLI beheren 

Azure Key Vault beheren sleutels voor Azure storage-accounts en klassieke opslagaccounts De functie Key Vault beheerde opslag account kunt u verschillende functies voor sleutelbeheer voltooien voor u.

Een [Azure storage-account](/azure/storage/storage-create-storage-account) maakt gebruik van een referentie die uit een accountnaam en een sleutel bestaat. De sleutel wordt automatisch gegenereerd en fungeert als een wachtwoord, in plaats van een als een cryptografische sleutel. Key Vault opslagaccountsleutels beheert door op te slaan als [Key Vault-geheimen](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). Sleutels met een Azure storage-account (gesynchroniseerde) worden weergegeven en worden periodiek opnieuw gegenereerd of _gedraaide_. 

Wanneer u de belangrijkste functie van beheerde opslag account gebruikt, moet u de volgende punten overwegen:

- Sleutelwaarden worden nooit geretourneerd in antwoord op een aanroeper.
- Alleen Key Vault, moet de sleutels van uw storage-account beheren. Geen de sleutels zelf beheren en te voorkomen dat met Key Vault-processen.
- Slechts één Key Vault-object moet opslagaccountsleutels beheren. Beheer van meerdere objecten zijn niet toegestaan.
- U kunt Key Vault voor het beheren van uw storage-account met een gebruiker-principal, maar niet met een service-principal op te vragen.
- Sleutels opnieuw genereren met behulp van Key Vault alleen. Niet handmatig opnieuw genereren van sleutels van uw opslagaccount. 

> [!NOTE]
> Integratie van Azure Storage met Azure Active Directory (Azure AD) is van Microsoft cloud-gebaseerde identiteits- en toegangsbeheer management-service.
> Azure AD-integratie is beschikbaar voor [Azure-blobs en wachtrijen](https://docs.microsoft.com/azure/storage/common/storage-auth-aad).
> Azure AD gebruiken voor verificatie en autorisatie.
> Azure AD biedt OAuth2 op basis van tokens toegang tot Azure Storage, net als Azure Key Vault.
>
> Azure AD kunt u voor het verifiëren van uw clienttoepassing met behulp van een toepassings- of -identiteit, in plaats van de referenties van het opslagaccount.
> U kunt een [Azure AD beheerde identiteit](/azure/active-directory/managed-identities-azure-resources/) bij het uitvoeren op Azure. Beheerde identiteiten noodzaak de voor clientverificatie en opslaan van referenties in en met uw toepassing.
> Azure AD maakt gebruik van op rollen gebaseerd toegangsbeheer (RBAC) voor het beheren van autorisatie, die ook wordt ondersteund door Key Vault.

### <a name="service-principal-application-id"></a>Service-principal toepassings-ID

Een Azure AD-tenant biedt elke geregistreerde toepassing met een [service-principal](/azure/active-directory/develop/developer-glossary#service-principal-object). De service-principal fungeert als de toepassings-id (ID). De toepassings-ID wordt gebruikt tijdens de installatie van de autorisatie voor toegang tot andere Azure-resources via RBAC.

Key Vault is een Microsoft-toepassing die vooraf geregistreerd in alle Azure AD-tenants. De Key Vault is geregistreerd onder dezelfde toepassings-ID en binnen elk Azure-cloud.

| Tenants | Cloud | Toepassings-id |
| --- | --- | --- |
| Azure AD | Azure Government | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Azure openbaar | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Overige  | Alle | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="prerequisites"></a>Vereisten

Voordat u Key Vault gebruiken voor het beheren van de sleutel van uw opslagaccount, controleert u de vereisten:

- Installeer de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).
- Maak een [Azure storage-account](https://azure.microsoft.com/services/storage/). Ga als volgt [stappen](https://docs.microsoft.com/azure/storage/).
- Naam van het opslagaccount moet alleen kleine letters en cijfers gebruiken. De lengte van de naam moet tussen 3 en 24 tekens lang zijn.        
      
## <a name="manage-storage-account-keys"></a>Opslagaccountsleutels beheren

Er zijn vier eenvoudige stappen Key Vault gebruikt voor het beheren van opslagsleutels voor account:

1. Een bestaand opslagaccount ophalen
1. Haal een bestaande sleutelkluis.
1. Een Key Vault beheerde storage-account toevoegen aan de kluis. Stel `key1` als de actieve sleutel met een punt opnieuw genereren van 180 dagen.
1. Gebruik `key1` om in te stellen van een storage-context voor het opgegeven opslagaccount.

> [!NOTE]
> Key Vault als een service operatormachtigingen in uw storage-account toegewezen.
> 
> Na het instellen van Azure Key Vault beheerde opslagaccountsleutels, moet u alleen de sleutels wijzigen met behulp van de Key Vault.
> Voor beheerde opslagaccountsleutels beheert Key Vault de rotatie van de opslagaccountsleutel.

1. Nadat u een opslagaccount maakt, moet u de volgende opdracht om op te halen van de resource-ID van het opslagaccount voor het beheren van uitvoeren:
    ```
    az storage account show -n storageaccountname
    ```

    Kopieer de waarde van de resource-ID van de opdrachtuitvoer:
    ```
    /subscriptions/<subscription ID>/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```

    Voorbeelduitvoer:
    ```
    "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    ```
    
1. De 'Storage Account Key Operator Service rol' RBAC-rol toewijzen aan Key Vault. Deze rol beperkt het toegangsbereik naar uw opslagaccount. Gebruik de functie 'Klassieke Storage Account Key Operator Service rol' voor een klassiek opslagaccount.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role" --assignee-object-id 93c27d83-f79b-4cb2-8dd4-4aa716542e74 --scope "/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<StorageAccountName>"
    ```
    
    `93c27d83-f79b-4cb2-8dd4-4aa716542e74` is de Object-ID voor Key Vault in de openbare cloud van Azure. Als u de Object-ID voor Key Vault in de Azure Government-cloud, Zie [Service-principal toepassings-ID](#service-principal-application-id).
    
1. Maak een opslagaccount Key Vault beheerd:

    Stel een periode voor opnieuw regenereren van 90 dagen. Na 90 dagen, Key Vault wordt opnieuw gegenereerd `key1` en de actieve sleutel uit swaps `key2` naar `key1`. `key1` vervolgens is gemarkeerd als de actieve sleutel. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="create-and-generate-tokens"></a>Maken en genereren van tokens

Key Vault voor het genereren van SAS-tokens kunt u ook vragen. Een shared access signature biedt gedelegeerde toegang tot resources in uw opslagaccount. U kunt clients toegang verlenen tot bronnen in uw opslagaccount zonder het delen van sleutels van uw account. Een shared access signature biedt u een veilige manier voor het delen van uw opslagresources zonder verlies van sleutels van uw account.

De opdrachten in deze sectie voert u de volgende acties:

- Instellen van een definitie van een gedeeld account toegang handtekening `<YourSASDefinitionName>`. De definitie is ingesteld op een storage-account voor Key Vault beheerde `<YourStorageAccountName>` in uw key vault `<VaultName>`.
- Maak een account SAS-token voor de services Blob, bestand, tabel en wachtrij. Het token wordt gemaakt voor het woord brontypen Service Container en Object. Het token wordt gemaakt met alle machtigingen, via https, en met de opgegeven begin- en einddatums.
- De definitie van een Key Vault beheerde opslag gedeelde toegang handtekening in de kluis instellen. De definitie is van de sjabloon-URI van de SAS-token dat is gemaakt. De definitie is van het type van de handtekening gedeelde toegang `account` en N dagen geldig is.
- Het werkelijke toegangstoken ophalen uit de Key Vault-geheim dat overeenkomt met de definitie van de shared access signature.

Nadat u de stappen in de vorige sectie hebt voltooid, voer de volgende opdrachten om te vragen van Key Vault voor het genereren van SAS-tokens. 

1. De definitie van een shared access signature maken. Nadat de definitie van de shared access signature is gemaakt, vraagt u Key Vault voor het genereren van meer gedeelde SAS-tokens. Voor deze bewerking moet de `storage` en `setsas` machtigingen.
    ```
    $sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
    ```

    Zie voor meer informatie over de werking, het [az storage account genereren-sas](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas) verwijzen naar documentatie.

    Nadat de bewerking is uitgevoerd, kopieert u de uitvoer.
    ```console
       "se=2020-01-01&sp=***"
    ```

1. Gebruik de `$sasToken` die worden gegenereerd door de vorige opdracht en de definitie van een shared access signature maken. Zie voor meer informatie over de opdrachtparameters, de [az keyvault sas-definitie maken](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters) verwijzen naar documentatie.
    ```
    az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
    ```

    Wanneer de gebruiker geen machtigingen voor de storage-account, moet u eerst de Object-ID van de gebruiker ophalen:
    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="fetch-tokens-in-code"></a>Tokens in de code ophalen

Uitvoeren van bewerkingen op uw storage-account met het ophalen van [gedeelde SAS-tokens](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) uit Key Vault.

Er zijn drie manieren om te verifiëren naar Key Vault:

- Een beheerde service-identiteit gebruiken. Deze benadering wordt ten zeerste aanbevolen.
- Een service-principal en certificaat gebruiken. 
- Gebruik een service-principal en het wachtwoord. Deze methode wordt niet aanbevolen.

Zie voor meer informatie, [Azure Key Vault: Basisconcepten](key-vault-whatis.md#basic-concepts).

Het volgende voorbeeld ziet u hoe u tokens handtekening voor gedeelde toegang ophalen. U kunt de tokens ophalen nadat u de definitie van een shared access signature hebt gemaakt. 

```cs
// After you get a security token, create KeyVaultClient with vault credentials.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a shared access signature token for your storage from Key Vault.
// The format for SecretUri is https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials by using the shared access signature token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Als uw SAS-token bijna verlopen is, wordt de SAS-token opnieuw ophalen uit Key Vault en bijwerken van de code.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

### <a name="azure-cli-commands"></a>Azure CLI-opdrachten

Zie voor informatie over de Azure CLI-opdrachten die relevant voor beheerde storage-accounts zijn, het [az keyvault opslag](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest) verwijzen naar documentatie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [sleutels, geheimen en certificaten](https://docs.microsoft.com/rest/api/keyvault/).
- Raadpleeg de artikelen op de [teamblog van Azure Key Vault](https://blogs.technet.microsoft.com/kv/).
