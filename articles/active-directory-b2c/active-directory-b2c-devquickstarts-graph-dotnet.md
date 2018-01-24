---
title: Gebruik de Graph-API - Azure AD B2C | Microsoft Docs
description: Hoe de Graph API-aanroep voor een B2C-tenant met behulp van een toepassings-id om het proces te automatiseren.
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: 33df6c4255d4ca672e65237c8be45b3f0bc7864e
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: Gebruik de Azure AD Graph API

>[!NOTE]
> Moet u de [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview?f=255&MSPPError=-2147217396) voor het beheren van gebruikers in een Azure AD B2C-directory. Dit wijkt af van de Microsoft Graph API. Klik [hier](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/) voor meer informatie.

Azure Active Directory (Azure AD) B2C tenants vaak erg groot. Dit betekent dat veel algemene beheertaken voor tenant moeten worden uitgevoerd via een programma. Een voorbeeld van een primaire is Gebruikersbeheer. Mogelijk moet u een bestaande gebruiker store migreren naar een B2C-tenant. U kunt hosten gebruikersregistratie op de pagina en gebruikersaccounts maken in uw Azure AD B2C-directory achter de schermen. Deze typen taken moeten kunnen maken, lezen, bijwerken en verwijderen van gebruikersaccounts. U kunt deze taken uitvoeren met behulp van de Azure AD Graph API.

Voor B2C-tenants zijn er twee primaire modi voor de communicatie met de Graph API.

* Voor interactieve, eenmalige taken, moet u fungeren als een administrator-account in de B2C-tenant wanneer u de taken uitvoeren. Deze modus moet een beheerder zich aanmelden met referenties waarover de beheerder om de aanroepen van de Graph API kan uitvoeren.
* Voor de geautomatiseerde, continue taken, moet u een type-serviceaccount de juiste rechten heeft voor beheertaken in te voorzien. In Azure AD, kunt u dit doen door een toepassing registreren en te verifiëren met Azure AD. Dit wordt gedaan met behulp van een **toepassings-ID** die gebruikmaakt van de [OAuth 2.0-clientreferenties verlenen](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). In dit geval wordt fungeert de toepassing als zichzelf niet als een gebruiker, de API van de grafiek aan te roepen.

