---
title: 'Veelgestelde vragen over: Azure AD SSPR | Microsoft Docs'
description: Veelgestelde vragen over Azure AD zelf uw wachtwoord opnieuw instellen
services: active-directory
keywords: Wachtwoordbeheer Active directory, wachtwoordbeheer, Azure AD self service voor wachtwoordherstel
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 53075d20aff073ff46dcd6dccaefea5fc8ec3483
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="password-management-frequently-asked-questions"></a>Veelgestelde vragen over wachtwoordbeheer

Hier volgen enkele veelgestelde vragen voor alle bewerkingen die betrekking hebben op wachtwoord opnieuw instellen.

Als er een algemene vraag over Azure AD en zelf uw wachtwoord opnieuw instellen, die niet wordt beantwoord hier, kunt u vragen de community om hulp op de [Azure Ad-forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Leden van de community opnemen Engineers, Product Managers, MVP's en fellow IT-Professionals.

Deze Veelgestelde vragen wordt opgedeeld in de volgende secties:

* [**Vragen over het registreren voor wachtwoord opnieuw instellen**](#password-reset-registration)
* [**Vragen over wachtwoordherstel**](#password-reset)
* [**Vragen over het wijzigen van wachtwoorden**](#password-change)
* [**Rapporten voor vragen over wachtwoordbeheer**](#password-management-reports)
* [**Vragen over Write-back van wachtwoord**](#password-writeback)

## <a name="password-reset-registration"></a>Registratie voor wachtwoord opnieuw instellen

* **V: kunnen mijn gebruikers registreren hun eigen wachtwoord opnieuw instellen van gegevens?**

  > **A:** Ja, zoals wachtwoord opnieuw instellen is ingeschakeld en worden ze in licentie gegeven, kunnen ze naar de portal van de registratie voor wachtwoord opnieuw instellen op http://aka.ms/ssprsetup registreren hun verificatiegegevens. Gebruikers kunnen ook registreren door te gaan naar het toegangspaneel bij http://myapps.microsoft.com, te klikken op het profieltabblad en te klikken op het Register voor de optie wachtwoord opnieuw instellen.
  >
  >
* **V: kan ik wachtwoord opnieuw instellen van gegevens definiëren namens mijn gebruikers?**

  > **A:** Ja, u kunt dit doen met Azure AD Connect, PowerShell, de [Azure-portal](https://portal.azure.com), of de beheerder van Office-portal. Zie voor meer informatie het artikel [gegevens die worden gebruikt door Azure AD selfservice voor wachtwoordherstel](active-directory-passwords-data.md).
  >
  >
* **V: kan ik gegevens voor vragen over de beveiliging van on-premises synchroniseren?**

  > **A:** dit niet mogelijk is vandaag de dag.
  >
  >
* **V: kunnen mijn gebruikers gegevens registreren zodanig dat deze gegevens door andere gebruikers niet zien?**

  > **A:** Ja, wanneer gebruikers zich registreren voor gegevens met behulp van de Wachtwoordregistratieportal opnieuw instellen dat het wordt opgeslagen in persoonlijke verificatie velden die alleen zichtbaar voor globale beheerders en de gebruiker.
  >
  >
* **V: Mijn gebruikers hoeft te worden geregistreerd voordat ze kunnen voor wachtwoordherstel gebruiken?**

  > **A:** Nee, als u onvoldoende verificatiegegevens namens hen definieert, gebruikers niet hoeven te registreren. Wachtwoord opnieuw moet worden ingesteld als u hebt opgeslagen gegevens in de juiste velden in de map juist opgemaakt.
  >
  >
* **V: kan ik synchroniseren of instellen van de telefoon voor authenticatie, authenticatie-e-mailadres of andere telefoon voor authenticatie velden namens mijn gebruikers?**

  > **A:** dit niet mogelijk is vandaag de dag.
  >
  >
* **V: hoe de portal voor wachtwoordregistratie weet welke opties mijn gebruikers?**

  > **A:** de registratieportal voor wachtwoordherstel alleen ziet u de opties die u hebt ingeschakeld voor uw gebruikers. Deze opties zijn gevonden onder de sectie gebruiker opnieuw instellen wachtwoordbeleid van uw directory configureren tabblad. Dit betekent bijvoorbeeld dat als u vragen over de beveiliging niet inschakelt, klikt u vervolgens gebruikers zich niet kunnen registreren voor die optie.
  >
  >
* **V: wanneer een gebruiker als beschouwd ingeschreven?**

  > **A:** een gebruiker wordt beschouwd geregistreerd voor SSPR, wanneer zij hebben geregistreerd ten minste de **aantal methoden die zijn vereist om in te stellen** die u hebt ingesteld in de [Azure-portal](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Wachtwoord opnieuw instellen

* **V: hoe lang moet ik wachten om te ontvangen van een e-mail, SMS of telefoongesprek van wachtwoord opnieuw instellen?**

  > **A:** SMS-berichten, e-mailadres en telefoongesprekken moet binnenkomen in onder één minuut, met het normale geval wordt de 5-20 seconden.
    >Als u de melding niet binnen deze tijd ontvangt:
        > * Controleer de map Ongewenste e-mail.
        > * Controleer het nummer of de e-mailadres waarmee contact wordt opgenomen is die u verwacht.
        > * Controleer dat de verificatiegegevens in de map is juist opgemaakt.
                >     * Voorbeeld: "+ 1 4255551234' of 'user@contoso.com'
  >
  >
* **V: welke talen worden ondersteund door het wachtwoord opnieuw instellen?**

  > **A:** -gebruikersinterface voor het wachtwoord opnieuw instellen van SMS-berichten en telefoongesprekken in dezelfde talen worden ondersteund in Office 365 worden gelokaliseerd.
  >
  >
* **V: welke onderdelen van de ervaring van wachtwoord opnieuw instellen wanneer ik ingesteld organisatie huisstijl in mijn directory ophalen huisstijl het tabblad configureren?**

  > **A:** de portal voor wachtwoord opnieuw instellen, ziet u het logo van uw organisatie en kunt u configureren van de contact op met uw beheerder koppeling om te verwijzen naar een aangepaste e-mailadres of URL. Een e-mailbericht dat wordt verzonden door het wachtwoord opnieuw instellen van uw organisatie logo, de kleuren, de naam in de hoofdtekst van het e-mailbericht bevat en aangepast met de naam van.
  >
  >
* **V: hoe kan ik mijn gebruikers opleiden waar u om hun wachtwoord opnieuw instellen?**

  > **A:** kunt proberen de suggesties in onze [SSPR implementatie artikel](active-directory-passwords-best-practices.md#email-based-rollout)
  >
  >
* **V: kan ik deze pagina vanaf een mobiel apparaat gebruiken?**

  > **A:** Ja, deze pagina werkt op mobiele apparaten.
  >
  >
* **V: ondersteund ontgrendelen lokale active directory-accounts wanneer gebruikers hun wachtwoord opnieuw instellen?**

  > **A:** Ja, als hun wachtwoord opnieuw instellen van een gebruiker en wachtwoord terugschrijven is geïmplementeerd met Azure AD Connect, die gebruikersaccount is automatisch ontgrendeld wanneer ze hun wachtwoord opnieuw instellen.
  >
  >
* **V: hoe kan ik wachtwoordherstel rechtstreeks in mijn gebruiker aanmelden Bureaubladervaring worden geïntegreerd?**

  > **A:** als u een Azure AD Premium-klant bent, kunt u Microsoft Identity Manager installeren zonder extra kosten en implementeren van de on-premises-oplossing wachtwoord opnieuw instellen om te voldoen aan deze vereiste.
  >
  >
* **V: kan ik andere beveiligingsvragen voor verschillende talen instellen?**

  > **A:** dit niet mogelijk is vandaag de dag.
  >
  >
* **V: hoe zoveel mogelijk vragen kunnen we configureren voor de verificatie-optie beveiligingsvragen**

  > **A:** kunt u maximaal 20 aangepaste beveiligingsvragen in de [Azure-portal](https://portal.azure.com).
  >
  >
* **V: hoe lang mogelijk beveiligingsvragen?**

  > **A:** beveiligingsvragen mogelijk tussen 3 en 200 tekens bevatten.
  >
  >
* **V: hoe lang mogelijk antwoorden op beveiligingsvragen?**

  > **A:** antwoorden mogelijk 3 tot 40 tekens lang zijn.
  >
  >
* **V: zijn dubbele antwoorden op beveiligingsvragen afgewezen?**

  > **A:** Ja, er dubbele antwoorden op beveiligingsvragen afwijzen.
  >
  >
* **V: kan registreren van een gebruiker de dezelfde beveiligingsvraag het meer dan één keer?**

  > **A:** Nee, wanneer een gebruiker zich registreert voor een bepaald criterium, ze kunnen niet worden geregistreerd voor deze vraag een tweede keer.
  >
  >
* **V: is het mogelijk is de ondergrens van beveiligingsvragen voor registratie instellen en resetten?**

  > **A:** Ja, een limiet kan worden ingesteld voor registratie en een andere voor het opnieuw instellen. 3-5-beveiligingsvragen mogelijk zijn vereist voor inschrijving en 3-5 mogelijk zijn vereist voor opnieuw instellen.
  >
  >
* **V: ik mijn beleid om te vereisen dat gebruikers beveiligingsvragen gebruiken voor het opnieuw instellen hebt geconfigureerd, maar Azure beheerders lijken anders worden geconfigureerd.**

  > **A:** dit is het verwachte gedrag. Microsoft zorgt ervoor dat een sterke twee gate wachtwoord opnieuw instellen van standaardbeleid voor de rol van een Azure-beheerder. Beheerders van het gebruik van beveiligingsvragen wordt uitgeschakeld. Meer informatie over dit beleid kan worden gevonden in het artikel [wachtwoordbeleid en -beperkingen in Azure Active Directory](active-directory-passwords-policy.md#administrator-password-policy-differences)
  >
  >
* **V: als een gebruiker meer dan het maximum aantal vragen dat vereist is om in te stellen is geregistreerd, hoe beveiligingsvragen geselecteerd tijdens het opnieuw instellen?**

  > **A:** N beveiligingsvragen willekeurig geselecteerd van het totale aantal vragen dat een gebruiker is geregistreerd, waarbij N staat voor de **aantal vragen dat vereist is om in te stellen**. Bijvoorbeeld, als een gebruiker 5 beveiligingsvragen geregistreerd heeft, maar alleen 3 nodig zijn om in te stellen, zijn 3 van de 5 willekeurig geselecteerd en gepresenteerd op opnieuw instellen. Als de gebruiker de antwoorden op de vragen verkeerde ontvangt, wordt het selectieproces om te voorkomen dat vraag hammering optreedt.
  >
  >
* **V: heb u voorkomen dat gebruikers vaak in een korte periode voor wachtwoordherstel probeert?**

  > **A:** Ja, er zijn ingebouwd in het wachtwoord opnieuw instellen om te beveiligen tegen misbruik beveiligingsfuncties. Gebruikers kunnen alleen proberen opnieuw instellen van 5 wachtwoordpogingen binnen een uur voordat ze worden geblokkeerd voor 24 uur. Gebruikers kunnen alleen proberen te valideren een telefoonnummer 5 maal binnen een uur voordat ze worden geblokkeerd voor 24 uur. Gebruikers kunnen alleen een één verificatiemethode proberen 5 maal binnen een uur voordat ze worden geblokkeerd voor 24 uur.
  >
  >
* **V: voor hoe lang de e-mail en SMS eenmalige wachtwoordcode geldig zijn?**

  > **A:** de levensduur van de sessie voor wachtwoordherstel is 15 minuten. De gebruiker heeft vanaf het begin van de bewerking van wachtwoord opnieuw instellen van 15 minuten hun wachtwoord opnieuw instellen. De e-mail en SMS eenmalige wachtwoordcode zijn ongeldig nadat deze periode is verstreken.
  >
  >

## <a name="password-change"></a>Wachtwoord wijzigen

* **V: waar moeten Mijn gebruikers hun wachtwoord moeten wijzigen gaan?**

  > **A:** gebruikers kunnen hun wachtwoord wijzigen waar ze hun profielfoto of pictogram ziet (zoals in de rechterbovenhoek van hun [Office 365](https://portal.office.com) of [Toegangspaneel](https://myapps.microsoft.com) optreedt. Gebruikers kunnen hun wachtwoorden op wijzigen de [Toegangspaneel profielpagina](https://account.activedirectory.windowsazure.com/r#/profile). Gebruikers kunnen ook worden gevraagd hun wachtwoord moeten wijzigen automatisch op de Azure AD in het scherm als hun wachtwoord is verlopen. Ten slotte gebruikers mogelijk gaat u naar de [Azure AD-wachtwoord wijzigen Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) rechtstreeks als ze willen hun wachtwoord wijzigen.
  >
  >
* **V: kunnen mijn gebruikers worden gewaarschuwd in de Office-Portal wanneer hun on-premises wachtwoord is verlopen?**

  > **A:** dit vandaag is mogelijk als u AD FS gebruikt door de volgende instructies te volgen: [wachtwoord beleid Claims verzenden met AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Als u synchronisatie van wachtwoordhash, is dit niet mogelijk vandaag. Dit is omdat we niet wachtwoordbeleid van on-premises synchroniseren zodat het is niet mogelijk om te posten verlopen-meldingen naar cloud ervaringen. In beide gevallen is het ook mogelijk te [Waarschuw gebruikers waarvan de wachtwoorden bijna verlopen zijn met behulp van PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >

## <a name="password-management-reports"></a>Wachtwoord-rapporten

* **V: hoe lang duurt het voor de gegevens worden weergegeven op de wachtwoord-rapporten?**

  > **A:** gegevens op de wachtwoord-rapporten moet worden weergegeven in 5-10 minuten. Deze sommige gevallen is het een uur duren kan om te worden weergegeven.
  >
  >
* **V: hoe kan ik de wachtwoord-rapporten filteren?**

  > **A:** kunt u de wachtwoord-rapporten filteren door te klikken op het Vergrootglas kleine uiterst rechts van de kolomlabels boven aan het rapport. Als u doen uitgebreidere filteren wilt, kunt u het rapport naar excel en maak een draaitabel kunt downloaden.
  >
  >
* **V: Wat is het maximum aantal gebeurtenissen in de wachtwoord-rapporten worden opgeslagen?**

  > **A:** maximaal 75.000 wachtwoord opnieuw instellen of het wachtwoord opnieuw instellen van inschrijving gebeurtenissen worden opgeslagen in de wachtwoord-rapporten, back-spanning maximaal 30 dagen.  We werken als u dit aantal zodanig dat meer gebeurtenissen wilt uitbreiden.
  >
  >
* **V: hoe ver terug de wachtwoordbeheer rapporten gaan doen?**

  > **A:** de wachtwoordbeheer rapporten weergeven-bewerkingen plaatsvinden in de afgelopen 30 dagen. Op dit moment als u nodig hebt om deze gegevens te archiveren kunt u periodiek de rapporten downloaden en opslaan op een andere locatie.
  >
  >
* **V: is er een maximale aantal rijen dat kan worden weergegeven op de wachtwoord-rapporten?**

  > **A:** Ja, maximaal 75.000 rijen kan worden weergegeven op een van de wachtwoord-rapporten, of ze worden weergegeven in de gebruikersinterface of wordt gedownload.
  >
  >
* **V: is er een API voor toegang tot het wachtwoord opnieuw instellen of de registratie rapportagegegevens?**

  > **A:** Ja, Zie de volgende documentatie voor meer informatie over hoe u toegang hebt tot de rapportage van de gegevensstroom voor wachtwoordherstel.  [Informatie over wachtwoordherstel rapportages programmatisch toegang tot](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Wachtwoord terugschrijven

* **V: hoe werkt wachtwoord terugschrijven achter de schermen?**

  > **A:** Zie [de werking van wachtwoord terugschrijven](active-directory-passwords-writeback.md) voor een uitleg van wat er gebeurt als u terugschrijven van wachtwoord en hoe gegevens via het systeem loopt terug naar uw on-premises-omgeving inschakelen.
  >
  >
* **V: hoe lang duurt wachtwoord terugschrijven voordat werken?  Is er een vertraging synchronisatie zoals met wachtwoordhashsynchronisatie?**

  > **A:** wachtwoord terugschrijven is snel. Het is een synchrone pijplijn die fundamenteel anders dan de synchronisatie van wachtwoordhash werkt. Wachtwoord terugschrijven kan gebruikers realtime feedback over het succes van hun wachtwoord opnieuw instellen of wijzigen van de bewerking. De gemiddelde tijd voor een geslaagde write-back van een wachtwoord is onder 500 ms.
  >
  >
* **V: hoe wordt mijn account/toegang tot de cloud getroffen als mijn lokale account is uitgeschakeld?**

  > **A:** als uw lokale ID is uitgeschakeld, uw cloud-ID/access worden ook uitgeschakeld op de volgende synchronisatie-interval via AAD Connect standaard is dit elke 30 minuten.
  >
  >
* **V: als mijn lokale account wordt beperkt door een beleid on-premises Active Directory-wachtwoord, houdt SSPR zich aan dit beleid wanneer ik het wachtwoord wijzigen?**

  > **A:** Ja, SSPR is afhankelijk van en houdt zich aan de on-premises AD-wachtwoordbeleid, met inbegrip van typische wachtwoordbeleid van de AD-domein, evenals alle fijnmazige wachtwoordbeleid dat is bestemd voor een bepaalde gebruiker gedefinieerd.
  >
  >
* **V: wat typen accounts werkt wachtwoord terugschrijven voor?**

  > **A:** wachtwoord terugschrijven is geschikt voor gebruikers van federatieve en het wachtwoord-Hash gesynchroniseerd.
  >
  >
* **V: bevat wachtwoord terugschrijven mijn domein wachtwoordbeleid afdwingen?**

  > **A:** Ja, wachtwoord terugschrijven worden afgedwongen wachtwoord leeftijd, geschiedenis, complexiteit, filters en eventuele andere beperkingen die u in plaats van wachtwoorden in uw lokale domein kan plaatsen.
  >
  >
* **V: wachtwoord terugschrijven veilig is?  Hoe kan ik dat ik won't ingebroken zijn?**

  > **A:** Ja, Write-back van wachtwoord is beveiligd. Bekijk meer informatie over de vier beveiligingslagen geïmplementeerd door de service van de Write-back van wachtwoord, de [wachtwoord terugschrijven beveiligingsmodel](active-directory-passwords-writeback.md#password-writeback-security-model) sectie in de werking van wachtwoord terugschrijven.
  >
  >

## <a name="next-steps"></a>Volgende stappen

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
