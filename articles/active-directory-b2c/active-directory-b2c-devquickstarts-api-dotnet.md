---
title: Azure AD B2C | Microsoft Docs
description: Een .NET-web-API maken met Azure Active Directory B2C, beveiligd met OAuth 2.0-toegangstokens voor verificatie.
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/22/2016
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: a977cb509fb64d7c986e2e0f7e2b5e4e3e45dec0
ms.openlocfilehash: afc0249def57095e58e4d57230faee01daf7bb52


---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: een .NET-web-API maken
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Met Azure Active Directory (Azure AD) B2C kunt u een web-API beveiligen met OAuth 2.0-toegangstokens. Met deze tokens kunnen client-apps die gebruikmaken van Azure AD B2C, worden geverifieerd bij de API. In dit artikel wordt uitgelegd hoe u een .NET-Model-View-Controller (MVC)-API voor een 'takenlijst' maakt, waarmee gebruikers taken kunnen maken, ophalen, bijwerken en verwijderen. De web-API is beveiligd met Azure AD B2C en biedt geverifieerde gebruikers alleen de mogelijkheid om hun takenlijst te beheren.

## <a name="create-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken
Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor alle gebruikers, apps, groepen en meer. Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u doorgaat in deze handleiding.

## <a name="create-an-application"></a>Een app maken
Vervolgens maakt u een app in uw B2C-directory. Hiermee geeft u informatie door aan Azure AD die nodig is om veilig te communiceren met uw app. Volg [deze instructies](active-directory-b2c-app-registration.md) om een app te maken. Zorg ervoor dat:

* U een **web-app** of **web-API** in de toepassing opneemt.
* U de **URI voor omleiden** `https://localhost:44316/` gebruikt voor de web-app. Dit is de standaardlocatie van de web-app-client voor dit codevoorbeeld.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U hebt deze later nodig.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Het beleid maken
In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). De client in dit codevoorbeeld bevat drie identiteitservaringen: registreren, aanmelden en profiel bewerken. U moet een beleid maken voor elk type, zoals wordt beschreven in het [naslagartikel voor beleid](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Wanneer u uw drie beleidsregels maakt:

* Kiest u **Registratie met gebruikers-id** of **Registratie via e-mail** op de blade met identiteitsproviders.
* Kiest u **Weergavenaam** en andere registratiekenmerken in het registratiebeleid.
* Kiest u **Weergavenaam**- en **Object-id**-claims als toepassingsclaims voor elk beleid. U kunt ook andere claims kiezen.
* Kopieert u de **Naam** van elk beleid nadat u dit hebt gemaakt. U hebt deze beleidsnamen later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, kunt u uw app maken.

## <a name="download-the-code"></a>De code downloaden
De code voor deze zelfstudie [wordt bewaard in GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Als u het voorbeeld wilt maken terwijl u aan het werk bent, kunt u [een basisproject downloaden als zip-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). U kunt het basisproject ook klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

De voltooide app is ook [beschikbaar als zip-bestand](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) of in de `complete`-vertakking van dezelfde opslagplaats.

Nadat u de voorbeeldcode hebt gedownload, opent u het SLN-bestand in Visual Studio om aan de slag te gaan. Het oplossingsbestand bevat twee projecten: `TaskWebApp` en `TaskService`. `TaskWebApp` is een MVC-webtoepassing waarmee de gebruiker werkt. `TaskService` is de web-API voor de back-end van de app waarin elke takenlijst van de gebruiker wordt opgeslagen.

## <a name="configure-the-task-web-app"></a>De web-app voor taken configureren
Wanneer een gebruiker met `TaskWebApp` werkt, verzendt de client aanvragen naar Azure AD en worden tokens teruggestuurd waarmee de web-API `TaskService` kan worden aangeroepen. Als u de gebruiker wilt aanmelden en tokens wilt ophalen, moet u bepaalde informatie over uw app opgeven voor `TaskWebApp`. Open in het project `TaskWebApp` het bestand `web.config` in de hoofdmap van het project en vervang de waarden in de sectie `<appSettings>`.  De waarden `AadInstance`, `RedirectUri` en `TaskServiceUrl` kunt u ongewijzigd laten.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

In dit artikel wordt het bouwen van de `TaskWebApp`-client niet besproken.  Zie [onze zelfstudie voor .NET-web-apps](active-directory-b2c-devquickstarts-web-dotnet.md) voor meer informatie over het bouwen van een web-app met Azure AD B2C.

## <a name="secure-the-api"></a>De API beveiligen
Wanneer u een client hebt die de API aanroept namens gebruikers, kunt u `TaskService` beveiligen met Bearer-tokens van OAuth 2.0. De API kan tokens accepteren en valideren met behulp van de Microsoft Open Web Interface voor .NET-bibliotheek (OWIN).

### <a name="install-owin"></a>OWIN installeren
Installeer eerst de OWIN OAuth-verificatiepijplijn:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Uw B2C-gegevens invoeren
Open het bestand `web.config` in de hoofdmap van het `TaskService`-project en vervang de waarden in de sectie `<appSettings>`. Deze waarden worden gebruikt in de API- en OWIN-bibliotheek.  U kunt de waarde `AadInstance` ongewijzigd laten.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Een OWIN-opstartklasse toevoegen
Voeg een OWIN-opstartklasse toe aan het `TaskService`-project met de naam `Startup.cs`.  Klik met de rechtermuisknop op het project, selecteer achtereenvolgens **Toevoegen** en **Nieuw item** en zoek naar OWIN.

```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0-verificatie configureren
Open het bestand `App_Start\Startup.Auth.cs` en implementeer de `ConfigureAuth(...)`-methode:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>De taakcontroller beveiligen
Nadat de app is geconfigureerd voor het gebruik van OAuth 2.0-verificatie, kunt u uw web-API beveiligen door een `[Authorize]`-tag toe te voegen aan de taakcontroller. Dit is de controller waarop alle bewerkingen van de takenlijst worden uitgevoerd, dus moet u de hele controller beveiligen op klasseniveau. U kunt ook de `[Authorize]`-tag toevoegen aan afzonderlijke acties voor nauwkeuriger beheer.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Gebruikersgegevens ophalen uit het token
`TasksController` slaat taken in een database op waarin elke taak een gekoppelde gebruiker heeft die 'eigenaar' is van de taak. De eigenaar wordt geïdentificeerd met de **object-id** van de gebruiker. (Daarom moet u de object-id toevoegen als toepassingsclaim in al uw beleidsregels.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren
Bouw ten slotte `TaskWebApp` en `TaskService` en voer deze uit. Registreer u bij de app met een e-mailadres of gebruikersnaam. Maak een paar taken in de takenlijst van de gebruiker en bekijk hoe deze zelfs bewaard blijven in de API nadat u de client hebt gestopt en opnieuw hebt gestart.

## <a name="edit-your-policies"></a>Het beleid bewerken
Nadat u een API hebt beveiligd met behulp van Azure AD B2C, kunt u experimenteren met het beleid voor de app en de effecten hiervan (of het gebrek daaraan) op de API bekijken. U kunt de toepassingsclaims in het beleid bewerken en de beschikbare gebruikersgegevens in de web-API wijzigen. Claims die u toevoegt, zijn beschikbaar in uw .NET MVC-web-API in het `ClaimsPrincipal`-object, zoals eerder in dit artikel is beschreven.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->



<!--HONumber=Nov16_HO2-->


