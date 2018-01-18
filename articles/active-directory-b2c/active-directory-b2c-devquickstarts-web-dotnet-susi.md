---
title: ASP.NET Azure Active Directory B2C-verificatie, aanmelden, wachtwoord opnieuw instellen
description: Het bouwen van een webtoepassing met sign-up-to-date/aanmelden, profiel bewerken en wachtwoord opnieuw instellen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.custom: seohack1
ms.openlocfilehash: ffc46f4348a2ac3cae51c859a24c609756a710fe
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a>Een ASP.NET-web-app maken met Azure Active Directory B2C registreren, aanmelden, profiel bewerken en wachtwoord opnieuw instellen

In deze handleiding ontdekt u hoe u:

> [!div class="checklist"]
> * Azure AD B2C identiteit functies toevoegen aan uw web-app
> * Registreren van uw web-app in uw Azure AD B2C-directory
> * Maak een gebruiker sign-up-to-date/aanmelden, profiel bewerken, en beleid voor uw web-app-wachtwoord opnieuw instellen

## <a name="prerequisites"></a>Vereisten

- U moet uw B2C-Tenant verbinden met een Azure-account. U kunt een gratis Azure-account maken [hier](https://azure.microsoft.com/en-us/).
- U moet [Microsoft Visual Studio](https://www.visualstudio.com/) of een vergelijkbaar programma weergeven en wijzigen van de voorbeeldcode.

## <a name="create-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken

Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor alle gebruikers, apps en groepen. Als u niet een al hebt, maakt u een B2C-directory voordat u in deze handleiding doorgaat.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> U moet verbinding maken met de B2C-Tenant van uw Azure-abonnement. Na het selecteren van **maken**, selecteer de **koppeling een bestaande Azure AD B2C-Tenant met mijn Azure-abonnement** optie en klik vervolgens in de **Azure AD B2C-Tenant** vervolgkeuzelijst, selecteer de tenant die u wilt koppelen.

## <a name="create-and-register-an-application"></a>Het maken en een toepassing registreren

Vervolgens moet u maken en registreren van de app in uw B2C-directory. Dit bevat informatie die Azure AD B2C moet veilig te communiceren met uw app. 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Wanneer u klaar bent, hebt u zowel een API en een systeemeigen toepassing in de instellingen van de toepassing.

## <a name="create-policies-on-your-b2c-tenant"></a>Beleid op uw B2C-tenant maken

In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). Dit codevoorbeeld bevat drie identiteitservaringen: **aanmelding & aanmelden**, **profiel bewerken**, en **wachtwoordherstel**.  U moet één beleidsregel maken voor elk type, zoals wordt beschreven in het [naslagartikel voor beleid](active-directory-b2c-reference-policies.md). Voor elk beleid moet u de weergave naamkenmerk of claim kunt selecteren en kopiëren van de naam van uw beleid voor later gebruik.

### <a name="add-your-identity-providers"></a>Id-providers toevoegen

Selecteer in de instellingen voor **identiteitsproviders** en kiest u registratie van de gebruikersnaam of e-aanmelding.

### <a name="create-a-sign-up-and-sign-in-policy"></a>Maak een beleid voor aanmelden en aanmelden

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a>Maken van een profiel te bewerken van beleid

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a>Maak een beleid voor wachtwoord opnieuw instellen

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

Nadat u uw beleid hebt gemaakt, bent u klaar om uw app te bouwen.

## <a name="download-the-sample-code"></a>De voorbeeldcode downloaden

De code voor deze zelfstudie wordt bewaard in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). U kunt het voorbeeld klonen door de volgende opdracht uit te voeren:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nadat u de voorbeeldcode hebt gedownload, opent u het SLN-bestand in Visual Studio om aan de slag te gaan. Het oplossingsbestand bevat twee projecten: `TaskWebApp` en `TaskService`. `TaskWebApp`is de MVC-webtoepassing die de gebruiker werkt. `TaskService` is de web-API voor de back-end van de app waarin elke takenlijst van de gebruiker wordt opgeslagen. In dit artikel wordt alleen de `TaskWebApp`-toepassing beschreven. Voor meer informatie over het bouwen van `TaskService` met behulp van Azure AD B2C, Zie [onze .NET web api-zelfstudie](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="update-code-to-use-your-tenant-and-policies"></a>Update-code voor het gebruik van uw tenant en -beleid

Ons voorbeeld is geconfigureerd voor gebruik van het beleid en de client-id van onze demo-tenant. Als u wilt verbinden met uw eigen tenant, moet u openen `web.config` in de `TaskWebApp` project en vervang de volgende waarden:

* `ida:Tenant` door de naam van uw tenant
* `ida:ClientId` door de id van uw web-app-toepassing
* `ida:ClientSecret` door de geheime sleutel voor uw web-app
* `ida:SignUpSignInPolicyId` door de naam van uw 'Aanmelden/registreren'-beleid
* `ida:EditProfilePolicyId` door de naam van uw 'Profiel bewerken'-beleid
* `ida:ResetPasswordPolicyId` door de naam van uw 'Wachtwoord opnieuw instellen'-beleid

## <a name="launch-the-app"></a>Start de app
Start de app in Visual Studio uit. Navigeer naar het tabblad takenlijst en noteert u de URl is: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...

Zich registreren voor de app met behulp van de naam van uw e-mailadres of de gebruiker. Meld u af, en vervolgens opnieuw aan te melden en het profiel bewerken of het wachtwoord opnieuw instellen. Meld u af en meld u aan als een andere gebruiker. 

## <a name="add-social-idps"></a>Sociale IDPs toevoegen

De app ondersteunt momenteel alleen de gebruiker zich kunnen registreren en aanmelden met behulp van **lokale accounts**; accounts opgeslagen in uw B2C-directory die gebruikmaken van een gebruikersnaam en wachtwoord. U kunt ondersteuning voor andere toevoegen met behulp van Azure AD B2C **identiteitsproviders** (IDPs) zonder uw code.

Volg de gedetailleerde instructies in deze artikelen wilt sociale IDPs toevoegen aan uw app, eerst. Voor elke IDP die u wilt ondersteunen, moet u een toepassing registreren in dat systeem en het verkrijgen van een client-ID.

* [Facebook ingesteld als een IDP](active-directory-b2c-setup-fb-app.md)
* [Google ingesteld als een IDP](active-directory-b2c-setup-goog-app.md)
* [Amazon ingesteld als een IDP](active-directory-b2c-setup-amzn-app.md)
* [LinkedIn instellen als een IDP](active-directory-b2c-setup-li-app.md)

Nadat u de id-providers aan uw B2C-directory toevoegen, bewerk elk van de drie beleidsregels om op te nemen van de nieuwe IDPs, zoals beschreven de [naslagartikel](active-directory-b2c-reference-policies.md). Nadat u uw beleid opslaat, voer de app opnieuw.  U ziet nu de nieuwe IDPs toegevoegd als aanmelden en aanmeldingsopties in elk van uw identiteit ervaringen.

U kunt experimenteren met het beleid en bekijk het effect op uw voorbeeld-app. Toevoegen of verwijderen van IDPs, toepassingsclaims bewerken of registratiekenmerken wijzigen. Experiment totdat u kunt zien hoe beleidsregels, verificatieaanvragen en OWIN met elkaar verbinden.

## <a name="sample-code-walkthrough"></a>Voorbeeld code-overzicht
De volgende secties ziet u hoe de voorbeeldcode van de toepassing is geconfigureerd. U kunt dit als richtlijn bij het ontwikkelen van toekomstige app.

### <a name="add-authentication-support"></a>Toevoegen van ondersteuning voor verificatie

U kunt nu uw app voor het gebruik van Azure AD B2C configureren. Uw app communiceert met Azure AD B2C door te sturen verificatieaanvragen OpenID Connect. De ervaring van de gebruiker die de app wil uitvoeren door te geven van het beleid wordt bepaald door de aanvragen. OWIN-bibliotheek van Microsoft kunt u deze aanvragen verzenden, het beleid uitvoeren, het beheren van gebruikerssessies en meer.

#### <a name="install-owin"></a>OWIN installeren

Om te beginnen, moet u de OWIN middleware NuGet pakketten toevoegen aan het project met behulp van de Visual Studio Package Manager-Console.

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a>Een OWIN-opstartklasse toevoegen

Voeg aan de API een OWIN-opstartklasse toe met de naam `Startup.cs`.  Klik met de rechtermuisknop op het project, selecteer achtereenvolgens **Toevoegen** en **Nieuw item** en zoek naar OWIN. De OWIN-middleware roept de `Configuration(…)`-methode aan als uw app wordt gestart.

In ons voorbeeld hebben we de klasseverklaring gewijzigd in `public partial class Startup` en het andere deel van de klasse geïmplementeerd in `App_Start\Startup.Auth.cs`. In de `Configuration`-methode hebben we een aanroep toegevoegd aan `ConfigureAuth`, die is gedefinieerd in `Startup.Auth.cs`. Na de wijzigingen ziet `Startup.cs` er als volgt uit:

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-the-authentication-middleware"></a>De verificatiemiddleware configureren

Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(...)` methode. De parameters die u opgeeft in `OpenIdConnectAuthenticationOptions` fungeren als coördinaten voor uw app om te communiceren met Azure AD B2C. Als u bepaalde parameters niet opgeeft, wordt de standaardwaarde gebruikt. Bijvoorbeeld: we niet opgeven de `ResponseType` in het voorbeeld zodat de standaardwaarde `code id_token` in elke uitgaande aanvraag aan Azure AD B2C wordt gebruikt.

U moet ook Cookieverificatie instellen. De middleware OpenID Connect maakt gebruik van cookies gebruikerssessies, onder andere onderhouden.

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure the OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate the metadata address using the tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify the callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify the claims to validate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement the "Notification" methods...
}
```

In `OpenIdConnectAuthenticationOptions` , definiëren we een set callback-functies voor specifieke meldingen die worden ontvangen door de middleware OpenID Connect. Deze problemen zijn gedefinieerd met behulp van een `OpenIdConnectAuthenticationNotifications` object en opgeslagen in de `Notifications` variabele. In ons voorbeeld definiëren we drie verschillende retouraanroepen, afhankelijk van de gebeurtenis.

### <a name="using-different-policies"></a>Met behulp van verschillende beleidsregels

De `RedirectToIdentityProvider` melding wordt geactiveerd wanneer een aanvraag wordt gedaan aan Azure AD B2C. In de functie voor retouraanroepen `OnRedirectToIdentityProvider`, we controleren in de uitgaande oproep als we wilt gebruiken een ander beleid. Om te kunnen doen wachtwoordherstel of een profiel te bewerken, moet u gebruikmaken van het bijbehorende beleid, zoals het wachtwoord opnieuw instellen van beleid in plaats van het standaardbeleid voor "Registreren of aanmelden".

In ons voorbeeld wanneer een gebruiker wil het wachtwoord opnieuw instellen of het profiel bewerken toevoegen we het beleid dat we de voorkeur geeft aan in de context OWIN. Dat kan worden gedaan door het volgende te doen:

```CSharp
    // Let the middleware know you are trying to use the edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

En u kunt de functie voor retouraanroepen implementeren `OnRedirectToIdentityProvider` door het volgende te doen:

```CSharp
/*
*  On each call to Azure AD B2C, check if a policy (e.g. the profile edit or password reset policy) has been specified in the OWIN context.
*  If so, use that policy when making the call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a>Afhandeling van autorisatiecodes

De `AuthorizationCodeReceived` melding wordt geactiveerd wanneer een autorisatiecode wordt ontvangen. Het OpenID Connect middleware biedt geen ondersteuning voor uitwisselen codes voor toegangstokens. U kunt de code voor het token in een callback-functie handmatig uitwisselen. Voor meer informatie, Zie de [documentatie](active-directory-b2c-devquickstarts-web-api-dotnet.md) waarin wordt uitgelegd hoe.

### <a name="handling-errors"></a>Foutafhandeling

De `AuthenticationFailed` melding wordt geactiveerd wanneer de verificatie is mislukt. In de callback-methode kunt u de fouten verwerken als u wilt. U moet echter een controle voor de foutcode toevoegen `AADB2C90118`. Tijdens het uitvoeren van het beleid 'Registreren of aanmelden', de gebruiker heeft de mogelijkheid om te selecteren een **wachtwoord vergeten?** koppeling. In dit geval stuurt Azure AD B2C uw app die fout die aangeeft dat uw app moet worden gebruikt voor het aanbrengen van een aanvraag met het beleid voor wachtwoordherstel in plaats daarvan.

```CSharp
/*
* Catch any failures received by the authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle the error code that Azure AD B2C throws when trying to reset a password from the login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-to-azure-ad"></a>Verificatieaanvragen verzenden naar Azure AD

Uw app is nu geconfigureerd voor communicatie met Azure AD B2C met behulp van het OpenID Connect-verificatieprotocol. OWIN beheert de details van verificatieberichten, het valideren van tokens van Azure AD B2C en het onderhoud van de gebruikerssessie. Alles wat u hoeft alleen nog stroom voor elke gebruiker te initiëren.

Wanneer een gebruiker selecteert **Sign up/aanmelden**, **profiel bewerken**, of **wachtwoord opnieuw instellen** in de web-app, de bijbehorende actie is aangeroepen `Controllers\AccountController.cs`:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign up or sign in
*/
public void SignUpSignIn()
{
    // Use the default policy to process the sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting to edit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let the middleware know you are trying to use the edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set the page to redirect to after editing the profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting to reset a password
*/
public void ResetPassword()
{
    // Let the middleware know you are trying to use the reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set the page to redirect to after changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

U kunt ook OWIN gebruiken voor het ondertekenen van de gebruiker uit de app. In `Controllers\AccountController.cs` we hebben:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign out
*/
public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

Naast het expliciet aanroepen van een beleid, kunt u een `[Authorize]` code in uw domeincontrollers die een beleid wordt uitgevoerd als de gebruiker niet aangemeld. Open `Controllers\HomeController.cs` en voeg de `[Authorize]` tag met de claims-controller.  OWIN selecteert het laatste beleid geconfigureerd wanneer de `[Authorize]` tag is bereikt.

```CSharp
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a>Gebruikersgegevens weergeven

Als u gebruikers verifiëren met behulp van OpenID Connect, Azure AD B2C een token ID terug naar de app die bevat **claims**. Dit zijn asserties over de gebruiker. U kunt de claims kunt gebruiken voor het aanpassen van uw app.

Open het `Controllers\HomeController.cs`-bestand. U hebt toegang tot gebruikersclaims in uw domeincontrollers via de `ClaimsPrincipal.Current` SPN-object.

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

U kunt een claim die uw toepassing op dezelfde manier krijgt openen.  Een lijst van alle claims de app ontvangt is beschikbaar voor u op de **Claims** pagina.
