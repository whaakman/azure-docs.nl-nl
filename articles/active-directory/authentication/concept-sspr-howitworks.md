---
title: Azure Active Directory Self-service voor wachtwoord deep dive
description: Hoe werkt selfservice wachtwoordherstel
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.openlocfilehash: 4b636b78c3992c6647def47cf3005ceb41e0d5ba
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495289"
---
# <a name="how-it-works-azure-ad-self-service-password-reset"></a>Het werkt als volgt: Azure AD-selfservice wachtwoord opnieuw instellen

Hoe werkt selfservice wachtwoordherstel (SSPR)? Wat betekent dat optie in de interface? Lees voor meer informatie over Azure Active Directory (Azure AD) SSPR verder.

|     |
| --- |
| Mobiele app-meldingen en mobiele app-code als methoden voor het Azure AD Self-service voor wachtwoord opnieuw instellen van de openbare preview-functies van Azure Active Directory zijn. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

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
   * Om weer te geven van het wachtwoord opnieuw instellen in een andere portal gelokaliseerde taal toevoegen '? mkt = "aan het einde van het wachtwoord opnieuw instellen URL met het volgende lokaliseren in Spaans voorbeeld [ https://passwordreset.microsoftonline.com/?mkt=es-us ](https://passwordreset.microsoftonline.com/?mkt=es-us).
