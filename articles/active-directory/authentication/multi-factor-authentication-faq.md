---
title: Azure multi-factor Authentication FAQ - Azure Active Directory
description: Veelgestelde vragen en antwoorden die betrekking hebben op de Azure multi-factor Authentication.
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
ms.openlocfilehash: 707e0aecdaf2a579d086e012519182078c1a6303
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313223"
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Veelgestelde vragen over Azure multi-factor Authentication

Deze Veelgestelde vragen vindt u antwoorden op veelgestelde vragen over Azure multi-factor Authentication en het gebruik van de multi-factor Authentication-service. Het onderverdeeld in vragen over de service in het algemeen modellen, gebruikerservaringen, facturering en het oplossen van problemen.

## <a name="general"></a>Algemeen

**V: Hoe wordt Azure multi-factor Authentication-Server gebruikersgegevens verwerkt?**

Gebruikersgegevens zijn opgeslagen met multi-factor Authentication-Server, alleen op de on-premises servers. In de cloud worden geen permanente gebruikersgegevens opgeslagen. Wanneer de gebruiker verificatie in twee stappen uitvoert, verzendt multi-Factor Authentication-Server gegevens naar de cloudservice van Azure multi-factor Authentication voor verificatie. Communicatie tussen de multi-factor Authentication-Server en de multi-factor Authentication-cloudservice maakt gebruik van Secure Sockets Layer (SSL) of Transport Layer Security (TLS) via poort 443, uitgaand.

Wanneer verificatie-aanvragen worden verzonden naar de cloudservice, gegevens worden verzameld voor verificatie en het gebruik van rapporten. Gegevensvelden die zijn opgenomen in de logboeken voor verificatie in twee stappen zijn als volgt:

* **Unieke ID** (een gebruikersnaam of on-premises multi-Factor Authentication Server-ID)
* **Eerste en laatste naam** (optioneel)
* **E-mailadres** (optioneel)
* **Telefoonnummer** (bij gebruik van een telefoongesprek of SMS-verificatie)
* **Apparaattoken** (als u verificatie voor mobiele Apps)
* **Verificatiemodus**
* **Verificatieresultaat**
* **Naam van de multi-factor Authentication-Server**
* **Multi-Factor Authentication Server IP**
* **Client IP** (indien beschikbaar)

De optionele velden kunnen worden geconfigureerd in de multi-factor Authentication-Server.

Het verificatieresultaat (geslaagd of DOS-aanval) en de reden als deze is geweigerd, wordt opgeslagen met de verificatiegegevens. Deze gegevens zijn beschikbaar in de verificatie- en gebruiksrapporten.

**V: Welke korte SMS-codes worden gebruikt voor het verzenden van SMS-berichten aan Mijn gebruikers?**

In de Verenigde Staten van Microsoft maakt gebruik van de volgende SMS korte codes:

   * 97671
   * 69829
   * 51789
   * 99399

In Canada Microsoft maakt gebruik van de volgende SMS korte codes:

   * 759731 
   * 673801

Microsoft garandeert niet consistent, SMS of spraak op basis van multi-factor Authentication vragen levering met hetzelfde getal. Uit oogpunt van onze gebruikers, kan Microsoft toevoegen of verwijderen van korte-codes op elk gewenst moment wanneer we aanpassingen van de route voor het verbeteren van de SMS-mailbezorging. Microsoft biedt geen ondersteuning voor korte-codes voor de landen naast de Verenigde Staten en Canada

## <a name="billing"></a>Billing

De meeste vragen over facturering kunnen worden beantwoord door te verwijzen naar een de [pagina met prijzen van multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) of de documentatie over [over het verkrijgen van Azure multi-factor Authentication](concept-mfa-licensing.md).

**V: Wordt mijn organisatie in rekening gebracht voor het verzenden van de telefonische oproepen en SMS-berichten die worden gebruikt voor verificatie?**

Nee, u betaalt geen afzonderlijke telefoongesprekken of SMS berichten verzonden naar gebruikers via de Azure multi-factor Authentication. Als u een MFA-provider per authenticatie, in rekening gebracht voor elke verificatie, maar niet voor de methode die wordt gebruikt.

