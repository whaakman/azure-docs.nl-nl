---
title: Migreren naar MSAL.NET | Azure
description: Meer informatie over de verschillen tussen Microsoft Authentication Library voor .NET (MSAL.NET) en Azure AD Authentication Library voor .NET (ADAL.NET) en migreren naar MSAL.NET.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 028c7f5d42587a6b2129bba07831b0e799d607f4
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544221"
---
# <a name="migrating-applications-to-msalnet"></a>Toepassingen te migreren naar MSAL.NET

Zowel Microsoft Authentication Library voor .NET (MSAL.NET) en Azure AD Authentication Library voor .NET (ADAL.NET) worden gebruikt voor verificatie van Azure AD-entiteiten en aanvragen tokens van Azure AD. Tot nu toe heeft de meeste ontwikkelaars gewerkt met Azure AD voor ontwikkelaars-platform (v1.0) voor verificatie van Azure AD-identiteiten (werk en school-accounts) door aan te vragen met behulp van Azure AD Authentication Library (ADAL). Nu, met behulp van MSAL.NET, kunt u verifiëren een bredere set van Microsoft-identiteiten (Azure AD-identiteiten en Microsoft-accounts en sociale en lokale accounts via Azure AD B2C) via het eindpunt van de Microsoft identity-platform. 

In dit artikel wordt beschreven hoe u kiezen tussen Microsoft Authentication Library voor .NET (MSAL.NET) en Azure AD Authentication Library voor .NET (ADAL.NET) en vergelijkt de twee bibliotheken.  

## <a name="differences-between-adal-and-msal-apps"></a>Verschillen tussen ADAL en MSAL-apps
In de meeste gevallen wilt u MSAL.NET en de Microsoft identity-platform-eindpunt, dat de nieuwste generatie van Microsoft-verificatiebibliotheken is gebruiken. Met behulp van MSAL.NET, u verkrijgen van tokens voor gebruikers aanmelden bij uw toepassing met Azure AD (werk en school-accounts), Microsoft (persoonlijk)-accounts (MSA) of Azure AD B2C. 

Als u al bekend bent met de Azure AD voor ontwikkelaars (v1.0)-eindpunt (en ADAL.NET), kunt u lezen [wat is er anders bij het Microsoft identity-platform (v2.0) eindpunt?](active-directory-v2-compare.md).

