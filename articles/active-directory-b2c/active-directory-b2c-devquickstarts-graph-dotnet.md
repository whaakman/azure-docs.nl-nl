---
title: De Graph API in Azure Active Directory B2C gebruiken | Microsoft Docs
description: Over het aanroepen van de Graph-API voor een B2C-tenant met behulp van een toepassings-id om het proces te automatiseren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: da8aac2968ba020dd2b98253b12e8c9f223966e5
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442493"
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: De Azure AD Graph API gebruiken

>[!NOTE]
> Moet u de [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview?f=255&MSPPError=-2147217396) voor het beheren van gebruikers in een Azure AD B2C-directory. Dit wijkt af van de Microsoft Graph API. Klik [hier](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/) voor meer informatie.

Azure Active Directory (Azure AD) B2C-tenants zijn meestal groot. Dit betekent dat veel algemene beheertaken voor tenant moeten worden uitgevoerd via een programma. Een voorbeeld van een primaire is beheer van gebruikers. Mogelijk moet u een bestaande gebruiker store migreren naar een B2C-tenant. Kunt u gebruikersregistratie op uw eigen pagina hosten en Maak gebruikersaccounts in uw Azure AD B2C-directory achter de schermen. Deze typen taken vereist de mogelijkheid om te maken, lezen, bijwerken en verwijderen van gebruikersaccounts. U kunt deze taken uitvoeren met behulp van de Azure AD Graph API.

Voor B2C-tenants zijn er twee primaire modi voor de communicatie met de Graph API.

* Voor taken, interactieve, eenmalig worden uitgevoerd, moet u fungeren als een administrator-account in de B2C-tenant wanneer u de taken uitvoeren. Deze modus moet een beheerder zich aanmelden met referenties voordat die beheerder de aanroepen voor de Graph API kunt uitvoeren.
* Voor een geautomatiseerde, doorlopende taken, moet u een soort van service-account dat u met de benodigde machtigingen opgeeft om beheertaken uitvoeren. In Azure AD, kunt u dit doen door een toepassing registreren en zich verifiëren bij Azure AD. Dit wordt gedaan met behulp van een **toepassings-ID** die gebruikmaakt van de [verlenen van OAuth 2.0-clientreferenties](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). In dit geval wordt de toepassing fungeert als zelf, niet als een gebruiker, de Graph-API aan te roepen.

