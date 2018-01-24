---
title: Azure AD v2.0 .NET web-app aanroepen API aan de slag | Microsoft Docs
description: Het bouwen van een .NET MVC-Web-App dat aanroepen web services met behulp van persoonlijke Microsoft-accounts en werk of schoolaccounts voor aanmelden.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f59c9e2c523db319565c1cca13eb85f809b2bdd6
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="calling-a-web-api-from-a-net-web-app"></a>Een web-API aanroept vanuit een .NET-web-app
U kunt snel verificatie toevoegen aan uw web-apps en web-API's met ondersteuning voor beide persoonlijke Microsoft-accounts en werk-of schoolaccounts met het v2.0-eindpunt.  Wij je hier een MVC-web-app die gebruikers in het gebruik van het OpenID Connect, met hulp van Microsoft OWIN middleware ondertekent bouwen.  De web-app wordt OAuth 2.0-toegangstokens ophalen voor een web api is beveiligd met OAuth 2.0 waarmee maken, lezen en verwijderen op een bepaalde gebruiker 'takenlijst'.

Deze zelfstudie wordt de nadruk gelegd voornamelijk over het gebruik van MSAL verkrijgen en toegangstokens gebruiken in een web-app beschreven in de volledige [hier](active-directory-v2-flows.md#web-apps).  Als de vereisten, wilt u mogelijk eerst meer informatie over hoe [basic aanmelden toevoegen aan een web-app](active-directory-v2-devquickstarts-dotnet-web.md) of hoe [een web-API goed is beveiligd](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

## <a name="download-sample-code"></a>Voorbeeldcode downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Als u wilt volgen, kunt u [basis van de app downloaden als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) of het geraamte:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

U kunt ook [downloaden van de voltooide app als een ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) of te klonen van de voltooide app:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Een app registreren
Maakt een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of als volgt [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

* Noteer de **toepassings-Id** toegewezen aan uw app, moet u deze snel.
* Maak een **App geheim** van de **wachtwoord** type en noteer de waarde voor later
* Voeg de **Web** platform voor uw app.
* Voer de juiste **omleidings-URI**. De omleidings-uri geeft u aan Azure AD waarbij verificatie antwoorden moeten worden omgeleid: de standaardwaarde voor deze zelfstudie is `https://localhost:44326/`.

## <a name="install-owin"></a>OWIN installeren
Voeg de OWIN middleware NuGet-pakketten naar de `TodoList-WebApp` project met de Package Manager-Console.  De OWIN-middleware wordt aan- en afmeldingsaanvragen te verzenden, sessie van de gebruiker beheren en informatie over de gebruiker, onder andere gebruikt.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Meld u aan de gebruiker
Nu configureren de OWIN-middleware gebruiken de [OpenID Connect-verificatieprotocol](active-directory-v2-protocols.md).  

* Open de `web.config` bestand in de hoofdmap van de `TodoList-WebApp` project en voer de configuratiewaarden van uw app in de `<appSettings>` sectie.
  * De `ida:ClientId` is de **toepassings-Id** toegewezen aan uw app in de portal voor wachtwoordregistratie.
  * De `ida:ClientSecret` is de **App geheim** u hebt gemaakt in de portal voor wachtwoordregistratie.
  * De `ida:RedirectUri` is de **omleidings-Uri** u hebt ingevoerd in de portal.
* Open de `web.config` bestand in de hoofdmap van de `TodoList-Service` project en vervang de `ida:Audience` met dezelfde **toepassings-Id** zoals hierboven.
* Open het bestand `App_Start\Startup.Auth.cs` en voeg `using` -instructies voor de bibliotheken van hierboven.
* Implementeren in hetzelfde bestand de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIDConnectAuthenticationOptions` fungeert als coördinaten voor uw app om te communiceren met Azure AD.

```csharp
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Gebruik MSAL toegangstokens ophalen
In de `AuthorizationCodeReceived` melding, die we willen gebruiken [OAuth 2.0 in combinatie met OpenID Connect](active-directory-v2-protocols.md) om in te wisselen van de authorization_code voor een toegangstoken uit aan de taak Service.  MSAL kan dit proces toegankelijk maken voor u:

* Installeer eerst de evaluatieversie van MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* En voeg een andere `using` instructie voor de `App_Start\Startup.Auth.cs` -bestand voor MSAL.
* Voeg nu een nieuwe methode de `OnAuthorizationCodeReceived` gebeurtenis-handler.  Deze handler MSAL gebruikt om te verkrijgen van een toegangstoken voor de API van de lijst met taken en het token opgeslagen in de MSAL tokencache voor later:

```csharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* MSAL heeft in de web-apps, een uitbreidbaar tokencache die kan worden gebruikt voor het opslaan van tokens.  Dit voorbeeld implementeert de `NaiveSessionCache` die gebruikmaakt van HTTP-sessie opslag voor cache-tokens.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>De Web-API aanroepen
Nu is het tijd om daadwerkelijk gebruiken de access_token die u hebt verkregen in stap 3.  Open de web-app `Controllers\TodoListController.cs` bestand, waardoor alle CRUD-aanvragen naar de API van de lijst met taken.

* U kunt MSAL opnieuw gebruiken hier access_tokens ophalen uit de cache MSAL.  Voeg eerst een `using` -instructie voor MSAL tot dit bestand.
  
    `using Microsoft.Identity.Client;`
* In de `Index` van de actie, gebruik MSAL `AcquireTokenSilentAsync` methode voor het ophalen van een access_token die kunnen worden gebruikt om gegevens te lezen van de takenlijst-service:

```csharp
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* Het resulterende token in het voorbeeld vervolgens worden toegevoegd aan de HTTP GET-aanvraag als de `Authorization` koptekst, die de takenlijst-service gebruikt voor het verifiëren van de aanvraag.
* Als de takenlijst-service retourneert een `401 Unauthorized` antwoord wordt de access_tokens in MSAL niet meer geldig voor een bepaalde reden.  In dit geval moet u eventuele access_tokens niet verwijderen uit de cache MSAL en weergeven van de gebruiker een bericht dat ze aanmelden moeten mogelijk, die wordt opnieuw gestart de stroom token verkrijgen.

```csharp
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

* Op dezelfde manier als MSAL kan een access_token retourneren voor een of andere reden is, moet u opdracht geven de gebruiker zich opnieuw aanmeldt.  Dit is net zo eenvoudig als het afvangen van een `MSALException`:

```csharp
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

* De exacte dezelfde `AcquireTokenSilentAsync` aanroep is implementd in de `Create` en `Delete` acties.  In de web-apps, kunt u deze methode MSAL access_tokens ophalen wanneer u nodig hebt in uw app.  MSAL zorgt voor het ophalen, opslaan in cache en vernieuwen van tokens voor u.

Ten slotte bouwen en uitvoeren van uw app!  Meld u aan met een Microsoft-Account of een Azure AD-Account en zien hoe de identiteit van de gebruiker wordt weergegeven in de bovenste navigatiebalk.  Toevoegen en verwijderen van enkele items uit de takenlijst van de gebruiker om te zien van dat de OAuth 2.0 API-aanroepen in actie beveiligd.  U hebt nu een web-app & web-API beide beveiligd via standaardprotocollen die gebruikers met hun persoonlijke en zakelijke/school accounts kunnen worden geverifieerd.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) [vindt u hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Volgende stappen
Voor aanvullende bronnen voor uitchecken:

* [De ontwikkelaarshandleiding v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow 'azure active directory'-tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden
We raden u aan in te stellen dat u meldingen ontvangt wanneer er beveiligingsincidenten optreden. Ga hiervoor naar [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneer u op Security Advisory Alerts.

