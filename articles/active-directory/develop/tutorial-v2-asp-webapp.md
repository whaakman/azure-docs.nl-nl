---
title: Aan de slag met Azure AD v 2.0 ASP.NET-webserver | Microsoft Docs
description: Implementatie van micro soft-aanmelding bij een ASP.NET-oplossing met een traditionele, op de webbrowser gebaseerde toepassing met OpenID Connect Connect Standard
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fce6ab2c5068d1628860356a9df0dd33c73948b3
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69511908"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Aanmelding met Microsoft toevoegen aan een ASP.NET-web-app

In deze hand leiding wordt gedemonstreerd hoe u aanmelden met micro soft implementeert met behulp van een ASP.NET MVC-oplossing met een traditionele webbrowser toepassing met behulp van OpenID Connect Connect.

Aan het einde van deze hand leiding kan uw toepassing aanmeldingen van persoonlijke accounts accepteren, bijvoorbeeld outlook.com, live.com en anderen. Deze accounts omvatten ook werk-en school accounts van een bedrijf of organisatie die is geïntegreerd met Azure Active Directory.

> Voor deze hand leiding is Visual Studio 2019 vereist.  Is dit niet het geval?  [Down load Visual Studio 2019 gratis](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Hoe de voor beeld-app die wordt gegenereerd door deze hand leiding werkt

![Toont hoe de voor beeld-app die door deze zelf studie wordt gegenereerd, werkt](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.svg)

De voorbeeld toepassing die u maakt, is gebaseerd op het scenario waarin u de browser gebruikt om toegang te krijgen tot een ASP.NET-website die een gebruiker vraagt om zich te verifiëren via een aanmeldings knop. In dit scenario vindt het grootste gedeelte van het werk om de webpagina weer te geven plaats aan de serverzijde.

## <a name="libraries"></a>Bibliotheken

In deze hand leiding worden de volgende bibliotheken gebruikt:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware die een toepassing in staat stelt om OpenIdConnect te gebruiken voor verificatie|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware die een toepassing in staat stelt om een gebruikerssessie te onderhouden met behulp van cookies|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Maakt het mogelijk om OWIN-toepassingen uit te voeren op IIS met behulp van de ASP.NET-aanvraagpijplijn|

## <a name="set-up-your-project"></a>Uw project instellen

In deze sectie vindt u de stappen voor het installeren en configureren van de verificatie pijplijn via OWIN middleware in een ASP.NET-project met behulp van OpenID Connect Connect.

> Wilt u in plaats daarvan het Visual Studio-project van dit voor beeld downloaden? [Down load een project](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) en ga naar de [configuratie stap](#register-your-application) om het code voorbeeld te configureren voordat u het programma uitvoert.

### <a name="create-your-aspnet-project"></a>Uw ASP.NET-project maken

1. In Visual Studio:`File` > `New` > `Project`
2. Selecteer *onder C#Visual \Web*de `ASP.NET Web Application (.NET Framework)`optie.
3. Geef uw toepassing een naam en klik op *OK*
4. Selecteer `Empty` en schakel het selectie vakje in `MVC` om verwijzingen toe te voegen

## <a name="add-authentication-components"></a>Onderdelen voor verificatie toevoegen

1. In Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Voeg *NuGet-pakketten voor OWIN-middleware* toe door het volgende te typen in het Package Manager Console-venster:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Over deze bibliotheken
> De bovenstaande bibliotheken maken SSO (eenmalige aanmelding) met behulp van OpenID Connect mogelijk via verificatie op basis van cookies. Nadat de verificatie is voltooid en het token dat de gebruiker vertegenwoordigt, is verzonden naar de toepassing, wordt met OWIN-middleware een sessiecookie gemaakt. De browser gebruikt deze cookie vervolgens op volgende aanvragen, zodat de gebruiker het wacht woord niet opnieuw hoeft te typen en er geen aanvullende verificatie nodig is.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>De verificatiepijplijn configureren

De onderstaande stappen worden gebruikt voor het maken van een OWIN-middleware voor het configureren van OpenID Connect Connect-verificatie. Deze klasse wordt automatisch uitgevoerd wanneer het IIS-proces wordt gestart.

> [!TIP]
> Ga als volgt te werk als uw project geen `Startup.cs`-bestand bevat in de hoofdmap:
> 1. Klik met de rechter muisknop op de hoofdmap van het project: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Noem deze `Startup.cs`
>
>> Zorg dat de geselecteerde klasse een OWIN-opstartklasse is, en geen C#-standaardklasse. Verzeker u hiervan door te kijken of `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` wordt weergegeven boven de naamruimte.

1. *OWIN* en *micro soft. Identity model* -verwijzingen `Startup.cs`toevoegen aan:

    ```csharp
    using Microsoft.Owin;
    using Owin;
    using Microsoft.IdentityModel.Protocols.OpenIdConnect;
    using Microsoft.IdentityModel.Tokens;
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    using Microsoft.Owin.Security.Notifications;
    ```

2. Vervang de opstart klasse door de onderstaande code:

    ```csharp
    public class Startup
    {
        // The Client ID is used by the application to uniquely identify itself to Azure AD.
        string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

        // RedirectUri is the URL where the user will be redirected to after they sign in.
        string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

        // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
        static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

        // Authority is the URL for authority, composed by Azure Active Directory v2.0 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
        string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

        /// <summary>
        /// Configure OWIN to use OpenIdConnect 
        /// </summary>
        /// <param name="app"></param>
        public void Configuration(IAppBuilder app)
        {
            app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

            app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
                new OpenIdConnectAuthenticationOptions
                {
                    // Sets the ClientId, authority, RedirectUri as obtained from web.config
                    ClientId = clientId,
                    Authority = authority,
                    RedirectUri = redirectUri,
                    // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                    PostLogoutRedirectUri = redirectUri,
                    Scope = OpenIdConnectScope.OpenIdProfile,
                    // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                    ResponseType = OpenIdConnectResponseType.IdToken,
                    // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                    // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                    // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter
                    TokenValidationParameters = new TokenValidationParameters()
                    {
                        ValidateIssuer = false // This is a simplification
                    },
                    // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed
                    }
                }
            );
        }

        /// <summary>
        /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
        /// </summary>
        /// <param name="context"></param>
        /// <returns></returns>
        private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
        {
            context.HandleResponse();
            context.Response.Redirect("/?errormessage=" + context.Exception.Message);
            return Task.FromResult(0);
        }
    }
    ```

> [!NOTE]
> De `ValidateIssuer = false` instelling is een vereenvoudiging voor deze Quick Start. In echte toepassingen die u nodig hebt om de uitgever te valideren, ziet u de voor beelden om te begrijpen hoe u dat doet.

<!--start-collapse-->
> ### <a name="more-information"></a>Meer informatie
> De parameters die u opgeeft in *OpenIDConnectAuthenticationOptions*, dienen als coördinaten waarmee de toepassing kan communiceren met Azure AD. Omdat de OpenID Connect Connect-verbinding gebruikmaakt van cookies op de achtergrond, moet u ook cookie-verificatie instellen, zoals in de bovenstaande code wordt weer gegeven. De waarde *ValidateIssuer* zorgt ervoor dat de toegang via OpenIdConnect niet wordt beperkt tot één specifieke organisatie.
<!--end-collapse-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Een controller toevoegen om aanmeldings- en afmeldingsaanvragen te verwerken

In deze stap ziet u hoe u een nieuwe controller maakt om aanmeldings-en afmeldings methoden beschikbaar te maken.

1.  Klik met de `Controllers` rechter muisknop op de map en selecteer`Add` > `Controller`
2.  Selecteer `MVC (.NET version) Controller – Empty`.
3.  Klik op *toevoegen*
4.  Geef deze `HomeController` een naam en klik op *toevoegen*
5.  *OWIN* -verwijzingen toevoegen aan de klasse:

    ```csharp
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```
    
6. Voeg de twee onderstaande methoden toe voor het afhandelen van de aanmelding en het afmelden bij uw controller door een verificatie vraag te initiëren via code:
    
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

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>De startpagina van de app maken om gebruikers aan te melden via een aanmeldknop

Maak in Visual Studio een nieuwe weergave om de aanmeldknop toe te voegen en gebruikersgegevens weer te geven na de verificatie:

1.  Klik met de `Views\Home` rechter muisknop op de map en selecteer`Add View`
2.  Noem deze `Index`.
3.  Voeg de volgende HTML toe aan het bestand. Deze bevat de aanmeldknop:

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
> Op deze pagina wordt een knop Aanmelden toegevoegd in SVG-indeling met een zwarte achtergrond:<br/>![Aanmelden met Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Ga naar de(https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "richt lijnen voor de huis stijl")van [Deze pagina]voor meer aanmeldings knoppen.
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Een controller toevoegen om claims van de gebruiker weer te geven
Deze controller demonstreert het gebruik van het kenmerk `[Authorize]` om een controller te beveiligen. Met dit kenmerk wordt de toegang tot de controller beperkt doordat alleen geverifieerde gebruikers toegang krijgen. De onderstaande code maakt gebruik van het kenmerk om gebruikers claims weer te geven die zijn opgehaald als onderdeel van de aanmelding.

1.  Klik met de `Controllers` rechter muisknop op de map:`Add` > `Controller`
2.  Selecteer `MVC {version} Controller – Empty`.
3.  Klik op *toevoegen*
4.  Noem deze `ClaimsController`
5.  Vervang de code van uw klasse controller door de onderstaande code. Hiermee voegt u `[Authorize]` het kenmerk toe aan de klasse:

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
> Vanwege het gebruik van het kenmerk `[Authorize]` kunnen alle methoden van deze controller alleen worden uitgevoerd als de gebruiker is geverifieerd. Als de gebruiker niet is geverifieerd en probeert toegang te krijgen tot de controller, initieert OWIN een verificatie test en dwingt hij de gebruiker te verifiëren. De bovenstaande code bevat een lijst met claims voor specifieke gebruikers kenmerken die zijn opgenomen in de token-id van de gebruiker. Deze kenmerken omvatten de volledige naam en gebruikersnaam van de gebruiker, en het globale onderwerp voor de gebruikers-id. Ze omvatten ook de *Tenant-id*. Dit is de id voor de organisatie van de gebruiker. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Een weergave maken om de claims van de gebruiker weer te geven

Maak in Visual Studio een nieuwe weergave om de claims van de gebruiker weer te geven op een webpagina:

1.  Klik met de `Views\Claims` rechter muisknop op de map en:`Add View`
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

## <a name="register-your-application"></a>Uw toepassing registreren

U hebt twee opties om uw toepassing te registreren en de registratie gegevens van uw toepassing aan uw oplossing toe te voegen:

### <a name="option-1-express-mode"></a>Optie 1: Express-modus

U kunt uw toepassing snel registreren door het volgende te doen:

1. Ga naar het deel venster nieuwe [Azure Portal-app-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) .
1. Voer een naam in voor de toepassing en klik op **Registreren**.
1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus

Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:

1. Ga naar Visual Studio en:
   1. in Solution Explorer selecteert u het project en bekijkt u de venster Eigenschappen (als u geen venster Eigenschappen ziet, drukt u op F4).
   1. Wijzig SSL ingeschakeld in `True`.
   1. Klik met de rechter muisknop op het project in Visual Studio, klik vervolgens op **Eigenschappen**en op het tabblad **Web** . Wijzig in het gedeelte *servers* de *project-URL* als de SSL-URL.
   1. Kopieer de SSL-URL. In de volgende stap gaat u deze URL toevoegen aan de lijst met omleidings-Url's in de lijst met omleidings-Url's van de registratie portal:<br/><br/>![Projecteigenschappen](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
1. Navigeer naar de pagina micro soft-identiteits platform voor ontwikkel aars [app-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) .
1. Selecteer **nieuwe registratie**.
1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
   1. Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `ASPNET-Tutorial`.
   1. Voeg de SSL-URL die u uit Visual Studio hebt gekopieerd in stap 1 ( `https://localhost:44368/`bijvoorbeeld) toe aan de **antwoord-URL**en klik op **registreren**.
1. Selecteer **Verificatie**, stel **ID-tokens** in onder **Impliciete toekenning** en selecteer vervolgens **Opslaan**.
1. Voeg het volgende toe `web.config` in de hoofdmap van het gedeelte: `configuration\appSettings`

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. Vervang `ClientId` door de toepassings-id die u zojuist hebt geregistreerd.
1. Vervang `redirectUri` door de SSL-URL van uw project.

## <a name="test-your-code"></a>Uw code testen

Als u uw toepassing wilt testen in Visual Studio, drukt u op **F5** om het project uit te voeren. De browser wordt geopend op de<span></span>locatie http://localhost: {Port} en u ziet de knop **Aanmelden met Microsoft** . Selecteer de knop om het aanmeldings proces te starten.

Wanneer u klaar bent om uw test uit te voeren, gebruikt u een Microsoft Azure Active Directory (Azure AD)-account (werk-of school account) of een persoonlijke Microsoft-account (<span>Live.</span> com of <span>Outlook.</span> com) om u aan te melden.

![Aanmelden met Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Meld u aan bij uw Microsoft-account](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Toepassings resultaten weer geven

Nadat u zich hebt aangemeld, wordt de gebruiker omgeleid naar de start pagina van uw website. De start pagina is de HTTPS-URL die is opgegeven in de registratie gegevens van uw toepassing in de micro soft Application Registration-Portal. De start pagina bevat een welkomst bericht *' Hello \<User >, '* een koppeling om u af te melden en een koppeling om de claims van de gebruiker weer te geven. De koppeling voor de claims van de gebruiker bladert naar de *claim* controller die u eerder hebt gemaakt.

### <a name="browse-to-see-the-users-claims"></a>Bladeren om de claims van de gebruiker weer te geven

Als u de claims van de gebruiker wilt zien, selecteert u de koppeling om naar de weer gave controller te bladeren die alleen beschikbaar is voor geverifieerde gebruikers.

#### <a name="view-the-claims-results"></a>De claim resultaten weer geven

Nadat u naar de weer gave controller hebt gebladerd, ziet u een tabel die de basis eigenschappen voor de gebruiker bevat:

|Eigenschap |Waarde |Beschrijving |
|---|---|---|
|**Name** |Volledige naam van de gebruiker | De voor-en achternaam van de gebruiker.
|**Gebruikersnaam** |gebruiker<span>@domain.com</span> | De gebruikers naam die wordt gebruikt om de gebruiker te identificeren.
|**Subject** |Subject |Een teken reeks waarmee de gebruiker via het web op unieke wijze wordt geïdentificeerd.|
|**Tenant ID** |Guid | Een unieke **GUID** die de Azure AD-organisatie van de gebruiker vertegenwoordigt.|

Daarnaast ziet u een tabel met alle claims in de verificatie aanvraag. Zie de [lijst met claims die zich in een Azure AD-ID-token bevinden](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)voor meer informatie.

### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Toegang tot een methode met een kenmerk voor autoriseren testen (optioneel)

Voer de volgende stappen uit om de toegang te testen als een anonieme `Authorize` gebruiker naar een controller die wordt beveiligd met het kenmerk:

1. Selecteer de koppeling om u aan te melden bij de gebruiker en het afmeldings proces te volt ooien.
2. Typ http://<span></span>localhost: {Port}/claims in uw browser om toegang te krijgen tot uw controller die is beveiligd `Authorize` met het kenmerk.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Verwachte resultaten na toegang tot een beveiligde controller

U wordt gevraagd om u te verifiëren om de weer gave beveiligde controller te gebruiken.

## <a name="advanced-options"></a>Geavanceerde opties

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Uw hele website beveiligen
Als u uw hele website wilt beveiligen, voegt u in het bestand **Global. asax** het `AuthorizeAttribute` kenmerk `GlobalFilters` toe aan het `Application_Start` filter in de methode:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Beperken wie zich kan aanmelden bij uw toepassing

Wanneer u de toepassing bouwt die door deze hand leiding wordt gemaakt, accepteert uw toepassing standaard aanmeldingen van persoonlijke accounts (waaronder outlook.com, live.com en anderen), evenals werk-en school accounts van elk bedrijf of organisatie dat is geïntegreerd met Azure Active Directory. Dit is een aanbevolen optie voor SaaS-toepassingen.

Als u de toegang voor gebruikers aanmelding voor uw toepassing wilt beperken, zijn er meerdere opties beschikbaar:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Optie 1: Toegang tot de toepassing beperken tot alleen gebruikers uit het Active Directory-exemplaar van één bepaalde organisatie (één tenant)

Deze optie is een veelvoorkomend scenario voor *LOB-toepassingen*: Als u wilt dat uw toepassing aanmeldingen alleen accepteert van accounts die deel uitmaken van een specifieke Azure Active Directory-exemplaar (inclusief *Gast accounts* van die instantie), doet u het volgende:

1. Wijzig in het bestand **Web. config** de waarde voor de `Tenant` para meter van `Common` naar de Tenant `contoso.onmicrosoft.com`naam van de organisatie, zoals.
2. Stel in uw [OWIN-opstart klasse](#configure-the-authentication-pipeline)het `ValidateIssuer` argument in `true`op.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>Optie 2: De toegang tot uw toepassing beperken tot gebruikers in een specifieke lijst met organisaties

U kunt aanmeldings toegang beperken tot alleen gebruikers accounts in een Azure AD-organisatie die zich in de lijst met toegestane organisaties bevindt:
1. Stel in uw [OWIN-opstart klasse](#configure-the-authentication-pipeline)het `ValidateIssuer` argument in `true`op.
2. Stel de waarde van de `ValidIssuers` para meter in op de lijst met toegestane organisaties.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Optie 3: Een aangepaste methode gebruiken om verleners te valideren

U kunt een aangepaste methode implementeren om verleners te valideren met behulp van de para meter **IssuerValidator** . Meer informatie over het gebruik van deze para meter vindt u in de [TokenValidationParameters-klasse](/previous-versions/visualstudio/dn464192(v=vs.114)).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over web apps die web-Api's aanroepen:.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>De stappen voor het maken van de toepassing die wordt gebruikt in deze snelstart

> [!div class="nextstepaction"]
> [Web-apps die web-Api's aanroepen]( https://aka.ms/msal-net-authorization-code)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Help ons het micro soft Identity-platform te verbeteren. Vertel ons wat u denkt door een korte enquête met twee vragen te volt ooien.

> [!div class="nextstepaction"]
> [Micro soft Identity platform-enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)