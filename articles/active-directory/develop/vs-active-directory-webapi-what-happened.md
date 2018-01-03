---
title: Wijzigingen die zijn aangebracht aan een project WebApi wanneer u verbinding met Azure AD maakt | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurt met uw project WebApi wanneer u verbinding met Azure AD maakt met behulp van Visual Studio
services: active-directory
documentationcenter: 
author: kraigb
manager: mtillman
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 8a0f6e1838bcc550829c0da92dc224e1df859e9c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Wat is er gebeurd met mijn WebApi-project (Visual Studio Azure Active Directory verbonden service)
> [!div class="op_single_selector"]
> * [Aan de slag](vs-active-directory-webapi-getting-started.md)
> * [Wat is er gebeurd](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Verwijzingen zijn toegevoegd
### <a name="nuget-package-references"></a>NuGet-pakket met verwijzingen
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>.NET-verwijzingen
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Codewijzigingen
### <a name="code-files-were-added-to-your-project"></a>Codebestanden zijn toegevoegd aan uw project
Een verificatie-Opstartklasse **App_Start/Startup.Auth.cs** is toegevoegd aan uw project met starten van de logica voor Azure AD-verificatie.

### <a name="startup-code-was-added-to-your-project"></a>Starten van de code is toegevoegd aan uw project
Als u al een Opstartklasse in uw project de **configuratie** methode is bijgewerkt met een aanroep van `ConfigureAuth(app)`. Anders wordt is een Opstartklasse toegevoegd aan uw project.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Uw app.config of web.config-bestand heeft de nieuwe configuratiewaarden.
De volgende configuratie-items zijn toegevoegd.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>Een Azure AD-App is gemaakt
Een Azure AD-toepassing is gemaakt in de map die u hebt geselecteerd in de wizard.

[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Als ik gecontroleerd *afzonderlijke gebruikersaccounts verificatie uit te schakelen*, welke aanvullende wijzigingen zijn aangebracht aan mijn project?
NuGet-pakket met verwijzingen zijn verwijderd en de bestanden zijn verwijderd en een back-up. Afhankelijk van de status van uw project, kunt u wellicht handmatig verwijderen van aanvullende verwijzingen of bestanden of code desgewenst wijzigen.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet-pakket verwijzingen verwijderd (voor die aanwezig)
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Codebestanden back-up gemaakt en verwijderd (voor die aanwezig)
Elk van de volgende bestanden is een back-up en verwijderd uit het project. Back-upbestanden bevinden zich in een map 'Back-up' in de hoofdmap van de directory van het project.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>Codebestanden back-up gemaakt (die aanwezig)
Elk van de volgende bestanden is een back-up maken voordat het wordt vervangen. Back-upbestanden bevinden zich in een map 'Back-up' in de hoofdmap van de directory van het project.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Als ik gecontroleerd *mapgegevens lezen*, welke aanvullende wijzigingen zijn aangebracht aan mijn project?
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Als u meer wijzigingen zijn aangebracht in uw app.config of web.config
De volgende aanvullende configuratie-items zijn toegevoegd.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Uw App in Azure Active Directory is bijgewerkt
Uw App in Azure Active Directory is bijgewerkt met de *mapgegevens lezen* machtigingen en de sleutel is gemaakt is die wordt gebruikt als de *ida: wachtwoord* in de `web.config` bestand.

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

