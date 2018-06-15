---
title: Azure Active Directory gebaseerde verificatie voor Android
description: Meer informatie over de ondersteunde scenario's en de vereisten voor het configureren op certificaten gebaseerde verificatie in oplossingen met Android-apparaten
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: annaba
ms.openlocfilehash: ef58fd6c4701f367c6b8664c9cf9ee76e15fbd70
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
ms.locfileid: "33880738"
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Azure Active Directory gebaseerde verificatie voor Android

Android-apparaten kunnen u certificaten gebaseerde verificatie (CBA) gebruiken om te verifiëren met Azure Active Directory met een clientcertificaat op hun apparaat bij het verbinden met:

* Mobiele Office-toepassingen zoals Microsoft Outlook en Microsoft Word
* Exchange ActiveSync (EAS)-clients

Configuratie van deze functie wordt voorkomen moet een combinatie van gebruikersnaam en wachtwoord invoeren in bepaalde e-mail en Microsoft Office-toepassingen op uw mobiele apparaat.

Dit onderwerp vindt u de vereisten en de ondersteunde scenario's voor het configureren van CBA op een apparaat iOS(Android) voor gebruikers van tenants in Office 365 Enterprise, Business, Education, US Government, China en Duitsland plant.

Deze functie is beschikbaar in preview in Office 365 US Government verdediging en Federal plannen.

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

Voor Azure Active Directory voor het intrekken van een clientcertificaat, moet het AD FS-token hebben de volgende claims:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (Het serienummer van het clientcertificaat)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (De tekenreeks voor de verlener van het clientcertificaat)

Azure Active Directory toegevoegd deze claims naar het vernieuwingstoken als ze beschikbaar in de AD FS-token (of andere SAML-token zijn). Wanneer het vernieuwingstoken dat worden gevalideerd moet, wordt deze informatie wordt gebruikt om te controleren van de intrekking.

Als een best practice moet u van uw organisatie AD FS-foutpagina's bijwerken met de volgende informatie:

* De vereiste voor het installeren van de Microsoft-Authenticator voor Android.
* Instructies voor het ophalen van het certificaat van een gebruiker.

Zie voor meer informatie [aanpassen van de AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

Sommige Office-apps (met moderne verificatie is ingeschakeld) verzenden '*prompt = aanmelding*' naar Azure AD in de aanvraag. Standaard Azure AD vertaalt '*prompt = aanmelding*'in de aanvraag voor ADFS als'*wauth = usernamepassworduri*' (AD FS wilt U/P Auth vragen) en '*wfresh = 0*' (AD FS te vragen status SSO negeren en een nieuwe Authentication uitvoeren). Als u verificatie inschakelen op basis van certificaten voor deze apps wilt, moet u het standaardgedrag voor Azure AD te wijzigen. Stel de '*PromptLoginBehavior*'in de instellingen van het federatieve domein naar'*uitgeschakelde*'.
U kunt de [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet deze taak uit te voeren:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Ondersteuning voor Exchange ActiveSync-clients

Bepaalde Exchange ActiveSync-toepassingen (Lollipop) voor Android 5.0 of hoger worden ondersteund. Om te bepalen of deze functie biedt ondersteuning voor uw e-mailtoepassing, contact op met de ontwikkelaar van uw toepassing.

## <a name="next-steps"></a>Volgende stappen

Als u verificatie configureren op basis van certificaten in uw omgeving wilt, Zie [aan de slag met verificatie op basis van certificaten op Android](active-directory-certificate-based-authentication-get-started.md) voor instructies.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
