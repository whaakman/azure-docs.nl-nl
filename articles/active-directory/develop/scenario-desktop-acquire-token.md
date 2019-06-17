---
title: Desktop-app die aanroepen van web-API's (ophalen van een token voor de app) - Microsoft identity-platform
description: Informatie over het bouwen van een bureaublad-app die web-API's aanroept (ophalen van een token voor de app |)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecf5b874345a94e8fd3d3a0783f8e48c7484377d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111254"
---
# <a name="desktop-app-that-calls-web-apis---acquire-a-token"></a>Desktop-app die web-API's - roept een token verkrijgen

Als u hebt gemaakt dat u `IPublicClientApplication`, gebruikt u deze voor het verkrijgen van een token dat u vervolgens gebruiken voor het aanroepen van een web-API.

## <a name="recommended-pattern"></a>Aanbevolen patroon

De web-API wordt gedefinieerd door de `scopes`. Ongeacht de ervaring die u in uw toepassing opgeeft is voor het patroon dat u wilt gebruiken:

- Systematisch bij het ophalen van een token uit de cache token door aan te roepen `AcquireTokenSilent`
- Als deze aanroep is mislukt, gebruikt u de `AcquireToken` stroom die u wilt gebruiken (Hier wordt vertegenwoordigd door `AcquireTokenXX`)

```CSharp
AuthenticationResult result;
var accounts = await app.GetAccountsAsync();
IAccount account = ChooseAccount(accounts); // for instance accounts.FirstOrDefault
                                            // if the app manages is at most one account  
try
{
 result = await app.AcquireTokenSilent(scopes, account)
                   .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
  result = await app.AcquireTokenXX(scopes, account)
                    .WithOptionalParameterXXX(parameter)
                    .ExecuteAsync();
}
```

Hier is nu de details van de verschillende manieren om te verkrijgen van tokens in een bureaubladtoepassing

## <a name="acquiring-a-token-interactively"></a>Ophalen van een token interactief

Het volgende voorbeeld ziet minimale code voor het interactief ophalen van een token voor het lezen van het gebruikersprofiel met Microsoft Graph.

```CSharp
string[] scopes = new string["user.read"];
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

### <a name="mandatory-parameters"></a>Verplichte parameters

`AcquireTokenInteractive` heeft slechts één verplichte parameter ``scopes``, bevat een opsomming van tekenreeksen die definiëren van de bereiken waarvoor een token vereist is. Als het token voor de Microsoft Graph is, kunnen de vereiste bereiken in api-verwijzing van de Microsoft graph API worden gevonden in de sectie met de naam 'Machtigingen'. Bijvoorbeeld [lijst van de gebruiker contactpersonen](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts), het bereik 'User.Read","Contacts.Read"moet worden gebruikt. Zie ook [referentie voor Microsoft Graph-machtigingen](https://developer.microsoft.com/graph/docs/concepts/permissions_reference).

Op Android, moet u ook opgeven de bovenliggende activiteit (met behulp van `.WithParentActivityOrWindow`, Zie hieronder) zodat de token terug opgehaald voor de betreffende bovenliggende activiteit na de interactie. Als u dit niet opgeeft, wordt een uitzondering gegenereerd bij het aanroepen van `.ExecuteAsync()`.

### <a name="specific-optional-parameters"></a>Specifieke optionele parameters

#### <a name="withparentactivityorwindow"></a>WithParentActivityOrWindow

Interactief is, UI belangrijk. `AcquireTokenInteractive` heeft een specifieke optionele parameter om op te geven voor platforms ondersteunen het inschakelen van de bovenliggende gebruikersinterface. Wanneer gebruikt in een bureaubladtoepassing `.WithParentActivityOrWindow` heeft een ander type afhankelijk van het platform:

```CSharp
// net45
WithParentActivityOrWindow(IntPtr windowPtr)
WithParentActivityOrWindow(IWin32Window window)

// Mac
WithParentActivityOrWindow(NSWindow window)

