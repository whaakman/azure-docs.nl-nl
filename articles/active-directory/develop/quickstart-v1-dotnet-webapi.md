---
title: Bouw een .NET web-API die is geïntegreerd met Azure AD voor verificatie en autorisatie | Microsoft Docs
description: Zo bouwt u een .NET MVC web-API die is geïntegreerd met Azure Active Directory voor verificatie en autorisatie.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: f44955e13cb5774119dc083f4a3220b432d9cd9e
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098061"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>Snelstart: Bouw een .NET web-API die is geïntegreerd met Azure Active Directory voor verificatie en autorisatie

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Als u een toepassing bouwt die toegang verschaft aan beschermde bronnen, dan dient u te weten hoe u ongewenste toegang tot die bronnen kunt voorkomen. Azure Active Directory (Azure AD) maakt het eenvoudig en duidelijk om een web-API te helpen beschermen door OAuth 2.0 bearer-toegangstokens te gebruiken met slechts een paar regels code.

In ASP.NET web-apps, kunt u deze bescherming verwezenlijken door de Microsoft implementatie te gebruiken van de door de gemeenschap ontwikkelde OWIN-middleware, inbegrepen bij .NET Framework 4.5. Hier kunt u uw eigen OWIN gebruiken om een takenlijst web-API aan te maken die:

* Bepaalt welke API’s beschermd zijn.
* Hiermee wordt gevalideerd dat de web-API-aanroepen een geldige toegangscode bevatten.

In deze snelstart, bouwt u de takenlijst API en ziet u hoe:

1. U registreert een toepassing met Azure AD.
2. De app kan instellen om de OWIN-autorisatiepijplijn te gebruiken.
3. U een client configureert om de web-API aan te roepen.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorgt u dat u aan deze vereisten voldoet:

* [Download het raamwerk van de app](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) of [download het voltooide voorbeeld](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Elk is een Visual Studio 2013 oplossing.
* Heb een Azure Active Directory tenant waarin u uw toepassing kunt registreren. Als u die nog niet hebt, raadpleegt u [zie hoe u er een krijgt](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Stap 1: Een toepassing met Azure AD registreren

Om uw toepassing zeker te stellen, dient u eerst een toepassing in uw tenant aan te maken en Azure Active Directory kerngegevens te verstrekken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Kies uw Azure Active Directory-tenant door in de rechterbovenhoek van deze pagina uw account te selecteren, en vervolgens de navigatie **Directory schakelen** te kiezen.
    * Sla deze stap over als u maar één Azure tenant in uw account hebt, of als u al de juiste Azure Active Directory tenant hebt gekozen.

3. Selecteer in het navigatiedeelvenster aan de linkerkant **Azure Active Directory**.
4. Selecteer **App-registraties** en kies **Toevoegen**.
5. Volg de prompts voor het maken van een nieuwe **Webtoepassing en/of Web-API**.
    * **Naam** beschrijft uw toepassing voor gebruikers. Voer de **Takenlijst-service** in.
    * **Omleidings-URI** is een combinatie van een schema en een tekenreeks die door Azure Active Directory wordt gebruikt om tokens die u hebt aangevraagd te retourneren. Voer `https://localhost:44321/` voor deze waarde in.

6. Op de pagina **Instellingen > Eigenschappen** van uw toepassing werkt u de URI voor de app-id bij. Voer een tenant-specifieke identificatie in. Geef bijvoorbeeld `https://contoso.onmicrosoft.com/TodoListService` op.
7. Sla de configuratie op. Laat de portal open omdat u straks ook de clienttoepassing moet registreren.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Stap 2: De app instellen om de OWIN-autorisatiepijplijn te gebruiken

Om binnenkomende verzoeken en tokens te valideren, dient u uw toepassing in te stellen om met Azure Active Directory te kunnen communiceren.

1. Open om te beginnen de oplossing en voeg de pakketten OWIN middleware NuGet toe aan het project TodoListService via het Package Manager Console.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Voeg een OWIN-opstartklasse toe aan het project TodoListService met de naam `Startup.cs`.  Klik met de rechtermuisknop op het project, selecteer **Toevoegen > Nieuw item** en zoek naar **OWIN**. De OWIN-middleware roept de `Configuration(…)`-methode aan als uw app wordt gestart.

3. Wijzig de klassedeclaratie naar `public partial class Startup`. We hebben voor u al een deel van deze klasse in een ander bestand geïmplementeerd. In de `Configuration(…)` methode plaatst u een gesprek naar `ConfgureAuth(…)` om verificatie voor uw web-app in te stellen.

    ```csharp
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Open het bestand `App_Start\Startup.Auth.cs` en implementeer de `ConfigureAuth(…)`-methode. De parameters die u in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` heb opgegeven, dienen als coördinaten voor uw app om met Azure Active Directory te communiceren.

    ```csharp
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. Gebruik de `[Authorize]`-kenmerken om uw controllers en handelingen te beschermen met JSON Web Token (JWT) bearer-verificatie. Decoreer de `Controllers\TodoListController.cs` klasse met een autorisatievlag waardoor de gebruiker wordt gedwongen om in te loggen om die pagina te kunnen bezoeken.

    ```csharp
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Wanneer een geautoriseerde beller met succes een van de `TodoListController` API’s heeft ingeroepen, dan heeft de handeling mogelijk toegang nodig tot informatie over de beller. OWIN biedt toegang tot de claims in de bearer-token via het `ClaimsPrincpal` object.  

6. Een veelvoorkomende vereiste voor web-API’s, is om de "scopes" te valideren die aanwezig zijn in de token om er zeker van te zijn dat de gebruiker toestemming heeft gegeven aan de machtigingen die vereist zijn om de takenlijst-service te benaderen.

    ```csharp
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Open het bestand `web.config` in de hoofdmap van het project TodoListService, en geef de configuratiewaarden op in de sectie `<appSettings>`.
    * `ida:Tenant` is de naam van uw Azure Active Directory-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    * `ida:Audience` is de App ID URI van de toepassing die u in de Azure Portal hebt ingevoerd.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>Stap 3: Een clienttoepassing configureren en de service uitvoeren

Voordat u de service Takenlijst in actie kunt zien, dient u de Takenlijst-client te configureren zodat deze tokens kan ophalen van Azure Active Directory en aanroepen naar de service kan uitvoeren.

1. Ga terug naar [Azure Portal](https://portal.azure.com).
1. Creëer een nieuwe toepassing in uw Azure Active Directory-tenant, en selecteer **Systeemeigen clienttoepassing** in de volgende prompt.
    * **Naam** beschrijft uw toepassing voor gebruikers.
    * Voer `http://TodoListClient/` in voor de **Omleidings-URI** waarde.

1. Nadat u de registratie hebt voltooid, wijst Azure Active Directory een unieke toepassings-id toe aan uw app. U hebt deze waarde nodig in de volgende stappen. Kopieer deze daarom vanaf het toepassingspagina.
1. Selecteer op de pagina **Instellingen** de **Vereiste machtigingen** en selecteer vervolgens **Toevoegen**. Vind en selecteer de Takenlijst-service, voeg de machtiging **Toegang Takenlijst-service toe** onder **Gedelegeerde toestemmingen**, en selecteer vervolgens **Gereed**.
1. Open in Visual Studio `App.config` in het TodoListClient-project, en voer uw configuratiewaarden in de sectie `<appSettings>` in.

    * `ida:Tenant` is de naam van uw Azure Active Directory-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    * `ida:ClientId` is the app-id die u uit het Azure Portal hebt gekopieerd.
    * `todo:TodoListResourceId` is de App ID URI van de toepassing Takenlijst-service die u in de Azure Portal hebt ingevoerd.

1. Schoon elk project op, bouw en run het.
1. Als u dit nog niet hebt gedaan, maak een gebruiker aan in uw tenant met een a *.onmicrosoft.com-domein.
1. Log met die gebruiker in op de Takenlijst-client en voeg een paar taken toe aan de takenlijst van die gebruiker.

## <a name="next-steps"></a>Volgende stappen

* Download als naslaginformatie het volledige voorbeeld (zonder uw configuratiewaarden) van [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). U kunt nu naar andere identiteitsscenario's gaan.
