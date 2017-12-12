---
title: Azure MFA configureren | Microsoft Docs
description: Dit is de pagina van de Azure multi-factor authentication waarop wordt beschreven wat u vervolgens kunt doen met MFA.  Dit omvat rapporten, fraudewaarschuwing, eenmalig overslaan, aangepaste spraakberichten caching, vertrouwde IP-adressen en app-wachtwoorden.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 4dc029e01762d9ab6eb1c930f2941d0c1ac8ed7d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure multi-factor Authentication-instellingen configureren

Dit artikel helpt u bij het beheren van Azure multi-factor Authentication nu dat u actief zijn.  Deze heeft verschillende onderwerpen die u helpen bij het Maak optimaal gebruik van Azure multi-factor Authentication.  Niet al deze functies zijn beschikbaar in elke [versie van Azure multi-factor Authentication](/multi-factor-authentication-get-started.md#what-features-do-i-need).

| Functie | Beschrijving | 
|:--- |:--- |
| [Blokkeren en blokkering van gebruikers](#block-and-unblock) |Gebruikers blokkeren/blokkering opheffen kunnen voorkomen dat gebruikers verificatieaanvragen ontvangt. |
| [Fraudewaarschuwing](#fraud-alert) |Fraudewaarschuwing worden geconfigureerd en zo instellen dat uw gebruikers frauduleuze probeert te krijgen tot hun bronnen kunnen rapporteren. |
| [Eenmalig overslaan](#one-time-bypass) |Eenmalig overslaan kan een gebruiker één keer verifiëren door te multi-factor authentication 'negeren'. |
| [Aangepaste spraakberichten](#custom-voice-messages) |Aangepaste spraakberichten kunnen u uw eigen opnamen of begroetingen met meervoudige verificatie gebruiken. |
| [Opslaan in cache](#caching-in-azure-multi-factor-authentication) |In cache opslaan, kunt u een specifiek tijdstip periode zo instellen dat latere authenticatiepogingen automatisch laten slagen. |
| [Goedgekeurde IP-adressen](#trusted-ips) |Beheerders van een beheerd of federatieve tenant kunnen goedgekeurde IP-adressen gebruiken voor het overslaan van de verificatie in twee stappen voor gebruikers die zich via Lokaal intranet van het bedrijf aanmelden. |
| [App-wachtwoorden](#app-passwords) |Een app-wachtwoord kan een toepassing die niet MFA bewust multi-factor authentication overslaan en doorgaan met werken. |
| [Houd er rekening mee multi-factor Authentication voor onthouden apparaten en browsers](#remember-multi-factor-authentication-for-devices-that-users-trust) |Hiermee kunt u apparaten onthouden voor een bepaald aantal dagen nadat een gebruiker heeft aangemeld met MFA. |
| [Selecteerbare verificatiemethoden](#selectable-verification-methods) |Kunt u de verificatiemethoden die beschikbaar zijn voor gebruikers om te gebruiken. |

## <a name="block-and-unblock"></a>Blokkeren en blokkering
Gebruikers blokkeren/blokkering kunnen worden gebruikt om te voorkomen dat gebruikers ontvangen van verificatieaanvragen. Alle verificatiepogingen voor geblokkeerde gebruikers worden automatisch geweigerd. Geblokkeerde gebruikers blijven geblokkeerd voor 90 dagen vanaf het moment dat ze worden geblokkeerd.

### <a name="block-a-user"></a>Een gebruiker blokkeren
1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **MFA-Server** > **gebruikers blokkeren/blokkering**.
3. Klik op **toevoegen** om een gebruiker te blokkeren.
4. Selecteer de **replicatiegroep**, de geblokkeerde gebruikersnaam als invoer  **username@domain.com** , en voer een opmerking in het **reden** veld.
5. Klik op **toevoegen** voltooid blokkeren van de gebruiker.

### <a name="unblock-a-user"></a>Een gebruiker de blokkering opheffen
1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **MFA-Server** > **gebruikers blokkeren/blokkering**.
3. Klik op **blokkering** in de **actie** kolom naast de gebruiker die u wilt deblokkeren.
4. Voer een opmerking in het **reden voor het opheffen van de blokkering** veld.
5. Klik op **blokkering** voltooid deblokkeren van de gebruiker.

## <a name="fraud-alert"></a>Fraudewaarschuwing
Fraudewaarschuwing worden geconfigureerd en zo instellen dat uw gebruikers frauduleuze probeert te krijgen tot hun bronnen kunnen rapporteren.  Gebruikers kunnen rapporteren van fraude met de mobiele app of via hun telefoon.

### <a name="turn-on-fraud-alert"></a>Fraudewaarschuwing inschakelen
1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **MFA-Server** > **fraudewaarschuwing**.

   ![Fraudewaarschuwing](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Schakel **dat gebruikers kunnen indienen Fraudewaarschuwingen** naar **op**.
4. Selecteer **Opslaan**.

### <a name="configuration-options"></a>Configuratie-opties

- **Gebruiker blokkeren wanneer fraude wordt gemeld** : als een gebruiker rapporten fraude, hun account is geblokkeerd tot 90 dagen of totdat een beheerder blokkering hun account opgeheven. Een beheerder kan aanmeldingen met behulp van het rapport aanmelden controleren en onderneem gepaste actie om te voorkomen dat toekomstige fraude. Vervolgens kan een beheerder [deblokkeren](#unblock-a-user) account van de gebruiker.
- **Code voor het melden van fraude tijdens de eerste begroeting** : wanneer gebruikers een telefoonoproep om uit te voeren verificatie in twee stappen, krijgen ze normaal druk op # om te bevestigen dat hun aanmelden. Als ze fraude rapporteren wilt, invoeren ze een code voordat u op #. Deze code is **0** standaard, maar u kunt deze aanpassen.

> [!NOTE]
> Microsoft standaard gesproken begroetingen vertelt gebruikers op 0# verzenden van een fraudewaarschuwing voor te drukken. Als u wilt met een code dan 0, moet u registreren en uw eigen aangepaste gesproken begroetingen met instructies voor het juiste uploaden.

### <a name="view-fraud-reports"></a>Fraude-rapporten weergeven
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer aan de linkerkant **Active Directory**.
3. Selecteer de map die u wilt beheren. 
4. Selecteer **configureren**
5. Selecteer onder multi-factor Authentication **service-instellingen beheren**.
6. Selecteer onderaan op de pagina Service-instellingen, **gaat u naar de portal**.
7. In de Azure multi-factor Authentication-beheerportal, onder A-rapport weergeven, klikt u op **fraudewaarschuwing**.
8. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook gebruikersnamen, telefoonnummers en de status van de gebruiker opgeven.
9. Klik op **uitvoeren** online zetten van een rapport van Fraudewaarschuwingen. Klik op **exporteren naar CSV** als u wilt exporteren van het rapport.

## <a name="one-time-bypass"></a>Eenmalig overslaan
Eenmalig overslaan kan een gebruiker één keer verifiëren zonder verificatie in twee stappen uitvoeren. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden. In situaties waar de mobiele app of de telefoon geen een melding of telefoongesprek ontvangt, kunt u een eenmalig overslaan inschakelen zodat de gebruiker toegang heeft tot de gewenste resource.

### <a name="create-a-one-time-bypass"></a>Maken van een eenmalig overslaan

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **MFA-Server** > **eenmalig overslaan**.

   ![Eenmalig overslaan](./media/multi-factor-authentication-whats-next/onetimebypass.png)
3. Selecteer **Toevoegen**.
4. Selecteer indien nodig, de replicatiegroep voor deze overslaan.
5. Geef de gebruikersnaam (in de vorm van username@domain.com), wordt het aantal seconden dat het overslaan voor moet duren en de reden voor het overslaan. 
6. Selecteer **Toevoegen**. De tijdslimiet wordt ingevoerd onmiddellijk, zodat de gebruiker moet zich aanmelden voordat het eenmalig overslaan verloopt. 

### <a name="view-the-one-time-bypass-report"></a>Het rapport eenmalig overslaan weergeven
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer aan de linkerkant **Active Directory**.
3. Selecteer de map die u wilt beheren. 
4. Selecteer **configureren**
5. Selecteer onder multi-factor Authentication **service-instellingen beheren**.
6. Selecteer onderaan op de pagina Service-instellingen, **gaat u naar de portal**.
7. In de Azure multi-factor Authentication-beheerportal, onder A-rapport weergeven, klikt u op **eenmalige**.
8. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook gebruikersnamen, telefoonnummers en de status van de gebruiker opgeven.
9. Klik op **uitvoeren** online zetten van een rapport van omleidingen. Klik op **exporteren naar CSV** als u wilt exporteren van het rapport.

## <a name="custom-voice-messages"></a>Aangepaste spraakberichten
Aangepaste spraakberichten kunnen u uw eigen opnamen of begroetingen gebruiken voor verificatie in twee stappen. Deze kunnen worden gebruikt als aanvulling op of ter vervanging van de Microsoft registreert.

Voordat u begint rekening mee worden met de volgende beperkingen:

* Ondersteunde bestandsindelingen zijn .wav en .mp3.
* De maximale bestandsgrootte is 5 MB.
* Verificatieberichten moet korter zijn dan 20 seconden. Meer dan 20 seconden kan ervoor zorgen dat de verificatie is mislukt, omdat de gebruiker niet reageert voordat het bericht is voltooid, waardoor de verificatie time-out.

### <a name="set-up-a-custom-message"></a>Een aangepast bericht instellen

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **MFA-Server** > **telefoongesprek instellingen**.

   ![Instellingen voor telefonische oproep](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Selecteer **wenskaart toevoegen**.
4. Kies het type begroeting en de taal.
5. Selecteer een MP3- of .wav geluidsbestand uploaden.
6. Selecteer **Toevoegen**.

## <a name="caching-in-azure-multi-factor-authentication"></a>Opslaan in cache in Azure multi-factor Authentication
In cache opslaan, kunt u een specifiek tijdstip periode zo instellen dat latere authenticatiepogingen binnen deze periode automatisch laten slagen. Dit is vooral wanneer on-premises systemen zoals VPN meerdere aanvragen voor verificatie verzenden bij de eerste aanvraag nog in voortgang is gebruikt. Caching, kunnen de volgende aanvragen automatisch mislukt nadat de eerste verificatie uitgevoerd door de gebruiker is geslaagd. 

Opslaan in cache is niet bedoeld om te worden gebruikt voor aanmeldingen bij Azure AD.

### <a name="set-up-caching"></a>Opslaan in cache instellen 
1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **MFA-Server** > **regels opslaan in cache**.

   ![Regels opslaan in cache](./media/multi-factor-authentication-whats-next/cachingrules.png)

4. Selecteer **Toevoegen**.
5. Selecteer het cachetype in de vervolgkeuzelijst en geef het aantal max. aantal cache-seconden. 
6. Indien nodig, een verificatietype te selecteren en een toepassing opgeven. 
7. Selecteer **Toevoegen**.


## <a name="trusted-ips"></a>Goedgekeurde IP-adressen
Goedgekeurde dat IP-adressen is een functie van Azure MFA die beheerders van een tenant beheerd of federatieve gebruiken kunnen voor het overslaan van de verificatie in twee stappen voor gebruikers die vanaf een lokaal intranet van het bedrijf aanmelden zich. Deze functie is beschikbaar met de volledige versie van Azure multi-factor Authentication, niet de gratis versie voor beheerders. Zie voor meer informatie over het verkrijgen van de volledige versie van Azure multi-factor Authentication [Azure multi-factor Authentication](multi-factor-authentication.md).

| Type van Azure AD-Tenant | Beschikbare goedgekeurde IP-opties |
|:--- |:--- |
| Managed |<li>Specifiek IP-adresbereiken: beheerders kunnen een bereik met IP-adressen die verificatie in twee stappen voor gebruikers die vanaf het bedrijfsintranet aanmelden zich omzeilen opgeven.</li> |
| Federatief |<li>Alle federatieve gebruikers: alle federatieve gebruikers die vanaf binnen de organisatie aanmelden zich wordt verificatie in twee stappen met behulp van een claim uitgegeven door AD FS overslaan.</li><br><li>Specifiek IP-adresbereiken: beheerders kunnen een bereik met IP-adressen die verificatie in twee stappen voor gebruikers die vanaf het bedrijfsintranet aanmelden zich omzeilen opgeven. |

Dit werkt alleen in binnen het intranet van een bedrijf overslaan. Als u alle federatieve gebruikers hebt geselecteerd, en een gebruiker zich aanmeldt via buiten het bedrijfsintranet, moet die de gebruiker heeft bijvoorbeeld om te verifiëren met behulp van verificatie in twee stappen, zelfs als de gebruiker een AD FS-claim aanbiedt. 

**De eindgebruikerservaring voor binnen corpnet:**

Wanneer de goedgekeurde IP-adressen is uitgeschakeld, verificatie in twee stappen is vereist voor de browser stromen en app-wachtwoorden vereist zijn voor oudere interactieve apps. 

Wanneer de goedgekeurde IP-adressen is ingeschakeld, wordt verificatie in twee stappen is *niet* vereist voor browser-stromen en app-wachtwoorden zijn *niet* vereist voor oudere rijke ClientApps, mits de gebruiker al een app-wachtwoord nog niet is gemaakt. Nadat een app-wachtwoord gebruikt wordt, blijft deze vereiste. 

**Eindgebruikerservaring buiten corpnet:**

Verificatie in twee stappen is vereist voor de browser stromen of goedgekeurde IP-adressen is ingeschakeld of niet, en app-wachtwoorden vereist zijn voor oudere interactieve apps. 

### <a name="enable-named-locations-using-conditional-access"></a>Benoemde locaties met behulp van voorwaardelijke toegang inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **voorwaardelijke toegang** > **locaties met de naam**
3. Selecteer **nieuwe locatie**
4. Geef een naam voor de locatie
5. Selecteer **markeren als vertrouwde locatie**
6. Geef het IP-adresbereik in CIDR-notatie (voorbeeld 192.168.1.1/24)
7. Selecteer **maken**

### <a name="enable-trusted-ips-using-conditional-access"></a>Goedgekeurde IP-adressen met behulp van voorwaardelijke toegang inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **voorwaardelijke toegang** > **locaties met de naam**
3. Selecteer **MFA configureren goedgekeurde IP-adressen**
4. Op de pagina Service-instellingen onder goedgekeurde IP-adressen, hebt u twee opties:
   
   * **Voor aanvragen van federatieve gebruikers die afkomstig zijn van mijn intranet** : Schakel het selectievakje in. Alle federatieve gebruikers die vanaf het bedrijfsnetwerk aanmelden zich wordt verificatie in twee stappen met behulp van een claim uitgegeven door AD FS overslaan. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS: ' c: [Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] = > issue(claim = c); "

   * **Voor aanvragen van een specifiek bereik van openbare IP-adressen** – Geef de IP-adressen in het tekstvak opgegeven CIDR-notatie. Bijvoorbeeld: xxx.xxx.xxx.0/24 voor IP-adressen in het bereik xxx.xxx.xxx.1 – xxx.xxx.xxx.254 of xxx.xxx.xxx.xxx/32 voor een enkel IP-adres. U kunt maximaal 50 IP-adresbereiken. Gebruikers die zich via deze IP-adressen aanmelden overslaan verificatie in twee stappen.
5. Selecteer **Opslaan**.

### <a name="enable-trusted-ips-using-service-settings"></a>Goedgekeurde IP-adressen met behulp van de service-instellingen inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**
3. Selecteer **Multi-Factor Authentication**
4. Selecteer onder multi-factor Authentication **service-instellingen**.
5. Op de pagina Service-instellingen onder goedgekeurde IP-adressen, hebt u twee opties:
   
   * **Voor aanvragen van federatieve gebruikers die afkomstig zijn van mijn intranet** : Schakel het selectievakje in. Alle federatieve gebruikers die vanaf het bedrijfsnetwerk aanmelden zich wordt verificatie in twee stappen met behulp van een claim uitgegeven door AD FS overslaan. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS: ' c: [Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] = > issue(claim = c); "

   * **Voor aanvragen van een specifiek bereik van openbare IP-adressen** – Geef de IP-adressen in het tekstvak opgegeven CIDR-notatie. Bijvoorbeeld: xxx.xxx.xxx.0/24 voor IP-adressen in het bereik xxx.xxx.xxx.1 – xxx.xxx.xxx.254 of xxx.xxx.xxx.xxx/32 voor een enkel IP-adres. U kunt maximaal 50 IP-adresbereiken. Gebruikers die zich via deze IP-adressen aanmelden overslaan verificatie in twee stappen.
6. Selecteer **Opslaan**.

![Goedgekeurde IP-adressen](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Appwachtwoorden
Bepaalde apps, zoals Office 2010 of ouder en Apple Mail ondersteunen geen verificatie in twee stappen. Ze zijn niet geconfigureerd voor het accepteren van een tweede verificatie. Voor het gebruik van deze apps, moet u 'app-wachtwoorden' gebruiken in plaats van uw traditionele wachtwoord. Het app-wachtwoord kunt de toepassing voor verificatie in twee stappen overslaan en doorgaan met werken.

> [!NOTE]
> Moderne verificatie voor de Office 2013-Clients
> 
> Office 2013-clients (inclusief Outlook) en nieuwere ondersteuning moderne verificatieprotocollen en kunnen worden ingeschakeld voor gebruik met verificatie in twee stappen. Eenmaal is ingeschakeld, zijn app-wachtwoorden niet vereist voor deze clients.  Zie voor meer informatie [Office 2013 modern authentication openbare preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Belangrijk om te weten over app-wachtwoorden
Hier volgt een lijst met belangrijke dingen die u over app-wachtwoorden weten moet.

* App-wachtwoorden moeten slechts één keer per app worden opgegeven. Gebruikers hoeven te volgen en deze elke keer invoeren.
* Het wachtwoord wordt automatisch gegenereerd en is niet geleverd door de gebruiker. Dit is omdat de automatisch gegenereerde wachtwoorden moeilijker voor aanvallers om aan te raden en veiliger.
* Er is een limiet van 40 wachtwoorden per gebruiker. 
* Apps die wachtwoorden van de cache en worden gebruikt in scenario's voor lokale kunnen mislukken omdat het app-wachtwoord is niet bekend is buiten de organisatie-id. Een voorbeeld e-mailberichten voor het Exchange die zich on-premises maar gearchiveerde e-mail is in de cloud. Hetzelfde wachtwoord werkt niet.
* Zodra multi-factor authentication-server is ingeschakeld voor een gebruikersaccount, kunnen de app-wachtwoorden worden gebruikt met de meeste niet-browserclients zoals Outlook en Lync. Beheeracties kunnen niet worden uitgevoerd met behulp van app-wachtwoorden via niet-browsertoepassingen zoals Windows PowerShell, zelfs als die gebruiker over een Administrator-account.  Een serviceaccount maken met een sterk wachtwoord om uit te voeren PowerShell-scripts en dat account voor verificatie in twee stappen niet inschakelt.

> [!WARNING]
> App-wachtwoorden werken niet in hybride omgevingen waarin clients communiceren met zowel on-premises en in de cloud autodiscover-eindpunten. Dit is omdat domeinwachtwoorden vereist zijn voor de verificatie van lokale en app-wachtwoorden nodig zijn om te verifiëren met de cloud.

### <a name="naming-guidance-for-app-passwords"></a>Richtlijnen voor App-wachtwoorden voor de naamgeving
Namen van App-wachtwoord moeten overeenkomen met het apparaat waarop ze worden gebruikt. Als u een laptop met niet-browsertoepassingen zoals Outlook, Word en Excel hebt, bijvoorbeeld één app-wachtwoord met de naam Laptop maken en dit app-wachtwoord gebruiken in deze toepassingen. Vervolgens maakt u een andere app-wachtwoord bureaublad met de naam voor de toepassingen op uw pc. 

Microsoft raadt u aan één appwachtwoord per apparaat, niet één appwachtwoord per toepassing maken.

### <a name="federated-sso-app-passwords"></a>App-wachtwoorden voor federatieve (SSO)
Azure AD biedt ondersteuning voor federatie (eenmalige aanmelding) met het lokale Windows Server Active Directory Domain Services (AD DS). Als uw organisatie is gefedereerd met Azure AD en u wilt gebruikmaken van Azure multi-factor Authentication, zijn de volgende informatie over app-wachtwoorden is belangrijk voor u. Deze sectie is alleen van toepassing op klanten van federatieve (SSO).

* App-wachtwoorden worden geverifieerd door Azure AD en daarom federation overslaan. Federatie wordt alleen actief gebruikt bij het instellen van app-wachtwoorden.
* Voor federatieve gebruikers (SSO), de id-Provider (IdP) is geen contact opgenomen met, in tegenstelling tot de passieve stroom. De wachtwoorden worden opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, heeft dat gegevens naar de organisatie-id met behulp van DirSync in realtime stromen. Account uitschakelen/verwijderen kan duren tot drie uur te synchroniseren, vertragen uitschakelen/verwijderen van App-wachtwoord in Azure AD.
* On-premises instellingen voor toegangsbeheer van client worden niet herkend door het app-wachtwoord.
* Er is geen lokale verificatie mogelijkheid logboekregistratie/controle is beschikbaar voor App-wachtwoord.
* Bepaalde geavanceerde architectuur ontwerpen moet mogelijk een combinatie van organisatie-gebruikersnaam en wachtwoorden app bij het gebruik van verificatie in twee stappen met clients, afhankelijk van waar ze verifiëren. Voor clients die worden geverifieerd bij een on-premises infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij Azure AD, gebruikt u het app-wachtwoord.

  Stel bijvoorbeeld dat u hebt een architectuur die uit het volgende bestaat:

  * U federeert uw lokale exemplaar van Active Directory met Azure AD
  * U via Exchange online
  * U gebruikt Lync specifiek on-premises
  * U gebruikt Azure multi-factor Authentication

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  In dergelijke gevallen moet u het volgende doen:

  * Bij het aanmelden bij Lync, gebruiken de gebruikersnaam en wachtwoord van uw organisatie.
  * Wanneer u probeert te krijgen tot het adresboek via een Outlook-client die is verbonden met Exchange online, gebruikt u een app-wachtwoord.

### <a name="allow-app-password-creation"></a>Maken van app-wachtwoord toestaan
Gebruikers kunnen geen app-wachtwoorden maken standaard. Deze functie moet zijn ingeschakeld. Als u wilt toestaan dat gebruikers de mogelijkheid te maken van app-wachtwoorden, gebruikt u de volgende procedure:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**
3. Selecteer **Multi-Factor Authentication**
4. Selecteer onder multi-factor Authentication **service-instellingen**.
6. Schakel het keuzerondje naast **gebruikers toestaan te maken van app-wachtwoorden voor aanmelding bij niet-browsertoepassingen**.

![App-wachtwoorden maken](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>App-wachtwoorden maken
Gebruikers kunnen app-wachtwoorden maken tijdens de initiële inschrijving. Een optie aan het einde van het registratieproces waarmee ze appwachtwoorden te maken, wordt verstrekt.

Gebruikers kunnen ook app-wachtwoorden maken na de registratie door hun instellingen in de Azure portal of de Office 365-portal te wijzigen. Zie voor meer informatie en gedetailleerde stappen voor uw gebruikers [wat zijn app-wachtwoorden in Azure multi-factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Houd er rekening mee multi-factor Authentication voor apparaten die gebruikers vertrouwen
Onthoud multi-factor Authentication voor apparaten en browsers dat gebruikers vertrouwensrelatie een gratis functie voor alle gebruikers van MFA is. Deze instelling geeft u gebruikers de mogelijkheid voor het overslaan van MFA voor een bepaald aantal dagen na het uitvoeren van een geslaagde aanmelden met MFA. Dit kan bruikbaarheid verbeteren door het aantal keren dat een gebruiker verificatie in twee stappen op hetzelfde apparaat uitvoeren kan minimaliseren.

Echter, als een account of apparaat is geknoeid, onthoud MFA voor vertrouwde apparaten kan invloed hebben op beveiliging. Als een zakelijke account wordt aangetast of een vertrouwd apparaat is zoekgeraakt of gestolen, moet u [multi-factor Authentication herstellen op alle apparaten](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Deze actie worden de vertrouwde status van alle apparaten ingetrokken en de gebruiker is vereist om opnieuw verificatie in twee stappen uitvoeren. U kunt ook instrueert u uw gebruikers MFA om op te herstellen met de instructies in hun eigen apparaten [beheren van uw instellingen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Hoe werkt het?

Onthouden van multi-factor Authentication werkt door het instellen van een permanente cookie op de browser wanneer een gebruiker controleert de ' niet opnieuw vragen voor **X** dagen ' vak bij het aanmelden. De gebruiker niet gevraagd voor MFA opnieuw van die browser totdat de cookie verloopt. Als de gebruiker een andere browser op hetzelfde apparaat opent of hun cookies wist, wordt deze gevraagd om te controleren of opnieuw. 

De ' niet opnieuw vragen voor **X** dagen ' checkbox wordt niet weergegeven op niet-browsertoepassingen, al dan niet ondersteuning van moderne verificatie. Deze apps gebruik vernieuwen van tokens die nieuwe toegangstokens om het uur leveren. Wanneer een vernieuwingstoken is gevalideerd, Azure AD-controles uitgevoerd van de laatste tijd in twee stappen verificatie valt binnen het geconfigureerde aantal dagen. 

Daarom MFA onthouden op vertrouwde apparaten beperkt het aantal authenticaties op web-apps (die normaal gesproken vragen telkens) maar verhoogt het aantal authenticaties voor verificatieclients (die normaal gesproken vragen om de 90 dagen).

> [!NOTE]
>Deze functie is niet compatibel met de functie 'Aangemeld blijven' van AD FS wanneer gebruikers verificatie in twee stappen voor AD FS via de Azure MFA-Server of een MFA-oplossing van derden uitvoeren. Als uw gebruikers "Aangemeld blijven" Selecteer op de AD FS en ook hun apparaat als vertrouwd voor MFA markeren, is ze niet kunnen verifiëren nadat het 'MFA onthouden' aantal dagen is verlopen. Azure AD de verificatie van een nieuwe in twee stappen aanvraagt, maar de AD FS retourneert een token met het oorspronkelijke MFA claim en de datum in plaats van het uitvoeren van verificatie in twee stappen weer. Hiermee stelt u uit een lus verificatie tussen Azure AD en AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Onthoud dat multi-factor authentication inschakelen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**
3. Selecteer **Multi-Factor Authentication**
4. Selecteer onder multi-factor Authentication **service-instellingen**.
5. Op de pagina Service-instellingen onder **beheren onthouden multi-factorauthenticatie**, Controleer de **gebruikers toestaan om te onthouden meervoudige verificatie op vertrouwde apparaten** vak.

   ![Onthouden van apparaten](./media/multi-factor-authentication-whats-next/remember.png)

6. Stel het aantal dagen dat u wilt toestaan dat de vertrouwde apparaten verificatie in twee stappen overslaan. De standaardwaarde is 14 dagen.
7. Selecteer **Opslaan**.

### <a name="mark-a-device-as-trusted"></a>Een apparaat als vertrouwd is ingeschakeld

Zodra u deze functie inschakelt, gebruikers wilt markeren een apparaat als vertrouwde wanneer ze zich aanmelden door te controleren **niet opnieuw vragen**.

![Niet opnieuw vragen - schermafbeelding](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Selecteerbare verificatiemethoden
U kunt kiezen welke verificatiemethoden zijn beschikbaar voor uw gebruikers. De volgende tabel geeft een kort overzicht van elke methode.

Wanneer uw gebruikers hun account voor MFA registreren, kiest u de methode voorkeursoptie voor verificatie buiten de opties die u hebt ingeschakeld. De richtlijnen voor het registratieproces wordt beschreven in [Mijn account voor verificatie in twee stappen instellen](multi-factor-authentication-end-user-first-time.md)

| Methode | Beschrijving |
|:--- |:--- |
| Bellen naar telefoon |Een geautomatiseerd telefoongesprek plaatst. De gebruiker beantwoordt het gesprek en drukt # in op de toetsenblok van de telefoon om te verifiëren. Dit telefoonnummer is niet gesynchroniseerd met lokale Active Directory. |
| Sms-bericht naar telefoon |Verzendt een SMS-bericht met een verificatiecode. De gebruiker wordt gevraagd om de verificatiecode invoeren in de interface voor aanmelden. Dit proces heet SMS in één richting. SMS in twee richtingen betekent dat de gebruiker tekst weer een bepaalde code moet. SMS in twee richtingen is gedeprecieerd en wordt niet meer wordt ondersteund vanaf 14 November 2018. Gebruikers die zijn geconfigureerd voor SMS in twee richtingen wordt automatisch kan worden overgeschakeld naar 'oproep naar telefoon' verificatie op dat moment.|
| Melding via mobiele app |Een pushmelding verzendt naar uw telefoon of geregistreerd apparaat. De gebruiker de melding weergaven en **controleren** om verificatie te voltooien. <br>De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Verificatiecode via mobiele app |De Microsoft Authenticator-app genereert elke 30 seconden een nieuwe OATH-verificatiecode uit. De gebruiker voert deze bevestigingscode in de interface voor aanmelden.<br>De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Hoe verificatiemethoden in-of uitschakelen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**
3. Selecteer **Multi-Factor Authentication**
4. Selecteer onder multi-factor Authentication **service-instellingen**.
5. Op de pagina Service-instellingen onder **verificatie-opties**, selecteren/uitschakelen van de opties die u wilt gebruiken.

   ![Verificatie-opties](./media/multi-factor-authentication-whats-next/authmethods.png)

6. Klik op **Opslaan**.