Uw gebruikers mogelijk worden berekend voor telefoongesprekken of SMS-berichten, op basis van hun persoonlijke telefoonservice ontvangen.

**V: Is het factureringsmodel per gebruiker in rekening voor alle ingeschakelde gebruikers of alleen de resources die verificatie in twee stappen uitgevoerd?**

Facturering is gebaseerd op het aantal gebruikers dat is geconfigureerd voor het gebruik van meervoudige verificatie, ongeacht of ze verificatie in twee stappen die maand worden uitgevoerd.

**V: Hoe werkt facturering voor multi-factor Authentication?**

Wanneer u een per gebruiker of per authenticatie MFA-provider maakt, wordt elke maand op basis van gebruik van uw organisatie Azure-abonnement gefactureerd. Dit factureringsmodel is vergelijkbaar met hoe Azure facturen voor het gebruik van virtuele machines en websites.

Wanneer u een abonnement voor Azure multi-factor Authentication koopt, betaalt uw organisatie de jaarlijkse licentiekosten alleen voor elke gebruiker. Licenties voor MFA en Office 365, Azure AD Premium of Enterprise Mobility + Security-bundels worden in rekening gebracht op deze manier. 

Meer informatie over uw opties in [over het verkrijgen van Azure multi-factor Authentication](concept-mfa-licensing.md).

**V: Is er een gratis versie van Azure multi-factor Authentication?**

In sommige gevallen, Ja.

