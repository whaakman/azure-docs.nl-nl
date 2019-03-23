---
title: Verificatie op basis van certificaten op iOS - Azure Active Directory
description: Meer informatie over de ondersteunde scenario's en de vereisten voor certificaten gebaseerde verificatie configureren in oplossingen met iOS-apparaten
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: cda1b1c2a484f3aa627b8b9cf486528d13f27be8
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369425"
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>Azure Active Directory gebaseerde verificatie op iOS

iOS-apparaten kunnen u certificaten gebaseerde verificatie gebruiken om te verifiëren bij Azure Active Directory met een clientcertificaat op hun apparaat bij het verbinden met:

* Mobiele Office-toepassingen zoals Microsoft Outlook en Microsoft Word
* Exchange ActiveSync (EAS) clients

Configuratie van deze functie elimineert de noodzaak om een combinatie van gebruikersnaam en wachtwoord bepaalde e-mail en Microsoft Office-toepassingen in uw mobiele apparaat.

In dit onderwerp vindt u met de vereisten en de ondersteunde scenario's voor het configureren van deze verificatie op een apparaat iOS(Android) voor gebruikers van tenants in Office 365 Enterprise, bedrijven, onderwijs, US Government, China en Duitsland-abonnementen.

Deze functie is beschikbaar in preview in Office 365 US Government Defense en federale plannen.

## <a name="microsoft-mobile-applications-support"></a>Ondersteuning voor mobiele toepassingen van Microsoft

| Apps | Ondersteuning |
| --- | --- |
| Azure Information Protection-app |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| Intune-bedrijfsportal |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| Microsoft Teams |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| OneNote |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| OneDrive |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| Outlook |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| Power BI |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| Skype voor Bedrijven |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| Word / Excel / PowerPoint |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |
| Yammer |![Vinkje, wat inhoudt ondersteuning voor deze toepassing][1] |

## <a name="requirements"></a>Vereisten

De versie van het besturingssysteem van het apparaat moet iOS 9 en hoger

Een federation-server moet worden geconfigureerd.

Microsoft Authenticator is vereist voor Office-toepassingen in iOS.

Voor Azure Active Directory om een clientcertificaat intrekken, moet de AD FS-token hebben de volgende claims:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (Het serienummer van het clientcertificaat)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (De tekenreeks voor de verlener van het clientcertificaat)

Deze claims voor het vernieuwingstoken dat door Azure Active Directory toegevoegd als deze beschikbaar in de AD FS-token (of andere SAML-token zijn). Wanneer het vernieuwingstoken dat worden gevalideerd moet, wordt deze informatie wordt gebruikt om te controleren of de certificaatintrekkingslijst.

Als een best practice, moet u van uw organisatie AD FS-foutpagina's bijwerken met de volgende informatie:

* De vereiste voor het installeren van de Microsoft Authenticator voor iOS
* Instructies voor het ophalen van het certificaat van een gebruiker.

Zie voor meer informatie, [aanpassen van de AD FS Sign-in's](https://technet.microsoft.com/library/dn280950.aspx).

Sommige Office-apps (met moderne verificatie ingeschakeld) verzenden '*prompt = aanmelding*' naar Azure AD in de aanvraag. Standaard Azure AD wordt omgezet '*prompt = aanmelding*'in de aanvraag voor AD FS als'*wauth = usernamepassworduri*' (AD FS wilt U/P Auth vraagt) en '*wfresh = 0*' (AD FS om te vragen negeren van SSO-staat en voer een nieuwe verificatie). Als u verificatie inschakelen op basis van certificaten voor deze apps wilt, moet u het standaardgedrag van de Azure AD te wijzigen. Stelt u de '*PromptLoginBehavior*'in de instellingen van uw federatief domein naar'*uitgeschakelde*'.
U kunt de [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet deze taak uit te voeren:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Ondersteuning voor Exchange ActiveSync-clients

Op iOS 9 of hoger, wordt de systeemeigen iOS-e-mailclient ondersteund. Voor alle andere Exchange ActiveSync-toepassingen, om te bepalen of deze functie wordt ondersteund, neem contact op met de ontwikkelaar van uw toepassing.

## <a name="next-steps"></a>Volgende stappen

Als u configureren van verificatie op basis van het certificaat in uw omgeving wilt, Zie [aan de slag met verificatie op basis van certificaten op Android](../authentication/active-directory-certificate-based-authentication-get-started.md) voor instructies.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
