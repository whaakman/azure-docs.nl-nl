
## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Toevoegen van een domeincontroller voor het afhandelen van aanmelden en afmeldingsaanvragen te verzenden

Deze stap ziet een nieuwe domeincontroller om aan- en afmeldingsaanvragen methoden zichtbaar te maken.

1.  Met de rechtermuisknop op de `Controllers` map en selecteer`Add` > `Controller`
2.  Selecteer `MVC (.NET version) Controller – Empty`.
3.  Klik op *toevoegen*
4.  Naam `HomeController` en klik op *toevoegen*
5.  Voeg *OWIN* verwijzingen naar de klasse:

```csharp
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
```
<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
De twee methoden hieronder om af te handelen aanmelden en afmelden toevoegen aan uw domeincontroller door een verificatievraag via code initiëren:
</li>
</ol>

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

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>De startpagina van de app aan te melden gebruikers via een knop maken

Maak een nieuwe weergave voor het toevoegen van de knop aanmelden en gebruikersgegevens na verificatie weergeven in Visual Studio:

1.  Met de rechtermuisknop op de `Views\Home` map en selecteer`Add View`
2.  Noem deze `Index`.
3.  De volgende HTML-code, waaronder de knop aanmelden toevoegen aan het bestand:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Guide</title>
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
### <a name="more-information"></a>Meer informatie
> Deze pagina wordt de knop aanmelden in SVG-indeling met een zwarte achtergrond toegevoegd:<br/>![Aanmelden met Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Voor meer aanmelden knoppen, gaat u naar de [deze pagina](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "richtlijnen huisstijl").
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Toevoegen van een domeincontroller om claims van de gebruiker weer te geven
Deze domeincontroller ziet u het gebruik van de `[Authorize]` kenmerk voor het beveiligen van een domeincontroller. Dit kenmerk wordt toegang tot de controller beperken door alleen geverifieerde gebruikers. De code hieronder maakt gebruik van het kenmerk om claims van de gebruiker die zijn opgehaald als onderdeel van de aanmeldingspagina weer te geven.

1.  Met de rechtermuisknop op de `Controllers` map:`Add` > `Controller`
2.  Selecteer `MVC {version} Controller – Empty`.
3.  Klik op *toevoegen*
4.  Geef deze de naam`ClaimsController`
5.  Vervang de code van de klasse van uw domeincontroller met de code hieronder - Hiermee voegt u de `[Authorize]` kenmerk aan de klasse:

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
        var claimsPrincipalCurrent = System.Security.Claims.ClaimsPrincipal.Current;
        //You get the user’s first and last name below:
        ViewBag.Name = claimsPrincipalCurrent.FindFirst("name").Value;

        // The 'preferred_username' claim can be used for showing the username
        ViewBag.Username = claimsPrincipalCurrent.FindFirst("preferred_username").Value;

        // The subject claim can be used to uniquely identify the user across the web
        ViewBag.Subject = claimsPrincipalCurrent.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier).Value;

        // TenantId is the unique Tenant Id - which represents an organization in Azure AD
        ViewBag.TenantId = claimsPrincipalCurrent.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;

        return View();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie
> Vanwege het gebruik van de `[Authorize]` kenmerk, alle methoden van deze domeincontroller kan alleen worden uitgevoerd als de gebruiker is geverifieerd. Als de gebruiker niet is geverifieerd en probeert te krijgen tot de domeincontroller, wordt de OWIN een verificatievraag initiëren en afdwingen van de gebruiker te verifiëren. De bovenstaande code wordt gekeken naar de verzameling claims van de `ClaimsPrincipal.Current` -exemplaar voor een specifieke gebruikerskenmerken in een token van de gebruiker. Deze kenmerken zijn de volledige naam van de gebruiker en gebruikersnaam, evenals het onderwerp globale gebruiker-id. Bevat ook de *Tenant-ID*, die staat voor de ID voor de organisatie van de gebruiker. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Een weergave voor de claims van de gebruiker maken

Maak een nieuwe weergave voor de claims van de gebruiker in een webpagina in Visual Studio:

1.  Met de rechtermuisknop op de `Views\Claims` map en:`Add View`
2.  Noem deze `Index`.
3.  De volgende HTML-code toevoegen aan het bestand:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Sample</title>
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