// .Net Standard (this will be on all platforms at runtime, but only on NetStandard at build time)
WithParentActivityOrWindow(object parent).
```

Opmerking:

- Op .NET Standard, de verwachte `object` is een `Activity` op Android, een `UIViewController` in iOS, een `NSWindow` op de MAC, en een `IWin32Window` of `IntPr` op Windows.
- Op Windows, moet u aanroepen `AcquireTokenInteractive` vanuit de gebruikersinterface-thread, zodat de ingesloten browser de juiste context van de UI-synchronisatie haalt.  Niet aanroepen vanuit de UI-thread, is het mogelijk dat berichten niet goed pomp en/of impasse van scenario's met de gebruikersinterface. Één manier om MSAL aanroepen vanuit de UI-thread als u op de UI-thread niet al is het gebruik van de `Dispatcher` op WPF.
- Als u van WPF-, gebruikmaakt om op te halen van een venster van een besturingselement WPF, kunt u `WindowInteropHelper.Handle` klasse. De aanroep is vervolgens van een besturingselement WPF (`this`):
  
  ```CSharp
  result = await app.AcquireTokenInteractive(scopes)
                    .WithParentActivityOrWindow(new WindowInteropHelper(this).Handle)
                    .ExecuteAsync();
  ```

#### <a name="withprompt"></a>WithPrompt

`WithPrompt()` wordt gebruikt voor het beheren van de interactiviteit van de gebruiker door een Prompt op te geven

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

De klasse definieert de volgende constanten toe:

- ``SelectAccount``: de STS om weer te geven van het dialoogvenster voor het selecteren van account met accounts waarvoor de gebruiker een sessie heeft wordt afgedwongen. Deze optie is handig wanneer ontwikkelaars van toepassingen zodat de gebruiker de keuze uit verschillende identiteiten wilt maken. Deze optie stations MSAL voor het verzenden van ``prompt=select_account`` met de id-provider. Deze optie is de standaardinstelling en het doet van goede taak van het leveren van de best mogelijke ervaring op basis van de beschikbare gegevens (account, aanwezigheid van een sessie voor de gebruiker, enzovoort. ...). Deze niet wijzigt, tenzij er een goede reden om dit te doen.
- ``Consent``: kan de toepassing ontwikkelaar om af te dwingen van de gebruiker om toestemming gevraagd, zelfs als deze is toegestaan voordat. In dit geval MSAL verzendt `prompt=consent` met de id-provider. Deze optie kan worden gebruikt in sommige beveiligingstoepassingen zeer sterk gericht op waar de governance organisatie vereist dat de gebruiker wordt weergegeven het dialoogvenster telkens wanneer die de toepassing wordt gebruikt.
- ``ForceLogin``: Hiermee kunt u de ontwikkelaar van de toepassing aan de gebruiker gevraagd om referenties door de service, zelfs als deze Gebruikersprompt wouldn't nodig hebt. Deze optie is handig als ophalen van een token is mislukt, zodat de gebruiker opnieuw-aanmelden-in. In dit geval MSAL verzendt `prompt=login` met de id-provider. We hebben ook gezien het wordt gebruikt in sommige beveiligingstoepassingen zeer sterk gericht op waar de governance organisatie vereist dat de gebruiker relogs in telkens wanneer ze toegang krijgen specifieke onderdelen van een toepassing tot.
- ``Never`` (voor .NET 4.5 en WinRT alleen) de gebruiker geen melding, maar in plaats daarvan wordt geprobeerd de cookie die zijn opgeslagen in de verborgen ingesloten webweergave gebruiken (Zie hieronder: Webweergaven in MSAL.NET). Met deze optie kan mislukken, en in dat geval `AcquireTokenInteractive` genereert een uitzondering om aan te melden dat een UI-interactie nodig is en u moet gebruiken een andere `Prompt` parameter.
- ``NoPrompt``: Wordt niet elke prompt niet verzenden naar de id-provider. Deze optie is alleen nuttig voor het beleid bewerken Azure AD B2C (Zie [B2C specificaties](https://aka.ms/msal-net-b2c-specificities)).

#### <a name="withextrascopetoconsent"></a>WithExtraScopeToConsent

Deze optie wordt gebruikt in een geavanceerd scenario waarin u wilt dat de gebruiker vooraf toestemming geven voor verschillende resources vooraf (en niet wilt gebruiken de incrementele toestemming, die normaal gesproken wordt gebruikt voor MSAL.NET / het v2.0 Microsoft identity-platform). Zie voor meer informatie [instructies: de gebruiker toestemming vooraf voor verschillende resources](scenario-desktop-production.md#how-to-have--the-user-consent-upfront-for-several-resources).

```CSharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

#### <a name="withcustomwebui"></a>WithCustomWebUi

##### <a name="withcustomwebui-is-an-extensibility-point"></a>WithCustomWebUi is een uitbreidingspunt

