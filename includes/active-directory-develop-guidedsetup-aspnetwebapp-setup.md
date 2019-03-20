---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 701b08cafd2e6d38a68252ea5919c8f4cd7de9b4
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203591"
---
## <a name="set-up-your-project"></a>Uw project instellen

In deze sectie bevat de stappen voor het installeren en configureren van de verificatiepijplijn via OWIN-middleware op een ASP.NET-project met behulp van OpenID Connect.

> Voorkeur voor het downloaden van dit voorbeeld Visual Studio-project in plaats daarvan? [Een project hebt gedownload](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) en gaat u naar de [configuratiestap](#register-your-application) het codevoorbeeld configureren voordat u uitvoert.

### <a name="create-your-aspnet-project"></a>Uw ASP.NET-project maken

1. In Visual Studio: `File` > `New` > `Project`
2. Onder *Visual C# \Web*, selecteer `ASP.NET Web Application (.NET Framework)`.
3. Geef uw toepassing en klikt u op *OK*
4. Selecteer `Empty` en schakel het selectievakje in om toe te voegen `MVC` verwijzingen

## <a name="add-authentication-components"></a>Onderdelen voor verificatie toevoegen

1. In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Voeg *NuGet-pakketten voor OWIN-middleware* toe door het volgende te typen in het Package Manager Console-venster:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Over deze bibliotheken
> De bovenstaande bibliotheken maken SSO (eenmalige aanmelding) met behulp van OpenID Connect mogelijk via verificatie op basis van cookies. Nadat de verificatie is voltooid en het token dat de gebruiker vertegenwoordigt, is verzonden naar de toepassing, wordt met OWIN-middleware een sessiecookie gemaakt. De browser vervolgens deze cookie wordt gebruikt bij volgende aanvragen, zodat de gebruiker hoeft niet te Typ nogmaals het wachtwoord en er zijn geen aanvullende verificatie nodig is.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>De verificatiepijplijn configureren
De onderstaande stappen worden gebruikt voor het maken van een OWIN-middleware-Opstartklasse OpenID Connect-verificatie configureren. Deze klasse wordt automatisch uitgevoerd wanneer uw IIS-proces wordt gestart.

> [!TIP]
> Ga als volgt te werk als uw project geen `Startup.cs`-bestand bevat in de hoofdmap:
> 1. Met de rechtermuisknop op de hoofdmap van het project: > `Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Noem deze `Startup.cs`
>
>> Zorg dat de geselecteerde klasse een OWIN-opstartklasse is, en geen C#-standaardklasse. Verzeker u hiervan door te kijken of `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` wordt weergegeven boven de naamruimte.

1. Voeg *OWIN* en *Microsoft.IdentityModel* verwijzingen naar `Startup.cs`:

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

2. Opstartklasse vervangen door de volgende code:

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
                        ValidateIssuer = false
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

<!--start-collapse-->
> ### <a name="more-information"></a>Meer informatie
> De parameters die u opgeeft in *OpenIDConnectAuthenticationOptions*, dienen als coördinaten waarmee de toepassing kan communiceren met Azure AD. Omdat de middleware OpenID Connect maakt gebruik van cookies op de achtergrond, moet u ook het instellen van de cookie-verificatie als de code hierboven wordt weergegeven. De waarde *ValidateIssuer* zorgt ervoor dat de toegang via OpenIdConnect niet wordt beperkt tot één specifieke organisatie.
<!--end-collapse-->
