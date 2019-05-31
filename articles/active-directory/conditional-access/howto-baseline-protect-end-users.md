---
title: Basisbeleid eindgebruiker protection (preview) - Azure Active Directory
description: Beleid voor voorwaardelijke toegang voor meervoudige verificatie vereisen voor gebruikers
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
ms.openlocfilehash: 104665774eee885cc2f562e9813cffcf23aa943e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235510"
---
# <a name="baseline-policy-end-user-protection-preview"></a>Beleid: Eindgebruiker protection (preview)

We vaak om na te denken dat beheerdersaccounts zijn de enige accounts die u wilt met multi-factor authentication (MFA beveiligen). Beheerders hebben ruime toegang tot gevoelige informatie en kunnen wijzigingen aanbrengen in de instellingen voor brede, door het abonnement. Ongeldige actoren doorgaans echter doel eindgebruikers. Nadat u hebt toegang gekregen, kunt deze beveiligingsrisico toegang aanvragen tot vertrouwelijke informatie namens de oorspronkelijke accounteigenaar of downloaden van de gehele map om uit te voeren een phishing-aanval op de hele organisatie. Er is een veelgebruikte methode voor het verbeteren van de beveiliging voor alle gebruikers om te vereisen een sterkere vorm van verificatie-account, zoals multi-factor authentication (MFA).

Voor het bereiken van een redelijke evenwicht tussen beveiliging en gebruiksgemak, al dan niet mogen gebruikers telkens één wanneer ze zich aanmelden bij worden gevraagd. Verificatieaanvragen die overeenkomen met normale gebruikersgedrag, zoals ze zich aanmelden vanaf het apparaat vanaf dezelfde locatie, hebben een lage kans van inbreuk. Alleen aanmeldingen die als riskant worden beschouwd en weergeven van de kenmerken van een actor slecht moet worden gevraagd MFA uitdagingen met zich mee.

![MFA vereisen voor gebruikers](./media/howto-baseline-protect-end-users/baseline-policy-end-user-protection.png)

Beveiliging van de eindgebruiker is een risico's gebaseerde MFA [Basisbeleid](concept-baseline-protection.md) die alle gebruikers in een map, inclusief alle beheerdersrollen beveiligt. Als u dit beleid vereist dat alle gebruikers zich registreren voor MFA met behulp van de Authenticator-App. Gebruikers kunnen de prompt MFA-registratie overslaan gedurende 14 dagen, waarna ze worden geblokkeerd en aanmelden totdat ze zich voor MFA registreren. Zodra geregistreerd voor MFA, wordt gebruikers gevraagd voor MFA alleen tijdens de riskante aanmelding pogingen. Verdachte gebruikersaccounts worden geblokkeerd totdat hun wachtwoord opnieuw is ingesteld en risicogebeurtenissen is gesloten.

> [!NOTE]
> Dit beleid is van toepassing op alle gebruikers, met inbegrip van Gast-account en wordt geëvalueerd bij het aanmelden bij alle toepassingen.

## <a name="recovering-compromised-accounts"></a>Herstellen van accounts aangetast

Ter bescherming van onze klanten, zoekt de service van Microsoft voor de referentie is gelekt paren met openbaar beschikbare gebruikersnaam en wachtwoord. Als ze overeenkomen met een van onze gebruikers, wij helpen om dat account direct beveiligen. Gebruikers die worden aangeduid als bestaande uit een gelekte referentie worden bevestigd aangetast. Deze gebruikers kunnen worden aangemeld totdat hun wachtwoord opnieuw is ingesteld.

Een Azure AD Premium-licentie toegewezen gebruikers kan toegang via selfservice voor wachtwoordherstel (SSPR) kunnen herstellen als de mogelijkheid is ingeschakeld in de directory. Gebruikers zonder een premium-licentie die worden geblokkeerd, moeten contact opnemen met een beheerder een handmatige wachtwoord opnieuw kunnen instellen en de gemarkeerde gebruiker risicogebeurtenis negeren.

### <a name="steps-to-unblock-a-user"></a>Stappen voor het deblokkeren van een gebruiker

Bevestig dat de gebruiker is geblokkeerd door het beleid door het controleren van de gebruiker aanmelden Logboeken.

1. Er moet een beheerder zich aanmeldt bij de **Azure-portal** en navigeer naar **Azure Active Directory** > **gebruikers** > Klik op de naam van de gebruiker en Ga aan aanmeldingen.
1. Om te starten voor wachtwoord opnieuw instellen op een geblokkeerde gebruiker, moet een beheerder om te navigeren naar **Azure Active Directory** > **gebruikers die zijn gemarkeerd voor risico's**
1. Klik op de gebruiker waarvan het account is geblokkeerd om informatie over recente van de gebruiker aanmelden activiteit weer te geven.
1. Klik op wachtwoord opnieuw instellen als u wilt toewijzen van een tijdelijk wachtwoord dat moet worden gewijzigd bij de volgende aanmelding.
1. Klik op alle gebeurtenissen om de risicoscore van de gebruiker opnieuw in te sluiten.