`WithCustomWebUi` is een uitbreidingspunt waarmee dat u uw eigen gebruikersinterface in openbare clienttoepassingen opgeven en zodat de gebruiker via het /Authorize-eindpunt van de id-provider en laat ze zich aanmelden en toestemming geven. MSAL.NET kunt, vervolgens wisselen de verificatiecode op te geven en een token verkrijgen. Het wordt bijvoorbeeld in Visual Studio gebruikt dat elektronen toepassingen (bijvoorbeeld VS Feedback) bieden de tussenkomst van de web, maar laat deze MSAL.NET voor de meeste van het werk. U kunt deze ook gebruiken als u wilt bieden UI-automatisering. In de openbare clienttoepassingen, MSAL.NET maakt gebruik van de standaard PKCE ([RFC 7636 - bewijs-sleutel voor de Exchange-Code met OAuth openbare Clients](https://tools.ietf.org/html/rfc7636)) om ervoor te zorgen dat de beveiliging is voldaan: Alleen MSAL.NET kunt de code worden ingewisseld.

  ```CSharp
  using Microsoft.Identity.Client.Extensions;
  ```

##### <a name="how-to-use-withcustomwebui"></a>Het gebruik van WithCustomWebUi

Als u wilt gebruiken `.WithCustomWebUI`, moet u:
  
  1. Implementeer de `ICustomWebUi` interface (Zie [hier](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/src/Microsoft.Identity.Client/Extensibility/ICustomWebUI.cs#L32-L70). U moet in feite een methode implementeren `AcquireAuthorizationCodeAsync` back-de URL waarmee de id-provider wilt accepteren van de URL van de autorisatie-code (berekend door MSAL.NET), zodat de gebruiker via de interactie met de id-provider en vervolgens te retourneren zijn uw implementatie terug (inclusief de autorisatiecode) genoemd. Als u problemen hebt, uw implementatie moet genereren een `MsalExtensionException` uitzondering goed samenwerken met MSAL.
  2. In uw `AcquireTokenInteractive` -aanroep kunt u `.WithCustomUI()` modifier doorgeven van het exemplaar van uw aangepaste web-UI

     ```CSharp
     result = await app.AcquireTokenInteractive(scopes)
                       .WithCustomWebUi(yourCustomWebUI)
                       .ExecuteAsync();
     ```

##### <a name="examples-of-implementation-of-icustomwebui-in-test-automation---seleniumwebui"></a>Voorbeelden van de uitvoering van ICustomWebUi in test-automation - SeleniumWebUI

Het team van MSAL.NET hebben onze UI-tests als u wilt gebruikmaken van dit mechanisme voor uitbreidbaarheid herschreven. Als u geïnteresseerd bent u eens kunt hebben de [SeleniumWebUI](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/tests/Microsoft.Identity.Test.Integration/Infrastructure/SeleniumWebUI.cs#L15-L160) klasse in de broncode MSAL.NET

#### <a name="other-optional-parameters"></a>Overige optionele parameters

Meer informatie over alle andere optionele parameters voor `AcquireTokenInteractive` uit de naslagdocumentatie voor [AcquireTokenInteractiveParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="integrated-windows-authentication"></a>Geïntegreerde Windows-verificatie

Als u wilt een domein-gebruiker aanmelden bij een domein of Azure AD machine toegevoegd, u wilt gebruiken:

```csharp
AcquireTokenByIntegratedWindowsAuth(IEnumerable<string> scopes)
```

### <a name="constraints"></a>Beperkingen

- AcquireTokenByIntegratedWindowsAuth (IWA) kan alleen worden gebruikt voor **federatieve** alleen gebruikers die is, wordt voor gebruikers in een Active Directory gemaakt en ondersteund door Azure Active Directory. Gebruikers die rechtstreeks in AAD, zonder back-ups van AD - gemaakt **beheerd** gebruikers - deze authenticatiestroom niet gebruiken. Deze beperking niet van invloed op de stroom van de gebruikersnaam en wachtwoord.
- IWA is bedoeld voor apps die zijn geschreven voor .NET Framework-, .NET Core- en UWP-platforms
- IWA komt niet overslaan voor MFA (Multi-factor authentication). Als MFA is geconfigureerd, kan IWA mislukken als een MFA-controle vereist is, omdat voor MFA tussenkomst van de gebruiker is vereist.
  > [!NOTE]
  > Dit is een lastige. IWA is niet-interactieve, maar 2FA interactie met de gebruiker vereist. U doet geen controle over wanneer de id-provider aanvraagt 2FA moet worden uitgevoerd, de tenant-beheerder heeft. Uit onze observaties is 2FA is vereist wanneer u zich vanaf een ander land als niet is verbonden via VPN met een bedrijfsnetwerk, en soms zelfs aanmeldt wanneer die zijn verbonden via VPN. Niet had verwacht een deterministische set regels, Azure Active Directory maakt gebruik van AI voor steeds meer als 2FA vereist is. U moet terugval naar een gebruiker krijgt (interactieve verificatie of het apparaat codestroom) als IWA is mislukt.

- De instantie die wordt doorgegeven in de `PublicClientApplicationBuilder` moet zijn:
  - tenant-ed (van het formulier `https://login.microsoftonline.com/{tenant}/` waar `tenant` is de guid die de tenant-ID of een domein dat is gekoppeld aan de tenant vertegenwoordigt.
  - voor een werk- en schoolaccounts (`https://login.microsoftonline.com/organizations/`)

  > Persoonlijke Microsoft-accounts worden niet ondersteund (u kunt geen met/Common of /consumers tenants)

- Omdat geïntegreerde Windows-verificatie een stroom op de achtergrond is:
  - de gebruiker van uw toepassing moet eerder zijn toegestaan dat de toepassing wordt gebruikt
  - of de tenant-beheerder moet eerder hebben ingestemd met alle gebruikers in de tenant om het gebruik van de toepassing.
  - Met andere woorden:
    - kunt u als ontwikkelaar hebt gedrukt de **verlenen** knop op de Azure-portal voor uzelf.
    - of een tenantbeheerder heeft gedrukt de **Grant/revoke-beheerderstoestemming voor {tenantdomein}** knop in de **API-machtigingen** tabblad van de registratie van de toepassing (Zie [machtigingen toevoegen toegang tot web-API's](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-permissions-to-access-web-apis))
    - of u een manier om gebruikers toestemming geven voor de toepassing hebt opgegeven (Zie [aanvragen van toestemming van de individuele gebruiker](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-individual-user-consent))
    - of u een manier voor de tenantbeheerder toestemming voor de toepassing hebt opgegeven (Zie [beheerderstoestemming](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant))

- Deze stroom is ingeschakeld voor .net-desktop-, .net core- en Windows Universal (UWP) Apps. Op .NET core is alleen de overbelasting houdend met de gebruikersnaam beschikbaar, zoals het .NET Core-platform geen vragen de gebruikersnaam voor het besturingssysteem stellen.
  
Zie voor meer informatie over toestemming [v2.0 machtigingen en toestemming](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)

### <a name="how-to-use-it"></a>Het gebruik ervan

Normaal gesproken moet u slechts één parameter (`scopes`). Echter afhankelijk van de manier waarop de Windows-beheerder is het beleid, het is mogelijk dat toepassingen op uw windows-computer zijn niet toegestaan om te controleren of de gebruiker is aangemeld. In dat geval een tweede verificatiemethode gebruiken `.WithUsername()` en geef de gebruikersnaam van de gebruiker is aangemeld als een indeling voor UPN - `joe@contoso.com`.

Het volgende voorbeeld geeft de meest recente aanvraag, met uitleg van het type uitzonderingen die u kunt ophalen en de bijbehorende oplossingen

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app = PublicClientApplicationBuilder
      .Create(clientId)
      .WithAuthority(authority)
      .Build();

 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
      .ExecuteAsync();
 }
 else
 {
  try
  {
   result = await app.AcquireTokenByIntegratedWindowsAuth(scopes)
      .ExecuteAsync(CancellationToken.None);
  }
  catch (MsalUiRequiredException ex)
  {
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'.Send an interactive authorization request for this user and resource.

   // you need to get user consent first. This can be done, if you are not using .NET Core (which does not have any Web UI)
   // by doing (once only) an AcquireToken interactive.

   // If you are using .NET core or don't want to do an AcquireTokenInteractive, you might want to suggest the user to navigate
   // to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read

   // AADSTS50079: The user is required to use multi-factor authentication.
   // There is no mitigation - if MFA is configured for your tenant and AAD decides to enforce it,
   // you need to fallback to an interactive flows such as AcquireTokenInteractive or AcquireTokenByDeviceCode
   }
   catch (MsalServiceException ex)
   {
    // Kind of errors you could have (in ex.Message)

    // MsalServiceException: AADSTS90010: The grant type is not supported over the /common or /consumers endpoints. Please use the /organizations or tenant-specific endpoint.
    // you used common.
    // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted or otherwise organizations

    // MsalServiceException: AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
    // Explanation: this can happen if your application was not registered as a public client application in Azure AD
    // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   }
   catch (MsalClientException ex)
   {
      // Error Code: unknown_user Message: Could not identify logged in user
      // Explanation: the library was unable to query the current Windows logged-in user or this user is not AD or AAD
      // joined (work-place joined users are not supported).

      // Mitigation 1: on UWP, check that the application has the following capabilities: Enterprise Authentication,
      // Private Networks (Client and Server), User Account Information

      // Mitigation 2: Implement your own logic to fetch the username (e.g. john@contoso.com) and use the
      // AcquireTokenByIntegratedWindowsAuth form that takes in the username

      // Error Code: integrated_windows_auth_not_supported_managed_user
      // Explanation: This method relies on an a protocol exposed by Active Directory (AD). If a user was created in Azure
      // Active Directory without AD backing ("managed" user), this method will fail. Users created in AD and backed by
      // AAD ("federated" users) can benefit from this non-interactive method of authentication.
      // Mitigation: Use interactive authentication
   }
 }


 Console.WriteLine(result.Account.Username);
}
```

Zie voor een lijst van mogelijke parameters op AcquireTokenByIntegratedWindowsAuthentication, [AcquireTokenByIntegratedWindowsAuthParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyintegratedwindowsauthparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="username--password"></a>Gebruikersnaam / wachtwoord

Hiervoor de gebruikersnaam en wachtwoord kunt u ook een token verkrijgen. Deze stroom is beperkt en wordt niet aanbevolen, maar er zijn nog steeds gebruiksvoorbeelden wanneer dit nodig is.

### <a name="this-flow-isnt-recommended"></a>Deze stroom wordt niet aanbevolen

Deze stroom is **niet aanbevolen** omdat uw toepassing een gebruiker wordt gevraagd hun wachtwoord niet beveiligd. Zie voor meer informatie over dit probleem [in dit artikel](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). De gewenste stroom voor het verkrijgen van een token op de achtergrond op Windows-domein-machines is [geïntegreerde Windows-verificatie](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Integrated-Windows-Authentication). Anders kunt u ook gebruiken [codestroom van apparaat](https://aka.ms/msal-net-device-code-flow)

> Hoewel dit handig in sommige gevallen (DevOps-scenario's), is als u wilt gebruiken van gebruikersnaam en wachtwoord in interactieve scenario's waarin u uw onw gebruikersinterface hebt opgeven, moet u echt Denk na over hoe u afstappen van het. Met behulp van de gebruikersnaam en wachtwoord u worden geven tot een aantal dingen:

> - Core-tenants van moderne identiteit: wachtwoord opgehaald gevangen, opnieuw afgespeeld. Omdat we dit concept van een share-geheim die kan worden onderschept hebben.
> Dit is niet compatibel met de configuratie.
> - gebruikers hoeven te doen MFA niet mogelijk om aan te melden (wat er geen tussenkomst van de is)
> - Gebruikers niet mogelijk om u te eenmalige aanmelding

### <a name="constraints"></a>Beperkingen

Er gelden ook de volgende beperkingen:

- De gebruikersnaam en wachtwoord-stroom is niet compatibel met voorwaardelijke toegang en verificatie met meerdere factoren: Als gevolg hiervan, als uw app wordt uitgevoerd in een Azure AD-tenant waar de tenantbeheerder is multi-factor authentication wordt vereist, niet u deze stroom gebruiken. Veel organisaties doen dat.
- Dit werkt alleen voor werk en schoolaccounts (niet MSA)
- De stroom is beschikbaar op het bureaublad van .net en .net core, maar niet voor UWP

### <a name="b2c-specifics"></a>B2C-specificaties

[Meer informatie over het gebruik van ROPC met B2C](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics#resource-owner-password-credentials-ropc-with-b2c).

### <a name="how-to-use-it"></a>Hoe kan ik gebruiken?

`IPublicClientApplication`bevat de methode `AcquireTokenByUsernamePassword`

Het volgende voorbeeld geeft een vereenvoudigde aanvraag

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuild.Create(clientId)
       .WithAuthority(authority)
       .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAsync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password
    securePassword.AppendChar(c);  // keystroke by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                     securePassword)
                      .ExecuteAsync();
  }
  catch(MsalException)
  {
   // See details below
  }
 }
 Console.WriteLine(result.Account.Username);
}
```

Het volgende voorbeeld geeft de meest recente aanvraag, met uitleg van het type uitzonderingen die u kunt ophalen en de bijbehorende oplossingen

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuild.Create(clientId)
                                   .WithAuthority(authority)
                                   .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password keystroke
    securePassword.AppendChar(c);  // by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                    securePassword)
                    .ExecuteAsync();
  }
  catch (MsalUiRequiredException ex) when (ex.Message.Contains("AADSTS65001"))
  {
   // Here are the kind of error messages you could have, and possible mitigations

   // ------------------------------------------------------------------------
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'. Send an interactive authorization request for this user and resource.

   // Mitigation: you need to get user consent first. This can be done either statically (through the portal),
   /// or dynamically (but this requires an interaction with Azure AD, which is not possible with
   // the username/password flow)
   // Statically: in the portal by doing the following in the "API permissions" tab of the application registration:
   // 1. Click "Add a permission" and add all the delegated permissions corresponding to the scopes you want (for instance
   // User.Read and User.ReadBasic.All)
   // 2. Click "Grant/revoke admin consent for <tenant>") and click "yes".
   // Dynamically, if you are not using .NET Core (which does not have any Web UI) by
   // calling (once only) AcquireTokenInteractive.
   // remember that Username/password is for public client applications that is desktop/mobile applications.
   // If you are using .NET core or don't want to call AcquireTokenInteractive, you might want to:
   // - use device code flow (See https://aka.ms/msal-net-device-code-flow)
   // - or suggest the user to navigate to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ErrorCode: invalid_grant
   // SubError: basic_action
   // MsalUiRequiredException: AADSTS50079: The user is required to use multi-factor authentication.
   // The tenant admin for your organization has chosen to oblige users to perform multi-factor authentication.
   // Mitigation: none for this flow
   // Your application cannot use the Username/Password grant.
   // Like in the previous case, you might want to use an interactive flow (AcquireTokenInteractive()),
   // or Device Code Flow instead.
   // Note this is one of the reason why using username/password is not recommended;
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // Message = "AADSTS70002: Error validating credentials.
   // AADSTS50126: Invalid username or password
   // In the case of a managed user (user from an Azure AD tenant opposed to a
   // federated user, which would be owned
   // in another IdP through ADFS), the user has entered the wrong password
   // Mitigation: ask the user to re-enter the password
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // MsalServiceException: ADSTS50034: To sign into this application the account must be added to
   // the {domainName} directory.
   // or The user account does not exist in the {domainName} directory. To sign into this application,
   // the account must be added to the directory.
   // The user was not found in the directory
   // Explanation: wrong username
   // Mitigation: ask the user to re-enter the username.
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_request")
  {
   // ------------------------------------------------------------------------
   // AADSTS90010: The grant type is not supported over the /common or /consumers endpoints.
   // Please use the /organizations or tenant-specific endpoint.
   // you used common.
   // Mitigation: as explained in the message from Azure AD, the authority you use in the application needs
   // to be tenanted or otherwise "organizations". change the
   // "Tenant": property in the appsettings.json to be a GUID (tenant Id), or domain name (contoso.com)
   // if such a domain is registered with your tenant
   // or "organizations", if you want this application to sign-in users in any Work and School accounts.
   // ------------------------------------------------------------------------

  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "unauthorized_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS700016: Application with identifier '{clientId}' was not found in the directory '{domain}'.
   // This can happen if the application has not been installed by the administrator of the tenant or consented
   // to by any user in the tenant.
   // You may have sent your authentication request to the wrong tenant
   // Cause: The clientId in the appsettings.json might be wrong
   // Mitigation: check the clientId and the app registration
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
   // Explanation: this can happen if your application was not registered as a public client application in Azure AD
   // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException)
  {
   throw;
  }

  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user_type")
  {
   // Message = "Unsupported User Type 'Unknown'. Please see https://aka.ms/msal-net-up"
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "user_realm_discovery_failed")
  {
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user. That's for instance the case
   // if you use a phone number
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user")
  {
   // the username was probably empty
   // ex.Message = "Could not identify the user logged into the OS. See https://aka.ms/msal-net-iwa for details."
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "parsing_wstrust_response_failed")
  {
   // ------------------------------------------------------------------------
   // In the case of a Federated user (that is owned by a federated IdP, as opposed to a managed user owned in an Azure AD tenant)
   // ID3242: The security token could not be authenticated or authorized.
   // The user does not exist or has entered the wrong password
   // ------------------------------------------------------------------------
  }
 }

 Console.WriteLine(result.Account.Username);
}
```

Voor meer informatie over de parameters die kunnen worden toegepast op `AcquireTokenByUsernamePassword`, Zie [AcquireTokenByUsernamePasswordParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyusernamepasswordparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="command-line-tool-without-web-browser"></a>Opdrachtregel-hulpprogramma (zonder webbrowser)

### <a name="device-code-flow-why-and-how"></a>Apparaatcode flow waarom? en hoe u?

Als u bij het schrijven van een opdrachtregelprogramma (die geen webbesturingselementen), en niet of niet wilt gebruiken van de vorige stromen, moet u gebruiken `AcquireTokenWithDeviceCode`.

Interactieve verificatie met Azure AD is een webbrowser vereist (Zie voor meer informatie [gebruik van webbrowsers](https://aka.ms/msal-net-uses-web-browser)). Echter om gebruikers te verifiëren op apparaten of besturingssystemen die geen een webbrowser, kan de stroom van apparaat de gebruiker gebruikt een ander apparaat (bijvoorbeeld een andere computer of een mobiele telefoon) aan te melden interactief. Met behulp van de stroom van het apparaat, krijgt de toepassing tokens via een proces in twee stappen is speciaal ontworpen voor deze apparaten/OS. Voorbeelden van dergelijke toepassingen worden toepassingen die worden uitgevoerd op iOT, of met opdrachtregelprogramma's (CLI). Het idee is dat:

1. Wanneer de verificatie van de gebruiker is vereist, de app biedt een code en vraag de gebruiker om een ander apparaat (zoals een smartphone internetverbinding) gebruiken om te navigeren naar een URL (bijvoorbeeld `https://microsoft.com/devicelogin`), waarbij de gebruiker wordt gevraagd de code op te geven. Dat klaar, de webpagina de gebruiker via een normale verificatie-ervaring leidt, met inbegrip van toestemming wordt gevraagd en meervoudige verificatie, indien nodig.

2. Bij een geslaagde verificatie, de opdrachtregel-app ontvangt de vereiste tokens via een back-kanaal en gebruikt voor het uitvoeren van de web-API-aanroepen nodig is.

### <a name="code"></a>Code

`IPublicClientApplication`een methode met de naam bevat `AcquireTokenWithDeviceCode`

```CSharp
 AcquireTokenWithDeviceCode(IEnumerable<string> scopes,
                            Func<DeviceCodeResult, Task> deviceCodeResultCallback)
```

Deze methode wordt gebruikt als parameters:

- De `scopes` om aan te vragen van een toegangstoken voor
- Een callbackfunctie die ontvangt de `DeviceCodeResult`

  ![image](https://user-images.githubusercontent.com/13203188/56024968-7af1b980-5d11-11e9-84c2-5be2ef306dc5.png)

De volgende voorbeeldcode bevat de meest recente aanvraag, met uitleg van het type van de uitzonderingen die u kunt ophalen en hun risicobeperking.

```CSharp
static async Task<AuthenticationResult> GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication pca = PublicClientApplicationBuilder
      .Create(clientId)
      .WithAuthority(authority)
      .Build();

 AuthenticationResult result = null;
 var accounts = await app.GetAccountsAsync();

 // All AcquireToken* methods store the tokens in the cache, so check the cache first
 try
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
       .ExecuteAsync();
 }
 catch (MsalUiRequiredException ex)
 {
  // A MsalUiRequiredException happened on AcquireTokenSilent.
  // This indicates you need to call AcquireTokenInteractive to acquire a token
  System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
 }

 try
 {
  result = await app.AcquireTokenWithDeviceCode(scopes,
      deviceCodeCallback =>
  {
       // This will print the message on the console which tells the user where to go sign-in using
       // a separate browser and the code to enter once they sign in.
       // The AcquireTokenWithDeviceCode() method will poll the server after firing this
       // device code callback to look for the successful login of the user via that browser.
       // This background polling (whose interval and timeout data is also provided as fields in the
       // deviceCodeCallback class) will occur until:
       // * The user has successfully logged in via browser and entered the proper code
       // * The timeout specified by the server for the lifetime of this code (typically ~15 minutes) has been reached
       // * The developing application calls the Cancel() method on a CancellationToken sent into the method.
       //   If this occurs, an OperationCanceledException will be thrown (see catch below for more details).
       Console.WriteLine(deviceCodeResult.Message);
       return Task.FromResult(0);
  }).ExecuteAsync();

  Console.WriteLine(result.Account.Username);
  return result;
 }
 catch (MsalServiceException ex)
 {
  // Kind of errors you could have (in ex.Message)

  // AADSTS50059: No tenant-identifying information found in either the request or implied by any provided credentials.
  // Mitigation: as explained in the message from Azure AD, the authoriy needs to be tenanted. you have probably created
  // your public client application with the following authorities:
  // https://login.microsoftonline.com/common or https://login.microsoftonline.com/organizations

  // AADSTS90133: Device Code flow is not supported under /common or /consumers endpoint.
  // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted

  // AADSTS90002: Tenant <tenantId or domain you used in the authority> not found. This may happen if there are
  // no active subscriptions for the tenant. Check with your subscription administrator.
  // Mitigation: if you have an active subscription for the tenant this might be that you have a typo in the
  // tenantId (GUID) or tenant domain name.
 }
 catch (OperationCanceledException ex)
 {
  // If you use a CancellationToken, and call the Cancel() method on it, then this may be triggered
  // to indicate that the operation was cancelled.
  // See https://docs.microsoft.com/dotnet/standard/threading/cancellation-in-managed-threads
  // for more detailed information on how C# supports cancellation in managed threads.
 }
 catch (MsalClientException ex)
 {
  // Verification code expired before contacting the server
  // This exception will occur if the user does not manage to sign-in before a time out (15 mins) and the
  // call to `AcquireTokenWithDeviceCode` is not cancelled in between
 }
}
```

## <a name="file-based-token-cache"></a>Tokencache op basis van bestanden

In MSAL.NET, wordt een token in-memory-cache standaard opgegeven.

### <a name="serialization-is-customizable-in-windows-desktop-apps-and-web-appsweb-apis"></a>Serialisatie kan worden aangepast in de Windows-bureaublad-apps en web apps/web-API 's

In het geval van .NET Framework en .NET core, als u niets extra, doet de token in-memory-cache is geldig voor de duur van de toepassing. Om te begrijpen waarom serialisatie is geen opgegeven gebruiksklaar, houd er rekening mee MSAL .NET desktop/core-toepassingen kunnen worden-console of Windows-toepassingen (die toegang tot het bestandssysteem), **, maar ook** webtoepassingen of web-API. Deze Web-apps en web-API's kunnen sommige specifieke cache-mechanismen gebruiken, zoals databases, gedistribueerde cache, redis-caches enzovoort. Als u wilt een permanente tokencache-toepassing in .NET-Desktop- of Core, moet u de serialisatie aanpassen.

Klassen en interfaces die betrokken zijn bij tokencache serialisatie zijn de volgende typen:

- ``ITokenCache``, waarmee wordt gedefinieerd gebeurtenissen abonneren op tokencache serialisatie aanvragen, evenals de methoden voor het serialiseren of deserialiseren van de cache in verschillende indelingen (ADAL v3.0, MSAL 2.x en MSAL 3.x = ADAL 5.0)
- ``TokenCacheCallback`` een retouraanroep doorgegeven aan de gebeurtenissen zodat u de serialisatie kan verwerken. ze gaat worden aangeroepen met argumenten van het type ``TokenCacheNotificationArgs``.
- ``TokenCacheNotificationArgs`` biedt alleen de ``ClientId`` van de toepassing en een verwijzing naar de gebruiker waarvoor het token beschikbaar is

  ![image](https://user-images.githubusercontent.com/13203188/56027172-d58d1480-5d15-11e9-8ada-c0292f1800b3.png)

> [!IMPORTANT]
> MSAL.NET token caches voor u gemaakt en biedt u de `IToken` bij het aanroepen van een toepassing in de cache `GetUserTokenCache` en `GetAppTokenCache` methoden. U mag worden niet zelf de interface implementeren. Uw eigen verantwoordelijkheid, wanneer u een serialisatie aangepaste tokencache implementeren is:
>
> - Reageren op `BeforeAccess` en `AfterAccess` "events". De`BeforeAccess` gemachtigde is verantwoordelijk voor het deserialiseren van de cache, terwijl de `AfterAccess` een is verantwoordelijk voor het serialiseren van de cache.
> - Onderdeel van deze gebeurtenissen opslaan of -blobs, die de gebeurtenis-argument worden doorgegeven aan de opslag die u wilt laden.

De strategieën zijn afhankelijk van verschillende als u een serialisatie-tokencache voor een openbare client-toepassing (Desktop) of een vertrouwelijke client-toepassing (web-app/web-API, daemon-app schrijft).

Sinds MSAL V2.x hebt u verschillende opties, afhankelijk van de als u wilt dat het serialiseren van de cache alleen naar de MSAL.NET-indeling (uniforme indeling cache die wordt gedeeld met MSAL, maar ook op de verschillende platformen), of als u ook wilt ondersteunen de [verouderde](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) Tokencache serialisatie van ADAL V3.

De aanpassing van Token-cache-serialisatie voor het delen van de status voor eenmalige aanmelding tussen ADAL.NET 3.x ADAL.NET 5.x en MSAL.NET in het onderdeel van het volgende voorbeeld wordt uitgelegd: [active-directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2)

### <a name="simple-token-cache-serialization-msal-only"></a>Eenvoudige tokencache serialisatie (alleen MSAL)

Hieronder volgt een voorbeeld van een implementatie naïve van aangepaste serialisatie van een token cache voor pc-toepassingen. Hier volgen enkele van de gebruiker-tokencache in een bestand in dezelfde map als de toepassing.

Nadat u de toepassing bouwt, u de serialisatie inschakelen door het aanroepen van ``TokenCacheHelper.EnableSerialization()`` doorgeven van de toepassing `UserTokenCache`

```CSharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

Deze helperklasse lijkt op het volgende codefragment:

```CSharp
static class TokenCacheHelper
 {
  public static void EnableSerialization(ITokenCache tokenCache)
  {
   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Path to the token cache
  /// </summary>
  public static readonly string CacheFilePath = System.Reflection.Assembly.GetExecutingAssembly().Location + ".msalcache.bin3";

  private static readonly object FileLock = new object();


  private static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeMsalV3(File.Exists(CacheFilePath)
            ? ProtectedData.Unprotect(File.ReadAllBytes(CacheFilePath),
                                      null,
                                      DataProtectionScope.CurrentUser)
            : null);
   }
  }

  private static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     // reflect changesgs in the persistent store
     File.WriteAllBytes(CacheFilePath,
                         ProtectedData.Protect(args.TokenCache.SerializeMsalV3(),
                                                 null,
                                                 DataProtectionScope.CurrentUser)
                         );
    }
   }
  }
 }
```

Een voorbeeld van een product kwaliteit tokencache serializer op basis van bestanden voor openbare clienttoepassingen (voor bureaublad toepassingen die worden uitgevoerd op Windows, Mac en linux) beschikbaar via is de [Microsoft.Identity.Client.Extensions.Msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal) Open-source-bibliotheek. U kunt opnemen in uw toepassingen uit de volgende nuget-pakket: [Microsoft.Identity.Client.Extensions.Msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

> Disclaimer. De bibliotheek Microsoft.Identity.Client.Extensions.Msal is een uitbreiding op MSAL.NET. Klassen in deze bibliotheken mogelijk maken hun manier MSAL.NET in de toekomst als, of met belangrijke wijzigingen.

### <a name="dual-token-cache-serialization-msal-unified-cache--adal-v3"></a>Dual-tokencache serialisatie (MSAL unified cache + ADAL V3)

Als u wilt implementeren tokencache serialisatie in de cache met de gecombineerde indeling (gemeenschappelijk ADAL.NET 4.x en MSAL.NET 2.x gebruikt, en met andere MSALs van dezelfde generatie of ouder, op hetzelfde platform), u kunt laat u inspireren door de volgende code :

```CSharp
string appLocation = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location;
string cacheFolder = Path.GetFullPath(appLocation) + @"..\..\..\..");
string adalV3cacheFileName = Path.Combine(cacheFolder, "cacheAdalV3.bin");
string unifiedCacheFileName = Path.Combine(cacheFolder, "unifiedCache.bin");

IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
                                    .Build();
FilesBasedTokenCacheHelper.EnableSerialization(app.UserTokenCache,
                                               unifiedCacheFileName,
                                               adalV3cacheFileName);

```

Deze tijd de helperklasse ziet eruit zoals de volgende code:

```CSharp
using System;
using System.IO;
using System.Security.Cryptography;
using Microsoft.Identity.Client;

namespace CommonCacheMsalV3
{
 /// <summary>
 /// Simple persistent cache implementation of the dual cache serialization (ADAL V3 legacy
 /// and unified cache format) for a desktop applications (from MSAL 2.x)
 /// </summary>
 static class FilesBasedTokenCacheHelper
 {
  /// <summary>
  /// Get the user token cache
  /// </summary>
  /// <param name="adalV3CacheFileName">File name where the cache is serialized with the
  /// ADAL V3 token cache format. Can
  /// be <c>null</c> if you don't want to implement the legacy ADAL V3 token cache
  /// serialization in your MSAL 2.x+ application</param>
  /// <param name="unifiedCacheFileName">File name where the cache is serialized
  /// with the Unified cache format, common to
  /// ADAL V4 and MSAL V2 and above, and also across ADAL/MSAL on the same platform.
  ///  Should not be <c>null</c></param>
  /// <returns></returns>
  public static void EnableSerialization(ITokenCache cache, string unifiedCacheFileName, string adalV3CacheFileName)
  {
   usertokenCache = cache;
   UnifiedCacheFileName = unifiedCacheFileName;
   AdalV3CacheFileName = adalV3CacheFileName;

   usertokenCache.SetBeforeAccess(BeforeAccessNotification);
   usertokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Token cache
  /// </summary>
  static ITokenCache usertokenCache;

  /// <summary>
  /// File path where the token cache is serialized with the unified cache format
  /// (ADAL.NET V4, MSAL.NET V3)
  /// </summary>
  public static string UnifiedCacheFileName { get; private set; }

  /// <summary>
  /// File path where the token cache is serialized with the legacy ADAL V3 format
  /// </summary>
  public static string AdalV3CacheFileName { get; private set; }

  private static readonly object FileLock = new object();

  public static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeAdalV3(ReadFromFileIfExists(AdalV3CacheFileName));
    try
    {
     args.TokenCache.DeserializeMsalV3(ReadFromFileIfExists(UnifiedCacheFileName));
    }
    catch(Exception ex)
    {
     // Compatibility with the MSAL v2 cache if you used one
     args.TokenCache.DeserializeMsalV2(ReadFromFileIfExists(UnifiedCacheFileName));
    }
   }
  }

  public static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     WriteToFileIfNotNull(UnifiedCacheFileName, args.TokenCache.SerializeMsalV3());
     if (!string.IsNullOrWhiteSpace(AdalV3CacheFileName))
     {
      WriteToFileIfNotNull(AdalV3CacheFileName, args.TokenCache.SerializeAdalV3());
     }
    }
   }
  }

  /// <summary>
  /// Read the content of a file if it exists
  /// </summary>
  /// <param name="path">File path</param>
  /// <returns>Content of the file (in bytes)</returns>
  private static byte[] ReadFromFileIfExists(string path)
  {
   byte[] protectedBytes = (!string.IsNullOrEmpty(path) && File.Exists(path))
       ? File.ReadAllBytes(path) : null;
   byte[] unprotectedBytes = encrypt ?
       ((protectedBytes != null) ? ProtectedData.Unprotect(protectedBytes, null, DataProtectionScope.CurrentUser) : null)
       : protectedBytes;
   return unprotectedBytes;
  }

  /// <summary>
  /// Writes a blob of bytes to a file. If the blob is <c>null</c>, deletes the file
  /// </summary>
  /// <param name="path">path to the file to write</param>
  /// <param name="blob">Blob of bytes to write</param>
  private static void WriteToFileIfNotNull(string path, byte[] blob)
  {
   if (blob != null)
   {
    byte[] protectedBytes = encrypt
      ? ProtectedData.Protect(blob, null, DataProtectionScope.CurrentUser)
      : blob;
    File.WriteAllBytes(path, protectedBytes);
   }
   else
   {
    File.Delete(path);
   }
  }

  // Change if you want to test with an un-encrypted blob (this is a json format)
  private static bool encrypt = true;
 }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-API aanroepen vanuit de desktop-app](scenario-desktop-call-api.md)
