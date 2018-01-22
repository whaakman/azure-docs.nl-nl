---
title: Identiteitsbeheer in de cloud voor webtoepassingen en mobiele toepassingen met Azure AD B2C | Microsoft Docs
description: Consumententoepassingen ontwikkelen met Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeedakhter-msft
ms.custom: seohack1
ms.openlocfilehash: 7881793ebce10b4f9fbc9c534d9d6fc0ebb38f0b
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a>Azure AD B2C: u kunt u op uw app richten, dan letten wij op de registratie en aanmelding

Azure AD B2C is een oplossing voor identiteitsbeheer in de cloud voor uw web- en mobiele toepassingen. Dit is een maximaal beschikbare, wereldwijde service die naar honderden miljoenen identiteiten kan worden geschaald. Azure AD B2C is ontwikkeld op een hoogwaardig, veilig platform en is bedoeld om uw toepassingen, uw bedrijf en uw klanten te beveiligen.

Met een minimale configuratie kan Azure AD B2C uw toepassing in staat stellen om het volgende te verifiëren:

* **Sociale accounts** (zoals Facebook, Google en LinkedIn)
* **Bedrijfsaccounts** (met behulp van open-standaardprotocollen, OpenID Connect of SAML)
* **Lokale accounts** (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord)

## <a name="get-started"></a>Aan de slag

Haal eerst uw eigen tenant op aan de hand van de stappen in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md) (Een Azure AD B2C-tenant maken).

Kies het scenario voor het ontwikkelen van uw toepassing:

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobiele en bureaubladapps](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobiele en bureaubladapps</center> | [Overzicht](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br /><br />[iOS](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[Android](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[Xamarin](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <center>![Web Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Overzicht](active-directory-b2c-reference-oidc.md)<br /><br />[ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [Node.js](active-directory-b2c-devquickstarts-web-node.md) |  |
| <center>![Apps met één pagina](../active-directory/develop/media/active-directory-developers-guide/SPA.png)<br />Apps met één pagina</center> | [Overzicht](active-directory-b2c-reference-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <center>![Web-API's](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)<br />Web-API's</center> | [ASP.NET](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [Een .NET-web-API aanroepen](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a>Nieuwe functies

Kijk regelmatig of er nieuwe informatie is over toekomstige wijzigingen in Azure Active Directory B2C. We tweeten ook over eventuele updates via @AzureAD.

* Naast "Geïntegreerd beleid" (algemene beschikbaarheid), is nu ook de functie ["Aangepast beleid"](active-directory-b2c-overview-custom.md) beschikbaar in de openbare preview.  Aangepaste beleidsregels zijn bedoeld voor identiteitsprofessionals die controle over de samenstelling van hun identiteitservaring nodig hebben.
* De functie [Toegangstoken](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) is nu beschikbaar in de openbare preview.
* [Algemene beschikbaarheid van in Europa gebaseerde Azure AD B2C](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/)-directory's is aangekondigd.
* Bekijk onze groeiende bibliotheek met [codevoorbeelden op GitHub](https://github.com/Azure-Samples?q=b2c).

## <a name="how-to-articles"></a>Artikelen met procedures

Lees meer over het gebruik van specifieke functies van Azure Active Directory B2C:

* Configureer accounts van [Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md) en [LinkedIn](active-directory-b2c-setup-li-app.md) voor gebruik in uw consumententoepassingen.
* [Gebruik aangepaste kenmerken om informatie over uw consumenten te verzamelen](active-directory-b2c-reference-custom-attr.md).
* [Schakel Azure Multi-Factor Authentication in uw consumententoepassingen in](active-directory-b2c-reference-mfa.md).
* [Stel selfservice wachtwoordherstel in voor uw consumenten](active-directory-b2c-reference-sspr.md).
* [Pas het uiterlijk aan van de registratie- en aanmeldingspagina en andere consumentenpagina's](active-directory-b2c-reference-ui-customization.md) die door Azure Active Directory B2C worden verwerkt.
* [Gebruik Azure Active Directory Graph API om consumenten programmatisch te maken, te lezen, bij te werken en te verwijderen](active-directory-b2c-devquickstarts-graph-dotnet.md) in uw Azure Active Directory B2C-tenant.

## <a name="next-steps"></a>Volgende stappen

Deze koppelingen zijn handig als u de service uitgebreid wilt verkennen:

* Zie de [Prijsinformatie over Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Bekijk onze [codevoorbeelden](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) voor Azure Active Directory B2C. 
* Vraag hulp bij Stack Overflow met de tags [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c).
* Geef ons uw mening via [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c). We horen graag wat u ervan vindt.
* Lees de [naslaginformatie voor Azure AD B2C-protocollen](active-directory-b2c-reference-protocols.md).
* Lees de [naslaginformatie voor Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md).
* Lees de [veelgestelde vragen over Azure Active Directory B2C](active-directory-b2c-faqs.md).
* [Aanvragen voor bestandsondersteuning voor Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden

We raden u aan in te stellen dat u meldingen ontvangt wanneer er beveiligingsincidenten optreden. Ga hiervoor naar [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneer u op Security Advisory Alerts.

