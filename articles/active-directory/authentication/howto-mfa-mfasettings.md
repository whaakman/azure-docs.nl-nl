---
title: Azure multi-factor Authentication configureren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Azure multi-factor Authentication-instellingen configureren in Azure portal
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 8ba1286f7283a1062b2b94d58c2439e8461c1573
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48817123"
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure multi-factor Authentication-instellingen configureren

In dit artikel helpt u bij het beheren van Azure multi-factor Authentication nu dat u actief en werkend bent. Het bevat informatie over verschillende onderwerpen waarmee u optimaal gebruikmaken van Azure multi-factor Authentication. Niet alle functies zijn beschikbaar in elke [versie van Azure multi-factor Authentication](concept-mfa-whichversion.md#what-features-do-i-need).

| Functie | Beschrijving | 
|:--- |:--- |
| [Blokkeren of te deblokkeren van gebruikers](#block-and-unblock-users) |De gebruikers blokkeren/deblokkeren functie gebruiken om te voorkomen dat gebruikers ontvangen van verificatieaanvragen. |
| [Fraudewaarschuwing](#fraud-alert) |De waarschuwing fraude-functie configureren zodat uw gebruikers frauduleuze probeert te krijgen tot hun resources kunnen rapporteren. |
| [Eenmalige bypass](#one-time-bypass) |De eenmalige bypass-functie gebruiken om gebruikers kunnen worden geverifieerd door één keer _overslaan_ multi-factor Authentication. |
| [Aangepaste spraakberichten](#custom-voice-messages) |Gebruik de functie aangepaste berichten kunt uw eigen-opnamen of begroetingen met multi-factor Authentication. |
| [Opslaan in cache](#caching-in-azure-multi-factor-authentication) |Gebruik de functie voor opslaan in cache instellen van een bepaalde periode zodat verdere authenticatiepogingen automatisch laten slagen. |
| [Goedgekeurde IP-adressen](#trusted-ips) |Beheerders van een beheerde of een federatieve tenant kunnen de goedgekeurde IP-adressen-functie gebruiken om verificatie in twee stappen voor gebruikers die zich aanmelden vanaf het bedrijfsintranet over te slaan. |
| [App-wachtwoorden](#app-passwords) |Gebruik de functie voor app-wachtwoord om in te schakelen van een toepassing op de multi-factor Authentication eenmalig overslaan en doorgaan met werken. |
| [Houd er rekening mee multi-factor Authentication voor vertrouwde apparaten en browsers](#remember-multi-factor-authentication-for-trusted-devices) |Deze functie gebruiken om te onthouden van vertrouwde apparaten en browsers voor een bepaald aantal dagen nadat een gebruiker heeft is aangemeld met behulp van multi-factor Authentication. |
| [Selecteerbare verificatiemethoden](#selectable-verification-methods) |Deze functie gebruiken om te selecteren van de lijst met verificatiemethoden die gebruikers mogen gebruiken. |

## <a name="block-and-unblock-users"></a>Blokkeren of te deblokkeren van gebruikers

Gebruik de _blokkeren of te deblokkeren gebruikers_ functie om te voorkomen dat gebruikers ontvangen van verificatieaanvragen. Alle verificatiepogingen voor geblokkeerde gebruikers worden automatisch geweigerd. Gebruikers blijven geblokkeerd gedurende 90 dagen vanaf het moment dat ze worden geblokkeerd.

### <a name="block-a-user"></a>Een gebruiker blokkeren

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **gebruikers blokkeren/deblokkeren**.
3. Selecteer **toevoegen** om een gebruiker te blokkeren.
4. Selecteer de **replicatiegroep**. Voer de gebruikersnaam voor de geblokkeerde gebruiker als **gebruikersnaam<span></span>@domain.com**. Voer een opmerking in het **reden** veld.
5. Selecteer **toevoegen** voltooid blokkeren van de gebruiker.

### <a name="unblock-a-user"></a>Een gebruiker deblokkeren

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **gebruikers blokkeren/deblokkeren**.
3. Selecteer **opheffen van blokkeringen** in de **actie** kolom naast de gebruiker de blokkering opheffen.
4. Voer een opmerking in het **reden voor deblokkeren** veld.
5. Selecteer **opheffen van blokkeringen** voltooid deblokkeren van de gebruiker.

## <a name="fraud-alert"></a>Fraudewaarschuwing

Configureer de _fraudewaarschuwing_ functie zodat uw gebruikers frauduleuze probeert te krijgen tot hun resources kunnen rapporteren. Gebruikers kunnen pogingen fraude rapporteren met behulp van de mobiele app of via de telefoon.

### <a name="turn-on-fraud-alerts"></a>Fraudewaarschuwingen inschakelen

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **fraudewaarschuwing**.

   ![Fraudewaarschuwingen inschakelen](./media/howto-mfa-mfasettings/fraudalert.png)

3. Stel de **gebruikers toestaan te dienen Fraudewaarschuwingen** instelt op **op**.
4. Selecteer **Opslaan**.

### <a name="configuration-options"></a>Configuratie-opties

- **Gebruiker blokkeren wanneer fraude wordt gemeld**: als een gebruiker fraude wordt gemeld, hun account gedurende 90 dagen of tot een beheerder beter hun account zicht is geblokkeerd. Een beheerder kan aanmeldingen met behulp van het rapport bekijken en passende maatregelen nemen om toekomstige fraude te voorkomen. Een beheerder kan vervolgens [deblokkeren](#unblock-a-user) account van de gebruiker.
- **Code voor het melden van fraude tijdens de eerste begroeting**: wanneer gebruikers een telefonische oproep om uit te voeren van verificatie in twee stappen ontvangen, ze normaal gesproken drukt u op **#** om te bevestigen het aanmelden. Fraude, de gebruiker krijgt een code voor te drukken **#**. Deze code is **0** standaard, maar u kunt deze aanpassen.

  >[!NOTE]
  >De standaard gesproken begroetingen van Microsoft vertelt u gebruikers op **0#** een fraudewaarschuwing te verzenden. Als u wilt gebruiken een andere waarde dan **0**, vastleggen en uploaden van uw eigen aangepaste gesproken begroetingen met de juiste instructies voor uw gebruikers.
  >

### <a name="view-fraud-reports"></a>Fraude-rapporten weergeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **aanmeldingen**. Het rapport fraude maakt nu deel uit van de standaardrapporten voor Azure AD-aanmeldingen.

## <a name="one-time-bypass"></a>Eenmalige bypass

De _eenmalig overslaan_ functie kunt u een gebruiker zich één keer verifiëren zonder verificatie in twee stappen uitvoeren. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden. In situaties waar de mobiele app of de telefoon niet een melding of automatische oproep ontvangen is, kunt u een eenmalig overslaan toestaan, zodat de gebruiker toegang heeft tot de gewenste resource.

### <a name="create-a-one-time-bypass"></a>Maken van een eenmalig overslaan

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **eenmalig overslaan**.

   ![Eenmalige toegang maken](./media/howto-mfa-mfasettings/onetimebypass.png)

3. Selecteer **Toevoegen**.
4. Selecteer indien nodig de replicatiegroep voor de mogelijkheid tot overslaan.
5. Voer de gebruikersnaam als **gebruikersnaam<span></span>@domain.com**. Voer het aantal seconden dat de mogelijkheid tot overslaan moet duren. Voer de reden voor de mogelijkheid tot overslaan. 
6. Selecteer **Toevoegen**. De tijdslimiet gaat direct in. De gebruiker moet zich aanmelden voordat het eenmalig overslaan verloopt. 

### <a name="view-the-one-time-bypass-report"></a>Bekijk het rapport met eenmalig overslaan

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Active Directory** > **MFA-Server** > **eenmalig overslaan**.

## <a name="custom-voice-messages"></a>Aangepaste spraakberichten
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
1. Blader naar **Azure Active Directory** > **MFA-Server** > **instellingen van telefoongesprekken**.

   ![Record aangepast telefoonnummer berichten](./media/howto-mfa-mfasettings/phonecallsettings.png)

1. Selecteer **begroeting toevoegen**.
1. Kies het type begroeting. 
1. Kies de taal.
1. Selecteer een MP3- of WAV-geluidsbestand uploaden.
1. Selecteer **Toevoegen**.

## <a name="caching-in-azure-multi-factor-authentication"></a>Opslaan in cache in Azure multi-factor Authentication

U kunt instellen dat een bepaalde periode om toe te staan verificatiepogingen nadat een gebruiker is geverifieerd met behulp van de _caching_ functie. Latere verificatiepogingen voor de gebruiker binnen de opgegeven voltooid periode automatisch. Opslaan in cache wordt voornamelijk gebruikt wanneer de on-premises systemen, zoals VPN, verzenden meerdere aanvragen voor verificatie, terwijl de eerste aanvraag nog steeds uitgevoerd wordt. Deze functie kunt de volgende aanvragen voor het automatisch uitgevoerd nadat de gebruiker is geslaagd voor de eerste verificatie wordt uitgevoerd. 

>[!NOTE]
>De functie voor opslaan in cache is niet bedoeld om te worden gebruikt voor aanmeldingen met Azure Active Directory (Azure AD).

### <a name="set-up-caching"></a>Opslaan in cache instellen 

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **regels voor Caching**.

   ![Regels voor caching instellen](./media/howto-mfa-mfasettings/cachingrules.png)

3. Selecteer **Toevoegen**.
4. Selecteer de **cachetype** uit de vervolgkeuzelijst. Geef het maximum aantal **seconden in de cache**. 
5. Indien nodig, selecteert u een verificatietype en geef een toepassing. 
6. Selecteer **Toevoegen**.

## <a name="trusted-ips"></a>Goedgekeurde IP-adressen

De _goedgekeurde IP-adressen_ functie van Azure multi-factor Authentication wordt gebruikt door beheerders van een beheerde of een federatieve-tenant. De functie omzeilt verificatie in twee stappen voor gebruikers die zich aanmelden vanaf het bedrijfsintranet. De functie is beschikbaar met de volledige versie van Azure multi-factor Authentication en niet de gratis versie voor beheerders. Zie voor meer informatie over het ophalen van de volledige versie van Azure multi-factor Authentication [Azure multi-factor Authentication](multi-factor-authentication.md).

Als uw organisatie de NPS-extensie implementeert voor MFA on-premises toepassingen Houd er rekening mee wordt altijd het IP-adres van bron naar de NPS-server de verificatie probeert stromen via worden weergegeven.

| Azure AD-tenant-type | Vertrouwde IP-adressen functieopties |
|:--- |:--- |
| Beheerd |**Specifieke IP-adressen**: beheerders een bereik van IP-adressen die niet kunnen gebruikmaken van verificatie in twee stappen voor gebruikers die zich aanmelden vanaf het bedrijfsintranet opgeven.|
| Federatief |**Alle federatieve gebruikers**: alle federatieve gebruikers die zich aanmelden vanaf binnen de organisatie kunnen verificatie in twee stappen overslaan. De gebruikers omzeilen verificatie met behulp van een claim dat is uitgegeven door Active Directory Federation Services (AD FS).<br/>**Specifieke IP-adressen**: beheerders een bereik van IP-adressen die niet kunnen gebruikmaken van verificatie in twee stappen voor gebruikers die zich aanmelden vanaf het bedrijfsintranet opgeven. |

De goedgekeurde IP-adressen overslaan werkt alleen in het bedrijfsintranet. Als u selecteert de **alle federatieve gebruikers** optie en een gebruiker zich aanmeldt via buiten het bedrijfsintranet, de gebruiker heeft om te verifiëren met behulp van verificatie in twee stappen. Het proces is hetzelfde, zelfs als de gebruiker een AD FS claim geeft. 

**Eindgebruikerservaring binnen bedrijfsnetwerk**

Wanneer de goedgekeurde IP-adressen-functie is uitgeschakeld, is verificatie in twee stappen vereist voor browser-stromen. App-wachtwoorden zijn vereist voor oudere interactieve toepassingen. 

Als de goedgekeurde IP-adressen-functie is ingeschakeld, verificatie in twee stappen is *niet* vereist voor de browser stromen. App-wachtwoorden zijn *niet* vereist zijn voor oudere interactieve toepassingen, mits de gebruiker een app-wachtwoord nog niet is gemaakt. Nadat een app-wachtwoord gebruikt wordt, blijft het wachtwoord vereist. 

**Eindgebruikerservaring buiten corpnet**

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
   
   * **Voor aanvragen van federatieve gebruikers die afkomstig zijn van mijn intranet**: als u wilt deze optie kiest, schakel het selectievakje in. Alle federatieve gebruikers die aanmelden met het bedrijfsnetwerk verificatie in twee stappen overslaan met behulp van een claim dat is uitgegeven door AD FS. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```
     
   * **Voor aanvragen van een specifiek bereik van openbare IP-adressen**: als u wilt deze optie kiest, voert u de IP-adressen in het tekstvak met behulp van de CIDR-notatie.
   
     * Gebruik voor IP-adressen die zich in het bereik xxx.xxx.xxx.1 via xxx.xxx.xxx.254 notatie, zoals **xxx.xxx.xxx.0/24**.
     * Gebruik voor één IP-adres, de notatie, zoals **xxx<span></span>.xxx.xxx.xxx/32**.
     
     Voer maximaal 50 IP-adresbereiken. Gebruikers die zich aanmelden vanaf deze IP-adressen overslaan verificatie in twee stappen.

5. Selecteer **Opslaan**.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>De goedgekeurde IP-adressen inschakelen met behulp van service-instellingen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication, **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **goedgekeurde IP-adressen**, kiest u een (of beide) van de volgende twee opties:
   
   * **Voor aanvragen van federatieve gebruikers op mijn intranet**: als u wilt deze optie kiest, schakel het selectievakje in. Alle federatieve gebruikers die aanmelden met het bedrijfsnetwerk verificatie in twee stappen overslaan met behulp van een claim dat is uitgegeven door AD FS. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```

   * **Voor aanvragen van een opgegeven bereik van IP-subnetten**: als u wilt deze optie kiest, voert u de IP-adressen in het tekstvak met behulp van de CIDR-notatie. 
     
     * Gebruik voor IP-adressen die zich in het bereik xxx.xxx.xxx.1 via xxx.xxx.xxx.254 notatie, zoals **xxx.xxx.xxx.0/24**.
     * Gebruik voor één IP-adres, de notatie, zoals **xxx<span></span>.xxx.xxx.xxx/32**.
     
     Voer maximaal 50 IP-adresbereiken. Gebruikers die zich aanmelden vanaf deze IP-adressen overslaan verificatie in twee stappen.

6. Selecteer **Opslaan**.

![Goedgekeurde IP-adressen met service-instellingen inschakelen](./media/howto-mfa-mfasettings/trustedips3.png)

## <a name="app-passwords"></a>App-wachtwoorden

Sommige toepassingen, zoals Office 2010 of ouder en Apple Mail, ondersteuning geen voor verificatie in twee stappen. De apps die niet zijn geconfigureerd voor het accepteren van een tweede verificatie. Voor het gebruik van deze toepassingen profiteren van de _app-wachtwoorden_ functie. U kunt een app-wachtwoord gebruiken in plaats van uw traditionele wachtwoord om een app voor verificatie in twee stappen overslaan en doorgaan met werken.

Moderne verificatie wordt ondersteund voor de Microsoft Office 2013-clients en hoger. Office 2013 clients, inclusief Outlook, moderne-verificatieprotocollen ondersteunen en kan worden ingeschakeld om te werken met verificatie in twee stappen. Nadat de client is ingeschakeld, app-wachtwoorden niet nodig zijn voor de client.

### <a name="considerations-about-app-passwords"></a>Overwegingen over app-wachtwoorden

Wanneer u app-wachtwoorden, houd rekening met de volgende belangrijke punten:

* App-wachtwoorden worden slechts één keer per toepassing ingevoerd. Gebruikers hebben geen bijhouden van de wachtwoorden of deze elke keer invoeren.
* Het wachtwoord wordt automatisch gegenereerd en niet is opgegeven door de gebruiker. Automatisch gegenereerde wachtwoorden moeilijker voor aanvallers te raden is en is beter te beveiligen.
* Er is een limiet van 40 wachtwoorden per gebruiker. 
* Toepassingen die wachtwoorden in de cache en deze gebruiken in on-premises scenario's kunnen starten mislukt, omdat het app-wachtwoord is niet bekend is buiten het account voor werk of school. Een voorbeeld van dit scenario is Exchange e-mails die zich on-premises, maar de gearchiveerde e-mail is in de cloud. In dit scenario wordt werkt hetzelfde wachtwoord niet.
* Nadat de multi-factor Authentication is ingeschakeld op een gebruikersaccount, kunnen app-wachtwoorden worden gebruikt met de meeste niet-browserclients zoals Outlook en Microsoft Skype voor bedrijven. Beheeracties kunnen niet worden uitgevoerd met behulp van app-wachtwoorden via niet-browsertoepassingen, zoals Windows PowerShell. De acties kunnen niet worden uitgevoerd, zelfs wanneer de gebruiker een Administrator-account heeft. Om uit te voeren PowerShell-scripts, een service-account maken met een sterk wachtwoord en het account voor verificatie in twee stappen niet inschakelen.

>[!WARNING]
>App-wachtwoorden werken niet in hybride omgevingen waarin clients communiceren met zowel on-premises en automatisch opsporen-eindpunten in de cloud. Domeinwachtwoorden zijn vereist voor het verifiëren van on-premises. App-wachtwoorden zijn vereist om te verifiëren met de cloud.
>

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

  ![Met behulp van app-wachtwoorden in een organisatie voor federatieve](./media/howto-mfa-mfasettings/federated.png)

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

   ![Gebruikers toestaan te maken van app-wachtwoorden](./media/howto-mfa-mfasettings/trustedips3.png)

### <a name="create-app-passwords"></a>App-wachtwoorden maken

Gebruikers kunnen app-wachtwoorden maken tijdens de initiële inschrijving. De gebruiker heeft de optie voor het maken van app-wachtwoorden aan het einde van het registratieproces.

Gebruikers kunnen ook app-wachtwoorden maken na de registratie. De app-wachtwoorden kunnen worden gewijzigd via de instellingen in de Azure-portal of de Office 365-portal. Zie voor meer informatie en gedetailleerde stappen voor uw gebruikers [wat zijn app-wachtwoorden in Azure multi-factor Authentication?](../user-help/multi-factor-authentication-end-user-app-passwords.md)

<a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>
## <a name="remember-multi-factor-authentication-for-trusted-devices"></a>Houd er rekening mee multi-factor Authentication voor vertrouwde apparaten
De _multi-factor Authentication onthouden_ functie voor apparaten en browsers die worden vertrouwd door de gebruiker is een gratis functie voor alle gebruikers van multi-factor Authentication. Gebruikers kunnen achteraf gedurende een opgegeven aantal dagen, nadat ze hebben is aangemeld op een apparaat met behulp van multi-factor Authentication overslaan. De functie verbetert de bruikbaarheid door het minimaliseren van het aantal keren dat die een gebruiker heeft verificatie in twee stappen uitvoeren op hetzelfde apparaat.

>[!IMPORTANT]
>Als een account of een apparaat is geknoeid, kan multi-Factor Authentication onthouden voor vertrouwde apparaten beveiliging beïnvloeden. Als een zakelijk account wordt aangetast of als een vertrouwd apparaat is zoekgeraakt of gestolen, moet u [multi-factor Authentication herstellen op alle apparaten](howto-mfa-userdevicesettings.md#restore-mfa-on-all-remembered-devices-for-a-user).
>
>De herstelbewerking trekt u de status van de vertrouwde van alle apparaten en de gebruiker is vereist voor het uitvoeren van verificatie opnieuw uit. U kunt ook vertelt u gebruikers multi-factor Authentication herstellen op hun eigen apparaten met de instructies in [uw instellingen beheren voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted).
>

### <a name="how-the-feature-works"></a>De werking van de functie

De functie van de multi-factor Authentication onthouden stelt een permanente cookies in de browser wanneer een gebruiker selecteert de **niet opnieuw vragen gedurende X dagen** optie bij het aanmelden. De gebruiker is niet opnieuw gevraagd voor meervoudige verificatie van die dezelfde browser totdat de cookie verloopt. Als de gebruiker een andere browser op hetzelfde apparaat opent of hun cookies wist, worden ze opnieuw gevraagd te bevestigen. 

De **niet opnieuw vragen gedurende X dagen** optie op niet-browsertoepassingen, ongeacht of de app ondersteunt met moderne verificatie wordt niet weergegeven. Gebruik van deze apps _vernieuwingstokens_ die elk uur nieuwe toegangstokens bieden. Wanneer een vernieuwingstoken is gevalideerd, wordt Azure AD gecontroleerd dat de laatste verificatie in twee stappen is opgetreden in het opgegeven aantal dagen. 

De functie vermindert het aantal verificaties op web-apps, waarbij elke keer normaal gevraagd. De functie verhoogt het aantal authenticaties voor moderne verificatie-clients die normaal gesproken om de 90 dagen vraagt.

>[!IMPORTANT]
>De **multi-factor Authentication onthouden** functie is niet compatibel met de **aangemeld blijven** functie van AD FS, wanneer gebruikers verificatie voor AD FS via meerdere factoren van Azure uitvoeren Authentication-Server of een oplossing van derden met meervoudige verificatie.
>
>Als uw gebruikers selecteren **aangemeld blijven** op AD FS en ook hun apparaat als vertrouwd voor multi-factor Authentication is ingeschakeld, wordt niet automatisch de gebruiker gecontroleerd nadat de **multi-factor authenticationonthouden**aantal dagen verloopt. Azure AD vraagt een nieuwe verificatie, maar de AD FS retourneert een token met de oorspronkelijke multi-factor Authentication-claim en datum, in plaats presterende verificatie in twee stappen opnieuw. Hiermee stelt u deze reactie uit een lus verificatie tussen Azure AD en AD FS.
>

### <a name="enable-remember-multi-factor-authentication"></a>Schakel multi-factor Authentication onthouden

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication, **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **beheren multi-factor authentication onthouden**, selecteer de **toestaan dat gebruikers multi-factor authentication onthouden op apparaten die ze vertrouwen**optie.

   ![Houd er rekening mee multi-factor Authentication voor vertrouwde apparaten](./media/howto-mfa-mfasettings/remember.png)

6. Stel het aantal dagen om toe te staan van vertrouwde apparaten verificatie in twee stappen overslaan. De standaardwaarde is 14 dagen.
7. Selecteer **Opslaan**.

### <a name="mark-a-device-as-trusted"></a>Een apparaat als vertrouwd is ingeschakeld

Nadat u de multi-factor Authentication onthouden functie hebt ingeschakeld, gebruikers een apparaat als vertrouwd als kunnen markeren ze zich aanmelden door te selecteren **niet opnieuw vragen**.

![Selecteer 'Deze vraag niet' voor vertrouwde apparaten](./media/howto-mfa-mfasettings/trusted.png)

## <a name="selectable-verification-methods"></a>Selecteerbare verificatiemethoden

U kunt de verificatiemethoden die beschikbaar voor uw gebruikers met behulp van zijn de _selecteerbare verificatiemethoden_ functie. De volgende tabel geeft een kort overzicht van de methoden.

Wanneer uw gebruikers hun account voor Azure multi-factor Authentication registreren, kiezen ze hun gewenste verificatiemethode uit de opties die u hebt ingeschakeld. Richtlijnen voor het registratieproces voor de gebruiker is opgegeven in [instellen van mijn account voor verificatie in twee stappen](../user-help/multi-factor-authentication-end-user-first-time.md).

| Methode | Beschrijving |
|:--- |:--- |
| Bellen naar telefoon |Plaatst een geautomatiseerd telefoongesprek. De gebruiker beantwoordt het gesprek en drukt # in het toetsenblok van de telefoon om te verifiëren. Het telefoonnummer is niet gesynchroniseerd met on-premises Active Directory. |
| Sms-bericht naar telefoon |Verzendt een SMS-bericht met een verificatiecode. De gebruiker wordt gevraagd de verificatiecode invoeren in de interface van aanmelding. Dit proces heet SMS in één richting. SMS in twee richtingen betekent dat de gebruiker tekst weer een bepaalde code moet. SMS in twee richtingen is afgeschaft en wordt niet ondersteund na 14 November 2018. Gebruikers die zijn geconfigureerd voor SMS in twee richtingen worden automatisch overgeschakeld naar de _oproep naar telefoon_ verificatie op dat moment.|
| Melding via mobiele app |Verzendt een pushmelding naar uw telefoon of geregistreerd apparaat. De gebruiker de melding weergaven en **controleren** om verificatie te voltooien. De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Verificatiecode via mobiele app |De Microsoft Authenticator-app genereert een nieuwe OATH-verificatiecode elke 30 seconden. De gebruiker voert de verificatiecode in de interface van aanmelding. De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="enable-and-disable-verification-methods"></a>In- en uitschakelen van verificatiemethoden

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication, **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **verificatieopties**, selecteer/Hef de selectie van de methoden om te bieden aan uw gebruikers.

   ![Selecteer de verificatiemethoden](./media/howto-mfa-mfasettings/authmethods.png)

6. Klik op **Opslaan**.
