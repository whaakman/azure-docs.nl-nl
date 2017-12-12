---
title: Azure multi-factor Authentication Veelgestelde vragen | Microsoft Docs
description: "Veelgestelde vragen en antwoorden die betrekking hebben op de Azure multi-factor Authentication. Multi-factor Authentication is een methode voor het verifiëren van de identiteit van een gebruiker die meer dan een gebruikersnaam en wachtwoord vereist. Dit biedt een extra beveiligingslaag voor de gebruiker zich aanmelden en transacties."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: joflore
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4532cbab7f727e71908d5a2da43a11351f7aeeb3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Veelgestelde vragen over Azure multi-factor Authentication
Deze Veelgestelde vragen over de antwoorden op veelgestelde vragen over Azure multi-factor Authentication en het gebruik van de multi-factor Authentication-service. Het onderverdeeld in vragen over de service in het algemeen modellen, gebruikerservaringen, facturering en het oplossen van problemen.

## <a name="general"></a>Algemeen
**V: hoe wordt Azure multi-factor Authentication-Server gebruikersgegevens verwerkt?**

Gebruikersgegevens zijn opgeslagen met multi-factor Authentication-Server alleen op de on-premises servers. In de cloud worden geen permanente gebruikersgegevens opgeslagen. Wanneer de gebruiker verificatie in twee stappen uitvoert, verzendt multi-Factor Authentication-Server gegevens naar de cloudservice van Azure multi-factor Authentication voor verificatie. Communicatie tussen de multi-factor Authentication-Server en de multi-factor Authentication-cloudservice maakt gebruik van Secure Sockets Layer (SSL) of Transport Layer Security (TLS) via poort 443 uitgaand.

Wanneer verificatieaanvragen worden verzonden naar de cloudservice worden gegevens verzameld voor de verificatie- en gebruiksgegevens rapporteert. Gegevensvelden die zijn opgenomen in de verificatie-Logboeken in twee stappen zijn als volgt:

* **Unieke ID** (beide gebruikers naam of on-premises multi-Factor Authentication Server-ID)
* **Eerste en laatste naam** (optioneel)
* **E-mailadres** (optioneel)
* **Telefoonnummer** (als u een telefoongesprek of SMS-verificatie)
* **Apparaattoken** (als u verificatie voor mobiele Apps)
* **Verificatiemodus**
* **Verificatieresultaat**
* **Naam van de multi-factor Authentication-Server**
* **Multi-factor Authentication-Server IP**
* **Client IP** (indien beschikbaar)

De optionele velden kunnen worden geconfigureerd in de multi-factor Authentication-Server.

Het resultaat van de verificatie (slagen of mislukken) en de reden als deze is geweigerd, wordt opgeslagen met de verificatiegegevens. Deze gegevens is beschikbaar in de verificatie- en gebruiksrapporten.

**V: wat korte SMS-codes worden gebruikt voor het verzenden van SMS-berichten aan Mijn gebruikers?**

In de Verenigde Staten Microsoft maakt gebruik van de volgende SMS korte codes:

   * 97671
   * 69829
   * 51789
   * 99399

Maakt gebruik van de volgende korte SMS-codes in Canada Microsoft:

   * 759731 
   * 673801

Microsoft garandeert geen consistente, SMS of multi-Factor-verificatie op basis van een stem vragen levering met hetzelfde getal. Met het oog op onze gebruikers kan Microsoft toevoegen of verwijderen van korte codes op elk gewenst moment terwijl we de route-aanpassingen voor het verbeteren van de SMS-deliverability aanbrengt. Microsoft biedt geen ondersteuning voor korte codes voor landen naast de Verenigde Staten en Canada

## <a name="billing"></a>Facturering
De meeste vragen over facturering kunnen worden beantwoord door te verwijzen naar een de [pagina met prijzen van multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) of de documentatie over [het ophalen van Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md).

**V: is mijn organisatie kosten in rekening gebracht voor de verzending van telefoongesprekken en SMS-berichten die worden gebruikt voor verificatie?**

