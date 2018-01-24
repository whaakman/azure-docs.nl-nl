---
title: Azure AD v2.0 .NET web-app aanmelden aan de slag | Microsoft Docs
description: Het bouwen van een .NET MVC-webtoepassing die gebruikers met beide persoonlijke Microsoft-Account en werk-of schoolaccounts ondertekent.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: c8b97ac6-0a06-4367-81b6-7d1d98152b14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: a23b3b1084cf6776cee8583891ae3d879183d072
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Aanmelden toevoegen aan een .NET MVC-web-app
Met het v2.0-eindpunt kunt u snel verificatie van uw web-apps met ondersteuning voor beide persoonlijke Microsoft-accounts en werk- of schoolaccount accounts toevoegen.  In ASP.NET-web-apps, kunt u dit doen met behulp van Microsoft OWIN middleware is opgenomen in .NET Framework 4.5.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

 Wij je hier een web-app die gebruikmaakt van OWIN Meld u aan de gebruiker, sommige informatie weer over de gebruiker en meld u aan de gebruiker buiten de app bouwen.

## <a name="download"></a>Downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Als u wilt volgen, kunt u [basis van de app downloaden als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) of het geraamte:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

De voltooide app is verstrekt aan het einde van deze zelfstudie ook.

## <a name="register-an-app"></a>Een app registreren
Maakt een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of als volgt [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

* Noteer de **toepassings-Id** toegewezen aan uw app, moet u deze snel.
* Voeg de **Web** platform voor uw app.
* Voer de juiste **omleidings-URI**. De omleidings-uri geeft u aan Azure AD waarbij verificatie antwoorden moeten worden omgeleid: de standaardwaarde voor deze zelfstudie is `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installeren en OWIN-verificatie configureren
Geconfigureerd hier de OWIN-middleware voor gebruik van het OpenID Connect-verificatieprotocol.  OWIN wordt gebruikt op aan- en afmeldingsaanvragen te verzenden en informatie ophalen over de gebruiker, onder andere de gebruikerssessie te beheren.

1. Om te beginnen, opent u de `web.config` bestand in de hoofdmap van het project en voer waarden in configuratie van uw app in de `<appSettings>` sectie.

  * De `ida:ClientId` is de **toepassings-Id** toegewezen aan uw app in de portal voor wachtwoordregistratie.
  * De `ida:RedirectUri` is de **omleidings-Uri** u hebt ingevoerd in de portal.

2. Voeg vervolgens de OWIN middleware NuGet-pakketten toe aan het project met de Package Manager-Console.

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. Toevoegen van een 'OWIN-Opstartklasse' aan het project aangeroepen `Startup.cs` rechts Klik op het project--> **toevoegen** --> **Nieuw Item** --> Zoek naar 'OWIN'.  De OWIN-middleware roept de `Configuration(...)`-methode aan als uw app wordt gestart.
4. Wijzig de klassendeclaratie naar `public partial class Startup` -we hebben u al deel uit van deze klasse geïmplementeerd in een ander bestand.  In de `Configuration(...)` methode, een aanroep van ConfigureAuth(...) verificatie voor uw web-app instellen  

        ```csharp
        [assembly: OwinStartup(typeof(Startup))]
        
        namespace TodoList_WebApp
        {
            public partial class Startup
            {
                public void Configuration(IAppBuilder app)
                {
                    ConfigureAuth(app);
                }
            }
        }
        ```

5. Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(...)` methode.  De parameters die u opgeeft in `OpenIdConnectAuthenticationOptions` fungeert als coördinaten voor uw app om te communiceren met Azure AD.  U moet ook Cookie-verificatie instellen - het OpenID Connect middleware maakt gebruik van cookies onder de behandelt.

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
                                             Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                                             RedirectUri = redirectUri,
                                             Scope = "openid email profile",
                                             ResponseType = "id_token",
                                             PostLogoutRedirectUri = redirectUri,
                                             TokenValidationParameters = new TokenValidationParameters
                                             {
                                                     ValidateIssuer = false,
                                             },
                                             Notifications = new OpenIdConnectAuthenticationNotifications
                                             {
                                                     AuthenticationFailed = OnAuthenticationFailed,
                                             }
                                     });
                     }
        ```

## <a name="send-authentication-requests"></a>Verificatieaanvragen verzenden
Uw app is nu geconfigureerd om te communiceren met het v2.0-eindpunt met het OpenID Connect-verificatieprotocol.  OWIN heeft gezorgd voor alle lelijke details van verificatieberichten, het valideren van tokens van Azure AD en het onderhoud van de gebruikerssessie.  Alles wat u hoeft alleen nog uw gebruikers kunnen aanmelden en afmelden geven.

- U kunt labels in uw domeincontrollers vereist die gebruiker zich aanmeldt voor de toegang tot een bepaalde pagina autoriseren.  Open `Controllers\HomeController.cs`, en voeg de `[Authorize]` label aan het Info-controller.
        
        ```csharp
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- U kunt ook OWIN rechtstreeks uitgeven verificatieaanvragen van binnen uw code.  Open `Controllers\AccountController.cs`.  In de acties SignIn() en SignOut() uitgeven OpenID Connect uitdaging afmelden aanvragen, respectievelijk.

        ```csharp
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- Open nu `Views\Shared\_LoginPartial.cshtml`.  Dit is waar u de gebruiker koppelingen voor het aanmelden en afmelden van uw app weergeven en afdrukken de naam van de gebruiker in een weergave.

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@
        
                        Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
                    </li>
                    <li>
                        @Html.ActionLink("Sign out", "SignOut", "Account")
                    </li>
                </ul>
            </text>
        }
        else
        {
            <ul class="nav navbar-nav navbar-right">
                <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
            </ul>
        }
        ```

## <a name="display-user-information"></a>Gebruikersgegevens weergeven
Bij het verifiëren van gebruikers met OpenID Connect retourneert het v2.0-eindpunt een id_token naar de app die claims of asserties over de gebruiker bevat.  U kunt deze claims gebruiken voor het aanpassen van uw app:

- Open het `Controllers\HomeController.cs`-bestand.  U hebt toegang tot claims van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` SPN-object.

        ```csharp
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // The object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // The subject or nameidentifier claim can be used to uniquely identify the user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a>Voer
Ten slotte bouwen en uitvoeren van uw app!   Meld u aan met een persoonlijk Microsoft-Account of een account voor werk of school en zien hoe de identiteit van de gebruiker wordt weergegeven in de bovenste navigatiebalk.  U hebt nu een web-app die is beveiligd met standaardprotocollen die gebruikers met hun persoonlijke en zakelijke/school accounts kunnen worden geverifieerd.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) [is opgegeven als een ZIP hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), of u kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Volgende stappen
U kunt nu verplaatsen naar geavanceerdere onderwerpen.  U wilt proberen:

[Beveiligen van een Web-API met de het v2.0-eindpunt >>](active-directory-devquickstarts-webapi-dotnet.md)

Voor aanvullende bronnen voor uitchecken:

* [De ontwikkelaarshandleiding v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow 'azure active directory'-tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden
We raden u aan in te stellen dat u meldingen ontvangt wanneer er beveiligingsincidenten optreden. Ga hiervoor naar [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneer u op Security Advisory Alerts.
