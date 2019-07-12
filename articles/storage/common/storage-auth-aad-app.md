---
title: Verifiëren met Azure Active Directory voor toegang tot blob- en wachtrijservices gegevens vanuit de clienttoepassing
description: Gebruik Azure Active Directory om te verifiëren vanaf een clienttoepassing in, een OAuth 2.0-token ophalen en autoriseren van aanvragen voor Azure Blob storage en Queue storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/05/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: da10b70b85e284173abbd1779fb1d39f477ca0cd
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723224"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues"></a>Verifiëren met Azure Active Directory vanaf een aanvraag voor toegang tot blobs en wachtrijen

Een groot voordeel van het gebruik van Azure Active Directory (Azure AD) met Azure Blob-opslag of Queue storage is dat uw referenties niet meer nodig hebt om te worden opgeslagen in uw code. U kunt in plaats daarvan een OAuth 2.0-toegangstoken aanvragen bij de Microsoft identity-platform (voorheen Azure AD). Azure AD verifieert de beveiligings-principal (een gebruiker, groep of service-principal) waarop de toepassing wordt uitgevoerd. Als de verificatie slaagt, Azure AD retourneert het toegangstoken voor de toepassing en de toepassing vervolgens het toegangstoken kunt gebruiken om aanvragen naar Azure Blob-opslag of Queue storage te autoriseren.

In dit artikel laat zien hoe uw systeemeigen toepassing of de web-App voor verificatie configureren met Microsoft identity-platform 2.0. De code voorbeeld onderdelen .NET, maar andere talen gebruiken een soortgelijke benadering. Zie voor meer informatie over Microsoft identity-platform 2.0 [overzicht van Microsoft identity-platform (v2.0)](../../active-directory/develop/v2-overview.md).

Zie voor een overzicht van de OAuth 2.0-stroom voor het verlenen van code, [autoriseren de toegang tot Azure Active Directory web-apps met behulp van de OAuth 2.0-code verlenen stroom](../../active-directory/develop/v2-oauth2-auth-code-flow.md).

## <a name="assign-a-role-to-an-azure-ad-security-principal"></a>Een rol toewijzen aan een Azure AD beveiligings-principal