Nee, er zijn niet in rekening gebracht voor afzonderlijke telefoongesprekken geplaatst of tekst verzonden berichten aan gebruikers via de Azure multi-factor Authentication. Als u een MFA-provider per authenticatie gebruikt, kunt u voor elke verificatie, maar niet voor de methode waarmee wordt gefactureerd.

Uw gebruikers mogelijk worden kosten in rekening gebracht voor de telefoongesprekken of SMS-berichten volgens hun persoonlijke telefoonservice ontvangen.

**V: bevat het facturering model per gebruiker in rekening voor alle ingeschakelde gebruikers of alleen de resources die verificatie in twee stappen uitgevoerd?**

Facturering is gebaseerd op het aantal gebruikers dat is geconfigureerd voor het gebruik van multi-factor Authentication, ongeacht of ze verificatie in twee stappen die maand uitgevoerd.

**V: hoe werkt multi-factor Authentication facturering?**

Wanneer u een MFA-provider per gebruiker of per authenticatie maakt, wordt de Azure-abonnement van uw organisatie gefactureerd maandelijks op basis van gebruik. Deze factureringsmodel is vergelijkbaar met hoe Azure facturen voor informatie over het gebruik van virtuele machines en websites.

Wanneer u een abonnement voor Azure multi-factor Authentication koopt, betaalt uw organisatie alleen de jaarlijkse licentiekosten voor elke gebruiker. MFA licenties en Office 365, Azure AD Premium of Enterprise Mobility + Security bundels wordt gefactureerd op deze manier. 

Meer informatie over de opties in [het ophalen van Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md).

**V: is er een gratis versie van Azure multi-factor Authentication?**

In sommige gevallen Ja.

Multi-factor Authentication voor beheerders van Azure biedt een subset van Azure MFA-functies kosteloos voor toegang tot Microsoft online services, waaronder de Azure en Office 365-beheerder portals. Deze aanbieding is alleen van toepassing op globale beheerders in Azure Active Directory-exemplaren die u niet de volledige versie van Azure MFA via een MFA-licentie, een bundel of een zelfstandige op basis van verbruik-provider hebt. Als uw beheerders de gratis versie gebruiken, en vervolgens het aanschaffen van een volledige versie van Azure MFA, worden klikt u vervolgens alle globale beheerders uitgebreid naar de betaalde versie automatisch.

Multi-factor Authentication voor Office 365-gebruikers biedt een subset van Azure MFA-functies kosteloos voor toegang tot Office 365-services, met inbegrip van Exchange Online en SharePoint Online. Deze aanbieding geldt voor gebruikers die beschikken over een Office 365-licentie is toegewezen, wanneer de volledige versie van Azure MFA via een MFA-licentie, een bundel of een zelfstandige op basis van verbruik provider heeft geen door de overeenkomende instantie van Azure Active Directory.

**V: kan mijn organisatie tussen per gebruiker en per authenticatie verbruik factureringsmodellen op elk gewenst moment schakelen?**

Als uw organisatie MFA als zelfstandige service met facturering op basis van verbruik koopt, kiest u een factureringsmodel selecteren bij het maken van een MFA-provider. U kunt het facturering model niet wijzigen nadat een MFA-provider is gemaakt. U kunt echter de MFA-provider verwijderen en maak vervolgens een met een ander facturering model.

Wanneer een MFA-provider is gemaakt, kan deze worden gekoppeld aan een Azure Active Directory (ook wel ' Azure AD-tenant'). Als de huidige MFA-Provider is gekoppeld aan een Azure AD-tenant, kunt u veilig verwijderen van de MFA-provider en een die is gekoppeld aan dezelfde Azure AD-tenant maken. Als u voldoende licenties voor MFA, Azure AD Premium of EMS (Enterprise Mobility + Security) hebt voor alle gebruikers voor wie MFA is ingeschakeld, kunt u de MFA-provider ook helemaal verwijderen.

