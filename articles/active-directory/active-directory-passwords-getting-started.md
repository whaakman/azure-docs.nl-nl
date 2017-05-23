---
title: 'Quick Start: Azure AD-self-service voor wachtwoordherstel | Microsoft Docs'
description: Wachtwoorden snel opnieuw instellen voor Azure AD via self-service
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: joflore
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: b4d2819f51506651ebeb5b7b7c2d016c0cfc154d
ms.contentlocale: nl-nl
ms.lasthandoff: 05/08/2017


---
# <a name="quick-start-azure-ad-self-service-password-reset"></a>Quick Start: wachtwoorden opnieuw instellen voor Azure AD via self-service

## <a name="rapidly-deploy-self-service-password-reset"></a>Wachtwoorden snel opnieuw instellen via self-service

De self-service voor wachtwoordherstel (SSPR) biedt een eenvoudige manier voor IT-beheerders om gebruikers in staat te stellen hun eigen wachtwoord of account opnieuw in te stellen of te ontgrendelen. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem gebruiken. U ontvangt ook meldingen om u te waarschuwen over misbruik.

In deze handleiding wordt ervan uitgegaan dat u een werkende proefversie hebt van de Azure AD-tenant of dat u er een licentie voor hebt. Als u hulp nodig hebt bij het instellen van Azure AD ziet u het artikel [Aan de slag met Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/).

1. Selecteer in uw bestaande Azure AD-tenant de optie **Wachtwoord opnieuw instellen**

2. In het scherm **Eigenschappen** kiest u onder Selfservice voor wachtwoord opnieuw instellen is ingeschakeld een van de volgende opties
    * Niemand: niemand mag de SSPR-functionaliteit gebruiken
    * Een groep: alleen leden van een specifieke Azure AD-groep die u kiest mogen de SSPR-functionaliteit gebruiken
    * Iedereen: alle gebruikers met een account in uw Azure AD-tenant mogen de SSPR-functionaliteit gebruiken

3. In het scherm **Verificatiemethoden** kiest u een van de volgende opties
    * Aantal methoden dat is vereist om het wachtwoord opnieuw in te stellen: minimaal een en maximaal twee
    * Methoden voor gebruikers: er is er ten minste één nodig, maar het kan geen kwaad om een extra keuze beschikbaar te maken
        * **E-mailadres**: er wordt een e-mail met een code verstuurd naar het e-mailadres voor verificatie dat door de gebruiker is geconfigureerd
        * **Mobiele telefoon** geeft de gebruiker de keuze om een oproep of sms-bericht te ontvangen met een code, via het geconfigureerde mobiele telefoonnummer
        * **Zakelijke telefoon**: hiermee wordt de gebruiker voor een code gebeld via het geconfigureerde zakelijke telefoonnummer
        * **Beveiligingsvragen**: hiervoor moet u een optie kiezen
            * Het aantal vragen dat is vereist voor de registratie: het minimale aantal voor een succesvolle registratie. Dit betekent dat een gebruiker meer vragen kan beantwoorden en dat er een groep vragen wordt gemaakt waaruit kan worden gekozen. Deze optie kan worden ingesteld op 3, 4 of 5. Het aantal moet gelijk zijn aan of groter zijn dan het aantal vragen dat moet opnieuw moet worden ingesteld.
            * Het aantal vragen dat is vereist om het wachtwoord opnieuw in te stellen: kan worden ingesteld op 3, 4 of 5 vragen. Deze vragen moeten correct worden beantwoord; daarna mag een gebruiker zijn of haar wachtwoord pas opnieuw instellen of ontgrendelen.
                * U kunt aangepaste vragen toevoegen door op Aangepast te klikken bij het selecteren van beveiligingsvragen

4. AANBEVOLEN: in **Aanpassing** kunt u de koppeling Contact opnemen met de beheerder laten leiden naar een zelf gekozen pagina of e-mailadres

5. OPTIONEEL: in het scherm **Registratie** hebben beheerders verschillende opties:
    * Vereisen dat gebruiker zich bij aanmelding registreren
    * Het aantal dagen waarna gebruikers wordt gevraagd om de verificatiegegevens opnieuw te bevestigen

6. OPTIONEEL: in het scherm **Melding** hebben beheerders verschillende opties:
    * Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord
    * Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen

**U hebt op dit moment SSPR geconfigureerd voor uw Azure AD-tenant**. U kunt hier stoppen of doorgaan om het synchroniseren van wachtwoorden met een on-premises AD-domein te configureren.

> [!NOTE]
> Test self-service voor wachtwoordherstel met een gebruiker en niet als beheerder, aangezien Microsoft sterke verificatievereisten afdwingt voor Azure-accounts van beheerders. Zie ons [artikel over wachtwoordbeleid](active-directory-passwords-policy.md#administrator-password-policy-differences) voor meer informatie over het wachtwoordbeleid voor beheerders.

## <a name="configure-synchronization-to-existing-identity-source"></a>Synchronisatie met bestaande identiteitsbronnen configureren

Als u on-premises identiteitssynchronisatie met Azure AD wilt inschakelen, moet u [Azure AD Connect](./connect/active-directory-aadconnect.md) installeren en configureren op een server in uw organisatie. Deze toepassing draagt zorg voor het synchroniseren van gebruikers en groepen uit uw bestaande identiteitsbron met uw Azure AD-domein.

[Aan de slag met Azure AD Connect met Express-instellingen](./connect/active-directory-aadconnect-get-started-express.md)

[Upgraden van DirSync of Azure AD Sync naar Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)

## <a name="disabling-self-service-password-reset"></a>Self-service voor wachtwoordherstel uitschakelen

Het uitschakelen van self-service voor wachtwoordherstel is heel eenvoudig. Open uw Azure AD-tenant en ga naar **Wachtwoordherstel**, **Eigenschappen**. Kies **Niemand** onder **Self-service voor wachtwoord opnieuw instellen is ingeschakeld**

## <a name="next-steps"></a>Volgende stappen
De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD

* [**Licentieverlening**](active-directory-passwords-licensing.md): uw Azure AD-licentieverlening configureren
* [**Gegevens**](active-directory-passwords-data.md): informatie over de gegevens die nodig zijn en hoe deze worden gebruikt voor wachtwoordbeheer
* [**Implementatie**](active-directory-passwords-best-practices.md): SSPR plannen en implementeren voor uw gebruikers op basis van de hier gegeven informatie
* [**Aanpassen**](active-directory-passwords-customize.md): het uiterlijk van de ervaring van self-service voor wachtwoordherstel aanpassen voor uw bedrijf.
* [**Beleid**](active-directory-passwords-policy.md): Azure AD-wachtwoordbeleid begrijpen en instellen
* [**Rapportage**](active-directory-passwords-reporting.md): detecteren of, waar en wanneer uw gebruikers de functionaliteit voor self-service voor wachtwoordherstel gebruiken
* [**Gedetailleerde technische informatie**](active-directory-passwords-how-it-works.md): neem een kijkje achter de schermen om te begrijpen hoe het werkt
* [**Veelgestelde vragen**](active-directory-passwords-faq.md): hoe? Hoe komt dat? Wat? Waar? Wie? Wanneer? - Antwoorden op vragen die u altijd wilde stellen
* [**Probleemoplossing**](active-directory-passwords-troubleshoot.md): informatie over het oplossen van algemene problemen die optreden bij de self-service voor wachtwoordherstel

