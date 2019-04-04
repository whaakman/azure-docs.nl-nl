---
title: Snelstart Azure AD v2.0 ASP.NET Core-web-app | Microsoft Docs
description: Informatie over het implementeren van Microsoft-aanmelding in een ASP.NET Core-web-app met behulp van OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/03/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0fa2e38a680e8590a89131717136a7960c1d3680
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58903587"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Snelstart: aanmelding met Microsoft toevoegen aan een ASP.NET Core-web-app

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

In deze snelstart leert u hoe een ASP.NET Core-web-app persoonlijke accounts (hotmail.com, outlook.com, anderen) en werk- en schoolaccounts kan aanmelden vanuit een willekeurig exemplaar van Azure Active Directory (Azure AD).

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze Quick Start werkt](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro-updated.png)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Ga naar de [Azure portal - App-registraties](https://aka.ms/aspnetcore2-1-aad-quickstart-v2).
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
> 1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
> 1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina.
> 1. Selecteer **registratie van nieuwe**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>    - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `AspNetCore-Quickstart`.
>    - Bij **Antwoord-URL** voegt u `https://localhost:44321/` toe en selecteert u **Registreren**.
> 1. Selecteer het menu **Verificatie** en voeg dan de volgende gegevens toe:
>    - Bij **Antwoord-URL** voegt u `https://localhost:44321/signin-oidc` toe en selecteert u **Registreren**.
>    - Bij **Geavanceerde instellingen** stelt u de **afmeldings-URL** in op `https://localhost:44321/signout-oidc`.
>    - Bij **Impliciete toekenning** controleert u de **id-tokens**.
>    - Selecteer **Opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: uw toepassing configureren in Azure Portal
> Als u het codevoorbeeld uit deze snelstart wilt gebruiken, moet u antwoord-URL's toevoegen als `https://localhost:44321/` en `https://localhost:44321/signin-oidc`, voegt u de afmeldings-URL toe als `https://localhost:44321/signout-oidc` en vraagt u de id-tokens aan die moeten worden uitgegeven door het autorisatie-eindpunt.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging hebt aangebracht voor mij]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-aspnet-webapp/green-check.png) uw toepassing is geconfigureerd met deze kenmerken.

#### <a name="step-2-download-your-aspnet-core-project"></a>Stap 2: uw ASP.NET Core-project downloaden

- [De oplossing voor Visual Studio 2017 downloaden](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Stap 3: uw Visual Studio-project configureren

1. Pak het zip-bestand uit in een lokale map in de hoofdmap (bijvoorbeeld **C:\Azure-Samples**)
1. Als u Visual Studio 2017 gebruikt, opent u de oplossing in Visual Studio (optioneel).
1. Bewerk het bestand **appsettings.json**. Zoek `ClientId` en werk de waarde van `ClientId` met de **(client) toepassings-ID** waarde van de toepassing die u zojuist hebt geregistreerd. 

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "Enter_the_Tenant_Info_Here"
    ```

> [!div renderon="docs"]
> Waar:
> - `Enter_the_Application_Id_here` -is de **(client) toepassings-ID** voor de toepassing die u in de Azure-portal hebt geregistreerd. U vindt de **toepassings-id (client-id)** op de pagina **Overzicht** van de app.
> - `Enter_the_Tenant_Info_Here` -is een van de volgende opties:
>   - Als uw toepassing **Alleen accounts in deze organisatiemap** ondersteunt, vervangt u deze waarde door de **tenant-id** of **tenantnaam** (bijvoorbeeld contoso.microsoft.com)
>   - Als uw toepassing ondersteunt **Accounts in een organisatie-map**, vervang deze waarde met `organizations`
>   - Als uw toepassing ondersteunt **alle Microsoft-accountgebruikers**, vervang deze waarde met `common`
>
> > [!TIP]
> > Om de waarden van **Toepassings-id (client-id)**, **Map-id (tenant-id)** en **Ondersteunde accounttypen** te achterhalen, gaat u naar de **Overzichtspagina** van de app in de Azure-portal.

## <a name="more-information"></a>Meer informatie

Deze sectie bevat een overzicht van de code die vereist is voor het aanmelden van gebruikers. Dit kan handig zijn om te begrijpen hoe de code werkt, wat de belangrijkste argumenten zijn en ook als u aanmelding wilt toevoegen aan een bestaande ASP.NET Core-toepassing.

### <a name="startup-class"></a>Opstartklasse

De *Microsoft.AspNetCore.Authentication*-middleware maakt gebruik van een opstartklasse die wordt uitgevoerd wanneer in het hostingproces het volgende wordt geïnitialiseerd:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  services.Configure<CookiePolicyOptions>(options =>
  {
    // This lambda determines whether user consent for non-essential cookies is needed for a given request.
    options.CheckConsentNeeded = context => true;
    options.MinimumSameSitePolicy = SameSiteMode.None;
  });

  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
          .AddAzureAD(options => Configuration.Bind("AzureAd", options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
    options.Authority = options.Authority + "/v2.0/";         // Azure AD v2.0

    options.TokenValidationParameters.ValidateIssuer = false; // accept several tenants (here simplified)
  });

  services.AddMvc(options =>
  {
     var policy = new AuthorizationPolicyBuilder()
                     .RequireAuthenticatedUser()
                     .Build();
     options.Filters.Add(new AuthorizeFilter(policy));
  })
  .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
}
```

Met de methode `AddAuthentication` wordt de service geconfigureerd voor het toevoegen van verificatie op basis van cookies. Deze verificatie wordt gebruikt in browserscenario's én om vragen te sturen naar OpenID Connect. 

Met de regel die `.AddAzureAd` bevat, wordt de Azure AD-verificatie toegevoegd aan uw toepassing. Deze wordt dan geconfigureerd voor aanmelding met het Azure AD v2.0-eindpunt.

> |Waar  |  |
> |---------|---------|
> | ClientId  | Toepassings-id (client-id) van de toepassing die is geregistreerd in de Azure-portal. |
> | Instantie | Het STS-eindpunt voor gebruikersverificatie. Meestal is dit <https://login.microsoftonline.com/{tenant}/v2.0> voor openbare cloud, waarbij {tenant} de naam is van uw tenant, uw tenant-id of *common* voor een verwijzing naar het algemene eindpunt (gebruikt voor toepassingen met meerdere tenants) |
> | TokenValidationParameters | Een lijst met parameters voor de validatie van tokens. In dit geval is `ValidateIssuer` ingesteld op `false` om aan te geven dat aanmeldingen vanaf persoonlijke, werk- of schoolaccounts kunnen worden geaccepteerd. |

### <a name="protect-a-controller-or-a-controllers-method"></a>Een controller of de methode van een controller beveiligen

U kunt een controller of controllermethoden beveiligen met behulp van het kenmerk `[Authorize]`. Met dit kenmerk wordt toegang tot de controller of methoden beperkt door alleen geverifieerde gebruikers toe te staan. Dit betekent dat de verificatiecontrole kan worden gestart om de controller te benaderen als de gebruiker niet is geverifieerd.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de GitHub-opslagplaats van deze ASP.NET Core-quickstart voor meer informatie, inclusief instructies over het toevoegen van verificatie aan een gloednieuwe ASP.NET Core-webtoepassing:

> [!div class="nextstepaction"]
> [Codevoorbeeld voor ASP.NET Core Web-App](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

