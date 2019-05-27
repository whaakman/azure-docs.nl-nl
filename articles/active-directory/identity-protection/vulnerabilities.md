---
title: Beveiligingsproblemen die worden gedetecteerd door Azure Active Directory Identity Protection
description: Overzicht van de beveiligingsproblemen die worden gedetecteerd door Azure Active Directory Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.date: 04/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e792551f4cac857f56454c67d527e01cb9c4281
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113129"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Beveiligingsproblemen die worden gedetecteerd door Azure Active Directory Identity Protection

Beveiligingslekken in zwakke punten in een omgeving die kunnen worden misbruikt door een aanvaller zijn. Het is raadzaam om beheerders deze beveiligingslekken ter verbetering van de beveiligingsstatus van hun organisatie.

![Zwakke plekken die zijn gerapporteerd door Identity Protection](./media/vulnerabilities/identity-protection-vulnerabilities.png)

De volgende secties vindt u een overzicht van de beveiligingsproblemen die zijn gerapporteerd door Identity Protection.

## <a name="multi-factor-authentication-registration-not-configured"></a>Multi-factor Authentication-registratie niet is geconfigureerd

Dit beveiligingsprobleem helpt bij het beoordelen van de implementatie van Azure multi-factor Authentication in uw organisatie.

Azure multi-factor Authentication biedt een tweede beveiligingslaag voor verificatie van de gebruiker. Deze om de toegang tot gegevens en toepassingen beveiligen terwijl aan de wensen van gebruikers voor een eenvoudige aanmeldprocedure. Azure multi-factor Authentication biedt verificatieopties voor van eenvoudig te gebruiken, zoals:

* Telefoonoproep
* Sms-bericht
* Melding van een mobiele app
* OTP-verificatiecode

Het is raadzaam dat u Azure multi-factor Authentication voor gebruikersaanmeldingen nodig hebt. Meervoudige verificatie speelt een belangrijke rol in het beleid voor voorwaardelijke toegang op basis van risico beschikbaar via Identity Protection.

Voor meer informatie raadpleegt u [Wat is Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Onbeheerde cloud-apps

Hierdoor kunt u niet-beheerde cloud-apps in uw organisatie identificeren.

IT-medewerkers zijn vaak niet op de hoogte van alle cloud-toepassingen in hun organisatie. Het is gemakkelijk om te zien waarom beheerders zou twijfels hebt over de niet-geautoriseerde toegang tot zakelijke gegevens, mogelijke gegevenslekken en andere beveiligingsrisico's.

Het is raadzaam implementeren van Cloud Discovery voor het detecteren van niet-beheerde cloud-toepassingen, en voor het beheren van deze toepassingen met Azure Active Directory.

Zie voor meer informatie, [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Beveiligingswaarschuwingen van Privileged Identity Management

Dit beveiligingsprobleem helpt u bij het opsporen en oplossen van waarschuwingen over bevoorrechte identiteiten in uw organisatie.  

Als u wilt dat gebruikers kunnen bevoorrechte bewerkingen uitvoeren, organisaties moeten gebruikers tijdelijk of permanent bevoorrechte toegang verlenen in Azure AD, Azure of Office 365-resources en andere SaaS-apps. Elk van deze gebruikers met bevoegdheden verhoogt de kwetsbaarheid van uw organisatie. Dit beveiligingsprobleem kunt u gebruikers identificeren met onnodige bevoegde toegang en passende actie ondernemen om te verlagen of elimineren van het risico dat ze met zich meebrengen.

We raden organisaties gebruik Azure AD Privileged Identity Management te beheren, beheren en bewaken van bevoorrechte identiteiten in Azure AD, evenals andere Microsoft online services zoals Office 365 en Microsoft Intune.

Zie [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md) voor meer informatie.

## <a name="see-also"></a>Zie ook

[Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
