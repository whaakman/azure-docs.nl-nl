---
title: Een web-API - ASP.NET beveiligen - Azure Active Directory B2C | Microsoft Docs
description: Een .NET-web-API maken met Azure Active Directory B2C, beveiligd met OAuth 2.0-toegangstokens voor verificatie.
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 01/14/2018
ms.author: parakhj
ms.custom: seohack1
ms.openlocfilehash: 68c0f50e20b5a1a44ff2cb1bc5df35d60928b911
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: een .NET-web-API maken

Met Azure Active Directory (Azure AD) B2C kunt u een web-API beveiligen met OAuth 2.0-toegangstokens. Met deze tokens kunnen client-apps worden geverifieerd bij de API. In dit artikel wordt uitgelegd hoe u een .NET MVC-API voor een takenlijst maakt, waarmee gebruikers van uw clienttoepassing taken kunnen maken, ophalen, bijwerken en verwijderen. De web-API is beveiligd met Azure AD B2C en biedt geverifieerde gebruikers alleen de mogelijkheid om hun takenlijst te beheren.

## <a name="create-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken

Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken. Een directory is een container voor alle gebruikers, apps, groepen en meer. Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u doorgaat in deze handleiding.

> [!NOTE]
> De clienttoepassing en web-API moeten dezelfde Azure AD B2C-directory gebruiken.
>

## <a name="create-a-web-api"></a>Een web-API maken

Vervolgens maakt u een web-API-app in uw B2C-directory. Hiermee geeft u informatie door aan Azure AD die nodig is om veilig te communiceren met uw app. Volg [deze instructies](active-directory-b2c-app-registration.md) om een app te maken. Zorg ervoor dat:

* U een **web-app** of **web-API** in de toepassing opneemt.
* Gebruik de **Omleidings-URI** `https://localhost:44332/` voor de web-app. Dit is de standaardlocatie van de web-app-client voor dit codevoorbeeld.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U hebt deze later nodig.
* Voer een app-id **URI voor de app-id** in. Kopieer de volledige **URI voor de app-id**. U hebt deze later nodig.
* Voeg machtigingen toe via het menu **Gepubliceerde scopes**.

## <a name="create-your-policies"></a>Het beleid maken

In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). U moet een beleid maken om te communiceren met Azure AD B2C. U doet er verstandig aan het gecombineerde beleid 'Aanmelden/registreren' te gebruiken, zoals beschreven in het [artikel met referenties naar beleid](active-directory-b2c-reference-policies.md). Wanneer u het beleid maakt, vergeet dan niet het volgende:

