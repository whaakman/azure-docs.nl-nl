---
title: Web-app die gebruikers worden aangemeld (aanmelden) - Microsoft identity-platform
description: Informatie over het bouwen van een web-app waarmee gebruikers zich aanmeldt (aanmelden)
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
ms.openlocfilehash: 3fb7fbba7ec48da580d2a630ae51aa20b3307848
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074618"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Web-app waarmee gebruikers worden aangemeld: Meld u aan

Informatie over het aanmelden toevoegen aan de code voor uw web-app waarmee gebruikers zich aanmeldt.

## <a name="sign-in"></a>aanmelden

De code die we in het vorige artikel hebben [de configuratie van app-code](scenario-web-app-sign-user-app-configuration.md) is alles wat u nodig voor het implementeren van afmelden. Als de gebruiker zich heeft aangemeld bij uw app, wilt u waarschijnlijk kunnen ze zich afmelden. ASP.NET core verwerkt afmelden voor u.

## <a name="what-sign-out-involves"></a>Wat afmelden omvat

Afmelden van een web-app is over het verwijderen van meer dan de informatie over het aangemelde account van de status van de web-app.
De web-app moet de gebruiker ook omleiden naar de Microsoft identity platform v2.0 `logout` eindpunt afmelden. Wanneer uw web-app leidt de gebruiker de `logout` eindpunt, dit eindpunt wordt gewist sessie vanuit de browser van de gebruiker. Als uw app niet hebt gaat u naar de `logout` eindpunt, de gebruiker kan verifiëren aan uw app zonder hun referenties opnieuw in te voeren omdat ze een geldige eenmalige aanmelding bij sessie met het v2.0-eindpunt van de Microsoft Identity-platform hebben zouden.

Zie voor meer informatie, de [verzenden van een aanvragen voor afmelden](v2-protocols-oidc.md#send-a-sign-out-request) sectie de [v2.0 voor Microsoft Identity-platform en de OpenID Connect-protocol](v2-protocols-oidc.md) conceptuele documentatie.

## <a name="application-registration"></a>Een toepassing registreren

Tijdens de registratie van de toepassing moet zijn geregistreerd een **afmelden URI boeken**. In onze zelfstudie hebt u geregistreerd `https://localhost:44321/signout-oidc` in de **afmeldings-URL van** veld van de **geavanceerde instellingen** sectie de **verificatie** pagina. Zie voor meer informatie, [ de webApp-app te registreren](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp)

## <a name="aspnet-core-code"></a>ASP.NET Core-code

### <a name="signout-button"></a>De knop die afmelden wordt weergegeven

De afmeldings-knop wordt weergegeven in `Views\Shared\_LoginPartial.cshtml` en wordt alleen weergegeven wanneer er een geverifieerde account (dat wil zeggen wanneer de gebruiker eerder heeft aangemeld).

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

### <a name="signout-action-of-the-accountcontroller"></a>`Signout()` actie van de `AccountController`

Druk op de **Afmelden** knop op het web-app-triggers de `SignOut` actie op de `Account` controller. In eerdere versies van de ASP.NET core-sjablonen, de `Account` controller is ingesloten met de web-app, maar dit is niet meer het geval omdat het is nu onderdeel van het framework van ASP.NET Core zelf. 

De code voor de `AccountController` is beschikbaar via de ASP.NET core-opslagplaats op uit [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs). Het besturingselement account:

- Stelt een OpenID omleidings-URI op `/Account/SignedOut` zodat de controller wordt aangeroepen wanneer Azure AD is de afmeldings-uitgevoerd.
- Aanroepen `Signout()`, waarmee de OpenIdConnect middleware Neem contact op met de Microsoft identity-platform `logout` eindpunt waarmee:

  - Wist de sessiecookie van de browser, en
  - Aanroepen ten slotte roept terug de **afmeldings-URL van**, welke) wordt standaard weergegeven de ondertekende weergave pagina [SignedOut.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) ook beschikbaar als onderdeel van ASP.NET Core.

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>De aanroep van onderschept de `logout` eindpunt

De ASP.NET Core OpenIdConnect middleware waarmee uw app worden onderschept de aanroep van de Microsoft identity-platform `logout` eindpunt door te geven van een OpenIdConnect-gebeurtenis met de naam `OnRedirectToIdentityProviderForSignOut`. De web-app gebruikt om te proberen om te voorkomen dat het dialoogvenster Selecteer het account om u te worden weergegeven voor de gebruiker bij het afmelden. Deze worden onderschept wordt uitgevoerd in de `AddAzureAdV2Authentication` van de `Microsoft.Identity.Web` herbruikbare-bibliotheek. Zie [StartupHelpers.cs L58-L66](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L58-L66)

```CSharp
public static IServiceCollection AddAzureAdV2Authentication(this IServiceCollection services,
                                                            IConfiguration configuration)
{
    ...
    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
        ...
        options.Authority = options.Authority + "/v2.0/";
        ...
        // Attempt to avoid displaying the select account dialog when signing out
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            var user = context.HttpContext.User;
            context.ProtocolMessage.LoginHint = user.GetLoginHint();
            context.ProtocolMessage.DomainHint = user.GetDomainHint();
            await Task.FromResult(0);
        };
    }
}
```

## <a name="aspnet-code"></a>ASP.NET Code

In ASP.NET, afmelden wordt geactiveerd met de methode SignOut() op een domeincontroller (bijvoorbeeld AccountController). Deze methode maakt geen deel uit van het framework ASP.NET (in tegenstelling tot wat er in ASP.NET Core gebeurt) en geen gebruik maakt van asynchrone, en daarom is het:

- verzendt een OpenId afmelding challenge
- de cache
- wordt omgeleid naar de pagina die software wil

```CSharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 MsalAppBuilder.ClearUserTokenCache();
 Response.Redirect("/");
}
```

## <a name="protocol"></a>Protocol

Als u niet wilt u ASP.NET Core- of ASP.NET, kunt u de documentatie van protocol, die beschikbaar is bij kijken [Open ID Connect](./v2-protocols-oidc.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-web-app-sign-user-production.md)
