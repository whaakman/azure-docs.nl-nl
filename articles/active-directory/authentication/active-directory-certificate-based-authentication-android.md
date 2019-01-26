---
title: Azure Active Directory gebaseerde verificatie op Android
description: Meer informatie over de ondersteunde scenario's en de vereisten voor certificaten gebaseerde verificatie configureren in oplossingen met Android-apparaten
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: annaba
ms.openlocfilehash: 3127d4d80368723d117226c98ad51637724906b1
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55080937"
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Azure Active Directory gebaseerde verificatie op Android

Android-apparaten kunnen u certificaten gebaseerde verificatie gebruiken om te verifiëren bij Azure Active Directory met een clientcertificaat op hun apparaat bij het verbinden met:

* Mobiele Office-toepassingen zoals Microsoft Outlook en Microsoft Word
* Exchange ActiveSync (EAS) clients

Configuratie van deze functie elimineert de noodzaak om een combinatie van gebruikersnaam en wachtwoord bepaalde e-mail en Microsoft Office-toepassingen in uw mobiele apparaat.

In dit onderwerp vindt u met de vereisten en de ondersteunde scenario's voor het configureren van deze verificatie op een apparaat iOS(Android) voor gebruikers van tenants in Office 365 Enterprise, bedrijven, onderwijs, US Government, China en Duitsland-abonnementen.

Deze functie is beschikbaar in preview in Office 365 US Government Defense en federale plannen.

## <a name="microsoft-mobile-applications-support"></a>Ondersteuning voor mobiele toepassingen van Microsoft

| Apps | Ondersteuning |
| --- | --- |
| Azure Information Protection-app |![Selecteren][1] |
| Intune-bedrijfsportal |![Selecteren][1] |
| Microsoft Teams |![Selecteren][1] |
| OneNote |![Selecteren][1] |
| OneDrive |![Selecteren][1] |
| Outlook |![Selecteren][1] |
| Power BI |![Selecteren][1] |
| Skype voor Bedrijven |![Selecteren][1] |
| Word / Excel / PowerPoint |![Selecteren][1] |
| Yammer |![Selecteren][1] |

### <a name="implementation-requirements"></a>Vereisten voor implementatie

De versie van het besturingssysteem van het apparaat moet Android 5.0 (Lollipop) en hoger.

Een federation-server moet worden geconfigureerd.

Voor Azure Active Directory om een clientcertificaat intrekken, moet de AD FS-token hebben de volgende claims:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (Het serienummer van het clientcertificaat)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (De tekenreeks voor de verlener van het clientcertificaat)

Deze claims voor het vernieuwingstoken dat door Azure Active Directory toegevoegd als deze beschikbaar in de AD FS-token (of andere SAML-token zijn). Wanneer het vernieuwingstoken dat worden gevalideerd moet, wordt deze informatie wordt gebruikt om te controleren of de certificaatintrekkingslijst.

Als een best practice, moet u van uw organisatie AD FS-foutpagina's bijwerken met de volgende informatie:

* De vereiste voor het installeren van de Microsoft Authenticator voor Android.
* Instructies voor het ophalen van het certificaat van een gebruiker.

Zie voor meer informatie, [aanpassen van de AD FS Sign-in's](https://technet.microsoft.com/library/dn280950.aspx).

Sommige Office-apps (met moderne verificatie ingeschakeld) verzenden '*prompt = aanmelding*' naar Azure AD in de aanvraag. Standaard Azure AD wordt omgezet '*prompt = aanmelding*'in de aanvraag voor AD FS als'*wauth = usernamepassworduri*' (AD FS wilt U/P Auth vraagt) en '*wfresh = 0*' (AD FS om te vragen negeren van SSO-staat en voer een nieuwe verificatie). Als u verificatie inschakelen op basis van certificaten voor deze apps wilt, moet u het standaardgedrag van de Azure AD te wijzigen. Stel de '*PromptLoginBehavior*'in de instellingen van uw federatief domein naar'*uitgeschakelde*'.
U kunt de [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet deze taak uit te voeren:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Ondersteuning voor Exchange ActiveSync-clients

Bepaalde Exchange ActiveSync-toepassingen met Android 5.0 (Lollipop) of later worden ondersteund. Om te bepalen als deze functie biedt ondersteuning voor uw e-mailtoepassing, neem contact op met de ontwikkelaar van uw toepassing.

## <a name="next-steps"></a>Volgende stappen

Als u configureren van verificatie op basis van het certificaat in uw omgeving wilt, Zie [aan de slag met verificatie op basis van certificaten op Android](active-directory-certificate-based-authentication-get-started.md) voor instructies.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
