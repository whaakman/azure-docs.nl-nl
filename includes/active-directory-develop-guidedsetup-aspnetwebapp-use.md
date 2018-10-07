---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 167fccd8e0546bc8f5ac1b24489cae68cc14191f
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48842904"
---
## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Toevoegen van een domeincontroller voor het afhandelen van aanmelding en afmeldingsaanvragen te verzenden

Deze stap ziet u hoe u een nieuwe controller als aanmelden en afmelden methoden beschikbaar wilt maken.

1.  Klik met de rechtermuisknop de `Controllers` map en selecteer `Add` > `Controller`
2.  Selecteer `MVC (.NET version) Controller – Empty`.
3.  Klik op *toevoegen*
4.  Geef het de naam `HomeController` en klikt u op *toevoegen*
5.  Voeg *OWIN* verwijzingen naar de klasse:

    ```csharp
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```
    
6. De twee methoden hieronder om af te handelen aanmelden en afmelden bij de domeincontroller toevoegen door te starten van een verificatiecontrole via code:
    
    ```csharp
    /// <summary>
    /// Send an OpenID Connect sign-in request.
    /// Alternatively, you can just decorate the SignIn method with the [Authorize] attribute
    /// </summary>
    public void SignIn()
    {
        if (!Request.IsAuthenticated)
        {
            HttpContext.GetOwinContext().Authentication.Challenge(
                new AuthenticationProperties{ RedirectUri = "/" },
                OpenIdConnectAuthenticationDefaults.AuthenticationType);
        }
    }
    
    /// <summary>
    /// Send an OpenID Connect sign-out request.
    /// </summary>
    public void SignOut()
    {
        HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
    }
    ```

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Maken van de startpagina van de app aan te melden bij gebruikers via een knop aanmelden

Maak in Visual Studio een nieuwe weergave om de knop Aanmelden toe te voegen en gebruikersgegevens weer te geven na de verificatie:

1.  Klik met de rechtermuisknop de `Views\Home` map en selecteer `Add View`
2.  Noem deze `Index`.
3.  Voeg de volgende HTML toe aan het bestand. Deze bevat de knop Aanmelden:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Guide</title>
    </head>
    <body>
    @if (!Request.IsAuthenticated)
    {
        <!-- If the user is not authenticated, display the sign-in button -->
        <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
            <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
            <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
            <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
            <rect x="150" y="129" width="122" height="122" fill="#F35325" />
            <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
            <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
            <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
            <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
            </svg>
        </a>
    }
    else
    {
        <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
        <br /><br />
        @Html.ActionLink("See Your Claims", "Index", "Claims")
        <br /><br />
        @Html.ActionLink("Sign out", "SignOut", "Home")
    }
    @if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
    {
        <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
    }
    </body>
    </html>
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Meer informatie
> Deze pagina wordt een knop aanmelden in SVG-indeling met een zwarte achtergrond toegevoegd:<br/>![Aanmelden bij Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Voor meer aanmelding knoppen, gaat u naar de [deze pagina](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "Huisstijlrichtlijnen").
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Toevoegen van een domeincontroller om claims van gebruiker weer te geven
Deze controller demonstreert het gebruik van het kenmerk `[Authorize]` om een controller te beveiligen. Met dit kenmerk wordt de toegang tot de controller beperkt doordat alleen geverifieerde gebruikers toegang krijgen. De code hieronder maakt gebruik van het kenmerk om weer te geven van gebruikersclaims die zijn opgehaald als onderdeel van de aanmelding.

1.  Klik met de rechtermuisknop de `Controllers` map: `Add` > `Controller`
2.  Selecteer `MVC {version} Controller – Empty`.
3.  Klik op *toevoegen*
4.  Noem deze `ClaimsController`
5.  Vervang de code van de controllerklasse met de onderstaande - code wordt toegevoegd de `[Authorize]` kenmerk aan de klasse:

    ```csharp
    [Authorize]
    public class ClaimsController : Controller
    {
        /// <summary>
        /// Add user's claims to viewbag
        /// </summary>
        /// <returns></returns>
        public ActionResult Index()
        {
            var userClaims = User.Identity as System.Security.Claims.ClaimsIdentity;
    
            //You get the user’s first and last name below:
            ViewBag.Name = userClaims?.FindFirst("name")?.Value;
    
            // The 'preferred_username' claim can be used for showing the username
            ViewBag.Username = userClaims?.FindFirst("preferred_username")?.Value;
    
            // The subject/ NameIdentifier claim can be used to uniquely identify the user across the web
            ViewBag.Subject = userClaims?.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
    
            // TenantId is the unique Tenant Id - which represents an organization in Azure AD
            ViewBag.TenantId = userClaims?.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid")?.Value;
    
            return View();
        }
    }
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Meer informatie
> Vanwege het gebruik van het kenmerk `[Authorize]` kunnen alle methoden van deze controller alleen worden uitgevoerd als de gebruiker is geverifieerd. Als de gebruiker niet is geverifieerd en probeert te krijgen tot de controller, wordt de OWIN een verificatiecontrole initiëren en afdwingen dat de gebruiker om te verifiëren. De bovenstaande code kijkt naar de lijst met claims voor de kenmerken van de specifieke gebruiker is opgenomen in het Id-token van de gebruiker. Deze kenmerken omvatten de volledige naam en gebruikersnaam van de gebruiker, en het globale onderwerp voor de gebruikers-id. Ze omvatten ook de *Tenant-id*. Dit is de id voor de organisatie van de gebruiker. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Maken van een weergave om claims van de gebruiker weer te geven

Maak in Visual Studio een nieuwe weergave om de claims van de gebruiker weer te geven op een webpagina:

1.  Klik met de rechtermuisknop de `Views\Claims` map en: `Add View`
2.  Noem deze `Index`.
3.  Voeg de volgende HTML toe aan het bestand:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Sample</title>
        <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
    </head>
    <body style="padding:50px">
        <h3>Main Claims:</h3>
        <table class="table table-striped table-bordered table-hover">
            <tr><td>Name</td><td>@ViewBag.Name</td></tr>
            <tr><td>Username</td><td>@ViewBag.Username</td></tr>
            <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
            <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
        </table>
        <br />
        <h3>All Claims:</h3>
        <table class="table table-striped table-bordered table-hover table-condensed">
        @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
        {
            <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
        }
        </table>
        <br />
        <br />
        @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
    </body>
    </html>
    ```
