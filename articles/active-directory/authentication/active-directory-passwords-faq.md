---
title: Selfservice voor wachtwoordherstel FAQ - Azure Active Directory
description: Veelgestelde vragen over Azure AD Self-service voor wachtwoord opnieuw instellen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcb825a193071d263d800dafdd6296246eb74aec
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57878190"
---
# <a name="password-management-frequently-asked-questions"></a>Veelgestelde vragen over wachtwoordbeheer

Hier volgen enkele veelgestelde vragen (FAQ) voor alles met betrekking tot wachtwoord opnieuw instellen.

Als u een algemene vraag hebt over Azure Active Directory (Azure AD) en selfservice voor wachtwoordherstel (SSPR) die hier niet wordt beantwoord, u kunt vraag het de community voor hulp op het [Azure AD-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Leden van de community zijn engineers, productmanagers, MVP's en andere IT-professionals.

Deze Veelgestelde vragen is opgesplitst in de volgende secties:

* [Registratie voor vragen over het wachtwoord opnieuw instellen](#password-reset-registration)
* [Vragen over het wachtwoord opnieuw instellen](#password-reset)
* [Vragen over het wijzigen van wachtwoord](#password-change)
* [Vragen over wachtwoord-rapporten](#password-management-reports)
* [Vragen over het terugschrijven van wachtwoorden](#password-writeback)

## <a name="password-reset-registration"></a>Registratie van het opnieuw ingestelde wachtwoord

* **V:  Kunnen mijn gebruikers registreren hun eigen wachtwoord opnieuw instellen van gegevens?**

  > **A:** Ja. Als voor wachtwoord opnieuw instellen is ingeschakeld en ze een licentie hebben, kunnen gebruikers gaan naar de registratieportal voor wachtwoordherstel (https://aka.ms/ssprsetup) hun verificatiegegevens te registreren. Gebruikers kunnen ook registreren via het toegangsvenster (https://myapps.microsoft.com). Als u wilt registreren via het toegangsvenster, moeten ze hun profielfoto selecteert, selecteert u **profiel**, en selecteer vervolgens de **registreren voor wachtwoordherstel** optie.
  >
  >
* **V:  Als ik wachtwoord inschakelt opnieuw instellen voor een groep en vervolgens besluit zodat het voor iedereen zijn mijn gebruikers vereist opnieuw registreren?**

  > **A:** Nee. Gebruikers die gegevens van de verificatie hebt ingevuld, hoeven niet opnieuw registreren.
  >
  >
* **V:  Kan ik wachtwoord opnieuw instellen van gegevens definiëren namens mijn gebruikers?**

  > **A:** Ja, u kunt dit doen met Azure AD Connect, PowerShell, de [Azure-portal](https://portal.azure.com), of het Office 365-beheercentrum. Zie voor meer informatie, [gegevens die worden gebruikt door Azure AD Self-service voor wachtwoord opnieuw instellen](howto-sspr-authenticationdata.md).
  >
  >
* **V:  Kan ik gegevens voor vragen over de beveiliging van on-premises synchroniseren?**

  > **A:** Nee, dit is niet mogelijk vandaag nog.
  >
  >
* **V:  Mijn gebruikers kunnen registreren gegevens zodanig dat andere gebruikers kunnen deze gegevens niet zien?**

  > **A:** Ja. Wanneer gebruikers zich registreren gegevens met behulp van het wachtwoord opnieuw instellen-portal voor wachtwoordregistratie, de gegevens worden opgeslagen in persoonlijke verificatie velden die alleen zichtbaar voor globale beheerders en de gebruiker zijn.
  >
  >
* **V:  Mijn gebruikers moet worden geregistreerd voordat ze opnieuw instellen van wachtwoord kunnen gebruiken?**

  > **A:** Nee. Als u voldoende verificatie-informatie namens hen definieert, worden gebruikers niet hoeven te registreren. Wachtwoord opnieuw instellen werkt, zolang u de gegevens die zijn opgeslagen in de juiste velden in de map correct zijn opgemaakt.
  >
  >
* **V:  Kan ik synchroniseren of instellen van de telefoon voor authenticatie, verificatie-e-mailadres of verificatie via telefoonnummer velden namens mijn gebruikers?**

  > **A:** De velden die kunnen worden ingesteld door een globale beheerder zijn gedefinieerd in het artikel [gegevensvereisten voor SSPR](howto-sspr-authenticationdata.md).
  >
  >
* **V:  Hoe de portal voor wachtwoordregistratie bepalen welke opties om weer te geven van mijn gebruikers?**

  > **A:** De registratieportal voor wachtwoordherstel ziet u alleen de opties die u hebt ingeschakeld voor uw gebruikers. Deze opties vindt u onder de **gebruiker opnieuw instellen wachtwoordbeleid** sectie van uw directory **configureren** tabblad. Bijvoorbeeld, als u vragen over de beveiliging niet inschakelt, zich klikt u vervolgens gebruikers niet kunnen registreren voor die optie.
  >
  >
* **V:  Wanneer wordt een gebruiker beschouwd als geregistreerd?**

  > **A:** Een gebruiker wordt beschouwd als geregistreerd voor self-service voor Wachtwoordherstel wanneer ze zich hebben geregistreerd ten minste de **aantal methoden dat is vereist om opnieuw in te** een wachtwoord dat u hebt ingesteld in de [Azure-portal](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Wachtwoord opnieuw instellen

* **V:  Voorkomen u dat gebruikers meerdere pogingen om een wachtwoord opnieuw instellen in een korte periode?**

  > **A:** Ja, zijn er beveiligingsfuncties die is ingebouwd in het wachtwoord opnieuw instellen om deze te beschermen tegen misbruik. 
  >
  > Gebruikers kunnen proberen slechts vijf wachtwoord opnieuw instellen hebt ingevoerd binnen een periode van 24 uur voordat ze zijn vergrendeld voor 24 uur. 
  >
  > Gebruikers kunnen proberen te valideren van een telefoonnummer, een SMS-bericht sturen of valideren beveiligingsvragen en antwoorden slechts vijf keer binnen een uur voordat ze zijn vergrendeld voor 24 uur. 
  >
  > Gebruikers kunnen een e-mailbericht verzenden maximaal 10 keer binnen een periode van 10 minuten voordat ze zijn vergrendeld voor 24 uur.
  >
  > De items worden opnieuw ingesteld zodra een gebruiker hun wachtwoord opnieuw instellen.
  >
  >
* **V:  Hoe lang moet ik wachten om te ontvangen van een e-mail, SMS of telefonische oproep van wachtwoord opnieuw instellen?**

  > **A:** E-mails, SMS-berichten en telefoongesprekken moet binnenkomen minder dan een minuut. De normale case is 5 tot en met 20 seconden.
  > Als u deze melding niet binnen dit tijdsbestek ontvangt:
  > * Controleer de map Ongewenste e-mail.
  > * Controleer of het aantal bestanden of e-mailadres waarmee contact wordt opgenomen die u verwacht.
  > * Controleer of de verificatiegegevens in de map juist is opgemaakt, bijvoorbeeld: + 1 4255551234 of *gebruiker\@contoso.com*. 
* **V:  Welke talen worden ondersteund door het wachtwoord opnieuw instellen?**

  > **A:** Het wachtwoord opnieuw instellen-gebruikersinterface SMS-berichten en gesprekken in de dezelfde talen die worden ondersteund in Office 365 worden gelokaliseerd.
  >
  >
* **V:  Bepaalt u welke onderdelen van de ervaring wachtwoordherstel ophalen merkproducten wanneer ik Stel voor de organisatie-huisstijl items in mijn map, het tabblad configureren?**

  > **A:** De portal voor wachtwoord opnieuw instellen van uw organisatie logo ziet en kunt u de koppeling 'Neem contact op met uw beheerder' om te verwijzen naar een aangepaste e-mail of een URL configureren. Een e-mailbericht dat wordt verzonden door het wachtwoord opnieuw instellen-logo, kleuren en de naam van uw organisatie in de hoofdtekst van het e-mailbericht bevat, en is aangepast via de instellingen voor die specifieke naam.
  >
  >
* **V:  Hoe kan ik mijn gebruikers over waar u om hun wachtwoord opnieuw in te trainen?**

  > **A:** Probeer enkele van de suggesties in onze [SSPR implementatie](howto-sspr-deployment.md#sample-communication) artikel.
  >
  >
* **V:  Kan ik deze pagina op een mobiel apparaat gebruiken?**

  > **A:** Ja, deze pagina werkt op mobiele apparaten.
  >
  >
* **V:  Bieden u ondersteuning voor lokale Active Directory-accounts ontgrendelen wanneer gebruikers hun wachtwoord opnieuw instellen?**

  > **A:** Ja. Wanneer een gebruiker stelt hun wachtwoord opnieuw in als het terugschrijven van wachtwoorden is geïmplementeerd via Azure AD Connect, is het account van die gebruiker automatisch ontgrendeld wanneer ze hun wachtwoord opnieuw instellen.
  >
  >
* **V:  Hoe Integreer ik wachtwoordherstel rechtstreeks op het bureaublad van de gebruiker aanmelden ervaring?**

  > **A:** Als u een Azure AD Premium-klant bent, kunt u Microsoft Identity Manager installeren zonder extra kosten en de on-premises wachtwoord opnieuw instellen-oplossing implementeren.
  >
  >
* **V:  Kan ik verschillende beveiligingsvragen voor verschillende talen instellen?**

  > **A:** Nee, dit is niet mogelijk vandaag nog.
  >
  >
* **V:  Het aantal vragen kan ik configureren voor de beveiligingsoptie voor de verificatie van vragen?**

  > **A:** U kunt maximaal 20 aangepaste beveiligingsvragen in configureren de [Azure-portal](https://portal.azure.com).
  >
  >
* **V:  Hoe lang kunnen beveiligingsvragen zijn?**

  > **A:** Vragen over de beveiliging kunnen 3 tot 200 tekens lang zijn.
  >
  >
* **V:  Hoe lang kunnen de antwoorden op vragen over de beveiliging zijn?**

  > **A:** Antwoorden kunnen 3 tot en met 40 tekens lang zijn.
  >
  >
* **V:  Dubbele antwoorden op beveiligingsvragen geweigerd zijn?**

  > **A:** Ja, we afwijzen dubbele antwoorden op vragen over de beveiliging.
  >
  >
* **V:  Kan een gebruiker de dezelfde beveiligingsvraag meer dan één keer registreren?**

  > **A:** Nee. Nadat een gebruiker zich registreert voor een bepaalde vraag, kunnen ze niet registreren voor deze vraag een tweede keer.
  >
  >
* **V:  Is het mogelijk een minimum van vragen over de beveiliging voor registratie en opnieuw instellen?**

  > **A:** Ja, een limiet kan worden ingesteld voor registratie en een andere voor het opnieuw instellen. Vragen over de beveiliging van de drie tot vijf kunnen vereist zijn voor de registratie, en drie tot vijf vragen is vereist voor het opnieuw instellen.
  >
  >
* **V:  Kan ik mijn beleid om te vereisen dat gebruikers vragen over de beveiliging gebruiken voor het opnieuw instellen hebt geconfigureerd, maar de Azure-beheerders lijken op verschillende manieren worden geconfigureerd.**

  > **A:** Dit is het verwachte gedrag. Microsoft dwingt voor elke Azure-beheerdersrol standaard een sterk beleid met twee-staps-verificatie af voor het opnieuw instellen van wachtwoorden. Dit voorkomt dat beheerders met behulp van vragen over de beveiliging. U vindt meer informatie over dit beleid in de [wachtwoordbeleid en beperkingen in Azure Active Directory](concept-sspr-policy.md) artikel.
  >
  >
* **V:  Als een gebruiker meer dan het maximum aantal vragen dat vereist is voor het opnieuw instellen is geregistreerd, hoe worden de beveiligingsvragen geselecteerd tijdens het opnieuw instellen?**

  > **A:** *N* aantal beveiligingsvragen worden willekeurig geselecteerd van het totale aantal vragen dat een gebruiker is geregistreerd voor waar *N* is het bedrag dat is ingesteld voor de **aantal vragen dat vereist is in te stellen** optie. Bijvoorbeeld, als een gebruiker vijf vragen over de beveiliging is geregistreerd, maar slechts drie vereist voor het opnieuw instellen van een wachtwoord zijn, drie van de vijf vragen willekeurig geselecteerd en vindt u op opnieuw instellen. Als u wilt voorkomen, vraag voorkomen als de gebruiker de antwoorden op vragen krijgt verkeerde het selectieproces opnieuw wordt gestart.
  >
  >
* **V:  Hoe lang worden de e-mail en SMS eenmalige wachtwoordcodes geldig?**

  > **A:** De levensduur van de sessie voor wachtwoord opnieuw instellen is 15 minuten. De gebruiker heeft vanaf het begin van de bewerking voor wachtwoordherstel, 15 minuten aan hun wachtwoord opnieuw instellen. De e-mailadres en een eenmalige wachtwoordcode per SMS zijn ongeldig nadat deze periode is verlopen.
  >
  >
* **V:  Kan ik voorkomen dat gebruikers hun wachtwoord opnieuw instelt?**

  > **A:** Ja, als u een groep gebruiken om in te schakelen van self-service voor Wachtwoordherstel, kunt u een afzonderlijke gebruiker verwijderen uit de groep waarmee gebruikers hun wachtwoord opnieuw instellen. Als de gebruiker een globale beheerder is, ze de mogelijkheid om hun wachtwoord opnieuw in te behouden en dit kan niet worden uitgeschakeld.
  >
  >

## <a name="password-change"></a>Wachtwoord wijzigen

* **V:  Waar moeten Mijn gebruikers hun wachtwoord moeten wijzigen gebleven?**

  > **A:** Gebruikers kunnen hun wachtwoord wijzigen waar ze ziet hun profielafbeelding of pictogram, zoals in de rechterbovenhoek van hun [Office 365](https://portal.office.com) portal of [Toegangsvenster](https://myapps.microsoft.com) optreedt. Gebruikers kunnen hun wachtwoord wijzigen de [profiel Toegangsvenster pagina](https://account.activedirectory.windowsazure.com/r#/profile). Gebruikers kunnen ook worden gevraagd hun wachtwoord moeten wijzigen automatisch op de aanmeldingspagina van Azure AD als hun wachtwoorden zijn verlopen. Ten slotte, kunnen gebruikers bladeren naar de [Azure AD portal voor wachtwoordwijziging](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) rechtstreeks als ze willen hun wachtwoord wijzigen.
  >
  >
* **V:  Kunnen mijn gebruikers worden geïnformeerd in de Office-portal wanneer hun on-premises wachtwoord is verlopen?**

  > **A:** Ja, dit is mogelijk vandaag nog als u Active Directory Federation Services (AD FS). Als u AD FS gebruikt, volgt u de instructies in de [verzenden wachtwoord beleid claims met AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) artikel. Als u wachtwoord-hashsynchronisatie, is dit niet mogelijk vandaag nog. We worden wachtwoordbeleid van on-premises directory's, niet gesynchroniseerd, zodat het is niet mogelijk is voor ons om verlopen-meldingen naar de cloud ervaringen te verzenden. In beide gevallen is het ook mogelijk om te [meldingen verzenden naar gebruikers waarvan de wachtwoorden worden binnenkort verloopt via PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **V:  Kan ik voorkomen dat gebruikers hun wachtwoord wijzigen?**

  > **A:** Voor alleen-cloud, gebruikers, kunnen niet wachtwoordwijzigingen worden geblokkeerd. Voor on-premises gebruikers, kunt u instellen de **gebruiker kan wachtwoord niet wijzigen** optie geselecteerd. De geselecteerde gebruikers wijzigen hun wachtwoord niet.
  >
  >

## <a name="password-management-reports"></a>Rapporten voor wachtwoord

* **V:  Hoe lang duurt het voordat gegevens worden weergegeven op de wachtwoord-rapporten?**

  > **A:** Gegevens moet worden weergegeven op de wachtwoord-rapporten in 5 tot 10 minuten. In sommige gevallen, kan het duren tot een uur worden weergegeven.
  >
  >
* **V:  Hoe kan ik de wachtwoord-rapporten filteren?**

  > **A:** Om te filteren de wachtwoord-rapporten, selecteer de kleine Vergrootglas uiterst rechts van de kolomlabels, aan de bovenkant van het rapport. Als u doen uitgebreidere filteren wilt, kunt u downloaden van het rapport naar Excel en maak een draaitabel.
  >
  >
* **V: Wat is het maximum aantal gebeurtenissen die zijn opgeslagen in de rapporten voor wachtwoord?**

  > **A:** Maximaal 75.000 voor wachtwoord opnieuw instellen of het wachtwoord opnieuw instellen van inschrijving gebeurtenissen opgeslagen in de rapporten voor wachtwoord, spanning terug tot 30 dagen. Er wordt gewerkt om uit te breiden van dit getal in om door te meer gebeurtenissen worden opgenomen.
  >
  >
* **V:  Hoe ver terug gaat u de rapporten voor wachtwoord?**

  > **A:** De wachtwoord-rapporten weergeven bewerkingen die zijn opgetreden in de afgelopen 30 dagen. Voorlopig als u nodig hebt om deze gegevens te archiveren kunt u periodiek de rapporten downloaden en opslaan in een andere locatie.
  >
  >
* **V:  Is er een maximum aantal rijen die kunnen worden weergegeven in de rapporten voor wachtwoord?**

  > **A:** Ja. Maximaal 75.000 rijen kan worden weergegeven in een van de rapporten voor wachtwoord, ongeacht of ze worden weergegeven in de gebruikersinterface of worden gedownload.
  >
  >
* **V:  Is er een API voor toegang tot het wachtwoord opnieuw instellen of de registratie waarvoor gegevens zijn gerapporteerd?**

  > **A:** Ja. Zie voor meer informatie hoe u toegang hebt tot de rapportage van de gegevensstroom voor wachtwoord opnieuw instellen, [informatie over toegang tot wachtwoord opnieuw instellen via een programma rapportagegebeurtenissen](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Wachtwoord terugschrijven

* **V:  Hoe werkt het terugschrijven van wachtwoorden op de achtergrond?**

  > **A:** Zie het artikel [hoe het terugschrijven van wachtwoorden werkt](howto-sspr-writeback.md) voor een uitleg over wat er gebeurt wanneer u het terugschrijven van wachtwoorden en hoe gegevens stromen via het systeem terug in uw on-premises-omgeving.
  >
  >
* **V:  Hoe lang het terugschrijven van wachtwoorden duurt om te werken? Is er een synchronisatie vertraging er is met de synchronisatie van wachtwoordhashes?**

  > **A:** Wachtwoord terugschrijven is direct. Het is een synchrone pijplijn die fundamenteel anders dan de wachtwoord-hashsynchronisatie werkt. Write-back van wachtwoord kan gebruikers op te halen van real-time feedback over het succes van hun wachtwoord opnieuw instellen of wijzigen van de bewerking. De gemiddelde tijd voor een geslaagde write-back van wachtwoord is dan 500 ms.
  >
  >
* **V:  Als mijn on-premises-account is uitgeschakeld, hoe worden mijn cloud-account en toegang worden beïnvloed?**

  > **A:** Als uw on-premises-ID is uitgeschakeld, kan uw cloud-ID en de toegang wordt ook op basis van de volgende synchronisatie-interval via Azure AD Connect worden uitgeschakeld. Standaard is deze synchronisatie om de 30 minuten.
  >
  >
* **V:  Als mijn on-premises-account wordt beperkt door een on-premises-beleid voor het wachtwoord van Active Directory, SSPR volgen dit beleid wanneer ik mijn wachtwoord wijzigen?**

  > **A:** Ja, SSPR afhankelijk en houdt zich aan het wachtwoordbeleid van on-premises Active Directory. Dit beleid bevat de typische wachtwoordbeleid van Active Directory-domein, evenals alle gedefinieerde, Fijnmazig wachtwoordbeleid die bedoeld zijn voor een gebruiker.
  >
  >
* **V:  Welke soorten accounts kiest, wordt het terugschrijven van wachtwoorden gebruiken?**

  > **A:** Wachtwoord terugschrijven werkt voor gebruikersaccounts die worden gesynchroniseerd vanuit Active Directory on-premises naar Azure AD, met inbegrip van federatieve, wachtwoord-hash gesynchroniseerde en Pass Through-Autentication-gebruikers.
  >
  >
* **V:  Dwingt het terugschrijven van wachtwoorden wachtwoordbeleid van mijn domein?**

  > **A:** Ja. Wachtwoord terugschrijven wordt afgedwongen wachtwoordduur, geschiedenis, complexiteit, filters en eventuele andere beperkingen die u in plaats van wachtwoorden in uw lokale domein mogelijk plaatsen.
  >
  >
* **V:  Is het terugschrijven van wachtwoorden beveiligd?  Hoe kan ik dat ik wordt niet ingebroken zijn?**

  > **A:** Ja, het terugschrijven van wachtwoorden is beveiligd. Bekijk meer informatie over de meerdere beveiligingslagen die door de service wachtwoord terugschrijven hebt geïmplementeerd, de [wachtwoord terugschrijven security](concept-sspr-writeback.md#password-writeback-security) sectie de [wachtwoord terugschrijven overzicht](howto-sspr-writeback.md) artikel.
  >
  >

## <a name="next-steps"></a>Volgende stappen

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