Multi-factor Authentication voor Azure-beheerders biedt een subset van functies van Azure MFA zonder kosten voor toegang tot Microsoft online services, waaronder de [Azure-portal](https://portal.azure.com) en [Microsoft 365-beheercentrum](https://admin.microsoft.com). Deze aanbieding is alleen van toepassing op globale beheerders in Azure Active Directory-instanties die niet de volledige versie van Azure MFA via een MFA-licentie, fixes of een zelfstandige op basis van gebruik provider hebben. Als uw beheerders gebruik van de gratis versie, en vervolgens u een volledige versie van Azure MFA aanschaffen, klikt u vervolgens alle globale beheerders zijn met verhoogde bevoegdheden voor de betaalde versie automatisch.

Multi-factor Authentication voor Office 365-gebruikers biedt een subset van functies van Azure MFA zonder kosten voor toegang tot Office 365-services, met inbegrip van Exchange Online en SharePoint Online. Deze aanbieding geldt voor gebruikers die beschikken over een Office 365-licentie toegewezen, wanneer het corresponderende exemplaar van Azure Active Directory niet de volledige versie van Azure MFA via een MFA-licentie, fixes of een zelfstandige op basis van gebruik provider hebt.

**V: Kan mijn organisatie wisselen tussen per gebruiker en per authenticatie verbruik factureringsmodellen op elk gewenst moment?**

Als uw organisatie MFA als zelfstandige service met facturering op basis van verbruik koopt, kiest u een factureringsmodel bij het maken van een MFA-provider. U kunt het factureringsmodel niet wijzigen nadat een MFA-provider is gemaakt. U kunt echter de MFA-provider verwijderen en maak vervolgens een met een ander model voor facturering.

Wanneer een MFA-provider is gemaakt, kan deze worden gekoppeld aan een Azure Active Directory (ook wel ' Azure AD-tenant'). Als de huidige MFA-Provider is gekoppeld aan een Azure AD-tenant, kunt u veilig verwijderen van de MFA-provider en er een maken die is gekoppeld aan dezelfde Azure AD-tenant. Als u voldoende licenties voor MFA, Azure AD Premium of EMS (Enterprise Mobility + Security) hebt voor alle gebruikers voor wie MFA is ingeschakeld, kunt u de MFA-provider ook helemaal verwijderen.

Als de MFA-provider is *niet* is gekoppeld aan een Azure AD-tenant of u de nieuwe MFA-provider te koppelen aan een andere Azure AD-tenant, gebruikersinstellingen en configuratieopties niet overgedragen. Bestaande Azure MFA-servers moeten ook opnieuw worden geactiveerd met behulp van de activeringsreferenties die worden gegenereerd door de nieuwe MFA-provider. Als u de MFA-servers opnieuw activeert om ze te koppelen aan de nieuwe MFA-provider, is dit niet van invloed op telefonische verificatie en verificatie via een sms-bericht. Mobiele app-meldingen werken echter niet meer voor gebruikers totdat ze de mobiele app opnieuw activeren.

Meer informatie over MFA-providers in [aan de slag met Azure multi-factor Authentication-Provider](concept-mfa-authprovider.md).

**V: Kan mijn organisatie wisselen op basis van gebruik facturering en abonnementen (een model op basis van een licentie) op elk gewenst moment?**

In sommige gevallen, Ja.

Als uw directory een *per gebruiker* Azure multi-factor Authentication-provider, kunt u MFA licenties toevoegen. Gebruikers met licenties zijn niet worden geteld in de facturering per gebruiker op basis van gebruik. Gebruikers zonder licentie kunnen nog steeds worden ingeschakeld voor MFA via de MFA-provider. Als u koopt en licenties voor uw gebruikers die zijn geconfigureerd toewijzen voor het gebruik van multi-factor Authentication, kunt u de Azure multi-factor Authentication-provider verwijderen. Als u in de toekomst meer gebruikers dan licenties hebt, kunt u altijd een andere per gebruiker MFA-provider maken.

Als uw directory een *per authenticatie* Azure multi-factor Authentication-provider, u wordt altijd gefactureerd voor elke verificatie, zolang de MFA-provider is gekoppeld aan uw abonnement. U kunt MFA-licenties toewijzen aan gebruikers, maar u nog steeds gefactureerd voor elke aanvraag van de verificatie in twee stappen of het afkomstig van iemand met een MFA-licentie toegewezen is of niet.

**V: Hoeft mijn organisatie te gebruiken en synchroniseer identiteiten voor het gebruik van Azure multi-factor Authentication?**

Als uw organisatie gebruikmaakt van een op consumptie gebaseerd factureringsmodel, wordt Azure Active Directory is optioneel, maar niet vereist. Als de MFA-provider is niet gekoppeld aan een Azure AD-tenant, kunt u alleen de Azure multi-factor Authentication-Server on-premises implementeren.

Azure Active Directory is vereist voor het licentiemodel omdat licenties aan de Azure AD-tenant worden toegevoegd wanneer u koopt en deze aan gebruikers in de directory toewijzen.

## <a name="manage-and-support-user-accounts"></a>Beheren en ondersteunen van gebruikersaccounts

**V: Wat moet ik weet mijn gebruikers te doen als ze een antwoord op hun telefoon niet ontvangen?**

Laat uw gebruikers tot probeert 5 keer in vijf minuten om op te halen van een telefoongesprek of SMS voor verificatie. Microsoft maakt gebruik van meerdere providers voor het leveren van oproepen en SMS-berichten. Als dit niet werkt kunt u een ondersteuningsaanvraag openen met Microsoft om op te lossen.

Als de bovenstaande stappen niet Hopelijk werken geconfigureerd al uw gebruikers meer dan één verificatiemethode. Vraag hen om te proberen zich opnieuw aan te melden, maar een andere verificatiemethode op de aanmeldpagina te selecteren.

U kunt uw gebruikers om te verwijzen de [gids voor probleemoplossing door eindgebruikers](../user-help/multi-factor-authentication-end-user-troubleshoot.md).

**V: Wat moet ik doen als een van mijn gebruikers geen toegang hebt tot hun account?**

U kunt de account van de gebruiker opnieuw instellen door deze te gaan via het registratieproces opnieuw te maken. Meer informatie over [beheren van gebruikers- en apparaatinstellingen met Azure multi-factor Authentication in de cloud](howto-mfa-userdevicesettings.md).

**V: Wat moet ik doen als een van mijn gebruikers verliest een telefoon die van app-wachtwoorden gebruikmaakt?**

Om te voorkomen dat onbevoegde toegang, verwijdert u de app-wachtwoorden van de gebruiker. Nadat de gebruiker een vervangend apparaat heeft, kunnen ze de wachtwoorden opnieuw maken. Meer informatie over [beheren van gebruikers- en apparaatinstellingen met Azure multi-factor Authentication in de cloud](howto-mfa-userdevicesettings.md).

**V: Wat gebeurt er als een gebruiker zich niet op niet-browsertoepassingen?**

Als uw organisatie nog steeds oudere clients, en u gebruikt [toegestaan van het gebruik van app-wachtwoorden](howto-mfa-mfasettings.md#app-passwords), en vervolgens uw gebruikers kunnen niet bij deze verouderde clients met hun gebruikersnaam en wachtwoord aanmelden. In plaats daarvan moeten ze [app-wachtwoorden instellen](../user-help/multi-factor-authentication-end-user-app-passwords.md). Uw gebruikers moeten uitschakelen (verwijderen) hun gegevens aanmelden, start de app opnieuw en meld u aan met hun gebruikersnaam en *app-wachtwoord* in plaats van hun normale wachtwoord.

Als uw organisatie geen oudere clients, moet u uw gebruikers te maken van app-wachtwoorden niet toestaan.

> [!NOTE]
> Moderne verificatie voor Office 2013 clients
>
> App-wachtwoorden zijn alleen nodig is voor apps die moderne verificatie niet ondersteunen. Office 2013 clients ondersteunen moderne-verificatieprotocollen, maar moeten worden geconfigureerd. Nieuwere Office-clients ondersteunen automatisch moderne-verificatieprotocollen. Zie voor meer informatie de [Office 2013 modern authentication public preview aankondiging](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**V: Mijn gebruikers zeggen dat ze niet soms de SMS-bericht ontvangen of ze tweerichtings-SMS-berichten beantwoorden maar de verificatie een optreedt time-out.**

Levering van SMS-berichten en de ontvangst van antwoorden in SMS in twee richtingen niet gegarandeerd omdat er hebt factoren die van invloed zijn op de betrouwbaarheid van de service. Deze factoren omvatten het land van de bestemming, de mobiele telefoon-provider en de signaalsterkte.

Als uw gebruikers vaak problemen ondervindt met het ontvangen van SMS-berichten, laat ook de mobiele app of automatische oproep methode te gebruiken. De mobiele app kan meldingen zowel via mobiel en Wi-Fi-verbindingen ontvangen. Bovendien de mobiele app kunt verificatiecodes worden gegenereerd, zelfs wanneer het apparaat geen signaal helemaal heeft. De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?Linkid=825072), [IOS](https://go.microsoft.com/fwlink/?Linkid=825073), en [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071).

Als u de SMS-berichten gebruiken moet, wordt u aangeraden eenrichtings-SMS in plaats van SMS in twee richtingen indien mogelijk. SMS in één richting is betrouwbaarder en voorkomt dat gebruikers waarvoor globale SMS kosten worden berekend vanaf het beantwoorden van een SMS-bericht is verzonden vanaf een ander land.

**V: Kan ik de hoeveelheid tijd dat mijn gebruikers hoeven in te voeren van de verificatiecode van een SMS-bericht, voordat het systeem een optreedt time-out wijzigen?**

In sommige gevallen, Ja. 

Voor eenrichtings-SMS met Azure MFA-Server v7.0 of hoger, kunt u de time-out voor de instelling door een registersleutel. Nadat de MFA-cloudservice het SMS-bericht verzendt, wordt de verificatiecode (of een eenmalige wachtwoordcode) geretourneerd naar de MFA-Server. De MFA-Server slaat de code in het geheugen voor 300 seconden standaard. Als de gebruiker niet de code invoeren voordat de 300 seconden zijn verstreken, wordt de verificatie geweigerd. Volg deze stappen om te wijzigen van de standaard time-outinstelling:

1. Go to HKLM\Software\Wow6432Node\Positive Networks\PhoneFactor.
2. Maak een DWORD-registersleutel met de naam **pfsvc_pendingSmsTimeoutSeconds** en de tijd in seconden dat u de Azure MFA-Server voor het opslaan van eenmalige wachtwoordcodes wilt instellen.

>[!TIP] 
>Als u meerdere MFA-Servers hebt, weet alleen degene die de oorspronkelijke verificatieaanvraag verwerkt de verificatiecode in die is verzonden naar de gebruiker. Wanneer de gebruiker de code opgeeft, moet de verificatieaanvraag voor het valideren van deze worden verzonden naar dezelfde server. Als de codevalidatie wordt verzonden naar een andere server, wordt de verificatie geweigerd. 

SMS in twee richtingen met Azure MFA-Server, kunt u de time-outinstelling configureren in de MFA-beheerportal. Als gebruikers niet op de SMS binnen de gedefinieerde time-outperiode reageren, wordt de verificatie geweigerd. 

Voor eenrichtings-SMS met Azure MFA in de cloud (met inbegrip van de AD FS-adapter of de extensie voor Network Policy Server), kunt u de time-outinstelling niet configureren. Azure AD slaat de verificatiecode voor 180 seconden. 

**V: Kan ik hardware-tokens gebruiken met Azure multi-factor Authentication-Server?**

Als u van Azure multi-factor Authentication-Server gebruikmaakt, kunt u externe Open verificatie (OATH) op basis van tijd, eenmalig wachtwoord (mobiele TOTP)-tokens importeren, en deze vervolgens gebruiken voor verificatie in twee stappen.

U kunt ActiveIdentity tokens die mobiele TOTP OATH-tokens zijn als u de geheime sleutel in een CSV-bestand plaatsen en in Azure multi-factor Authentication-Server importeren gebruiken. Als de invoer van de gebruiker door het clientsysteem kunt accepteren, kunt u OATH-tokens met Active Directory Federation Services (ADFS), op formulieren gebaseerde verificatie van Internet Information Server (IIS) en Remote Authentication Dial-In User Service (RADIUS).

U kunt mobiele TOTP OATH-tokens van derden met de volgende indelingen importeren:  

- Draagbare symmetrische sleutelcontainer (PSKC)  
- CSV als het bestand een serienummer, een geheime sleutel in Base-32-notatie hebben en een bepaalde periode bevat  

**V: Kan ik Azure multi-factor Authentication-Server gebruiken voor het beveiligen van Terminal Services?**

Ja, maar als u van Windows Server 2012 R2 gebruikmaakt of later u alleen Terminal Services beveiligen kunt met behulp van extern bureaublad-Gateway (RD-Gateway).

Wijzigingen in de beveiliging in Windows Server 2012 R2 gewijzigd hoe Azure multi-factor Authentication-Server verbinding maakt met het beveiligingspakket Local Security Authority (LSA) in Windows Server 2012 en eerdere versies. Voor versies van Terminal Services in Windows Server 2012 of ouder, kunt u [beveiligen van een toepassing met Windows-verificatie](howto-mfaserver-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Als u van Windows Server 2012 R2 gebruikmaakt, moet u RD-Gateway.

**V: Heb ik de beller-ID in de MFA-Server hebt geconfigureerd, maar mijn gebruikers nog steeds multi-factor Authentication-oproepen ontvangen van een anonieme oproepende functie.**

Wanneer multi-factor Authentication-oproepen worden gebracht via het openbare telefoonnetwerk, worden soms ze gerouteerd via een provider die biedt geen ondersteuning voor beller-ID. Als gevolg hiervan beller-ID kan niet worden gegarandeerd, zelfs als de multi-factor Authentication-systeem altijd verzonden.

**V: Waarom wordt mijn gebruikers wordt gevraagd om hun beveiligingsgegevens te registreren?**
Er zijn diverse redenen dat gebruikers kunnen worden gevraagd om hun beveiligingsgegevens te registreren:

- De gebruiker voor MFA door de beheerder in Azure AD is ingeschakeld, maar geen informatie over beveiliging nog geregistreerd voor hun rekening.
- De gebruiker is ingeschakeld voor self-service voor wachtwoord opnieuw instellen in Azure AD. De beveiligingsinformatie kunnen ze hun wachtwoord in de toekomst opnieuw instellen als ze dit ooit vergeet.
- De gebruiker toegang krijgen tot een toepassing die een beleid voor voorwaardelijke toegang om MFA te vereisen en nog niet eerder hebt geregistreerd voor MFA.
- De gebruiker een apparaat wordt geregistreerd bij Azure AD (waaronder Azure AD Join), en uw organisatie vereist MFA voor device Registration service, maar de gebruiker is niet eerder hebt geregistreerd voor MFA.
- De gebruiker genereert Windows Hello voor bedrijven in Windows 10 (hiervoor MFA) en nog niet eerder zijn geregistreerd voor MFA.
- De organisatie heeft gemaakt en een registratie bij MFA-beleid dat is toegepast op de gebruiker ingeschakeld.
- De gebruiker eerder hebt geregistreerd voor MFA, maar een verificatiemethode die een beheerder heeft sindsdien uitgeschakeld hebt gekozen. De gebruiker moet daarom gaan via de MFA-registratie opnieuw om te selecteren van een nieuwe standaardmethode voor verificatie.

## <a name="errors"></a>Fouten

**V: Wat moeten gebruikers doen als er een foutbericht 'verificatieaanvraag is niet voor een geactiveerd account' bij het gebruik van mobiele app-meldingen?**

Vertel Volg deze procedure om hun account verwijderen uit de mobiele app, en vervolgens opnieuw toevoegen:

1. Ga naar [uw Azure-portal profiel](https://account.activedirectory.windowsazure.com/profile/) en meld u aan met uw organisatieaccount.
2. Selecteer **aanvullende beveiligingsverificatie**.
3. Verwijder het bestaande account vanuit de mobiele app.
4. Klik op **configureren**, en volg de instructies voor het configureren van de mobiele app.

**V: Wat moeten gebruikers doen als er een foutbericht 0x800434D4L tijdens het aanmelden bij een niet-browsertoepassing?**

De 0x800434D4L fout treedt op wanneer u probeert aan te melden bij een niet-browser-toepassing, geïnstalleerd op een lokale computer, dat werkt niet met accounts waarvoor verificatie in twee stappen.

Een tijdelijke oplossing voor deze fout is dat afzonderlijke accounts voor met betrekking tot beheer en bewerkingen voor niet-beheerders. U kunt later, postvakken tussen uw beheerdersaccount en niet-beheerdersaccount koppelen zodat u bij Outlook aanmelden kunt met behulp van uw niet-beheerdersaccount. Voor meer informatie over deze oplossing informatie over hoe u [bieden een beheerder de mogelijkheid te openen en weergeven van de inhoud van het postvak van gebruikers](https://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Volgende stappen

Als uw vraag hier niet wordt beantwoord, laat u het pakket in de opmerkingen onder aan de pagina. Of hier zijn enkele aanvullende opties om hulp te krijgen:

* Zoek de [Microsoft Support Knowledge Base](https://www.microsoft.com/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) voor oplossingen voor technische problemen.
* Zoeken en bladeren door technische vragen en antwoorden van de community of uw eigen vraag in de [forums van Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Als u een oudere PhoneFactor-klant bent en u vragen hebt of hulp krijgen bij opnieuw instellen van een wachtwoord, gebruikt de [wachtwoordherstel](mailto:phonefactorsupport@microsoft.com) koppeling naar een ondersteuningsaanvraag openen.
* Neem contact op met een ondersteuningsmedewerker via [ondersteuning voor Azure multi-factor Authentication-Server (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Wanneer u contact met ons opneemt, is het handig als u zo veel mogelijk informatie over uw probleem mogelijk kunt opnemen. Informatie die u opgeven kunt, bevat de pagina waar u hebt gezien de fout, de specifieke foutcode, de specifieke sessie-ID en de ID van de gebruiker die de fout hebt gezien.
