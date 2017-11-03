---
title: Azure AD .NET web-app aan de slag | Microsoft Docs
description: Maken van een .NET MVC-web-app die met Azure AD voor aanmelden integreert.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 3c1e558c9d41e385f80939203a3457b74e30973b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>ASP.NET-web-app aanmelden en afmelden met Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Dankzij één aanmelden en afmelden met slechts een paar regels code, kunt Azure Active Directory (Azure AD) u eenvoudig kunt uitbesteden Identiteitsbeheer voor web-app. U kunt gebruikers in en uit de ASP.NET-web-apps ondertekenen met behulp van de Microsoft-implementatie van Open Web Interface voor .NET (OWIN) middleware. OWIN middleware community aangestuurde is opgenomen in .NET Framework 4.5. In dit artikel laat zien hoe OWIN te gebruiken:

* Meld u aan gebruikers voor web-apps met behulp van Azure AD als de id-provider.
* Bepaalde gebruikersgegevens weergeven.
* Meld u aan gebruikers buiten de apps.

## <a name="before-you-get-started"></a>Voordat u aan de slag gaat
* Download de [basis app](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) of download de [voltooide voorbeeld](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).
* U moet ook een Azure AD-tenant waarin de app te registreren. Als u geen Azure AD-tenant al hebt [Lees hoe u een](active-directory-howto-tenant.md).

Wanneer u klaar bent, voert u de procedures in de volgende vier secties.

## <a name="step-1-register-the-new-app-with-azure-ad"></a>Stap 1: De nieuwe app te registreren met Azure AD
Voor het instellen van de app aan gebruikers worden geverifieerd, eerst registreren in de tenant door het volgende te doen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de accountnaam van uw op de bovenste balk. Onder de **Directory** , selecteert u de Active Directory-tenant waar u de app te registreren.
3. Klik op **meer Services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Volg de aanwijzingen voor het maken van een nieuwe **webtoepassing en/of WebAPI**.
  * **Naam** beschrijving van de app voor gebruikers.
  * **Aanmeldings-URL** is de basis-URL van de app. Standaard-URL voor het basisproject is https://localhost:44320 /.
6. Nadat u de registratie hebt voltooid, wijst Azure AD de app een unieke toepassings-ID. Kopieer de waarde van de pagina moet worden gebruikt in de volgende secties.
7. Van de **instellingen** -> **eigenschappen** pagina voor uw toepassing, het bijwerken van de App ID URI. De **App ID URI** is de unieke id voor de app. De naamgevingsconventie is `https://<tenant-domain>/<app-name>` (bijvoorbeeld `https://contoso.onmicrosoft.com/my-first-aad-app`).

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Stap 2: De app instellen voor de pijplijn OWIN-verificatie gebruiken
In deze stap configureert u de OWIN-middleware voor gebruik van het OpenID Connect-verificatieprotocol. U kunt OWIN aan- en afmeldingsaanvragen te verzenden, gebruikerssessies te beheren, ophalen van gebruikersgegevens, enzovoort.

1. Toevoegen om te beginnen, de OWIN middleware NuGet-pakketten aan het project met de Package Manager-Console.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. Een OWIN-Opstartklasse toevoegen aan het project met de naam `Startup.cs`, met de rechtermuisknop op het project, selecteer **toevoegen**, selecteer **Nieuw Item**, en zoek vervolgens naar **OWIN**. De OWIN-middleware roept de **Configuration(...)**  methode wanneer de app wordt gestart.
3. Wijzig de klassendeclaratie naar `public partial class Startup`. We hebben al deel uit van deze klasse geïmplementeerd voor u in een ander bestand. In de **Configuration(...)**  een aanroep van methode **ConfgureAuth(...)**  verificatie voor de app instellen.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. Open het bestand App_Start\Startup.Auth.cs en implementeer de **ConfigureAuth(...)**  methode. De parameters die u opgeeft in *OpenIDConnectAuthenticationOptions* fungeren als coördinaten voor de app om te communiceren met Azure AD. U moet ook voor het instellen van de cookie-verificatie, omdat het OpenID Connect middleware gebruikmaakt van cookies op de achtergrond.

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. Open het bestand web.config in de hoofdmap van het project en voer vervolgens de configuratiewaarden in de `<appSettings>` sectie.
  * `ida:ClientId`: De GUID die u hebt gekopieerd uit de Azure-portal in ' stap 1: de nieuwe app te registreren met Azure AD. "
  * `ida:Tenant`: De naam van uw Azure AD-tenant (bijvoorbeeld: contoso.onmicrosoft.com).
  * `ida:PostLogoutRedirectUri`: De indicator die Azure AD geeft aan waar een gebruiker wordt omgeleid nadat een afmelden aanvraag is voltooid.

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Stap 3: Gebruik OWIN om aan- en afmeldingsaanvragen te verzenden naar Azure AD
De app is nu geconfigureerd om te communiceren met Azure AD met behulp van het OpenID Connect-verificatieprotocol. OWIN heeft afgehandeld dat alle gegevens van verificatieberichten, het valideren van tokens van Azure AD en het onderhoud van gebruikerssessies. Alles wat u hoeft alleen nog uw gebruikers kunnen aanmelden en afmelden geven.

1. U kunt labels in uw domeincontrollers te vereisen dat gebruikers zich aanmelden voordat ze toegang krijgen bepaalde pagina's tot te autoriseren. Om dit te doen, opent u Controllers\HomeController.cs en voeg vervolgens de `[Authorize]` tag met de actie Info.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. U kunt ook OWIN rechtstreeks uitgeven verificatieaanvragen van binnen uw code. Open Controllers\AccountController.cs om dit te doen. Klik in de acties SignIn() en SignOut() uitgeven uitdaging OpenID Connect afmelden aanvragen.

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. Open Views\Shared\_LoginPartial.cshtml om weer te geven van de gebruiker de app aanmelden en afmelden koppelingen en de naam van de gebruiker in een weergave afdrukken.

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
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

## <a name="step-4-display-user-information"></a>Stap 4: Gebruikersgegevens weergeven
Als deze zich gebruikers met OpenID Connect verifieert, retourneert Azure AD een id_token naar de app die 'claims' of asserties over de gebruiker bevat. U kunt deze claims gebruiken voor het aanpassen van de app als volgt:

1. Open het bestand Controllers\HomeController.cs. U hebt toegang tot claims van de gebruiker in uw domeincontrollers via de `ClaimsPrincipal.Current` SPN-object.

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. Ontwikkel en voer de app. Als u dit nog niet hebt al een nieuwe gebruiker in uw tenant met een onmicrosoft.com-domein gemaakt, het is nu tijd om dit te doen. Dit doet u al volgt:

  a. Meld u aan met die gebruiker en houd er rekening mee hoe de identiteit van de gebruiker wordt weergegeven op de bovenste balk.

  b. Meld u af en vervolgens weer aanmelden als een andere gebruiker in uw tenant.

  c. Als u in bijzonder ambitieuze een hebt, registreren en uitvoeren van een ander exemplaar van deze app (met een eigen clientId) en één aanmelding toe in actie bekijken.

## <a name="next-steps"></a>Volgende stappen
Zie voor een verwijzing naar [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (zonder uw configuratiewaarden).

U kunt nu verder met geavanceerdere onderwerpen. Probeer bijvoorbeeld [beveiligen van een Web-API met Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
