---
title: Wijzigingen in een MVC-project, wanneer u verbinding met Azure AD maakt | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurt met uw MVC-project wanneer u verbinding met Azure AD maakt met behulp van Visual Studio verbonden services
services: active-directory
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: 1925a32ce5745673c08af3f9cfe63090d4adfa23
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn MVC-project (Visual Studio Azure Active Directory verbonden service)?

> [!div class="op_single_selector"]
> - [Aan de slag](vs-active-directory-dotnet-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-dotnet-what-happened.md)

In dit artikel identificeert de exacte wijzigingen aangebracht in am ASP.NET MVC-project bij het toevoegen van de [Azure Active Directory service met behulp van Visual Studio verbonden](vs-active-directory-add-connected-service.md).

Zie voor meer informatie over het werken met de gekoppelde service [aan de slag](vs-active-directory-dotnet-getting-started.md).

## <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op het project *.NET bestandsverwijzingen) en `packages.config` (NuGet-verwijzingen).

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
- Stel de eigenschap `IISUrl` naar `https://localhost:<port>/` waar `<port>` overeenkomt met de `IISExpressSSLPort` waarde.

## <a name="webconfig-or-appconfig-changes"></a>wijzigingen in web.config of app.config

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

- De volgende elementen onder toegevoegd `<configuration>`; waarden voor het project-mdf-bestand en de project-catalogus-id varieert:

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

- Een verificatie-Opstartklasse toegevoegd `App_Start/Startup.Auth.cs`, met starten van de logica voor Azure AD-verificatie. Als u hebt geselecteerd de **mapgegevens lezen** optie, dit bestand bevat ook de code voor het ontvangen van een OAuth-code en exchange-voor een toegangstoken.

- Een controllerklasse toegevoegd `Controllers/AccountController.cs`met `SignIn` en `SignOut` methoden.

- Een gedeeltelijke weergave toegevoegd `Views/Shared/_LoginPartial.cshtml`, met de koppeling van een actie voor `SignIn` en `SignOut`.

- Een gedeeltelijke weergave toegevoegd `Views/Account/SignoutCallback.cshtml`, met de HTML-code voor afmelden gebruikersinterface.

- Bijgewerkt de `Startup.Configuration` methode voor het opnemen van een aanroep van `ConfigureAuth(app)` als de klasse al bestaat; anders toegevoegd een `Startup` klasse die bevat de methode aanroept.

- Toegevoegd `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) of `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015), dat informatie bevat die Visual Studio gebruikt voor het bijhouden van het toevoegen van de gekoppelde service.

- Als u hebt geselecteerd de **mapgegevens lezen** optie toegevoegd `Models/ADALTokenCache.cs` en `Models/ApplicationDbContext.cs` ter ondersteuning van token opslaan in cache. Ook een extra domeincontroller en de weergave ter illustratie van toegang tot gebruikersprofielgegevens met Azure graph API's toegevoegd: `Controllers/UserProfileController.cs`, `Views/UserProfile/Index.cshtml`, en `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Back-up (Visual Studio 2015)

Wanneer u de gekoppelde service toevoegt, Visual Studio 2015 een back-up bestanden gewijzigd of verwijderd. Alle betrokken bestanden worden opgeslagen in de map `Backup/AzureAD`. Visual Studio 2017 maakt geen back-ups.

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
- De app om op te nemen bijgewerkt de **mapgegevens lezen** machtiging als deze optie is geselecteerd.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Volgende stappen

- [Verificatie-scenario's voor Azure Active Directory](active-directory-authentication-scenarios.md)
- [Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app](guidedsetups/active-directory-aspnetwebapp-v1.md)