2. De gebruiker voert een gebruikers-ID en een captcha is geslaagd.
3. Azure AD verifieert de gebruiker kan deze functie wilt gebruiken door de volgende controles uitvoeren:
   * Controleert of de gebruiker deze functie is ingeschakeld en is een Azure AD-licentie is toegewezen.
     * Als de gebruiker niet deze functie is ingeschakeld is of er een licentie is toegewezen, de gebruiker gevraagd contact op met de beheerder om hun wachtwoord opnieuw instellen.
   * Controleert of de gebruiker de juiste verificatiemethoden gedefinieerd voor hun rekening in overeenstemming met de van beheerdersbeleid.
     * Als het beleid is slechts één methode vereist, klikt u vervolgens het zorgt ervoor dat de gebruiker de juiste gegevens voor ten minste één van de verificatiemethoden ingeschakeld door het beheerdersbeleid voor de gedefinieerd heeft.
       * Als u de verificatiemethoden die niet zijn geconfigureerd, wordt de gebruiker aanbevolen contact opnemen met de beheerder om hun wachtwoord opnieuw instellen.
     * Als het beleid twee methoden vereist, klikt u vervolgens het zorgt ervoor dat de gebruiker de juiste gegevens voor ten minste twee van de verificatiemethoden ingeschakeld door het beheerdersbeleid voor de gedefinieerd heeft.
       * Als u de verificatiemethoden die niet zijn geconfigureerd, wordt de gebruiker aanbevolen contact opnemen met de beheerder om hun wachtwoord opnieuw instellen.
     * Als een Azure-beheerdersrol is toegewezen aan de gebruiker, wordt klikt u vervolgens het wachtwoord twee-gate-beleid afgedwongen. Meer informatie over dit beleid kan worden gevonden in de sectie [beheerder opnieuw instellen van beleid verschillen](concept-sspr-policy.md#administrator-reset-policy-differences).
   * Controles om te controleren of het wachtwoord van de gebruiker wordt beheerd on-premises (federatieve, Pass through-verificatie of wachtwoord-hash gesynchroniseerd).
     * Als u Write-back is geïmplementeerd en het wachtwoord van gebruikers is on-premises worden beheerd, is de gebruiker toegestaan om door te gaan om te verifiëren en hun wachtwoord opnieuw instellen.
     * Als u Write-back is niet geïmplementeerd en het wachtwoord van gebruikers is on-premises worden beheerd, en vervolgens de gebruiker gevraagd contact op met de beheerder om hun wachtwoord opnieuw instellen.
4. Als wordt vastgesteld dat de gebruiker kan het wachtwoord is opnieuw instelt, is door het opnieuw instellen van de gebruiker geleid.

## <a name="authentication-methods"></a>Verificatiemethoden

Als self-service voor Wachtwoordherstel is ingeschakeld, moet u ten minste één van de volgende opties voor de verificatiemethoden. Soms hoort u deze opties aangeduid als "gates." Is het raadzaam dat u **twee of meer verificatiemethoden kiezen** zodat uw gebruikers meer flexibiliteit hebben wanneer ze niet voor toegang tot een wanneer ze deze nodig hebben.

* Melding mobiele app (preview)
* Mobiele app-code (preview)
* Email
* Mobiele telefoon
* Telefoon (werk)
* Beveiligingsvragen

Gebruikers kunnen hun wachtwoord alleen opnieuw instellen als ze beschikken over gegevens aanwezig zijn in de verificatiemethoden die de beheerder heeft ingeschakeld.

> [!WARNING]
> Accounts die zijn toegewezen Azure-beheerdersrollen is vereist voor het gebruik van methoden, zoals gedefinieerd in de sectie [beheerder opnieuw instellen van beleid verschillen](concept-sspr-policy.md#administrator-reset-policy-differences).

![Verificatie][Authentication]

### <a name="number-of-authentication-methods-required"></a>Aantal verificatiemethoden dat is vereist

Deze optie bepaalt u het minimale aantal van de beschikbare verificatiemethoden of de poorten die een gebruiker opnieuw instellen of ontgrendelen van het wachtwoord moet doorlopen. Het kan worden ingesteld op een of twee.

Gebruikers kunnen meer verificatiemethoden opgeven als de beheerder deze verificatiemethode schakelt kiezen.

Als een gebruiker beschikt niet over de minimale vereiste methoden die zijn geregistreerd, zien ze een foutpagina accountinschrijving om aan te vragen die hun wachtwoord opnieuw instellen door een beheerder.

#### <a name="mobile-app-and-sspr-preview"></a>Mobiele app en SSPR (Preview)

Wanneer u een mobiele app, zoals de Microsoft Authenticator-app als een methode voor het wachtwoord opnieuw instellen, moet u rekening houden met het volgende voorbehoud:

* Wanneer beheerders vereisen een methode dat worden gebruikt om een wachtwoord opnieuw instellen, de verificatiecode is de enige beschikbare optie.
* Wanneer beheerders vereisen twee methoden dat worden gebruikt om een wachtwoord opnieuw instellen, gebruikers mogen gebruiken **uitvoeren** melding **of** verificatiecode naast eventuele andere methoden ingeschakeld.

| Het aantal methoden dat is vereist om het wachtwoord opnieuw in te stellen | Een | Twee |
| :---: | :---: | :---: |
| Mam-functies die beschikbaar zijn | Code | Code- of -meldingen |

Gebruikers hebben niet de optie voor het registreren van hun mobiele app bij het registreren voor self-service voor wachtwoord opnieuw van instellen [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup). Gebruikers kunnen hun mobiele app registreren [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup), of in de nieuwe preview-info registratie beveiliging op [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo).

> [!WARNING]
> U moet inschakelen de [geconvergeerd registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication (openbare preview)](concept-registration-mfa-sspr-converged.md) voordat gebruikers toegang kunnen krijgen tot de nieuwe ervaring op [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo).

### <a name="change-authentication-methods"></a>Verificatiemethoden wijzigen

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
3. Gebruikers zonder de mobiele telefoon of uw alternatieve e-mailadres velden ingevuld kunnen niet hun wachtwoord opnieuw instellen.

## <a name="registration"></a>Registratie

### <a name="require-users-to-register-when-they-sign-in"></a>Vereisen dat gebruikers te registreren wanneer ze zich aanmelden

Als u deze optie inschakelt, moet de gebruiker het wachtwoord opnieuw instellen om inschrijving te voltooien als ze zich aanmelden bij toepassingen die gebruikmaken van Azure AD. Deze werkstroom bevat de volgende toepassingen:

* Office 365
* Azure Portal
* Toegangsvenster
* Federatieve toepassingen
* Aangepaste toepassingen met behulp van Azure AD

Wanneer registratie vereisen is uitgeschakeld, worden gebruikers kunnen handmatig registreren. Ze kunnen beide Bezoek [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup) of Selecteer de **registreren voor wachtwoordherstel** koppeling onder de **profiel** tabblad in het toegangsvenster.

> [!NOTE]
> Gebruikers de registratieportal voor wachtwoordherstel kunnen sluiten door te selecteren **annuleren** of door het venster te sluiten. Maar ze wordt gevraagd om u te registreren telkens wanneer ze zich aanmelden totdat ze hun inschrijving.
>
> Deze interrupt verbroken niet van de gebruiker verbinding als ze al zijn aangemeld.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Stel het aantal dagen waarna gebruikers wordt gevraagd naar de verificatiegegevens

Deze optie bepaalt u de periode tussen instellen en de verificatiegegevens reconfirming en is alleen beschikbaar als u inschakelt de **vereisen dat gebruikers zich bij aanmelding registreren** optie.

Geldige waarden zijn 0-730 dagen met '0', wat betekent dat gebruikers nooit wordt gevraagd om u te verificatiegegevens.

## <a name="notifications"></a>Meldingen

### <a name="notify-users-on-password-resets"></a>Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord

Als deze optie is ingesteld op **Ja**, gebruikers hun wachtwoord opnieuw instellen, ontvangen een e-mailbericht melding dat hun wachtwoord is gewijzigd. Het e-mailbericht wordt verzonden via de SSPR-portal naar de primaire en alternatieve e-mailadressen die zich in het bestand in Azure AD. Niemand anders wordt de hoogte gebracht van de gebeurtenis opnieuw instellen.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen

Als deze optie is ingesteld op **Ja**, klikt u vervolgens *alle beheerders* ontvangt een e-mail naar het primaire e-mailadres in het bestand in Azure AD. Het e-mailbericht dat de melding verschijnt dat een andere beheerder het wachtwoord is gewijzigd met behulp van self-service voor Wachtwoordherstel.

Voorbeeld: Er zijn vier beheerders in een omgeving. Hun wachtwoord opnieuw instellen via Self-service voor Wachtwoordherstel van een beheerder. Beheerders B, C en D ontvangen een e-mailbericht waarschuwingen van het wachtwoord opnieuw instellen.

## <a name="on-premises-integration"></a>On-premises integratie

Als u installeren, configureren en Azure AD Connect inschakelen, hebt u de volgende aanvullende opties voor on-premises integraties. Als deze opties lichter gekleurd weergegeven zijn, is klikt u vervolgens Write-back niet juist geconfigureerd. Zie voor meer informatie, [configuratie van wachtwoord terugschrijven](howto-sspr-writeback.md).

![Writeback][Writeback]

Deze pagina vindt u een snel de status van de on-premises Write-backclient, een van de volgende berichten wordt weergegeven op basis van de huidige configuratie:

* Uw On-premises Write-backclient is actief en werkend.
* Azure AD is online en is verbonden met uw on-premises Write-backclient. Echter, het lijkt erop dat de geïnstalleerde versie van Azure AD Connect verouderd is. Houd rekening met [een upgrade van Azure AD Connect](../hybrid/how-to-upgrade-previous-version.md) om ervoor te zorgen dat u de nieuwste connectiviteitsfuncties en belangrijke bugfixes beschikt hebt.
* Helaas kan niet worden gecontroleerd de status van uw on-premises Write-back-client omdat de geïnstalleerde versie van Azure AD Connect verouderd is. [Azure AD Connect upgraden](../hybrid/how-to-upgrade-previous-version.md) kunnen om te controleren of de status van de verbinding.
* Helaas, het lijkt erop dat we nu kan geen verbinding met uw on-premises Write-backclient. [Azure AD Connect oplossen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) om de verbinding te herstellen.
* Helaas geen verbinding maken met uw on-premises Write omdat wachtwoord terugschrijven niet juist is geconfigureerd. [Configureer wachtwoord terugschrijven](howto-sspr-writeback.md) om de verbinding te herstellen.
* Helaas, het lijkt erop dat we nu kan geen verbinding met uw on-premises Write-backclient. Dit kan zijn veroorzaakt door tijdelijke problemen aan onze kant. Als het probleem zich blijft voordoen, [los problemen met Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) om de verbinding te herstellen.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Wachtwoorden terugschrijven naar uw on-premises directory

Dit besturingselement bepaalt of het terugschrijven van wachtwoorden is ingeschakeld voor deze map. Als u terugschrijven van wachtwoorden is ingeschakeld, betekent dit de status van de on-premises Write-back-service. Dit besturingselement is handig als u het terugschrijven van wachtwoorden tijdelijk uitschakelen wilt zonder Azure AD Connect opnieuw configureren.

* Als de switch is ingesteld op **Ja**, klikt u vervolgens het terugschrijven van wachtwoorden is ingeschakeld en federatieve, Pass through-verificatie of de wachtwoord-hash gesynchroniseerd gebruikers kunnen hun wachtwoord opnieuw instellen.
* Als de switch is ingesteld op **Nee**, klikt u vervolgens Write-back is uitgeschakeld en federatieve, Pass through-verificatie of de wachtwoord-hash gesynchroniseerd gebruikers zijn niet in staat hun wachtwoord opnieuw instellen.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Toestaan dat gebruikers voor het ontgrendelen van accounts zonder hun wachtwoord opnieuw instellen

Dit besturingselement Hiermee wordt aangegeven of gebruikers die gaat u naar de portal voor wachtwoord opnieuw instellen van moeten de optie voor het ontgrendelen van hun on-premises Active Directory-accounts zonder hun wachtwoord opnieuw in te krijgen. Azure AD ontgrendelt standaard accounts bij het uitvoeren van een wachtwoord opnieuw instellen. U kunt deze instelling gebruiken voor het scheiden van deze twee bewerkingen. 

* Indien ingesteld op **Ja**, en vervolgens gebruikers de mogelijkheid hun wachtwoord opnieuw instellen en het account ontgrendelen of hun account ontgrendelen krijgen zonder dat het wachtwoord opnieuw instellen.
* Indien ingesteld op **Nee**, klikt u vervolgens gebruikers zijn alleen kunnen worden gecombineerd voor wachtwoordherstel en bewerking ontgrendelen van het account.

### <a name="on-premises-active-directory-password-filters"></a>On-premises Active Directory-wachtwoordfilters

Azure AD-selfservice wachtwoord opnieuw instellen, voert het equivalent van een wachtwoord door de beheerder opnieuw instellen in Active Directory. Als u een derde partij wachtwoordfilter gebruikt voor het afdwingen van regels voor aangepaste wachtwoorden, en u wilt dat dit wachtwoordfilter is ingeschakeld tijdens de Azure AD selfservice voor wachtwoordherstel, zorg ervoor dat de oplossing van derden wachtwoord-filter is geconfigureerd om toe te passen de beheerderswachtwoord opnieuw instellen van scenario. [Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory](concept-password-ban-bad-on-premises.md) wordt standaard ondersteund.

## <a name="password-reset-for-b2b-users"></a>Wachtwoord opnieuw instellen voor B2B-gebruikers

Wachtwoord opnieuw instellen en wijzigen worden volledig ondersteund in alle configuraties met business-to-business (B2B). Wachtwoorden opnieuw instellen van B2B-gebruikers wordt ondersteund in de volgende drie gevallen:

   * **Gebruikers van de partnerorganisatie van een met een bestaande Azure AD-tenant**: Als de organisatie die u zijn een partnerschap aangegaan met een bestaande Azure AD-tenant, we *respecteren ongeacht wachtwoord opnieuw instellen van beleidsregels zijn ingeschakeld op deze tenant*. Voor wachtwoord opnieuw instellen om te werken, moet de partnerorganisatie alleen om ervoor te zorgen dat Azure AD SSPR is ingeschakeld. Er zijn geen extra kosten voor Office 365-klanten en kan worden ingeschakeld met de volgende stappen in onze [aan de slag met wachtwoordbeheer](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) handleiding.
   * **Gebruikers die zich aanmelden via** self-service-aanmelding: Als de organisatie samenwerking met gebruikt de [self-service-aanmelding](../users-groups-roles/directory-self-service-signup.md) functie om op te halen in een tenant, laat Microsoft ze opnieuw instellen van het wachtwoord met het e-mailbericht ze geregistreerd.
   * **B2B-gebruikers**: Een nieuwe B2B-gebruikers die zijn gemaakt met behulp van de nieuwe [Azure AD B2B-mogelijkheden](../active-directory-b2b-what-is-azure-ad-b2b.md) worden ook opnieuw instellen van wachtwoorden met het e-mailbericht ze tijdens de uitnodiging hebt geregistreerd.

Als u wilt testen in dit scenario, gaat u naar https://passwordreset.microsoftonline.com met een van deze gebruikers van partnerorganisaties. Als ze hebben een alternatief e-mailadres of verificatieadres die zijn gedefinieerd, heeft wachtwoord opnieuw instellen werkt zoals verwacht.

> [!NOTE]
> Microsoft-accounts die toegang voor gasten zijn verleend aan uw Azure AD-tenant, zoals die van Hotmail.com, Outlook.com of andere persoonlijke e-mailadressen zijn niet Azure AD SSPR kunnen gebruiken. Moeten ze hun wachtwoord opnieuw instellen met behulp van de informatie in de [wanneer u zich niet aanmelden bij uw Microsoft-account](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artikel.

## <a name="next-steps"></a>Volgende stappen

De volgende koppelingen bieden aanvullende informatie over wachtwoordherstel met behulp van Azure AD:

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md)
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
