---
title: Beveiligings problemen gedetecteerd door Azure Active Directory Identity Protection
description: Overzicht van de beveiligings problemen die worden gedetecteerd door Azure Active Directory Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 446e2ef33b714afe06f24f3dbfc46bc6a42790b8
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335147"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Beveiligings problemen gedetecteerd door Azure Active Directory Identity Protection

Beveiligings problemen zijn zwakke plekken in een omgeving die door een aanvaller kan worden misbruikt. Beheerders wordt aangeraden deze beveiligings problemen op te lossen om de beveiligings postuur van hun organisatie te verbeteren.

![Beveiligings problemen die worden gerapporteerd door identiteits beveiliging](./media/vulnerabilities/identity-protection-vulnerabilities.png)

In de volgende secties vindt u een overzicht van de beveiligings problemen die worden gerapporteerd door identiteits beveiliging.

## <a name="multi-factor-authentication-registration-not-configured"></a>Multi-factor Authentication-registratie is niet geconfigureerd

Dit beveiligings probleem helpt bij het beoordelen van de implementatie van Azure multi-factor Authentication in uw organisatie.

Azure multi-factor Authentication biedt gebruikers verificatie een tweede beveiligingslaag. Het helpt de toegang tot gegevens en toepassingen te beschermen terwijl de vraag naar gebruikers wordt gevergaderd voor een eenvoudig aanmeldings proces. Met Azure multi-factor Authentication kunt u op eenvoudige wijze verificatie opties gebruiken, zoals:

* Telefoongesprek
* Sms-bericht
* Melding van een mobiele app
* OTP-verificatie code

U wordt aangeraden Azure multi-factor Authentication voor gebruikers aanmeldingen te vereisen. Multi-factor Authentication speelt een belang rijke rol in op risico gebaseerd beleid voor voorwaardelijke toegang dat beschikbaar is via identiteits beveiliging.

Voor meer informatie raadpleegt u [Wat is Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Onbeheerde cloud-apps

Dit beveiligings probleem helpt u bij het identificeren van onbeheerde Cloud-apps in uw organisatie.

IT-personeel is vaak niet op de hoogte van alle Cloud toepassingen in hun organisatie. Het is eenvoudig om te zien waarom beheerders problemen hebben met onbevoegde toegang tot Bedrijfs gegevens, mogelijke gegevens lekken en andere beveiligings Risico's.

We raden u aan om Cloud Discovery te implementeren om niet-beheerde Cloud toepassingen te detecteren en om deze toepassingen te beheren met behulp van Azure Active Directory.

Zie [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery)voor meer informatie.

## <a name="security-alerts-from-privileged-identity-management"></a>Beveiligingswaarschuwingen van Privileged Identity Management

Dit beveiligings probleem helpt u bij het detecteren en oplossen van waarschuwingen over bevoorrechte identiteiten in uw organisatie.  

Om gebruikers in staat te stellen om privileged-bewerkingen uit te voeren, moeten organisaties tijdelijke of permanente toegang verlenen voor gebruikers in azure AD, Azure of Office 365 resources of andere SaaS-apps. Elk van deze bevoegde gebruikers verhoogt de kwets baarheid van uw organisatie. Dit beveiligings probleem helpt u bij het identificeren van gebruikers met onnodige privileged Access en het nemen van passende maat regelen om het risico te beperken of te elimineren dat ze vormen.

We raden organisaties aan Azure AD Privileged Identity Management te gebruiken om geprivilegieerde identiteiten in azure AD te beheren, beheren en controleren, evenals andere micro soft-onlineservices, zoals Office 365 of Microsoft Intune.

Zie [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md) voor meer informatie.

## <a name="see-also"></a>Zie ook

[Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
