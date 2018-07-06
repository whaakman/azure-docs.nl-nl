---
title: Self-service voor wachtwoord opnieuw instellen hoe het werkt - Azure Active Directory
description: Azure AD-selfservice wachtwoord opnieuw instellen voor gedetailleerde informatie over
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: f2d0b009c4451a4108222ac7aa7954ba6dd86699
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869004"
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Self-service voor wachtwoord opnieuw instellen in Azure AD-details

Hoe werkt selfservice wachtwoordherstel (SSPR)? Wat betekent dat optie in de interface? Lees voor meer informatie over Azure Active Directory (Azure AD) SSPR verder.

## <a name="how-does-the-password-reset-portal-work"></a>Hoe werkt de wachtwoordherstel portal?

Wanneer een gebruiker gaat u naar de portal voor wachtwoordherstel, een werkstroom wordt gestart om te bepalen:

   * Hoe moet de pagina worden gelokaliseerd?
   * Het gebruikersaccount dat geldig is?
   * Welke organisatie behoort de gebruiker tot?
   * Waar wordt het wachtwoord van de gebruiker beheerd?
   * Is de gebruiker een licentie voor het gebruik van de functie?

Lezen via de volgende stappen uit voor meer informatie over de logica achter het wachtwoord opnieuw instellen van pagina:

