---
title: 'Snelstart: aanmelden instellen voor een bureaublad-app met Azure Active Directory B2C | Microsoft Docs'
description: Voer een voorbeeld-ASP.NET-bureaublad-app uit die gebruikmaakt van Azure Active Directory B2C voor aanmelden bij een account.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 161c1ec72b14f4cc1b2517a0dc6282f0548575b3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55182299"
---
# <a name="quickstart-set-up-sign-in-for-a-desktop-app-using-azure-active-directory-b2c"></a>Quickstart: Aanmelden instellen voor een bureaublad-app met Azure Active Directory B2C 

Azure Active Directory (Azure AD) B2C bevat functionaliteit voor identiteitsbeheer in de cloud ter bescherming van uw toepassing, bedrijf en klanten. Met Azure AD B2C kunnen uw toepassingen zich met behulp van open-standaardprotocollen verifiëren bij sociale en enterpriseaccounts. In deze snelstart gebruikt u een WPF-desktoptoepassing (Windows Presentation Foundation) om u aan te melden via een id-provider voor sociale netwerken en voor het aanroepen van een met Azure AD B2C beveiligde web-API.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

- [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **ASP.NET- en webontwikkelworkload**. 
- Een sociaalnetwerkaccount van Facebook, Google, Microsoft of Twitter.
- [Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
    ```

## <a name="run-the-application-in-visual-studio"></a>De toepassing uitvoeren in Visual Studio

1. In de projectmap van de voorbeeldtoepassing opent u de oplossing **active-directory-b2c-wpf.sln** in Visual Studio.
2. Druk op **F5** om fouten in de toepassing op te sporen.

## <a name="sign-in-using-your-account"></a>Aanmelden met uw account

1. Klik op **Aanmelden** om de werkstroom **Registreren of aanmelden** te starten.

    ![Voorbeeldtoepassing](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

    In de voorbeeldtoepassing worden verschillende registratiemogelijkheden ondersteund. U kunt bijvoorbeeld een id-provider voor sociale netwerken gebruiken of een lokaal account maken met behulp van een e-mailadres. Voor deze snelstart gebruikt u een account van een id-provider voor sociale netwerken (Facebook, Google, Microsoft of Twitter). 


2. In Azure AD B2C wordt een aangepaste aanmeldingspagina voor het fictieve merk Wingtip Toys weergegeven voor de voorbeeldweb-app. Klik op de knop van de id-provider voor sociale netwerken die u wilt gebruiken om u aan te melden via een id-provider voor sociale netwerken. 

    ![Provider voor aanmelden of registreren](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

    U moet zich verifiëren (aanmelden) met behulp van de referenties van uw sociaalnetwerkaccount en de toepassing autoriseren om informatie uit uw sociaalnetwerkaccount te lezen. Door toegang te verlenen, kan de toepassing profielgegevens van het sociaalnetwerkaccount ophalen, zoals uw naam en plaats. 

2. Voltooi het aanmeldingsproces voor de id-provider.

    De profielgegevens van uw nieuwe account worden ingevuld met informatie uit uw sociaalnetwerkaccount.

## <a name="edit-your-profile"></a>Het profiel bewerken

Azure AD B2C biedt functionaliteit waarmee gebruikers hun profielen kunnen bijwerken. In de voorbeeldweb-app wordt een Azure AD B2C-gebruikersstroom voor profielbewerking gebruikt voor de werkstroom. 

1. Klik in de menubalk van de toepassing op **Profiel bewerken** om het door u gemaakte profiel te bewerken.

    ![Profiel bewerken](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

2. Kies de id-provider die is gekoppeld aan het account dat u hebt gemaakt. Als u bijvoorbeeld Twitter als id-provider hebt gebruikt bij het maken van uw account, kiest u Twitter om de gekoppelde profielgegevens te wijzigen.

3. Wijzig uw **weergavenaam** of **plaats** en klik op **Doorgaan**.

    In het tekstvak *Tokengegevens* wordt een nieuw toegangstoken weergegeven. Als u de wijzigingen in uw profiel wilt controleren, kopieert u het toegangstoken en plakt u het in de tokendecoder https://jwt.ms.

## <a name="access-a-protected-api-resource"></a>Toegang tot een beveiligde API-resource

Klik op **API aanroepen** om een aanvraag te verzenden naar de beveiligde resource. 

    ![Call API](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

    The application includes the Azure AD access token in the request to the protected web API resource. The web API sends back the display name contained in the access token.

U hebt uw Azure AD B2C-gebruikersaccount gebruikt voor het geautoriseerd aanroepen van een web-API die met Azure AD B2C is beveiligd.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere snelstarts of zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een voorbeelddesktoptoepassing gebruikt om u aan te melden met een aangepaste aanmeldingspagina, om u aan te melden via een id-provider voor sociale netwerken, een Azure AD B2C-account te maken en een met Azure AD B2C beveiligde web-API aan te roepen. 

Aan de slag met het maken van uw eigen Azure AD B2C-tenant. 

> [!div class="nextstepaction"]
> [Een Azure Active Directory B2C-tenant maken in Azure Portal](tutorial-create-tenant.md)
