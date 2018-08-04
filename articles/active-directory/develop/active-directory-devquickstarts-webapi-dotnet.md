---
title: .NET Web API van Azure AD aan de slag | Microsoft Docs
description: Over het bouwen van een .NET MVC-web-API die kan worden geïntegreerd met Azure AD voor verificatie en autorisatie.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: ca506d821fe3534468c0d370dd51464e5df90f79
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504658"
---
# <a name="azure-ad-net-web-api-getting-started"></a>.NET Web API van Azure AD aan de slag
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Als u een toepassing die toegang tot beveiligde resources biedt maakt, moet u weten hoe om te voorkomen dat onrechtmatige toegang tot deze bronnen.
Azure Active Directory (Azure AD) maakt het eenvoudig en snel te beveiligen van een web-API met behulp van OAuth 2.0-bearer access tokens met slechts een paar regels code.

In de ASP.NET-web-apps, kunt u deze beveiliging uitvoeren met behulp van de Microsoft-implementatie van de community-gestuurde OWIN-middleware opgenomen in .NET Framework 4.5. Hier gebruiken we OWIN te maken van een 'Takenlijst' web-API die:

* Hiermee wordt aangegeven welke API's zijn beveiligd.
* Hiermee valideert u dat de web-API-aanroepen een geldige toegangstoken bevatten.

Voor het bouwen van de te doen lijst API, moet u eerst:

1. U registreert een toepassing met Azure AD.
2. De app instellen voor de pijplijn OWIN-verificatie gebruiken.
3. Een clienttoepassing voor het aanroepen van de web-API configureren.

