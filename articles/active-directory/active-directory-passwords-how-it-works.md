---
title: Selfservice voor wachtwoordherstel hoe het werkt - Azure Active Directory
description: Azure AD selfservice voor wachtwoordherstel diepgaand
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: 8799931f6233f0f18b56bea39e3cbcbbf51274e9
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Selfservice voor wachtwoordherstel in Azure AD-diepgaand

Hoe selfservice voor wachtwoordherstel (SSPR) werk? Wat betekent dat optie in de interface? Lezen voor meer informatie over Azure Active Directory (Azure AD) SSPR blijven.

## <a name="how-does-the-password-reset-portal-work"></a>Hoe het wachtwoord opnieuw ingesteld portal werk?

Wanneer een gebruiker gaat dan naar de portal voor wachtwoordherstel, een werkstroom wordt gestart om te bepalen:

   * Hoe moet u de pagina gelokaliseerd?
   * Is het gebruikersaccount geldig?
   * Welke organisatie behoort de gebruiker tot?
   * Waar wordt het wachtwoord van de gebruiker beheerd?
   * Is de gebruiker een licentie voor het gebruik van de functie?

Lees de volgende stappen voor meer informatie over de logica achter het wachtwoord opnieuw instellen van pagina:

1. De gebruiker selecteert de **geen toegang tot uw account** koppelen of rechtstreeks naar gaat [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
   * Op basis van de landinstellingen van de browser, wordt de ervaring in de juiste taal weergegeven. De ervaring van wachtwoord opnieuw instellen is vertaald in dezelfde talen die ondersteuning biedt voor Office 365.
2. De gebruiker voert een gebruikers-ID en een captcha wordt doorgegeven.
3. Azure AD verifieert de gebruiker kan deze functie wilt gebruiken de volgende controles als volgt:
   * Controleert of de gebruiker deze functie is ingeschakeld en een Azure AD heeft licentie is toegewezen.
     * Als de gebruiker niet deze functie is ingeschakeld is of beschikken over een licentie is toegewezen, wordt de gebruiker gevraagd contact op met de beheerder om hun wachtwoord opnieuw instellen.
   * Controleert of de gebruiker het recht heeft uitdaging gegevens op hun account in overeenstemming met de beheerdersbeleid gedefinieerd.
     * Als het beleid slechts één uitdaging vereist, klikt u vervolgens het zorgt ervoor dat de gebruiker de juiste gegevens die zijn gedefinieerd voor ten minste één van de uitdagingen ingeschakeld door het beheerdersbeleid heeft.
       * Als de controle van de gebruiker niet is geconfigureerd, wordt de gebruiker aanbevolen contact opnemen met de beheerder om hun wachtwoord opnieuw instellen.
     * Als het beleid twee uitdagingen vereist, klikt u vervolgens het zorgt ervoor dat de gebruiker de juiste gegevens die zijn gedefinieerd voor ten minste twee van de uitdagingen ingeschakeld door het beheerdersbeleid heeft.
       * Als de controle van de gebruiker niet is geconfigureerd, wordt de gebruiker aanbevolen contact opnemen met de beheerder om hun wachtwoord opnieuw instellen.
   * Controleert als het wachtwoord van de gebruiker wordt beheerd lokaal (federatieve of wachtwoord-hash gesynchroniseerd).
     * Als u Write-back is geïmplementeerd en het wachtwoord van de gebruiker is on-premises worden beheerd, is de gebruiker toegestaan om door te gaan om te verifiëren en hun wachtwoord opnieuw instellen.
     * Als u Write-back is niet geïmplementeerd en het wachtwoord van de gebruiker is on-premises worden beheerd, wordt de gebruiker gevraagd om contact op met de beheerder om hun wachtwoord opnieuw instellen.
4. Als wordt vastgesteld dat de gebruiker kan opnieuw instellen van hun wachtwoord, wordt de gebruiker geleid door het proces opnieuw instellen.

## <a name="authentication-methods"></a>Verificatiemethoden

Als SSPR is ingeschakeld, moet u ten minste één van de volgende opties voor de verificatiemethoden. Soms hoort u deze opties aangeduid als 'poorten'. We raden u ten minste twee verificatiemethoden kiezen zodat uw gebruikers meer flexibiliteit hebben.

* E-mail
* Mobiele telefoon
* Telefoon (werk)
* Beveiligingsvragen

![Verificatie][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-the-authentication-data"></a>Welke velden worden gebruikt in de map voor de verificatiegegevens?

* **Telefoon (werk)**: komt overeen met de telefoon (werk).
    * Gebruikers hebben geen in te stellen van dit veld zelf. Het moet worden gedefinieerd door een beheerder.
* **Mobiele telefoon**: komt overeen met de telefoon voor authenticatie (niet openbaar zichtbaar) of de mobiele telefoon (openbaar zichtbaar).
    * De service eerst naar de telefoon voor authenticatie gezocht en vervolgens valt terug naar de mobiele telefoon als de telefoon voor authenticatie is niet aanwezig.
* **Alternatieve e-mailadres**: komt overeen met de verificatie-e (niet openbaar zichtbaar) of het alternatieve e-mailbericht.
    * De service eerst naar het e-mailadres voor de verificatie gezocht en vervolgens terug naar het alternatieve e-mailbericht is mislukt.

Standaard worden alleen de cloud kenmerken telefoon (werk) en de mobiele telefoon voor uw clouddirectory gesynchroniseerd vanuit uw on-premises directory voor verificatiegegevens.

Gebruikers kunnen hun wachtwoord alleen opnieuw instellen als ze gegevens zijn aanwezig in de verificatiemethoden die de beheerder is ingeschakeld en is vereist.

Als gebruikers hun mobiele telefoonnummer zichtbaar in de map niet wilt, maar ze nog steeds wilt gebruiken voor het wachtwoord opnieuw instellen, mag beheerders niet deze vullen in de map. Gebruikers moeten vervolgens vullen hun **telefoon voor authenticatie** kenmerk de [registratieportal voor wachtwoordherstel](http://aka.ms/ssprsetup). Beheerders kunnen deze informatie in het profiel van de gebruiker zien, maar het niet ergens anders gepubliceerd.

### <a name="the-number-of-authentication-methods-required"></a>Het aantal verificatiemethoden vereist

Deze optie bepaalt het minimumaantal van de beschikbare verificatiemethoden of de poorten die een gebruiker opnieuw instellen of hun wachtwoord ontgrendelen moet doorlopen. Worden kan ingesteld op een of twee.

Gebruikers kunnen kiezen om op te geven meer verificatiemethoden worden ingeschakeld als de beheerder deze verificatiemethode schakelt.

Als een gebruiker niet de minimale vereiste methoden die zijn geregistreerd heeft, zien ze een foutpagina die ze aanvragen dat hun wachtwoord opnieuw instellen door een beheerder worden omgeleid.

#### <a name="change-authentication-methods"></a>Verificatiemethoden wijzigen

Als u begint met een beleid dat slechts één vereiste verificatiemethode voor opnieuw instellen of ontgrendelen heeft geregistreerd en u instellen dat naar de twee methoden, wat er gebeurt?

| Aantal methoden die zijn geregistreerd | Aantal methoden vereist | Resultaat |
| :---: | :---: | :---: |
| 1 of meer | 1 | **Kan** opnieuw instellen of ontgrendelen |
| 1 | 2 | **Kan geen** opnieuw instellen of ontgrendelen |
| 2 of hoger | 2 | **Kan** opnieuw instellen of ontgrendelen |

Als u de typen verificatiemethoden die gebruikmaken van een gebruiker kunt wijzigen, kunt u per ongeluk voorkomen dat gebruikers om te kunnen SSPR gebruiken als ze niet beschikken over de minimale hoeveelheid gegevens die beschikbaar zijn.

Voorbeeld: 
1. Het oorspronkelijke beleid is geconfigureerd met twee verificatiemethoden zijn vereist. Alleen het telefoonnummer van office en de beveiligingsvragen worden gebruikt. 
2. De beheerder het beleid voor het gebruik van de beveiligingsvragen niet langer wijzigingen, maar staat het gebruik van een mobiele telefoon en een alternatief e-mailbericht.
3. Gebruikers zonder het alternatieve e-velden ingevuld en mobiele telefoon kunnen hun wachtwoorden niet opnieuw instellen.

### <a name="how-secure-are-my-security-questions"></a>Hoe veilig weet mijn beveiligingsvragen?

Als u beveiligingsvragen gebruikt, wordt u aangeraden gebruik ervan in combinatie met een andere methode. Beveiligingsvragen is minder veilig dan andere methoden, omdat sommige gebruikers mogelijk de antwoorden op vragen van een andere gebruiker op de hoogte.

> [!NOTE] 
> Beveiligingsvragen zijn privé en veilig opgeslagen op een gebruikersobject in Active directory en kunnen alleen worden beantwoord door gebruikers tijdens de registratie. Er is geen manier voor een beheerder om te lezen of wijzigen van de gebruiker vragen of antwoorden.
>

### <a name="security-question-localization"></a>Beveiliging vraag lokalisatie

De vooraf gedefinieerde vragen die volgen in de volledige set van Office 365 talen zijn gelokaliseerd en zijn gebaseerd op de browser van de gebruiker:

* In welke stad hebt u uw eerste partner ontmoet?
* In welke stad hebben uw ouders elkaar leren kennen?
* In welke stad woont uw jongste of oudste broer of zus?
* In welke stad is uw vader geboren?
* In welke plaats hebt u uw eerste baan gehad?
* In welke stad is uw moeder geboren?
* In welke plaats was u op oudejaarsavond in 2000?
* Wat is de achternaam van uw favoriete leraar op de middelbare school?
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

Aangepaste beveiligingsvragen zijn niet gelokaliseerd voor verschillende talen. Alle aangepaste vragen worden weergegeven in dezelfde taal als ze worden ingevoerd in de interface van de gebruiker met beheerdersrechten, zelfs als de browser van de gebruiker verschilt. Als u gelokaliseerde vragen, moet u de vooraf gedefinieerde vragen.

De maximale lengte van een aangepaste beveiligingsvraag is 200 tekens.

### <a name="security-question-requirements"></a>Vraag de beveiligingsvereisten

* De tekenlimiet antwoord minimaal is drie tekens.
* De maximale antwoord tekenlimiet is 40 tekens.
* Gebruikers kunnen niet dezelfde vraag meer dan één keer worden beantwoord.
* Gebruikers kunnen geen dezelfde meer dan één vraag te beantwoorden.
* Een tekenset kan worden gebruikt voor het definiëren van de vragen en antwoorden, met inbegrip van Unicode-tekens.
* Het aantal vragen gedefinieerd moet groter zijn dan of gelijk zijn aan het aantal vragen die zijn vereist om te registreren.

## <a name="registration"></a>Registratie

### <a name="require-users-to-register-when-they-sign-in"></a>Vereisen dat gebruikers zich registreren wanneer ze zich aanmelden

Om deze optie inschakelt, heeft een gebruiker die is ingeschakeld voor wachtwoordherstel voor het voltooien van de registratie van wachtwoord opnieuw instellen als deze zich voor toepassingen aanmelden met Azure AD. Dit omvat het volgende:

* Office 365
* Azure Portal
* Toegangsvenster
* Federatieve toepassingen
* Aangepaste toepassingen met Azure AD

Wanneer registratie vereisen is uitgeschakeld, kunnen gebruikers hun contactgegevens nog steeds handmatig registreren. Ze kunnen beide Bezoek [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) of Selecteer de **registreren voor wachtwoordherstel** koppeling onder de **profiel** tabblad in het deelvenster toegang.

> [!NOTE]
> Gebruikers kunnen de registratieportal voor wachtwoordherstel sluiten door te selecteren **annuleren** of door het venster te sluiten. Maar ze worden geregistreerd telkens wanneer ze zich aanmelden totdat ze hun registratie hebt voltooid, wordt gevraagd.
>
> Dit verbroken niet van de gebruiker verbinding als ze al bent aangemeld.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Het aantal dagen waarna gebruikers wordt gevraagd om te bevestigen hun verificatiegegevens instellen

Deze optie bepaalt u de periode tussen instellen en verificatiegegevens reconfirming en is alleen beschikbaar als u inschakelt de **vereisen dat gebruikers zich registreren wanneer** optie.

Geldige waarden zijn 0-730 dagen met '0', wat betekent dat gebruikers nooit worden gevraagd om opnieuw te bevestigen hun verificatiegegevens.

## <a name="notifications"></a>Meldingen

### <a name="notify-users-on-password-resets"></a>Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord

Als deze optie is ingesteld op **Ja**, en vervolgens de gebruiker die hun wachtwoord opnieuw instelt, een e-mailbericht melding ontvangt dat hun wachtwoord is gewijzigd. Het e-mailbericht wordt verzonden via de SSPR-portal naar hun primaire en alternatieve e-mailadressen die zich in het bestand in Azure AD. Niemand anders wordt de hoogte gesteld van deze opnieuw instellen van gebeurtenis.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen

Als deze optie is ingesteld op **Ja**, klikt u vervolgens *alle beheerders* ontvangt een e-mail naar het primaire e-mailadres in het bestand in Azure AD. Het e-mailbericht waarschuwt hen dat een andere beheerder het wachtwoord is gewijzigd met behulp van SSPR.

Voorbeeld: Zijn er vier beheerders in een omgeving. Beheerder A hun wachtwoord opnieuw instelt met behulp van SSPR. Beheerders B en C D ontvangen een e-mailbericht dat ze het wachtwoord opnieuw instellen van waarschuwingen.

## <a name="on-premises-integration"></a>On-premises integratie

Als u installeren, configureren en Azure AD Connect inschakelen, hebt u de volgende aanvullende opties voor lokale integraties. Als u deze opties zijn niet beschikbaar, is vervolgens Write-back niet juist geconfigureerd. Zie voor meer informatie [wachtwoord terugschrijven configureren](active-directory-passwords-writeback.md#configure-password-writeback).

![Writeback][Writeback]

Deze pagina bevat een snel de status van de lokale Write-back-client een van de volgende berichten worden weergegeven op basis van de huidige configuratie:

* Uw On-premises Write-back-client is actief.
* Azure AD is online en verbonden is met uw lokale Write-back-client. Het lijkt echter dat de geïnstalleerde versie van Azure AD Connect verouderd is. Houd rekening met [upgraden van Azure AD Connect](./connect/active-directory-aadconnect-upgrade-previous-version.md) om ervoor te zorgen dat u de meest recente verbindingsfuncties en belangrijke oplossingen voor problemen hebt.
* We kan helaas niet de status van uw lokale Write-back van client controleren omdat de geïnstalleerde versie van Azure AD Connect verouderd is. [Azure AD Connect upgraden](./connect/active-directory-aadconnect-upgrade-previous-version.md) kunnen de verbindingsstatus van uw controleren.
* Helaas lijkt we nu kan geen verbinding met uw lokale Write-back-client. [Problemen met Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) om de verbinding te herstellen.
* Helaas kan geen we verbinding met uw lokale Write-back-client doordat Write-back van wachtwoord niet correct is geconfigureerd. [Wachtwoord terugschrijven configureren](active-directory-passwords-writeback.md#configure-password-writeback) om de verbinding te herstellen.
* Helaas lijkt we nu kan geen verbinding met uw lokale Write-back-client. Dit kan zijn vanwege tijdelijke problemen bij ons. Als het probleem zich blijft voordoen, [problemen met Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) om de verbinding te herstellen.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Wachtwoorden terugschrijven naar uw on-premises directory

Dit besturingselement bepaalt of het wachtwoord terugschrijven is ingeschakeld voor deze map. Als u Write-back is ingeschakeld, betekent dit de status van de on-premises Write-back-service. Dit is handig als u een tijdelijk wachtwoord terugschrijven uitschakelen zonder opnieuw configureren van Azure AD Connect wilt maken.

* Als de switch is ingesteld op **Ja**, vervolgens Write-back is ingeschakeld, en federatieve en wachtwoord-hash gesynchroniseerd gebruikers kunnen hun wachtwoord opnieuw instellen.
* Als de switch is ingesteld op **Nee**, vervolgens Write-back is uitgeschakeld en federatieve en wachtwoord-hash gesynchroniseerd gebruikers niet kunnen hun wachtwoord opnieuw instellen.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Toestaan dat gebruikers voor het ontgrendelen van accounts zonder hun wachtwoord opnieuw instellen

Dit besturingselement Hiermee wordt aangegeven of gebruikers die de portal voor wachtwoord opnieuw instellen van bezoeken moeten de optie voor het ontgrendelen van de lokale Active Directory-accounts zonder dat hun wachtwoord opnieuw in te krijgen. Standaard Azure AD Hiermee ontgrendelt u accounts bij het uitvoeren van een wachtwoord opnieuw instellen. U kunt deze instelling gebruiken voor het scheiden van deze twee bewerkingen. 

* Indien ingesteld op **Ja**, en vervolgens gebruikers u de mogelijkheid hun wachtwoord opnieuw instellen en het account ontgrendelen, of hun account ontgrendelen krijgt zonder dat het wachtwoord opnieuw instellen.
* Indien ingesteld op **Nee**en vervolgens gebruikers zijn alleen mogelijk voor wachtwoordherstel gecombineerde bewerking ontgrendelen van het account.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Hoe wachtwoord opnieuw instellen voor B2B-gebruikers?
Wachtwoord opnieuw instellen en wijzigen worden volledig ondersteund voor alle configuraties voor business-to-business (B2B). B2B gebruiker-wachtwoord opnieuw instellen wordt ondersteund in de volgende drie gevallen:

   * **Gebruikers van een andere organisatie met een bestaande Azure AD-tenant**: als de organisatie dat u bent samenwerking met een bestaande Azure AD-tenant we *respecteren ongeacht wachtwoord opnieuw instellen van beleidsregels zijn ingeschakeld op deze tenant*. Voor wachtwoord opnieuw instellen om te werken, moet de partnerorganisatie NET om ervoor te zorgen dat de Azure AD SSPR is ingeschakeld. Er zijn geen extra kosten voor Office 365-klanten en deze kan worden ingeschakeld door de stappen in onze [aan de slag met wachtwoordbeheer](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) handleiding.
   * **Gebruikers die zich aanmeldt via** self-service aanmelding: als de organisatie die u bent samenwerking met gebruikte de [self-service aanmelding](active-directory-self-service-signup.md) om toegang te krijgen tot een tenant, we frameworkservice opnieuw instellen van het wachtwoord met het e-mailbericht ze geregistreerd.
   * **B2B-gebruikers**: nieuwe B2B gebruikers gemaakt met behulp van de nieuwe [Azure AD B2B-mogelijkheden](active-directory-b2b-what-is-azure-ad-b2b.md) is ook mogelijk om in te stellen hun wachtwoorden met het e-mailbericht ze tijdens de uitnodiging hebt geregistreerd.

Als u wilt testen van dit scenario, gaat u naar http://passwordreset.microsoftonline.com met een van deze partner-gebruikers. Als ze hebben een alternatieve e-mailadres of verificatie e-mailadres is gedefinieerd, heeft wachtwoord opnieuw instellen werkt zoals verwacht.

> [!NOTE]
> Microsoft-accounts die toegang voor gasten zijn verleend aan uw Azure AD-tenant, zoals die van Hotmail.com, Outlook.com of andere persoonlijke e-mailadressen kunnen geen Azure AD SSPR gebruiken. Ze moeten hun wachtwoord opnieuw instellen met behulp van de gegevens gevonden in de [wanneer u kan niet aanmelden bij je Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artikel.

## <a name="next-steps"></a>Volgende stappen

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

[Authentication]: ./media/active-directory-passwords-how-it-works/sspr-authentication-methods.png "Azure Active Directory-verificatiemethoden die beschikbaar zijn en hoeveel er vereist zijn"
[Writeback]: ./media/active-directory-passwords-how-it-works/troubleshoot-writeback-running.png "On-premises Integratieconfiguratie voor write-back van wachtwoord en informatie over probleemoplossing"
