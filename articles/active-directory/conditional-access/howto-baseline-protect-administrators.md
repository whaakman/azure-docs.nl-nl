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
ms.openlocfilehash: a1ce48126c3e8867ac7f2696d8cf7db992a9a60a
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003279"
---
# <a name="baseline-policy-require-mfa-for-admins"></a>Beleid: MFA vereisen voor beheerders

Gebruikers met toegang tot beschermde accounts hebben onbeperkte toegang tot uw omgeving. Vanwege de stroom die deze accounts hebt, moet u ze behandelen met speciale aandacht. Een veelgebruikte methode voor het verbeteren van de beveiliging van bevoegde accounts is een sterkere vorm van verificatie-account vereist wanneer deze worden gebruikt om aan te melden. In Azure Active Directory krijgt u een sterkere accountverificatie doordat multi-factor authentication (MFA).

**MFA vereisen voor beheerders** is een [Basisbeleid](concept-baseline-protection.md) waarvoor MFA is vereist telkens als een van de volgende beheerder met bevoorrechte rollen zich aanmeldt:

* Globale beheerder
* SharePoint-beheerder
* Exchange-beheerder
* Voorwaardelijke-toegangsbeheerder
* Beveiligingsbeheerder
* Helpdesk-beheerder / wachtwoordbeheerder
* Factureringsbeheerder
* Gebruikersbeheerder

Bij het inschakelen van de MFA vereisen voor beheerders beleid voor de bovenstaande negen beheerdersrollen moet zich registreren voor MFA met behulp van de Authenticator-App. Als MFA-registratie voltooid is, wordt beheerders moeten MFA telkens één wanneer ze zich aanmelden bij uitvoeren.

![MFA vereisen voor basisbeleid voor beheerders](./media/howto-baseline-protect-administrators/baseline-policy-require-mfa-for-admins.png)

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Omdat de **MFA vereisen voor beheerders** beleid wordt toegepast op alle essentiële beheerders, verschillende overwegingen hoeven te worden aangebracht in een probleemloze implementatie. Deze overwegingen zijn het identificeren van gebruikers en principes van de service in Azure AD die niet kunnen of moeten niet worden uitgevoerd, MFA, evenals toepassingen en clients die worden gebruikt door uw organisatie die geen ondersteuning voor moderne verificatie.

### <a name="legacy-protocols"></a>Verouderde protocollen

Verouderde verificatieprotocollen (IMAP, SMTP-, POP3-, enzovoort) worden gebruikt door e-mailclients verificatie-aanvragen. Deze protocollen bieden geen ondersteuning voor MFA. De meeste van de account compromissen gezien door Microsoft worden veroorzaakt door ongewenste actoren uitvoeren van aanvallen tegen verouderde protocollen probeert om MFA over te slaan. Om ervoor te zorgen dat MFA vereist is bij het aanmelden bij een Administrator-account en ongewenste actoren niet kunnen MFA overslaan, blokkeert dit beleid alle verificatieaanvragen naar administrator-accounts van verouderde protocollen.

> [!WARNING]
> Voordat u dit beleid inschakelt, zorg ervoor dat uw beheerders zijn niet verouderde verificatieprotocollen. Zie het artikel [het: Verouderde verificatie met Azure AD met voorwaardelijke toegang blokkeren](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) voor meer informatie.

### <a name="user-exclusions"></a>Uitsluitingen van gebruiker

Dit beleid biedt u de optie voor het uitsluiten van gebruikers. Voordat u het beleid inschakelt voor uw tenant, raden we u aan met uitzondering van de volgende accounts:

* **Voor toegang in noodgevallen** of **break-glas** accounts om te voorkomen dat tenant-brede accountvergrendeling. In het onwaarschijnlijke scenario dat alle beheerders toegang tot uw tenant worden geblokkeerd, kan uw EMS-access-Administrator-account worden gebruikt voor aanmelding bij de stappen voor het nemen van tenant toegang verkrijgen.
   * Meer informatie vindt u in het artikel [toegang in noodgevallen accounts beheren in Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Serviceaccounts** en **service principes**, zoals de Azure AD Connect Sync-Account. Service-accounts zijn niet-interactieve accounts die niet zijn gekoppeld aan een bepaalde gebruiker. Ze normaal gesproken worden gebruikt door back-endservices en programmatische toegang tot toepassingen. Service-accounts moeten worden uitgesloten omdat MFA via een programma kan niet worden voltooid.
   * Als uw organisatie deze accounts in scripts of code wordt gebruikt heeft, kunt u deze met overal vervangen [beheerde identiteiten](../managed-identities-azure-resources/overview.md). Als tijdelijke oplossing kunt kunt u deze specifieke accounts uitsluiten van de basislijn-beleid.
* Gebruikers die geen of is niet mogelijk een Smartphone gebruiken.
   * Dit beleid moet beheerders om te registreren voor MFA via de Microsoft Authenticator-app.

## <a name="enable-the-baseline-policy"></a>De basislijn-beleid inschakelen

Het beleid **Basisbeleid: MFA vereisen voor beheerders** wordt al geconfigureerd geleverd en worden weergegeven aan de bovenkant wanneer u gaat u naar de blade voor voorwaardelijke toegang in Azure portal.

Dit beleid inschakelt en beveiligen van uw beheerders:

1. Aanmelden bij de **Azure-portal** als hoofdbeheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer in de lijst met beleidsregels **Basisbeleid: MFA vereisen voor beheerders**.
1. Stel **beleid inschakelen** naar **beleid direct gebruiken**.
1. Gebruiker uitsluitingen toevoegen door te klikken op **gebruikers** > **uitgesloten gebruikers selecteren** en het kiezen van de gebruikers die moeten worden uitgesloten. Klik op **Selecteer** vervolgens **gedaan**.
1. Klik op **opslaan**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Basislijn protection-beleid voor voorwaardelijke toegang](concept-baseline-protection.md)
* [Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten](../../security/azure-ad-secure-steps.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)
