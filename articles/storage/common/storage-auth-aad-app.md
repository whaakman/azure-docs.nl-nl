---
title: Verifiëren met Azure Active Directory voor toegang tot blob- en wachtrijservices gegevens uit uw toepassingen (Preview) | Microsoft Docs
description: Gebruik Azure Active Directory om te verifiëren vanaf binnen een toepassing en autorisatie van aanvragen voor blobs en wachtrijen (Preview).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 11/21/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: b817723120f07de9159e47c1259a68eb95b9c2e3
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140775"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues-preview"></a>Verifiëren met Azure Active Directory vanaf een aanvraag voor toegang tot blobs en wachtrijen (Preview)

Een groot voordeel van het gebruik van Azure Active Directory (Azure AD) met Azure Storage is dat uw referenties niet meer nodig hebt om te worden opgeslagen in uw code. U kunt in plaats daarvan een OAuth 2.0-toegangstoken aanvragen bij Azure AD. Azure AD de verificatie van de beveiligings-principal (een gebruiker, groep of service-principal) wordt de toepassing wordt uitgevoerd. Als de verificatie slaagt, Azure AD retourneert het toegangstoken voor de toepassing en de toepassing vervolgens het toegangstoken kunt gebruiken om aanvragen naar Azure Storage te autoriseren.

In dit artikel laat zien hoe uw toepassing configureren voor verificatie met Azure AD. De code voorbeeld onderdelen .NET, maar andere talen gebruiken een soortgelijke benadering.

Voordat u een beveiligings-principal van uw Azure Storage-toepassing verifiëren kunt, op rollen gebaseerd beheer (RBAC) toegangsinstellingen configureren voor deze beveiligings-principal. Azure Storage definieert RBAC-rollen die machtigingen voor containers en wachtrijen omvatten. Wanneer de RBAC-rol is toegewezen aan een beveiligings-principal, krijgt deze beveiligings-principal toegang tot die resource. Zie voor meer informatie, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

Zie voor een overzicht van de OAuth 2.0-stroom voor het verlenen van code, [autoriseren de toegang tot Azure Active Directory web-apps met behulp van de OAuth 2.0-code verlenen stroom](../../active-directory/develop/v1-protocols-oauth-code.md).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="assign-an-rbac-role-to-an-azure-ad-security-principal"></a>Een RBAC-rol toewijzen aan een beveiligings-principal voor Azure AD

