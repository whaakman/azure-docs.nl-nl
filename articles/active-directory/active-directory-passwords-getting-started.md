---
title: 'Snelstartgids: Azure AD-SSPR | Microsoft Docs'
description: Wachtwoorden snel opnieuw instellen voor Azure AD via self-service
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7d97fad04a0aa549d0e3a182282f898302e8e41a
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Snelle implementatie van Azure AD-selfservice voor wachtwoordherstel

> [!IMPORTANT]
> **Bent u hier terechtgekomen omdat u problemen ondervindt met het aanmelden?** Als dat het geval is, vindt u hier meer informatie over het [wijzigen en opnieuw instellen van uw eigen wachtwoord](active-directory-passwords-update-your-own-password.md).

De self-service voor wachtwoordherstel (SSPR) biedt een eenvoudige manier voor IT-beheerders om gebruikers in staat te stellen hun eigen wachtwoord of account opnieuw in te stellen of te ontgrendelen. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem gebruiken. U ontvangt ook meldingen om u te waarschuwen over misbruik.

In deze handleiding wordt ervan uitgegaan dat u een werkende proefversie hebt van de Azure AD-tenant of dat u er een licentie voor hebt. Als u hulp nodig hebt bij het instellen van Azure AD ziet u het artikel [Aan de slag met Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>SSPR inschakelen voor uw Azure Active Directory-tenant

1. Selecteer in uw bestaande Azure AD-tenant de optie **Wachtwoord opnieuw instellen**

2. In het scherm **Eigenschappen** kiest u onder 'Selfservice voor wachtwoord opnieuw instellen is ingeschakeld' een van de volgende opties:
    * Niemand: niemand mag de SSPR-functionaliteit gebruiken.
    * Geselecteerde gebruikers: alleen leden van een specifieke Azure Active Directory-groep die u kiest mogen de SSPR-functionaliteit gebruiken. Het is raadzaam om een groep gebruikers te definiëren en deze instelling te gebruiken wanneer u dit implementeert voor een proof-of-concept.
    * Iedereen: alle gebruikers met een account in uw Azure Active Directory-tenant mogen de SSPR-functionaliteit gebruiken. U wordt aangeraden dit in te stellen wanneer u klaar bent voor het implementeren van deze functionaliteit in uw hele tenant nadat u een proof-of-concept hebt voltooid.

3. In het scherm **Verificatiemethoden** kiest u een van de volgende opties
    * Aantal methoden dat is vereist om het wachtwoord opnieuw in te stellen: minimaal een en maximaal twee
    * Methoden voor gebruikers: er is er ten minste één nodig, maar het kan geen kwaad om een extra keuze beschikbaar te maken
        * **E-mailadres**: er wordt een e-mail met een code verstuurd naar het e-mailadres voor verificatie dat door de gebruiker is geconfigureerd
        * **Mobiele telefoon** geeft de gebruiker de keuze om een oproep of sms-bericht te ontvangen met een code, via het geconfigureerde mobiele telefoonnummer
        * **Zakelijke telefoon**: hiermee wordt de gebruiker voor een code gebeld via het geconfigureerde zakelijke telefoonnummer
        * **Beveiligingsvragen**: hiervoor moet u een optie kiezen
            * Het aantal vragen dat is vereist voor de registratie: het minimale aantal voor een succesvolle registratie. Dit betekent dat een gebruiker meer vragen kan beantwoorden en dat er een groep vragen wordt gemaakt waaruit kan worden gekozen. Deze optie kan worden ingesteld op 3, 4 of 5. Het aantal moet gelijk zijn aan of groter zijn dan het aantal vragen dat moet opnieuw moet worden ingesteld.
                * U kunt aangepaste vragen toevoegen door op Aangepast te klikken bij het selecteren van beveiligingsvragen
            * Het aantal vragen dat is vereist om het wachtwoord opnieuw in te stellen: kan worden ingesteld op 3, 4 of 5 vragen. Deze vragen moeten correct worden beantwoord; daarna mag een gebruiker zijn of haar wachtwoord pas opnieuw instellen of ontgrendelen.
            
    ![Verificatie][Authentication]

4. AANBEVOLEN: in **Aanpassing** kunt u de koppeling 'Contact opnemen met de beheerder' laten leiden naar een zelf gekozen pagina of e-mailadres. U wordt aangeraden om deze koppeling in te stellen op bijvoorbeeld een e-mailadres of een website die uw gebruikers gebruiken voor ondersteuning.

5. OPTIONEEL: in het scherm **Registratie** hebben beheerders verschillende opties:
    * Vereisen dat gebruiker zich bij aanmelding registreren
    * Het aantal dagen waarna gebruikers wordt gevraagd om de verificatiegegevens opnieuw te bevestigen

6. OPTIONEEL: in het scherm **Melding** hebben beheerders verschillende opties:
    * Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord
    * Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen

**U hebt op dit moment SSPR geconfigureerd voor uw Azure AD-tenant**. Uw gebruikers kunnen de instructies in de artikelen [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md) en [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md) nu gebruiken om hun wachtwoord bij te werken zonder tussenkomst van de beheerder. U kunt hier stoppen als u alleen de cloud gebruikt of doorgaan om het synchroniseren van wachtwoorden met een on-premises AD-domein te configureren.

> [!IMPORTANT]
> Test self-service voor wachtwoordherstel met een gebruiker en niet als beheerder, aangezien Microsoft sterke verificatievereisten afdwingt voor Azure-accounts van beheerders. Zie ons [artikel over wachtwoordbeleid](active-directory-passwords-policy.md#administrator-password-policy-differences) voor meer informatie over het wachtwoordbeleid voor beheerders.

## <a name="configure-synchronization-to-existing-identity-source"></a>Synchronisatie met bestaande identiteitsbronnen configureren

Als u on-premises identiteitssynchronisatie met Azure AD wilt inschakelen, moet u [Azure AD Connect](./connect/active-directory-aadconnect.md) installeren en configureren op een server in uw organisatie. Deze toepassing draagt zorg voor het synchroniseren van gebruikers en groepen uit uw bestaande identiteitsbron met uw Azure AD-tenant.

* [Upgraden van DirSync of Azure AD Sync naar Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Aan de slag met Azure AD Connect met Express-instellingen](./connect/active-directory-aadconnect-get-started-express.md)
* [Configureer write-back van wachtwoord](active-directory-passwords-writeback.md#configuring-password-writeback) om wachtwoorden van Azure AD terug te schrijven naar uw on-premises directory.

### <a name="on-premises-policy-change"></a>On-premises beleid wijzigen

Als u gebruikers van een on-premises Active Directory-domein synchroniseert en wilt toestaan dat gebruikers hun wachtwoord direct opnieuw kunnen instellen, moet u de volgende wijziging voor uw on-premises wachtwoordbeleid toepassen:

**Computerconfiguratie** > **Beleid** > **Windows-instellingen** > **Beveiligingsinstellingen** > **Accountbeleid** > **Wachtwoordbeleid**

**Minimale gebruiksduur wachtwoord**: 0 dagen

Hiermee bepaalt u hoe lang (in dagen) een wachtwoord moet worden gebruikt voordat de gebruiker het kan wijzigen. Als u dit instelt op **0 dagen**, kunnen gebruikers SSPR gebruiken als hun wachtwoorden worden gewijzigd door hun ondersteuningsteams.

![Beleid][Policy]

## <a name="disabling-self-service-password-reset"></a>Selfservice voor wachtwoordherstel uitschakelen

Het uitschakelen van selfservice voor wachtwoordherstel is heel eenvoudig. Open de Azure AD-tenant en ga naar **Wachtwoordherstel > Eigenschappen**. Kies **Geen** onder **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld**

### <a name="learn-more"></a>Meer informatie
De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md).
* [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](active-directory-passwords-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u selfservice voor wachtwoordherstel kunt configureren voor uw gebruikers. Volg de onderstaande koppeling naar de portal om naar Azure Portal te gaan en deze stappen te voltooien.

> [!div class="nextstepaction"]
> [Selfservice voor wachtwoordherstel inschakelen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Azure Active Directory-verificatiemethoden die beschikbaar zijn en hoeveel er vereist zijn"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Groepsbeleid voor on-premises wachtwoord is ingesteld op 0 dagen"
