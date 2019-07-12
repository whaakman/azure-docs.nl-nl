---
title: Inzicht krijgen in een wereld zonder wachtwoorden met Azure Active Directory | Microsoft Docs
description: Deze handleiding helpt CEO, CIO, gebied, Chief identiteit architecten, Enterprise Architects en beveiliging en IT-besluitvormers die verantwoordelijk is voor het kiezen van een methode voor verificatie van de configuratie voor hun Azure Active Directory-implementatie.
services: active-directory
keywords: de configuratie, azuread
author: martincoetzer
ms.author: martinco
ms.date: 07/09/2019
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 00f66b6010bead3de131095a47ba1e419d2511c0
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723439"
---
# <a name="a-world-without-passwords-with-azure-active-directory"></a>Een wereld zonder wachtwoorden met Azure Active Directory

Het is tijd het opsplitsen van uw relatie met wachtwoorden. Wachtwoorden zijn goed voor ons in het verleden, maar in de hedendaagse digitale werkplek komen ze een relatief eenvoudig aanvalsvector voor hackers. Hackers hou van wachtwoorden en het is niet moeilijk om te zien waarom wanneer u rekening houden met die meestal afgekeurd wachtwoorden in Azure Active Directory (Azure AD) zijn onder andere termen, zoals het jaar, de maand, de seizoen of een lokale Sports werken samen. Bovendien [onderzoek heeft aangetoond](https://aka.ms/passwordguidance) dat traditionele aanbevelingen voor wachtwoordbeheer zoals lengtevereisten en complexiteitsvereisten wijziging frequenties ongewenste voor tal van redenen zijn gerelateerd aan menselijke aard.

Drie typen aanvallen die vaak worden gebruikt voor inbreuk op gebruikersaccounts zijn wachtwoord spray, phishing- en breach afspelen. Azure AD-functies zoals [slimme accountvergrendeling](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-smart-lockout), [uitsluiten van wachtwoorden](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises), en [wachtwoordbeveiliging](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) kan helpen beschermen tegen dit soort aanvallen. Op dezelfde manier implementeren [multi-factor authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) (MFA), of verificatie in twee stappen, biedt extra beveiliging doordat een tweede vorm van verificatie. Maar op de lange termijn een oplossing voor de configuratie is de beste oplossing voor ervoor te zorgen dat de veiligste methode voor verificatie.

In dit artikel is het begin uw reis om te begrijpen en te implementeren van Microsoft-oplossingen zonder wachtwoord uitproberen en help die u tussen een of meer van de volgende opties kiezen:

* **Windows Hello voor bedrijven**. In Windows 10 vervangt Windows Hello voor bedrijven wachtwoorden door sterke tweeledige verificatie op pc's en mobiele apparaten. Deze verificatie bestaat uit een nieuw type referenties van een gebruiker die is gekoppeld aan een apparaat en maakt gebruik van een biometrische of PINCODE.

* **De configuratie aanmelden met Microsoft Authenticator**. De Microsoft Authenticator-app kan worden gebruikt voor aanmelding bij een Azure AD-account zonder een wachtwoord te gebruiken. Net als bij de technologie van Windows Hello voor bedrijven, de Microsoft Authenticator gebruikmaakt van verificatie op basis van een sleutel waarmee de referenties van een gebruiker die is gekoppeld aan een apparaat en maakt gebruik van een biometrische of PINCODE.

* **Beveiligingssleutels FIDO2**. FIDO2 biedt cryptografische aanmeldingsreferenties die uniek zijn voor elke website en op een lokaal apparaat, zoals Windows Hello of externe sleutels worden opgeslagen. Deze sleutels zijn bestand tegen de risico's van phishing, diefstal van wachtwoord, en replay-aanvallen. In combinatie met gebruikersverificatie via biometrie of PINCODE, is de oplossing van twee factoren verificatie vergadering moderne beveiligingsbehoeften.

## <a name="the-future-of-passwordless-authentication"></a>De toekomst van de configuratie-verificatie

Deze dagen, banken, creditcardbedrijven, en andere organisaties en onlineservices vaak uw account beveiligen doordat u twee keer uw identiteit verifiëren: eenmaal met behulp van uw wachtwoord nog een keer per telefoon, tekst of een app. Tijdens verificatie met meerdere factoren beveiligingsproblemen met wachtwoorden die worden gedeeld adressen, heeft gestolen of geraden, het geen betrekking op het ongemak van meerdere factoren van probeert te onthouden zijn. Wat gebruikers en organisaties in cloudtijdperk van vandaag willen is zonder wachtwoord uitproberen verificatiemethoden die zeer veilige zijn *en* handige.

![Beveiliging voor uw gemak bedoeld; vs](./media/azure-ad/azure-ad-pwdless-image1.png)

Windows Hello voor bedrijven, zonder wachtwoord uitproberen aanmelden met Microsoft Authenticator en FIDO2 beveiligingssleutels die alle delen van een eenvoudige, algemene architectuur waarmee gebruikers een verificatiemethode die zowel zeer veilige en gemakkelijke manier om te gebruiken. Alle drie zijn gebaseerd op openbare/persoonlijke sleutel-technologie, moet een lokale beweging, zoals een biometrische of PINCODE en persoonlijke sleutels gebonden aan een enkel apparaat en veilig opgeslagen en nooit worden gedeeld.

## <a name="windows-hello-for-business"></a>Windows Hello voor Bedrijven

In Windows 10 vervangt Windows Hello voor bedrijven wachtwoorden door sterke tweeledige verificatie op computers en apparaten. De verificatie van een nieuw type referenties van een gebruiker die is gekoppeld aan een apparaat en maakt gebruik van een biometrische beweging of PINCODE waarmee gebruikers verifiëren met Azure AD bestaat en een on-premises Active Directory. Gewoon ondertekenen bij een apparaat met behulp van Windows Hello voor bedrijven is eenvoudig. Kunt u een PINCODE of biometrische beweging zoals een vingerafdruk of videodetectie erkenning.

### <a name="windows-hello-for-business-scenarios"></a>Windows Hello voor bedrijven-scenario 's

Windows Hello voor bedrijven is ideaal voor IT-medewerkers die hun eigen toegewezen Windows-PC hebben. De biometrische en referenties zijn rechtstreeks gekoppeld aan de PC van de gebruiker, waardoor de toegang van iedereen dan de eigenaar. Met PKI-integratie en ingebouwde ondersteuning voor eenmalige aanmelding (SSO) biedt Windows Hello voor bedrijven een eenvoudige en handige methode voor naadloos toegang tot bedrijfsbronnen on-premises en in de cloud.

### <a name="windows-hello-for-business-deployment-considerations"></a>Windows Hello voor Business-overwegingen voor implementatie

Windows Hello voor bedrijven is een gedistribueerd systeem die gebruikmaakt van verschillende onderdelen van het uitvoeren van device Registration service, inrichting en verificatie. Daarom implementatie, is vereist voor de juiste planning tussen meerdere teams binnen de organisatie. De Windows Hello voor bedrijven [Planningshandleiding](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) kan worden gebruikt om u te helpen u beslissingen voor het type van Windows Hello voor bedrijven-implementatie en de opties die u moet rekening houden met.

Voor on-premises of hybride implementaties, is het waarschijnlijk dat u hebt:

* Een goed verbonden netwerk van werken

* Toegang tot het internet

* Multi-factor Authentication-Server voor de ondersteuning van MFA tijdens Windows Hello voor bedrijven inrichten

* Juiste naamomzetting, zowel interne als externe namen

* Active Directory en een voldoende aantal domeincontrollers per site om verificatie te ondersteunen

* Active Directory certificaatservices 2012 of hoger

* Een of meer computers op werkstations met Windows 10, versie 1903

Omdat alle typen van Windows Hello voor zakelijke implementaties enterprise verleende certificaten voor domeincontrollers als een vertrouwensbasis gebruiken, wilt u automatisch vernieuwen verlopen certificaten door [configureren van automatische inschrijving voor de server en user certificaten](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/configure-server-certificate-autoenrollment). Voor on-premises implementaties is de id-provider de on-premises server waarop de functie Windows Server Active Directory Federation Services (AD FS) wordt uitgevoerd. Federatieve systemen moeten doorgaans een matrix met load balancing van servers, bekend als een farm. Deze farm is geconfigureerd in een interne netwerk en de topologie van de perimeter-netwerk om te hoge beschikbaarheid voor verificatieaanvragen.

Als u een Groepsbeleid om te vereisen Windows Hello voor bedrijven op de werkplek instelt, kunt u het voorbereiden van gebruikers in uw organisatie via waarin wordt uitgelegd hoe u Windows Hello gebruiken. Bijvoorbeeld wanneer iemand een nieuw apparaat heeft ingesteld, wordt hij gevraagd om te kiezen tussen **dit apparaat behoort tot mijn organisatie** of **dit is mijn eigen persoonlijke apparaat**. Voor zakelijke apparaten, moeten ze de voormalige selecteren. Gebruikers moeten ook worden laten weten welke verbindingsoptie ze moeten selecteren: **Koppelen aan Azure AD join** of **instellen van een lokaal account (later lid van domein)** .

Het is gebruikelijk voor gebruikers die gewend zijn om met behulp van een biometrische beweging dag na dag verifiëren voor uiteindelijk de PINCODE vergeet. Als u wilt aanroepen naar de helpdesk te voorkomen, is het aanbevolen dat u gebruikers de mogelijkheid om opnieuw in te bieden vergeten [wachtwoorden](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment) en [pincodes](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-features#pin-reset).

Er zijn veel opties waaruit u kiezen kunt bij het implementeren van Windows Hello voor bedrijven. Waarin u meerdere opties, zorgt u ervoor dat nagenoeg elke organisatie kunt implementeren Windows Hello voor bedrijven. Houd rekening met de volgende typen implementaties die worden ondersteund:

* [Hybride Azure AD join sleutel vertrouwensrelatie implementatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-key-trust)

* [Hybride Azure AD join vertrouwensrelatie Certificaatimplementatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust)

* [Azure AD Join Single Sign-on implementatiehandleidingen](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso)

* [Op de lokale sleutel vertrouwensrelatie implementatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-key-trust)

* [On Premises vertrouwensrelatie Certificaatimplementatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-cert-trust)

Veel opties biedt, kunt u de implementatie van complexe weergegeven. De meeste organisaties wordt echter waarschijnlijk bepalen dat ze de infrastructuur waarop de Windows Hello voor bedrijven-implementatie, is afhankelijk van de meeste al hebt geïmplementeerd. Ongeacht het geval is, is het belangrijk om te weten dat Windows Hello voor bedrijven een gedistribueerd systeem is en de juiste planning wordt aanbevolen.

Het is raadzaam dat u leest [plannen van een Windows Hello voor bedrijven-implementatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) om te bepalen op het aanbevolen implementatiemodel geschikt zijn voor uw organisatie. Vervolgens, op basis van de planning die u aanbrengt, verwijzen naar de [Windows Hello voor bedrijven-Implementatiehandleiding](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide) om te zorgen voor een geslaagde implementatie van Windows Hello voor bedrijven in uw bestaande omgeving.

### <a name="how-windows-hello-for-business-works"></a>Windows Hello voor bedrijven de werking van

#### <a name="user-sets-up-windows-hello-for-business"></a>Hiermee stelt u gebruikers van Windows Hello voor bedrijven

Windows Hello is ingesteld op het apparaat van de gebruiker na een initiële verificatie van de gebruiker tijdens de inschrijving, en Windows vraagt de gebruiker een beweging in. Dit kan een biometrische, zoals een vingerafdruk of videodetectie erkenning of een PINCODE instellen. Nadat deze is ingesteld, biedt de gebruiker de gebaar om hun identiteit te verifiëren. Windows gebruikt vervolgens Windows Hello om gebruikers te verifiëren.

Op basis van de mogelijkheden van uw Windows 10-apparaat, hebt ofwel u een ingebouwde beveiligde enclave, ook wel een hardware trusted platform module (TPM) of een TPM-software. De TPM slaat de persoonlijke sleutel, waarvoor uw face, een vingerafdruk of een PINCODE te ontgrendelen. De biometrische gegevens kunnen niet worden gebruikt en wordt nooit verzonden naar externe apparaten of servers. Er is geen één verzameling dat die een aanvaller kan inbreuk te stelen biometrische gegevens, omdat Windows Hello alleen biometrische id worden gegevens op het apparaat opgeslagen.

> [!TIP]
> Op het oppervlak, een PINCODE werkt zoals een wachtwoord, maar wel daadwerkelijk beter te beveiligen. Een belangrijk verschil tussen een wachtwoord en een PINCODE is dat de PINCODE is gekoppeld aan het apparaat waarop deze is ingesteld. Iemand die uw wachtwoord steelt kan zich aanmelden bij uw account vanaf elke locatie. Maar als ze uw PINCODE stelen, moeten ze zou uw fysieke apparaat te stelen. Bovendien, omdat een PINCODE lokaal op het apparaat is, deze is niet verzonden overal, zodat deze kan niet worden onderschept verzending of gestolen van een server.

#### <a name="user-using-windows-hello-for-business-for-sign-in"></a>Gebruiker met behulp van Windows Hello voor bedrijven voor aanmelden

Windows Hello voor bedrijven biometrische en pincodes gebruik van asymmetrische (openbaar/persoonlijk sleutelpaar) versleuteling voor verificatie. Tijdens verificatie wordt is de versleuteling gebonden aan de sessiesleutel TLS, beveiligt het verificatieproces zodat een man-in-the-middle-aanval (MiTM) kan niet de resulterende beveiligingstoken of artefact stelen en deze opnieuw vanuit andere plekken afspelen.

Windows Hello voor bedrijven biedt een handige aanmelden ervaring die de gebruiker naar Azure AD verifieert en Active Directory-resources. Azure AD join apparaten tijdens het aanmelden worden geverifieerd bij Azure en eventueel kunnen verifiëren met Active Directory. Hybride Azure Active Directory verbonden apparaten verifiëren met Active Directory tijdens het aanmelden en verifiëren bij Azure Active Directory op de achtergrond.

![Zie de broninstallatiekopie](./media/azure-ad/azure-ad-pwdless-image2.jpeg)

De volgende stappen ziet de verificatie aanmelden bij Azure Active Directory.

![Vergrendelingsscherm voor Windows 10](./media/azure-ad/azure-ad-pwdless-image3.png)

1. Gebruiker zich aanmeldt bij Windows met behulp van biometrische of PINCODE gebaar. Het gebaar Hiermee ontgrendelt u de Windows Hello voor bedrijven persoonlijke sleutel en verzonden naar de verificatie van de Cloud security support provider, aangeduid als de Cloud Azië en Stille Oceaan-provider.

2. De provider van Cloud-Azië en Stille Oceaan vraagt een nonce van Azure Active Directory.

3. Azure AD geeft als resultaat een nonce dat vijf minuten geldig is.

4. De Cloud Azië en Stille Oceaan-provider de nonce met behulp van de persoonlijke sleutel van de gebruiker zich aanmeldt en retourneert de ondertekende nonce naar de Azure Active Directory.

5. Azure Active Directory valideert de ondertekende nonce met behulp van de gebruiker veilig geregistreerde openbare sleutel met de nonce handtekening. Wanneer de geretourneerde ondertekende nonce valideren van de handtekening vervolgens Azure AD worden gevalideerd. Na de nonce wilt valideren, maakt Azure AD een PRT met sessiesleutel die naar transportsleutel is van het apparaat is versleuteld en naar de Cloud Azië en Stille Oceaan-provider wordt geretourneerd.

6. De provider van Cloud-Azië en Stille Oceaan ontvangt de versleutelde PRT met sessiesleutel. Met persoonlijke transportsleutel is van het apparaat, de provider van Cloud-Azië en Stille Oceaan ontsleutelt de sessiesleutel en beschermt de sessiesleutel met behulp van het apparaat TPM.

7. De Cloud Azië en Stille Oceaan-provider retourneert een geslaagde verificatie-antwoord aan Windows waarna de gebruiker toegang heeft tot zowel Windows als cloud en on-premises toepassingen zonder de noodzaak van opnieuw te verifiëren (SSO).

Zie voor een beter overzicht van het verificatieproces in andere scenario's met betrekking tot Windows Hello voor bedrijven, [Windows Hello voor bedrijven en verificatie](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-authentication#Azure-AD-join-authentication-to-Active-Directory-using-a-Key).

#### <a name="user-manages-their-windows-hello-for-business-credentials"></a>Gebruiker beheert hun Windows Hello voor bedrijven-referenties

De [Microsoft PIN reset-services](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-features#pin-reset) is een functie in Azure AD waarmee gebruikers hun PINCODE opnieuw instellen, indien nodig. Met behulp van Groepsbeleid, Microsoft Intune of een compatibel MDM, een beheerder kan configureren voor het veilig gebruik van de Microsoft PIN reset-service waarmee gebruikers hun vergeten PINCODE via instellingen of boven het vergrendelingsscherm opnieuw instellen zonder Windows 10-apparaten opnieuw te registreren.

Gebruikers moeten soms neerkomt dat u wachtwoorden. [Self-service voor wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment) (SSPR) is een andere Azure AD-functie waarmee gebruikers hun wachtwoord opnieuw instellen zonder contact opnemen met IT-personeel. Gebruikers moeten registreren voor of worden geregistreerd voor selfservice voor wachtwoord opnieuw instellen voordat u de service. Tijdens de registratie, de gebruiker ervoor kiest een of meer verificatiemethoden ingeschakeld door hun organisatie. SSPR kan gebruikers snel gedeblokkeerd ophalen en blijven werken, ongeacht waar ze zich bevinden of van de tijd van de dag. Doordat gebruikers zelf deblokkeren, kan uw organisatie verminderen de niet-productieve tijd en de van hoge ondersteuningskosten voor de meest voorkomende problemen met betrekking tot wachtwoord.

## <a name="passwordless-sign-in-with-microsoft-authenticator"></a>De configuratie aanmelden met Microsoft Authenticator

De configuratie aanmelden met Microsoft Authenticator is een andere configuratie-oplossing die kan worden gebruikt voor aanmelding bij Azure AD-accounts met behulp van aanmelden via telefoon. U moet nog steeds uw identiteit verifiëren door iets dat die u weet en iets dat u hebt, maar telefoon aanmelden kunt u uw wachtwoord ingevoerd en voert alle uw identiteit te verifiëren op uw mobiele apparaat met uw vingerafdruk, face of PINCODE te geven.

### <a name="microsoft-authenticator-passwordless-scenarios"></a>Microsoft Authenticator zonder wachtwoord uitproberen scenario 's

De Microsoft Authenticator-app kan gebruikers hun identiteit verifiëren en te verifiëren voor hun werk- of persoonlijk account. Het kan ook worden gebruikt te verbeteren van een wachtwoord met een eenmalige wachtwoordcode of push-melding of de noodzaak om een wachtwoord kan worden overgeslagen vervangen. In plaats van een wachtwoord, bevestig gebruikers hun identiteit via hun mobiele telefoon via een vingerafdruk-scan, videodetectie of iris opname of PINCODE. Gebaseerd op beveiligde technologie die vergelijkbaar is met wat Windows Hello wordt gebruikt en is dit hulpprogramma verpakt in een eenvoudige app op een mobiel apparaat waardoor het een handige optie voor gebruikers. De Microsoft Authenticator-app is beschikbaar voor Android en iOS.

### <a name="microsoft-authenticator-deployment-considerations"></a>Aandachtspunten bij de implementatie van de Microsoft Authenticator

Vereisten voor het gebruik van de Microsoft Authenticator-app moet gebeuren als de configuratie van aanmelding met Azure AD omvatten het volgende:

* Eindgebruikers kunnen worden ingeschakeld voor Azure multi-factor Authentication

* De mogelijkheid voor gebruikers hun apparaten inschrijven met behulp van Microsoft Intune of een oplossing voor externe mobiele Apparaatbeheer (MDM)

Ervan uitgaande dat aan deze vereisten wordt voldaan, beheerders inschakelen zonder wachtwoord uitproberen aanmelden via telefoon in de tenant met behulp van [Windows PowerShell.](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#enable-my-users) Zodra de aanmelding via telefoon is ingeschakeld in de tenant, eindgebruikers kunnen kiezen om te melden met hun telefoon door het selecteren van hun account voor werk of school op de **Accounts** scherm van de app, selecteer vervolgens **aanmelden via telefoon inschakelen** .

Ervan uitgaande dat aanmelding zonder wachtwoord uitproberen door een beheerder is ingeschakeld, moet krijgen eindgebruikers voldoen aan de volgende vereisten:

* Geregistreerd bij Azure multi-factor Authentication

* Meest recente versie van Microsoft Authenticator geïnstalleerd op apparaten met iOS 8.0 of hoger of Android 6.0 of hoger

* Account voor werk of school met pushmeldingen toegevoegd aan de app

Om te voorkomen dat het potentieel van vergrendeld ophalen uit uw account of met accounts op een nieuw apparaat opnieuw te maken, is het aanbevolen dat u Microsoft Authenticator te [back-up van uw accountreferenties](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-backup-recovery) naar de cloud. Na de back-up, kunt u de app ook gebruiken om te herstellen van uw gegevens op een nieuw apparaat mogelijk vermijden ophalen vergrendeld out- of accounts opnieuw hoeft te.

Omdat de meeste gebruikers gewend zijn om te verifiëren met behulp van alleen wachtwoorden, is het belangrijk dat uw organisatie gebruikers met betrekking tot dit proces opleidt. Bewustzijn verkleint de kans dat gebruikers contact opnemen met de helpdesk voor elk [problemen](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#known-issues) met betrekking tot aanmelden met behulp van de Microsoft Authenticator-app.

> [!NOTE]
> Een mogelijke point of failure voor deze oplossing is tijdens het roamen een gebruiker zich op een locatie waarbij er geen verbinding met Internet. Beveiligingssleutels FIDO2 en Windows Hello voor bedrijven zijn niet afhankelijk van deze beperking.

### <a name="how-passwordless-sign-in-with-microsoft-authenticator-works"></a>Hoe de configuratie aanmelden met Microsoft Authenticator werkt

#### <a name="user-sets-up-passwordless-sign-in-with-microsoft-authenticator"></a>Gebruiker stelt u de configuratie aanmelden met Microsoft Authenticator

Voordat u de Microsoft Authenticator-app kan worden gebruikt als een oplossing voor de configuratie te melden bij een Azure AD-account, moeten de stappen worden uitgevoerd door een beheerder en de eindgebruikers.

Eerst een beheerder moet [gebruik van de app als een referentie inschakelen](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#enable-my-users) in de tenant met behulp van Windows PowerShell. De beheerder moet ook eindgebruikers voor Azure multi-factor Authentication (MFA Azure) inschakelen en configureren van de Microsoft Authenticator-app als een van de [verificatiemethoden](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-mfasettings#verification-methods).

Eindgebruikers moet [download en installeer](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) de Microsoft Authenticator-app en [instellen van hun account](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app) gebruik van de Microsoft Authenticator-app als een van de verificatiemethoden.

> [!VIDEO https://www.youtube.com/embed/uWbkLuI4g30]

#### <a name="user-using-microsoft-authenticator-for-passwordless-sign-in"></a>Gebruiker met behulp van de Microsoft Authenticator voor aanmelding zonder wachtwoord uitproberen

De Microsoft Authenticator-app kan worden gebruikt voor aanmelding bij een Azure AD-account zonder een wachtwoord te gebruiken. Terwijl het vergrendelingsscherm van Windows 10 bevat geen de Microsoft Authenticator-app als een optie voor het aanmelden, kunnen gebruikers nog steeds hun gebruikersnaam invoeren en ontvangt vervolgens een pushmelding op hun mobiele apparaat om te controleren of aanwezigheid. Gebruikers bevestigen hun aanwezigheid die overeenkomt met een getal op het aanmeldingsscherm en vervolgens bieden een gezicht scan, vingerafdruk of PINCODE voor het ontgrendelen van de persoonlijke sleutel en de verificatie te voltooien. Deze methode meervoudige verificatie is veiliger dan een wachtwoord en gemakkelijker dan een wachtwoord en een code invoeren.

![Aanmelding verificator](./media/azure-ad/azure-ad-pwdless-image4.png)

Zonder wachtwoord uitproberen verificatie met behulp van Microsoft Authenticator volgt hetzelfde patroon als Windows Hello voor bedrijven, maar iets gecompliceerder is omdat de gebruiker worden geïdentificeerd moet, zodat die Azure AD met de Microsoft Authenticator app versie wordt vinden kan gebruikt.

![Authenticator-proces](./media/azure-ad/azure-ad-pwdless-image5.png)

1. De gebruiker voert hun gebruikersnaam.

2. Azure AD detecteert dat de gebruiker een sterke referentie heeft en de sterke referentie-stroom wordt gestart.

3. Melding wordt verzonden naar de app via Apple Push Notification Service (APNS) op iOS-apparaten of via Firebase Cloud Messaging (FCM) op Android-apparaten.

4. De gebruiker de pushmelding ontvangt en de app te openen.

5. De app roept Azure AD en een proof-of-presence challenge en nonce ontvangt.

6. De gebruiker is de uitdaging voltooid door te voeren hun biometrische of PINCODE voor het ontgrendelen van de persoonlijke sleutel.

7. De nonce is ondertekend met de persoonlijke sleutel en terug naar Azure AD.

8. Azure AD voert validatie van de openbare en persoonlijke sleutel en retourneert een token.

#### <a name="user-manages-their-passwordless-sign-in-with-microsoft-authenticator-credentials"></a>Gebruiker beheert het zonder wachtwoord uitproberen aanmelden met referenties van de Microsoft Authenticator

Met [gecombineerd registratie](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined), gebruikers kunnen registreren en profiteer van de voordelen van Azure multi-factor Authentication en de self-service voor wachtwoord opnieuw instellen. Gebruikers registreren en beheren van deze instellingen door te navigeren naar hun [pagina Mijn profiel](https://aka.ms/mysecurityinfo). Gecombineerd naast het inschakelen van self-service voor Wachtwoordherstel, registratie ondersteunt meerdere verificatiemethoden en acties.

## <a name="fido2-security-keys"></a>Beveiligingssleutels FIDO2

FIDO2 is de meest recente versie van de laag standard FIDO Alliance en bestaat uit twee onderdelen: de W3C webverificatie (WebAuthN) standaard- en het bijbehorende Client-naar-verificator Alliance FIDO-Protocol (CTAP2). De FIDO2-standaarden, zodat gebruikers kunnen gebruikmaken van hardware-, mobiele- en op basis van biometrie verificators eenvoudig verifiëren met veel apps en websites in mobiele en desktop-omgeving.

Microsoft en haar partners hebben samen gewerkt op FIDO2 beveiligingsapparaten voor Windows Hello eenvoudig en veilig verificatie op gedeelde apparaten in te schakelen. Beveiligingssleutels FIDO2 kunnen u uw referenties met u hebben en veilig worden geverifieerd bij een [Azure AD](https://aka.ms/azuread418)-gekoppelde Windows 10-apparaat dat deel uitmaakt van uw organisatie.

WebAuthN definieert een API waarmee u de ontwikkeling en implementatie van sterke, zonder wachtwoord uitproberen verificatie door de web-apps en services. Het protocol CTAP kan externe apparaten zoals FIDO-compatibele sleutels voor het werken met WebAuthN en fungeren als verificators. Bij webverificatie, kunnen gebruikers zich aanmelden met online services met de face-, vingerafdruk, PINCODE of draagbare FIDO2 beveiligingssleutels, gebruik te maken van sterke referenties van de openbare sleutel in plaats van wachtwoorden. Op dit moment WebAuthN wordt ondersteund in Microsoft Edge en ondersteuning voor Chrome en Firefox is in ontwikkeling.

Apparaten en tokens die aan FIDO2 WebAuthN en CTAP protocollen in overeenstemming brengen over een oplossing voor meerdere platformen van sterke verificatie zonder gebruik van wachtwoorden. Microsoft-partners werken op een groot aantal security key vormfactoren, zoals USB-sleutels en smartcards NFC is ingeschakeld.

### <a name="fido2-security-keys-scenarios"></a>FIDO2 beveiligingssleutels scenario 's

Beveiligingssleutels FIDO2 kunnen worden gebruikt voor aanmelding bij Azure AD door de beveiligingssleutel kiezen als de Referentieprovider op het vergrendelingsscherm van Windows 10. Een gebruikersnaam of wachtwoord is niet vereist, waardoor het een ideale oplossing voor de eerste regel werknemers die pc's tussen meerdere gebruikers delen. Ze zijn ook een optie voor uitstekende verificatie wanneer bedrijfsbeleid dicteert dat de referenties van een gebruiker fysiek gescheiden is van het apparaat moet. Gebruikers kunnen ook voor kiezen om te melden bij websites met behulp van hun beveiligingssleutel FIDO2 binnen de Microsoft Edge-browser op Windows 10 versie 1809 of hoger.

### <a name="fido2-security-keys-deployment-considerations"></a>Overwegingen voor de implementatie van beveiligingssleutels FIDO2

Beheerders kunnen FIDO2 ondersteuning inschakelen in Azure AD en de capaciteit toewijzen aan gebruikers of groepen. Het beleid kan ook worden gemaakt voor hoe sleutels zijn ingericht en beperkingen, zoals het toestaan of blokkeren van een specifieke set sleutels van de hardware-beveiliging configureren. Sleutels moeten fysiek worden gedistribueerd naar eindgebruikers.

**De vereisten voor het inschakelen van de configuratie zich aanmelden bij Azure AD en websites met behulp van beveiligingssleutels FIDO2 omvatten het volgende:**

* Azure AD

* Azure Multi-Factor Authentication

* Voorbeeld van registratie gecombineerd

* FIDO2 security key preview vereist een compatibele FIDO2 beveiligingssleutel

* Webverificatie (WebAuthN) is vereist voor Microsoft Edge op Windows 10 versie 1809 of hoger

* FIDO2 op basis van Windows-aanmelding in Azure AD vereist verbonden Windows 10, versie 1809 of hoger (de beste ervaring is op Windows 10 versie 1903 of hoger)

FIDO2-compatibele apparaten omvatten USB NFC en Bluetooth-apparaten. U wordt aangeraden u ervoor kiest de vormfactor die voldoet aan uw specifieke behoeften aangezien sommige platformen en browsers die nog niet FIDO2-compatibel zijn.

We raden u ook aan dat elke organisatie maakt een protocol voor gebruikers en beheerders te volgen een moeten sleutel zoekraken of worden gestolen. Gebruikers moeten de sleutel verloren of gestolen rapporteren, zodat administrators of de gebruiker kan hun sleutels uit het profiel van de gebruiker verwijderen en inrichten van een nieuwe.

### <a name="how-fido2-security-keys-works"></a>De werking van beveiligingssleutels FIDO2

#### <a name="user-sets-up-fido2-security-key"></a>Gebruiker heeft ingesteld FIDO2 beveiligingssleutel

Hoewel beheerders kunnen [handmatig inrichten sleutels](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-enable) en distribueer ze aan eindgebruikers, inrichten en de Referentieprovider FIDO2 op het vergrendelingsscherm van Windows 10 in te schakelen worden ondersteund door [Intune](https://docs.microsoft.com/intune/windows-enrollment-methods). Beheerders moeten ook gebruik van de [Azure-portal](https://portal.azure.com/) token hardwareapparaten inschakelen als een methode voor verificatie van de configuratie.

Implementeren van beveiligingssleutels FIDO2 is ook vereist dat gebruikers registreren hun sleutels die gebruikmaken van [gecombineerd registratie](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined). Met de registratie van de gecombineerde, kunnen gebruikers één keer registreren en profiteer van de voordelen van Azure multi-factor Authentication en eenmalige aanmelding voor wachtwoordherstel (SSPR).

Naast de hardware-token selecteren als de standaardmethode voor verificatie met meerdere factoren, is het raadzaam dat u ook een aanvullende verificatiemethode selecteren.

* Microsoft Authenticator - melding

* Authenticator-app of hardware token--code

* Telefonische oproep

* Sms-bericht

#### <a name="user-using-fido2-security-key-for-sign-in"></a>Gebruiker met behulp van de beveiligingssleutel FIDO2 voor aanmelden

FIDO2 biedt een abstractielaag tussen de vormfactor wordt gebruikt als de verificator en cryptografie met openbare/persoonlijke sleutels om in te schakelen ingebouwde platform verificators zoals Windows Hello en beveiliging sleutels worden omgezet in een persoonlijke sleutel en het leveren van een openbare sleutel dat kan worden gebruikt als een id voor toegang tot externe resources. Beveiligingssleutels FIDO2 zijn uitgerust met hun eigen ingebouwde beveiligde enclave die de persoonlijke sleutel wordt opgeslagen en vereist de biometrische of PINCODE te ontgrendelen. Referenties niet opnieuw kunnen worden gebruikt, opnieuw wordt afgespeeld, of gedeeld tussen services en vallen niet onder phishing en MiTM-aanvallen of server schendingen.

![Aanmelding FIDO2](./media/azure-ad/azure-ad-pwdless-image6.png)

Beveiligingssleutels FIDO2 bieden veilige verificatie, onafhankelijk van de vormfactor. De beveiligingssleutel bevat de referenties en moet worden beveiligd met een extra tweede factor zoals een vingerafdruk (geïntegreerd in de beveiligingssleutel) of een PINCODE worden opgegeven bij de Windows-aanmelding. Microsoft-partners werken op een aantal belangrijke vormfactoren voor beveiliging. Enkele voorbeelden van USB-sleutels en NFC smartcards ingeschakeld.

> [!NOTE]
> Een beveiligingssleutel moet implementeren, bepaalde functies en uitbreidingen van het protocol FIDO2 CTAP [compatibel is met Microsoft](https://aka.ms/fido2securitykeys). Microsoft heeft getest om deze oplossingen voor compatibiliteit met Windows 10 en Azure Active Directory.

![Het aanmeldingsproces FIDO2](./media/azure-ad/azure-ad-pwdless-image9.png)

1. De gebruiker het apparaat FIDO2 in de computer wordt geplaatst.

2. Windows detecteert de beveiligingssleutel FIDO2.

3. Windows wordt een verificatieaanvraag verzonden.

4. Azure AD stuurt terug een nonce.

5. De gebruiker is hun gebaar om te ontgrendelen van de persoonlijke sleutel die zijn opgeslagen in de FIDO2 beveiligingssleutel beveiligde enclave voltooid.

6. De beveiligingssleutel FIDO2 ondertekent de nonce met de persoonlijke sleutel.

7. De PRT tokenaanvraag met ondertekende nonce is verzonden naar Azure AD.

8. Azure AD verifieert de ondertekende nonce met behulp van de openbare sleutel FIDO2.

9. Azure AD retourneert PRT voor toegang tot on-premises bronnen.

#### <a name="user-manages-their-fido2-security-key-credentials"></a>Gebruiker beheert hun belangrijke beveiligingsreferenties van FIDO2

Net als bij de Microsoft Authenticator-app, beheer van referenties voor FIDO2 beveiligingssleutels is gebaseerd op gecombineerde registratie-ervaring voor eindgebruikers.

## <a name="deciding-a-passwordless-method"></a>Een methode voor de configuratie bepalen

Kiezen tussen deze drie opties voor de configuratie, is afhankelijk van de beveiliging, platform en app-vereisten van uw bedrijf.

Hier zijn enkele factoren u moet denken bij het kiezen van de technologie van Microsoft-wachtwoord te hebben:

||**Windows Hello voor Bedrijven**|**De configuratie aanmelden met de Microsoft Authenticator-app**|**Beveiligingssleutels FIDO2**|
|:-|:-|:-|:-|
|**Vereiste**| Windows 10, versie 1809 of hoger<br>Azure Active Directory| Microsoft Authenticator-app<br>Telefoon (iOS en Android-apparaten met Android 6.0 of hoger.)|Windows 10, versie 1809 of hoger<br>Azure Active Directory|
|**modus**|Platform|Software|Hardware|
|**-Systemen en apparaten**|Pc's met een ingebouwde Trusted Platform Module (TPM)<br>Herkenning van PINCODE en biometrie |Herkenning van PINCODE en biometrie op telefoon|FIDO2 beveiligingsapparaten die Microsoft compatibel zijn|
|**Gebruikerservaring**|Meld u aan met behulp van een PINCODE of biometrische recognition (videodetectie, iris of vingerafdruk) met Windows-apparaten.<br>Windows Hello-verificatie is gekoppeld aan het apparaat. de gebruiker moet het apparaat en een onderdeel aanmelden, zoals een PINCODE of biometrische factor voor toegang tot bedrijfsbronnen.|Meld u aan met behulp van een mobiele telefoon met scannen, videodetectie of iris vingerafdrukherkenning of VASTMAKEN.<br>Gebruikers vanaf hun computer of mobiele telefoon, moet u zich aanmelden bij werk- of persoonlijk account.|Meld u aan met behulp van FIDO2 beveiligingsapparaat (biometrie, PINCODE en NFC)<br>Gebruiker kan toegang tot apparaat op basis van organisatie-besturingselementen en verificatie op basis van de PINCODE, biometrische apparaten, zoals USB-sleutels en smartcards NFC is ingeschakeld, sleutels of wearables gebruiken.|
|**Ingeschakelde scenario 's**| Geen wachtwoord meer ervaring met Windows-apparaat.<br>Van toepassing op specifieke zakelijke PC met de mogelijkheid voor eenmalige aanmelding voor apparaten en toepassingen.|Zonder wachtwoord overal oplossing met behulp van de mobiele telefoon.<br>Van toepassing op toegang tot werk of persoonlijk toepassingen op Internet vanaf elk apparaat.|Geen wachtwoord meer ervaring voor werknemers met behulp van biometrie, PINCODE of NFC.<br>Van toepassing op gedeelde pc's en een mobiele telefoon is waar niet een beschikbare optie (zoals helpdeskmedewerkers, openbare kiosk of ziekenhuis team)|

Gebruik de volgende tabel om te kiezen welke methode biedt ondersteuning voor uw vereisten en gebruikers.

|Persona|Scenario|Omgeving|De configuratie-technologie|
|:-|:-|:-|:-|
|**Admin**|Beveiligde toegang tot een apparaat voor beheertaken|Toegewezen Windows 10-apparaat|Windows Hello voor bedrijven en/of FIDO2 beveiligingssleutel|
|**Admin**|Beheertaken op niet-Windows-apparaten| Mobiel of niet-windows-apparaat|De configuratie aanmelden met de Microsoft Authenticator-app|
|**Informatiemedewerker**|Productiviteit werk|Toegewezen Windows 10-apparaat|Windows Hello voor bedrijven en/of FIDO2 beveiligingssleutel|
|**Informatiemedewerker**|Productiviteit werk| Mobiel of niet-windows-apparaat|De configuratie aanmelden met de Microsoft Authenticator-app|
|**Frontline worker**|Kiosken in een fabriek, fabriek, retail of gegevens invoeren|Gedeelde Windows 10-apparaten|FIDO2 beveiligingssleutels|

## <a name="getting-started"></a>Aan de slag

De configuratie-verificatie is de groep van de toekomst en het pad naar een veiligere omgeving. Het verdient aanbeveling dat het organisaties beginnen met het plannen voor deze wijziging en de bijbehorende afhankelijkheden op wachtwoorden. Als u wilt beginnen, houd rekening met de volgende doelen:

* Gebruikers voor MFA, Microsoft Authenticator-app + voorwaardelijke toegang inschakelen.

* Implementatie Azure AD-wachtwoord beveiliging + updatebeleid.

* Gebruikers voor SSPR met gecombineerde registratie inschakelen.

* Microsoft Authenticator-App implementeren voor mobility.

* Implementeer Windows Hello voor bedrijven (1903: Blijf op de hoogte).

* Implementeer FIDO2 apparaten voor gebruikers die telefoons kunnen niet worden gebruikt.

* Indien mogelijk moet u verificatie op basis van wachtwoorden uitschakelen.

Als u wilt deze doelen kan bereiken, raden we aan de volgende benadering:

1. Azure Active Directory te kunnen profiteren van functies, zoals Azure MFA en voorwaardelijke toegang inschakelen.

2. Multi-factor authentication voor extra beveiliging inschakelen.

3. Schakel selfservice voor wachtwoordherstel in het geval van gebruikers moeten terugvallen op een wachtwoord te gebruiken.

4. Implementeer Microsoft Authenticator aanmelden via telefoon voor mobiliteit van toegevoegd.

5. Implementeer Windows Hello voor bedrijven naar al uw Windows 10-apparaten.

6. Beveiligingssleutels FIDO2 voorbereiden.

> [!NOTE]
> Verwijzen naar de Azure Active Directory [licenties pagina](https://azure.microsoft.com/pricing/details/active-directory/) voor meer informatie over de licentievereisten duidelijk zijn voor de configuratie-methoden.

## <a name="conclusion"></a>Conclusie

De afgelopen jaren is Microsoft de toezegging aan het inschakelen van een wereld zonder wachtwoorden blijven. Met Windows 10, Microsoft geïntroduceerd Windows Hello voor bedrijven, een sterk, hardware-beveiligde referentie van twee factoren waarmee u eenmalige aanmelding bij Azure Active Directory en Active Directory. Net als bij de technologie van Windows Hello voor bedrijven, de Microsoft Authenticator-app gebruikmaakt van verificatie op basis van een sleutel waarmee de referenties van een gebruiker die is gekoppeld aan een mobiel apparaat en maakt gebruik van een biometrische of PINCODE. Nu kunnen FIDO2 beveiligingssleutels u uw referenties met u hebben en zich aanmelden bij Azure AD door de beveiligingssleutel kiezen als de Referentieprovider op het vergrendelingsscherm van Windows 10. Alle drie van de oplossingen voor deze configuratie vermindert het risico van phishing, wachtwoord spray replay-aanvallen en gebruikers voorzien van een zeer veilige en handige manier om te melden en toegang tot gegevens vanaf elke locatie.

De acceptatie van technologieën zoals biometrie en cryptografie met openbare sleutels in het algemeen toegankelijk apparaten moderne verificatie met meerdere factoren is een van de meeste impact hebben stappen die nuttige manier van een bedrijf identiteit risico kunnen beperken. Gaan de configuratie is een op de lange termijn benadering voor veilige verificatie en het nog steeds in ontwikkeling. Opgegeven opkomende vereisten, kunnen organisaties voorbereiden zelf door het maken van een plan om te verplaatsen naar de configuratie-technologieën.

## <a name="next-steps"></a>Volgende stappen

* Een overzicht van [wat is de configuratie?](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless)
* [Het inschakelen van de configuratie in Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-enable)
