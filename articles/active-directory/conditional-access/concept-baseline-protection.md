---
title: Beleid voor voorwaardelijke toegang basislijn beveiliging - Azure Active Directory
description: Basislijn voorwaardelijk beleid voor het beveiligen van organisaties tegen veelvoorkomende aanvallen vanaf
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
ms.openlocfilehash: 39a591a335d022ef7b2b99fdec930ddf0496cd47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112594"
---
# <a name="what-are-baseline-policies"></a>Wat zijn beleidsregels voor basislijn?

Basislijn-beleidsregels zijn een set vooraf gedefinieerde beleidsregels waarmee organisaties tegen algemene aanvallen veel beter te beveiligen. Deze veelvoorkomende aanvallen zijn wachtwoord spray, opnieuw afspelen en phishing. Basislijn-beleidsregels zijn beschikbaar in alle edities van Azure AD. Microsoft maakt deze basislijn beveiligingsbeleid voor apps beschikbaar voor iedereen omdat aanvallen op basis van identiteit op de toename van de afgelopen jaren zijn. Het doel van deze vier beleidsregels is om ervoor te zorgen dat alle organisaties een basisbeveiliging ingeschakeld zonder hebben extra kosten.  

Aangepaste beleidsregels voor voorwaardelijke toegang beheren, is een Azure AD Premium-licentie vereist.

## <a name="baseline-policies"></a>Basislijnbeleid

![Beleid voor voorwaardelijke toegang basislijn in Azure portal](./media/concept-baseline-protection/conditional-access-baseline-policies.png)

Er zijn vier basislijn-beleidsregels die organisaties kunnen inschakelen:

* [MFA vereisen voor beheerders](howto-baseline-protect-administrators.md)
* [Eindgebruiker protection (preview)](howto-baseline-protect-end-users.md)
* [Verouderde blok-verificatie (preview)](howto-baseline-protect-legacy-auth.md)
* [MFA vereisen voor servicebeheer (preview)](howto-baseline-protect-azure.md)

Alle vier van dit beleid is van invloed op verouderde verificatiestromen zoals POP, IMAP en oudere Office-bureaublad-clients.

### <a name="require-mfa-for-admins"></a>MFA vereisen voor beheerders

Vanwege de kracht en de toegang van administrator-accounts, moet u ze behandelen met speciale aandacht. Een veelgebruikte methode voor het verbeteren van de beveiliging van bevoegde accounts is een sterkere vorm van verificatie-account vereist als ze worden gebruikt voor het aanmelden. In Azure Active Directory krijgt u een sterkere accountverificatie doordat beheerders voor registreren en gebruiken van Azure multi-factor Authentication.

[MFA vereisen voor beheerders](howto-baseline-protect-administrators.md) is een baseline-beleid dat is multi-factor authentication (MFA) vereist voor de volgende directory-rollen worden beschouwd als de meeste bevoegdheden Azure AD-rollen:

* Globale beheerder
* SharePoint-beheerder
* Exchange-beheerder
* Voorwaardelijke toegang beheerder
* Beveiligingsbeheerder
* Helpdesk-beheerder / wachtwoordbeheerder
* Factureringsbeheerder
* Gebruikerbeheerder

Als uw organisatie deze accounts in scripts of code wordt gebruikt heeft, kunt u deze met overal vervangen [beheerde identiteiten](../managed-identities-azure-resources/overview.md). Als tijdelijke oplossing, kunt u specifieke gebruikersaccounts uitsluiten van het Basisbeleid.

### <a name="end-user-protection-preview"></a>Eindgebruiker protection (preview)

Hoge bevoorrechte beheerders zijn niet de enige gerichte aanvallen. Ongeldige actoren vaak voor normale gebruikers. Nadat u hebt toegang gekregen, ook deze beveiligingsrisico de toegang tot vertrouwelijke informatie namens de oorspronkelijke accounteigenaar of downloaden van de gehele map en voert een phishing-aanval op de hele organisatie. Er is een veelgebruikte methode voor het verbeteren van de beveiliging voor alle gebruikers een sterkere vorm van verificatie-account vereist wanneer een riskante aanmelding wordt gedetecteerd.

**Eindgebruiker protection (preview)** is een baseline-beleid dat alle gebruikers in een map beveiligt. Als u dit beleid vereist dat alle gebruikers zich registreren voor Azure multi-factor Authentication binnen 14 dagen. Zodra geregistreerd, wordt gebruikers gevraagd voor MFA alleen tijdens de riskante aanmelding pogingen. Verdachte gebruikersaccounts worden geblokkeerd totdat het wachtwoord opnieuw instellen en ontslag van risico.

### <a name="block-legacy-authentication-preview"></a>Verouderde blok-verificatie (preview)

Verouderde verificatieprotocollen (ex: IMAP, SMTP-, POP3-) zijn protocollen die normaal gesproken door oudere e-mailclients worden gebruikt voor verificatie. Verouderde protocollen bieden geen ondersteuning voor meervoudige verificatie. Zelfs als u een beleid voor meervoudige verificatie vereisen voor uw directory hebt, kan een actor slecht verifiëren met behulp van een van deze verouderde protocollen en multi-factor authentication eenmalig overslaan.

Er is de beste manier om uw account beschermen tegen schadelijke verificatieaanvragen door verouderde protocollen te blokkeren dat ze.

De **blok verouderde verificatie (preview)** Basisbeleid verificatieverzoeken die worden gedaan met behulp van verouderde protocollen worden geblokkeerd. Moderne verificatie moet worden gebruikt om het aanmelden voor alle gebruikers. Gebruikt in combinatie met de andere beleidsregels voor de basislijn, worden aanvragen die afkomstig zijn van verouderde protocollen geblokkeerd. Alle gebruikers is bovendien vereist voor MFA wanneer vereist. Dit beleid worden niet geblokkeerd voor Exchange ActiveSync.

### <a name="require-mfa-for-service-management-preview"></a>MFA vereisen voor servicebeheer (preview)

Organisaties gebruiken een groot aantal Azure-services en deze beheren vanuit Azure Resource Manager gebaseerde hulpprogramma's zoals:

* Azure Portal
* Azure PowerShell
* Azure-CLI

Met een van deze hulpprogramma's voor het uitvoeren van resource Manager is een bijzondere rechten actie. Deze hulpprogramma's kunnen brede, door het abonnement configuraties, zoals service-instellingen en facturering van abonnement wijzigen.

Beveiligen van bevoegde acties dit **MFA vereisen voor servicebeheer (preview)** beleid wordt meervoudige verificatie vereisen voor alle gebruikers toegang tot Azure portal, Azure PowerShell of Azure CLI.

## <a name="enable-a-baseline-policy"></a>Een beleid inschakelen

Een Basisbeleid inschakelen:

1. Aanmelden bij de **Azure-portal** als hoofdbeheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer een basislijn-beleid dat u wilt inschakelen in de lijst met beleidsregels.
1. Stel **beleid inschakelen** naar **op**.
1. Klik op opslaan.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten](../../security/azure-ad-secure-steps.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)
* [MFA vereisen voor beheerders](howto-baseline-protect-administrators.md)
* [Eindgebruiker protection (preview)](howto-baseline-protect-end-users.md)
* [Verouderde blok-verificatie (preview)](howto-baseline-protect-legacy-auth.md)
* [MFA vereisen voor servicebeheer (preview)](howto-baseline-protect-azure.md)