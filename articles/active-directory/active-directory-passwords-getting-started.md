---
title: Snelstartgids Selfservice voor wachtwoordherstel - Azure Active Directory
description: Wachtwoorden snel opnieuw instellen voor Azure AD via self-service
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: bde8799f-0b42-446a-ad95-7ebb374c3bec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: 88123cead40968ebf7327b81f94233529f97aa2f
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-ad-self-service-password-reset-rapid-deployment"></a>Snelle implementatie van Azure AD-selfservice voor wachtwoordherstel

> [!IMPORTANT]
> **Bent u hier terechtgekomen omdat u problemen ondervindt met het aanmelden?** Zie in dat geval [Help, ik ben mijn wachtwoord voor Azure AD vergeten](active-directory-passwords-update-your-own-password.md).

De self-service voor wachtwoordherstel (SSPR) biedt een eenvoudige manier voor IT-beheerders om gebruikers in staat te stellen hun eigen wachtwoord of account opnieuw in te stellen of te ontgrendelen. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem openen. U ontvangt ook meldingen om u te waarschuwen over misbruik.

In deze handleiding wordt ervan uitgegaan dat u een werkende proefversie hebt van de Azure Active Directory-tenant (Azure AD) of dat u er een licentie voor hebt. Als u hulp nodig hebt bij het instellen van Azure AD ziet u [Aan de slag met Azure AD](get-started-azure-ad.md).

## <a name="enable-sspr-for-your-azure-ad-tenant"></a>SSPR inschakelen voor uw Azure Active Directory-tenant

1. Selecteer in uw bestaande Azure AD-tenant de optie **Wachtwoord opnieuw instellen**.

2. Op de pagina **Eigenschappen** kiest u onder **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld** een van de volgende opties:
   * **Geen**: niemand kan de SSPR-functionaliteit gebruiken.
   * **Geselecteerd**: alleen leden van een specifieke Azure Active Directory-groep die u kiest kunnen de SSPR-functionaliteit gebruiken. Het is raadzaam om een groep gebruikers te definiëren en deze instelling te gebruiken wanneer u deze functionaliteit implementeert voor het testen van een concept.
   * **Iedereen**: alle gebruikers met een account in uw Azure Active Directory-tenant kunnen de SSPR-functionaliteit gebruiken. U wordt aangeraden deze instelling te gebruiken wanneer u klaar bent voor het implementeren van deze functionaliteit in uw hele tenant nadat u het testen van een concept hebt voltooid.

   > [!IMPORTANT]
   > U kunt de wachtwoorden voor Azure-beheerdersaccounts altijd opnieuw instellen, hoe deze optie ook is ingesteld. 

3. Op de pagina **Verificatiemethoden** kiest u het volgende:
   * **Aantal methoden dat is vereist om het wachtwoord opnieuw in te stellen**: minimaal een en maximaal twee.
   * **Methoden voor gebruikers**: er is er ten minste één nodig, maar het kan geen kwaad om een extra keuze beschikbaar te maken.
      * **E-mailadres**: er wordt een e-mail met een code verstuurd naar het e-mailadres voor verificatie dat door de gebruiker is geconfigureerd.
      * **Mobiele telefoon** geeft de gebruiker de keuze om een oproep of sms-bericht te ontvangen met een code, via het geconfigureerde mobiele telefoonnummer.
      * **Zakelijke telefoon**: hiermee wordt de gebruiker voor een code gebeld via het geconfigureerde zakelijke telefoonnummer.
      * **Beveiligingsvragen**: hiervoor moet u een optie kiezen:
         * **Het aantal vragen dat is vereist voor de registratie**: het minimum aantal vragen voor een succesvolle registratie. Een gebruiker kan ervoor kiezen om meer vragen te beantwoorden en een groep vragen te maken waaruit vragen worden opgehaald. Deze optie kan worden ingesteld op drie, vier of vijf vragen. Het aantal moet gelijk zijn aan of groter zijn dan het aantal vragen dat vereist is om het wachtwoord opnieuw in te stellen. De gebruiker kan aangepaste vragen toevoegen als deze de knop **Aangepast** kiest tijdens het selecteren van de beveiligingsvragen.
         * **Het aantal vragen dat is vereist om het wachtwoord opnieuw in te stellen**: kan worden ingesteld van drie tot vijf vragen. Deze vragen moeten correct worden beantwoord; daarna mag een gebruiker zijn of haar wachtwoord pas opnieuw instellen of ontgrendelen.
            
    ![Verificatie][Authentication]