In dit artikel bespreken we hoe u de automatische gebruiksvoorbeeld uitvoert. U kunt zien, moet gaan we verder met een .NET 4.5 `B2CGraphClient` die de gebruiker voert maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen. De client heeft een Windows-opdrachtregelinterface (CLI) waarmee u verschillende methoden aanroepen. De code is echter zich gedragen in een niet-interactieve, geautomatiseerde manier geschreven.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant verkrijgen
Voordat u kunt maken van toepassingen of gebruikers of helemaal met Azure AD communiceren, moet u een Azure AD B2C-tenant en een globale beheerdersaccount in de tenant. Als u nog geen een tenant hebt, [aan de slag met Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Uw toepassing registreren in uw tenant
Nadat u een B2C-tenant hebt, moet u registreren van uw toepassingen via de [Azure Portal](https://portal.azure.com).

> [!IMPORTANT]
> Als u wilt gebruiken de Graph API met uw B2C-tenant, moet u een specifieke toepassing registreren met behulp van de algemene *App registraties* menu in de Azure-Portal **niet** Azure AD B2C  *Toepassingen* menu. U kunt de al bestaande B2C toepassingen die u hebt geregistreerd in de Azure AD B2C niet opnieuw gebruiken *toepassingen* menu.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Uw account selecteren in de rechterbovenhoek van de pagina kiest u uw Azure AD B2C-tenant.
3. Kies in het navigatiedeelvenster links **meer Services**, klikt u op **App registraties**, en klik op **toevoegen**.
4. Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. 
    1. Selecteer **Web-App / API** als het toepassingstype.    
    2. Geef **een omleidings-URI** (bijvoorbeeld https://B2CGraphAPI) als het is niet relevant zijn voor dit voorbeeld.  
5. De toepassing wordt nu weergegeven in de lijst met toepassingen, klikt u op het verkrijgen van de **toepassings-ID** (ook wel bekend als een Client-ID). Kopieer de verbindingsreeks als u hebt deze nodig in een volgende sectie.
6. Klik in het menu instellingen op **sleutels** en voeg een nieuwe sleutel (ook wel bekend als clientgeheim). Ook het kopiëren voor gebruik in een volgende sectie.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Configureer maken, lezen en bijwerken van machtigingen voor uw toepassing
Nu moet u uw toepassing configureren voor het ophalen van de vereiste machtigingen te maken, lezen, bijwerken en verwijderen van gebruikers.

1. Als u doorgaat in de Azure portal App registraties menu, selecteer uw toepassing.
2. Klik in het menu instellingen op **vereist machtigingen**.
3. Klik in het menu vereiste machtigingen op **Windows Azure Active Directory**.
4. Selecteer in het menu toegang inschakelen de **lezen en schrijven directorygegevens** machtiging van **Toepassingsmachtigingen** en klik op **opslaan**.
5. Ten slotte terug in het menu van de vereiste machtigingen, klik op de **machtiging verlenen** knop.

U hebt nu een toepassing die gemachtigd om te maken is, lezen en bijwerken van gebruikers van uw B2C-tenant.

> [!NOTE]
> Machtigingen verlenen zorg enkele minuten duren voor het volledig verwerken.
> 
> 

## <a name="configure-delete-permissions-for-your-application"></a>Verwijdermachtigingen voor uw toepassing configureren
Op dit moment wordt de *lezen en schrijven directorygegevens* machtiging heeft **niet** de mogelijkheid om eventuele verwijderingen zoals het verwijderen van gebruikers doen. Als u wilt dat uw toepassing bieden de mogelijkheid om gebruikers te verwijderen, moet u deze extra stappen uit te voeren die betrekking hebben op PowerShell, anders kunt u doorgaan met de volgende sectie.

Als u niet al geïnstalleerd hebt, installeer eerst de [Azure AD PowerShell v1-module (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0):

```powershell
Install-Module MSOnline
```

Nadat u verbinding met uw Azure AD B2C-tenant de PowerShell-module maken.

> [!IMPORTANT]
> U moet een administrator-account voor B2C-tenant is gebruiken **lokale** aan de B2C-tenant. Deze accounts moeten uitzien: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Nu we gebruiken de **toepassings-ID** in het onderstaande de account administrator gebruikersrol waarmee het verwijderen van gebruikers voor de toepassing toewijzen script. Deze rollen hebben een bekende id's, hoeft u invoer uw **toepassings-ID** in het onderstaande script.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Nu uw toepassing heeft ook machtigingen voor het verwijderen van gebruikers van uw B2C-tenant.

## <a name="download-configure-and-build-the-sample-code"></a>Downloaden, configureren en de voorbeeldcode bouwen
Eerst de voorbeeldcode downloaden en deze uitgevoerd. Het duurt we vervolgens een nader bekijken.  U kunt [de voorbeeldcode als ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). U kunt dit ook klonen naar een map van uw keuze:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Open de `B2CGraphClient\B2CGraphClient.sln` Visual Studio-oplossing in Visual Studio. In de `B2CGraphClient` project, open het bestand `App.config`. Instellingen voor de drie app vervangen door uw eigen waarden:

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Vervolgens met de rechtermuisknop op de `B2CGraphClient` oplossing en opnieuw maken in het voorbeeld. Als u geslaagde, u hebt nu een `B2C.exe` uitvoerbare bestand zich in `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Gebruiker CRUD-bewerkingen met behulp van de Graph API bouwen
Voor het gebruik van de B2CGraphClient, opent u een `cmd` Windows command prompt en wijzig de directory in de `Debug` directory. Voer vervolgens de `B2C Help` opdracht.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Hiermee wordt een korte beschrijving van elke opdracht weergegeven. Telkens wanneer u een van deze opdrachten aanroepen `B2CGraphClient` doet een aanvraag bij de Azure AD Graph API.

### <a name="get-an-access-token"></a>Een toegangstoken opvragen
Elk verzoek aan de Graph API vereist een toegangstoken voor verificatie. `B2CGraphClient`de open source Active Directory Authentication Library (ADAL) gebruikt om u te helpen toegangstokens te verkrijgen. ADAL gemakkelijker token overname door te geven van een eenvoudige API en wordt gelet op een aantal belangrijke details, zoals caching toegangstokens. Er geen gebruik van ADAL tokens, hoewel ophalen. U kunt ook tokens ophalen door HTTP-aanvragen.

> [!NOTE]
> Deze voorbeeldcode maakt gebruik van ADAL v2 om te communiceren met de Graph API.  U moet ADAL v2 of v3 gebruiken om op te halen van de toegangstokens te geven die kunnen worden gebruikt met de Azure AD Graph API.
> 
> 

Wanneer `B2CGraphClient` wordt uitgevoerd, maakt het een exemplaar van de `B2CGraphClient` klasse. De constructor voor deze klasse stelt u een steigers ADAL-verificatie:

```csharp
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

We gebruiken de `B2C Get-User` opdracht als voorbeeld. Wanneer `B2C Get-User` wordt aangeroepen zonder eventuele extra ingangen, het aanroepen van de CLI de `B2CGraphClient.GetAllUsers(...)` methode. Deze methode aanroept `B2CGraphClient.SendGraphGetRequest(...)`, die een HTTP GET-aanvraag naar de Graph API worden verzonden. Voordat u `B2CGraphClient.SendGraphGetRequest(...)` verzendt de GET-aanvragen, het eerst ophalen van een token met behulp van ADAL:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

U kunt een toegangstoken ophalen voor de Graph API door het aanroepen van de ADAL `AuthenticationContext.AcquireToken(...)` methode. ADAL retourneert een `access_token` die staat voor de identiteit van de toepassing.

### <a name="read-users"></a>Lezen van gebruikers
Als u een lijst met gebruikers of een bepaalde gebruiker ophalen uit de Graph API wilt, kunt u een HTTP verzenden `GET` aanvraag voor de `/users` eindpunt. Een aanvraag voor alle gebruikers in een tenant ziet er als volgt:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Overzicht van deze aanvraag uitvoeren:

 ```
 > B2C Get-User
 ```

Er zijn twee belangrijke opmerkingen:

* Het toegangstoken dat is verkregen via de ADAL wordt toegevoegd aan de `Authorization` header met behulp van de `Bearer` schema.
* Voor B2C-tenants, moet u de queryparameter `api-version=1.6`.

Beide van deze gegevens worden verwerkt in de `B2CGraphClient.SendGraphGetRequest(...)` methode:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Maak gebruikersaccounts van consumenten
Wanneer u gebruikersaccounts in uw B2C-tenant maken, kunt u een HTTP verzenden `POST` aanvraag voor de `/users` eindpunt:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying to the end user
    "mailNickname": "joec",                        // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

De meeste van deze eigenschappen in deze aanvraag zijn vereist voor het maken van de consument gebruikers. Als u wilt weten, klikt u op [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Houd er rekening mee dat de `//` opmerkingen zijn opgenomen ter illustratie. Neem ze niet in een werkelijke-aanvraag.

Overzicht van de aanvraag, voert u een van de volgende opdrachten:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

De `Create-User` opdracht gebruikt u een .json-bestand als invoerparameter. Dit document bevat een JSON-weergave van een gebruikersobject. Er zijn twee voorbeeldbestanden .json in de voorbeeldcode: `usertemplate-email.json` en `usertemplate-username.json`. U kunt deze bestanden aan uw behoeften kunt wijzigen. Naast de bovenstaande vereiste velden zijn verschillende optionele velden die u kunt in deze bestanden zijn opgenomen. Meer informatie over de optionele velden vindt u in de [entiteitsverwijzing Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

U kunt zien hoe de POST-aanvraag is geconstrueerd in `B2CGraphClient.SendGraphPostRequest(...)`.

* Het toevoegen van een toegangstoken de `Authorization` koptekst van de aanvraag.
* Hiermee `api-version=1.6`.
* De gebruiker JSON-object in de hoofdtekst van de aanvraag opgenomen.

> [!NOTE]
> Als de accounts die u wilt migreren vanuit een bestaande gebruiker store lagere Wachtwoordsterkte dan heeft de [sterk Wachtwoordsterkte afgedwongen door Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kunt u uitschakelen de sterk wachtwoord vereist met behulp van de `DisableStrongPassword` de waarde in de `passwordPolicies` eigenschap. Bijvoorbeeld, kunt u een aanvraag voor het maken gebruiker hierboven beschreven als volgt wijzigen: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Gebruikersaccounts van consumenten bijwerken
Wanneer u gebruikersobjecten bijwerkt, is het proces vergelijkbaar met die gebruikersobjecten te maken. Maar dit proces maakt gebruik van de HTTP `PATCH` methode:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only the user's displayName
}
```

Probeer het bijwerken van een gebruiker door het bijwerken van uw JSON-bestanden met nieuwe gegevens. Vervolgens kunt u `B2CGraphClient` een van deze opdrachten uit te voeren:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Inspecteer de `B2CGraphClient.SendGraphPatchRequest(...)` methode voor meer informatie over het verzenden van deze aanvraag.

### <a name="search-users"></a>Gebruikers zoeken
U kunt zoeken naar gebruikers in uw B2C-tenant in een aantal manieren. Een van de gebruiker met object-ID of twee, met behulp van de gebruiker aanmelden id (dat wil zeggen, de `signInNames` eigenschap).

Voer een van de volgende opdrachten om te zoeken naar een specifieke gebruiker:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Hier volgen enkele voorbeelden:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Gebruikers verwijderen
Het proces voor het verwijderen van een gebruiker is eenvoudig. De HTTP gebruiken `DELETE` methode en constructie de URL met de juiste object-ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Een voorbeeld bekijken, voer de volgende opdracht en de delete-aanvraag is afgedrukt op de console weergeven:

```
> B2C Delete-User <object-id-of-user>
```

Inspecteer de `B2CGraphClient.SendGraphDeleteRequest(...)` methode voor meer informatie over het verzenden van deze aanvraag.

U kunt met de Azure AD Graph API naast Gebruikersbeheer veel acties uitvoeren. De [Azure AD Graph API-referentiemateriaal](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) worden details weergegeven over elke actie, samen met de voorbeeld-aanvragen.

## <a name="use-custom-attributes"></a>Aangepaste kenmerken gebruiken
De meeste toepassingen van de consument moeten een type aangepaste gebruikersprofielgegevens opslaan. Een manier die u kunt dit doen, is een aangepast kenmerk definiëren in uw B2C-tenant. U kunt dit kenmerk vervolgens dezelfde manier als u een andere eigenschap van een gebruikersobject behandelen behandelen. U kunt het kenmerk bijwerken, verwijdert u het kenmerk, opvragen door het kenmerk, het kenmerk verzenden als claim in aanmelden tokens en meer.

Als u wilt een aangepast kenmerk definiëren in uw B2C-tenant, Zie de [B2C aangepast kenmerkverwijzing](active-directory-b2c-reference-custom-attr.md).

U kunt de aangepaste kenmerken die zijn gedefinieerd in uw B2C-tenant met behulp van weergeven `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

De uitvoer van deze functies blijkt, zoals dat de details van elk aangepast kenmerk:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

U kunt de volledige naam zoals `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, als een eigenschap van de gebruikersobjecten.  Bijwerken van uw .json-bestand met de nieuwe eigenschap en een waarde voor de eigenschap en voer vervolgens:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Met behulp van `B2CGraphClient`, hebt u een servicetoepassing die uw gebruikers B2C-tenant programmatisch kunt beheren. `B2CGraphClient`een eigen toepassingsidentiteit gebruikt om de Azure AD Graph API te verifiëren. Ook krijgt deze tokens met behulp van een clientgeheim. Als u deze functionaliteit in uw toepassing opnemen, houd rekening met enkele belangrijke punten voor B2C-apps:

* U moet de toepassing geven de juiste machtigingen in de tenant.
* Nu moet u gebruikmaken van ADAL (geen MSAL) toegangstokens ophalen. (U kunt ook verzenden protocolberichten rechtstreeks, zonder gebruik van een bibliotheek.)
* Wanneer u de Graph API aanroept, gebruik `api-version=1.6`.
* Wanneer u maken en consumenten gebruikers bij te werken, is enkele eigenschappen zijn vereist, zoals hierboven is beschreven.

Als u vragen of aanvragen voor acties die u wilt uitvoeren met behulp van de Graph API op uw B2C-tenant hebt, laat u een opmerking voor dit artikel of bestand is een probleem in de GitHub-opslagplaats voor het voorbeeld van code.