In dit artikel leert u hoe u de automatische-use-case uitvoert. Bouwt u een .NET 4.5 `B2CGraphClient` die de gebruiker voert maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen. De client heeft een Windows-opdrachtregelinterface (CLI) waarmee u verschillende methoden aanroepen. De code is echter geschreven in een niet-interactieve, geautomatiseerde manier gedragen.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een Azure AD B2C-tenant verkrijgen
Voordat u toepassingen of gebruikers maken kunt, moet u een Azure AD B2C-tenant. Als u nog geen een tenant hebt, [aan de slag met Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Uw toepassing registreren in uw tenant
Nadat u een B2C-tenant hebt, moet u registreren van uw toepassing met de [Azure-portal](https://portal.azure.com).

> [!IMPORTANT]
> Voor het gebruik van de Graph API met uw B2C-tenant, moet u registreert een toepassing met de *App-registraties* service in Azure portal, **niet** Azure AD B2C *toepassingen*menu. De volgende instructies leiden u naar het menu registratiecertificaat. U kunt bestaande B2C-toepassingen die u hebt geregistreerd in de Azure AD B2C niet opnieuw gebruiken *toepassingen* menu.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Kies uw Azure AD B2C-tenant door uw account te selecteren in de rechterbovenhoek van de pagina.
3. Kies in het navigatiedeelvenster links **alle Services**, klikt u op **App-registraties**, en klikt u op **toevoegen**.
4. Volg de aanwijzingen op het scherm en maak een nieuwe toepassing. 
    1. Selecteer **Web-App / API** als het toepassingstype.    
    2. Geef **een aanmeldings-URL** (bijvoorbeeld https://B2CGraphAPI) omdat dit niet relevant zijn voor dit voorbeeld.  
5. De toepassing wordt nu weergegeven in de lijst met toepassingen, klikt u op het verkrijgen van de **toepassings-ID** (ook wel bekend als Client-ID). Kopieer het zoals u hebt deze nodig in een volgende sectie.
6. Klik in het menu instellingen op **sleutels**.
7. In de **wachtwoorden** sectie, geef de beschrijving van de sleutel en selecteer een tijdsduur en klik vervolgens op **opslaan**. Kopieer de sleutelwaarde (ook wel bekend als Clientgeheim) voor gebruik in een volgende sectie.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Configureer maken, lezen en machtigingen voor uw toepassing bijwerken
Nu moet u uw toepassing configureren voor het ophalen van de vereiste machtigingen te maken, lezen, bijwerken en verwijderen van gebruikers.

1. Als u doorgaat in App-registraties-menu van de Azure portal, selecteer uw toepassing.
2. Klik in het menu instellingen op **vereiste machtigingen**.
3. Klik in het menu aan de vereiste machtigingen op **Windows Azure Active Directory**.
4. Selecteer in het menu toegang inschakelen en de **mapgegevens lezen en schrijven** toestemming van **Toepassingsmachtigingen** en klikt u op **opslaan**.
5. Ten slotte terug in het menu van de vereiste machtigingen, klik op de **machtigingen verlenen** knop.

U hebt nu een toepassing die gemachtigd om te maken is, lezen en bijwerken van gebruikers van uw B2C-tenant.

> [!NOTE]
> Machtigingen verlenen moeten volledig verwerken van een paar minuten duren.
> 
> 

## <a name="configure-delete-permissions-for-your-application"></a>Machtigingen voor het verwijderen van uw toepassing configureren
Op dit moment de *mapgegevens lezen en schrijven* machtiging heeft **niet** omvatten de mogelijkheid voor alle verwijderingen zoals het verwijderen van gebruikers. Als u wilt dat uw toepassing de mogelijkheid om gebruikers te verwijderen, moet u deze extra stappen uitvoeren die betrekking hebben op PowerShell, anders kunt u doorgaan naar de volgende sectie.

Als u nog geïnstalleerd hebt, installeer eerst de [Azure AD PowerShell v1-module (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0):

```powershell
Install-Module MSOnline
```

Na de installatie wordt de PowerShell-module verbinding maken met uw Azure AD B2C-tenant.

> [!IMPORTANT]
> U moet gebruiken een B2C-tenant administrator-account dat is **lokale** voor de B2C-tenant. Deze accounts als volgt uitzien: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Nu we gebruiken de **toepassings-ID** in het script hieronder om de toepassing de beheerdersrol van de gebruiker-account waarmee het verwijderen van gebruikers toewijzen. Deze rollen zijn bekende id's, dus u hoeft alleen de invoer is uw **toepassings-ID** in het onderstaande script.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Nu uw toepassing heeft ook machtigingen voor het verwijderen van gebruikers uit uw B2C-tenant.

## <a name="download-configure-and-build-the-sample-code"></a>Download en bouw de voorbeeldcode configureren
Eerst de voorbeeldcode downloaden en deze uitvoeren. We zullen vervolgens nog eens kijken.  U kunt [download de voorbeeldcode als ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). U kunt dit ook klonen naar een map van uw keuze:

```cmd
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Open de `B2CGraphClient\B2CGraphClient.sln` Visual Studio-oplossing in Visual Studio. In de `B2CGraphClient` project, open het bestand `App.config`. Vervang de drie app-instellingen door uw eigen waarden:

```xml
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Vervolgens met de rechtermuisknop op de `B2CGraphClient` oplossing en opnieuw maken in het voorbeeld. Als u geslaagde, u hebt nu een `B2C.exe` uitvoerbare bestand zich bevindt in `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Gebruiker CRUD-bewerkingen met behulp van de Graph-API bouwen
Voor het gebruik van de B2CGraphClient, opent u een `cmd` Windows command prompt en wijzig de directory in de `Debug` directory. Voer vervolgens de `B2C Help` opdracht.

```cmd
cd B2CGraphClient\bin\Debug
B2C Help
```

Hiermee wordt een korte beschrijving van elke opdracht weergegeven. Telkens wanneer u een van deze opdrachten aanroepen `B2CGraphClient` dient een aanvraag in bij de Azure AD Graph API.

### <a name="get-an-access-token"></a>Een toegangstoken opvragen
Elk verzoek aan de Graph API vereist een toegangstoken voor verificatie. `B2CGraphClient` maakt gebruik van de open-source Active Directory Authentication Library (ADAL) voor de toegangstokens te verkrijgen. ADAL maakt ophalen van tokens eenvoudiger door te geven van een eenvoudige API en zorg ervoor dat u van bepaalde belangrijke gegevens, zoals toegangstokens opslaan in cache. U moet geen gebruikmaken van ADAL om op te halen van tokens, hoewel. U kunt ook tokens ophalen door het samenstellen van HTTP-aanvragen.

> [!NOTE]
> Deze voorbeeldcode maakt gebruik van ADAL v2 om te communiceren met de Graph API.  U moet ADAL v2 of v3 gebruiken om toegangstokens die kunnen worden gebruikt met de Azure AD Graph API.
> 
> 

Wanneer `B2CGraphClient` wordt uitgevoerd, maakt het een exemplaar van de `B2CGraphClient` klasse. De constructor voor deze klasse stelt u de opbouw van een ADAL-verificatie:

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

We gebruiken de `B2C Get-User` opdracht als voorbeeld. Wanneer `B2C Get-User` is aangeroepen zonder een aanvullende invoer, aanroepen van de CLI de `B2CGraphClient.GetAllUsers(...)` methode. Deze methode roept `B2CGraphClient.SendGraphGetRequest(...)`, die een HTTP GET-aanvraag naar de Graph-API verzendt. Voordat u `B2CGraphClient.SendGraphGetRequest(...)` verzendt de GET-aanvragen, het eerst wordt een token met behulp van ADAL:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

U kunt een toegangstoken ophalen voor de Graph-API door het aanroepen van de ADAL `AuthenticationContext.AcquireToken(...)` methode. ADAL retourneert vervolgens een `access_token` die staat voor de identiteit van de toepassing.

### <a name="read-users"></a>Gebruikers lezen
Als u wilt ophalen van een lijst van gebruikers of een bepaalde gebruiker ophalen van de Graph-API, kunt u een HTTP verzenden `GET` aanvragen naar de `/users` eindpunt. Een aanvraag voor alle gebruikers in een tenant ziet er als volgt:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Als u wilt deze aanvraag zien, voert u de volgende uit:

 ```cmd
 B2C Get-User
 ```

Er zijn twee belangrijke dingen om te weten:

* Het toegangstoken verkregen via ADAL wordt toegevoegd aan de `Authorization` koptekst met behulp van de `Bearer` schema.
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
Wanneer u gebruikersaccounts in uw B2C-tenant maakt, kunt u een HTTP verzenden `POST` aanvragen naar de `/users` eindpunt:

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

De meeste van deze eigenschappen in deze aanvraag zijn vereist voor het maken van gebruikers van de consument. Voor meer informatie, klikt u op [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Houd er rekening mee dat de `//` opmerkingen zijn opgenomen voor een afbeelding. Voegt deze toe aan een werkelijke-aanvraag.

Als u wilt zien van de aanvraag, voert u een van de volgende opdrachten:

```cmd
B2C Create-User ..\..\..\usertemplate-email.json
B2C Create-User ..\..\..\usertemplate-username.json
```

De `Create-User` opdracht gebruikt u een .json-bestand als een invoerparameter. Dit bevat een JSON-weergave van een gebruikersobject. Er zijn twee voorbeeld .json-bestanden in de voorbeeldcode: `usertemplate-email.json` en `usertemplate-username.json`. U kunt deze bestanden om aan uw behoeften te wijzigen. Naast de bovenstaande vereiste velden zijn verschillende optionele velden die u kunt gebruiken in deze bestanden zijn opgenomen. Meer informatie over de optionele velden kunnen u vinden in de [verwijzing naar de Azure AD Graph API-entiteit](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

U kunt zien hoe de POST-aanvraag is gebouwd `B2CGraphClient.SendGraphPostRequest(...)`.

* Het toevoegen van een toegangstoken uit aan de `Authorization` -header van de aanvraag.
* Hiermee `api-version=1.6`.
* Het bevat de JSON-gebruikersobject in de hoofdtekst van de aanvraag.

> [!NOTE]
> Als de accounts die u wilt migreren van een bestaande archief van de gebruiker heeft lagere Wachtwoordsterkte dan de [sterke Wachtwoordsterkte afgedwongen door Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kunt u uitschakelen dat de sterk wachtwoord vereist met behulp van de `DisableStrongPassword` de waarde in de `passwordPolicies` eigenschap. Bijvoorbeeld, kunt u als volgt hierboven aanvraag voor de gebruiker van het maken: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Bijwerken van gebruikersaccounts van consumenten
Tijdens het bijwerken van objecten van is het proces vergelijkbaar met die gebruikersobjecten te maken. Maar dit proces maakt gebruik van de HTTP `PATCH` methode:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only the user's displayName
}
```

Probeer bij te werken van een gebruiker door het bijwerken van uw JSON-bestanden met nieuwe gegevens. Vervolgens kunt u `B2CGraphClient` een van deze opdrachten uit te voeren:

```cmd
B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Inspecteer de `B2CGraphClient.SendGraphPatchRequest(...)` methode voor meer informatie over deze aanvraag te verzenden.

### <a name="search-users"></a>Gebruikers zoeken
U kunt zoeken naar gebruikers in uw B2C-tenant in een aantal manieren. Een van de gebruiker met object-ID of twee, met behulp van de gebruiker aanmelden id (dat wil zeggen, de `signInNames` eigenschap).

Voer een van de volgende opdrachten om te zoeken naar een specifieke gebruiker:

```cmd
B2C Get-User <user-object-id>
B2C Get-User <filter-query-expression>
```

Hier volgen enkele voorbeelden:

```cmd
B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Gebruikers verwijderen
Het proces voor het verwijderen van een gebruiker is eenvoudig. De HTTP gebruiken `DELETE` methode en constructie URL met de juiste object-ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Als u wilt zien, voer de volgende opdracht en de delete-aanvraag die wordt weergegeven in de console weergeven:

```cmd
B2C Delete-User <object-id-of-user>
```

Inspecteer de `B2CGraphClient.SendGraphDeleteRequest(...)` methode voor meer informatie over deze aanvraag te verzenden.

U kunt veel andere acties met de Azure AD Graph API naast het beheer van gebruikers kunt uitvoeren. De [Azure AD Graph API-verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) vindt u informatie over elke actie, samen met de voorbeeldaanvragen.

## <a name="use-custom-attributes"></a>Aangepaste kenmerken gebruiken
De meeste klanttoepassingen moeten voor het opslaan van een type aangepaste gebruikersprofielgegevens. Er is een manier die u kunt dit doen voor het definiëren van een aangepast kenmerk in uw B2C-tenant. U kunt dit kenmerk vervolgens dezelfde manier als u een andere eigenschap van een gebruikersobject behandelen behandelen. U kunt het kenmerk bijwerken, verwijderen van het kenmerk, query's uitvoeren met het kenmerk, verzenden van het kenmerk als een claim in tokens-aanmelding en meer.

Zie voor het definiëren van een aangepast kenmerk in uw B2C-tenant, de [B2C aangepaste kenmerkverwijzing](active-directory-b2c-reference-custom-attr.md).

U kunt de aangepaste kenmerken die zijn gedefinieerd in uw B2C-tenant met behulp van weergeven `B2CGraphClient`:

```cmd
B2C Get-B2C-Application
B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

De uitvoer van deze functies blijkt dat de details van elk aangepast kenmerk, zoals:

```json
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

U kunt de volledige naam, zoals `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, als een eigenschap van de gebruikersobjecten.  Bijwerken van uw .json-bestand met de nieuwe eigenschap en een waarde voor de eigenschap en voer vervolgens:

```cmd
B2C Update-User <object-id-of-user> <path-to-json-file>
```

Met behulp van `B2CGraphClient`, hebt u een servicetoepassing die de gebruikers van uw B2C-tenant op programmatische wijze kunt beheren. `B2CGraphClient` maakt gebruik van een eigen toepassings-id voor de verificatie bij de Azure AD Graph API. Ook krijgt deze tokens met behulp van een clientgeheim in. Als u deze functionaliteit in uw toepassing opnemen, houd rekening met enkele belangrijke punten voor B2C-apps:

* U moet de juiste machtigingen in de tenant van de toepassing geven.
* Nu moet u gebruikmaken van ADAL (niet MSAL) om op te halen toegangstokens. (U kunt ook verzenden protocolberichten rechtstreeks, zonder gebruik van een bibliotheek.)
* Wanneer u de Graph-API aanroept, gebruikt u `api-version=1.6`.
* Wanneer u maken en bijwerken van de consument gebruikers, zijn enkele eigenschappen vereist, zoals hierboven is beschreven.

Als u vragen of verzoeken voor acties die u wilt uitvoeren met behulp van de Graph API op uw B2C-tenant hebt, laat dan een reactie op in dit artikel of bestand is een probleem in de voorbeeldopslagplaats in GitHub-code.