4. Aanbevolen: onder **Aanpassing** kunt u de koppeling **Contact opnemen met de beheerder** laten leiden naar een zelf gekozen pagina of e-mailadres. U wordt aangeraden om deze koppeling in te stellen op bijvoorbeeld een e-mailadres of een website die uw gebruikers al gebruiken voor ondersteuningsvragen.

5. Optioneel: op de pagina **Registratie** hebben beheerders verschillende opties:
   * Vereisen dat gebruikers zich bij aanmelding registreren.
   * Het aantal dagen instellen waarna gebruikers wordt gevraagd om de verificatiegegevens opnieuw te bevestigen.

6. Optioneel: op de pagina **Meldingen** hebben beheerders verschillende opties:
   * Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord.
   * Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen.

U hebt op dit moment SSPR geconfigureerd voor uw Azure AD-tenant. Uw gebruikers kunnen de instructies in de artikelen [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md) en [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md) nu gebruiken om hun wachtwoord bij te werken zonder tussenkomst van de beheerder. U kunt hier stoppen als u alleen in de cloud werkt. U kunt ook doorgaan met de volgende sectie om de synchronisatie van wachtwoorden met een on-premises Active Directory-domein te configureren.

> [!TIP]
> Test self-service voor wachtwoordherstel als een gebruiker en niet als beheerder, aangezien Microsoft sterke verificatievereisten afdwingt voor Azure-accounts van beheerders. Zie ons [artikel over wachtwoordbeleid](active-directory-passwords-policy.md#administrator-password-policy-differences) voor meer informatie over het wachtwoordbeleid voor beheerders.

## <a name="configure-synchronization-to-an-existing-identity-source"></a>Synchronisatie met een bestaande identiteitsbron configureren

Als u on-premises identiteitssynchronisatie met Azure AD wilt inschakelen, moet u [Azure AD Connect](./connect/active-directory-aadconnect.md) installeren en configureren op een server in uw organisatie. Met deze toepassing worden gebruikers en groepen uit uw bestaande identiteitsbron met uw Azure AD-tenant gesynchroniseerd. Zie voor meer informatie:

* [Upgraden van DirSync of Azure AD Sync naar Azure AD Connect](./connect/active-directory-aadconnect-dirsync-deprecated.md)
* [Aan de slag met Azure AD Connect met Express-instellingen](./connect/active-directory-aadconnect-get-started-express.md)
* [Configureer write-back van wachtwoord](active-directory-passwords-writeback.md#configure-password-writeback) om wachtwoorden van Azure AD terug te schrijven naar uw on-premises directory

### <a name="on-premises-policy-change"></a>On-premises beleid wijzigen

Als u gebruikers van een on-premises Active Directory-domein synchroniseert en wilt toestaan dat gebruikers hun wachtwoord direct opnieuw kunnen instellen, moet u de volgende wijziging voor uw on-premises wachtwoordbeleid toepassen:

1. Blader naar **Computerconfiguratie** > **Beleid** > **Windows-instellingen** > **Beveiligingsinstellingen** > **Accountbeleid** > **Wachtwoordbeleid**.

2. Stel de **Minimale wachtwoordduur** in op **0 dagen**.

Hiermee bepaalt u hoe lang (in dagen) een wachtwoord moet worden gebruikt voordat de gebruiker het kan wijzigen. Als u de minimale wachtwoordduur instelt op **0 dagen**, kunnen gebruikers SSPR gebruiken als hun wachtwoorden worden gewijzigd door hun ondersteuningsteams.

![Beleid][Policy]

## <a name="disable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel uitschakelen

Selfservice voor wachtwoordherstel kan eenvoudig worden uitgeschakeld. Open uw Azure AD-tenant en ga naar **Wachtwoord opnieuw instellen** > **Eigenschappen** en selecteer vervolgens **Geen** onder **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld**.

### <a name="learn-more"></a>Meer informatie
De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD:

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md)
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

In deze snelstartgids hebt u geleerd hoe u selfservice voor wachtwoordherstel kunt configureren voor uw gebruikers. Ga verder naar Azure Portal om deze stappen uit te voeren:

> [!div class="nextstepaction"]
> [Selfservice voor wachtwoordherstel inschakelen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)

[Authentication]: ./media/active-directory-passwords-getting-started/sspr-authentication-methods.png "Azure Active Directory-verificatiemethoden die beschikbaar zijn en hoeveel er vereist zijn"
[Policy]: ./media/active-directory-passwords-getting-started/password-policy.png "Groepsbeleid voor on-premises wachtwoord is ingesteld op 0 dagen"