Als u wilt een beveiligings-principal van uw Azure Storage-toepassing verifiëren, moet u eerst op basis van rollen (RBAC) instellingen voor toegangsbeheer voor deze beveiligings-principal configureren. Azure Storage definieert RBAC-rollen die machtigingen voor containers en wachtrijen omvatten. Wanneer de RBAC-rol is toegewezen aan een beveiligings-principal, krijgt deze beveiligings-principal toegang tot die resource. Zie voor meer informatie, [beheren toegangsrechten tot Azure BLOB Storage en Queue gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Uw toepassing registreren bij een Azure AD-tenant

De eerste stap bij het gebruik van Azure AD toegang verlenen aan de storage-resources is bezig met het registreren van uw clienttoepassing in een Azure AD-tenant. Registreren van uw toepassing, kunt u voor het aanroepen van de Azure [Active Directory Authentication Library](../../active-directory/active-directory-authentication-libraries.md) (ADAL) vanuit uw code. De ADAL-bibliotheek biedt een API voor verificatie met Azure AD van uw toepassing. Registreren van uw toepassing kunt u toestaan van aanroepen van toepassing bij naar Azure Storage-API's met een toegangstoken.

Als u uw toepassing registreert, kunt u informatie opgeven over uw toepassing naar Azure AD. Vervolgens Azure AD biedt een client-ID (ook wel een *toepassings-ID*) waarmee u kunt uw toepassing koppelen aan Azure AD tijdens runtime. Zie voor meer informatie over de client-ID, [toepassing en service-principalobjecten in Azure Active Directory](../../active-directory/develop/app-objects-and-service-principals.md).

Voor het registreren van uw Azure Storage-toepassing, volg de stappen in de [toevoegen van een toepassing](../../active-directory/develop/quickstart-v1-add-azure-ad-app.md) in sectie [toepassingen integreren met Azure Active Directory](../../active-directory/active-directory-integrating-applications.md). Als u uw toepassing als een systeemeigen toepassing registreert, kunt u een geldige URI voor de **omleidings-URI**. De waarde hoeft niet te worden geen echt eindpunt.

![Schermopname waarin uw storage-toepassing registreren bij Azure AD](./media/storage-auth-aad-app/app-registration.png)

Nadat u uw toepassing hebt geregistreerd, ziet u de toepassings-ID (of de client-ID) onder **instellingen**:

![Schermopname van de client-ID](./media/storage-auth-aad-app/app-registration-client-id.png)

Zie voor meer informatie over het registreren van een toepassing met Azure AD [toepassingen integreren met Azure Active Directory](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). 

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Uw geregistreerde app-machtigingen verlenen voor Azure Storage

Vervolgens moet u uw toepassing machtigen voor het aanroepen van Azure Storage-API's. Deze stap kan uw toepassing autoriseren aanroepen naar Azure Storage met Azure AD.

1. Kies in het linker navigatiedeelvenster van de Azure-portal **alle services**, en zoek naar de **App-registraties**.
2. Zoeken naar de naam van de geregistreerde toepassing die u in de vorige stap hebt gemaakt.
3. Selecteer uw geregistreerde app en klikt u op **instellingen**. In de **API-toegang** sectie, selecteer **vereiste machtigingen**.
4. In de **vereiste machtigingen** blade, klikt u op de **toevoegen** knop.
5. Onder **Select an API**, zoek naar 'Azure-opslag' en selecteer **Azure Storage** uit de lijst met resultaten.

    ![Schermopname waarin machtigingen voor opslag](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. Onder **machtigingen selecteren**, schakel het selectievakje in naast **toegang tot Azure Storage**, en klikt u op **Selecteer**.
7. Klik op **Gereed**.

De **vereiste machtigingen** windows nu ziet u dat uw Azure AD-toepassing toegang heeft tot zowel Azure Active Directory en Azure Storage. Machtigingen worden toegekend aan Azure AD automatisch wanneer u eerst uw app registreren bij Azure AD.

![Schermopname van registreren app-machtigingen](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="net-code-example-create-a-block-blob"></a>.NET-codevoorbeeld: maken van een blok-blob

Het codevoorbeeld toont hoe u een token uit Azure AD. Het toegangstoken wordt gebruikt voor het verifiëren van de opgegeven gebruiker en klikt u vervolgens autoriseren een aanvraag voor het maken van een blok-blob. Als u dit voorbeeld werkt, volgt u eerst de stappen die worden beschreven in de voorgaande secties.

> [!NOTE]
> Als een eigenaar van uw Azure Storage-account, zijn u machtigingen voor toegang tot gegevens niet automatisch toegewezen. U moet zelf expliciet een RBAC-rol toewijzen voor Azure Storage. U kunt deze op het niveau van uw abonnement, resourcegroep, opslagaccount of container of wachtrij toewijzen. 
>
> Bijvoorbeeld, voor het uitvoeren van de voorbeeldcode op een storage-account waarvan u eigenaar bent en onder uw eigen gebruikers-id moet u de RBAC-rol voor inzenders van Blob-gegevens naar uzelf. Anders mislukt de aanroep voor het maken van de blob met de HTTP-statuscode 403 (verboden). Zie voor meer informatie, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Bekende waarden voor verificatie met Azure AD

Als u wilt een beveiligings-principal met Azure AD verifiëren, moet u enkele bekende waarden opnemen in uw code.

#### <a name="azure-ad-authority"></a>Azure AD-instantie

Voor de openbare cloud van Microsoft, de base Azure AD-instantie is als volgt, waar *tenant-id* uw Active Directory-tenant-ID (of de map-ID):

`https://login.microsoftonline.com/<tenant-id>/`

De tenant-ID geeft de Azure AD-tenant te gebruiken voor verificatie. Als u wilt ophalen van de tenant-ID, volg de stappen **ophalen van de tenant-ID voor uw Azure Active Directory**.

#### <a name="storage-resource-id"></a>Storage-resource-ID

Gebruik de Azure Storage-resource-ID voor een token verkrijgen voor het verifiëren van aanvragen voor Azure Storage:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>De tenant-ID niet ophalen voor uw Azure Active Directory

Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
2. Klik op **Eigenschappen**.
3. Kopieer de GUID-waarde opgegeven voor de **map-ID**. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![Schermopname waarin het kopiëren van de tenant-ID](./media/storage-auth-aad-app/aad-tenant-id.png)

### <a name="add-references-and-using-statements"></a>Verwijzingen toevoegen en met behulp van instructies  

In Visual Studio, installeert u de preview-versie van de Azure Storage-clientbibliotheek. Uit de **extra** in het menu **Nuget Package Manager**, klikt u vervolgens **Package Manager Console**. Typ de volgende opdracht uit in de console voor het installeren van de meest recente versie van de clientbibliotheek voor .NET:

```
Install-Package WindowsAzure.Storage
```

Installeer ook de nieuwste versie van ADAL:

```
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

Voeg vervolgens de volgende using-instructies toe aan uw code:

```dotnet
using System.Globalization;
using Microsoft.IdentityModel.Clients.ActiveDirectory; //ADAL client library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Een OAuth-token ophalen uit Azure AD

Vervolgens voegt u een methode die een token van Azure AD aanvraagt toe. Aanroepen voor het aanvragen van het token, de [AuthenticationContext.AcquireTokenAsync](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync) methode. Zorg ervoor dat u hebt de volgende waarden van de stappen die u eerder hebt uitgevoerd:

- Tenant (directory)-ID
- ID van de client (toepassing)
- Client doorstuur-URI

```dotnet
static string GetUserOAuthToken()
{
    const string ResourceId = "https://storage.azure.com/";
    const string AuthInstance = "https://login.microsoftonline.com/{0}/";
    const string TenantId = "<tenant-id>"; // Tenant or directory ID

    // Construct the authority string from the Azure AD OAuth endpoint and the tenant ID. 
    string authority = string.Format(CultureInfo.InvariantCulture, AuthInstance, TenantId);
    AuthenticationContext authContext = new AuthenticationContext(authority);

    // Acquire an access token from Azure AD. 
    AuthenticationResult result = authContext.AcquireTokenAsync(ResourceId, 
                                                                "<client-id>", 
                                                                new Uri(@"<client-redirect-uri>"), 
                                                                new PlatformParameters(PromptBehavior.Auto)).Result;

    return result.AccessToken;
}
```

### <a name="create-the-block-blob"></a>De blok-blob maken

Ten slotte het toegangstoken gebruiken om te maken van nieuwe storage-referenties, en deze referenties gebruiken om de blob te maken:

```dotnet
// Get the access token.
string accessToken = GetUserOAuthToken();

// Use the access token to create the storage credentials.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using those credentials
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);

blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
```

> [!NOTE]
> Azure AD-integratie met Azure Storage is vereist dat u HTTPS voor Azure Storage-bewerkingen gebruiken.

In het bovenstaande voorbeeld worden de .NET-clientbibliotheek verwerkt de autorisatie van de aanvraag voor het maken van de blok-blob. Andere storage-clientbibliotheken worden ook de autorisatie van de aanvraag voor u verwerkt. Echter, als u een Azure Storage-bewerking met een OAuth-token met behulp van de REST-API aanroept, klikt u vervolgens u moet autoriseren van de aanvraag met het OAuth-token.   

Voor het aanroepen van Blob en Queue-service-bewerkingen met behulp van OAuth-toegangstokens, geeft u het toegangstoken in de **autorisatie** koptekst met behulp van de **Bearer** -schema en geeft u een serviceversie van 2017-11-09 of hoger, als in het volgende voorbeeld weergegeven:

```
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

Zie voor meer informatie over het machtigen van Azure Storage-bewerkingen van REST [verifiëren met Azure Active Directory (Preview)](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC-rollen voor Azure-opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).
- Zie voor meer informatie over het gebruik van beheerde identiteiten voor Azure-resources met Azure Storage, [verifiëren toegang tot blobs en wachtrijen met Azure beheerde identiteiten voor Azure-Resources (Preview)](storage-auth-aad-msi.md).
- Als u wilt weten hoe u zich aanmeldt bij Azure CLI en PowerShell met een Azure AD-identiteit, Zie [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell (Preview)](storage-auth-aad-script.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure-Blobs en wachtrijen, de blog van het Azure Storage-team plaatst, [aankondiging van de Preview-versie van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).



