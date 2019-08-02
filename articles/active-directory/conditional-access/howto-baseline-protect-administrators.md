---
title: Voor het basislijn beleid is MFA vereist voor beheerders-Azure Active Directory
description: Beleid voor voorwaardelijke toegang om multi-factor Authentication voor beheerders te vereisen
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
ms.openlocfilehash: 213540a5b6c77146155365133f2cca08eea25351
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68608160"
---
# <a name="baseline-policy-require-mfa-for-admins-preview"></a>Basislijn beleid: MFA vereisen voor beheerders (preview-versie)

Gebruikers met toegang tot geprivilegieerde accounts hebben onbeperkte toegang tot uw omgeving. Als gevolg van de kracht van deze accounts, moet u deze met speciale zorg behandelen. Een gemeen schappelijke methode voor het verbeteren van de beveiliging van geprivilegieerde accounts is om een sterkere vorm van account verificatie te vereisen wanneer deze worden gebruikt om u aan te melden. In Azure Active Directory kunt u een sterkere account verificatie krijgen door multi-factor Authentication (MFA) te vereisen.

**MFA vereisen voor beheerders (preview)**  is een [basislijn beleid](concept-baseline-protection.md) dat MFA vereist telkens wanneer een van de volgende bevoegde beheerders rollen zich aanmeldt:

* Globale beheerder
* SharePoint-beheerder
* Exchange-beheerder
* Beheerder voor voorwaardelijke toegang
* Beveiligingsbeheerder
* Helpdesk beheerder/wachtwoord beheerder
* Factureringsbeheerder
* Gebruikersbeheerder

Bij het inschakelen van het beleid MFA vereisen voor beheerders is de bovenstaande negen beheerders rollen vereist voor de registratie voor MFA met de verificator-app. Zodra MFA-registratie is voltooid, moeten beheerders elke keer dat ze zich aanmelden een MFA uitvoeren.

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Omdat het beleid **vereisen MFA voor beheerders (preview)** van toepassing is op alle kritieke beheerders, moeten er verschillende overwegingen worden gemaakt om een soepele implementatie te garanderen. Deze overwegingen omvatten het identificeren van gebruikers en service principes in azure AD die geen gebruik kunnen maken van MFA en toepassingen en clients die door uw organisatie worden gebruikt en die geen ondersteuning bieden voor moderne verificatie.

### <a name="legacy-protocols"></a>Verouderde protocollen

Verouderde verificatie protocollen (IMAP, SMTP, POP3, enz.) worden door e-mailclients gebruikt om verificatie aanvragen uit te voeren. Deze protocollen bieden geen ondersteuning voor MFA. De meeste schendingen van het account die door micro soft worden gezien, worden veroorzaakt door ongeldige Actors die aanvallen uitvoeren op verouderde protocollen, waardoor MFA wordt omzeild. Om ervoor te zorgen dat MFA vereist is wanneer u zich aanmeldt bij een beheerders account en beschadigde actors niet in staat zijn om MFA over te slaan, blokkeert dit beleid alle verificatie aanvragen voor beheerders accounts van verouderde protocollen.

> [!WARNING]
> Voordat u dit beleid inschakelt, moet u ervoor zorgen dat uw beheerders geen verouderde verificatie protocollen gebruiken. Zie het artikel [: Blok keer verouderde verificatie voor Azure AD](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) met voorwaardelijke toegang voor meer informatie.

## <a name="enable-the-baseline-policy"></a>Het basislijn beleid inschakelen

Beleid voor **beleids basislijn: Vereisen dat MFA voor beheerders (preview)** vooraf is geconfigureerd en bovenaan wordt weer gegeven wanneer u navigeert naar de Blade voorwaardelijke toegang in azure Portal.

Om dit beleid in te scha kelen en uw beheerders te beschermen:

1. Meld u aan bij de **Azure Portal** als globale beheerder, beveiligings beheerder of beheerder van de voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer **basislijn beleid in de lijst met beleids regels: MFA vereisen voor beheerders (preview-versie**).
1. Stel **beleid inschakelen** om **beleid direct te gebruiken**in.
1. Klik op **Opslaan**.

> [!WARNING]
> Er is een optie om **beleid in de toekomst automatisch in te scha kelen** wanneer dit beleid in de preview-versie was. Deze optie is verwijderd om plotselinge gevolgen voor de gebruiker te minimaliseren. Als u deze optie hebt geselecteerd toen deze beschikbaar was, wordt het **beleid niet gebruiken** automatisch geselecteerd. Als u dit basislijn beleid wilt gebruiken, raadpleegt u de bovenstaande stappen om dit in te scha kelen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Basis beveiligings beleid voor voorwaardelijke toegang](concept-baseline-protection.md)
* [Vijf stappen voor het beveiligen van uw identiteits infrastructuur](../../security/fundamentals/steps-secure-identity.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)
