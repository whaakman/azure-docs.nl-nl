---
title: Azure AD v2.0 snelstartgids van ASP.NET Core web-apps | Microsoft Docs
description: Meer informatie over het implementeren van Microsoft-aanmelding op een ASP.NET Core Web-App met behulp van OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4ab3d0b74e8305d67af862020197c69b15221086
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830222"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Aanmelden bij Microsoft toevoegen aan een ASP.NET Core web-app

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

In deze snelstartgids bevat een codevoorbeeld van die u laat zien hoe een ASP.NET Core Web-App kan zich in personal (hotmail.com, live.com of anderen) en werk en schoolaccounts zijn uit een Azure Active Directory-exemplaar.

![Hoe de voorbeeld-app werkt die is gegenereerd door deze snelstart](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.png)


> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Uw toepassing registreren en de app uit de snelstart downloaden
>
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Uw toepassing en codevoorbeeld registreren en configureren
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> 
> 1. Ga naar de [Microsoft-portal voor app-registratie](https://apps.dev.microsoft.com/portal/register-app).
> 1. Voer een naam in voor uw toepassing, zorg ervoor dat de optie voor **Stapsgewijze instelling** is uitgeschakeld en klik op **Maken**.
> 1. Klik op `Add Platform` en selecteer vervolgens `Web`.
> 1. Zorg ervoor dat **Impliciete stroom toestaan** is *ingeschakeld*.
> 1. Voer in **Omleidings-URL's** `http://localhost:3110/` in.
> 1. Schuif omlaag naar de onderkant van de pagina en klik op **Opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Stap 1: Uw toepassing configureren in Azure Portal
> Voor een juiste werking van het codevoorbeeld uit deze snelstart, moet u een antwoord-URL als `http://localhost:3110/` toevoegen.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-aspnet-core-webapp/green-check.png) Uw toepassing is al geconfigureerd met dit kenmerk

#### <a name="step-2-download-your-aspnet-core-project"></a>Stap 2: Uw ASP.NET Core-project downloaden

- [De ASP.NET Core-project downloaden](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Stap 3: Uw project configureren

1. Pak het zip-bestand naar een lokale map dichter naar de hoofdmap - bijvoorbeeld **C:\Azure-Samples**
1. Als u Visual Studio 2017 gebruikt, opent u het project in Visual Studio (optioneel)
1. Bewerken **appsettings.json** en vervang de waarde voor `ClientId` met toepassings-Id van de toepassing die u zojuist hebt geregistreerd:

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "common"
    ```
1. Als uw toepassing is een *toepassing met één tenant* (die zijn gericht op accounts in de huidige map alleen) in uw **appsettings.json** bestand, zoek de waarde voor `TenantId` en vervang `common`met uw **Tenant-Id** of **tenantnaam** (bijvoorbeeld contoso.microsoft.com). U vindt de naam van de tenant op de **overzichtspagina**.

## <a name="more-information"></a>Meer informatie

Deze sectie bevat een overzicht van de code die vereist is voor het aanmelden van gebruikers. Dit is handig om te begrijpen hoe de code werkt, belangrijkste argumenten, en ook als u wilt aanmelden toevoegen aan een bestaande ASP.NET Core-toepassing.

### <a name="startup-class"></a>Opstartklasse

*Microsoft.AspNetCore.Authentication* middleware maakt gebruik van een Opstartklasse die wordt uitgevoerd wanneer het hostproces initialiseert:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddAzureAd(options => Configuration.Bind("AzureAd", options))
    .AddCookie();

    services.AddMvc();
}
```

De methode `AddAuthentication` configureert u de service voor het toevoegen van verificatie op basis van een Cookie - op browser scenario's gebruikt, evenals de uitdaging ingesteld op OpenIdConnect. 

De regel die `.AddAzureAd` wordt de Azure Active Directory-verificatie toegevoegd aan uw toepassing.

Behalve dat het bestand **AzureAdAuthenticationBuilderExtensions.cs** extensie methode toegevoegd aan AzureAd-verificatiepijplijn. Deze extensie-methode configureert de kenmerken die nodig zijn voor Azure AD-verificatie. De `Configure` methode in `IConfigureNamedOptions` interface bevat het volgende:

```csharp
public void Configure(string name, OpenIdConnectOptions options)
{
    options.ClientId = _azureOptions.ClientId;
    options.Authority = $"{_azureOptions.Instance}common/v2.0";   // V2 specific
    options.UseTokenLifetime = true;
    options.RequireHttpsMetadata = false;
    options.TokenValidationParameters.ValidateIssuer = false;     // accept several tenants (here simplified)
}
```
> |Waar  |  |
> |---------|---------|
> |ClientId     |Toepassings-Id van de toepassing geregistreerd in Azure portal|
> |Instantie | De STS-eindpunt voor de gebruiker om te verifiëren. Meestal https://login.microsoftonline.com/{tenant}/v2.0 voor openbare cloud, waarbij {tenant} de naam is van uw tenant, uw tenant-id of *common* voor een verwijzing naar het algemene eindpunt (gebruikt voor toepassingen met meerdere tenants)|
> |UseTokenLifetime |Geeft aan dat de verificatiecookie moet overeenkomen met die van het verificatietoken|
> |RequireHttpsMetadata     |HTTPS voor de metadata-adres of -instantie nodig. Het verdient aanbeveling deze waarde om te wijzigen `True`|
> |TokenValidationParameters     | Een lijst met parameters voor de validatie van tokens. In dit geval `ValidateIssuer` is ingesteld op `false` om aan te geven dat aanmeldingen vanaf een persoonlijke, of werk- of schoolaccount accounts kunnen worden geaccepteerd|

### <a name="initiate-an-authentication-challenge"></a>Verificatievraag initiëren

U kunt afdwingen dat de gebruiker zich aanmelden door aan te vragen een verificatiecontrole in uw controller, net zoals **AccountController.cs**:

```csharp
public IActionResult SignIn()
{
    var redirectUrl = Url.Action(nameof(HomeController.Index), "Home");
    return Challenge(
        new AuthenticationProperties { RedirectUri = redirectUrl },
        OpenIdConnectDefaults.AuthenticationScheme);
}
```

> [!TIP]
> Aanvragen van een verificatiecontrole met behulp van de bovenstaande methode is optioneel en commonsly gebruikt wanneer u een weergave beschikbaar voor geverifieerde en niet-geverifieerde gebruikers wilt. U kunt domeincontrollers beveiligen met behulp van de methode die wordt beschreven in de volgende sectie.

### <a name="protect-a-controller-or-a-controllers-method"></a>Een controller of de methode van een controller beveiligen

U kunt beveiligen met een domeincontroller of de controller methodss met behulp van de `[Authorize]` kenmerk. Dit kenmerk wordt beperkt tot de controller of methoden door toe te staan alleen geverifieerde gebruikers - dit betekent dat verificatiecontrole voor toegang tot de controller als de gebruiker is niet geverifieerd kan worden gestart.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de GitHub-opslagplaats voor deze snelstartgids ASP.NET Core voor meer informatie - en instructies over hoe u verificatie toevoegen aan een nieuwe ASP.NET Core-webtoepassing:

> [!div class="nextstepaction"]
> [Codevoorbeeld voor ASP.NET Core Web-App](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]