U moet echter wel ADAL.NET gebruiken als uw toepassing moet aan te melden bij de gebruikers met eerdere versies van [Active Directory Federation Services (ADFS)](/windows-server/identity/active-directory-federation-services). Zie voor meer informatie, [ondersteuning voor AD FS](https://aka.ms/msal-net-adfs-support).

De volgende afbeelding ziet u enkele van de verschillen tussen ADAL.NET en MSAL.NET ![Side-by-side-code](./media/msal-compare-msaldotnet-and-adaldotnet/differences.png)

### <a name="nuget-packages-and-namespaces"></a>NuGet-pakketten en -naamruimten

ADAL.NET wordt gebruikt vanaf de [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) NuGet-pakket. de naamruimte te gebruiken is `Microsoft.IdentityModel.Clients.ActiveDirectory`.

Gebruik van MSAL.NET die u wilt toevoegen de [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) NuGet verpakt en gebruik de `Microsoft.Identity.Client` naamruimte

### <a name="scopes-not-resources"></a>Resources niet bereiken

ADAL.NET verkrijgt tokens voor *resources*, maar MSAL.NET verkrijgt tokens voor *scopes*. Een aantal MSAL.NET AcquireToken onderdrukkingen vereisen een parameter met de naam van de bereiken (`IEnumerable<string> scopes`). Deze parameter is een eenvoudige lijst met tekenreeksen die de gewenste machtigingen en bronnen die worden aangevraagd declareren. Bekende bereiken zijn de [van Microsoft Graph-scopes](/graph/permissions-reference).

Het is ook mogelijk in MSAL.NET voor toegang tot v1.0 resources. Zie voor meer informatie [bereiken voor een toepassing v1.0](#scopes-for-a-web-api-accepting-v10-tokens). 

### <a name="core-classes"></a>Core-klassen

- Maakt gebruik van ADAL.NET [AuthenticationContext](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD) als de weergave van de verbinding met de Security Token Service (STS) of autorisatie-server via een instantie. Daarentegen MSAL.NET is ontworpen rond [clienttoepassingen](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications). Het biedt twee afzonderlijke klassen: `PublicClientApplication` en `ConfidentialClientApplication`

- Verkrijgen van Tokens: ADAL.NET en MSAL.NET hebben de dezelfde verificatie-aanroepen (`AcquireTokenAsync` en `AcquireTokenSilentAsync` voor ADAL.NET, en `AqquireTokenInteractive` en `AcquireTokenSilent` in MSAL.NET), maar met verschillende parameters die zijn vereist. Een verschil is het feit dat, in MSAL.NET, u niet meer hebben om door te geven de `ClientID` van uw toepassing in voor elke aanroep AcquireTokenXX. Inderdaad, de `ClientID` slechts eenmaal is ingesteld bij het bouwen van de (`IPublicClientApplication` of `IConfidentialClientApplication`).

### <a name="iaccount-not-iuser"></a>IAccount niet IUser

ADAL.NET gemanipuleerd gebruikers. Echter, een gebruiker is een mens of een softwareagent, maar kan beschikken over/eigenaar/worden die verantwoordelijk is voor een of meer accounts in het Microsoft identity-systeem (meerdere Azure AD-accounts, Azure AD B2C, persoonlijke Microsoft-accounts). 

MSAL.NET 2.x nu definieert het concept van Account (via de interface IAccount). Deze belangrijke wijziging biedt de juiste semantiek: het feit dat dezelfde gebruiker diverse accounts in verschillende Azure hebben kunt AD-mappen. MSAL.NET biedt ook betere informatie in de Gast-scenario's, zoals thuis accountinformatie wordt verstrekt.

Zie voor meer informatie over de verschillen tussen het IUser en IAccount [MSAL.NET 2.x](https://aka.ms/msal-net-2-released).

### <a name="exceptions"></a>Uitzonderingen

#### <a name="interaction-required-exceptions"></a>Tussenkomst van de vereiste uitzonderingen

MSAL.NET heeft meer expliciete uitzonderingen. Bijvoorbeeld, als verificatie op de achtergrond is mislukt in ADAL de procedure bestaat uit het ontdekken van de uitzondering en zoek naar de `user_interaction_required` foutcode:

```csharp
catch(AdalException exception)
{
 if (exception.ErrorCode == “user_interaction_required”)
 {
  try
  {“try to authenticate interactively”}}
 }
}
```

Zie voor meer informatie [het aanbevolen patroon om een token te verkrijgen](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token#recommended-pattern-to-acquire-a-token) met ADAL.NET

Met behulp van MSAL.NET, catch `MsalUiRequiredException` zoals beschreven in [AcquireTokenSilent](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-a-cached-token).

```csharp
catch(MsalUiRequiredException exception)
{
 try {“try to authenticate interactively”}
}
```

#### <a name="handling-claim-challenge-exceptions"></a>Afhandeling van claim challenge uitzonderingen

Claim in ADAL.NET, challenge uitzonderingen worden afgehandeld in de volgende manier:

- `AdalClaimChallengeException` is een uitzondering (afleiden uit `AdalServiceException`) gegenereerd door de service als een resource meer claims van de gebruiker (bijvoorbeeld verificatie met twee factoren vereist). De `Claims` lid bevat enkele JSON-fragment met de claims die naar verwachting.
- Nog steeds in ADAL.NET, de openbare clienttoepassing is ontvangen van deze uitzondering moet aanroepen de `AcquireTokenInteractive` overschrijven met een parameter claims. Met deze overschrijving van `AcquireTokenInteractive` nog niet wordt geprobeerd om de cache als het is niet nodig. De reden is dat het token in de cache niet beschikt over de juiste claims (anders een `AdalClaimChallengeException` zou niet gegenereerd). Er is daarom niet nodig om te kijken naar de cache. Houd er rekening mee dat de `ClaimChallengeException` kunnen worden ontvangen in een WebAPI OBO, doen terwijl de `AcquireTokenInteractive` moet worden aangeroepen in een openbare clienttoepassing deze Web-API aanroepen.
- Zie voor meer informatie, inclusief voorbeelden verwerking [AdalClaimChallengeException](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Exceptions-in-ADAL.NET#handling-adalclaimchallengeexception)

Claim in MSAL.NET, challenge uitzonderingen worden afgehandeld in de volgende manier:

- De `Claims` worden opgehaald de `MsalServiceException`.
- Er is een `.WithClaim(claims)` methode die kan worden toegepast op de `AcquireTokenInteractive` builder. 

### <a name="supported-grants"></a>Ondersteunde verleent

Niet alle verleent worden nog ondersteund in MSAL.NET en het v2.0-eindpunt. Hier volgt een samenvatting ADAL.NET en MSAL vergelijken. NET de verleent ondersteund.

#### <a name="public-client-applications"></a>Openbare clienttoepassingen

Hier volgen de verleent ondersteund in ADAL.NET en MSAL.NET voor Desktop- en mobiele toepassingen

Verlenen | ADAL.NET | MSAL.NET
----- |----- | -----
Interactief | [Interactieve verificatie](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-interactively---Public-client-application-flows) | [Verkrijgen van tokens interactief in MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Acquiring-tokens-interactively)
Geïntegreerde Windows-verificatie | [Geïntegreerde verificatie op Windows (Kerberos)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AcquireTokenSilentAsync-using-Integrated-authentication-on-Windows-(Kerberos)) | [Geïntegreerde Windows-verificatie](msal-authentication-flows.md#integrated-windows-authentication)
Gebruikersnaam / wachtwoord | [Verkrijgen van tokens met gebruikersnaam en wachtwoord](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-username-and-password)| [Verificatie voor gebruikersnaam wachtwoord](msal-authentication-flows.md#usernamepassword)
Stroom voor apparaatcode | [Apparaatprofiel voor apparaten die niet webbrowsers](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Device-profile-for-devices-without-web-browsers) | [De stroom van apparaat](msal-authentication-flows.md#device-code)

#### <a name="confidential-client-applications"></a>Vertrouwelijke client-toepassingen

Hier volgen de verleent ondersteund in ADAL.NET en MSAL.NET voor Web-Apps, Web-API's en daemon-toepassingen:

Type App | Verlenen | ADAL.NET | MSAL.NET
----- | ----- | ----- | -----
Web App, Web API, daemon | Clientreferenties | [Client referentie stromen in ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Client-credential-flows) | [Clientreferenties stromen in MSAL.NET](msal-authentication-flows.md#client-credentials))
Web-API | Namens | [Service serviceaanroepen-service namens de gebruiker met ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Service-to-service-calls-on-behalf-of-the-user) | [Voor andere gebruikers van MSAL.NET](msal-authentication-flows.md#on-behalf-of)
Web-app | Autorisatiecode | [Verkrijgen van tokens met autorisatiecodes op web-apps met ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Acquiring-tokens-with-authorization-codes-on-web-apps) | [Verkrijgen van tokens met autorisatiecodes op web-apps met een MSAL.NET](msal-authentication-flows.md#authorization-code)

### <a name="cache-persistence"></a>Cachepersistentie

ADAL.NET kunt u om uit te breiden de `TokenCache` klasse voor het implementeren van de functionaliteit van de gewenste persistentie op platforms zonder een veilige opslag (.NET Framework en .NET core) met behulp van de `BeforeAccess`, en `BeforeWrite` methoden. Zie voor meer informatie, [Token Cache serialisatie in ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization).

MSAL.NET maakt de tokencache een verzegelde klasse, verwijdert de mogelijkheid om deze kunt uitbreiden. De implementatie van tokencache persistentie moet daarom in de vorm van een helperklasse die met de verzegelde tokencache communiceert. Deze interactie wordt beschreven in [Token Cache serialisatie in MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/token-cache-serialization).

## <a name="signification-of-the-common-authority"></a>Significante van de algemene autoriteit

In v1.0, als u de https://login.microsoftonline.com/common -instantie, kunt u gebruikers zich aanmelden via een AAD-account (voor elke organisatie). Zie [instantie validatie in ADAL.NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/AuthenticationContext:-the-connection-to-Azure-AD#authority-validation)

Als u de https://login.microsoftonline.com/common v2.0-instantie, kunt u gebruikers zich aanmelden via een AAD-organisatie of een persoonlijk Microsoft-account (MSA). In MSAL.NET, als u wilt aanmelden beperken tot een AAD-account (hetzelfde gedrag als met ADAL.NET), moet u gebruiken https://login.microsoftonline.com/organizations. Zie voor meer informatie, de `authority` parameter in [openbare clienttoepassing](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#publicclientapplication).

## <a name="v10-and-v20-tokens"></a>V1.0 en v2.0-tokens

Er zijn twee versies van tokens:
- V1.0 tokens
- V2.0-tokens 

Het v1.0-eindpunt (gebruikt door ADAL) verzendt alleen v1.0 tokens.

Echter, verzendt het v2.0-eindpunt (gebruikt door MSAL) de versie van het token dat de Web-API accepteert. Een eigenschap van het manifest van de toepassing van de Web-API kan ontwikkelaars kiezen welke versie van het token wordt geaccepteerd. Zie `accessTokenAcceptedVersion` in de [toepassingsmanifest](reference-app-manifest.md) verwijzen naar documentatie.

Zie voor meer informatie over v1.0 en v2.0-tokens, [toegangstokens voor Azure Active Directory](access-tokens.md)

## <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>Bereiken voor een Web-API v1.0 tokens accepteren

OAuth2-machtigingen zijn machtigingsbereiken die een v1.0-web-API (resource)-toepassing beschikbaar voor clienttoepassingen maakt. Dit bereik aan machtigingen kunnen tijdens toestemming worden verleend aan clienttoepassingen. Zie de sectie over oauth2Permissions in [Azure Active Directory-toepassingsmanifest](active-directory-application-manifest.md).

### <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot specifieke OAuth2-machtigingen van een toepassing v1.0

Als u wilt verkrijgen van tokens voor specifieke scopes van een toepassing v1.0 (bijvoorbeeld de AAD graph, die is https://graph.windows.net), moet u maken `scopes` door het samenvoegen van een gewenste resource-id met een gewenste OAuth2-machtiging voor deze resource.

Bijvoorbeeld, voor toegang tot de naam van de gebruiker een v1.0 Web-API-App-ID-URI is `ResourceId`, u wilt gebruiken:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

Als u wilt lezen en schrijven met MSAL.NET Azure Active Directory met behulp van de AAD graph API (https://graph.windows.net/), maakt u een lijst met bereiken, zoals in het volgende codefragment:

```csharp
ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + “Directory.Read”, ResourceID + “Directory.Write”}
```

#### <a name="warning-should-you-have-one-or-two-slashes-in-the-scope-corresponding-to-a-v10-web-api"></a>Waarschuwing: Hebt u één of twee slashes in de scope die overeenkomt met een v1.0 Web-API

Als u wilt schrijven van het bereik dat overeenkomt met de Azure Resource Manager-API (https://management.core.windows.net/), moet u het volgende bereik (Houd er rekening mee de twee slashes) aanvragen 

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

Dit is omdat de Resource Manager-API een schuine streep in de doelgroep-claim verwacht (`aud`), en vervolgens is er een slash voor het scheiden van de naam van de API van het bereik.

De logica die wordt gebruikt door Azure AD is het volgende:
- Voor het eindpunt van de ADAL (v1.0) met een v1.0 access token (de enige mogelijke), aud resource =
- Voor MSAL (v2.0-eindpunt), waarin wordt gevraagd een toegangstoken voor een resource accepteren v2.0-tokens, aud = resource. Toepassings-id
- Azure AD wordt de gewenste doelgroep van het aangevraagde bereik voor MSAL (v2.0-eindpunt) waarin wordt gevraagd een toegangstoken voor een resource een toegangstoken v1.0 (dit is de bovenstaande aanvraag) accepteren, geparseerd door te nemen alles vóór de laatste slash en als de resource-id gebruiken. Dus als https://database.windows.net wordt verwacht dat een een publiek van 'https://database.windows.net/', moet u een bereik van aanvragen https://database.windows.net//.default. Zie ook geven #[747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Bron-url afsluitende schuine streep wordt weggelaten, waardoor sql auth-fout #747


### <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>Bereiken voor het aanvragen van toegang tot de machtigingen van een toepassing v1.0

Bijvoorbeeld, als u een token verkrijgen voor de statische bereiken van een toepassing v1.0 wilt, zou een moet gebruiken

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="scopes-to-request-in-the-case-of-client-credential-flow--daemon-app"></a>Bereiken om aan te vragen in het geval van een client referentie-gegevensstroom / daemon-app

In het geval van een client referentie-gegevensstroom, het bereik om door te geven ook worden `/.default`. Hiermee wordt aangegeven met Azure AD: "alle app-niveau machtigingen die de beheerder heeft ingestemd met de in de registratie van de toepassing.

## <a name="adal-to-msal-migration"></a>ADAL MSAL migreren

In v2 ADAL.NET. X-en vernieuwen van de tokens beschikbaar zodat u kunt gemaakt zijn het ontwikkelen van oplossingen om het gebruik van deze tokens door deze in cache en het gebruik van de `AcquireTokenByRefreshToken` methoden die door ADAL 2.x. Sommige van deze oplossingen zijn gebruikt in scenario's zoals:
* Langlopende services die onder andere dashboards vernieuwen namens de gebruikers, terwijl de gebruikers zijn niet meer verbonden acties uitvoeren. 
* WebFarm-scenario's voor het inschakelen van de client de RT brengen met de webservice (caching wordt uitgevoerd clientzijde, versleutelde cookie en niet serverzijde)

Dit is niet het geval is bij MSAL.NET, maar als er geen langer aanbevolen gebruik vernieuwen van tokens op deze manier uit veiligheidsoverwegingen. Dit wordt soms moeilijk te migreren naar MSAL 3.x als de API biedt een manier om door te geven in de eerder verkregen vernieuwingstokens. 

Gelukkig MSAL.NET beschikt nu over een API waarmee u voor het migreren van uw vorige vernieuwingstokens in de `IConfidentialClientApplication` 

```CSharp
/// <summary>
/// Acquires an access token from an existing refresh token and stores it and the refresh token into 
/// the application user token cache, where it will be available for further AcquireTokenSilent calls.
/// This method can be used in migration to MSAL from ADAL v2 and in various integration 
/// scenarios where you have a RefreshToken available. 
/// (see https://aka.ms/msal-net-migration-adal2-msal2)
/// </summary>
/// <param name="scopes">Scope to request from the token endpoint. 
/// Setting this to null or empty will request an access token, refresh token and ID token with default scopes</param>
/// <param name="refreshToken">The refresh token from ADAL 2.x</param>
IByRefreshToken.AcquireTokenByRefreshToken(IEnumerable<string> scopes, string refreshToken);
```
 
Met deze methode kunt u de eerder gebruikte vernieuwingstoken samen met eventuele bereiken (resources) u wilt opgeven. Het vernieuwingstoken worden uitgewisseld voor een nieuwe resourcegroep en in de cache opgeslagen in uw toepassing.  

Als deze methode is bedoeld voor scenario's die niet vaak worden, is het niet gemakkelijk toegankelijk zijn met de `IConfidentialClientApplication` zonder eerst te casten `IByRefreshToken`.

Dit codefragment laat zien wat code migratie in een vertrouwelijke client-toepassing. `GetCachedRefreshTokenForSignedInUser` Haal het vernieuwingstoken dat is opgeslagen in een bepaalde opslag met een eerdere versie van de toepassing die gebruikmaken van ADAL 2.x gebruikt. `GetTokenCacheForSignedInUser` een cache voor de aangemelde gebruiker gedeserialiseerd (zoals vertrouwelijke clienttoepassingen één cache per gebruiker moet).

```csharp
TokenCache userCache = GetTokenCacheForSignedInUser();
string rt = GetCachedRefreshTokenForSignedInUser();

IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(clientId)
 .WithAuthority(Authority)
 .WithRedirectUri(RedirectUri)
 .WithClientSecret(ClientSecret)
 .Build();
IByRefreshToken appRt = app as IByRefreshToken;
         
AuthenticationResult result = await appRt.AcquireTokenByRefreshToken(null, rt)
                                         .ExecuteAsync()
                                         .ConfigureAwait(false);
```

U ziet een toegangstoken en een ID-token in uw AuthenticationResult geretourneerd terwijl uw nieuwe vernieuwingstoken is opgeslagen in de cache.

U kunt deze methode ook gebruiken voor verschillende scenario's voor integratie van waar u een vernieuwingstoken beschikbaar hebt.

## <a name="next-steps"></a>Volgende stappen

U vindt meer informatie over de bereiken in [bereiken, machtigingen en toestemming in het eindpunt van de Microsoft identity-platform](v2-permissions-and-consent.md)