---
title: Test een Azure AD B2C-webtoepassing station | Microsoft Docs
description: Test station aanmelden, registreren, profiel te bewerken en opnieuw ingesteld wachtwoord gebruiker trajecten met behulp van een testomgeving met Azure AD B2C
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 2a51f15fd38a901548dcf4c56922f9715c328463
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-web-application-configured-with-azure-ad-b2c"></a>Een webtoepassing geconfigureerd met Azure AD B2C testen

Azure Active Directory B2C biedt cloud identity management te houden van uw toepassing, zakelijke en klanten die zijn beveiligd.  Deze snelstartgids maakt gebruik van een voorbeeld-app voor de lijst van taken voor het demonstreren van:

* Met behulp van de **registreren of aanmelden** beleid maken of meld u aan met een sociale id-provider of een lokale account via een e-mailadres. 
* Het aanroepen van een API die wordt beveiligd door Azure AD B2C maken en bewerken van taken.

## <a name="prerequisites"></a>Vereisten

* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - **ASP.NET- en web-ontwikkeling**

* Een sociaal-account van Facebook, Google, Microsoft- of Twitter. Als u geen een sociaal-account hebt, moet een geldig e-mailadres is vereist.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

[Downloaden of te klonen van de voorbeeldtoepassing](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) vanuit GitHub.

## <a name="run-the-app-in-visual-studio"></a>Voer de app in Visual Studio

Open in de projectmap voorbeeld toepassing de `B2C-WebAPI-DotNet.sln` oplossing in Visual Studio. 

De oplossing bestaat uit twee projecten:

* **TaskWebApp** : een ASP.NET MVC-webtoepassing waar een gebruiker hun taak lijstitems kan beheren.  
* **TaskService** – back-end van een ASP.NET-Web-API die u alle CRUD-bewerkingen beheert worden uitgevoerd op een gebruiker taken lijst. De web-app deze API-aanroepen en de resultaten worden weergegeven.

Voor deze Quick Start, moet u beide uitvoeren de `TaskWebApp` en `TaskService` projecten tegelijk. 

1. Klik in Solution Explorer met de rechtermuisknop op de oplossing en selecteer **Opstartprojecten instellen...** . 
2. Selecteer **meerdere opstartprojecten** keuzerondje.
3. Wijzig de **actie** voor beide projecten naar **Start**. Klik op **OK**.

