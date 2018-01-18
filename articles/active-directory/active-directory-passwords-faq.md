---
title: Selfservice voor wachtwoordherstel FAQ - Azure Active Directory
description: Veelgestelde vragen over Azure AD zelf uw wachtwoord opnieuw instellen
services: active-directory
keywords: Wachtwoordbeheer Active directory, wachtwoordbeheer, Azure AD self service voor wachtwoordherstel
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: seohack1
ms.openlocfilehash: 29b25a476344ed048673039beacdffd39469ec85
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="password-management-frequently-asked-questions"></a>Veelgestelde vragen over wachtwoordbeheer

Hier volgen enkele veelgestelde vragen (FAQ) voor alle bewerkingen die betrekking hebben op wachtwoord opnieuw instellen.

Als er een algemene vraag over Azure Active Directory (Azure AD) en selfservice voor wachtwoordherstel (SSPR) die hier niet wordt beantwoord, kunt u vragen de community om hulp op de [Azure AD-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Leden van de community opnemen engineers, productmanagers, MVP's en andere IT-professionals.

Deze Veelgestelde vragen wordt opgedeeld in de volgende secties:

* [Registratie voor vragen over het wachtwoord opnieuw instellen](#password-reset-registration)
* [Vragen over wachtwoordherstel](#password-reset)
* [Vragen over het wijzigen van wachtwoorden](#password-change)
* [Vragen over de wachtwoord-rapporten](#password-management-reports)
* [Vragen over Write-back van wachtwoord](#password-writeback)

## <a name="password-reset-registration"></a>Registratie van het opnieuw ingestelde wachtwoord

* **V: kunnen mijn gebruikers registreren hun eigen wachtwoord opnieuw instellen van gegevens?**

  > **A:** Ja. Zolang wachtwoord opnieuw instellen is ingeschakeld en worden ze in licentie gegeven, gaat u naar de registratieportal voor wachtwoordherstel (http://aka.ms/ssprsetup) registreren hun verificatiegegevens gebruikers. Gebruikers kunnen ook registreren via het toegangsvenster (http://myapps.microsoft.com). Als u wilt registreren via het toegangsvenster, moeten ze hun profielfoto selecteert, selecteert u **profiel**, en selecteer vervolgens de **registreren voor wachtwoordherstel** optie.
  >
  >
* **V: als ik wachtwoord opnieuw instellen voor een groep en vervolgens besluit in te schakelen voor iedereen zijn mijn gebruikers vereiste opnieuw registreren?**

  > **A:** Nee. Gebruikers die verificatiegegevens hebt ingevuld, zijn niet vereist voor het opnieuw te registreren.
  >
  >
* **V: kan ik wachtwoord opnieuw instellen van gegevens definiëren namens mijn gebruikers?**

  > **A:** Ja, u kunt dit doen met Azure AD Connect, PowerShell, de [Azure-portal](https://portal.azure.com), of het Office 365-beheercentrum. Zie voor meer informatie [gegevens die worden gebruikt door Azure AD zelf uw wachtwoord opnieuw instellen](active-directory-passwords-data.md).
  >
  >
* **V: kan ik gegevens voor vragen over de beveiliging van on-premises synchroniseren?**

  > **A:** Nee, dit niet mogelijk is vandaag de dag.
  >
  >
* **V: kunnen mijn gebruikers gegevens registreren zodanig dat deze gegevens door andere gebruikers niet zien?**

  > **A:** Ja. Wanneer gebruikers zich registreren gegevens met behulp van het wachtwoord opnieuw instellen van registratieportal, worden de gegevens worden opgeslagen in persoonlijke verificatie-velden alleen zichtbaar voor globale beheerders en de gebruiker zijn.
  >
  >
* **V: Mijn gebruikers hoeft te worden geregistreerd voordat ze kunnen voor wachtwoordherstel gebruiken?**

  > **A:** Nee. Als u onvoldoende verificatiegegevens namens hen definieert, worden gebruikers niet hebben om te registreren. Wachtwoord opnieuw moet worden ingesteld als u hebt de gegevens die zijn opgeslagen in de juiste velden in de map juist opgemaakt.
  >
  >
* **V: kan ik synchroniseren of instellen van de telefoon voor authenticatie, authenticatie e-mail of alternatieve verificatie phone velden namens mijn gebruikers?**

  > **A:** Nee, dit niet mogelijk is vandaag de dag.
  >
  >
* **V: hoe de portal voor wachtwoordregistratie bepalen welke opties mijn gebruikers?**

  > **A:** het wachtwoord opnieuw ingesteld registratie portal bevat alleen de opties die u hebt ingeschakeld voor uw gebruikers. Deze opties zijn gevonden onder de **gebruiker opnieuw instellen wachtwoordbeleid** gedeelte van uw map **configureren** tabblad. Bijvoorbeeld, als u vragen over de beveiliging niet inschakelt, zich klikt u vervolgens gebruikers niet kunnen registreren voor die optie.
  >
  >
* **V: wanneer een gebruiker als beschouwd ingeschreven?**

  > **A:** een gebruiker wordt beschouwd geregistreerd voor SSPR, wanneer zij hebben geregistreerd ten minste de **aantal methoden die zijn vereist om in te stellen** een wachtwoord dat u hebt ingesteld in de [Azure-portal](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Wachtwoord opnieuw instellen

* **V: heb u te voorkomen dat gebruikers meerdere pogingen om een wachtwoord in een korte periode opnieuw instellen?**

  > **A:** Ja, er zijn ingebouwd in het wachtwoord opnieuw instellen als u wilt beveiligen tegen misbruik beveiligingsfuncties. 
  >
  > Gebruikers kunnen proberen slechts vijf wachtwoord herstelpogingen binnen een periode van 24 uur voordat ze zijn vergrendeld voor 24 uur. 
  >
  > Gebruikers kunnen proberen te valideren, een telefoonnummer, een SMS-bericht verzenden of beveiligingsvragen en antwoorden slechts vijf keer valideren binnen een uur voordat ze zijn vergrendeld voor 24 uur. 
  >
  > Gebruikers kunnen een e-mailbericht verzenden maximaal 10 keer binnen een periode van tien minuten voordat ze zijn vergrendeld voor 24 uur.
  >
  > De items worden opnieuw ingesteld zodra een gebruiker hun wachtwoord opnieuw instellen.
  >
  >
* **V: hoe lang moet ik wachten om te ontvangen van een e-mail, SMS of telefoongesprek van wachtwoord opnieuw instellen?**

  > **A:** e-mailberichten, SMS-berichten en telefoongesprekken moet binnenkomen minder dan een minuut. Het normale geval is 5-20 seconden.
    >Als u de melding niet binnen deze tijd ontvangt:
        > * Controleer de map Ongewenste e-mail.
        > * Controleer of het nummer of de e-mailadres waarmee contact wordt gemaakt die u verwacht.
        > * Controleren of de verificatiegegevens in de map juist is opgemaakt, bijvoorbeeld: + 1 4255551234 of  *user@contoso.com* . 
  >
  >
* **V: welke talen worden ondersteund door het wachtwoord opnieuw instellen?**

  > **A:** -gebruikersinterface voor het wachtwoord opnieuw instellen van SMS-berichten en telefoongesprekken in dezelfde talen worden ondersteund in Office 365 worden gelokaliseerd.
  >
  >
* **V: welke onderdelen van de ervaring van wachtwoord opnieuw instellen wanneer ik de organisatie-huisstijl items instelt in mijn directory ophalen huisstijl het tabblad configureren?**

  > **A:** de portal voor wachtwoord opnieuw instellen van uw organisatie logo toont en kunt u de koppeling 'Contact op met uw beheerder' om te verwijzen naar een aangepaste e-mailadres of de URL configureren. Een e-mailbericht dat wordt verzonden door wachtwoordherstel logo, kleuren en de naam van uw organisatie in de hoofdtekst van het e-mailbericht bevat en wordt aangepast via de instellingen voor die specifieke naam.
  >
  >
* **V: hoe kan ik mijn gebruikers opleiden waar u om hun wachtwoord opnieuw instellen?**

  > **A:** kunt proberen de suggesties in onze [SSPR implementatie](active-directory-passwords-best-practices.md#email-based-rollout) artikel.
  >
  >
* **V: kan ik deze pagina vanaf een mobiel apparaat gebruiken?**

  > **A:** Ja, deze pagina werkt op mobiele apparaten.
  >
  >
* **V: ondersteund ontgrendelen lokale Active Directory-accounts wanneer gebruikers hun wachtwoord opnieuw instellen?**

  > **A:** Ja. Wanneer een gebruiker het wachtwoord opnieuw instelt als wachtwoord terugschrijven is geïmplementeerd met Azure AD Connect, is het account van die gebruiker automatisch ontgrendeld, wanneer ze hun wachtwoord opnieuw instellen.
  >
  >
* **V: hoe kan ik wachtwoordherstel rechtstreeks in mijn gebruiker aanmelden Bureaubladervaring worden geïntegreerd?**

  > **A:** als u een Azure AD Premium-klant bent, kunt u Microsoft Identity Manager installeren zonder extra kosten en implementeren van de on-premises-oplossing voor wachtwoord opnieuw instellen.
  >
  >
* **V: kan ik andere beveiligingsvragen voor verschillende talen instellen?**

  > **A:** Nee, dit niet mogelijk is vandaag de dag.
  >
  >
* **V: hoe zoveel mogelijk vragen kan ik configureren voor de beveiligingsoptie voor de verificatie van vragen?**

  > **A:** kunt u maximaal 20 aangepaste beveiligingsvragen in de [Azure-portal](https://portal.azure.com).
  >
  >
* **V: hoe lang kunnen beveiligingsvragen zijn?**

  > **A:** beveiligingsvragen mag niet 3 tot 200 tekens.
  >
  >
* **V: hoe lang kunnen de antwoorden op beveiligingsvragen zijn?**

  > **A:** antwoorden mag niet 3 tot 40 tekens.
  >
  >
* **V: zijn dubbele antwoorden op beveiligingsvragen afgewezen?**

  > **A:** Ja, er dubbele antwoorden op beveiligingsvragen afwijzen.
  >
  >
* **V: kan een gebruiker de dezelfde beveiligingsvraag meer dan één keer registreren?**

  > **A:** Nee. Nadat een gebruiker zich registreert voor een bepaald criterium, kunnen deze niet registreren voor deze vraag een tweede keer.
  >
  >
* **V: is het mogelijk is de ondergrens van beveiligingsvragen voor registratie instellen en resetten?**

  > **A:** Ja, een limiet kan worden ingesteld voor registratie en een andere voor het opnieuw instellen. Vragen over de beveiliging van de drie tot vijf kunnen vereist voor inschrijving, en drie tot vijf vragen kunnen worden vereist voor het opnieuw instellen.
  >
  >
* **V: ik mijn beleid om te vereisen dat gebruikers beveiligingsvragen gebruiken voor het opnieuw instellen hebt geconfigureerd, maar de Azure-beheerders lijken anders worden geconfigureerd.**

  > **A:** dit is het verwachte gedrag. Microsoft zorgt er een sterk wachtwoord twee poort opnieuw instellen standaardbeleid voor de rol van een Azure-beheerder. Dit voorkomt dat beheerders beveiligingsvragen gebruiken. U vindt meer informatie over dit beleid in de [wachtwoordbeleid en -beperkingen in Azure Active Directory](active-directory-passwords-policy.md#administrator-password-policy-differences) artikel.
  >
  >
* **V: als een gebruiker meer dan het maximum aantal vragen dat vereist is om in te stellen is geregistreerd, hoe worden de beveiligingsvragen geselecteerd tijdens het opnieuw instellen?**

  > **A:** *N* aantal beveiligingsvragen zijn willekeurig geselecteerd buiten het totale aantal vragen dat een gebruiker is geregistreerd voor waar *N* is het bedrag dat is ingesteld voor de  **Aantal vragen dat vereist is om in te stellen** optie. Bijvoorbeeld, als een gebruiker vijf beveiligingsvragen is geregistreerd, maar slechts drie vereist zijn voor een wachtwoord opnieuw instellen, drie van de vijf vragen willekeurig geselecteerd en vindt u op opnieuw instellen. Om te voorkomen dat vraag hammering, als de gebruiker de antwoorden op de vragen ontvangt verkeerde het selectieproces begint.
  >
  >
* **V: hoe lang zijn de e-mail en SMS eenmalige wachtwoordcodes geldig?**

  > **A:** de levensduur van de sessie voor wachtwoordherstel is 15 minuten. De gebruiker heeft vanaf het begin van de bewerking van het wachtwoord opnieuw instellen, 15 minuten hun wachtwoord opnieuw instellen. De e-mail en SMS eenmalige wachtwoordcode zijn ongeldig nadat deze periode is verstreken.
  >
  >
* **V: kan ik verhinderen dat gebruikers hun wachtwoord opnieuw instellen?**

  > **A:** Ja, als u een groep gebruiken om in te schakelen SSPR, u kunt een afzonderlijke gebruiker verwijderen uit de groep waarmee gebruikers hun wachtwoord opnieuw instellen.
  >
  >

## <a name="password-change"></a>Wachtwoord wijzigen

* **V: waar moeten Mijn gebruikers hun wachtwoord moeten wijzigen gaan?**

  > **A:** kunnen gebruikers hun wachtwoorden wijzigen waar ze ziet hun profielfoto of pictogram, zoals in de rechterbovenhoek van hun [Office 365](https://portal.office.com) portal of [Toegangspaneel](https://myapps.microsoft.com) optreedt. Gebruikers kunnen hun wachtwoorden op wijzigen de [toegang Configuratiescherm profielpagina](https://account.activedirectory.windowsazure.com/r#/profile). Gebruikers kunnen ook worden gevraagd hun wachtwoord moeten wijzigen automatisch op de aanmeldingspagina van Azure AD als hun wachtwoord is verlopen. Ten slotte gebruikers kunnen bladeren naar de [Azure AD-portal voor wachtwoord wijzigen](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) rechtstreeks als ze willen hun wachtwoord wijzigen.
  >
  >
* **V: kunnen mijn gebruikers worden gewaarschuwd in de Office-portal wanneer hun on-premises wachtwoord is verlopen?**

  > **A:** Ja, dit is mogelijk vandaag als u Active Directory Federation Services (AD FS) gebruiken. Als u AD FS gebruikt, volg de instructies in de [wachtwoord beleid claims met AD FS te verzenden](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) artikel. Als u synchronisatie van wachtwoordhash, is dit niet mogelijk vandaag. We synchroniseren niet wachtwoordbeleid van on-premises adreslijsten, zodat het is niet mogelijk om te posten verlopen-meldingen naar cloud ervaringen. In beide gevallen is het ook mogelijk te [Waarschuw gebruikers waarvan de wachtwoorden worden verloopt via PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **V: kan ik verhinderen dat gebruikers hun wachtwoord wijzigen?**

  > **A:** voor gebruikers alleen in de cloud, wachtwoordwijzigingen kunnen niet worden geblokkeerd. U kunt instellen voor on-premises gebruikers de **gebruiker kan wachtwoord niet wijzigen** optie geselecteerd. De geselecteerde gebruikers wijzigen hun wachtwoord niet.
  >
  >

## <a name="password-management-reports"></a>Wachtwoord-rapporten

* **V: hoe lang duurt het voor de gegevens worden weergegeven op de wachtwoord-rapporten?**

  > **A:** gegevens op de wachtwoord-rapporten moet worden weergegeven in 5 tot 10 minuten. In sommige gevallen is het mogelijk een uur duren worden weergegeven.
  >
  >
* **V: hoe kan ik de wachtwoord-rapporten filteren?**

  > **A:** wilt de wachtwoord-rapporten filteren, selecteert u het kleine Vergrootglas uiterst rechts van de kolomlabels boven aan het rapport. Als u doen uitgebreidere filteren wilt, kunt u downloaden van het rapport naar Excel en een draaitabel maken.
  >
  >
* **V: Wat is het maximum aantal gebeurtenissen die zijn opgeslagen in de wachtwoord-rapporten?**

  > **A:** maximaal 75.000 wachtwoord opnieuw instellen of het wachtwoord opnieuw instellen van inschrijving gebeurtenissen worden opgeslagen in de wachtwoord-rapporten, spanning terug tot 30 dagen. We werken als u dit aantal zodanig dat meer gebeurtenissen wilt uitbreiden.
  >
  >
* **V: hoe ver terug de wachtwoordbeheer rapporten gaan doen?**

  > **A:** de wachtwoordbeheer rapporten weergeven bewerkingen die in de afgelopen 30 dagen is opgetreden. Op dit moment als u nodig hebt om deze gegevens te archiveren kunt u periodiek de rapporten downloaden en opslaan op een andere locatie.
  >
  >
* **V: is er een maximale aantal rijen dat kan worden weergegeven op de wachtwoord-rapporten?**

  > **A:** Ja. Een maximum van 75.000 rijen kan worden weergegeven op een van de wachtwoord-rapporten, of ze worden weergegeven in de gebruikersinterface of worden gedownload.
  >
  >
* **V: is er een API voor toegang tot het wachtwoord opnieuw instellen of de registratie rapportagegegevens?**

  > **A:** Ja. Zie voor meer informatie over hoe u toegang hebt tot wachtwoord opnieuw instellen van de gegevensstroom reporting, [informatie over wachtwoordherstel rapportages programmatisch toegang tot](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Wachtwoord terugschrijven

* **V: hoe werkt wachtwoord terugschrijven achter de schermen?**

  > **A:** Zie het artikel [de werking van wachtwoord terugschrijven](active-directory-passwords-writeback.md) voor meer informatie over wat er gebeurt als u terugschrijven van wachtwoord en hoe gegevens loopt door het systeem back in uw on-premises omgeving.
  >
  >
* **V: hoe lang duurt wachtwoord terugschrijven voordat werken? Is er een synchronisatie uit te stellen er met wachtwoordhashsynchronisatie is?**

  > **A:** wachtwoord terugschrijven is snel. Het is een synchrone pijplijn die fundamenteel anders dan de synchronisatie van wachtwoordhash werkt. Wachtwoord terugschrijven kan gebruikers realtime feedback over het succes van hun wachtwoord opnieuw instellen of wijzigen van de bewerking. De gemiddelde tijd voor een geslaagde write-back van een wachtwoord is onder 500 ms.
  >
  >
* **V: als mijn lokale account is uitgeschakeld, hoe wordt mijn account van de cloud en de toegang van invloed op een?**

  > **A:** als uw lokale ID is uitgeschakeld, uw cloud-ID en de toegang wordt ook uitgeschakeld op het volgende interval synchroniseren met Azure AD Connect. Deze synchronisatie wordt standaard elke 30 minuten.
  >
  >
* **V: als mijn lokale account wordt beperkt door een beleid on-premises Active Directory-wachtwoord, houdt SSPR zich aan dit beleid wanneer ik mijn wachtwoord wijzigen?**

  > **A:** Ja, SSPR is afhankelijk van en houdt zich aan het beleid on-premises Active Directory-wachtwoord. Dit beleid bevat de typische wachtwoordbeleid van Active Directory-domein, evenals alle gedefinieerde, Fijnmazig wachtwoordbeleid die bedoeld zijn voor een gebruiker.
  >
  >
* **V: wat typen accounts werkt wachtwoord terugschrijven voor?**

  > **A:** wachtwoord terugschrijven werkt voor federatieve en wachtwoord-hash gebruikers gesynchroniseerd.
  >
  >
* **V: bevat wachtwoord terugschrijven mijn domein wachtwoordbeleid afdwingen?**

  > **A:** Ja. Wachtwoord terugschrijven worden afgedwongen wachtwoord leeftijd, geschiedenis, complexiteit, filters en eventuele andere beperkingen die u in plaats van wachtwoorden in uw lokale domein mogelijk plaatsen.
  >
  >
* **V: wachtwoord terugschrijven veilig is?  Hoe kan ik dat ik won't ingebroken zijn?**

  > **A:** Ja, Write-back van wachtwoord is beveiligd. Bekijk meer informatie over de vier beveiligingslagen geïmplementeerd door de service van de Write-back van wachtwoord, de [wachtwoord terugschrijven beveiligingsmodel](active-directory-passwords-writeback.md#password-writeback-security-model) sectie de [wachtwoord terugschrijven overzicht](active-directory-passwords-writeback.md) artikel.
  >
  >

## <a name="next-steps"></a>Volgende stappen

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
