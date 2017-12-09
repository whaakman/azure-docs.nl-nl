
## <a name="set-up-your-project"></a>Instellen van uw project

Deze sectie bevat de stappen voor het installeren en configureren van de verificatiepijplijn via OWIN middleware op een ASP.NET-project met OpenID Connect. 

> Voorkeur voor het downloaden van dit voorbeeld Visual Studio-project in plaats daarvan? [Downloaden van een project](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) en doorgaan met de [configuratiestap](#create-an-application-express) voor het configureren van het codevoorbeeld voordat wordt uitgevoerd.

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a>Uw ASP.NET-project maken

> 1. In Visual Studio:`File` > `New` > `Project`<br/>
> 2. Onder *Visual C# \Web*, selecteer `ASP.NET Web Application (.NET Framework)`.
> 3. Naam van uw toepassing en klik op *OK*
> 4. Selecteer `Empty` en schakel het selectievakje om toe te voegen `MVC` verwijzingen
<!--end-collapse-->

## <a name="add-authentication-components"></a>Verificatieonderdelen toevoegen

1. In Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Voeg *OWIN middleware NuGet-pakketten* door het volgende te typen in het venster Package Manager-Console:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Over deze bibliotheken

>De bovenstaande bibliotheken Schakel eenmalige aanmelding (SSO) met OpenID Connect via authenticatie op basis van een cookie. Nadat de verificatie is voltooid en het token voor de gebruiker wordt verzonden naar uw toepassing, maakt OWIN middleware een sessiecookie. De browser gebruikt deze cookie vervolgens bij volgende aanvragen, zodat de gebruiker hoeft niet hun wachtwoord opnieuw op te geven en geen aanvullende verificatie nodig is.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>De verificatiepijplijn configureren
De onderstaande stappen worden gebruikt voor het maken van een middleware OWIN-Opstartklasse OpenID Connect verificatie configureren. Deze klasse wordt automatisch uitgevoerd wanneer uw IIS-proces wordt gestart.

> Als uw project beschikt niet over een `Startup.cs` bestand in de hoofdmap:<br/>
> 1. Klik met de rechtermuisknop op de hoofdmap van het project: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Geef deze de naam`Startup.cs`

> Zorg ervoor dat de geselecteerde klasse is een OWIN-Opstartklasse en niet een standaard C#-klasse. Dit controleren door te controleren of er `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` boven de naamruimte.


1. Voeg *OWIN* en *Microsoft.IdentityModel* verwijzingen naar `Startup.cs`:

```csharp
using Microsoft.Owin;
using Owin;
using Microsoft.IdentityModel.Protocols;
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
using Microsoft.Owin.Security.Notifications;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Opstartklasse vervangen door de volgende code:
</li>
</ol>

```csharp
public class Startup
{        
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is the URL where the user will be redirected to after they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is the URL for authority, composed by Azure Active Directory v2 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
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
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
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
<!--start-collapse-->
> ### <a name="more-information"></a>Meer informatie

> De parameters die u opgeeft in *OpenIDConnectAuthenticationOptions* fungeren als coördinaten voor de toepassing om te communiceren met Azure AD. Omdat het OpenID Connect middleware gebruikmaakt van cookies op de achtergrond, moet u ook Cookieverificatie instellen als de code hierboven wordt weergegeven. De *ValidateIssuer* waarde vertelt OpenIdConnect niet toegang te beperken tot een specifieke organisatie.
<!--end-collapse-->

