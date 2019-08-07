---
title: Veelgestelde vragen over Azure multi-factor Authentication-Azure Active Directory
description: Veelgestelde vragen en antwoorden met betrekking tot Azure multi-factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46abe367c9047616174a1e43dffd57861e6278e8
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68811826"
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Veelgestelde vragen over Azure multi-factor Authentication

Deze veelgestelde vragen antwoorden op veelgestelde vragen over Azure multi-factor Authentication en het gebruik van de multi-factor Authentication-service. Het is onderverdeeld in vragen over de service in het algemeen, facturerings modellen, gebruikers ervaringen en probleem oplossing.

## <a name="general"></a>Algemeen

**V: Hoe worden gebruikers gegevens verwerkt door de Azure multi-factor Authentication-Server?**

Met de multi-factor Authentication-Server worden gebruikers gegevens alleen opgeslagen op de on-premises servers. In de cloud worden geen permanente gebruikersgegevens opgeslagen. Wanneer de gebruiker verificatie in twee stappen uitvoert, verzendt multi-factor Authentication-Server gegevens naar de Azure multi-factor Authentication-Cloud service voor verificatie. De communicatie tussen de multi-factor Authentication-Server en de multi-factor Authentication-Cloud service maakt gebruik van Secure Sockets Layer (SSL) of Transport Layer Security (TLS) via poort 443 uitgaand.

Wanneer verificatie aanvragen worden verzonden naar de Cloud service, worden gegevens verzameld voor verificatie-en gebruiks rapporten. De volgende gegevens velden zijn opgenomen in verificatie Logboeken in twee stappen:

* **Unieke id** (gebruikers naam of on-premises multi-factor Authentication-Server-ID)
* **Voor-en achternaam** Beschrijving
* **E-mail adres** Beschrijving
* **Telefoon nummer** (bij gebruik van een spraak oproep of SMS-verificatie)
* **Token van apparaat** (als u de verificatie van mobiele apps gebruikt)
* **Verificatie modus**
* **Verificatie resultaat**
* **Naam van de multi-factor Authentication-Server**
* **Multi-factor Authentication-Server-IP**
* **Client-IP** (indien beschikbaar)

De optionele velden kunnen worden geconfigureerd in de multi-factor Authentication-Server.

Het verificatie resultaat (geslaagd of geweigerd) en de reden als het is geweigerd, wordt opgeslagen met de verificatie gegevens. Deze gegevens zijn beschikbaar in verificatie-en gebruiks rapporten.

**V: Welke SMS-korte codes worden gebruikt voor het verzenden van SMS-berichten naar mijn gebruikers?**

In de Verenigde Staten gebruikt micro soft de volgende korte SMS-codes:

   * 97671
   * 69829
   * 51789
   * 99399

In Canada gebruikt micro soft de volgende korte SMS-codes:

   * 759731 
   * 673801

Micro soft biedt geen garantie voor het leveren van consistente en op spraak gebaseerde multi-factor Authentication-prompt met hetzelfde nummer. In het belang van onze gebruikers mag micro soft op elk gewenst moment korte codes toevoegen of verwijderen, aangezien we route aanpassingen aanbrengen om de levering van SMS te verbeteren. Micro soft biedt geen ondersteuning voor korte codes voor landen/regio's, naast de Verenigde Staten en Canada.

## <a name="billing"></a>Billing

De meeste facturerings vragen kunnen worden beantwoord door te verwijzen naar de [pagina met prijzen voor multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) of de documentatie over het [ophalen van Azure multi-factor Authentication](concept-mfa-licensing.md).

**V: Zijn mijn organisatie in rekening gebracht voor het verzenden van telefoon gesprekken en SMS-berichten die worden gebruikt voor verificatie?**

Nee, er worden geen kosten in rekening gebracht voor afzonderlijke telefoon gesprekken of SMS-berichten die naar gebruikers worden verzonden via Azure multi-factor Authentication. Als u een MFA-provider per verificatie gebruikt, worden er kosten in rekening gebracht voor elke authenticatie, maar niet voor de gebruikte methode.

