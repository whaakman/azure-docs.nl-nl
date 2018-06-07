---
title: Verifiëren met Azure AD van een Storage-toepassing (Preview) | Microsoft Docs
description: Verifiëren met Azure AD van een Azure Storage-toepassing (Preview).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 1bf4a8bba3b93c16f67d46f65292709ef2a1bba2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660805"
---
# <a name="authenticate-with-azure-ad-from-an-azure-storage-application-preview"></a>Verifiëren met Azure AD van een Azure Storage-toepassing (Preview)

Een groot voordeel van het gebruik van Azure Active Directory (Azure AD) met Azure Storage is dat uw referenties niet langer hoeft te worden opgeslagen in uw code. U kunt in plaats daarvan een OAuth 2.0-toegangstoken aanvragen van Azure AD. Azure AD de verificatie van de beveiligings-principal (een gebruiker, groep of service-principal) verwerkt de toepassing wordt uitgevoerd. Als de verificatie slaagt, Azure AD retourneert het toegangstoken voor de toepassing en de toepassing vervolgens het toegangstoken kunt gebruiken voor het autoriseren van aanvragen naar Azure Storage.

In dit artikel laat zien hoe uw toepassing voor verificatie met Azure AD configureren. Gebruik een soortgelijke benadering de .NET-code voorbeeld functies, maar andere talen.

