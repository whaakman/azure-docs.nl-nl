---
title: Web-app dat aanroepen van web-API's (code-configuratie) - Microsoft identity-platform
description: Informatie over het bouwen van een Web-app dat aanroepen van web-API's (configuratie van de app-code)
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
ms.openlocfilehash: bd7f393f889facf147cf25625d5c3b20f886ddf5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65784942"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Web-app dat aanroepen van web-API's - code configureren

Zoals te zien is de [Web-app-gebruikers zich aanmeldt scenario](scenario-web-app-sign-user-overview.md), gezien het feit dat de gebruiker aangemeld is overgedragen aan de Open-ID verbinding maken (OIDC) middleware, u wilt hook-up in de OIDC-proces. De manier om dat te doen, is afhankelijk van het framework u gebruikt (hier ASP.NET en ASP.NET Core), maar uiteindelijk moet u u abonneren op middleware OIDC-gebeurtenissen. Het principe is dat:

- Laat u ASP.NET of ASP.NET core vragen een autorisatiecode. Op deze manier zodat hierdoor core ASP.NET/ASP.NET de gebruiker zich aanmelden en toestemming geven,
- U moet zich abonneren op de ontvangst van de autorisatiecode door de Web-app.
- Wanneer de autorisatiecode wordt ontvangen, gebruikt u MSAL bibliotheken om te wisselen met de code en de resulterende access tokens en tokens store in de cache van token vernieuwen. Van daaruit kan de cache worden gebruikt in andere onderdelen van de toepassing aan te schaffen andere tokens op de achtergrond.

## <a name="libraries-supporting-web-app-scenarios"></a>Bibliotheken ondersteuning van scenario's voor Web-App

De bibliotheken die de autorisatiecodestroom ondersteuning voor Web-Apps zijn:

| Bibliotheek met MSAL | Description |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Ondersteunde platforms zijn .NET Framework en .NET Core-platforms (niet UWP Xamarin.iOS en Xamarin.Android als deze platforms worden gebruikt om openbare client-toepassingen te bouwen) |
| ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Ontwikkeling in voortgang: in openbare preview |
| ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Ontwikkeling in voortgang: in openbare preview |

## <a name="aspnet-core-configuration"></a>Configuratie van ASP.NET Core

In de ASP.NET Core, dingen gebeuren in de `Startup.cs` bestand. Moet u zich kunt abonneren op de `OnAuthorizationCodeReceived` open ID connect-evenement, en deze gebeurtenis, roept u MSAL. De NET-methode `AcquireTokenFromAuthorizationCode` die het effect van het opslaan van de token cache, het toegangstoken voor de aangevraagde bereiken en een vernieuwingstoken dat wordt gebruikt voor het vernieuwen van het toegangstoken als het bijna verlopen, of het opvragen van een token namens de gebruiker heeft , maar voor een andere resource.

De opmerkingen in de onderstaande code ziet u enkele lastige aspecten van weven MSAL.NET en ASP.NET Core

```CSharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Response type. We ask ASP.NET to request an Auth Code, and an IDToken
   options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

   // This "offline_access" scope is needed to get a refresh token when users sign in with
   // their Microsoft personal accounts
   // (it's required by MSAL.NET and automatically provided by Azure AD when users
   // sign in with work or school accounts, but not with their Microsoft personal accounts)
   options.Scope.Add(OidcConstants.ScopeOfflineAccess);
   options.Scope.Add("user.read"); // for instance

   // Handling the auth redemption by MSAL.NET so that a token is available in the token cache
   // where it will be usable from Controllers later (through the TokenAcquisition service)
   var handler = options.Events.OnAuthorizationCodeReceived;
   options.Events.OnAuthorizationCodeReceived = async context =>
   {
    // As AcquireTokenByAuthorizationCode is asynchronous we want to tell ASP.NET core
    // that we are handing the code even if it's not done yet, so that it does 
    // not concurrently call the Token endpoint.
    context.HandleCodeRedemption();

    // Call MSAL.NET AcquireTokenByAuthorizationCode
    var application = BuildConfidentialClientApplication(context.HttpContext,
                                                         context.Principal);
    var result = await application.AcquireTokenByAuthorizationCode(scopes.Except(scopesRequestedByMsalNet), context.ProtocolMessage.Code)
                                  .ExecuteAsync();

    // Do not share the access token with ASP.NET Core otherwise ASP.NET will cache it
    // and will not send the OAuth 2.0 request in case a further call to
    // AcquireTokenByAuthorizationCodeAsync in the future for incremental consent 
    // (getting a code requesting more scopes)
    // Share the ID Token so that the identity of the user is known in the application (in 
    // HttpContext.User)
    context.HandleCodeRedemption(null, result.IdToken);

    // Call the previous handler if any
    await handler(context);
   };
```

In ASP.NET Core, het bouwen van de clienttoepassing vertrouwelijke gegevens die zich in de HttpContext worden gebruikt. Deze HttpContext op de hoogte van de URL voor de Web-App en de gebruiker is aangemeld (in een `ClaimsPrincipal`). Gebruikt ook de configuratie van ASP.NET Core, die een sectie "AzureAD" is en die is gekoppeld aan de `_applicationOptions` gegevensstructuur. Ten slotte moet de toepassing onderhouden token caches.