Uw gebruikers worden mogelijk in rekening gebracht voor de telefoon gesprekken of SMS-berichten die ze ontvangen, op basis van hun persoonlijke telefoon service.

**V: Factureert het facturerings model per gebruiker voor alle ingeschakelde gebruikers, of alleen voor de gebruikers die een verificatie in twee stappen hebben uitgevoerd?**

De facturering is gebaseerd op het aantal gebruikers dat is geconfigureerd voor het gebruik van multi-factor Authentication, ongeacht of er een maand verificatie met twee stappen is uitgevoerd.

**V: Hoe werkt de facturering voor multi-factor Authentication?**

Wanneer u een MFA-provider per gebruiker of per verificatie maakt, wordt het Azure-abonnement van uw organisatie maandelijks gefactureerd op basis van het gebruik. Dit facturerings model is vergelijkbaar met hoe Azure factureert voor gebruik van virtuele machines en websites.

Wanneer u een abonnement voor Azure multi-factor Authentication aanschaft, betaalt uw organisatie alleen de jaarlijkse licentie kosten voor elke gebruiker. MFA-licenties en Office 365-, Azure AD Premium-of Enterprise Mobility + Security bundels worden op deze manier gefactureerd. 

Meer informatie over uw opties in [Azure multi-factor Authentication ophalen](concept-mfa-licensing.md).

**V: Is er een gratis versie van Azure multi-factor Authentication?**

In sommige gevallen Ja.