Aan de slag [het app-basisproject downloaden](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) of [downloaden van het voltooide voorbeeld](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Elk is een Visual Studio 2013-oplossing. U moet ook een Azure AD-tenant waar uw toepassing registreren. Als u niet hebt, [informatie over het verkrijgen van een](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Stap 1: Een toepassing registreren bij Azure AD
Als u wilt beveiligen uw toepassing, moet u eerst een toepassing maken in uw tenant en Azure AD voorzien van enkele belangrijke gegevens.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Kies uw Azure AD-tenant door te klikken op uw account in de rechterbovenhoek van de pagina, gevolgd door te klikken op de **schakelen tussen mappen** navigatie en selecteer vervolgens de juiste tenant.
 * Deze stap overslaan als u slechts één Azure AD-tenant voor uw account hebt of als u al hebt geselecteerd de juiste Azure AD-tenant.

3. Klik in het linkernavigatievenster op **Azure Active Directory**.

4. Klik op **App-registraties**, en selecteer vervolgens **toevoegen**.

5. Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of Web-API**.
  * **Naam** beschrijving van uw toepassing op gebruikers. Voer **takenlijst Service**.
  * **Omleidings-Uri** is een combinatie van schema en de tekenreeks die Azure AD gebruikt om tokens retourneert die door uw app heeft aangevraagd. Voer `https://localhost:44321/` voor deze waarde.

6. Uit de **instellingen** -> **eigenschappen** pagina voor uw toepassing, het bijwerken van de URI van de App-ID. Een tenant-specifieke ID invoeren. Geef bijvoorbeeld `https://contoso.onmicrosoft.com/TodoListService` op.

7. De configuratie op te slaan. Laat de portal geopend, omdat u ook moet over enkele ogenblikken uw clienttoepassing registreren.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Stap 2: De app instellen op de pijplijn OWIN-verificatie gebruiken
Voor het valideren van binnenkomende aanvragen en tokens, moet u voor het instellen van uw toepassing om te communiceren met Azure AD.

1. Als u wilt beginnen, opent u de oplossing en de OWIN-middleware NuGet-pakketten toevoegen aan de TodoListService-project met behulp van de Package Manager-Console.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Voeg een OWIN-Opstartklasse toe aan de TodoListService-project met de naam `Startup.cs`.  Met de rechtermuisknop op het project, selecteer **toevoegen** > **Nieuw Item**, en zoek vervolgens **OWIN**. De OWIN-middleware roept de `Configuration(…)`-methode aan als uw app wordt gestart.

3. Wijzigen van de klasse aangifte `public partial class Startup`. We hebben al deel uit van deze klasse voor u geïmplementeerd in een ander bestand. In de `Configuration(…)` een aanroep van methode `ConfgureAuth(…)` verificatie voor uw web-app instellen.

    ```csharp
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Open het bestand `App_Start\Startup.Auth.cs` en implementeren van de `ConfigureAuth(…)` methode. De parameters die u opgeeft in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` moet fungeren als coördinaten voor uw app om te communiceren met Azure AD.

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

5. U kunt nu gebruiken `[Authorize]` kenmerken voor beveiliging van uw domeincontrollers en de acties met JSON Web Token (JWT) bearer authentication. Bepaalt de `Controllers\TodoListController.cs` klasse met een autoriseren-tag bevindt. Hierdoor wordt de gebruiker zich aanmeldt voordat u toegang tot deze pagina geforceerd.

    ```csharp
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Wanneer een geautoriseerde aanroeper is roept een van de `TodoListController` API's, de actie mogelijk toegang tot informatie over de oproepende functie. OWIN biedt toegang tot de claims binnen het bearer-token via de `ClaimsPrincpal` object.  

6. Een algemene vereiste voor web-API's is het valideren van de in het token aanwezige 'scopes'. Dit zorgt ervoor dat de gebruiker heeft ingestemd met de machtigingen die vereist zijn voor toegang tot de te doen List-Service.

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

7. Open de `web.config` -bestand in de hoofdmap van de TodoListService-project en voer de configuratiewaarden van uw in de `<appSettings>` sectie.
  * `ida:Tenant` is de naam van uw Azure AD-tenant bijvoorbeeld contoso.onmicrosoft.com.
  * `ida:Audience` is de URI van de App-ID van de toepassing die u hebt ingevoerd in de Azure-portal.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>Stap 3: Een clienttoepassing configureren en uitvoeren van de service
Voordat u de te doen List-Service in actie zien kunt, moet u de client To Do List configureren zodat deze kunnen tokens van Azure AD en naar de service aanroepen.

1. Ga terug naar de [Azure-portal](https://portal.azure.com).

2. Een nieuwe toepassing maken in uw Azure AD-tenant, en selecteer **systeemeigen clienttoepassing** in de resulterende prompt.
  * **Naam** beschrijving van uw toepassing op gebruikers.
  * Voer `http://TodoListClient/` voor de **omleidings-Uri** waarde.

3. Nadat u de registratie hebt voltooid, wijst Azure AD een unieke toepassings-ID toe aan uw app. U moet deze waarde in de volgende stappen, dus te kopiëren uit de toepassingspagina.

4. Uit de **instellingen** weergeeft, schakelt **vereiste machtigingen**, en selecteer vervolgens **toevoegen**. Zoek en selecteer de te doen lijst Service, toevoegen de **toegang TodoListService** machtiging onder **gedelegeerde machtigingen**, en klik vervolgens op **gedaan**.

5. Open in Visual Studio, `App.config` in de TodoListClient project en voer vervolgens de configuratiewaarden van uw in de `<appSettings>` sectie.

  * `ida:Tenant` is de naam van uw Azure AD-tenant bijvoorbeeld contoso.onmicrosoft.com.
  * `ida:ClientId` is de app-ID die u hebt gekopieerd vanuit Azure portal.
  * `todo:TodoListResourceId` is de URI van de App-ID van de toepassing te doen List-Service die u hebt ingevoerd in de Azure-portal.

## <a name="next-steps"></a>Volgende stappen
Ten slotte opschonen, bouwen en uitvoeren van elk project. Als u niet hebt gedaan, is nu tijd om te maken van een nieuwe gebruiker in uw tenant met een *. onmicrosoft.com-domein. Meld u aan bij de client To Do List met die gebruiker en sommige taken toevoegen aan de takenlijst van de gebruiker.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is beschikbaar in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). U kunt nu verder met meer identity-scenario's.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
