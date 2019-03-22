---
title: Azure multi-factor Authentication - Azure Active Directory configureren
description: In dit artikel wordt beschreven hoe u Azure multi-factor Authentication-instellingen configureren in Azure portal
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: f622be53297a9d091a62a1239f022bbd4fb71347
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311761"
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure multi-factor Authentication-instellingen configureren

In dit artikel helpt u bij het beheren van instellingen van de multi-factor Authentication in Azure portal. Het bevat informatie over verschillende onderwerpen waarmee u optimaal gebruikmaken van Azure multi-factor Authentication. Niet alle functies zijn beschikbaar in elke [versie van Azure multi-factor Authentication](concept-mfa-whichversion.md#what-features-do-i-need).

U hebt toegang tot de instellingen in verband met multi-factor Authentication van Azure portal door te bladeren naar **Azure Active Directory** > **MFA**.

![Azure portal - Azure AD multi-factor Authentication-instellingen](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-portal.png)

## <a name="settings"></a>Instellingen

Sommige van deze instellingen gelden voor MFA-Server en/of de Azure MFA.

| Functie | Description |
| ------- | ----------- |
| Accountvergrendeling | Tijdelijk Vergrendel accounts in de multi-factor authentication-service als er te veel verificatiepogingen in een rij geweigerd. Deze functie is alleen van toepassing op gebruikers die een pincode om te verifiëren. (MFA-Server) |
| [Gebruikers blokkeren/deblokkeren](#block-and-unblock-users) | Gebruikt voor het blokkeren van bepaalde gebruikers op de MFA-Server (on-premises) geen multi-factor Authentication-aanvragen worden ontvangen. Alle verificatiepogingen voor geblokkeerde gebruikers worden automatisch geweigerd. Gebruikers blijven geblokkeerd gedurende 90 dagen vanaf het moment dat ze worden geblokkeerd. |
| [Fraudewaarschuwing](#fraud-alert) | Instellingen die betrekking hebben op gebruikers de mogelijkheid voor het rapporteren van frauduleuze verificatie aanvragen van MFA-Server configureren. |
| Meldingen | Meldingen van gebeurtenissen van MFA-Server inschakelen. |
| [OATH-tokens](concept-authentication-methods.md#oath-hardware-tokens-public-preview) | In de cloud-Azure MFA-omgevingen gebruikt voor het beheren van OATH-tokens voor gebruikers. |
| [Instellingen van telefoongesprekken](#phone-call-settings) | Configureer instellingen met betrekking tot telefoongesprekken en begroeting voor cloud en on-premises omgevingen. |
| Providers | Hiermee wordt een bestaande verificatieproviders weergegeven dat u mogelijk hebt gekoppeld aan uw account. Nieuwe verificatieproviders kunnen niet worden gemaakt vanaf 1 September 2018 |

## <a name="manage-mfa-server"></a>MFA-server beheren

Instellingen in deze sectie zijn alleen voor MFA-Server.

| Functie | Description |
| ------- | ----------- |
| Serverinstellingen | MFA-Server downloaden en activeringsreferenties voor het initialiseren van uw omgeving te genereren |
| [Eenmalige bypass](#one-time-bypass) | Toestaan dat een gebruiker verifiëren zonder verificatie in twee stappen uitvoeren voor een beperkte periode. |
| [Regels voor caching](#caching-rules) |  Opslaan in cache wordt voornamelijk gebruikt wanneer de on-premises systemen, zoals VPN, verzenden meerdere aanvragen voor verificatie, terwijl de eerste aanvraag nog steeds uitgevoerd wordt. Deze functie kunt de volgende aanvragen voor het automatisch uitgevoerd nadat de gebruiker is geslaagd voor de eerste verificatie wordt uitgevoerd. |
| Serverstatus | Zie de status van uw on-premises MFA-servers met inbegrip van versie, status, IP-adres, en laatste communicatie tijd en datum. |

## <a name="activity-report"></a>Activiteitenrapport

Het reporting beschikbaar hier is specifiek voor MFA-Server (on-premises). Voor Azure MFA (cloud) ziet rapporten het rapport aanmeldingen in Azure AD.

## <a name="block-and-unblock-users"></a>Blokkeren of te deblokkeren van gebruikers

Gebruik de _blokkeren of te deblokkeren gebruikers_ functie om te voorkomen dat gebruikers ontvangen van verificatieaanvragen. Alle verificatiepogingen voor geblokkeerde gebruikers worden automatisch geweigerd. Gebruikers blijven geblokkeerd gedurende 90 dagen vanaf het moment dat ze worden geblokkeerd. Deze functie is specifiek voor MFA-Server (on-premises).

### <a name="block-a-user"></a>Een gebruiker blokkeren

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA** > **gebruikers blokkeren/deblokkeren**.
3. Selecteer **toevoegen** om een gebruiker te blokkeren.
4. Selecteer de **replicatiegroep**. Voer de gebruikersnaam voor de geblokkeerde gebruiker als **gebruikersnaam\@domein.com**. Voer een opmerking in het **reden** veld.
5. Selecteer **toevoegen** voltooid blokkeren van de gebruiker.

### <a name="unblock-a-user"></a>Een gebruiker deblokkeren

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA** > **gebruikers blokkeren/deblokkeren**.
3. Selecteer **opheffen van blokkeringen** in de **actie** kolom naast de gebruiker de blokkering opheffen.
4. Voer een opmerking in het **reden voor deblokkeren** veld.
5. Selecteer **opheffen van blokkeringen** voltooid deblokkeren van de gebruiker.

## <a name="fraud-alert"></a>Fraudewaarschuwing

Configureer de _fraudewaarschuwing_ functie zodat uw gebruikers frauduleuze probeert te krijgen tot hun resources kunnen rapporteren. Gebruikers kunnen pogingen fraude rapporteren met behulp van de mobiele app of via de telefoon. Deze functie is specifiek voor MFA-Server (on-premises).

### <a name="turn-on-fraud-alerts"></a>Fraudewaarschuwingen inschakelen

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA** > **fraudewaarschuwing**.
3. Stel de **gebruikers toestaan te dienen Fraudewaarschuwingen** instelt op **op**.
4. Selecteer **Opslaan**.

### <a name="configuration-options"></a>Configuratie-opties

* **Gebruiker blokkeren wanneer fraude wordt gemeld**: Als een gebruiker fraude wordt gemeld, wordt hun account geblokkeerd gedurende 90 dagen of tot een beheerder beter hun account zicht. Een beheerder kan aanmeldingen met behulp van het rapport bekijken en passende maatregelen nemen om toekomstige fraude te voorkomen. Een beheerder kan vervolgens [deblokkeren](#unblock-a-user) account van de gebruiker.
* **Code voor het melden van fraude tijdens de eerste begroeting**: Wanneer gebruikers een telefonische oproep om uit te voeren van verificatie in twee stappen ontvangen, ze normaal gesproken drukt u op **#** om te bevestigen het aanmelden. Fraude, de gebruiker krijgt een code voor te drukken **#**. Deze code is **0** standaard, maar u kunt deze aanpassen.

   >[!NOTE]
   >De standaard gesproken begroetingen van Microsoft vertelt u gebruikers op **0#** een fraudewaarschuwing te verzenden. Als u wilt gebruiken een andere waarde dan **0**, vastleggen en uploaden van uw eigen aangepaste gesproken begroetingen met de juiste instructies voor uw gebruikers.
   >

### <a name="view-fraud-reports"></a>Fraude-rapporten weergeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **aanmeldingen**. Het rapport fraude maakt nu deel uit van de standaardrapporten voor Azure AD-aanmeldingen.

## <a name="phone-call-settings"></a>Instellingen voor telefoongesprek

### <a name="caller-id"></a>Beller-ID

**Nummer nummerweergave MFA** -dit is het aantal gebruikers zien op hun telefoon. Alleen de VS gebaseerde getallen zijn toegestaan.

>[!NOTE]
>Wanneer multi-factor Authentication-oproepen worden gebracht via het openbare telefoonnetwerk, worden soms ze gerouteerd via een provider die biedt geen ondersteuning voor beller-ID. Als gevolg hiervan beller-ID kan niet worden gegarandeerd, zelfs als de multi-factor Authentication-systeem altijd verzonden.

### <a name="custom-voice-messages"></a>Aangepaste spraakberichten

U kunt uw eigen-opnamen of begroetingen gebruiken voor verificatie met de _aangepaste spraakberichten_ functie. Deze berichten kunnen worden gebruikt in aanvulling op of de Microsoft-opnamen vervangt.

Voordat u begint, worden op de hoogte van de volgende beperkingen:

* De ondersteunde bestandsindelingen zijn wav- en .mp3.
* De maximale bestandsgrootte is 5 MB.
* Verificatieberichten moet korter zijn dan 20 seconden. Berichten die langer dan 20 seconden zijn kunnen ertoe leiden dat de verificatie mislukt. De gebruiker mogelijk niet reageren voordat het bericht is voltooid en de verificatie een optreedt time-out.

### <a name="custom-message-language-behavior"></a>Aangepast bericht taal gedrag

Wanneer een aangepaste gesproken bericht wordt afgespeeld naar de gebruiker, afhankelijk van de taal van het bericht deze factoren:

* De taal van de huidige gebruiker.
  * De taal die wordt gedetecteerd door de browser van de gebruiker.
  * Andere scenario's voor verificatie kunnen zich anders gedragen.
* De taal van de beschikbare aangepaste berichten.
  * Deze taal wordt gekozen door de beheerder bij een aangepast bericht wordt toegevoegd.

Bijvoorbeeld, als er slechts één aangepaste bericht, met een systeemtaal Duits:

* Een gebruiker die wordt geverifieerd in de Duitse taal wordt het aangepaste Duitse bericht horen.
* Een gebruiker die wordt geverifieerd in het Engels wordt het standaard Engels bericht horen.

### <a name="set-up-a-custom-message"></a>Instellen van een aangepast bericht

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar **Azure Active Directory** > **MFA** > **instellingen van telefoongesprekken**.
1. Selecteer **begroeting toevoegen**.
1. Kies het type begroeting.
1. Kies de taal.
1. Selecteer een MP3- of WAV-geluidsbestand uploaden.
1. Selecteer **Toevoegen**.

## <a name="one-time-bypass"></a>Eenmalige bypass

De _eenmalig overslaan_ functie kunt u een gebruiker zich één keer verifiëren zonder verificatie in twee stappen uitvoeren. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden. In situaties waar de mobiele app of de telefoon niet een melding of automatische oproep ontvangen is, kunt u een eenmalig overslaan toestaan, zodat de gebruiker toegang heeft tot de gewenste resource.

### <a name="create-a-one-time-bypass"></a>Maken van een eenmalig overslaan

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA** > **eenmalig overslaan**.
3. Selecteer **Toevoegen**.
4. Selecteer indien nodig de replicatiegroep voor de mogelijkheid tot overslaan.
5. Voer de gebruikersnaam als **gebruikersnaam\@domein.com**. Voer het aantal seconden dat de mogelijkheid tot overslaan moet duren. Voer de reden voor de mogelijkheid tot overslaan.
6. Selecteer **Toevoegen**. De tijdslimiet gaat direct in. De gebruiker moet zich aanmelden voordat het eenmalig overslaan verloopt.

### <a name="view-the-one-time-bypass-report"></a>Bekijk het rapport met eenmalig overslaan

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA** > **eenmalig overslaan**.

## <a name="caching-rules"></a>Regels voor opslaan in cache

U kunt instellen dat een bepaalde periode om toe te staan verificatiepogingen nadat een gebruiker is geverifieerd met behulp van de _caching_ functie. Latere verificatiepogingen voor de gebruiker binnen de opgegeven voltooid periode automatisch. Opslaan in cache wordt voornamelijk gebruikt wanneer de on-premises systemen, zoals VPN, verzenden meerdere aanvragen voor verificatie, terwijl de eerste aanvraag nog steeds uitgevoerd wordt. Deze functie kunt de volgende aanvragen voor het automatisch uitgevoerd nadat de gebruiker is geslaagd voor de eerste verificatie wordt uitgevoerd.

>[!NOTE]
>De functie voor opslaan in cache is niet bedoeld om te worden gebruikt voor aanmeldingen met Azure Active Directory (Azure AD).

### <a name="set-up-caching"></a>Opslaan in cache instellen

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA** > **regels voor Caching**.
3. Selecteer **Toevoegen**.
4. Selecteer de **cachetype** uit de vervolgkeuzelijst. Geef het maximum aantal **seconden in de cache**.
5. Indien nodig, selecteert u een verificatietype en geef een toepassing.
6. Selecteer **Toevoegen**.

## <a name="mfa-service-settings"></a>MFA service-instellingen

Instellingen voor app-wachtwoorden, goedgekeurde IP-adressen, verificatie-opties, en vergeet niet dat multi-factor authentication voor Azure multi-factor Authentication kan worden gevonden in de service-instellingen. Service-instellingen kunnen worden geopend vanuit de Azure-portal door te bladeren naar **Azure Active Directory** > **MFA** > **aan de slag**  >  **Configureren** > **aanvullende instellingen voor cloud-gebaseerde MFA**.

![Azure multi-factor Authentication-service-instellingen](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-service-settings.png)

## <a name="app-passwords"></a>App-wachtwoorden

Sommige toepassingen, zoals Office 2010 of ouder en Apple Mail voordat u iOS 11 ondersteunen geen verificatie in twee stappen. De apps die niet zijn geconfigureerd voor het accepteren van een tweede verificatie. Voor het gebruik van deze toepassingen profiteren van de _app-wachtwoorden_ functie. U kunt een app-wachtwoord gebruiken in plaats van uw traditionele wachtwoord om een app voor verificatie in twee stappen overslaan en doorgaan met werken.

Moderne verificatie wordt ondersteund voor de Microsoft Office 2013-clients en hoger. Office 2013 clients, inclusief Outlook, moderne-verificatieprotocollen ondersteunen en kan worden ingeschakeld om te werken met verificatie in twee stappen. Nadat de client is ingeschakeld, app-wachtwoorden niet nodig zijn voor de client.

>[!NOTE]
>App-wachtwoorden werken niet met beleid voor voorwaardelijke toegang op basis van meervoudige verificatie en moderne verificatie.

### <a name="considerations-about-app-passwords"></a>Overwegingen over app-wachtwoorden

Wanneer u app-wachtwoorden, houd rekening met de volgende belangrijke punten:

* App-wachtwoorden worden slechts één keer per toepassing ingevoerd. Gebruikers hebben geen bijhouden van de wachtwoorden of deze elke keer invoeren.
* Het wachtwoord wordt automatisch gegenereerd en niet is opgegeven door de gebruiker. Automatisch gegenereerde wachtwoorden moeilijker voor aanvallers te raden is en is beter te beveiligen.
* Er is een limiet van 40 wachtwoorden per gebruiker.
* Toepassingen die wachtwoorden in de cache en deze gebruiken in on-premises scenario's kunnen starten mislukt, omdat het app-wachtwoord is niet bekend is buiten het account voor werk of school. Een voorbeeld van dit scenario is Exchange e-mails die zich on-premises, maar de gearchiveerde e-mail is in de cloud. In dit scenario wordt werkt hetzelfde wachtwoord niet.
* Nadat de multi-factor Authentication is ingeschakeld op een gebruikersaccount, kunnen app-wachtwoorden worden gebruikt met de meeste niet-browserclients zoals Outlook en Microsoft Skype voor bedrijven. Beheeracties kunnen niet worden uitgevoerd met behulp van app-wachtwoorden via niet-browsertoepassingen, zoals Windows PowerShell. De acties kunnen niet worden uitgevoerd, zelfs wanneer de gebruiker een Administrator-account heeft. Om uit te voeren PowerShell-scripts, een service-account maken met een sterk wachtwoord en het account voor verificatie in twee stappen niet inschakelen.

>[!WARNING]
>App-wachtwoorden werken niet in hybride omgevingen waarin clients communiceren met zowel on-premises en cloud automatisch worden gedetecteerd eindpunten. Domeinwachtwoorden zijn vereist voor het verifiëren van on-premises. App-wachtwoorden zijn vereist om te verifiëren met de cloud.

### <a name="guidance-for-app-password-names"></a>Richtlijnen voor namen van app-wachtwoord

Namen van App-wachtwoord moeten vergelijkbaar zijn met het apparaat waarop ze worden gebruikt. Als u een laptop met niet-browsertoepassingen zoals Outlook, Word en Excel hebt, maakt u één app-wachtwoord met de naam **Laptop** voor deze apps. Maken van een andere app-wachtwoord met de naam **Desktop** voor dezelfde toepassingen die worden uitgevoerd op uw desktopcomputer.

>[!NOTE]
>We raden u aan één appwachtwoord per apparaat, in plaats van een app-wachtwoord per toepassing te maken.

### <a name="federated-or-single-sign-on-app-passwords"></a>Federatieve of eenmalige aanmelding-app-wachtwoorden

Azure AD biedt ondersteuning voor Federatie of eenmalige aanmelding (SSO) met on-premises Windows Server Active Directory Domain Services (AD DS). Als uw organisatie is gefedereerd met Azure AD en u Azure multi-factor Authentication, kunt u de volgende punten over de app-wachtwoorden.

>[!NOTE]
>De volgende punten gelden alleen voor federatieve (SSO)-klanten.

* App-wachtwoorden worden gecontroleerd door Azure AD, en daarom federaties omzeild. Federatie wordt actief gebruikt alleen bij het instellen van app-wachtwoorden.
* De id-Provider (IdP) is geen contact gemaakt voor federatieve gebruikers (SSO), in tegenstelling tot de passieve stroom. De app-wachtwoorden worden opgeslagen in het account voor werk of school. Als een gebruiker het bedrijf verlaat, gegevens van de gebruiker aan het account werk- of schoolaccount stromen met behulp van **DirSync** in realtime. De uitschakelen/verwijderen van het account kan maximaal drie uur te synchroniseren, die de uitschakelen/verwijderen van het app-wachtwoord in Azure AD kan uitstellen duren.
* On-premises client Access Control-instellingen worden niet herkend door de functie voor app-wachtwoorden.
* Er zijn geen on-premises verificatie logboekregistratie/auditingfunctie is beschikbaar voor gebruik met de functie voor app-wachtwoorden.
* Sommige geavanceerde architecturen vereisen een combinatie van referenties voor verificatie met clients. Deze referenties kunnen opnemen een werk of school-account gebruikersnaam en wachtwoorden en app-wachtwoorden. De vereisten, is afhankelijk van hoe de verificatie wordt uitgevoerd. Voor clients die verificatie op basis van een on-premises infrastructuur, een werk- of school-account gebruikersnaam en wachtwoord voor een vereiste. Een app-wachtwoord is vereist voor clients die worden geverifieerd bij Azure AD.

  Stel bijvoorbeeld dat u hebt de volgende architectuur:

  * Uw on-premises exemplaar van Active Directory is gefedereerd met Azure AD.
  * Bent u Exchange online.
  * U bent gebruikmaken van Skype voor bedrijven on-premises.
  * U gebruikt Azure multi-factor Authentication.

  In dit scenario gebruikt u de volgende referenties:

  * Als u wilt aanmelden bij Skype voor bedrijven, gebruikt u uw werk of school-account gebruikersnaam en wachtwoord.
  * Gebruiken voor toegang tot het adresboek van een Outlook-client die verbinding met Exchange online maakt, een app-wachtwoord.

### <a name="allow-users-to-create-app-passwords"></a>Gebruikers toestaan te maken van app-wachtwoorden

Gebruikers kunnen geen app-wachtwoorden maken standaard. De functie voor app-wachtwoorden moet zijn ingeschakeld. Als u wilt bieden gebruikers de mogelijkheid te maken van app-wachtwoorden, gebruik de volgende procedure:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication, **service-instellingen**.
5. Op de **Service-instellingen** weergeeft, schakelt de **gebruikers toestaan te maken van app-wachtwoorden te melden bij niet-browsertoepassingen** optie.

### <a name="create-app-passwords"></a>App-wachtwoorden maken

Gebruikers kunnen app-wachtwoorden maken tijdens de initiële inschrijving. De gebruiker heeft de optie voor het maken van app-wachtwoorden aan het einde van het registratieproces.

Gebruikers kunnen ook app-wachtwoorden maken na de registratie. Zie voor meer informatie en gedetailleerde stappen voor uw gebruikers [wat zijn app-wachtwoorden in Azure multi-factor Authentication?](../user-help/multi-factor-authentication-end-user-app-passwords.md)

## <a name="trusted-ips"></a>Goedgekeurde IP-adressen

De _goedgekeurde IP-adressen_ functie van Azure multi-factor Authentication wordt gebruikt door beheerders van een beheerde of een federatieve-tenant. De functie omzeilt verificatie in twee stappen voor gebruikers die zich aanmelden vanaf het bedrijfsintranet. De functie is beschikbaar met de volledige versie van Azure multi-factor Authentication en niet de gratis versie voor beheerders. Zie voor meer informatie over het ophalen van de volledige versie van Azure multi-factor Authentication [Azure multi-factor Authentication](multi-factor-authentication.md).

> [!NOTE]
> MFA vertrouwde IP-adressen en voorwaardelijke toegang benoemde locaties werken alleen met IPV4-adressen.

Als uw organisatie de NPS-extensie implementeert voor MFA on-premises toepassingen Houd er rekening mee wordt altijd het IP-adres van bron naar de NPS-server de verificatie probeert stromen via worden weergegeven.

| Azure AD-tenant-type | Vertrouwde IP-adressen functieopties |
|:--- |:--- |
| Managed |**Specifieke IP-adressen**: Beheerders opgeven voor een bereik van IP-adressen die niet kunnen gebruikmaken van verificatie in twee stappen voor gebruikers die zich aanmelden vanaf het bedrijfsintranet.|
| Federatief |**Alle federatieve gebruikers**: Alle federatieve gebruikers die zich aanmelden vanaf binnen de organisatie kunnen verificatie in twee stappen overslaan. De gebruikers omzeilen verificatie met behulp van een claim dat is uitgegeven door Active Directory Federation Services (AD FS).<br/>**Specifieke IP-adressen**: Beheerders opgeven voor een bereik van IP-adressen die niet kunnen gebruikmaken van verificatie in twee stappen voor gebruikers die zich aanmelden vanaf het bedrijfsintranet. |

De goedgekeurde IP-adressen overslaan werkt alleen in het bedrijfsintranet. Als u selecteert de **alle federatieve gebruikers** optie en een gebruiker zich aanmeldt via buiten het bedrijfsintranet, de gebruiker heeft om te verifiëren met behulp van verificatie in twee stappen. Het proces is hetzelfde, zelfs als de gebruiker een AD FS claim geeft. 

### <a name="end-user-experience-inside-of-corpnet"></a>Eindgebruikerservaring binnen bedrijfsnetwerk

Wanneer de goedgekeurde IP-adressen-functie is uitgeschakeld, is verificatie in twee stappen vereist voor browser-stromen. App-wachtwoorden zijn vereist voor oudere interactieve toepassingen.

Als de goedgekeurde IP-adressen-functie is ingeschakeld, verificatie in twee stappen is *niet* vereist voor de browser stromen. App-wachtwoorden zijn *niet* vereist zijn voor oudere interactieve toepassingen, mits de gebruiker een app-wachtwoord nog niet is gemaakt. Nadat een app-wachtwoord gebruikt wordt, blijft het wachtwoord vereist. 

### <a name="end-user-experience-outside-corpnet"></a>Eindgebruikerservaring buiten corpnet

Ongeacht of de goedgekeurde IP-adressen-functie is ingeschakeld, is verificatie in twee stappen vereist voor browser-stromen. App-wachtwoorden zijn vereist voor oudere interactieve toepassingen.

### <a name="enable-named-locations-by-using-conditional-access"></a>Benoemde locaties door middel van voorwaardelijke toegang inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **voorwaardelijke toegang** > **benoemde locaties**.
3. Selecteer **nieuwe locatie**.
4. Voer een naam voor de locatie.
5. Selecteer **als vertrouwde locatie markeren**.
6. Voer het IP-adresbereik in CIDR-notatie, zoals **192.168.1.1/24**.
7. Selecteer **Maken**.

### <a name="enable-the-trusted-ips-feature-by-using-conditional-access"></a>De goedgekeurde IP-adressen-functie inschakelen door middel van voorwaardelijke toegang

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **voorwaardelijke toegang** > **benoemde locaties**.
3. Selecteer **vertrouwde IP's voor MFA configureren**.
4. Op de **Service-instellingen** pagina onder **goedgekeurde IP-adressen**, kiezen uit een van de volgende twee opties:

   * **Voor aanvragen van federatieve gebruikers die afkomstig zijn van mijn intranet**: Als u wilt deze optie kiest, schakel het selectievakje in. Alle federatieve gebruikers die aanmelden met het bedrijfsnetwerk verificatie in twee stappen overslaan met behulp van een claim dat is uitgegeven door AD FS. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **Voor aanvragen van een specifiek bereik van openbare IP-adressen**: Als u wilt deze optie kiest, voert u de IP-adressen in het tekstvak met behulp van de CIDR-notatie.
      * Gebruik voor IP-adressen die zich in het bereik xxx.xxx.xxx.1 via xxx.xxx.xxx.254 notatie, zoals **xxx.xxx.xxx.0/24**.
      * Gebruik voor één IP-adres, de notatie, zoals **xxx.xxx.xxx.xxx/32**.
      * Voer maximaal 50 IP-adresbereiken. Gebruikers die zich aanmelden vanaf deze IP-adressen overslaan verificatie in twee stappen.

5. Selecteer **Opslaan**.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>De goedgekeurde IP-adressen inschakelen met behulp van service-instellingen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication, **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **goedgekeurde IP-adressen**, kiest u een (of beide) van de volgende twee opties:

   * **Voor aanvragen van federatieve gebruikers op mijn intranet**: Als u wilt deze optie kiest, schakel het selectievakje in. Alle federatieve gebruikers die aanmelden met het bedrijfsnetwerk verificatie in twee stappen overslaan met behulp van een claim dat is uitgegeven door AD FS. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **Voor aanvragen van een opgegeven bereik van IP-subnetten**: Als u wilt deze optie kiest, voert u de IP-adressen in het tekstvak met behulp van de CIDR-notatie.
      * Gebruik voor IP-adressen die zich in het bereik xxx.xxx.xxx.1 via xxx.xxx.xxx.254 notatie, zoals **xxx.xxx.xxx.0/24**.
      * Gebruik voor één IP-adres, de notatie, zoals **xxx.xxx.xxx.xxx/32**.
      * Voer maximaal 50 IP-adresbereiken. Gebruikers die zich aanmelden vanaf deze IP-adressen overslaan verificatie in twee stappen.

6. Selecteer **Opslaan**.

## <a name="verification-methods"></a>Verificatiemethoden

U kunt de verificatiemethoden die beschikbaar voor uw gebruikers zijn. De volgende tabel geeft een kort overzicht van de methoden.

Wanneer uw gebruikers hun account voor Azure multi-factor Authentication registreren, kiezen ze hun gewenste verificatiemethode uit de opties die u hebt ingeschakeld. Richtlijnen voor het registratieproces voor de gebruiker is opgegeven in [instellen van mijn account voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-first-time.md).

| Methode | Description |
|:--- |:--- |
| Bellen naar telefoon |Plaatst een geautomatiseerd telefoongesprek. De gebruiker beantwoordt het gesprek en drukt # in het toetsenblok van de telefoon om te verifiëren. Het telefoonnummer is niet gesynchroniseerd met on-premises Active Directory. |
| Sms-bericht naar telefoon |Verzendt een SMS-bericht met een verificatiecode. De gebruiker wordt gevraagd de verificatiecode invoeren in de interface van aanmelding. Dit proces heet SMS in één richting. SMS in twee richtingen betekent dat de gebruiker tekst weer een bepaalde code moet. SMS in twee richtingen is afgeschaft en wordt niet ondersteund na 14 November 2018. Gebruikers die zijn geconfigureerd voor SMS in twee richtingen worden automatisch overgeschakeld naar de _oproep naar telefoon_ verificatie op dat moment.|
| Melding via mobiele app |Verzendt een pushmelding naar uw telefoon of geregistreerd apparaat. De gebruiker de melding weergaven en **controleren** om verificatie te voltooien. De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071), [Android](https://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](https://go.microsoft.com/fwlink/?Linkid=825073). |
| Verificatiecode uit de mobiele app of hardwaretoken |De Microsoft Authenticator-app genereert een nieuwe OATH-verificatiecode elke 30 seconden. De gebruiker voert de verificatiecode in de interface van aanmelding. De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071), [Android](https://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](https://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="enable-and-disable-verification-methods"></a>In- en uitschakelen van verificatiemethoden

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication, **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **verificatieopties**, selecteer/Hef de selectie van de methoden om te bieden aan uw gebruikers.
6. Klik op **Opslaan**.

Meer informatie over het gebruik van verificatiemethoden kunnen u vinden in het artikel [wat verificatiemethoden zijn](concept-authentication-methods.md).

## <a name="remember-multi-factor-authentication"></a>Multi-Factor Authentication onthouden

De _multi-factor Authentication onthouden_ functie voor apparaten en browsers die worden vertrouwd door de gebruiker is een gratis functie voor alle gebruikers van multi-factor Authentication. Gebruikers kunnen achteraf gedurende een opgegeven aantal dagen, nadat ze hebben is aangemeld op een apparaat met behulp van multi-factor Authentication overslaan. De functie verbetert de bruikbaarheid door het minimaliseren van het aantal keren dat die een gebruiker heeft verificatie in twee stappen uitvoeren op hetzelfde apparaat.

>[!IMPORTANT]
>Als een account of een apparaat is geknoeid, kan multi-Factor Authentication onthouden voor vertrouwde apparaten beveiliging beïnvloeden. Als een zakelijk account wordt aangetast of als een vertrouwd apparaat is zoekgeraakt of gestolen, moet u [multi-factor Authentication herstellen op alle apparaten](howto-mfa-userdevicesettings.md#restore-mfa-on-all-remembered-devices-for-a-user).
>
>De herstelbewerking trekt u de status van de vertrouwde van alle apparaten en de gebruiker is vereist voor het uitvoeren van verificatie opnieuw uit. U kunt ook vertelt u gebruikers multi-factor Authentication herstellen op hun eigen apparaten met de instructies in [uw instellingen beheren voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted).
>

### <a name="how-the-feature-works"></a>De werking van de functie

De functie van de multi-factor Authentication onthouden stelt een permanente cookies in de browser wanneer een gebruiker selecteert de **niet opnieuw vragen gedurende X dagen** optie bij het aanmelden. De gebruiker is niet opnieuw gevraagd voor meervoudige verificatie van die dezelfde browser totdat de cookie verloopt. Als de gebruiker een andere browser op hetzelfde apparaat opent of hun cookies wist, worden ze opnieuw gevraagd te bevestigen.

De **niet opnieuw vragen gedurende X dagen** optie op niet-browsertoepassingen, ongeacht of de app ondersteunt met moderne verificatie wordt niet weergegeven. Gebruik van deze apps _vernieuwingstokens_ die elk uur nieuwe toegangstokens bieden. Wanneer een vernieuwingstoken is gevalideerd, wordt Azure AD gecontroleerd dat de laatste verificatie in twee stappen is opgetreden in het opgegeven aantal dagen.

De functie vermindert het aantal verificaties op web-apps, waarbij elke keer normaal gevraagd. De functie verhoogt het aantal authenticaties voor moderne verificatie-clients die normaal gesproken om de 90 dagen vraagt. Het aantal authenticaties in combinatie met beleid voor voorwaardelijke toegang kan ook worden verhogen.

>[!IMPORTANT]
>De **multi-factor Authentication onthouden** functie is niet compatibel met de **aangemeld blijven** functie van AD FS, wanneer gebruikers verificatie voor AD FS via meerdere factoren van Azure uitvoeren Authentication-Server of een oplossing van derden met meervoudige verificatie.
>
>Als uw gebruikers selecteren **aangemeld blijven** op AD FS en ook hun apparaat als vertrouwd voor multi-factor Authentication is ingeschakeld, wordt niet automatisch de gebruiker gecontroleerd nadat de **multi-factor authenticationonthouden**aantal dagen verloopt. Azure AD vraagt een nieuwe verificatie, maar de AD FS retourneert een token met de oorspronkelijke multi-factor Authentication-claim en datum, in plaats presterende verificatie in twee stappen opnieuw. **Hiermee stelt u deze reactie uit een lus verificatie tussen Azure AD en AD FS.**
>

### <a name="enable-remember-multi-factor-authentication"></a>Schakel multi-factor Authentication onthouden

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication, **service-instellingen**.
5. Op de **Service-instellingen** pagina **beheren multi-factor authentication onthouden**, selecteer de **toestaan dat gebruikers multi-factor authentication onthouden op apparaten die ze vertrouwen**optie.
6. Stel het aantal dagen om toe te staan van vertrouwde apparaten verificatie in twee stappen overslaan. De standaardwaarde is 14 dagen.
7. Selecteer **Opslaan**.

### <a name="mark-a-device-as-trusted"></a>Een apparaat als vertrouwd is ingeschakeld

Nadat u de multi-factor Authentication onthouden functie hebt ingeschakeld, gebruikers een apparaat als vertrouwd als kunnen markeren ze zich aanmelden door te selecteren **niet opnieuw vragen**.

## <a name="next-steps"></a>Volgende stappen

[Azure AD-aanmeldingspagina huisstijl aanpassen](../fundamentals/customize-branding.md)
