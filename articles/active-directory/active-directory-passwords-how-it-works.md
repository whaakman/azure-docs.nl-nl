---
title: Hoe het werkt? Azure AD SSPR | Microsoft Docs
description: Azure AD selfservice voor wachtwoordherstel diepgaand
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 71310534ec62b62bcd408d75060859c79bc470cf
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Selfservice voor wachtwoordherstel in Azure AD-diepgaand

Hoe werkt SSPR? Wat betekent dat optie in de interface? Blijven lezen voor meer informatie over Azure AD zelf uw wachtwoord opnieuw instellen.

## <a name="how-does-the-password-reset-portal-work"></a>Hoe het wachtwoord opnieuw ingesteld portal werk

Wanneer een gebruiker navigeert naar de portal voor wachtwoordherstel, een werkstroom wordt gestart om te bepalen:

   * Hoe moet u de pagina gelokaliseerd?
   * Is het gebruikersaccount geldig?
   * Welke organisatie behoort de gebruiker tot?
   * Waar het wachtwoord van gebruikers wordt beheerd?
   * Is de gebruiker een licentie voor het gebruik van de functie?


Lees de volgende stappen voor meer informatie over de logica achter het wachtwoord opnieuw instellen van pagina.

1. Gebruiker klikt op de geen toegang tot uw accountkoppeling of gaan rechtstreeks naar [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
2. Op basis van de landinstellingen van de browser die de ervaring wordt weergegeven in de juiste taal. De ervaring van wachtwoord opnieuw instellen is vertaald in dezelfde talen, zoals Office 365 ondersteunt.
3. Gebruiker voert een gebruikers-ID en een captcha wordt doorgegeven.
4. Azure AD verifieert of de gebruiker is deze functie gebruiken de volgende controles als volgt:
   * Controleert of de gebruiker deze functie is ingeschakeld en een Azure AD-licentie toegewezen.
     * Als de gebruiker beschikt niet over deze functie is ingeschakeld of een licentie is toegewezen, wordt de gebruiker gevraagd contact op met de beheerder om hun wachtwoord opnieuw instellen.
   * Controleert of de gebruiker het recht heeft uitdaging gegevens op hun account in overeenstemming met de beheerdersbeleid gedefinieerd.
     * Als het beleid vereist dat slechts één uitdaging, wordt vervolgens gewaarborgd dat de gebruiker de juiste gegevens die zijn gedefinieerd voor ten minste één van de uitdagingen ingeschakeld door het beheerdersbeleid heeft.
       * Als de gebruiker niet is geconfigureerd, wordt de gebruiker aanbevolen contact opnemen met de beheerder om hun wachtwoord opnieuw instellen.
     * Als het beleid twee uitdagingen vereist, wordt vervolgens gewaarborgd dat de gebruiker de juiste gegevens die zijn gedefinieerd voor ten minste twee van de uitdagingen ingeschakeld door het beheerdersbeleid heeft.
       * Als de gebruiker niet is geconfigureerd, is er de gebruiker contact op met de beheerder om hun wachtwoord opnieuw instellen.
   * Controleert als het wachtwoord van de gebruiker on-premises (federatieve of wachtwoord-hash gesynchroniseerd) wordt beheerd.
     * Als u Write-back is geïmplementeerd en het wachtwoord van de gebruiker on-premises wordt beheerd, is de gebruiker toegestaan om door te gaan om te verifiëren en hun wachtwoord opnieuw instellen.
     * Als Write-back is niet geïmplementeerd en het wachtwoord van gebruikers wordt beheerd op on-premises, wordt de gebruiker gevraagd contact op met de beheerder om hun wachtwoord opnieuw instellen.
5. Als wordt vastgesteld dat de gebruiker kan opnieuw instellen van hun wachtwoord, wordt de gebruiker geleid door het proces opnieuw instellen.

## <a name="authentication-methods"></a>Verificatiemethoden

Als Self-Service wachtwoord opnieuw instellen (SSPR) is ingeschakeld, moet u ten minste één van de volgende opties voor verificatiemethoden. Soms hoort u deze opties poorten genoemd. We raden u ten minste twee verificatiemethoden kiest, zodat uw gebruikers meer flexibiliteit hebben.

* E-mail
* Mobiele telefoon
* Telefoon (werk)
* Beveiligingsvragen

![Verificatie][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-authentication-data"></a>Welke velden worden gebruikt in de map voor verificatiegegevens

* Telefoon (werk) komt overeen met de telefoon (werk)
    * Gebruikers hebben geen Stel dit veld zelf moet deze worden gedefinieerd door een beheerder
* Mobiele telefoon komt overeen met de telefoon voor authenticatie (niet openbaar zichtbaar) of mobiele telefoon (openbaar zichtbaar)
    * De service eerst naar de telefoon voor authenticatie gezocht, vervolgens schakelt terug naar mobiele telefoon als niet geïnstalleerd
* Alternatief e-mailadres komt overeen met de verificatie-e (niet openbaar zichtbaar) of alternatieve e-mailadres
    * De service eerst voor verificatie E-mail gezocht en vervolgens terug naar het alternatieve e-mailadres is mislukt

Standaard worden alleen de cloud kenmerken telefoon (werk) en mobiele telefoonnummers voor uw clouddirectory gesynchroniseerd vanuit uw on-premises directory voor verificatiegegevens.

Gebruikers kunnen hun wachtwoord alleen opnieuw instellen als ze gegevens zijn aanwezig in de verificatiemethoden die de beheerder is ingeschakeld en is vereist.

Als gebruikers hun mobiele telefoonnummer zichtbaar in de map niet wilt, maar toch wilt gebruiken voor wachtwoord opnieuw instellen, beheerders moeten het niet vullen in de map en de gebruiker moet vervolgens vullen hun **telefoon voor authenticatie** kenmerk de [registratieportal voor wachtwoordherstel](http://aka.ms/ssprsetup). Beheerders kunnen deze informatie in het profiel van de gebruiker zien, maar het niet ergens anders is gepubliceerd.

### <a name="number-of-authentication-methods-required"></a>Aantal verificatiemethoden vereist

Deze optie bepaalt u het minimale aantal van de beschikbare verificatiemethoden of poorten van een gebruiker opnieuw instellen of hun wachtwoord ontgrendelen moet doorlopen en kan worden ingesteld op 1 of 2.

Gebruikers kunnen kiezen om op te geven meer verificatiemethoden als ze zijn ingeschakeld door de beheerder.

Als een gebruiker niet de minimale vereiste methoden die zijn geregistreerd heeft, zien ze een foutpagina die ze hun wachtwoord opnieuw instellen van een beheerder vragen worden omgeleid.

### <a name="how-secure-are-my-security-questions"></a>Hoe veilig weet mijn beveiligingsvragen

Als u beveiligingsvragen gebruiken, raden wij deze gebruikt met een andere methode als ze minder veilig dan andere methoden worden kunnen omdat sommige mensen een andere gebruiker vragen worden beantwoord weet mogelijk.

> [!NOTE] 
> Beveiligingsvragen zijn privé en veilig opgeslagen op een gebruikersobject in Active directory en kunnen alleen worden beantwoord door gebruikers tijdens de registratie. Er is geen manier voor een beheerder om te lezen of wijzigen van de gebruiker vragen of antwoorden.
>

### <a name="security-question-localization"></a>Beveiliging vraag lokalisatie

Alle vooraf gedefinieerde vragen als u worden in de volledige set van Office 365-talen op basis van de browser van de gebruiker gelokaliseerd.

* In welke stad hebt u uw eerste partner ontmoet?
* In welke stad hebben uw ouders elkaar leren kennen?
* In welke stad woont uw jongste of oudste broer of zus?
* In welke stad is uw vader geboren?
* In welke plaats hebt u uw eerste baan gehad?
* In welke stad is uw moeder geboren?
* In welke plaats was u op oudejaarsavond in 2000?
* Wat is de achternaam van uw favoriete leraar op hoog * school?
* Voor welke universiteit bent u uitgeloot?
* Waar hebt u uw trouwreceptie gehouden?
* Wat is de tweede naam van uw vader?
* Wat is uw lievelingseten?
* Wat zijn de voor- en achternaam van uw oma van moederskant?
* Wat is de tweede naam van uw moeder?
* Wat is uw oudste Broer of zus geboren maand en jaar? (bijvoorbeeld November 1985)
* Wat is de tweede naam van uw oudste broer of zus?
* Wat zijn de voor- en achternaam van uw opa van vaderskant?
* Wat is de tweede naam van uw jongste broer of zus?
* Op welke school zat u in de brugklas?
* Wat zijn de voor- en achternaam van uw beste jeugdvriend?
* Wat zijn de voor- en achternaam van uw eerste partner?
* Wat is de achternaam van uw favoriete leraar op de lagere school?
* Wat zijn het merk en model van uw eerste auto of motor?
* Wat is de naam van uw eerste school?
* In welk ziekenhuis bent u geboren?
* In welke straat staat of stond het huis waar u bent opgegroeid?
* Wat is de naam van uw jeugdidool?
* Wat is de naam van uw favoriete knuffeldier?
* Wat is de naam van uw eerste huisdier?
* Wat was uw bijnaam als kind?
* Wat was uw favoriete sport op de middelbare school?
* Wat was uw eerste baan?
* Wat zijn of waren de laatste vier cijfers van het telefoonnummer van uw ouderlijk huis?
* Wat wilde u worden toen u klein was?
* Wie is de grootste beroemdheid die u ooit hebt ontmoet?

### <a name="custom-security-questions"></a>Aangepaste beveiligingsvragen

Aangepaste beveiligingsvragen zijn niet gelokaliseerd voor verschillende talen. Alle aangepaste vragen worden weergegeven in dezelfde taal als die ze worden ingevoerd in de interface van de gebruiker met beheerdersrechten, zelfs als de browser van de gebruiker verschilt. Als u gelokaliseerde vragen nodig hebt, gebruikt u de vooraf gedefinieerde vragen.

De maximale lengte van een aangepaste beveiligingsvraag is 200 tekens.

### <a name="security-question-requirements"></a>Vraag de beveiligingsvereisten

* Minimale antwoord maximum aantal tekens is 3 tekens
* Maximale antwoord maximum aantal tekens is 40 tekens
* Gebruikers kunnen niet dezelfde vraag meer dan één keer worden beantwoord
* Gebruikers kunnen niet dezelfde meer dan één vraag te beantwoorden bepalen
* Een tekenset kan worden gebruikt voor het definiëren van vragen en antwoorden, met inbegrip van Unicode-tekens
* Het aantal vragen gedefinieerd moet groter zijn dan of gelijk zijn aan het aantal vragen dat vereist is om te registreren

## <a name="registration"></a>Registratie

### <a name="require-users-to-register-when-signing-in"></a>Vereisen dat gebruiker zich bij aanmelding registreren

Deze optie inschakelt, moet registratie van een gebruiker die is ingeschakeld voor wachtwoord opnieuw instellen voor het voltooien van het wachtwoord opnieuw instellen als ze zich aanmelden voor toepassingen die gebruikmaken van Azure AD aan te melden zoals die volgen:

* Office 365
* Azure Portal
* Toegangsvenster
* Federatieve toepassingen
* Aangepaste toepassingen die gebruikmaken van Azure AD

Wanneer dit is uitgeschakeld. gebruikers kunnen nog steeds handmatig zich registreren hun contactgegevens in via [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) of door te klikken op de **registreren voor wachtwoordherstel** koppeling onder het profieltabblad in de deelvenster toegang.

> [!NOTE]
> Gebruikers de registratieportal voor wachtwoordherstel kunnen negeren door op Annuleren te klikken of u het venster sluit, maar telkens wanneer die ze zich aanmelden totdat ze registratie hebt voltooid, wordt gevraagd.
>

### <a name="number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Het aantal dagen waarna gebruikers wordt gevraagd om de verificatiegegevens opnieuw te bevestigen

Deze optie bepaalt u de periode tussen instellen en verificatiegegevens reconfirming en is alleen beschikbaar als u inschakelt de **vereisen dat gebruikers zich registreren wanneer** optie.

Geldige waarden zijn 0 730 dagen met 0, wat betekent dat nooit gebruikers vragen om hun verificatie-informatie hebt bevestigd

## <a name="notifications"></a>Meldingen

### <a name="notify-users-on-password-resets"></a>Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord

Als deze optie is ingesteld op Ja, ontvangt een melding dat hun wachtwoord is gewijzigd via de SSPR-portal naar de primaire en alternatieve e-mailadressen in het bestand in Azure AD e-mailbericht met de gebruiker die hun wachtwoord opnieuw instelt. Niemand anders wordt de hoogte gesteld van deze opnieuw instellen van gebeurtenis.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen

Als deze optie is ingesteld op Ja, klikt u vervolgens **alle beheerders** ontvangt een e-mail naar het primaire e-mailadres in het bestand in Azure AD melding dat hun wachtwoord met behulp van SSPR is gewijzigd door een andere beheerder.

Voorbeeld: Zijn er vier beheerders in een omgeving. Beheerder "A" instelt hun wachtwoord met behulp van SSPR. Beheerders B en C D ontvangen een e-mailbericht waarschuwen dat ze dit gebeurt.

## <a name="on-premises-integration"></a>On-premises integratie

Als u hebt geïnstalleerd, geconfigureerd en Azure AD Connect is ingeschakeld, wordt u de volgende aanvullende opties voor lokale integraties hebben.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Wachtwoorden terugschrijven naar uw on-premises directory

Hiermee bepaalt u of wachtwoord terugschrijven is ingeschakeld voor deze map en als write-back ingeschakeld is, geeft de status van de on-premises Write-back-service. Dit is handig als u wilt het terugschrijven van wachtwoorden tijdelijk uitschakelen zonder Azure AD Connect opnieuw te configureren.

* Als de switch is ingesteld op Ja, vervolgens Write-back is ingeschakeld en federatieve en wachtwoord-hash gesynchroniseerd gebruikers kunnen hun wachtwoord opnieuw instellen.
* Als de switch is ingesteld zijn op Nee, vervolgens Write-back is uitgeschakeld en federatieve en wachtwoord-hash gesynchroniseerd gebruikers niet kunnen hun wachtwoord opnieuw instellen.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Toestaan dat gebruikers voor het ontgrendelen van accounts zonder hun wachtwoord opnieuw instellen

Geeft aan of gebruikers die de portal voor wachtwoord opnieuw instellen van bezoeken moeten krijgt de mogelijkheid voor het ontgrendelen van de lokale Active Directory-accounts zonder hun wachtwoord opnieuw instellen of niet. Standaard Azure AD ontgrendelt accounts bij het uitvoeren van een wachtwoord opnieuw instellen, deze instelling kunt u voor het scheiden van deze twee bewerkingen. 

* Indien ingesteld op 'Ja', klikt u vervolgens gebruikers u de mogelijkheid hun wachtwoord opnieuw instellen en het account ontgrendelen of krijgt voor het ontgrendelen zonder het wachtwoord opnieuw instellen.
* Als ingesteld op 'Nee', wordt gebruikers zijn alleen mogelijk om uit te voeren van een gecombineerde wachtwoord opnieuw instellen en het ontgrendelen van het account opnieuw.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Hoe wachtwoord opnieuw instellen voor B2B-gebruikers?
Wachtwoord opnieuw instellen en wijzigen worden volledig ondersteund in alle B2B-configuraties. De volgende drie gevallen worden ondersteund voor wachtwoordherstel voor B2B-gebruiker.

1. **Gebruikers van een partner org met een bestaande Azure AD-tenant** - als de organisatie die u samenwerking met een bestaande Azure AD-tenant we **respecteren ongeacht wachtwoord opnieuw instellen van beleidsregels zijn ingeschakeld in deze tenant**. Voor wachtwoord opnieuw instellen om te werken, de partner organisatie alleen behoeften om te controleren of Azure AD SSPR is ingeschakeld, die zonder extra kosten voor O365-klanten, en kan worden ingeschakeld door de stappen in onze [aan de slag met wachtwoordbeheer](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords)handleiding.
2. **Gebruikers die zich heeft aangemeld met behulp van [self-service aanmelding](active-directory-self-service-signup.md)**  - als de organisatie die u zijn samenwerking met gebruikte de [self-service aanmelding](active-directory-self-service-signup.md) om toegang te krijgen tot een tenant, laten we opnieuw instellen met de e-mailbericht die ze geregistreerd.
3. **B2B-gebruikers** -nieuwe B2B gebruikers gemaakt met behulp van de nieuwe [Azure AD B2B-mogelijkheden](active-directory-b2b-what-is-azure-ad-b2b.md) is ook mogelijk om in te stellen hun wachtwoorden met het e-mailbericht ze tijdens de uitnodiging hebt geregistreerd.

Als u wilt testen van dit scenario, gaat u naar http://passwordreset.microsoftonline.com met een van deze partner-gebruikers. Zolang ze beschikken over een alternatieve e-mailadres of verificatie e-mailadres is gedefinieerd, heeft wachtwoord opnieuw instellen werkt zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD

* [Hoe ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Opnieuw instellen of wijzigen van uw wachtwoord](active-directory-passwords-update-your-own-password.md).
* [Registreren voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag licentieverlening?](active-directory-passwords-licensing.md)
* [Welke gegevens wordt gebruikt door de SSPR en welke gegevens moet u voor uw gebruikers vullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties met SSPR?](active-directory-passwords-policy.md)
* [Wat is Write-back van wachtwoord en waarom ik van belang?](active-directory-passwords-writeback.md)
* [Hoe meld ik op activiteit in SSPR?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in de SSPR en wat ze aan de hand?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets is verbroken. Hoe kan ik SSPR oplossen?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag waarvoor is geen ergens anders](active-directory-passwords-faq.md)

[Authentication]: ./media/active-directory-passwords-how-it-works/sspr-authentication-methods.png "Azure AD-verificatiemethoden die beschikbaar zijn en hoeveel er nodig is"