---
title: 'Snelstart: aanmelden instellen voor een bureaublad-app met Azure Active Directory B2C | Microsoft Docs'
description: Voer een voorbeeld-ASP.NET-bureaublad-app uit die gebruikmaakt van Azure Active Directory B2C voor aanmelden bij een account.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.custom: mvc
ms.date: 2/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: af4fe8ce4d9f5584241b56762ddf9c60aa28f0ba
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293367"
---
# <a name="quickstart-set-up-sign-in-for-a-desktop-app-using-azure-active-directory-b2c"></a>Snelstart: aanmelden instellen voor een bureaublad-app met Azure Active Directory B2C 

Azure Active Directory (Azure AD) B2C bevat functionaliteit voor identiteitsbeheer in de cloud ter bescherming van uw toepassing, bedrijf en klanten. Met Azure AD B2C zijn uw apps in staat om zich met behulp van open-standaardprotocollen te verifiëren bij sociale accounts en Enterprise-accounts.

In deze snelstart gebruikt u een voorbeeld van een voor Azure AD B2C geconfigureerde WPF-desktop-app (Windows Presentation Foundation). Deze app wordt gebruikt voor het aanmelden via een id-provider voor sociale netwerken en voor het aanroepen van een met Azure AD B2C beveiligde web-API.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload**. 
* Een sociaalnetwerkaccount van Facebook, Google, Microsoft of Twitter.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

[Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

## <a name="run-the-app-in-visual-studio"></a>De app uitvoeren in Visual Studio

In de projectmap van de voorbeeldtoepassing opent u de `active-directory-b2c-wpf.sln`-oplossing in Visual Studio.

Druk op **F5** om fouten in de toepassing op te sporen.

## <a name="create-an-account"></a>Een account maken

Klik op **Aanmelden** om de werkstroom **Registreren of aanmelden** op basis van een Azure AD B2C-beleid te starten.

![Voorbeeldtoepassing](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

In de voorbeeldtoepassing worden verschillende registratiemogelijkheden ondersteund. U kunt bijvoorbeeld een id-provider voor sociale netwerken gebruiken of een lokaal account maken met behulp van een e-mailadres. Voor deze snelstart gebruikt u een account van een id-provider voor sociale netwerken (Facebook, Google, Microsoft of Twitter). 

### <a name="sign-up-using-a-social-identity-provider"></a>Aanmelden met een id-provider voor sociale netwerken

In Azure AD B2C wordt een aangepaste aanmeldingspagina voor het fictieve merk Wingtip Toys weergegeven voor de voorbeeldweb-app. 

1. Klik op de knop van de id-provider voor sociale netwerken die u wilt gebruiken om u aan te melden via een id-provider voor sociale netwerken. 

    ![Provider voor aanmelden of registreren](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

    U moet zich verifiëren (aanmelden) met behulp van de referenties van uw sociaalnetwerkaccount en de toepassing autoriseren om informatie uit uw sociaalnetwerkaccount te lezen. Door toegang te verlenen, kan de toepassing profielgegevens van het sociaalnetwerkaccount ophalen, zoals uw naam en plaats. 

2. Voltooi het aanmeldingsproces voor de id-provider. Als u bijvoorbeeld Twitter hebt gekozen, voert u uw referenties voor Twitter in en klikt u op **Aanmelden**.

    ![Verifiëren en autoriseren met een sociaalnetwerkaccount](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

    De profielgegevens van uw nieuwe account worden ingevuld met informatie uit uw sociale account. 

3. Wijzig de gegevens desgewenst en klik op **Doorgaan**. De ingevoerde waarden worden gebruikt voor uw Azure AD B2C-gebruikersaccountprofiel.

    ![Aanmeldingsprofielgegevens van nieuw account](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

    U hebt een nieuw Azure AD B2C-gebruikersaccount gemaakt dat gebruikmaakt van een id-provider. Na het aanmelden wordt het toegangstoken weergegeven in het tekstvak *Tokengegevens*. Het toegangstoken wordt gebruikt voor de toegang tot de API-resource.

## <a name="edit-your-profile"></a>Het profiel bewerken

Azure Active Directory B2C biedt functionaliteit waarmee gebruikers hun profielen kunnen bijwerken.  In de voorbeeldweb-app wordt een Azure AD B2C-bewerkingsprofielbeleid gebruikt voor de werkstroom. 

1. Klik op **Profiel bewerken** om het door u gemaakte profiel te bewerken.

    ![Profiel bewerken](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

2. Kies de id-provider die is gekoppeld aan het account dat u hebt gemaakt. Als u bijvoorbeeld Twitter als id-provider hebt gebruikt bij het maken van uw account, kiest u Twitter om de gekoppelde profielgegevens te wijzigen.

3. Wijzig uw **weergavenaam** of **plaats** en klik op **Doorgaan**.

    In het tekstvak *Tokengegevens* wordt een nieuw toegangstoken weergegeven. Als u de wijzigingen in uw profiel wilt controleren, kopieert u het toegangstoken en plakt u het in de tokendecoder https://jwt.ms.

## <a name="access-a-protected-web-api-resource"></a>Toegang tot een beveiligde web-API-resource

Klik op **API aanroepen** om een aanvraag te verzenden naar de met Azure AD B2C beveiligde resource. 

![API aanroepen](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

De toepassing bevat het toegangstoken van Azure AD in de aanvraag voor de beveiligde web-API-resource. De web-API retourneert de weergavenaam uit het toegangstoken.

U hebt uw Azure AD B2C-gebruikersaccount gebruikt voor het geautoriseerd aanroepen van een web-API die met Azure AD B2C is beveiligd.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere snelstarts of zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

De volgende stap is het maken van uw eigen Azure AD B2C-tenant en het configureren van het voorbeeld voor uitvoering met deze tenant. 

> [!div class="nextstepaction"]
> [Een Azure Active Directory B2C-tenant maken in Azure Portal](tutorial-create-tenant.md)