```CSharp
/// <summary>
/// Creates an MSAL Confidential client application
/// </summary>
/// <param name="httpContext">HttpContext associated with the OIDC response</param>
/// <param name="claimsPrincipal">Identity for the signed-in user</param>
/// <returns></returns>
private IConfidentialClientApplication BuildConfidentialClientApplication(HttpContext httpContext, ClaimsPrincipal claimsPrincipal)
{
 var request = httpContext.Request;

 // Find the URI of the application)
 string currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, azureAdOptions.CallbackPath ?? string.Empty);

 // Updates the authority from the instance (including national clouds) and the tenant
 string authority = $"{azureAdOptions.Instance}{azureAdOptions.TenantId}/";

 // Instantiates the application based on the application options (including the client secret)
 var app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
               .WithRedirectUri(currentUri)
               .WithAuthority(authority)
               .Build();

 // Initialize token cache providers. In the case of Web applications, there must be one
 // token cache per user (here the key of the token cache is in the claimsPrincipal which
 // contains the identity of the signed-in user)
 if (this.UserTokenCacheProvider != null)
 {
  this.UserTokenCacheProvider.Initialize(app.UserTokenCache, httpContext, claimsPrincipal);
 }
 if (this.AppTokenCacheProvider != null)
 {
  this.AppTokenCacheProvider.Initialize(app.AppTokenCache, httpContext);
 }
 return app;
}
```

`AcquireTokenByAuthorizationCode` echt de autorisatiecode die is aangevraagd door ASP.NET wordt ingewisseld en haalt de tokens die zijn toegevoegd aan MSAL.NET gebruiker tokencache. Van daaruit ze vervolgens gebruikt, in de ASP.NET Core-controllers.

## <a name="aspnet-configuration"></a>ASP.NET-configuratie

De manier waarop ASP.NET dingen verwerkt is vergelijkbaar, behalve dat de configuratie van OpenIdConnect en het abonnement op de `OnAuthorizationCodeReceived` gebeurtenis plaatsvindt de `App_Start\Startup.Auth.cs` bestand. Vindt u soortgelijke concepten, behalve dat hier, moet u de RedirectUri opgeven in het configuratiebestand, dit een beetje minder is krachtige:

```CSharp
private void ConfigureAuth(IAppBuilder app)
{
 app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

 app.UseCookieAuthentication(new CookieAuthenticationOptions { });

 app.UseOpenIdConnectAuthentication(
 new OpenIdConnectAuthenticationOptions
 {
  Authority = Globals.Authority,
  ClientId = Globals.ClientId,
  RedirectUri = Globals.RedirectUri,
  PostLogoutRedirectUri = Globals.RedirectUri,
  Scope = Globals.BasicSignInScopes, // a basic set of permissions for user sign in & profile access
  TokenValidationParameters = new TokenValidationParameters
  {
  // We'll inject our own issuer validation logic below.
  ValidateIssuer = false,
  NameClaimType = "name",
  },
  Notifications = new OpenIdConnectAuthenticationNotifications()
  {
   AuthorizationCodeReceived = OnAuthorizationCodeReceived,
  }
 });
}
```

```CSharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
{
 // Upon successful sign in, get & cache a token using MSAL
 string userId = context.AuthenticationTicket.Identity.FindFirst(ClaimTypes.NameIdentifier).Value;
 IConfidentialClientApplication clientapp;
 clientApp = ConfidentialClientApplicationBuilder.Create(Globals.ClientId)
                  .WithClientSecret(Globals.ClientSecret)
                  .WithRedirectUri(Globals.RedirectUri)
                  .WithAuthority(new Uri(Globals.Authority))
                  .Build();

  EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);

 AuthenticationResult result = await clientapp.AcquireTokenByAuthorizationCode(scopes, context.Code)
                                              .ExecuteAsync();
}
```

### <a name="msalnet-token-cache-for-a-aspnet-core-web-app"></a>Cache MSAL.NET Token voor een webtoepassing voor ASP.NET (kernen)

In de web-apps (of web-API's als een kwestie van feiten), de implementatie-tokencache wijkt af van de bureaubladtoepassingen tokencache-implementaties (die zijn vaak [bestand op basis van](scenario-desktop-acquire-token.md#file-based-token-cache). Het kan de sessie ASP.NET/ASP.NET Core, of een Redis-cache, of een database of zelfs Azure Blob-opslag gebruiken. In de code fragment boven dit is het object van de `EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);` aanroepen van de methode die wordt gebonden een cacheservice. De details van wat er gebeurt hier valt buiten het bereik van deze handleiding scenario, maar vindt u koppelingen hieronder.

> [!IMPORTANT]
> Een zeer belangrijke dingen om te profiteren van is dat voor web-Apps en web-API's, moet er een tokencache per gebruiker (per account). U moet de tokencache voor elk account serialiseren.

Voorbeelden van het token caches gebruiken voor Web-apps en web-API's zijn beschikbaar in de [zelfstudie ASP.NET Core Web-Apps](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) in de fase [2-2-tokencache](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache). Voor implementaties een overzicht van de volgende map hebben [TokenCacheProviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) in de [microsoft-verificatie-extensies-voor-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) bibliotheek (in de [ Microsoft.Identity.Client.Extensions.Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) map.

## <a name="next-steps"></a>Volgende stappen

Op dit moment, wanneer de gebruiker zich aanmeldt wordt een token opgeslagen in de tokencache. We gaan kijken hoe deze worden vervolgens gebruikt in andere onderdelen van de Web-app.

> [!div class="nextstepaction"]
> [Meld u aan de Web-App](scenario-web-app-call-api-sign-in.md)
