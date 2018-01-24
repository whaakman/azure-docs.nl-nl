---
title: Aanmelden voor een .NET MVC-web-API met behulp van het Azure AD v2.0-eindpunt toevoegen | Microsoft Docs
description: Het bouwen van een .NET MVC Web-Api die tokens van beide persoonlijke Microsoft-Account accepteert en werk- of schoolaccount accounts.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: e77bc4e0-d0c9-4075-a3f6-769e2c810206
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 65f25e2496065ca1aaba443a9d6b3e29239e0218
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="secure-an-mvc-web-api"></a>Een MVC-web-API beveiligen
U kunt met Azure Active Directory het v2.0-eindpunt, beveiligen een Web-API met [OAuth 2.0](active-directory-v2-protocols.md) tokens, waardoor gebruikers met beide persoonlijke Microsoft-account openen en werk- of schoolaccount gebruikersaccounts aan veilige toegang tot uw Web-API.

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

In ASP.NET web-API's, kunt u dit doen met behulp van Microsoft OWIN middleware is opgenomen in .NET Framework 4.5.  We gebruiken hier OWIN voor het bouwen van een 'Takenlijst' MVC Web-API waarmee clients kunnen maken en taken te lezen uit de takenlijst van de gebruiker.  De web-API valideert dat inkomende aanvragen een ongeldig toegangstoken bevatten en alle aanvragen die niet zijn gevalideerd op een beveiligde route afwijzen.  Dit voorbeeld is gebouwd met behulp van Visual Studio 2015.

## <a name="download"></a>Downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Als u wilt volgen, kunt u [basis van de app downloaden als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) of het geraamte:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

De geraamte app omvat alle standaardcode voor een eenvoudige API, maar alle benodigde onderdelen identiteitsgerelateerde ontbreekt. Als u niet volgen wilt, kunt u in plaats daarvan klonen of [het voltooide voorbeeld downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Een app registreren
Maakt een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of als volgt [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

* Noteer de **toepassings-Id** toegewezen aan uw app, moet u deze snel.

Deze visual studio-oplossing bevat ook een 'TodoListClient', dit een eenvoudige WPF-app is.  De TodoListClient wordt gebruikt om u te laten zien hoe een gebruiker zich aanmeldt en hoe een client aanvragen kunt geven tot uw Web-API.  In dit geval worden zowel de TodoListClient en de TodoListService vertegenwoordigd door dezelfde app.  Als u wilt de TodoListClient configureren, moet u ook:

* Voeg de **Mobile** platform voor uw app.

## <a name="install-owin"></a>OWIN installeren
Nu u een app hebt geregistreerd, moet u uw app instellen om te communiceren met het v2.0-eindpunt om te valideren van binnenkomende aanvragen en -tokens.

* Om te beginnen, opent u de oplossing en de OWIN middleware NuGet-pakketten toevoegen aan het TodoListService-project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuth-verificatie configureren
* Een OWIN-Opstartklasse toevoegen aan het project met de naam TodoListService `Startup.cs`.  Rechtermuisknop te klikken op het project op--> **toevoegen** --> **Nieuw Item** --> Zoek naar 'OWIN'.  De OWIN-middleware roept de `Configuration(…)`-methode aan als uw app wordt gestart.
* Wijzig de klassendeclaratie naar `public partial class Startup` -we hebben u al deel uit van deze klasse geïmplementeerd in een ander bestand.  In de `Configuration(…)` methode, een aanroep van ConfgureAuth(...) verificatie voor uw web-app instellen.

```csharp
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(…)` methode, waarmee de Web-API wordt ingesteld om te accepteren van tokens van het v2.0-eindpunt.

```csharp
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* Nu kunt u `[Authorize]` kenmerken ter bescherming van uw domeincontrollers en de acties met OAuth 2.0-bearer-verificatie.  Opmaken de `Controllers\TodoListController.cs` klasse met een tag autoriseren.  Hierdoor wordt de gebruiker zich aanmelden voor de toegang tot deze pagina geforceerd.

```csharp
[Authorize]
public class TodoListController : ApiController
{
```

* Wanneer een geautoriseerde beller is roept een van de `TodoListController` API's, de actie moet mogelijk toegang tot informatie over de aanroeper.  OWIN biedt toegang tot de claims binnen het bearer-token via de `ClaimsPrincipal` object.  

```csharp
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* Tot slot opent u de `web.config` bestand in de hoofdmap van het project TodoListService en voer de configuratiewaarden van uw in de `<appSettings>` sectie.
  * Uw `ida:Audience` is de **toepassings-Id** van de app die u hebt ingevoerd in de portal.

## <a name="configure-the-client-app"></a>De client-app configureren
Voordat u de Todo List-Service in actie zien kunt, moet u de Client Todo-lijst configureren zodat het kan tokens uit het v2.0-eindpunt ophalen en naar de service aanroepen.

* Open in het project TodoListClient `App.config` en voer de configuratiewaarden van uw in de `<appSettings>` sectie.
  * Uw `ida:ClientId` toepassings-Id die u hebt gekopieerd uit de portal.

Ten slotte wilt opschonen, bouwen en uitvoeren van elk project!  U hebt nu een .NET MVC Web-API die tokens van beide persoonlijke Microsoft-accounts en werk-of schoolaccounts accepteert.  Meld u aan bij de TodoListClient en roept u uw web-api taken toevoegen aan de takenlijst van de gebruiker.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) [is opgegeven als een ZIP hier](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), of u kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Volgende stappen
U kunt nu verplaatsen naar andere onderwerpen.  U wilt proberen:

[Een Web-API aanroept vanuit een Web-App >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Voor aanvullende bronnen voor uitchecken:

* [De ontwikkelaarshandleiding v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow 'azure active directory'-tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden
We raden u aan in te stellen dat u meldingen ontvangt wanneer er beveiligingsincidenten optreden. Ga hiervoor naar [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneer u op Security Advisory Alerts.