1. De gebruiker selecteert de **geen toegang tot uw account** koppelen of rechtstreeks naar gaat [ https://aka.ms/sspr ](https://passwordreset.microsoftonline.com).
   * Op basis van de landinstellingen van de browser, wordt de ervaring in de juiste taal weergegeven. De ervaring wachtwoordherstel is gelokaliseerd in dezelfde talen die ondersteuning biedt voor Office 365.
2. De gebruiker voert een gebruikers-ID en een captcha is geslaagd.
3. Azure AD verifieert de gebruiker kan deze functie wilt gebruiken door de volgende controles uitvoeren:
   * Controleert of de gebruiker deze functie is ingeschakeld en is een Azure AD-licentie is toegewezen.
     * Als de gebruiker niet deze functie is ingeschakeld is of er een licentie is toegewezen, de gebruiker gevraagd contact op met de beheerder om hun wachtwoord opnieuw instellen.
   * Controleert of de gebruiker het recht heeft een uitdaging gegevens die zijn gedefinieerd voor hun rekening in overeenstemming met de van beheerdersbeleid.
     * Als het beleid slechts één uitdaging vereist, klikt u vervolgens het zorgt ervoor dat de gebruiker de juiste gegevens voor ten minste een van de uitdagingen die is ingeschakeld door het beheerdersbeleid voor de gedefinieerd heeft.
       * Als de gebruikersvraag niet is geconfigureerd, wordt klikt u vervolgens de gebruiker aanbevolen contact opnemen met de beheerder om hun wachtwoord opnieuw instellen.
     * Als het beleid zijn twee uitdagingen vereist, klikt u vervolgens het zorgt ervoor dat de gebruiker de juiste gegevens voor ten minste twee van de uitdagingen ingeschakeld door het beheerdersbeleid voor de gedefinieerd heeft.
       * Als de gebruikersvraag niet is geconfigureerd, wordt klikt u vervolgens de gebruiker aanbevolen contact opnemen met de beheerder om hun wachtwoord opnieuw instellen.
   * Controles om te controleren of het wachtwoord van de gebruiker wordt beheerd on-premises (federatieve, Pass through-verificatie of wachtwoord-hash gesynchroniseerd).
     * Als u Write-back is geïmplementeerd en het wachtwoord van gebruikers is on-premises worden beheerd, is de gebruiker toegestaan om door te gaan om te verifiëren en hun wachtwoord opnieuw instellen.
     * Als u Write-back is niet geïmplementeerd en het wachtwoord van gebruikers is on-premises worden beheerd, en vervolgens de gebruiker gevraagd contact op met de beheerder om hun wachtwoord opnieuw instellen.
4. Als wordt vastgesteld dat de gebruiker kan het wachtwoord is opnieuw instelt, is door het opnieuw instellen van de gebruiker geleid.

## <a name="authentication-methods"></a>Verificatiemethoden

Als self-service voor Wachtwoordherstel is ingeschakeld, moet u ten minste één van de volgende opties voor de verificatiemethoden. Soms hoort u deze opties aangeduid als "gates." Het is raadzaam dat u ten minste twee verificatiemethoden kiezen, zodat uw gebruikers meer flexibiliteit hebben.

* Email
* Mobiele telefoon
* Telefoon (werk)
* Beveiligingsvragen

![Verificatie][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-the-authentication-data"></a>Welke velden worden gebruikt in de map voor de verificatiegegevens?

* **Telefoon (werk)**: komt overeen met de telefoon.
    * Gebruikers kunnen geen stelt u dit veld zelf. Deze moet worden gedefinieerd door een beheerder.
* **Mobiele telefoon**: komt overeen met de telefoon voor authenticatie (niet zichtbaar voor iedereen) of de mobiele telefoon (openbaar zichtbaar).
    * De service eerst naar de telefoon voor authenticatie gezocht en vervolgens weer terug naar de mobiele telefoon als de telefoon voor authenticatie is niet aanwezig.
* **Alternatief e-mailadres**: komt overeen met de verificatie-e (niet zichtbaar voor iedereen) of het alternatieve e-mailbericht.
    * De service eerst gekeken naar het verificatie-e-mailbericht en klikt u vervolgens terug naar het alternatieve e-mailbericht is mislukt.

Standaard worden alleen de telefoon van cloud-kenmerken en de mobiele telefoon gesynchroniseerd naar uw clouddirectory vanuit uw on-premises directory voor gegevens van de verificatie.

Gebruikers kunnen hun wachtwoord alleen opnieuw instellen als ze beschikken over gegevens aanwezig zijn in de verificatiemethoden die de beheerder is ingeschakeld en is vereist.

Als gebruikers niet dat het mobiele telefoonnummer wilt zichtbaar is in de map, maar ze nog steeds wilt gebruiken voor wachtwoord opnieuw instellen, moeten beheerders niet vullen het in de map. Gebruikers moeten vervolgens vullen hun **telefoon voor authenticatie** kenmerk de [-registratieportal voor wachtwoordherstel](https://aka.ms/ssprsetup). Beheerders kunnen deze informatie in het profiel van de gebruiker zien, maar deze niet elders wordt gepubliceerd.

### <a name="the-number-of-authentication-methods-required"></a>Het aantal verificatiemethoden dat is vereist

Deze optie bepaalt u het minimale aantal van de beschikbare verificatiemethoden of de poorten die een gebruiker opnieuw instellen of ontgrendelen van het wachtwoord moet doorlopen. Het kan worden ingesteld op een of twee.

Gebruikers kunnen meer verificatiemethoden opgeven als de beheerder deze verificatiemethode schakelt kiezen.

Als een gebruiker beschikt niet over de minimale vereiste methoden die zijn geregistreerd, zien ze een foutpagina accountinschrijving om aan te vragen die hun wachtwoord opnieuw instellen door een beheerder.

#### <a name="change-authentication-methods"></a>Verificatiemethoden wijzigen

Als u begint met een beleid dat slechts één vereist verificatiemethode voor opnieuw instellen of ontgrendelen is geregistreerd en u kunt wijzigen dat aan de twee methoden, wat gebeurt er?

| Aantal methoden dat is geregistreerd | Aantal methoden dat is vereist | Resultaat |
| :---: | :---: | :---: |
| 1 of meer | 1 | **Kan** opnieuw instellen of ontgrendelen |
| 1 | 2 | **Kan geen** opnieuw instellen of ontgrendelen |
| 2 of meer | 2 | **Kan** opnieuw instellen of ontgrendelen |

Als u de typen verificatiemethoden die gebruikmaken van een gebruiker kunt wijzigen, stopt u mogelijk per ongeluk gebruikers geen SSPR gebruiken als ze niet beschikken over de minimale hoeveelheid gegevens die beschikbaar zijn.

Voorbeeld: 
1. Het oorspronkelijke beleid is geconfigureerd met twee verificatiemethoden zijn vereist. Alleen de zakelijke telefoonnummer en de beveiligingsvragen worden gebruikt. 
2. De beheerder het beleid voor het gebruik van de beveiligingsvragen niet meer wordt gewijzigd, maar Hiermee staat u het gebruik van een mobiele telefoon en een alternatief e-mailadres.
3. Gebruikers zonder een alternatief e-mailadres velden ingevuld en de mobiele telefoon kunnen niet hun wachtwoord opnieuw instellen.

### <a name="how-secure-are-my-security-questions"></a>Hoe veilig zijn mijn beveiligingsvragen?

Als u vragen over de beveiliging gebruikt, raden wij deze gebruikt in combinatie met een andere methode. Vragen over de beveiliging is minder veilig dan andere methoden, omdat sommige mensen u de antwoorden op vragen van een andere gebruiker kent mogelijk.

> [!NOTE] 
> Beveiligingsvragen zijn privé en veilig opgeslagen op een gebruikersobject in de map en kunnen alleen door gebruikers worden beantwoord tijdens de registratie. Er is geen manier voor een beheerder om te lezen of wijzigen van de gebruiker vragen of antwoorden.
>

### <a name="security-question-localization"></a>Beveiliging vraag lokalisatie

De vooraf gedefinieerde vragen die volgen in de volledige set met Office 365-talen zijn gelokaliseerd en zijn gebaseerd op de landinstelling van de browser:

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
* Wat is uw oudste Broer of zus geboren maand en jaar? (bijvoorbeeld November 1985.)
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

Aangepaste beveiligingsvragen zijn niet gelokaliseerd voor verschillende talen. Alle aangepaste vragen worden weergegeven in dezelfde taal als ze zijn ingevoerd in de interface van de gebruiker met beheerdersrechten, zelfs als de landinstellingen van de browser van de gebruiker is anders. Als u gelokaliseerde vragen, moet u de vooraf gedefinieerde vragen.

De maximale lengte van een aangepaste beveiligingsvraag is 200 tekens.

De portal voor wachtwoord opnieuw instellen en vragen weergeven in een andere gelokaliseerde taal toevoegen '? mkt =<Locale>' aan het einde van het wachtwoord opnieuw instellen URL met het volgende lokaliseren in Spaans voorbeeld [ https://passwordreset.microsoftonline.com/?mkt=es-us ](https://passwordreset.microsoftonline.com/?mkt=es-us).

### <a name="security-question-requirements"></a>Vraag beveiligingsvereisten

* Het maximum aantal tekens minimumlengte voor antwoord is drie tekens.
* De maximale beantwoorden tekenlimiet is 40 tekens.
* De dezelfde vraag meer dan één keer kunnen niet worden beantwoord door gebruikers.
* Gebruikers kunnen niet hetzelfde antwoord op meer dan één vraag opgeven.
* Een tekenset kan worden gebruikt om de vragen en antwoorden, met inbegrip van Unicode-tekens te definiëren.
* Het aantal vragen dat is gedefinieerd moet groter zijn dan of gelijk zijn aan het aantal vragen die zijn vereist om u te registreren.

## <a name="registration"></a>Registratie

### <a name="require-users-to-register-when-they-sign-in"></a>Vereisen dat gebruikers te registreren wanneer ze zich aanmelden

Om deze optie inschakelt, heeft een gebruiker die is ingeschakeld voor wachtwoord opnieuw instellen voor het voltooien van de registratie van wachtwoord opnieuw instellen als ze zich aanmelden bij toepassingen met behulp van Azure AD. Dit omvat het volgende:

* Office 365
* Azure Portal
* Toegangsvenster
* Federatieve toepassingen
* Aangepaste toepassingen met behulp van Azure AD

Wanneer registratie vereisen is uitgeschakeld, kunnen gebruikers hun contactgegevens nog steeds handmatig registreren. Ze kunnen beide Bezoek [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup) of Selecteer de **registreren voor wachtwoordherstel** koppeling onder de **profiel** tabblad in het toegangsvenster.

> [!NOTE]
> Gebruikers de registratieportal voor wachtwoordherstel kunnen sluiten door te selecteren **annuleren** of door het venster te sluiten. Maar ze wordt gevraagd om u te registreren telkens wanneer ze zich aanmelden totdat ze hun inschrijving.
>
> Dit verbroken niet van de gebruiker verbinding als ze al zijn aangemeld.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Stel het aantal dagen waarna gebruikers wordt gevraagd naar de verificatiegegevens

Deze optie bepaalt u de periode tussen instellen en de verificatiegegevens reconfirming en is alleen beschikbaar als u inschakelt de **vereisen dat gebruikers zich bij aanmelding registreren** optie.

Geldige waarden zijn 0-730 dagen met '0', wat betekent dat gebruikers nooit wordt gevraagd om u te verificatiegegevens.

## <a name="notifications"></a>Meldingen

### <a name="notify-users-on-password-resets"></a>Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord

Als deze optie is ingesteld op **Ja**, en vervolgens de gebruiker die hun wachtwoord opnieuw instelt, een e-mailbericht melding ontvangt dat hun wachtwoord is gewijzigd. Het e-mailbericht wordt verzonden via de SSPR-portal naar de primaire en alternatieve e-mailadressen die zich in het bestand in Azure AD. Niemand anders wordt een melding ontvangen van deze opnieuw instellen van gebeurtenis.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen

Als deze optie is ingesteld op **Ja**, klikt u vervolgens *alle beheerders* ontvangt een e-mail naar het primaire e-mailadres in het bestand in Azure AD. Het e-mailbericht dat de melding verschijnt dat een andere beheerder het wachtwoord is gewijzigd met behulp van self-service voor Wachtwoordherstel.

Voorbeeld: Zijn er vier beheerders in een omgeving. Hun wachtwoord opnieuw instellen via Self-service voor Wachtwoordherstel van een beheerder. Beheerders B, C en D ontvangen een e-mailbericht dat deze van het wachtwoord opnieuw instellen.

## <a name="on-premises-integration"></a>On-premises integratie

Als u installeren, configureren en Azure AD Connect inschakelen, hebt u de volgende aanvullende opties voor on-premises integraties. Als deze opties lichter gekleurd weergegeven zijn, is klikt u vervolgens Write-back niet juist geconfigureerd. Zie voor meer informatie, [configuratie van wachtwoord terugschrijven](howto-sspr-writeback.md#configure-password-writeback).

![Write-back][Writeback]

Deze pagina biedt een snel de status van de on-premises Write-back-client een van de volgende berichten worden weergegeven op basis van de huidige configuratie:

* Uw On-premises Write-backclient is actief en werkend.
* Azure AD is online en is verbonden met uw on-premises Write-backclient. Echter, het lijkt erop dat de geïnstalleerde versie van Azure AD Connect verouderd is. Houd rekening met [een upgrade van Azure AD Connect](./../connect/active-directory-aadconnect-upgrade-previous-version.md) om ervoor te zorgen dat u de nieuwste connectiviteitsfuncties en belangrijke bugfixes beschikt hebt.
* Helaas kan niet worden gecontroleerd de status van uw on-premises Write-back-client omdat de geïnstalleerde versie van Azure AD Connect verouderd is. [Azure AD Connect upgraden](./../connect/active-directory-aadconnect-upgrade-previous-version.md) kunnen om te controleren of de status van de verbinding.
* Helaas, het lijkt erop dat we nu kan geen verbinding met uw on-premises Write-backclient. [Azure AD Connect oplossen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) om de verbinding te herstellen.
* Helaas geen verbinding maken met uw on-premises Write omdat wachtwoord terugschrijven niet juist is geconfigureerd. [Configureer wachtwoord terugschrijven](howto-sspr-writeback.md#configure-password-writeback) om de verbinding te herstellen.
* Helaas, het lijkt erop dat we nu kan geen verbinding met uw on-premises Write-backclient. Dit kan zijn veroorzaakt door tijdelijke problemen aan onze kant. Als het probleem zich blijft voordoen, [los problemen met Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) om de verbinding te herstellen.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Wachtwoorden terugschrijven naar uw on-premises directory

Dit besturingselement bepaalt of het terugschrijven van wachtwoorden is ingeschakeld voor deze map. Als u terugschrijven van wachtwoorden is ingeschakeld, betekent dit de status van de on-premises Write-back-service. Dit is handig als u het terugschrijven van wachtwoorden tijdelijk uitschakelen wilt zonder Azure AD Connect opnieuw configureren.

* Als de switch is ingesteld op **Ja**, klikt u vervolgens het terugschrijven van wachtwoorden is ingeschakeld en federatieve, Pass through-verificatie of de wachtwoord-hash gesynchroniseerd gebruikers kunnen hun wachtwoord opnieuw instellen.
* Als de switch is ingesteld op **Nee**, klikt u vervolgens Write-back is uitgeschakeld en federatieve, Pass through-verificatie of de wachtwoord-hash gesynchroniseerd gebruikers zijn niet in staat hun wachtwoord opnieuw instellen.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Toestaan dat gebruikers voor het ontgrendelen van accounts zonder hun wachtwoord opnieuw instellen

Dit besturingselement Hiermee wordt aangegeven of gebruikers die gaat u naar de portal voor wachtwoord opnieuw instellen van moeten de optie voor het ontgrendelen van hun on-premises Active Directory-accounts zonder hun wachtwoord opnieuw in te krijgen. Azure AD ontgrendelt standaard accounts bij het uitvoeren van een wachtwoord opnieuw instellen. U kunt deze instelling gebruiken voor het scheiden van deze twee bewerkingen. 

* Indien ingesteld op **Ja**, en vervolgens gebruikers de mogelijkheid hun wachtwoord opnieuw instellen en het account ontgrendelen of hun account ontgrendelen krijgen zonder dat het wachtwoord opnieuw instellen.
* Indien ingesteld op **Nee**, klikt u vervolgens gebruikers zijn alleen kunnen worden gecombineerd voor wachtwoordherstel en bewerking ontgrendelen van het account.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Hoe werkt wachtwoordherstel voor B2B-gebruikers?
Wachtwoord opnieuw instellen en wijzigen worden volledig ondersteund in alle configuraties met business-to-business (B2B). Wachtwoorden opnieuw instellen van B2B-gebruikers wordt ondersteund in de volgende drie gevallen:

   * **Gebruikers van de partnerorganisatie van een met een bestaande Azure AD-tenant**: als de organisatie die u zijn een partnerschap aangegaan met een bestaande Azure AD-tenant, we *respecteren ongeacht wachtwoord opnieuw instellen van beleidsregels zijn ingeschakeld op deze tenant*. Voor wachtwoord opnieuw instellen om te werken, moet de partnerorganisatie alleen om ervoor te zorgen dat Azure AD SSPR is ingeschakeld. Er zijn geen extra kosten voor Office 365-klanten en kan worden ingeschakeld met de volgende stappen in onze [aan de slag met wachtwoordbeheer](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) handleiding.
   * **Gebruikers die zich aanmelden via** self-service-aanmelding: als de organisatie u zijn een partnerschap aangegaan met gebruikt de [self-service-aanmelding](../users-groups-roles/directory-self-service-signup.md) functie om op te halen in een tenant, laat Microsoft ze opnieuw instellen van het wachtwoord met het e-mailbericht ze geregistreerd.
   * **B2B-gebruikers**: een nieuwe B2B-gebruikers die zijn gemaakt met behulp van de nieuwe [Azure AD B2B-mogelijkheden](../active-directory-b2b-what-is-azure-ad-b2b.md) worden ook opnieuw instellen van wachtwoorden met het e-mailbericht ze tijdens de uitnodiging hebt geregistreerd.

Als u wilt testen in dit scenario, gaat u naar http://passwordreset.microsoftonline.com met een van deze gebruikers van partnerorganisaties. Als ze hebben een alternatief e-mailadres of verificatieadres die zijn gedefinieerd, heeft wachtwoord opnieuw instellen werkt zoals verwacht.

> [!NOTE]
> Microsoft-accounts die toegang voor gasten zijn verleend aan uw Azure AD-tenant, zoals die van Hotmail.com, Outlook.com of andere persoonlijke e-mailadressen zijn niet Azure AD SSPR kunnen gebruiken. Moeten ze hun wachtwoord opnieuw instellen met behulp van de informatie in de [wanneer u zich niet aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artikel.

## <a name="next-steps"></a>Volgende stappen

De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD:

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../active-directory-passwords-reset-register.md)
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Authentication]: ./media/concept-sspr-howitworks/sspr-authentication-methods.png "Azure Active Directory-verificatiemethoden die beschikbaar zijn en hoeveel er vereist zijn"
[Writeback]: ./media/concept-sspr-howitworks/troubleshoot-writeback-running.png "On-premises configuratie voor integratie van wachtwoord terugschrijven van wachtwoorden en informatie voor probleemoplossing"
