---
title: Service-naar-serviceverificatie naar Azure Key Vault met behulp van .NET
description: De bibliotheek Microsoft.Azure.Services.AppAuthentication gebruiken om te verifiëren bij Azure Key Vault met behulp van .NET.
keywords: verificatie van Azure sleutelkluis lokale referenties
author: msmbaldwin
manager: barbkess
services: key-vault
ms.author: mbaldwin
ms.date: 07/06/2019
ms.topic: conceptual
ms.service: key-vault
ms.openlocfilehash: 6a748031f9d35e26eeb544f154477ea3449903f5
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67796102"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Service-naar-serviceverificatie naar Azure Key Vault met behulp van .NET

U moet een referentie voor de Azure Active Directory (AD), een gedeeld geheim of een certificaat voor authenticatie naar Azure Key Vault. 

Beheren van deze referenties kan lastig en het is verleidelijk om referenties in een app door ze in de bron-of configuratiebestanden te bundelen.  De `Microsoft.Azure.Services.AppAuthentication` voor .NET-bibliotheek vereenvoudigt dit probleem. De referenties van de ontwikkelaar wordt gebruikt om te verifiëren tijdens de lokale ontwikkeling. Wanneer de oplossing wordt later geïmplementeerd op Azure, wordt de bibliotheek automatisch overgeschakeld naar de referenties van de toepassing.    Met de ontwikkelaarsreferenties van tijdens het ontwikkelen van lokale is veiliger omdat u niet hoeft te maken van Azure AD-referenties of referenties tussen ontwikkelaars delen.

De `Microsoft.Azure.Services.AppAuthentication` bibliotheek beheert verificatie automatisch, op zijn beurt waarmee u zich richten op uw oplossing in plaats van uw referenties.  Deze biedt ondersteuning voor lokale ontwikkeling met Microsoft Visual Studio, Azure CLI of Azure AD-geïntegreerde verificatie. Wanneer geïmplementeerd voor een Azure-resource die ondersteuning biedt voor een beheerde identiteit, gebruikt de bibliotheek automatisch [beheerde identiteiten voor een Azure-resources](../active-directory/msi-overview.md). Er zijn geen wijzigingen code of configuratie nodig. De bibliotheek ondersteunt ook direct gebruik van Azure AD [clientreferenties](../azure-resource-manager/resource-group-authenticate-service-principal.md) wanneer een beheerde identiteit is niet beschikbaar, of wanneer de beveiligingscontext van de ontwikkelaar kan niet worden bepaald tijdens de lokale ontwikkeling.

## <a name="using-the-library"></a>Met behulp van de bibliotheek

Voor .NET-toepassingen, de eenvoudigste manier om te werken met een beheerde identiteit is via de `Microsoft.Azure.Services.AppAuthentication` pakket. Hier wordt aan de slag:

1. Voeg verwijzingen toe aan de [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet-pakketten aan uw toepassing. 

2. Voeg de volgende code toe:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // Instantiate a new KeyVaultClient object, with an access token to Key Vault
    var azureServiceTokenProvider1 = new AzureServiceTokenProvider();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider1.KeyVaultTokenCallback));

    // Optional: Request an access token to other Azure services
    var azureServiceTokenProvider2 = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider2.GetAccessTokenAsync("https://management.azure.com/").ConfigureAwait(false);
    ```

De `AzureServiceTokenProvider` klasse in de cache van het token in het geheugen opgeslagen en opgehaald uit Azure AD net voordat het verloopt. Als gevolg hiervan, u hoeft niet langer om te controleren is verlopen voordat de `GetAccessTokenAsync` methode. Roept de methode als u wilt dat het token wordt gebruikt. 

De `GetAccessTokenAsync` methode vereist een resource-id. Zie voor meer informatie, [welke Azure-services ondersteunen beheerde identiteiten voor Azure-resources](../active-directory/msi-overview.md).

## <a name="local-development-authentication"></a>Lokale ontwikkeling-verificatie

Voor lokale ontwikkeling, er zijn twee primaire authenticatie-scenario's: [zich verifiëren bij Azure-services](#authenticating-to-azure-services), en [verificatie bij aangepaste services](#authenticating-to-custom-services).

### <a name="authenticating-to-azure-services"></a>Verificatie bij Azure-Services

Lokale machines bieden geen ondersteuning voor beheerde identiteiten voor Azure-resources.  Als gevolg hiervan de `Microsoft.Azure.Services.AppAuthentication` bibliotheek maakt gebruik van uw ontwikkelaarsreferenties om uit te voeren in uw lokale ontwikkelomgeving. Wanneer de oplossing wordt geïmplementeerd naar Azure, de bibliotheek maakt gebruik van een beheerde identiteit overschakelen naar een stroom voor de referentie verlenen van OAuth 2.0-client.  Dit betekent dat u kunt dezelfde code testen lokaal en extern zonder zorgen.

Voor lokale ontwikkeling `AzureServiceTokenProvider` worden opgehaald met behulp van **Visual Studio**, **Azure-opdrachtregelinterface** (CLI), of **Azure AD-geïntegreerde verificatie**. Elke optie geprobeerd sequentieel worden verwerkt en de bibliotheek maakt gebruik van de eerste optie die is geslaagd. Als er geen optie werkt, een `AzureServiceTokenProviderException` uitzondering is opgetreden met gedetailleerde informatie.

### <a name="authenticating-with-visual-studio"></a>Verificatie met Visual Studio

Verificatie met Visual Studio heeft de volgende vereisten:

1. [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) of hoger.

2. De [App verificatie-extensie voor Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354), beschikbaar als een afzonderlijke-extensie voor Visual Studio 2017 Update 5 en bundel met het product in Update 6 en hoger. Met Update 6 of hoger, kunt u de installatie van de App-verificatie-extensie controleren door te selecteren van de hulpmiddelen van Azure-ontwikkeling in het installatieprogramma voor Visual Studio.
 
Aanmelden bij Visual Studio en gebruik **extra**&nbsp;>&nbsp;**opties**&nbsp;>&nbsp;**Azure-Service Verificatie** om te selecteren van een account voor lokale ontwikkeling. 

Als u problemen bij het gebruik van Visual Studio, zoals fouten met betrekking tot het bestand tokenprovider zorgvuldig door deze stappen. 

Het is mogelijk ook die nodig zijn voor het verifiëren van uw developer-token. Om dit te doen, gaat u naar **extra**&nbsp;>&nbsp;**opties**>**Azure&nbsp;Service&nbsp;verificatie**  en zoek naar een **zich opnieuw verifiëren** koppeling onder het geselecteerde account.  Selecteer deze om te verifiëren. 

### <a name="authenticating-with-azure-cli"></a>Verificatie met Azure CLI

Azure CLI gebruiken voor lokale ontwikkeling:

1. Installeer [Azure CLI v2.0.12](/cli/azure/install-azure-cli) of hoger. Upgrade van eerdere versies. 

2. Gebruik **az login** zich aanmeldt bij Azure.

Gebruik `az account get-access-token` om te controleren of de toegang.  Als u een foutbericht ontvangt, controleert u stap 1 is voltooid. 

Als Azure-CLI is niet geïnstalleerd in de standaardmap, ontvangt u mogelijk een die fouten rapporteren die `AzureServiceTokenProvider` het pad niet vinden voor de Azure CLI.  Gebruik de **AzureCLIPath** omgevingsvariabele voor het definiëren van de Azure CLI-installatiemap. `AzureServiceTokenProvider` voegt de map die is opgegeven de **AzureCLIPath** omgevingsvariabele de **pad** omgevingsvariabele indien nodig.

Als u bent aangemeld bij Azure CLI met behulp van meerdere accounts of uw account toegang tot meerdere abonnementen heeft, moet u om op te geven van het specifieke abonnement moet worden gebruikt.  Gebruik het volgende om dit te doen:

```
az account set --subscription <subscription-id>
```

Met deze opdracht genereert uitvoer alleen op fouten.  Als u wilt controleren of de instellingen van het huidige, gebruikt u:

```
az account list
```

### <a name="authenticating-with-azure-ad-authentication"></a>Verificatie met Azure AD-verificatie

Voor het gebruik van Azure AD-verificatie, moet u controleren of:

- Uw on-premises active directory [wordt gesynchroniseerd met Azure AD](../active-directory/connect/active-directory-aadconnect.md).

- Uw code wordt uitgevoerd op een computer voor het domein.


### <a name="authenticating-to-custom-services"></a>Verificatie bij aangepaste services

Als de serviceaanroepen van een van Azure-services, wordt de vorige stappen werken omdat de toegang tot gebruikers en toepassingen voor Azure-services toestaan.  

Bij het maken van een service die een aangepaste service aanroept, gebruikt u de referenties van de Azure AD-client voor lokale ontwikkeling-verificatie.  Er zijn twee opties: 

1.  Een service-principal gebruiken om aan te melden bij Azure:

    1.  [Maken van een service-principal](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Azure CLI gebruiken voor het aanmelden:

        ```azurecli
        az login --service-principal -u <principal-id> --password <password> --tenant <tenant-id> --allow-no-subscriptions
        ```

        Omdat de service-principal geen toegang tot een abonnement hebt kan, gebruikt u de `--allow-no-subscriptions` argument.

2.  Omgevingsvariabelen gebruiken om op te geven van de details van de service-principal.  Zie voor meer informatie, [uitvoeren van de toepassing met behulp van een service-principal](#running-the-application-using-a-service-principal).

Zodra u zich hebt aangemeld bij Azure, `AzureServiceTokenProvider` de service-principal gebruikt voor het ophalen van een token voor lokale ontwikkeling.

Dit geldt alleen voor lokale ontwikkeling. Wanneer uw oplossing is geïmplementeerd in Azure, schakelt u de bibliotheek naar een beheerde identiteit voor de verificatie.

## <a name="running-the-application-using-managed-identity-or-user-assigned-identity"></a>Uitvoeren van de toepassing met behulp van beheerde identiteit of de gebruiker toegewezen identiteit 

Wanneer u de code op een Azure App Service of een Azure-VM met een beheerde identiteit is ingeschakeld uitvoeren, wordt automatisch de beheerde identiteit in de bibliotheek gebruikt. 

U kunt ook kan u verifiëren met een gebruiker toegewezen identiteit. Zie voor meer informatie over de gebruiker toegewezen identiteiten [over het beheren van identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work). Om te verifiëren met een gebruiker toegewezen identiteit, moet u de Client-ID van de gebruiker toegewezen identiteit in de verbindingsreeks opgeven. De verbindingsreeks is opgegeven in de [tekenreeks-Verbindingsondersteuning](#connection-string-support) onderstaande sectie.

## <a name="running-the-application-using-a-service-principal"></a>Uitvoeren van de toepassing met behulp van een Service-Principal 

Het is mogelijk nodig om u te maken van een Azure AD-Client-referentie om te verifiëren. Algemene voorbeelden zijn onder meer:

- Uw code wordt uitgevoerd op een lokale ontwikkelingsomgeving, maar niet onder de identiteit van de ontwikkelaar.  Service Fabric gebruikt bijvoorbeeld de [NetworkService-account](../service-fabric/service-fabric-application-secret-management.md) voor lokale ontwikkeling.
 
- Uw code wordt uitgevoerd op een lokale ontwikkelingsomgeving en u aanmelden bij een aangepaste service, zodat u uw developer-identiteit niet gebruiken. 
 
- Uw code wordt uitgevoerd op een Azure compute-resource die beheerde identiteiten nog niet voor Azure-resources, zoals Azure Batch biedt ondersteuning.

Er zijn drie primaire methoden van het gebruik van een Service-Principal voor uw toepassing wordt uitgevoerd. Als u wilt geen van beide gebruiken, moet u eerst [maken van een service-principal](/cli/azure/create-an-azure-service-principal-azure-cli).

### <a name="use-a-certificate-in-local-keystore-to-sign-into-azure-ad"></a>Een certificaat gebruiken in lokale keystore zich aanmelden bij Azure AD

1. Maak een service principal-certificaat met de Azure CLI [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) opdracht. 

    ```azurecli
    az ad sp create-for-rbac --create-cert
    ```

    Hiermee maakt u een .pem-bestand (persoonlijke sleutel) die worden opgeslagen in de basismap. Implementeren van dit certificaat aan de *LocalMachine /* of *CurrentUser* opslaan. 

    > [!Important]
    > De CLI-opdracht genereert een .pem-bestand, maar Windows biedt alleen ondersteuning voor PFX-certificaten. Voor het genereren van een PFX-certificaat in plaats daarvan, gebruikt u de PowerShell-opdrachten die hier worden weergegeven: [Service-principal maken met de zelf-ondertekend certificaat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate). Deze opdrachten wordt automatisch het certificaat ook implementeren.

1. Stel een omgevingsvariabele met de naam **AzureServicesAuthConnectionString** aan:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```
 
    Vervang *{AppId}* , *{tenant-id}* , en *{Thumbprint}* met waarden die in stap 1 is gegenereerd. Vervang *{CertificateStore}* in beide gevallen `LocalMachine` of `CurrentUser`, op basis van uw implementatieplan.

