---
title: Azure AD .NET-web-API aan de slag | Microsoft Docs
description: "Het bouwen van een .NET MVC-web-API die kan worden geïntegreerd met Azure AD voor verificatie en autorisatie."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f44d75f45073a5d9aa9b1863ed227aba4efcf785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a>Een web-API beveiligen met behulp van bearer-tokens van Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Als u een toepassing die toegang tot beveiligde bronnen biedt maakt, moet u weten hoe om te voorkomen dat onrechtmatige toegang tot deze bronnen.
Azure Active Directory (Azure AD) maakt het eenvoudig en snel een web-API beveiligen met behulp van OAuth 2.0-bearer-toegang met alleen een paar regels code tokens.

In ASP.NET-web-apps, kunt u deze beveiliging uitvoeren met behulp van de Microsoft-implementatie van de community aangestuurde OWIN-middleware opgenomen in .NET Framework 4.5. We gebruiken hier OWIN te maken van een 'Takenlijst' web-API die:

* Geeft aan dat de API's zijn beveiligd.
* Hiermee valideert u dat de web-API-aanroepen een ongeldig toegangstoken bevat.

Als u wilt de te doen lijst API maken, moet u eerst:

1. U registreert een toepassing met Azure AD.
2. De app instellen voor de pijplijn OWIN-verificatie gebruiken.
3. Configureer een clienttoepassing naar de web-API aanroepen.

Aan de slag [de basis van de app downloaden](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld downloaden](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Elk is een Visual Studio 2013-oplossing. U moet ook een Azure AD-tenant waarin u uw toepassing registreren. Als u niet hebt, [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Stap 1: Een toepassing registreren met Azure AD
Als u wilt beveiligen uw toepassing, moet u eerst een toepassing maakt in uw tenant en Azure AD voorzien van enkele belangrijke informatie.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Klik op uw account op de bovenste balk. In de **Directory** kiest u de Azure AD-tenant waar u uw toepassing registreren.

3. Klik op **meer Services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.

4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.

5. Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of Web-API**.
  * **Naam** beschrijving van uw toepassing voor gebruikers. Voer **takenlijst Service**.
  * **Omleidings-Uri** is een combinatie schema en de tekenreeks die Azure AD gebruikt om tokens retourneert die door uw app heeft aangevraagd. Voer `https://localhost:44321/` voor deze waarde.

6. Van de **instellingen** -> **eigenschappen** pagina voor uw toepassing, het bijwerken van de App ID URI. Voer een tenantspecifieke-id. Geef bijvoorbeeld `https://contoso.onmicrosoft.com/TodoListService` op.

7. De configuratie op te slaan. Laat de portal geopend, omdat u ook moet de clienttoepassing binnenkort registreren.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Stap 2: De app instellen voor de pijplijn OWIN-verificatie gebruiken
Voor het valideren van binnenkomende aanvragen en tokens, moet u voor het instellen van uw toepassing om te communiceren met Azure AD.

1. Om te beginnen, opent u de oplossing en de OWIN middleware NuGet-pakketten toevoegen aan het project TodoListService met behulp van de Package Manager-Console.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Een OWIN-Opstartklasse toevoegen aan het project met de naam TodoListService `Startup.cs`.  Met de rechtermuisknop op het project, selecteert u **toevoegen** > **Nieuw Item**, en zoek vervolgens naar **OWIN**. De OWIN-middleware roept de `Configuration(…)`-methode aan als uw app wordt gestart.

3. Wijzig de klassendeclaratie naar `public partial class Startup`. We hebben al deel uit van deze klasse geïmplementeerd voor u in een ander bestand. In de `Configuration(…)` een aanroep van methode `ConfgureAuth(…)` verificatie voor uw web-app instellen.

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(…)` methode. De parameters die u opgeeft in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` fungeert als coördinaten voor uw app om te communiceren met Azure AD.

    ```C#
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

5. Nu kunt u `[Authorize]` kenmerken om uw domeincontrollers en de acties met JSON Web Token (JWT) bearer-verificatie te beveiligen. Opmaken de `Controllers\TodoListController.cs` klasse met een tag autoriseren. Hierdoor wordt de gebruiker zich aanmelden voor de toegang tot deze pagina geforceerd.

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Wanneer een geautoriseerde beller is roept een van de `TodoListController` API's, de actie moet mogelijk toegang tot informatie over de aanroeper. OWIN biedt toegang tot de claims binnen het bearer-token via de `ClaimsPrincpal` object.  

6. Een algemene vereiste voor web-API's is het valideren van de in het token aanwezige 'scopes'. Dit zorgt ervoor dat de gebruiker heeft ingestemd met de vereiste machtigingen voor toegang tot de te doen lijst Service.

    ```C#
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

7. Open de `web.config` bestand in de hoofdmap van het project TodoListService en voer de configuratiewaarden van uw in de `<appSettings>` sectie.
  * `ida:Tenant`is de naam van uw Azure AD-tenant bijvoorbeeld: contoso.onmicrosoft.com.
  * `ida:Audience`is de URI van de App-ID van de toepassing die u hebt ingevoerd in de Azure-portal.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>Stap 3: Een clienttoepassing configureren en uitvoeren van de service
Voordat u de te doen List-Service in actie zien kunt, moet u de client To Do List configureren zodat het kan tokens van Azure AD verkrijgen en naar de service aanroepen.

1. Ga terug naar de [Azure-portal](https://portal.azure.com).

2. Maak een nieuwe toepassing in uw Azure AD-tenant en selecteer **systeemeigen clienttoepassing** in de resulterende prompt.
  * **Naam** beschrijving van uw toepassing voor gebruikers.
  * Voer `http://TodoListClient/` voor de **omleidings-Uri** waarde.

3. Nadat u de registratie, wijst Azure AD een unieke toepassings-ID toe aan uw app. U moet deze waarde in de volgende stappen, dus kopiëren van de toepassingspagina.

4. Van de **instellingen** pagina **Required Permissions**, en selecteer vervolgens **toevoegen**. Zoek en selecteer de te doen lijst Service, toevoegen de **toegang TodoListService** machtiging onder **gedelegeerde machtigingen**, en klik vervolgens op **gedaan**.

5. Open in Visual Studio `App.config` in de TodoListClient project en voer vervolgens de configuratiewaarden van uw in de `<appSettings>` sectie.

  * `ida:Tenant`is de naam van uw Azure AD-tenant bijvoorbeeld: contoso.onmicrosoft.com.
  * `ida:ClientId`is de app-ID die u hebt gekopieerd uit de Azure-portal.
  * `todo:TodoListResourceId`is de URI van de App-ID van de toepassing om te doen List-Service die u hebt ingevoerd in de Azure portal.

## <a name="next-steps"></a>Volgende stappen
Ten slotte wilt opschonen, bouwen en uitvoeren van elk project. Als u nog niet gedaan hebt, is nu tijd om aan te maken van een nieuwe gebruiker in de tenant met een *. onmicrosoft.com-domein. Aanmelden bij de client To Do List bij die gebruiker en een aantal taken toevoegen aan de takenlijst van de gebruiker.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is beschikbaar in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). U kunt nu verder met meer identity-scenario's.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
