---
title: Web-app die web-Api's aanroept (code configuratie)-micro soft Identity-platform
description: Meer informatie over het bouwen van een web-app die web-Api's aanroept (de code configuratie van de app)
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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad995908ff20d123a77b511d127652aa17c4634
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494532"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Web-app die web-Api's aanroept-code configuratie

Zoals u kunt zien in het scenario voor de gebruikers van de [Web-app](scenario-web-app-sign-user-overview.md), geeft u aan dat de gebruiker die zich aanmeldt, wordt gedelegeerd aan de middleware Open ID Connect (OIDC), die u wilt aansluiten op het OIDC-proces. De manier om dit te doen, is afhankelijk van het gebruikte Framework (hier ASP.NET en ASP.NET Core), maar in het eind gaat u een abonnement nemen op middleware OIDC-gebeurtenissen. Het principe is dat:

- U kunt ASP.NET of ASP.NET core een autorisatie code laten aanvragen. Door deze ASP.NET/ASP.NET-kern te doen, kunnen gebruikers zich aanmelden en toestemming geven
- U abonneert u op de ontvangst van de autorisatie code door de web-app.
- Wanneer de verificatie code wordt ontvangen, gebruikt u MSAL-bibliotheken voor het inwisselen van de code en de resulterende toegangs tokens en het vernieuwen van tokens in de token cache. Van daaruit kan de cache worden gebruikt in andere delen van de toepassing om andere tokens op de achtergrond te verkrijgen.

> [!NOTE]
> De code fragmenten uit dit artikel worden geëxtraheerd uit de volgende voor beelden op GitHub, die volledig functioneel zijn:
>
> - [Incrementele zelf studie voor de web-app ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)
> - [ASP.NET Web-app-voor beeld](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)

## <a name="libraries-supporting-web-app-scenarios"></a>Bibliotheken die web-app-scenario's ondersteunen

De bibliotheken die de autorisatie code stroom voor Web Apps ondersteunen zijn:

| MSAL-bibliotheek | Description |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Ondersteunde platforms zijn .NET Framework en .NET Core-platformen (niet UWP, Xamarin. iOS en Xamarin. Android als deze platformen worden gebruikt voor het bouwen van open bare client toepassingen) |
| ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Ontwikkeling in uitvoering-in open bare preview |
| ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Ontwikkeling in uitvoering-in open bare preview |

## <a name="aspnet-core-configuration"></a>ASP.NET Core Configuratie

In ASP.net core gebeuren er dingen in het `Startup.cs` bestand. U moet zich abonneren op de `OnAuthorizationCodeReceived` Open-ID Connect-gebeurtenis en deze gebeurtenis moet MSAL aanroepen. De methode `AcquireTokenFromAuthorizationCode` die het gevolg is van de opslag in de token cache, het toegangs token voor de aangevraagde `scopes`en een vernieuwings token dat wordt gebruikt voor het vernieuwen van het toegangs token wanneer het bijna is verlopen, of voor het verkrijgen van een token namens dezelfde gebruiker , maar voor een andere resource.

```CSharp
string[] scopes = new string[]{ "user.read" };
string[] scopesRequestedByMsalNet = new string[]{ "openid", "profile", "offline_access" };
```

Met de opmerkingen in de onderstaande code krijgt u inzicht in een aantal trucige aspecten van patroon MSAL.NET en ASP.NET Core. Volledige details vindt u in de [ASP.net core-webtoepassing incrementele zelf studie, hoofd stuk 2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)

```CSharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Response type. We ask ASP.NET to request an Auth Code, and an IDToken
   options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

   // This "offline_access" scope is needed to get a refresh token when users sign in with
   // their Microsoft personal accounts
   // (it's required by MSAL.NET and automatically provided by Azure AD when users
   // sign in with work or school accounts, but not with their Microsoft personal accounts)
   options.Scope.Add("offline_access");
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

In ASP.NET Core maakt het samen stellen van de vertrouwelijke client toepassing gebruik van gegevens die zich in de HTTP context bevinden. Dit `HttpContext` is alleen van toepassing op de URL voor de web-app en de aangemelde gebruiker (in `ClaimsPrincipal`een). 

Ook wordt de ASP.NET Core configuratie gebruikt, die een ' AzureAD-sectie bevat, en die is gebonden aan:

- de `_applicationOptions` gegevens structuur van het type [ConfidentialClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationoptions?view=azure-dotnet)
- het `azureAdOptions` exemplaar van het type [AzureAdOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/AzureADOptions.cs) gedefinieerd in `Authentication.AzureAD.UI`ASP.net core. Ten slotte moet de toepassing token caches onderhouden.

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
 string currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, _applicationOptions.CallbackPath ?? string.Empty);

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
 if (UserTokenCacheProvider != null)
 {
  UserTokenCacheProvider.Initialize(app.UserTokenCache, httpContext, claimsPrincipal);
 }
 if (AppTokenCacheProvider != null)
 {
  AppTokenCacheProvider.Initialize(app.AppTokenCache, httpContext);
 }
 return app;
}
```

Zie de zelf studies voor de ASP.NET Core-web-apps voor meer informatie over de token cache-providers [| Token caches](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/455d32f09f4f6647b066ebee583f1a708376b12f/2-WebApp-graph-user/2-2-TokenCache)

> [!NOTE]
> `AcquireTokenByAuthorizationCode`de autorisatie code die door ASP.NET is aangevraagd, wordt in feite ingewisseld en de tokens die worden toegevoegd aan de cache van het MSAL.NET-gebruikers token worden opgehaald. Van daaruit worden ze vervolgens gebruikt in de ASP.NET Core-controllers.

## <a name="aspnet-configuration"></a>ASP.NET-configuratie

De manier waarop ASP.net dingen afhandelt, is vergelijkbaar, behalve dat de configuratie van OpenIdConnect en `OnAuthorizationCodeReceived` het abonnement op de `App_Start\Startup.Auth.cs` gebeurtenis plaatsvindt in het bestand. U vindt soort gelijke concepten, behalve dat hier moet u de RedirectUri in het configuratie bestand opgeven. Dit is een beetje minder krachtig:

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

Ten slotte kan de vertrouwelijke client toepassing ook de identiteit bewijzen met behulp van een client certificaat of een client bevestiging, in plaats van een client geheim.
Het gebruik van client verklaringen is een geavanceerd scenario dat wordt beschreven in [client verklaringen](msal-net-client-assertions.md)

### <a name="msalnet-token-cache-for-a-aspnet-core-web-app"></a>MSAL.NET-token cache voor een ASP.NET-Web-app (kern geheugen)

In web-apps (of Web-Api's als belang rijk) verschilt de implementatie van de token cache van de implementaties van de token cache van het Desktop-toepassing (dit is vaak [gebaseerd op bestanden](scenario-desktop-acquire-token.md#file-based-token-cache). Het kan de ASP.NET/ASP.NET-kern sessie, of een redis-cache, of zelfs Azure Blob-opslag gebruiken. In het bovenstaande code fragment is dit het object van de `EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);` methode aanroep, die een cache service verbindt. De details van wat er hier gebeurt, valt buiten het bereik van deze scenario gids, maar koppelingen worden hieronder vermeld.

> [!IMPORTANT]
> Het is belang rijk om te realiseren dat voor web-apps en Web-Api's één token cache per gebruiker (per account) moet zijn. U moet de token cache voor elk account serialiseren.

Voor beelden van het gebruik van token caches voor web apps en Web-Api's zijn beschikbaar in de [zelf studie](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) voor de ASP.net core-web-app in de phase [2-2-token cache](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache). Voor implementaties ziet u de volgende map [TokenCacheProviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) in de bibliotheek [micro soft-Authentication-Extensions-voor-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) (in de map [micro soft. Identity. client. Extensions. Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) ).

## <a name="next-steps"></a>Volgende stappen

Wanneer de gebruiker zich aanmeldt, wordt er op dit punt een token opgeslagen in de token cache. Laten we eens kijken hoe het wordt gebruikt in andere onderdelen van de web-app.

> [!div class="nextstepaction"]
> [Aanmelden bij de web-app](scenario-web-app-call-api-sign-in.md)