1. Voer de toepassing uit. 

### <a name="use-a-shared-secret-credential-to-sign-into-azure-ad"></a>Een gedeelde geheime referentie aan te melden bij Azure AD gebruiken

1. Een certificaat van service-principal maken met een wachtwoord met [az ad sp create-for-rbac--wachtwoord](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac). 

2. Stel een omgevingsvariabele met de naam **AzureServicesAuthConnectionString** aan:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret} 
    ```

    Vervang _{AppId}_ , _{tenant-id}_ , en _{ClientSecret}_ met waarden die in stap 1 is gegenereerd.

3. Voer de toepassing uit. 

Als alles goed, geen verdere wijzigingen in de code ingesteld zijn nodig.  `AzureServiceTokenProvider` maakt gebruik van de omgevingsvariabele en het certificaat te verifiëren met Azure AD. 

### <a name="use-a-certificate-in-key-vault-to-sign-into-azure-ad"></a>Een certificaat in Key Vault gebruiken voor aanmelding bij Azure AD

Deze optie kunt u een service-principal-clientcertificaat in Key Vault opslaan en deze gebruiken voor verificatie van service-principal. U kunt deze gebruiken voor de volgende scenario's:

* Lokale verificatie, waar u wilt verifiëren met behulp van een expliciete service-principal en veilig houden van de service-principal referentie in een key vault wilt. Developer-account moet toegang hebben tot de key vault. 
* Verificatie van Azure waar u wilt met expliciete referenties (bijvoorbeeld voor scenario's met meerdere tenants) en wilt de referentie van service-principal veilig bewaren in een key vault. Beheerde identiteit moet hebben toegang tot key vault. 

De beheerde identiteit of uw developer-identiteit moet gemachtigd zijn om op te halen van het certificaat van de Key Vault. De bibliotheek AppAuthentication maakt gebruik van de opgehaalde certificaat als referentie voor de service-principal-client.

Met een certificaat voor verificatie van service-principal

1. Een certificaat van service-principal maken en automatisch opgeslagen in uw Key Vault met de Azure CLI [az ad sp create-for-rbac--keyvault <keyvaultname> --cert <certificatename> --maken-cert--skip-toewijzing](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) opdracht:

    ```azurecli
    az ad sp create-for-rbac --keyvault <keyvaultname> --cert <certificatename> --create-cert --skip-assignment
    ```
    
    De certificaat-id is een URL in de indeling `https://<keyvaultname>.vault.azure.net/secrets/<certificatename>`