Als uw MFA-provider *niet* gekoppeld aan een Azure AD-tenant of u de nieuwe MFA-provider een koppeling naar een andere Azure AD-tenant, instellingen en configuratie-opties niet worden overgedragen. Bestaande Azure MFA-servers moeten ook opnieuw worden geactiveerd met behulp van de activeringsreferenties die worden gegenereerd door de nieuwe MFA-provider. Als u de MFA-servers opnieuw activeert om ze te koppelen aan de nieuwe MFA-provider, is dit niet van invloed op telefonische verificatie en verificatie via een sms-bericht. Mobiele app-meldingen werken echter niet meer voor gebruikers totdat ze de mobiele app opnieuw activeren.

Meer informatie over MFA-providers in [aan de slag met een Azure multi-factor Authentication-Provider](multi-factor-authentication-get-started-auth-provider.md).

**V: kan mijn organisatie schakelen tussen op basis van verbruik facturering en abonnementen (een licentie-model) op elk gewenst moment?**

In sommige gevallen Ja.

Als uw directory een *per gebruiker* Azure multi-factor Authentication-provider, kunt u MFA licenties toevoegen. Gebruikers met licenties worden niet in de facturering per gebruiker op basis van verbruik worden geteld. Gebruikers zonder licentie kunnen nog steeds worden ingeschakeld voor MFA via de MFA-provider. Als u koopt en licenties voor uw gebruikers die zijn geconfigureerd toewijzen voor het gebruik van multi-factor Authentication, kunt u de Azure multi-factor Authentication-provider verwijderen. Als u in de toekomst meer gebruikers dan licenties hebt, kunt u altijd een andere per gebruiker MFA-provider maken.

Als uw directory een *per authenticatie* Azure multi-factor Authentication-provider, elke verificatie worden altijd gefactureerd, zolang de MFA-provider is gekoppeld aan uw abonnement. U kunt MFA licenties toewijzen aan gebruikers, maar u hebt nog steeds worden gefactureerd voor elke aanvraag van de verificatie in twee stappen of ze afkomstig zijn van iemand met een MFA-licentie toegewezen of niet.

**V: Mijn organisatie heeft om te gebruiken en synchroniseren van identiteiten voor het gebruik van Azure multi-factor Authentication?**

Als uw organisatie gebruikmaakt van een factureringsmodel op basis van verbruik, wordt Azure Active Directory is optioneel, maar niet vereist. Als de MFA-provider is niet gekoppeld aan een Azure AD-tenant, kunt u alleen de Azure multi-factor Authentication-Server on-premises implementeren.

Azure Active Directory is vereist voor het licentiemodel licenties zijn toegevoegd aan de Azure AD-tenant wanneer u koopt en deze aan gebruikers in de directory toewijzen.

## <a name="manage-and-support-user-accounts"></a>Beheer en de ondersteuning van gebruikersaccounts

**V: wat moet ik mijn gebruikers te doen als ze niet reageert op hun telefoon hebt ontvangen of geen hun telefoon met hen hebt?**

Alle gebruikers geconfigureerd ideale geval meer dan één verificatiemethode. Vraag hen om te proberen zich opnieuw aan te melden, maar een andere verificatiemethode op de aanmeldpagina te selecteren.

U kunt uw gebruikers om te verwijzen de [eindgebruiker probleemoplossingsgids](./end-user/multi-factor-authentication-end-user-troubleshoot.md).


**V: wat moet ik doen als een van mijn gebruikers geen toegang hebben tot hun account?**

U kunt de account van de gebruiker opnieuw instellen door deze opnieuw te gaan via het registratieproces te maken. Meer informatie over [beheren van instellingen voor gebruikers en apparaten met Azure multi-factor Authentication in de cloud](multi-factor-authentication-manage-users-and-devices.md).

**V: wat moet ik doen als een van mijn gebruikers verliest een telefoon die met behulp van app-wachtwoorden?**

