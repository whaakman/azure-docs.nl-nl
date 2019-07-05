---
title: Basislijn-beleid MFA vereisen voor beheerders - Azure Active Directory
description: Beleid voor voorwaardelijke toegang voor meervoudige verificatie vereisen voor beheerders
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4474283b9a233e39497cd05f0f04ea0984f02401
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560936"
---
# <a name="baseline-policy-require-mfa-for-admins-preview"></a>Beleid: MFA vereisen voor beheerders (preview)

Gebruikers met toegang tot beschermde accounts hebben onbeperkte toegang tot uw omgeving. Vanwege de stroom die deze accounts hebt, moet u ze behandelen met speciale aandacht. Een veelgebruikte methode voor het verbeteren van de beveiliging van bevoegde accounts is een sterkere vorm van verificatie-account vereist wanneer deze worden gebruikt om aan te melden. In Azure Active Directory krijgt u een sterkere accountverificatie doordat multi-factor authentication (MFA).

**MFA vereisen voor beheerders (preview)**  is een [Basisbeleid](concept-baseline-protection.md) waarvoor MFA is vereist telkens als een van de volgende beheerder met bevoorrechte rollen zich aanmeldt:

* Globale beheerder
* SharePoint-beheerder
* Exchange-beheerder
* Voorwaardelijke toegang beheerder
* Beveiligingsbeheerder
* Helpdesk-beheerder / wachtwoordbeheerder
* Factureringsbeheerder
* Gebruikerbeheerder

Bij het inschakelen van de MFA vereisen voor beheerders beleid voor de bovenstaande negen beheerdersrollen moet zich registreren voor MFA met behulp van de Authenticator-App. Als MFA-registratie voltooid is, wordt beheerders moeten MFA telkens één wanneer ze zich aanmelden bij uitvoeren.

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Omdat de **MFA vereisen voor beheerders (preview)** beleid wordt toegepast op alle essentiële beheerders, verschillende overwegingen hoeven te worden aangebracht in een probleemloze implementatie. Deze overwegingen zijn het identificeren van gebruikers en principes van de service in Azure AD die niet kunnen of moeten niet worden uitgevoerd, MFA, evenals toepassingen en clients die worden gebruikt door uw organisatie die geen ondersteuning voor moderne verificatie.

### <a name="legacy-protocols"></a>Verouderde protocollen

Verouderde verificatieprotocollen (IMAP, SMTP-, POP3-, enzovoort) worden gebruikt door e-mailclients verificatie-aanvragen. Deze protocollen bieden geen ondersteuning voor MFA. De meeste van de account compromissen gezien door Microsoft worden veroorzaakt door ongewenste actoren uitvoeren van aanvallen tegen verouderde protocollen probeert om MFA over te slaan. Om ervoor te zorgen dat MFA vereist is bij het aanmelden bij een Administrator-account en ongewenste actoren niet kunnen MFA overslaan, blokkeert dit beleid alle verificatieaanvragen naar administrator-accounts van verouderde protocollen.

> [!WARNING]
> Voordat u dit beleid inschakelt, zorg ervoor dat uw beheerders zijn niet verouderde verificatieprotocollen. Zie het artikel [het: Verouderde verificatie met Azure AD met voorwaardelijke toegang blokkeren](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) voor meer informatie.

## <a name="enable-the-baseline-policy"></a>De basislijn-beleid inschakelen

Het beleid **Basisbeleid: MFA vereisen voor beheerders (preview)** wordt al geconfigureerd geleverd en worden weergegeven aan de bovenkant wanneer u gaat u naar de blade voor voorwaardelijke toegang in Azure portal.

Dit beleid inschakelt en beveiligen van uw beheerders:

1. Aanmelden bij de **Azure-portal** als hoofdbeheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer in de lijst met beleidsregels **Basisbeleid: MFA vereisen voor beheerders (preview)** .
1. Stel **beleid inschakelen** naar **beleid direct gebruiken**.
1. Klik op **opslaan**.

> [!WARNING]
> Er is een optie **automatisch inschakelen van beleid in de toekomst** waarop dit beleid is in preview. We hebben verwijderd deze optie om te minimaliseren impact op plotselinge gebruikers. Als u deze optie hebt geselecteerd, wanneer deze beschikbaar is, is **beleid niet gebruiken** wordt automatisch geselecteerd. Als ze willen gebruiken deze basislijn-beleid, raadpleegt u bovenstaande stappen om te schakelen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Beleid voor voorwaardelijke toegang basislijn-beveiliging](concept-baseline-protection.md)
* [Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten](../../security/azure-ad-secure-steps.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)