1. Vervang `{KeyVaultCertificateSecretIdentifier}` in deze verbindingsreeks met de certificaat-id:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}
    ```

    Als bijvoorbeeld uw key vault is met de naam 'myKeyVault' en u een certificaat met de naam 'myCert' hebt gemaakt, zou de certificaat-id:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier=https://myKeyVault.vault.azure.net/secrets/myCert
    ```


## <a name="connection-string-support"></a>Ondersteuning voor tekenreeks-verbindingen

Standaard `AzureServiceTokenProvider` meerdere methoden gebruikt voor het ophalen van een token. 

Voor het beheren van het proces, gebruik een verbindingsreeks die is doorgegeven aan de `AzureServiceTokenProvider` constructor of opgegeven in de *AzureServicesAuthConnectionString* omgevingsvariabele. 

De volgende opties worden ondersteund:

| Verbindingsreeksoptie | Scenario | Opmerkingen|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Lokale ontwikkeling | AzureServiceTokenProvider maakt gebruik van Azure CLI-token ophalen. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Lokale ontwikkeling | AzureServiceTokenProvider maakt gebruik van Visual Studio-token ophalen. |
| `RunAs=CurrentUser` | Lokale ontwikkeling | AzureServiceTokenProvider maakt gebruik van geïntegreerde verificatie van Azure AD-token ophalen. |
| `RunAs=App` | [Beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/index.yml) | AzureServiceTokenProvider maakt gebruik van een beheerde identiteit ophalen-token. |
| `RunAs=App;AppId={ClientId of user-assigned identity}` | [De gebruiker toegewezen identiteit voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) | AzureServiceTokenProvider maakt gebruik van een gebruiker toegewezen id ophalen-token. |
| `RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}` | Aangepaste services verificatie | KeyVaultCertificateSecretIdentifier = the certificate's secret identifier. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`| Service-principal | `AzureServiceTokenProvider` maakt gebruik van certificaten aan van Azure AD-token ophalen. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateSubjectName={Subject};CertificateStoreLocation={LocalMachine or CurrentUser}` | Service-principal | `AzureServiceTokenProvider` certificaat wordt gebruikt om op te halen token uit Azure AD|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Service-principal |`AzureServiceTokenProvider` maakt gebruik van geheim aan van Azure AD-token ophalen. |

## <a name="samples"></a>Voorbeelden

Om te zien de `Microsoft.Azure.Services.AppAuthentication` bibliotheek in actie, Zie de volgende voorbeelden van code.

1. [Gebruik van een beheerde identiteit voor het ophalen van een geheim uit Azure Key Vault tijdens runtime](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Een Azure Resource Manager-sjabloon van een Azure-VM met een beheerde identiteit programmatisch implementeren](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Voorbeeld van .NET Core en een beheerde identiteit gebruiken voor het aanroepen van Azure-services van een Azure Linux VM](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beheerde identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/index.yml).
- Meer informatie over [scenario's Azure AD-verificatie](../active-directory/develop/active-directory-authentication-scenarios.md).