Voordat u een beveiligingsprincipal van uw Azure Storage-toepassing verifiëren kunt, op rollen gebaseerde toegangsbeheer (RBAC) toegangsinstellingen configureren voor deze beveiligings-principal. Azure Storage definieert RBAC-rollen die machtigingen voor containers en -wachtrijen omvatten. Wanneer de RBAC-rol is toegewezen aan een beveiligings-principal, is die beveiligings-principal toegang tot deze resource. Zie voor meer informatie [beheren rechten voor het storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

Zie voor een overzicht van de stroom van OAuth 2.0 code grant [autoriseren toegang tot Azure Active Directory-webtoepassingen met de code OAuth 2.0 verlenen stroom](../../active-directory/develop/active-directory-protocols-oauth-code.md).

> [!IMPORTANT]
> Deze preview is bedoeld voor gebruik van niet-productieve alleen. Productie service level agreements (Sla's) worden pas beschikbaar Azure AD-integratie voor Azure Storage is gedeclareerd in het algemeen beschikbaar. Als u Azure AD-integratie is nog niet ondersteund voor uw scenario, blijven de gedeelde sleutel autorisatie of SAS-tokens gebruiken in uw toepassingen. Zie voor meer informatie over de evaluatieversie [verifiëren van toegang tot Azure Storage met Azure Active Directory (Preview)](storage-auth-aad.md).
>
> Tijdens de preview kunnen de RBAC-roltoewijzingen doorgeven maximaal vijf minuten duren.

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Uw toepassing registreren met een Azure AD-tenant

De eerste stap bij het gebruik van Azure AD toegang verlenen aan de storage-resources is de clienttoepassing wordt geregistreerd bij Azure AD-tenant. Registreren van uw toepassing, kunt u de Azure aanroepen [Active Directory Authentication Library](../../active-directory/active-directory-authentication-libraries.md) (ADAL) vanuit uw code. De ADAL biedt een API voor de verificatie met Azure AD van uw toepassing. Registreren van uw toepassing kunt u voor het aanroepen van deze toepassing naar Azure Storage-API's met een toegangstoken autoriseren.

Wanneer u uw toepassing registreert, kunt u informatie opgeven over uw toepassing naar Azure AD. Vervolgens Azure AD levert een client-ID (ook wel een *toepassings-ID*) waarmee u kunt uw toepassing koppelen aan Azure AD tijdens runtime. Zie voor meer informatie over de client-ID, [toepassing en service-principal objecten in Azure Active Directory](../../active-directory/develop/active-directory-application-objects.md).

Voor het registreren van uw Azure Storage-toepassing, volg de stappen in de [een toepassing toe te voegen](../../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) in sectie [toepassingen integreren met Azure Active Directory](../../active-directory/active-directory-integrating-applications.md). Als u uw toepassing als een systeemeigen toepassing registreren, kunt u een geldige URI voor de **omleidings-URI**. De waarde hoeft niet te worden van een echte-eindpunt.

![Schermopname die laat zien hoe u kunt uw storage-toepassing registreren met Azure AD](./media/storage-auth-aad-app/app-registration.png)

Nadat u uw toepassing hebt geregistreerd, ziet u de toepassings-ID (of de client-ID) onder **instellingen**:

![Schermopname van de client-ID](./media/storage-auth-aad-app/app-registration-client-id.png)

Zie voor meer informatie over het registreren van een toepassing met Azure AD [toepassingen integreren met Azure Active Directory](../../active-directory/develop/active-directory-integrating-applications.md). 

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Uw geregistreerde app-machtigingen verlenen voor Azure Storage

Vervolgens moet u uw Toepassingsmachtigingen verlenen voor Azure Storage-API aanroepen. Deze stap kunt uw toepassing te machtigen aanroepen naar Azure Storage met Azure AD.

1. Kies in het navigatiedeelvenster links van de Azure portal, **alle services**, en zoek naar **App registraties**.
2. Zoek de naam van de geregistreerde toepassing die u in de vorige stap hebt gemaakt.
3. Selecteer uw geregistreerde app en klik op **instellingen**. In de **API-toegang** sectie **vereist machtigingen**.
4. In de **vereist machtigingen** blade, klikt u op de **toevoegen** knop.
5. Onder **selecteert u een API**, zoek naar 'Azure Storage' en selecteer **Azure Storage** uit de lijst met resultaten.

    ![Schermopname van machtigingen voor opslag weergeven](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. Onder **machtigingen selecteren**, schakel het selectievakje in naast **toegang tot Azure Storage**, en klik op **Selecteer**.
7. Klik op **Gereed**.

De **Required Permissions** windows nu ziet u dat uw Azure AD-toepassing heeft toegang tot Azure Active Directory en Azure Storage. Machtigingen worden toegekend aan Azure AD automatisch wanneer u eerst uw app registreren bij Azure AD.

![Schermopname van registreren app-machtigingen](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="net-code-example-create-a-block-blob"></a>.NET-codevoorbeeld: maken van een blok-blob

De voorbeeldcode laat zien hoe u een toegang krijgt token van Azure AD. Het toegangstoken wordt gebruikt voor de opgegeven gebruiker verifiëren en autoriseren vervolgens een aanvraag voor het maken van een blok-blob. Als u dit voorbeeld werken, eerst de stappen in de vorige secties beschreven.

> [!NOTE]
> Als een eigenaar van uw Azure Storage-account zijn u machtigingen voor toegang tot gegevens niet automatisch toegewezen. U moet zelf expliciet een RBAC-rol toewijzen voor Azure Storage. U kunt deze op het niveau van uw abonnement, resourcegroep, storage-account of container of wachtrij toewijzen. 
>
> Bijvoorbeeld, als u wilt uitvoeren in de voorbeeldcode in een opslagaccount waar u eigenaar bent en onder uw eigen identiteit, moet u de RBAC-rol voor Blob Inzender aan uzelf toewijzen. Anders mislukt de aanroep voor het maken van de blob met HTTP-statuscode 403 (verboden). Zie voor meer informatie [beheren rechten voor het storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Bekende waarden voor verificatie met Azure AD

Om te verifiëren een beveiligings-principal met Azure AD, moet u een aantal bekende waarden wilt opnemen in uw code.

#### <a name="azure-ad-oauth-endpoint"></a>Azure AD-OAuth-eindpunt

De base Azure AD-instantie eindpunt voor OAuth 2.0 is als volgt, waarbij *tenant-id* is uw Active Directory-tenant-ID (of de map-ID):

`https://login.microsoftonline.com/<tenant-id>/oauth2/token`

De tenant-ID identificeert de Azure AD-tenant wilt gebruiken voor verificatie. Volg de stappen in voor het ophalen van de tenant-ID, **de tenant-ID niet ophalen voor uw Azure Active Directory**.

#### <a name="storage-resource-id"></a>Opslag resource-ID

Gebruik de Azure Storage resource-ID voor een token verkrijgen voor het verifiëren van aanvragen naar Azure Storage:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>De tenant-ID niet ophalen voor uw Azure Active Directory

Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
2. Klik op **Eigenschappen**.
3. Kopieer de GUID-waarde opgegeven voor de **map-ID**. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![Schermafdruk waarin het kopiëren van de tenant-ID](./media/storage-auth-aad-app/aad-tenant-id.png)

### <a name="add-references-and-using-statements"></a>Voeg verwijzingen toe en using-instructies  

In Visual Studio, installeert u de preview-versie van de Azure Storage-clientbibliotheek. Van de **extra** selecteert u **Nuget Package Manager**, klikt u vervolgens **Package Manager Console**. Typ de volgende opdracht in de console:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Vervolgens voegt u de volgende using-instructies toe aan uw code:

```dotnet
using Microsoft.IdentityModel.Clients.ActiveDirectory; //ADAL client library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Een OAuth-token ophalen uit Azure AD

Vervolgens voegt u een methode die een token van Azure AD-aanvragen. Aanroepen om aan te vragen het token, de [AuthenticationContext.AcquireTokenAsync](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync) methode.

```dotnet
static string GetUserOAuthToken()
{
    const string ResourceId = "https://storage.azure.com/"; // Storage resource endpoint
    const string AuthEndpoint = "https://login.microsoftonline.com/{0}/oauth2/token"; // Azure AD OAuth endpoint
    const string TenantId = "<tenant-id>"; // Tenant or directory ID

    // Construct the authority string from the Azure AD OAuth endpoint and the tenant ID. 
    string authority = string.Format(CultureInfo.InvariantCulture, AuthEndpoint, TenantId);
    AuthenticationContext authContext = new AuthenticationContext(authority);

    // Acquire an access token from Azure AD. 
    AuthenticationResult result = authContext.AcquireTokenAsync(ResourceId, 
                                                                "<client-id>", 
                                                                new Uri(@"<client-redirect-uri>"), 
                                                                new PlatformParameters(PromptBehavior.Auto)).Result;

    return result.AccessToken;
}
```

### <a name="create-the-block-blob"></a>Maken van het blok-blob

Ten slotte het toegangstoken gebruiken voor het maken van nieuwe referenties voor opslag en deze referenties gebruiken om te maken van de blob:

```dotnet
// Get the access token.
string accessToken = GetUserOAuthToken();

// Use the access token to create the storage credentials.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using those credentials
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
```

> [!NOTE]
> Azure AD-integratie met Azure Storage vereist het gebruik van HTTPS voor Azure Storage-bewerkingen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de rollen RBAC voor Azure-opslag, [beheren rechten voor het storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).
- Zie voor meer informatie over het gebruik van de Service-identiteit beheerd met Azure Storage, [verifiëren met Azure AD van een Azure Managed Service-identiteit (Preview)](storage-auth-aad-msi.md).
- Zie voor meer informatie over hoe u aan te melden bij Azure CLI en PowerShell met een Azure AD-identiteit, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell (Preview)](storage-auth-aad-script.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure Blobs en wachtrijen, het Azure Storage-team blogbericht, [aankondigen van de Preview van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).



