---
title: Test een web-app van Azure AD B2C ingeschakeld station | Microsoft Docs
description: Test station aanmelden, registreren, profiel te bewerken en opnieuw ingesteld wachtwoord gebruiker trajecten met behulp van een testomgeving met Azure AD B2C
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: patricka
ms.openlocfilehash: bc56da695145f396a2899fb9dc7add3af9a549e8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="test-drive-an-azure-ad-b2c-enabled-web-app"></a>Test station een Azure AD B2C ingeschakeld web-app

Azure Active Directory B2C biedt cloud identity management te houden van uw toepassing, zakelijke en klanten die zijn beveiligd. Deze snelstartgids maakt gebruik van een voorbeeld-app voor de lijst van taken voor het demonstreren van:

> [!div class="checklist"]
> * Aanmelden met een aangepaste aanmeldingspagina.
> * Meld u aan met een sociale id-provider.
> * Maken en beheren van uw Azure AD B2C-account en de gebruiker het profiel.
> * Het aanroepen van een web-API die zijn beveiligd door Azure AD B2C.

## <a name="prerequisites"></a>Vereisten

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET en web ontwikkeling** werkbelasting. 
* Een sociaal-account van Facebook, Google, Microsoft- of Twitter.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

[Downloaden of te klonen van de voorbeeldtoepassing](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) vanuit GitHub.

## <a name="run-the-app-in-visual-studio"></a>Voer de app in Visual Studio

Open in de projectmap voorbeeld toepassing de `B2C-WebAPI-DotNet.sln` oplossing in Visual Studio. 

De oplossing is een voorbeeld van taak lijst die bestaat uit twee projecten:

* **TaskWebApp** : een ASP.NET MVC-webtoepassing waar een gebruiker hun taak lijstitems kan beheren.  
* **TaskService** – back-end van een ASP.NET-Web-API die wordt beheerd bewerkingen uitgevoerd op de lijst taakitems van een gebruiker. De web-app deze web-API aanroept en de resultaten worden weergegeven.

Voor deze Quick Start, moet u beide uitvoeren de `TaskWebApp` en `TaskService` projecten tegelijk. 

1. Selecteer in het menu Visual Studio **projecten > Opstartprojecten instellen...** . 
2. Selecteer **meerdere opstartprojecten** keuzerondje.
3. Wijzig de **actie** voor beide projecten naar **Start**. Klik op **OK**.

![Starten van de pagina instellen in Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Selecteer **fouten opsporen > Foutopsporing starten** het bouwen en uitvoeren van beide toepassingen. Elke toepassing wordt geopend in een eigen browsertabblad:

`https://localhost:44316/`-Deze pagina is de ASP.NET-webtoepassing. U communiceert rechtstreeks met deze toepassing in de Quick Start.
`https://localhost:44332/`-Deze pagina is de web-API die wordt aangeroepen door de ASP.NET-webtoepassing.

## <a name="create-an-account"></a>Een account maken

Klik op de **aanmelden / aanmelden** koppeling in de ASP.NET-webtoepassing starten de **registreren of aanmelden** werkstroom. Wanneer u een account maakt, kunt u een bestaande sociale identiteit provider-account of een e-mailaccount. Gebruik een account van de provider van sociale identiteit van Facebook, Google, Microsoft- of Twitter voor deze snelstartgids.

![Voorbeeld ASP.NET-web-app](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Aanmelden met een identiteitsprovider van sociale

Klik op de knop van de id-provider die u wilt gebruiken voor het aanmelden met een identiteitsprovider van sociale. 

![Provider aanmelden of registreren](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

U moet verifiëren met uw account sociale referenties en autoriseren van de toepassing om informatie te lezen uit je account sociale (sign-in). Door toegang te verlenen, kan de toepassing profielgegevens ophalen van de sociale account zoals uw naam en plaats. 

Het proces aanmelden voltooid voor de id-provider. Bijvoorbeeld: als u Twitter, Voer uw referenties voor Twitter en klikt u op **aanmelden**.

![Verifiëren en autoriseren met een sociaal-account](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Uw nieuwe Azure AD B2C profiel accountgegevens zijn al ingevuld met informatie uit uw sociale-account.

De weergavenaam, functietitel en plaats velden bijwerken en klik op **doorgaan**.  De waarden die u invoert worden gebruikt voor het hulpprogramma voor het account van uw Azure AD B2C-gebruikersprofiel.

![Nieuwe registratie profiel accountgegevens](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Er is:

> [!div class="checklist"]
> * Geverifieerd met een id-provider.
> * Een Azure AD B2C-account gemaakt. 

## <a name="edit-your-profile"></a>Uw profiel bewerken

Azure Active Directory B2C biedt functionaliteit waarmee gebruikers hun profielen bijwerken. Klik op de profielnaam in de menubalk van web-toepassing en selecteer **profiel bewerken** bewerken van het profiel dat u hebt gemaakt.

![Profiel bewerken](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Wijzig uw **weergavenaam** en **stad**.  Klik op **doorgaan** uw profiel bijwerken.

![Profiel bijwerken](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Let op dat de weergavenaam van uw in de rechterbovenhoek van de pagina wordt de bijgewerkte naam weergegeven. 

## <a name="access-a-secured-web-api-resource"></a>Toegang krijgen tot een beveiligde web API-resource

Klik op **takenlijst** in te voeren en uw taak lijstitems te wijzigen. De ASP.NET-webtoepassing bevat een toegangstoken in de aanvraag op het web API resource aanvragende toestemming voor het uitvoeren van bewerkingen op de taakitems lijst van de gebruiker. 

Voer tekst in de **Nieuw Item** in het tekstvak. Klik op **toevoegen** aanroepen van de Azure AD B2C beveiligd web-API waarmee een lijstitem taak toegevoegd.

![Een taak lijstitem toevoegen](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

U hebt uw Azure AD B2C-gebruikersaccount is gebruikt voor een geautoriseerde gesprek een web-API van Azure AD B2C, beveiligd.

## <a name="next-steps"></a>Volgende stappen

Het voorbeeld in deze snelstartgids gebruikt kan worden gebruikt om te proberen andere Azure AD B2C-scenario's, waaronder:

* Maken van een nieuwe lokale account met behulp van een e-mailadres.
* Opnieuw instellen van uw lokale wachtwoord.

Als u klaar bent om dieper uw eigen Azure AD B2C-tenant maken en configureren van het voorbeeld wilt uitvoeren met behulp van uw eigen tenant, probeer de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Een ASP.NET-web-app maken met Azure Active Directory B2C registreren, aanmelden, profiel bewerken en wachtwoord opnieuw instellen](active-directory-b2c-devquickstarts-web-dotnet-susi.md)