![Starten van de pagina instellen in Visual Studio](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Selecteer **fouten opsporen > Foutopsporing starten** het bouwen en uitvoeren van beide toepassingen. Elke toepassing wordt geopend in een eigen browsertabblad:

* `https://localhost:44316/`-Deze pagina is de ASP.NET-webtoepassing. U communiceert rechtstreeks met deze toepassing in de Quick Start.
* `https://localhost:44332/`-Deze pagina is de web-API die wordt aangeroepen door de ASP.NET-webtoepassing.

## <a name="create-an-account"></a>Een account maken

Klik op de **aanmelden / aanmelden** koppeling in de ASP.NET-webtoepassing starten de **registreren of aanmelden** werkstroom. Wanneer u een account maakt, kunt u een bestaande sociale identiteit provider-account of een e-mailaccount.

![Voorbeeld ASP.NET-web-app](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Aanmelden met een identiteitsprovider van sociale

Klik op de knop van de id-provider die u wilt gebruiken voor het aanmelden met een identiteitsprovider van sociale. Als u liever een e-mailadres, Ga naar de [aanmelden met een e-mailadres](#sign-up-using-an-email-address) sectie.

![Provider aanmelden of registreren](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

U moet verifiëren met uw account sociale referenties en autoriseren van de toepassing om informatie te lezen uit je account sociale (sign-in). Door toegang te verlenen, kan de toepassing profielgegevens ophalen van de sociale account zoals uw naam en plaats. 

![Verifiëren en autoriseren met een sociaal-account](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Het proces aanmelden voltooid voor de id-provider. Bijvoorbeeld, klik op **aanmelden** knop voor Twitter.

De details van uw nieuwe profiel zijn al ingevuld met informatie uit uw sociale-account.

![Nieuwe registratie profiel accountgegevens](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

De weergavenaam, functietitel en plaats velden bijwerken en klik op **doorgaan**.  De waarden die u invoert worden gebruikt voor het hulpprogramma voor het account van uw Azure AD B2C-gebruikersprofiel.

U hebt een nieuwe Azure AD B2C-gebruikersaccount die gebruikmaakt van een id-provider gemaakt. 

Volgende stap: [Jump om weer te geven van de claims](#view-your-claims) sectie.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

Als u een sociale account niet gebruikt kiest voor verificatie, kunt u een Azure AD B2C gebruikersaccount in met een geldig e-mailadres. Een lokale gebruikersaccount van Azure AD B2C gebruikt Azure Active Directory als de id-provider. Met uw e-mailadres, klikt u op de **geen account? Nu aanmelden** koppeling.

![Aanmelden of registreren via e-mail](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-email-web.png)

Voer een geldig e-mailadres in en klikt u op **sturen verificatiecode**. Een geldig e-mailadres is vereist voor de verificatiecode ontvangen van Azure AD B2C. 

Voer de verificatiecode die u via e-mail ontvangt en klik op **code controleren**.

Je profielgegevens toevoegen en klik op **maken**.

![Aanmelden bij de nieuwe account via e-mail](media/active-directory-b2c-quickstarts-web-app/sign-up-new-account-profile-email-web.png)

Een nieuwe Azure AD B2C lokaal gebruikersaccount hebt gemaakt.

## <a name="reset-your-password"></a>Uw wachtwoord opnieuw instellen

Als u uw account met een e-mailadres hebt gemaakt, heeft Azure AD B2C functionaliteit waarmee gebruikers hun wachtwoord opnieuw instellen. Als u wilt bewerken van het profiel dat u hebt gemaakt, klik op de profielnaam in de menubalk en selecteer **wachtwoord opnieuw instellen**.

Controleer of uw e-mailadres door te voeren en te klikken op **sturen verificatiecode**. Een verificatiecode naar uw e-mailadres verzonden.

Voer de verificatiecode in die u hebt ontvangen in e-mailbericht en klik op **code controleren**.

Nadat u uw e-mailadres is geverifieerd, klikt u op **doorgaan**.

Voer uw nieuwe wachtwoord en klik op **doorgaan**.

## <a name="view-your-claims"></a>Uw claims weergeven

Klik op **Claims** in de menubalk van web application om de claims die zijn gekoppeld aan uw laatste actie weer te geven. 

![Aanmelden bij de nieuwe account via e-mail](media/active-directory-b2c-quickstarts-web-app/view-claims-sign-up-web.png)

In dit voorbeeld is de laatste actie is voor de *aanmelden of registreren* optreden. U ziet de **claimtype** `http://schemas.microsoft.com/claims/authnclassreference` is `b2c_1_susi` die wijzen op de laatste actie is registreren of aanmelden. Als de laatste actie opnieuw instellen van wachtwoorden, is de **claimtype** zou `b2c_1_reset`.

## <a name="edit-your-profile"></a>Uw profiel bewerken

Azure Active Directory B2C biedt functionaliteit waarmee gebruikers hun profielen bijwerken. Klik op de profielnaam in de menubalk van web-toepassing en selecteer **profiel bewerken** bewerken van het profiel dat u hebt gemaakt.

![Profiel bewerken](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Wijzig uw **weergavenaam** en **stad**.  Klik op **doorgaan** uw profiel bijwerken.

![Profiel bijwerken](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

U ziet u de updates van de naam weergegeven in de rechterbovenhoek van de pagina na het wijzigen van de naam van uw. 

Klik op **Claims**. Wijzigingen aangebracht in **weergavenaam** en **stad** worden doorgevoerd in de claims.

![Weergave claims](media/active-directory-b2c-quickstarts-web-app/view-claims-update-web.png)

 U ziet de **claimtype** `http://schemas.microsoft.com/claims/authnclassreference` is bijgewerkt naar `b2c_1_edit_profile` die wijzen op de laatste actie is een profiel bewerken. Vergeet niet, de naam en plaats de nieuwe waarden zijn *Sara S.* en *Seattle*.

## <a name="access-a-resource"></a>Toegang tot een bron

Klik op **takenlijst** in te voeren en uw taak lijstitems te wijzigen. De ASP.NET-webtoepassing bevat een toegangstoken in de aanvraag op het web API resource aanvragende toestemming voor het uitvoeren van bewerkingen op de taakitems lijst van de gebruiker. 

Voer tekst in de **Nieuw Item** in het tekstvak. Klik op **toevoegen** aanroepen van de Azure AD B2C beveiligd web-API waarmee een lijstitem taak toegevoegd.

![Een taak lijstitem toevoegen](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

## <a name="other-scenarios"></a>Andere scenario's

Andere scenario's voor het testen van station zijn als volgt:

* Meld u af bij de toepassing en klik op **takenlijst**. U ziet hoe u wordt gevraagd aan te melden en lijstitems worden doorgevoerd. 
* Een nieuwe account maken met een ander type account. Gebruik bijvoorbeeld een identiteitsprovider van sociale als u een account met een e-mailadres eerder hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen

De volgende stap is uw eigen Azure AD B2C-tenant maken en configureren van het voorbeeld wilt uitvoeren met behulp van uw tenant. 

> [!div class="nextstepaction"]
> [Een Azure Active Directory B2C-tenant maken in de Azure portal](active-directory-b2c-get-started.md)