Multi-factor Authentication voor Azure-beheerders biedt gratis een subset van Azure MFA-functies voor toegang tot micro soft onlineservices, met inbegrip van de [Azure Portal](https://portal.azure.com) en [Microsoft 365 beheer centrum](https://admin.microsoft.com). Deze aanbieding is alleen van toepassing op globale beheerders in Azure Active Directory exemplaren die niet beschikken over de volledige versie van Azure MFA via een MFA-licentie, een bundel of een zelfstandige provider op basis van verbruik. Als uw beheerders de gratis versie gebruiken en u vervolgens een volledige versie van Azure MFA aanschaft, worden alle globale beheerders automatisch uitgebreid naar de betaalde versie.

Multi-factor Authentication voor Office 365-gebruikers biedt een subset van Azure MFA-functies zonder kosten voor toegang tot Office 365-Services, waaronder Exchange Online en share point online. Deze aanbieding is van toepassing op gebruikers aan wie een Office 365-licentie is toegewezen, wanneer het bijbehorende exemplaar van Azure Active Directory niet de volledige versie van Azure MFA heeft via een MFA-licentie, een bundel of een zelfstandige provider op basis van verbruik.

**V: Kan mijn organisatie op elk gewenst moment scha kelen tussen een facturerings model per gebruiker en per verificatie?**

Als uw organisatie MFA aanschaft als zelfstandige service met facturering op basis van verbruik, kiest u een facturerings model wanneer u een MFA-provider maakt. U kunt het facturerings model niet wijzigen nadat een MFA-provider is gemaakt. 

Als uw MFA-provider *niet* is gekoppeld aan een Azure AD-Tenant of als u de nieuwe MFA-provider koppelt aan een andere Azure AD-Tenant, worden de gebruikers instellingen en configuratie opties niet overgedragen. Bestaande Azure MFA-servers moeten ook opnieuw worden geactiveerd met behulp van de activeringsreferenties die worden gegenereerd door de nieuwe MFA-provider. Als u de MFA-servers opnieuw activeert om ze te koppelen aan de nieuwe MFA-provider, is dit niet van invloed op telefonische verificatie en verificatie via een sms-bericht. Mobiele app-meldingen werken echter niet meer voor gebruikers totdat ze de mobiele app opnieuw activeren.

Meer informatie over MFA-providers vindt u in [aan de slag met een Azure multi-factor Authentication-provider](concept-mfa-authprovider.md).

**V: Kan mijn organisatie op elk gewenst moment scha kelen tussen op verbruik gebaseerde facturering en abonnementen (een model op basis van een licentie)?**

In sommige gevallen Ja.

Als uw Directory een Azure multi-factor Authentication-provider *per gebruiker* heeft, kunt u MFA-licenties toevoegen. Gebruikers met licenties worden niet meegeteld in de facturering op basis van verbruik per gebruiker. Gebruikers zonder licenties kunnen nog steeds worden ingeschakeld voor MFA via de MFA-provider. Als u licenties aanschaft en toewijst voor alle gebruikers die zijn geconfigureerd voor het gebruik van multi-factor Authentication, kunt u de Azure multi-factor Authentication-provider verwijderen. U kunt altijd een andere MFA-provider per gebruiker maken als u in de toekomst meer gebruikers dan licenties hebt.

Als uw Directory een Azure multi-factor Authentication-provider *per verificatie* heeft, wordt u altijd gefactureerd voor elke verificatie, zolang de MFA-provider is gekoppeld aan uw abonnement. U kunt MFA-licenties toewijzen aan gebruikers, maar u wordt nog steeds gefactureerd voor elke verificatie aanvraag in twee stappen, ongeacht of deze afkomstig is van iemand met een MFA-licentie die is toegewezen of niet.

**V: Moet mijn organisatie identiteiten gebruiken en synchroniseren voor het gebruik van Azure multi-factor Authentication?**

Als uw organisatie gebruikmaakt van een facturerings model op basis van verbruik, is Azure Active Directory optioneel, maar niet vereist. Als uw MFA-provider niet is gekoppeld aan een Azure AD-Tenant, kunt u de Azure multi-factor Authentication-server alleen on-premises implementeren.

Azure Active Directory is vereist voor het licentie model, omdat licenties worden toegevoegd aan de Azure AD-Tenant wanneer u deze aanschaft en toewijst aan gebruikers in de Directory.

## <a name="manage-and-support-user-accounts"></a>Gebruikers accounts beheren en ondersteunen

**V: Wat moet ik vertellen dat mijn gebruikers doen als ze geen antwoord op hun telefoon ontvangen?**

Laat uw gebruikers vijf keer over vijf minuten proberen om een telefoon gesprek of SMS voor verificatie te ontvangen. Micro soft gebruikt meerdere providers voor het leveren van aanroepen en SMS-berichten. Als dit niet werkt, opent u een ondersteunings aanvraag van micro soft om verder te gaan met het oplossen van problemen.

Als de bovenstaande stappen niet werken, is het niet mogelijk dat al uw gebruikers meer dan één verificatie methode hebben geconfigureerd. Vraag hen om te proberen zich opnieuw aan te melden, maar een andere verificatiemethode op de aanmeldpagina te selecteren.

U kunt uw gebruikers laten wijzen naar de [gids voor probleem oplossing voor eind gebruikers](../user-help/multi-factor-authentication-end-user-troubleshoot.md).

**V: Wat moet ik doen als een van mijn gebruikers geen toegang kan krijgen tot hun account?**

U kunt het account van de gebruiker opnieuw instellen door het registratie proces opnieuw door te voeren. Meer informatie over [het beheren van gebruikers-en apparaatinstellingen met Azure multi-factor Authentication in de Cloud](howto-mfa-userdevicesettings.md).

**V: Wat moet ik doen als een van mijn gebruikers een telefoon kwijtraakt die app-wacht woorden gebruikt?**

Verwijder alle app-wacht woorden van de gebruiker om onbevoegde toegang te voor komen. Nadat de gebruiker een vervangend apparaat heeft, kunnen de wacht woorden opnieuw worden gemaakt. Meer informatie over [het beheren van gebruikers-en apparaatinstellingen met Azure multi-factor Authentication in de Cloud](howto-mfa-userdevicesettings.md).

**V: Wat gebeurt er als een gebruiker zich niet kan aanmelden bij niet-browser-apps?**

Als uw organisatie nog steeds verouderde clients gebruikt en u [het gebruik van app-wacht woorden hebt toegestaan](howto-mfa-mfasettings.md#app-passwords), kunnen uw gebruikers zich niet aanmelden bij deze verouderde clients met hun gebruikers naam en wacht woord. In plaats daarvan moeten ze [app-wacht woorden instellen](../user-help/multi-factor-authentication-end-user-app-passwords.md). Uw gebruikers moeten hun aanmeldings gegevens wissen (verwijderen), de app opnieuw starten en u vervolgens aanmelden met de gebruikers naam en het *app-wacht woord* in plaats van het normale wacht woord.

Als uw organisatie geen verouderde clients heeft, mag u uw gebruikers niet toestaan om app-wacht woorden te maken.

> [!NOTE]
> Moderne authenticatie voor Office 2013-clients
>
> App-wacht woorden zijn alleen nodig voor apps die moderne authenticatie niet ondersteunen. Office 2013-clients ondersteunen moderne verificatie protocollen, maar moeten worden geconfigureerd. Moderne verificatie is nu beschikbaar voor elke klant die de update van 2015 maart of hoger voor Office 2013 uitvoert. Zie het blog bericht [Updated Office 365 moderne authenticatie](https://www.microsoft.com/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview/)voor meer informatie.

**V: Mijn gebruikers zeggen dat ze het tekst bericht soms niet ontvangen of dat ze reageren op tekst berichten in twee richtingen, maar dat er een time-out optreedt voor de verificatie.**

Het leveren van SMS-berichten en de ontvangst van antwoorden in twee richtings SM'S worden niet gegarandeerd omdat er niet-instel bare factoren zijn die van invloed kunnen zijn op de betrouw baarheid van de service. Deze factoren zijn onder andere het doel land/de regio, de mobiele telefoon provider en de signaal sterkte.

Als uw gebruikers vaak problemen ondervinden met het betrouw bare ontvangen van SMS-berichten, kunt u ze vertellen dat ze de mobiele app of de telefoon gesprek methode moeten gebruiken. De mobiele app kan meldingen ontvangen via mobiele en Wi-Fi-verbindingen. Daarnaast kan de mobiele app verificatie codes genereren, zelfs wanneer het apparaat helemaal geen signaal heeft. De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?Linkid=825072), [IOS](https://go.microsoft.com/fwlink/?Linkid=825073)en [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071).

Als u tekst berichten moet gebruiken, raden we u aan om in een eenrichtings-SMS te gebruiken in plaats van twee richtings SM'S wanneer dat mogelijk is. Eenrichtings-SMS is betrouwbaarder en Hiermee wordt voor komen dat gebruikers algemene SMS-kosten in rekening worden gebracht bij het beantwoorden van een SMS-bericht dat is verzonden vanuit een ander land/regio.

**V: Kan ik de hoeveelheid tijd wijzigen die gebruikers nodig hebben om de verificatie code van een SMS-bericht in te voeren voordat het systeem time-out heeft?**

In sommige gevallen moet u Ja. 

Voor eenrichtings-SMS met Azure MFA server v 7.0 of hoger kunt u de time-outinstelling configureren door een register sleutel in te stellen. Nadat de MFA-Cloud service het tekst bericht heeft verzonden, wordt de verificatie code (of een eenmalige wachtwoord code) naar de MFA-server geretourneerd. De MFA-server slaat de code standaard 300 seconden op in het geheugen. Als de gebruiker de code niet invoert voordat de 300 seconden zijn verstreken, wordt de verificatie geweigerd. Gebruik de volgende stappen om de standaard time-outinstelling te wijzigen:

1. Ga naar HKLM\Software\Wow6432Node\Positive Networks\PhoneFactor.
2. Maak een DWORD-register sleutel met de naam **pfsvc_pendingSmsTimeoutSeconds** en stel de tijd in seconden in die u wilt dat de Azure MFA-server eenmalige wachtwoord codes opslaat.

>[!TIP] 
>Als u meerdere MFA-servers hebt, kent alleen het account dat de oorspronkelijke verificatie aanvraag heeft verwerkt, de verificatie code die naar de gebruiker is verzonden. Wanneer de gebruiker de code invoert, moet de verificatie aanvraag om deze te valideren naar dezelfde server worden verzonden. Als de code validatie naar een andere server wordt verzonden, wordt de verificatie geweigerd. 

Voor twee richtings-SMS-servers met Azure MFA-server kunt u de time-outinstelling configureren in de MFA-Beheerportal. Als gebruikers niet binnen de gedefinieerde time-outperiode reageren op het SMS, wordt de verificatie geweigerd. 

U kunt de time-outinstelling niet configureren voor SMS-eenrichtings verificatie met Azure MFA in de Cloud (met inbegrip van de AD FS adapter of de Network Policy Server extensie). Azure AD slaat de verificatie code voor 180 seconden op. 

**V: Kan ik hardware-tokens gebruiken met de Azure multi-factor Authentication-Server?**

Als u Azure multi-factor Authentication-Server gebruikt, kunt u open-time-tokens (op basis van OATH), eenmalige wacht woorden (mobiele TOTP) van derden importeren en deze vervolgens gebruiken voor verificatie in twee stappen.

U kunt ActiveIdentity-tokens gebruiken die OATH mobiele TOTP-tokens zijn als u de geheime sleutel in een CSV-bestand plaatst en op de Azure multi-factor Authentication-server importeert. U kunt OATH-tokens gebruiken met Active Directory Federation Services (ADFS), op formulieren gebaseerde verificatie van Internet Information Server (IIS) en externe verificatie Dial-in User Service (RADIUS) zolang het client systeem de gebruikers invoer kan accepteren.

U kunt OATH mobiele TOTP-tokens van derden importeren met de volgende indelingen:  

- Portable symmetrische-sleutel container (PSKC)  
- CSV als het bestand een serie nummer, een geheime sleutel in de basis-32-indeling en een tijds interval bevat  

**V: Kan ik de Azure multi-factor Authentication-Server gebruiken voor het beveiligen van Terminal Services?**

Ja, maar als u Windows Server 2012 R2 of hoger gebruikt, kunt u alleen Terminal Services beveiligen met behulp van Extern bureaublad-gateway (RD-gateway).

Wijzigingen in de beveiliging in Windows Server 2012 R2 gewijzigd hoe Azure multi-factor Authentication-Server verbinding maakt met het beveiligings pakket voor de lokale beveiligings autoriteit (LSA) in Windows Server 2012 en eerdere versies. Voor versies van Terminal Services in Windows Server 2012 of eerder kunt u [een toepassing beveiligen met Windows-verificatie](howto-mfaserver-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Als u Windows Server 2012 R2 gebruikt, hebt u RD-gateway nodig.

**V: Ik heb een beller-ID geconfigureerd in MFA-server, maar mijn gebruikers ontvangen nog steeds multi-factor Authentication-oproepen van een anonieme aanroeper.**

Wanneer multi-factor Authentication-aanroepen via het open bare telefoonnet werk worden geplaatst, worden ze soms gerouteerd via een transporteur die geen beller-ID ondersteunt. Als gevolg hiervan is de beller-ID niet gegarandeerd, zelfs al wordt deze door het multi-factor Authentication-systeem altijd verzonden.

**V: Waarom worden mijn gebruikers gevraagd om hun beveiligings gegevens te registreren?**
Er zijn verschillende redenen waarom gebruikers kunnen worden gevraagd om hun beveiligings gegevens te registreren:

- De gebruiker is ingeschakeld voor MFA door hun beheerder in azure AD, maar er zijn nog geen beveiligings gegevens voor hun account geregistreerd.
- De gebruiker is ingeschakeld voor Self-service voor wachtwoord herstel in azure AD. De beveiligings informatie helpt ze hun wacht woord in de toekomst opnieuw in te stellen als ze het ooit verg eten.
- De gebruiker heeft een toepassing geopend die een beleid voor voorwaardelijke toegang heeft om MFA te vereisen en nog niet eerder is geregistreerd voor MFA.
- De gebruiker registreert een apparaat met Azure AD (met inbegrip van Azure AD-deelname) en voor uw organisatie is MFA vereist voor apparaatregistratie, maar de gebruiker is niet eerder geregistreerd voor MFA.
- De gebruiker genereert Windows hello voor bedrijven in Windows 10 (waarvoor MFA is vereist) en is niet eerder geregistreerd voor MFA.
- De organisatie heeft een MFA-registratie beleid gemaakt en ingeschakeld dat is toegepast op de gebruiker.
- De gebruiker is eerder geregistreerd voor MFA, maar er is een verificatie methode gekozen die een beheerder heeft uitgeschakeld. De gebruiker moet daarom MFA-registratie opnieuw door lopen om een nieuwe standaard verificatie methode te selecteren.

## <a name="errors"></a>Fouten

**V: Wat moeten gebruikers doen als het fout bericht ' verificatie aanvraag is niet voor een geactiveerd account ' wordt weer gegeven bij het gebruik van meldingen voor mobiele apps?**

Laat ze deze procedure volgen om hun account uit de mobiele app te verwijderen en voeg het vervolgens opnieuw toe:

1. Ga naar [uw Azure Portal-profiel](https://account.activedirectory.windowsazure.com/profile/) en meld u aan met uw organisatie account.
2. Selecteer **aanvullende beveiligings verificatie**.
3. Verwijder het bestaande account uit de mobiele app.
4. Klik op **configureren**en volg de instructies voor het opnieuw configureren van de mobiele app.

**V: Wat moeten gebruikers doen als ze een 0x800434D4L-fout bericht zien wanneer ze zich aanmelden bij een niet-browser toepassing?**

De 0x800434D4L-fout treedt op wanneer u zich probeert aan te melden bij een niet-browser toepassing, die is geïnstalleerd op een lokale computer en niet werkt met accounts waarvoor verificatie in twee stappen is vereist.

Een tijdelijke oplossing voor deze fout is het gebruik van afzonderlijke gebruikers accounts voor bewerkingen die betrekking hebben op beheerders en niet voor beheerders. Later kunt u post vakken koppelen tussen uw beheerders account en een niet-beheerders account, zodat u zich kunt aanmelden bij Outlook met behulp van uw niet-beheerders account. Voor meer informatie over deze oplossing leert u hoe u [een beheerder de mogelijkheid biedt om de inhoud van het postvak van een gebruiker te openen en weer te geven](https://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Volgende stappen

Als uw vraag hier niet wordt beantwoord, kunt u deze in de opmerkingen onder aan de pagina laten staan. U kunt ook de volgende aanvullende opties gebruiken om hulp te krijgen:

* Zoek in de [Microsoft ondersteuning Knowledge Base](https://www.microsoft.com/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) naar oplossingen voor veelvoorkomende technische problemen.
* Zoek en blader door technische vragen en antwoorden van de Community of vraag uw eigen vraag in de [Azure Active Directory forums](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Als u een verouderde Phone factor-klant bent en u vragen hebt of hulp nodig hebt bij het opnieuw instellen van een wacht woord, gebruikt u de koppeling [wacht woord opnieuw instellen](mailto:phonefactorsupport@microsoft.com) om een ondersteunings aanvraag te openen.
* Neem contact op met een ondersteunings medewerker via [ondersteuning voor Azure multi-factor Authentication-Server (Phone factor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Wanneer u contact met ons opneemt, is het handig als u zoveel mogelijk informatie over uw probleem kunt opnemen. Informatie die u kunt opgeven, omvat de pagina waar u de fout hebt gezien, de specifieke fout code, de specifieke sessie-ID en de ID van de gebruiker die de fout heeft gezien.
