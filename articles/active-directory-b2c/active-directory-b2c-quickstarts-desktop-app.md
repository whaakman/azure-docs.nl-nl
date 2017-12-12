---
title: Test station een bureaubladtoepassing Azure AD B2C | Microsoft Docs
description: Test station aanmelden, registreren, profiel te bewerken en opnieuw ingesteld wachtwoord gebruiker trajecten met behulp van een testomgeving met Azure AD B2C
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: mtillman
editor: PatAltimore
ms.assetid: 86293627-26fb-4e96-a76b-f263f9a945bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 51f5643f0bd975beb939c2d5a8853810fb609ec9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="test-drive-a-desktop-application-configured-with-azure-ad-b2c"></a>Een bureaubladtoepassing geconfigureerd met Azure AD B2C testen

Azure Active Directory B2C biedt cloud identity management te houden van uw toepassing, zakelijke en klanten die zijn beveiligd.  Deze snelstartgids wordt een voorbeeldapp van het bureaublad van Windows Presentation Foundation (WPF) gebruikt om aan te tonen:

* Met behulp van de **registreren of aanmelden** beleid maken of meld u aan met een sociale id-provider of een lokale account via een e-mailadres. 
* **Een API aanroepen** uw weergegeven naam ophalen uit een Azure AD B2C beveiligde resource.

## <a name="prerequisites"></a>Vereisten

* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - **Bureaublad .NET-ontwikkeling**

* Een sociaal-account van Facebook, Google, Microsoft- of Twitter. Als u geen een sociaal-account hebt, moet een geldig e-mailadres is vereist.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

[Downloaden of te klonen van de voorbeeldtoepassing](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) vanuit GitHub.

## <a name="run-the-app-in-visual-studio"></a>Voer de app in Visual Studio

Open in de projectmap voorbeeld toepassing de `active-directory-b2c-wpf.sln` oplossing in Visual Studio. 

Selecteer **fouten opsporen > Foutopsporing starten** het bouwen en uitvoeren van de toepassing. 

## <a name="create-an-account"></a>Een account maken

Klik op **aanmelden** starten de **registreren of aanmelden** werkstroom. Wanneer u een account maakt, kunt u een bestaande sociale identiteit provider-account of een e-mailaccount.

![Voorbeeldtoepassing](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Aanmelden met een identiteitsprovider van sociale

Klik op de knop van de id-provider die u wilt gebruiken voor het aanmelden met een identiteitsprovider van sociale. Als u liever een e-mailadres, Ga naar de [aanmelden met een e-mailadres](#sign-up-using-an-email-address) sectie.

![Provider aanmelden of registreren](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

U moet verifiëren met uw account sociale referenties en autoriseren van de toepassing om informatie te lezen uit je account sociale (sign-in). Door toegang te verlenen, kan de toepassing profielgegevens ophalen van de sociale account zoals uw naam en plaats. 

![Verifiëren en autoriseren met een sociaal-account](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

De details van uw nieuwe profiel zijn al ingevuld met informatie uit uw sociale-account. Wijzig de details als u wilt en klikt u op **doorgaan**.

![Nieuwe registratie profiel accountgegevens](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

U hebt een nieuwe Azure AD B2C-gebruikersaccount die gebruikmaakt van een id-provider gemaakt. Na het aanmelden, het toegangstoken wordt weergegeven in de *info Token* in het tekstvak. Het toegangstoken wordt gebruikt bij het openen van de API-resource.

![Verificatietoken](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

Volgende stap: [gaan bewerken van uw profiel](#edit-your-profile) sectie.

### <a name="sign-up-using-an-email-address"></a>Aanmelden met een e-mailadres

Als u een sociale account niet gebruikt kiest voor verificatie, kunt u een Azure AD B2C gebruikersaccount in met een geldig e-mailadres. Een lokale gebruikersaccount van Azure AD B2C gebruikt Azure Active Directory als de id-provider. Met uw e-mailadres, klikt u op de **geen account? Nu aanmelden** koppeling.

![Aanmelden of registreren via e-mail](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-email-wpf.png)

Voer een geldig e-mailadres in en klikt u op **sturen verificatiecode**. Een geldig e-mailadres is vereist voor de verificatiecode ontvangen van Azure AD B2C.

Voer de verificatiecode die u via e-mail ontvangt en klik op **code controleren**.

Je profielgegevens toevoegen en klik op **maken**.

![Aanmelden bij de nieuwe account via e-mail](media/active-directory-b2c-quickstarts-desktop-app/sign-up-new-account-profile-email-wpf.png)

Een nieuwe Azure AD B2C lokaal gebruikersaccount hebt gemaakt. Na het aanmelden, het toegangstoken wordt weergegeven in de *info Token* in het tekstvak. Het toegangstoken wordt gebruikt bij het openen van de API-resource.

![Verificatietoken](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="edit-your-profile"></a>Uw profiel bewerken

Azure Active Directory B2C biedt functionaliteit waarmee gebruikers hun profielen bijwerken. Klik op **profiel bewerken** bewerken van het profiel dat u hebt gemaakt.

![Profiel bewerken](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

Kies de id-provider die is gekoppeld aan het account dat u hebt gemaakt. Als u Twitter als de id-provider gebruikt wanneer u uw account hebt gemaakt, bijvoorbeeld kiezen Twitter om de details van de bijbehorende profiel te wijzigen.

![Kies een provider die is gekoppeld aan een profiel te bewerken](media/active-directory-b2c-quickstarts-desktop-app/edit-account-choose-provider-wpf.png)

Wijzig uw **weergavenaam** of **stad**. 

![Profiel bijwerken](media/active-directory-b2c-quickstarts-desktop-app/update-profile-wpf.png)

Een nieuw toegangstoken wordt weergegeven in de *info Token* in het tekstvak. Als u controleren of de wijzigingen in uw profiel wilt, kopieer en plak het toegangstoken in het token decoder https://jwt.ms.

![Verificatietoken](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="access-a-resource"></a>Toegang tot een bron

Klik op **API aanroepen** beveiligd resource https://fabrikamb2chello.azurewebsites.net/hello om te vragen naar de Azure AD B2C. 

![API aanroepen](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

De toepassing bevat het toegangstoken dat wordt weergegeven in de *info Token* in het tekstvak in de aanvraag. De API stuurt terug de weergavenaam die is opgenomen in het toegangstoken.

## <a name="next-steps"></a>Volgende stappen

De volgende stap is uw eigen Azure AD B2C-tenant maken en configureren van het voorbeeld wilt uitvoeren met behulp van uw tenant. 

> [!div class="nextstepaction"]
> [Een Azure Active Directory B2C-tenant maken in de Azure portal](active-directory-b2c-get-started.md)
