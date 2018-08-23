---
title: Wijzigingen in een WebAPI-project, wanneer u verbinding met Azure AD maakt
description: Hierin wordt beschreven wat er gebeurt met uw WebAPI-project als u verbinding met Azure AD maken met behulp van Visual Studio
services: active-directory
author: ghogen
manager: douge
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 04732d6541fd6132360d4c235b35979c70772922
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060784"
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn WebAPI-project (Visual Studio Azure Active Directory verbonden service)

> [!div class="op_single_selector"]
> - [Aan de slag](vs-active-directory-webapi-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-webapi-what-happened.md)

In dit artikel geeft de exacte wijzigingen aangebracht in ASP.NET WebAPI-, ASP.NET-toepassing voor één pagina-en ASP.NET-API van Azure, bij het toevoegen van de [Azure Active Directory service met behulp van Visual Studio verbonden](vs-active-directory-add-connected-service.md). Geldt ook voor de projecten ASP.NET Azure Mobile Service in Visual Studio 2015.

Zie voor meer informatie over het werken met de gekoppelde service [aan de slag](vs-active-directory-webapi-getting-started.md).

## <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op de projectverwijzingen voor bestand *.NET) en `packages.config` (NuGet-verwijzingen).

| Type | Referentie |
| --- | --- |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.ActiveDirectory |
| .NET; NuGet | Microsoft.Owin.Security.Jwt |
| .NET; NuGet | Microsoft.Owin.Security.OAuth |
| .NET; NuGet | Owin |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |

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
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms<br>(Visual Studio 2015 alleen) |
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
        <add key="ida:Tenant" value="<your selected Azure domain>" />
        <add key="ida:Audience" value="<your selected domain + / + project name>" />
    </appSettings>
    ```

- Visual Studio 2017: ook de volgende vermelding onder toegevoegd `<appSettings>`"

    ```xml
    <add key="ida:MetadataAddress" value="<domain URL + /federationmetadata/2007-06/federationmetadata.xml>" />
    ```

- Toegevoegd `<dependentAssembly>` elementen onder de `<runtime><assemblyBinding>` knooppunt voor `System.IdentityModel.Tokens.Jwt`.

- Als u hebt geselecteerd de **mapgegevens lezen** optie, worden de volgende configuratie-item onder toegevoegd `<appSettings>`:

    ```xml
    <add key="ida:Password" value="<Your Azure AD app's new password>" />
    ```

## <a name="code-changes-and-additions"></a>Codewijzigingen en toevoegingen

- Toegevoegd de `[Authorize]` kenmerk `Controllers/ValueController.cs` en andere bestaande domeincontrollers.

- Een verificatie-Opstartklasse toegevoegd `App_Start/Startup.Auth.cs`, die opstartlogica voor Azure AD-verificatie of dienovereenkomstig aangepast. Als u hebt geselecteerd de **mapgegevens lezen** optie, dit bestand bevat ook code voor het ontvangen van een OAuth-code en het exchange voor een toegangstoken.

- (Visual Studio 2015 met ASP.NET 4 alleen app) Verwijderd `App_Start/IdentityConfig.cs` en toegevoegd `Controllers/AccountController.cs`, `Models/IdentityModel.cs`, en `Providers/ApplicationAuthProvider.cs`.

- Toegevoegd `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) of `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015), dat informatie bevat die Visual Studio gebruikt voor het bijhouden van het toevoegen van de gekoppelde service.

### <a name="file-backup-visual-studio-2015"></a>Back-up (Visual Studio 2015)

Bij het toevoegen van de gekoppelde service, Visual Studio 2015 een back-up van bestanden gewijzigd of verwijderd. Alle betrokken bestanden worden opgeslagen in de map `Backup/AzureAD`. Visual Studio 2017 maakt geen back-ups.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ApplicationOAuthProvider.cs`

## <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een Azure AD-toepassing in het domein dat u hebt geselecteerd bij het toevoegen van de gekoppelde service gemaakt.
- Bijgewerkt van de app om op te nemen de **mapgegevens lezen** machtiging als deze optie is geselecteerd.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Volgende stappen

- [Verificatiescenario's voor Azure Active Directory](authentication-scenarios.md)
- [Aanmelden bij Microsoft toevoegen aan een ASP.NET-web-app](quickstart-v1-aspnet-webapp.md)