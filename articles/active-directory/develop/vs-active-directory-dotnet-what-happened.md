---
title: Wijzigingen in een MVC-project, wanneer u verbinding met Azure AD maakt
description: Hierin wordt beschreven wat er gebeurt met uw MVC-project als u verbinding met Azure AD maken met behulp van Visual Studio verbonden services
services: active-directory
author: ghogen
manager: douge
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: b79dc4a65193b04390f64cfbfc8cedb6758af0f0
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492124"
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn MVC-project (Visual Studio Azure Active Directory verbonden service)?

> [!div class="op_single_selector"]
> - [Aan de slag](vs-active-directory-dotnet-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-dotnet-what-happened.md)

In dit artikel geeft de exacte wijzigingen aangebracht in am ASP.NET MVC-project bij het toevoegen van de [Azure Active Directory service met behulp van Visual Studio verbonden](vs-active-directory-add-connected-service.md).

Zie voor meer informatie over het werken met de gekoppelde service [aan de slag](vs-active-directory-dotnet-getting-started.md).

## <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op de projectverwijzingen voor bestand *.NET) en `packages.config` (NuGet-verwijzingen).

| Type | Referentie |
| --- | --- |
| .NET; NuGet | Microsoft.IdentityModel.Protocol.Extensions |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.Cookies |
| .NET; NuGet | Microsoft.Owin.Security.OpenIdConnect |
| .NET; NuGet | Owin |
| .NET        | System.IdentityModel |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |
| .NET        | System.Runtime.Serialization |

Aanvullende verwijzingen als u hebt geselecteerd de **mapgegevens lezen** optie:

| Type | Referentie |
| --- | --- |
| .NET; NuGet | EntityFramework |
| .NET        | EntityFramework.SqlServer (Visual Studio 2015 alleen) |
| .NET; NuGet | Microsoft.Azure.ActiveDirectory.GraphClient |
| .NET; NuGet | Microsoft.Data.Edm |
| .NET; NuGet | Microsoft.Data.OData |
| .NET; NuGet | Microsoft.Data.Services.Client |
| .NET; NuGet | Microsoft.IdentityModel.Clients.ActiveDirectory |
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms (Visual Studio 2015 alleen) |
| .NET; NuGet | System.Spatial |

De volgende verwijzingen worden verwijderd (ASP.NET 4 projecten, zoals in Visual Studio 2015):

| Type | Referentie |
| --- | --- |
| .NET; NuGet | Microsoft.AspNet.Identity.Core |
| .NET; NuGet | Microsoft.AspNet.Identity.EntityFramework |
| .NET; NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Wijzigingen in bestanden

- Stel de eigenschap `IISExpressSSLPort` naar een afzonderlijke getal.
- Stel de eigenschap `WebProject_DirectoryAccessLevelKey` op 0 of 1 als u hebt geselecteerd de **mapgegevens lezen** optie.
- Stel de eigenschap `IISUrl` naar `https://localhost:<port>/` waar `<port>` komt overeen met de `IISExpressSSLPort` waarde.

## <a name="webconfig-or-appconfig-changes"></a>Web.config of app.config wijzigingen

- De volgende configuratie-items toegevoegd:

    ```xml
    <appSettings>
        <add key="ida:ClientId" value="<ClientId from the new Azure AD app>" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="<your selected Azure domain>" />
        <add key="ida:TenantId" value="<the Id of your selected Azure AD tenant>" />
        <add key="ida:PostLogoutRedirectUri" value="<project start page, such as https://localhost:44335>" />
    </appSettings>
    ```

- Toegevoegd `<dependentAssembly>` elementen onder de `<runtime><assemblyBinding>` knooppunt voor `System.IdentityModel.Tokens.Jwt` en `Microsoft.IdentityModel.Protocol.Extensions`.

Aanvullende wijzigingen Als u hebt geselecteerd de **mapgegevens lezen** optie:

- De volgende configuratie-item onder toegevoegd `<appSettings>`:

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- De volgende elementen onder toegevoegd `<configuration>`; waarden voor de project-mdf-bestand en de project-catalogus-id verschilt:

    ```xml
    <configSections>
      <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
      <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>

    <connectionStrings>
      <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\<project-mdf-file>.mdf;Initial Catalog=<project-catalog-id>;Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>

    <entityFramework>
      <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
        <parameters>
          <parameter value="mssqllocaldb" />
        </parameters>
      </defaultConnectionFactory>
      <providers>
        <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
      </providers>
    </entityFramework>
    ```

- Toegevoegd `<dependentAssembly>` elementen onder de `<runtime><assemblyBinding>` knooppunt voor `Microsoft.Data.Services.Client`, `Microsoft.Data.Edm`, en `Microsoft.Data.OData`.

## <a name="code-changes-and-additions"></a>Codewijzigingen en toevoegingen

- Toegevoegd de `[Authorize]` kenmerk `Controllers/HomeController.cs` en andere bestaande domeincontrollers.

- Een verificatie-Opstartklasse toegevoegd `App_Start/Startup.Auth.cs`, met opstartlogica voor Azure AD-verificatie. Als u hebt geselecteerd de **mapgegevens lezen** optie, dit bestand bevat ook code voor het ontvangen van een OAuth-code en het exchange voor een toegangstoken.

- Een controllerklasse toegevoegd `Controllers/AccountController.cs`met `SignIn` en `SignOut` methoden.

- Een gedeeltelijke weergave toegevoegd `Views/Shared/_LoginPartial.cshtml`, met de koppeling van een actie voor `SignIn` en `SignOut`.

- Een gedeeltelijke weergave toegevoegd `Views/Account/SignoutCallback.cshtml`, met HTML-code voor afmelding gebruikersinterface.

- Bijgewerkt de `Startup.Configuration` methode om op te nemen van een aanroep van `ConfigureAuth(app)` als de klasse bestaat al; anders wordt toegevoegd een `Startup` klasse met de methode aanroept.

- Toegevoegd `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) of `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015), dat informatie bevat die Visual Studio gebruikt voor het bijhouden van het toevoegen van de gekoppelde service.

- Als u hebt geselecteerd de **mapgegevens lezen** optie toegevoegd `Models/ADALTokenCache.cs` en `Models/ApplicationDbContext.cs` ter ondersteuning van token in cache opslaan. Ook een extra domeincontroller en de weergave ter illustratie van toegang tot informatie uit gebruikersprofielen met behulp van Azure graph API's toegevoegd: `Controllers/UserProfileController.cs`, `Views/UserProfile/Index.cshtml`, en `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Back-up (Visual Studio 2015)

Bij het toevoegen van de gekoppelde service, Visual Studio 2015 een back-up van bestanden gewijzigd of verwijderd. Alle betrokken bestanden worden opgeslagen in de map `Backup/AzureAD`. Visual Studio 2017 maakt geen back-ups.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`
- `Views\Shared\_LoginPartial.cshtml`

## <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een Azure AD-toepassing in het domein dat u hebt geselecteerd bij het toevoegen van de gekoppelde service gemaakt.
- Bijgewerkt van de app om op te nemen de **mapgegevens lezen** machtiging als deze optie is geselecteerd.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Volgende stappen

- [Verificatiescenario's voor Azure Active Directory](authentication-scenarios.md)
- [Aanmelden bij Microsoft toevoegen aan een ASP.NET-web-app](quickstart-v1-aspnet-webapp.md)