Om te voorkomen dat onbevoegde toegang, verwijderen van de gebruiker app-wachtwoorden. Nadat de gebruiker een vervangende-apparaat heeft, kunnen ze de wachtwoorden opnieuw maken. Meer informatie over [beheren van instellingen voor gebruikers en apparaten met Azure multi-factor Authentication in de cloud](multi-factor-authentication-manage-users-and-devices.md).

**V: Wat gebeurt er als een gebruiker kan niet melden bij niet-browsertoepassingen?**

Als uw organisatie nog steeds oude clients, en u gebruikt [toegestaan van het gebruik van app-wachtwoorden](multi-factor-authentication-whats-next.md#app-passwords), en vervolgens uw gebruikers kunnen niet bij deze oudere clients met hun gebruikersnaam en wachtwoord aanmelden. In plaats daarvan moeten ze [app-wachtwoorden instellen](./end-user/multi-factor-authentication-end-user-app-passwords.md). Uw gebruikers moeten uitschakelen (verwijderen) hun gegevens aanmelden, start de app opnieuw en meld u aan met hun gebruikersnaam en *app-wachtwoord* in plaats van hun normale wachtwoord.

Als uw organisatie geen oudere clients, moet u uw gebruikers te maken van app-wachtwoorden niet toestaan.

> [!NOTE]
> Moderne verificatie voor clients met Office 2013
>
> App-wachtwoorden zijn alleen nodig voor apps die moderne verificatie niet ondersteunen. Office 2013 clients moderne verificatieprotocollen ondersteunen, maar moeten worden geconfigureerd. Nieuwere Office-clients ondersteunen automatisch moderne verificatieprotocollen. Zie voor meer informatie de [Office 2013 modern authentication public preview aankondiging](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**V: Mijn gebruikers zeggen dat soms ze het SMS-bericht niet ontvangen of ze tweerichtings-SMS-berichten beantwoorden maar de verificatie een optreedt time-out.**

Levering van tekstberichten en ontvangst van antwoorden in SMS in twee richtingen niet worden gegarandeerd omdat er hebt factoren die van invloed kunnen zijn op de betrouwbaarheid van de service. Deze factoren omvatten het land van bestemming, de mobiele telefoon-provider en de signaalsterkte.

Als uw gebruikers vaak problemen hebt met betrouwbaar tekstberichten ontvangen, laat de mobiele app of telefoongesprek methode gebruiken. De mobiele app kan meldingen zowel via Wi-Fi-verbindingen voor mobiel en ontvangen. Bovendien kan de mobiele app verificatiecodes genereren, zelfs wanneer het apparaat geen signaal helemaal heeft. De Microsoft Authenticator-app is beschikbaar voor [Android](http://go.microsoft.com/fwlink/?Linkid=825072), [IOS](http://go.microsoft.com/fwlink/?Linkid=825073), en [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

Als u de SMS-berichten gebruiken moet, wordt u aangeraden SMS in één richting in plaats van SMS in twee richtingen indien mogelijk. SMS in één richting is betrouwbaarder en kunnen gebruikers zich niet steeds globale SMS-berichten van het antwoord op een SMS-bericht dat is verzonden vanaf een ander land.

**V: kan ik de hoeveelheid tijd die mijn gebruikers hebben de verificatiecode invoeren van een SMS-bericht, voordat het systeem een optreedt time-out wijzigen?**

In sommige gevallen Ja. 

Voor eenzijdige SMS met Azure MFA-Server v7.0 of hoger kunt u de time-out voor de instelling door een registersleutel. Nadat de MFA-cloudservice, het SMS-bericht verzendt, wordt de verificatiecode (of een eenmalige wachtwoordcode) geretourneerd naar de MFA-Server. De MFA-Server slaat de code in het geheugen voor 300 seconden standaard. Als de gebruiker niet de code invoeren voordat de 300 seconden zijn verstreken, wordt de verificatie geweigerd. Volg deze stappen om de standaard time-outinstelling wijzigen:

1. Ga naar HKLM\Software\Wow6432Node\Positive Networks\PhoneFactor.
2. Maak een DWORD-registersleutel aangeroepen **pfsvc_pendingSmsTimeoutSeconds** en de tijd in seconden dat u de Azure MFA-Server voor het opslaan van eenmalige wachtwoordcodes wilt instellen.

>[!TIP] 
>Als u meerdere Servers voor MFA hebt, kent alleen de regel die de oorspronkelijke verificatieaanvraag verwerkt de verificatiecode in die is verzonden naar de gebruiker. Wanneer de gebruiker de code opgeeft, moet de verificatieaanvraag naar te valideren met dezelfde server worden verzonden. Als de codevalidatie wordt verzonden naar een andere server, wordt de verificatie geweigerd. 

SMS in twee richtingen met Azure MFA-Server, kunt u de time-outinstelling configureren in de MFA-beheerportal. Als gebruikers niet binnen de opgegeven time-outperiode voor de SMS reageert, wordt de verificatie geweigerd. 

U kunt de time-outinstelling niet configureren voor eenzijdige SMS met Azure MFA in de cloud (met inbegrip van de AD FS-adapter of de uitbreiding Network Policy Server). Azure AD slaat de verificatiecode voor 180 seconden. 

**V: kan ik Hardwaretokens gebruiken met Azure multi-factor Authentication-Server?**

Als u Azure multi-factor Authentication-Server gebruikt, kunt u Open verificatie (OATH) op basis van tijd, eenmalig wachtwoord (TOTP)-tokens van derde importeren en ze vervolgens gebruiken voor verificatie in twee stappen.

U kunt ActiveIdentity tokens die TOTP OATH-tokens zijn als u de geheime sleutel in een CSV-bestand te plaatsen en in Azure multi-factor Authentication-Server importeren gebruiken. U kunt OATH-tokens met Active Directory Federation Services (ADFS), formulieren gebaseerde verificatie van Internet Information Server (IIS) en Remote Authentication Dial-In User Service (RADIUS) gebruiken, zolang het clientsysteem de gebruikersinvoer kan accepteren.

U kunt importeren TOTP OATH-tokens van derden met de volgende indelingen:  

- Draagbare symmetrische sleutelcontainer (PSKC)  
- CSV als het bestand een serienummer, een geheime sleutel in Base-32-notatie hebben en een tijdsinterval bevat  

**V: kan ik Azure multi-factor Authentication-Server gebruiken voor het beveiligen van Terminal Services**

Ja, maar als u van Windows Server 2012 R2 gebruikmaakt of later u alleen Terminal Services beveiligen kunt met behulp van extern bureaublad-Gateway (RD-Gateway).

Wijzigingen in de beveiliging in Windows Server 2012 R2 gewijzigd hoe Azure multi-factor Authentication-Server verbinding maakt met het beveiligingspakket Local Security Authority (LSA) in Windows Server 2012 en eerdere versies. Voor versies van Terminal Services in Windows Server 2012 of ouder, kunt u [beveiligen van een toepassing met de Windows-verificatie](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Als u Windows Server 2012 R2 gebruikt, moet u extern bureaublad-Gateway.

**V: ik beller-ID in de MFA-Server hebt geconfigureerd, maar mijn gebruikers nog steeds multi-factor Authentication-oproepen ontvangen van een anonieme aanroeper.**

Wanneer multi-factor Authentication-oproepen via het telefoonnetwerk openbare worden geplaatst, worden soms ze gerouteerd via een luchtvaartmaatschappij die biedt geen ondersteuning voor beller-ID. Als gevolg hiervan beller-ID kan niet worden gegarandeerd, zelfs als de multi-factor Authentication-systeem altijd verzonden.

**V: Waarom wordt mijn gebruikers wordt gevraagd hun beveiligingsgegevens registreren?**
Er zijn diverse redenen dat gebruikers registreren hun beveiligingsgegevens kunnen worden gevraagd:

- De gebruiker is ingeschakeld voor meervoudige verificatie door de beheerder in Azure AD, maar heeft geen beveiligingsinformatie nog geregistreerd voor hun rekening.
- De gebruiker is ingeschakeld voor de selfservice voor wachtwoordherstel in Azure AD. De beveiligingsgegevens helpt ze hun wachtwoord in de toekomst opnieuw instellen als ze deze ooit vergeet.
- De gebruiker toegang krijgt tot een toepassing die heeft een beleid voor voorwaardelijke toegang om MFA te vereisen en nog niet eerder is geregistreerd voor MFA.
- De gebruiker registreren van een apparaat met Azure AD (inclusief Azure AD Join), en uw organisatie MFA voor apparaatregistratie is vereist, maar de gebruiker niet eerder is geregistreerd voor MFA.
- De gebruiker genereert Windows Hello voor bedrijven in Windows 10 (waarvoor MFA is vereist) en nog niet eerder is geregistreerd voor MFA.
- De organisatie heeft gemaakt en een registratieprocedure voor MFA-beleid dat is toegepast op de gebruiker is ingeschakeld.
- De gebruiker eerder geregistreerd voor MFA, maar een verificatiemethode die een beheerder heeft sindsdien uitgeschakeld hebt gekozen. De gebruiker moet de registratieprocedure voor MFA opnieuw te selecteren van een nieuwe standaard verificatiemethode daarom doorlopen.


## <a name="errors"></a>Fouten
**V: wat moeten gebruikers doen als ze een foutbericht 'authenticatie-aanvraag is niet voor een geactiveerd account' zien wanneer u meldingen van de mobiele app?**

Vertel Volg deze procedure om hun account van de mobiele app verwijderen en vervolgens opnieuw toevoegen:

1. Ga naar [uw Azure-portal profiel](https://account.activedirectory.windowsazure.com/profile/) en meld u aan met uw organisatieaccount.
2. Selecteer **aanvullende beveiligingsverificatie**.
3. Verwijder het bestaande account uit de mobiele app.
4. Klik op **configureren**, en volg de instructies voor het configureren van de mobiele app.

**V: wat moeten gebruikers doen als er een foutbericht 0x800434D4L tijdens het aanmelden bij een niet-browsertoepassing?**

De 0x800434D4L-fout treedt op wanneer u probeert aan te melden bij een niet-browsertoepassing, geïnstalleerd op een lokale computer, dat niet werkt met accounts waarvoor verificatie in twee stappen.

Een tijdelijke oplossing voor deze fout is dat afzonderlijke accounts voor admin-gerelateerde en niet-beheerders bewerkingen. U kunt later postvakken tussen uw beheerdersaccount en niet-beheerdersaccount koppelen zodat u bij Outlook aanmelden kunt met behulp van uw niet-beheerdersaccount. Voor meer informatie over deze oplossing meer informatie over hoe [bieden een beheerder de mogelijkheid te openen en weergeven van de inhoud van het postvak van de gebruiker](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Volgende stappen
Als uw vraag hier niet beantwoord, laat u dit in de opmerkingen onder aan de pagina. Of Hier volgen enkele aanvullende opties voor het Help-informatie:

* Zoek de [Microsoft Support Knowledge Base](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) voor oplossingen voor technische problemen.
* Zoeken en bladeren technische vragen en antwoorden van de community of uw eigen vraag in de [Azure Active Directory-forums](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Als u een oudere PhoneFactor-klant bent en u vragen hebt of hulp bij het opnieuw instellen van een wachtwoord, gebruik de [wachtwoordherstel](mailto:phonefactorsupport@microsoft.com) koppeling naar een ondersteuningsaanvraag te openen.
* Neem contact op met een supportmedewerker via [ondersteuning van Azure multi-factor Authentication-Server (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Wanneer u contact met ons, is het handig als u zo veel mogelijk gegevens over uw probleem mogelijk kunt opnemen. Informatie die u kunt opgeven bevat de pagina waar u hebt gezien de fout, de specifieke foutcode, de specifieke sessie-ID en de ID van de gebruiker die de fout hebt gezien.