De gebruiker kan nu aanmelden, hun wachtwoord opnieuw instellen en toegang tot de toepassing.

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Omdat de **eindgebruiker protection** beleid wordt toegepast op alle gebruikers in uw directory, verschillende overwegingen hoeven te worden aangebracht in een probleemloze implementatie. Deze overwegingen zijn het identificeren van gebruikers en principes van de service in Azure AD die niet kunnen of moeten niet worden uitgevoerd, MFA, evenals toepassingen en clients die worden gebruikt door uw organisatie die geen ondersteuning voor moderne verificatie.

### <a name="legacy-protocols"></a>Verouderde protocollen

Verouderde verificatieprotocollen (IMAP, SMTP-, POP3-, enzovoort) worden gebruikt door e-mailclients verificatie-aanvragen. Deze protocollen bieden geen ondersteuning voor MFA.  De meeste van de account compromissen gezien door Microsoft worden veroorzaakt door ongewenste actoren uitvoeren van aanvallen tegen verouderde protocollen probeert om MFA over te slaan. Om ervoor te zorgen dat MFA vereist is bij het aanmelden bij een account en ongewenste actoren niet kunnen MFA overslaan, blokkeert dit beleid alle verificatieaanvragen naar administrator-accounts van verouderde protocollen.

> [!WARNING]
> Voordat u dit beleid inschakelt, zorg ervoor dat uw gebruikers zijn niet verouderde verificatieprotocollen. Zie het artikel [het: Verouderde verificatie met Azure AD met voorwaardelijke toegang blokkeren](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) voor meer informatie.

### <a name="user-exclusions"></a>Uitsluitingen van gebruiker

Dit beleid biedt u de optie voor het uitsluiten van gebruikers. Voordat u het beleid inschakelt voor uw tenant, raden we u aan met uitzondering van de volgende accounts:

* **Voor toegang in noodgevallen** of **break-glas** accounts om te voorkomen dat tenant-brede accountvergrendeling. In het onwaarschijnlijke scenario dat alle beheerders toegang tot uw tenant worden geblokkeerd, kan uw EMS-access-Administrator-account worden gebruikt voor aanmelding bij de stappen voor het nemen van tenant toegang verkrijgen.
   * Meer informatie vindt u in het artikel [toegang in noodgevallen accounts beheren in Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Serviceaccounts** en **service principes**, zoals de Azure AD Connect Sync-Account. Service-accounts zijn niet-interactieve accounts die niet zijn gekoppeld aan een bepaalde gebruiker. Ze normaal gesproken worden gebruikt door back-endservices en programmatische toegang tot toepassingen. Service-accounts moeten worden uitgesloten omdat MFA via een programma kan niet worden voltooid.
   * Als uw organisatie deze accounts in scripts of code wordt gebruikt heeft, kunt u deze met overal vervangen [beheerde identiteiten](../managed-identities-azure-resources/overview.md). Als tijdelijke oplossing kunt kunt u deze specifieke accounts uitsluiten van de basislijn-beleid.
* Gebruikers die geen of is niet mogelijk een Smartphone gebruiken.
   * Dit beleid vereist dat gebruikers zich registreren voor MFA via de Microsoft Authenticator-app.

## <a name="enable-the-baseline-policy"></a>De basislijn-beleid inschakelen

Het beleid **Basisbeleid: Eindgebruiker protection (preview)** wordt al geconfigureerd geleverd en worden weergegeven aan de bovenkant wanneer u gaat u naar de blade voor voorwaardelijke toegang in Azure portal.

Dit beleid inschakelt en Bescherm uw gebruikers:

1. Aanmelden bij de **Azure-portal** als hoofdbeheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer in de lijst met beleidsregels **Basisbeleid: Eindgebruiker protection (preview)** .
1. Stel **beleid inschakelen** naar **beleid direct gebruiken**.
1. Gebruiker uitsluitingen toevoegen door te klikken op **gebruikers** > **uitgesloten gebruikers selecteren** en het kiezen van de gebruikers die moeten worden uitgesloten. Klik op **Selecteer** vervolgens **gedaan**.
1. Klik op **opslaan**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Basislijn protection-beleid voor voorwaardelijke toegang](concept-baseline-protection.md)
* [Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten](../../security/azure-ad-secure-steps.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)