Als u wilt een beveiligings-principal van uw Azure Storage-toepassing verifiëren, moet u eerst op basis van rollen (RBAC) instellingen voor toegangsbeheer voor deze beveiligings-principal configureren. Azure Storage definieert ingebouwde RBAC-rollen die machtigingen voor containers en wachtrijen omvatten. Wanneer de RBAC-rol is toegewezen aan een beveiligings-principal, krijgt deze beveiligings-principal toegang tot die resource. Zie voor meer informatie, [beheren toegangsrechten tot Azure BLOB Storage en Queue gegevens met RBAC](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Uw toepassing registreren bij een Azure AD-tenant

De eerste stap bij het gebruik van Azure AD toegang verlenen aan de storage-resources met een Azure AD-tenant van uw clienttoepassing registreert de [Azure-portal](https://portal.azure.com). Als u uw clienttoepassing registreert, kunt u informatie over de toepassing naar Azure AD opgeven. Vervolgens Azure AD biedt een client-ID (ook wel een *toepassings-ID*) waarmee u kunt uw toepassing koppelen aan Azure AD tijdens runtime. Zie voor meer informatie over de client-ID, [toepassing en service-principalobjecten in Azure Active Directory](../../active-directory/develop/app-objects-and-service-principals.md).

Volg de stappen die worden weergegeven voor het registreren van uw Azure Storage-toepassing, [Quick Start: Een toepassing registreren met het Microsoft identity-platform](../../active-directory/develop/quickstart-configure-app-access-web-apis.md). De volgende afbeelding ziet u algemene instellingen voor het registreren van een web-App:

![Schermopname die laat zien hoe u uw storage-toepassing registreren bij Azure AD](./media/storage-auth-aad-app/app-registration.png)

> [!NOTE]
> Als u uw toepassing als een systeemeigen toepassing registreert, kunt u een geldige URI voor de **omleidings-URI**. Systeemeigen toepassing maakt, heeft deze waarde geen een echte URL. Voor webtoepassingen moet de omleidings-URI een geldige URI, omdat hiermee de URL die tokens worden geleverd.

Nadat u uw toepassing hebt geregistreerd, ziet u de toepassings-ID (of de client-ID) onder **instellingen**:

![Schermopname van de client-ID](./media/storage-auth-aad-app/app-registration-client-id.png)

Zie voor meer informatie over het registreren van een toepassing met Azure AD [toepassingen integreren met Azure Active Directory](../../active-directory/develop/quickstart-v2-register-an-app.md).

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Uw geregistreerde app-machtigingen verlenen voor Azure Storage

Vervolgens uw Toepassingsmachtigingen verlenen voor Azure Storage-API's aanroepen. Deze stap kan uw toepassing om aanvragen naar Azure Storage met Azure AD te autoriseren.

1. Op de **overzicht** -pagina voor uw geregistreerde toepassing selecteert **API weergavemachtigingen**.
1. In de **API-machtigingen** sectie, selecteer **toevoegen van een machtiging** en kies **Microsoft APIs**.
1. Selecteer **Azure Storage** uit de lijst met resultaten om weer te geven de **aanvragen API-machtigingen** deelvenster.
1. Onder **wat voor soort machtigingen is uw toepassing vereist?** , bekijk de beschikbare machtigingstype dat is **overgedragen machtigingen**. Deze optie is standaard geselecteerd voor u.
1. In de **machtigingen selecteren** sectie van de **aanvragen API-machtigingen** in het deelvenster het selectievakje naast **user_impersonation**, klikt u vervolgens op **toevoegen machtigingen**.

    ![Schermopname die laat zien machtigingen voor opslag](media/storage-auth-aad-app/registered-app-permissions-1.png)

De **API-machtigingen** deelvenster nu ziet u dat uw geregistreerde Azure AD-toepassing toegang heeft tot zowel Microsoft Graph en Azure Storage. Machtigingen worden toegekend aan Microsoft Graph automatisch wanneer u uw app eerst bij Azure AD registreren.

![Schermopname van registreren app-machtigingen](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="create-a-client-secret"></a>Een clientgeheim maken

De toepassing moet een clientgeheim voor de identiteit bewijst bij het aanvragen van een token. Als u wilt toevoegen van het clientgeheim, de volgende stappen uit:

1. Navigeer naar uw app-registratie in Azure portal.
1. Selecteer de **certificaten en geheimen** instelling.
1. Onder **Client geheimen**, klikt u op **nieuwe clientgeheim** om een nieuw geheim te maken.
1. Geef een beschrijving voor de geheime sleutel in en kies het gewenste Vervalinterval opgegeven.
1. Kopieer de waarde van het nieuwe geheim direct naar een veilige locatie. De volledige waarde wordt aan u slechts één keer weergegeven.

    ![Schermopname die laat zien clientgeheim](media/storage-auth-aad-app/client-secret.png)

## <a name="client-libraries-for-token-acquisition"></a>Clientbibliotheken voor het ophalen van tokens

Zodra u uw toepassing geregistreerd en deze machtigingen voor toegang tot gegevens in Azure Blob-opslag of Queue storage, kunt u code toevoegen aan uw toepassing een beveiligings-principal verifiëren en verkrijgen van een OAuth 2.0-token. Om te verifiëren en verkrijgen van het token, kunt u een van de [verificatiebibliotheken van Microsoft identity-platform](../../active-directory/develop/reference-v2-libraries.md) of een andere open-source-bibliotheek die ondersteuning biedt voor OpenID Connect 1.0. Uw toepassing kunt vervolgens het toegangstoken gebruiken om een aanvraag indient voor Azure Blob-opslag of Queue storage.

Zie voor een lijst met scenario's waarvoor verkrijgen van tokens wordt ondersteund, de [scenario's](https://aka.ms/msal-net-scenarios) sectie van de [Microsoft Authentication Library (MSAL) voor .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) GitHub-opslagplaats.

## <a name="net-code-example-create-a-block-blob"></a>Voorbeeld van .NET-code: Een blok-blob maken

Het codevoorbeeld toont hoe u een token uit Azure AD. Het toegangstoken wordt gebruikt voor het verifiëren van de opgegeven gebruiker en klikt u vervolgens autoriseren een aanvraag voor het maken van een blok-blob. Als u dit voorbeeld werkt, volgt u eerst de stappen die worden beschreven in de voorgaande secties.

Om aan te vragen het token, moet u de volgende waarden van de registratie van uw app:

- De naam van uw Azure AD-domein. Ophalen van deze waarde uit de **overzicht** pagina van uw Azure Active Directory.
- De tenant (of een map)-ID. Ophalen van deze waarde uit de **overzicht** pagina van uw app-registratie.
- De id van client (of een toepassing). Ophalen van deze waarde uit de **overzicht** pagina van uw app-registratie.
- De client doorstuur-URI. Ophalen van deze waarde uit de **verificatie** -instellingen voor de registratie van uw app.
- De waarde van het clientgeheim. Deze waarde wordt opgehaald uit de locatie waarnaar u eerder hebt gekopieerd.

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Bekende waarden voor verificatie met Azure AD

Als u wilt een beveiligings-principal met Azure AD verifiëren, moet u enkele bekende waarden opnemen in uw code.

#### <a name="azure-ad-authority"></a>Azure AD-instantie

Voor de openbare cloud van Microsoft, de base Azure AD-instantie is als volgt, waar *tenant-id* uw Active Directory-tenant-ID (of de map-ID):

`https://login.microsoftonline.com/<tenant-id>/`

De tenant-ID geeft de Azure AD-tenant te gebruiken voor verificatie. Dit is ook aangeduid als de id van de directory. Als u wilt ophalen van de tenant-ID, gaat u naar de **overzicht** pagina voor uw app-registratie in Azure portal en kopieer de waarde van daaruit.

#### <a name="storage-resource-id"></a>Storage-resource-ID

De Azure Storage-resource-ID gebruiken om een token voor het verlenen van aanvragen voor Azure Storage te verkrijgen:

`https://storage.azure.com/`

### <a name="create-a-storage-account-and-container"></a>Een storage-account en een container maken

De als codevoorbeeld wilt uitvoeren, moet u een opslagaccount in hetzelfde abonnement als uw Azure Active Directory maken. Vervolgens maakt u een container in het storage-account. De voorbeeldcode wordt een blok-blob maken in deze container.

Vervolgens kunt expliciet toewijzen aan de **Gegevensbijdrager voor Blob** rol aan het gebruikersaccount waarmee u de voorbeeldcode wordt uitgevoerd. Zie voor instructies over het toewijzen van deze rol in Azure portal [toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac-portal.md).

> [!NOTE]
> Wanneer u een Azure Storage-account maakt, zijn u de machtigingen voor toegang tot gegevens via Azure AD niet automatisch toegewezen. U moet zelf expliciet een RBAC-rol toewijzen voor Azure Storage. U kunt deze op het niveau van uw abonnement, resourcegroep, opslagaccount of container of wachtrij toewijzen.

### <a name="create-a-web-application-that-authorizes-access-to-blob-storage-with-azure-ad"></a>Maak een webtoepassing die gemachtigd voor toegang tot Blob-opslag met Azure AD

Wanneer uw toepassing toegang heeft tot Azure Storage, daarbij op de naam van de gebruiker, wat betekent dat blob of een wachtrij bronnen worden geopend met behulp van de machtigingen van de gebruiker die is aangemeld. Als u wilt dit codevoorbeeld uitproberen, moet u een webtoepassing waarin de gebruiker zich aanmelden met een Azure AD-identiteit. U kunt uw eigen maken of gebruiken van de voorbeeldtoepassing die is geleverd door Microsoft.

Een voltooide voorbeeld-web-App die een token verkrijgt en gebruikt voor het maken van een blob in Azure Storage is beschikbaar op [GitHub](https://aka.ms/aadstorage). Controleren en het uitvoeren van de voltooide voorbeeld kunnen handig zijn voor het begrijpen van de codevoorbeelden zijn. Voor instructies over het uitvoeren van de voltooide voorbeeld, Zie de sectie [weergeven en voer het voltooide voorbeeld](#view-and-run-the-completed-sample).

#### <a name="add-references-and-using-statements"></a>Verwijzingen toevoegen en met behulp van instructies  

Installeer de Azure Storage-clientbibliotheek vanuit Visual Studio. Uit de **extra** in het menu **Nuget Package Manager**, klikt u vervolgens **Package Manager Console**. Typ de volgende opdrachten in het consolevenster het nodig om pakketten te installeren vanuit de Azure Storage-clientbibliotheek voor .NET:

```console
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.Azure.Storage.Common
```

Voeg vervolgens de volgende using-instructies toe aan het bestand HomeController.cs:

```csharp
using Microsoft.Identity.Client; //MSAL library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

#### <a name="create-a-block-blob"></a>Een blok-blob maken

Voeg het volgende codefragment voor het maken van een blok-blob:

```csharp
private static async Task<string> CreateBlob(string accessToken)
{
    // Create a blob on behalf of the user
    TokenCredential tokenCredential = new TokenCredential(accessToken);
    StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

    // Replace the URL below with your storage account URL
    CloudBlockBlob blob =
        new CloudBlockBlob(
            new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
            storageCredentials);
    await blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
    return "Blob successfully created";
}
```

> [!NOTE]
> U moet HTTPS gebruiken voor het autoriseren van blob- en wachtrijservices bewerkingen met een OAuth 2.0-token.

In het bovenstaande voorbeeld worden de .NET-clientbibliotheek verwerkt de autorisatie van de aanvraag voor het maken van de blok-blob. Azure Storage-clientbibliotheken voor andere talen worden ook de autorisatie van de aanvraag voor u verwerkt. Echter, als u een Azure Storage-bewerking met een OAuth-token met behulp van de REST-API aanroept, klikt u vervolgens u moet autoriseren van de aanvraag met het OAuth-token.

Voor het aanroepen van Blob en Queue-service-bewerkingen met behulp van OAuth-toegangstokens, geeft u het toegangstoken in de **autorisatie** koptekst met behulp van de **Bearer** -schema en geeft u een serviceversie van 2017-11-09 of hoger, als in het volgende voorbeeld weergegeven:

```https
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

#### <a name="get-an-oauth-token-from-azure-ad"></a>Een OAuth-token ophalen uit Azure AD

Vervolgens voegt u een methode die een token van Azure AD aanvraagt toe. Het token dat u aanvragen worden namens de gebruiker en gebruiken we de GetTokenOnBehalfOfUser-methode.

Houd er rekening mee dat als u onlangs hebt aangemeld, en vraagt u een token voor de `storage.azure.com` resource, moet u de gebruiker met een gebruikersinterface waarin de gebruiker toestemming om namens hen een dergelijke actie te geven kan presenteren. Om dat u nodig hebt om af te vangen de `MsalUiRequiredException` en voeg de functionaliteit om aan te vragen de toestemming van de gebruiker, zoals weergegeven in het volgende voorbeeld:

```csharp
public async Task<IActionResult> Blob()
{
    var scopes = new string[] { "https://storage.azure.com/user_impersonation" };
    try
    {
        var accessToken =
            await _tokenAcquisition.GetAccessTokenOnBehalfOfUser(HttpContext, scopes);
        ViewData["Message"] = await CreateBlob(accessToken);
        return View();
    }
    catch (MsalUiRequiredException ex)
    {
        AuthenticationProperties properties = BuildAuthenticationPropertiesForIncrementalConsent(scopes, ex);
        return Challenge(properties);
    }
}
```

Toestemming is het proces van een gebruiker toestemming om een toepassing te krijgen tot beveiligde bronnen namens hen te verlenen. Het Microsoft identity-platform 2.0 ondersteunt incrementele toestemming te geven, wat betekent dat een beveiligings-principal kan in eerste instantie een minimale set machtigingen aanvragen en machtigingen na verloop van tijd, toevoegen indien nodig. Wanneer uw code een toegangstoken vraagt, geeft u het bereik van de machtigingen die uw app moet op een bepaald moment door in de `scope` parameter. Zie de sectie voor meer informatie over de incrementele toestemming **incrementele en dynamische toestemming** in [waarom bijwerken met Microsoft identity-platform (v2.0)?](../../active-directory/develop/azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent).

De volgende methode wordt de verificatie-eigenschappen voor het aanvragen van incrementele toestemming:

```csharp
private AuthenticationProperties BuildAuthenticationPropertiesForIncrementalConsent(string[] scopes, MsalUiRequiredException ex)
{
    AuthenticationProperties properties = new AuthenticationProperties();

    // Set the scopes, including the scopes that ADAL.NET / MSAL.NET need for the Token cache.
    string[] additionalBuildInScopes = new string[] { "openid", "offline_access", "profile" };
    properties.SetParameter<ICollection<string>>(OpenIdConnectParameterNames.Scope, scopes.Union(additionalBuildInScopes).ToList());

    // Attempt to set the login_hint so that the logged-in user is not presented with an account selection dialog.
    string loginHint = HttpContext.User.GetLoginHint();
    if (!string.IsNullOrWhiteSpace(loginHint))
    {
        properties.SetParameter<string>(OpenIdConnectParameterNames.LoginHint, loginHint);

        string domainHint = HttpContext.User.GetDomainHint();
        properties.SetParameter<string>(OpenIdConnectParameterNames.DomainHint, domainHint);
    }

    // Specify any additional claims that are required (for instance, MFA).
    if (!string.IsNullOrEmpty(ex.Claims))
    {
        properties.Items.Add("claims", ex.Claims);
    }

    return properties;
}
```

## <a name="view-and-run-the-completed-sample"></a>Weergeven en uitvoeren van de voltooide voorbeeld

Voor het uitvoeren van de voorbeeldtoepassing, eerst klonen of downloaden van [GitHub](https://aka.ms/aadstorage). Werk vervolgens de toepassing, zoals beschreven in de volgende secties.

### <a name="provide-values-in-the-settings-file"></a>Geef de waarden in het bestand met instellingen

Werk vervolgens de *appsettings.json* bestand door uw eigen waarden als volgt:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "<azure-ad-domain-name>.onmicrosoft.com",
    "TenantId": "<tenant-id>",
    "ClientId": "<client-id>",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc",

    // To call an API
    "ClientSecret": "<client-secret>"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="update-the-storage-account-and-container-name"></a>De naam van het storage-account en container bijwerken

In de *HomeController.cs* bestand, het bijwerken van de URI die verwijst naar de blok-blob voor het gebruik van de naam van uw opslagaccount en container:

```csharp
CloudBlockBlob blob = new CloudBlockBlob(
                      new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
                      storageCredentials);
```

### <a name="enable-implicit-grant-flow"></a>Impliciete stroom inschakelen

De als voorbeeld wilt uitvoeren, moet u mogelijk de impliciete stroom voor de registratie van uw app configureren. Volg deze stappen:

1. Navigeer naar uw app-registratie in Azure portal.
1. Selecteer in het gedeelte beheer de **verificatie** instelling.
1. Onder **geavanceerde instellingen**, in de **impliciete** sectie, schakel de selectievakjes om in te schakelen toegangstokens en ID-tokens, zoals wordt weergegeven in de volgende afbeelding:

    ![Schermopname die laat zien hoe u instellingen voor impliciete stroom inschakelen](media/storage-auth-aad-app/enable-implicit-grant-flow.png)

### <a name="update-the-port-used-by-localhost"></a>De poort die wordt gebruikt door ' localhost ' bijwerken

Wanneer u het voorbeeld uitvoert, merkt u dat u de URI die is opgegeven in de appregistratie van uw moet te gebruiken omleidings bijwerken de *localhost* poort toegewezen tijdens runtime. Volg deze stappen voor het bijwerken van de omleidings-URI om de toegewezen poort te gebruiken:

1. Navigeer naar uw app-registratie in Azure portal.
1. Selecteer in het gedeelte beheer de **verificatie** instelling.
1. Onder **omleidings-URI's**, bewerken, de poort zodat deze overeenkomt met die gebruikt door de voorbeeld-App, zoals wordt weergegeven in de volgende afbeelding:

    ![Schermafbeelding van de omleidings-URI's voor app-registratie](media/storage-auth-aad-app/redirect-uri.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het Microsoft identity-platform, [identiteitsplatform van Microsoft](https://docs.microsoft.com/azure/active-directory/develop/).
- Zie voor meer informatie over RBAC-rollen voor Azure-opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md).
- Zie voor meer informatie over het gebruik van beheerde identiteiten voor Azure-resources met Azure Storage, [verifiëren van toegang tot blobs en wachtrijen met Azure Active Directory en beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md).