* Kies een **Weergavenaam** en andere registratiekenmerken in uw beleid.
* Kiest u **Weergavenaam**- en **Object-id**-claims als toepassingsclaims voor elk beleid. U kunt ook andere claims kiezen.
* Kopieert u de **naam** van elk beleid nadat u dit hebt gemaakt. U hebt de beleidsnaam later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u het beleid hebt gemaakt, kunt u uw app maken.

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie wordt bewaard in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). U kunt het voorbeeld klonen door de volgende opdracht uit te voeren:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Nadat u de voorbeeldcode hebt gedownload, opent u het SLN-bestand in Visual Studio om aan de slag te gaan. Het oplossingsbestand bevat twee projecten: `TaskWebApp` en `TaskService`. `TaskWebApp` is een MVC-webtoepassing waarmee de gebruiker werkt. `TaskService` is de web-API voor de back-end van de app waarin elke takenlijst van de gebruiker wordt opgeslagen. In dit artikel wordt alleen de `TaskService`-toepassing beschreven. Zie voor meer informatie over het bouwen van `TaskWebApp` met Azure AD B2C [onze zelfstudie voor .NET-web-apps](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>De Azure AD B2C-configuratie bijwerken

Ons voorbeeld is geconfigureerd voor gebruik van het beleid en de client-id van onze demo-tenant. Als u uw eigen tenant wilt gebruiken, moet u het volgende doen:

1. Open `web.config` in het `TaskService`-project en vervang de waarden voor
    * `ida:Tenant` door de naam van uw tenant
    * `ida:ClientId` door de id van uw web-API-toepassing
    * `ida:SignUpSignInPolicyId` door de naam van uw 'Aanmelden/registreren'-beleid

2. Open `web.config` in het `TaskWebApp`-project en vervang de waarden voor
    * `ida:Tenant` door de naam van uw tenant
    * `ida:ClientId` door de id van uw web-app-toepassing
    * `ida:ClientSecret` door de geheime sleutel voor uw web-app
    * `ida:SignUpSignInPolicyId` door de naam van uw 'Aanmelden/registreren'-beleid
    * `ida:EditProfilePolicyId` door de naam van uw 'Profiel bewerken'-beleid
    * `ida:ResetPasswordPolicyId` door de naam van uw 'Wachtwoord opnieuw instellen'-beleid
    * `api:ApiIdentifier` door de URI voor de app-id


## <a name="secure-the-api"></a>De API beveiligen

Wanneer u een client hebt die de API aanroept, kunt u uw API (bijv. `TaskService`) beveiligen met Bearer-tokens van OAuth 2.0. Dit zorgt ervoor dat elke aanvraag voor uw API pas geldig wordt als de aanvraag een Bearer-token heeft. De API kan Bearer-tokens accepteren en valideren met behulp van de Microsoft Open Web Interface voor .NET-bibliotheek (OWIN).

### <a name="install-owin"></a>OWIN installeren

Begin met het installeren van de OWIN OAuth-verificatiepijplijn met behulp van de Visual Studio Package Manager Console.

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

Hiermee installeert u de OWIN-middleware die Bearer-tokens accepteert en valideert.

### <a name="add-an-owin-startup-class"></a>Een OWIN-opstartklasse toevoegen

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

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0-verificatie configureren

Open het bestand `App_Start\Startup.Auth.cs` en implementeer de `ConfigureAuth(...)`-methode. Het kan er bijvoorbeeld als volgt uitzien:

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure the authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where the audience of the token is equal to the client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-the-task-controller"></a>De taakcontroller beveiligen

Nadat de app is geconfigureerd voor het gebruik van OAuth 2.0-verificatie, kunt u uw web-API beveiligen door een `[Authorize]`-tag toe te voegen aan de taakcontroller. Dit is de controller waarop alle bewerkingen van de takenlijst worden uitgevoerd, dus moet u de hele controller beveiligen op klasseniveau. U kunt ook de `[Authorize]`-tag toevoegen aan afzonderlijke acties voor nauwkeuriger beheer.

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Gebruikersgegevens ophalen uit het token

`TasksController` slaat taken in een database op waarin elke taak een gekoppelde gebruiker heeft die 'eigenaar' is van de taak. De eigenaar wordt geïdentificeerd met de **object-id** van de gebruiker. (Daarom moet u de object-id toevoegen als toepassingsclaim in al uw beleidsregels.)

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-the-permissions-in-the-token"></a>De machtigingen in het token valideren

Een algemene vereiste voor web-API's is het valideren van de in het token aanwezige 'scopes'. Dit zorgt ervoor dat de gebruiker heeft ingestemd met de machtigingen die vereist zijn voor toegang tot de takenlijstservice.

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "The Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Bouw ten slotte `TaskWebApp` en `TaskService` en voer deze uit. Maak een paar taken in de takenlijst van de gebruiker en bekijk hoe deze zelfs bewaard blijven in de API nadat u de client hebt gestopt en opnieuw hebt gestart.

## <a name="edit-your-policies"></a>Het beleid bewerken

Nadat u een API hebt beveiligd met behulp van Azure AD B2C, kunt u experimenteren met het beleid 'Registreren/aanmelden' voor de app en de effecten hiervan (of het gebrek daaraan) op de API bekijken. U kunt de toepassingsclaims in het beleid bewerken en de beschikbare gebruikersgegevens in de web-API wijzigen. Claims die u toevoegt, zijn beschikbaar in uw .NET MVC-web-API in het `ClaimsPrincipal`-object, zoals eerder in dit artikel is beschreven.
