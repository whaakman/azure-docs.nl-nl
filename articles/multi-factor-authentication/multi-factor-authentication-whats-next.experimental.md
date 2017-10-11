---
title: Azure MFA configureren | Microsoft Docs
description: Dit is de pagina van de Azure multi-factor authentication waarop wordt beschreven wat u vervolgens kunt doen met MFA.  Dit omvat rapporten, fraudewaarschuwing, eenmalig overslaan, aangepaste spraakberichten caching, vertrouwde IP-adressen en app-wachtwoorden.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: kgremban
ms.openlocfilehash: 9d77b9329116afcf2fdde48d672c95020738138c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure multi-factor Authentication-instellingen configureren
Dit artikel helpt u bij het beheren van Azure multi-factor Authentication nu dat u actief zijn.  Deze heeft verschillende onderwerpen die u helpen bij het Maak optimaal gebruik van Azure multi-factor Authentication.  Niet al deze functies zijn beschikbaar in elke versie van Azure multi-factor Authentication.

| Functie | Beschrijving | 
|:--- |:--- |
| [Fraudewaarschuwing](#fraud-alert) |Fraudewaarschuwing worden geconfigureerd en zo instellen dat uw gebruikers frauduleuze probeert te krijgen tot hun bronnen kunnen rapporteren. |
| [Eenmalig overslaan](#one-time-bypass) |Eenmalig overslaan kan een gebruiker één keer verifiëren door te multi-factor authentication 'negeren'. |
| [Aangepaste spraakberichten](#custom-voice-messages) |Aangepaste spraakberichten kunnen u uw eigen opnamen of begroetingen met meervoudige verificatie gebruiken. |
| [Opslaan in cache](#caching-in-azure-multi-factor-authentication) |In cache opslaan, kunt u een specifiek tijdstip periode zo instellen dat latere authenticatiepogingen automatisch laten slagen. |
| [Goedgekeurde IP-adressen](#trusted-ips) |Beheerders van een beheerd of federatieve tenant kunnen goedgekeurde IP-adressen gebruiken voor het overslaan van de verificatie in twee stappen voor gebruikers die zich via Lokaal intranet van het bedrijf aanmelden. |
| [App-wachtwoorden](#app-passwords) |Een app-wachtwoord kan een toepassing die niet MFA bewust multi-factor authentication overslaan en doorgaan met werken. |
| [Houd er rekening mee multi-factor Authentication voor onthouden apparaten en browsers](#remember-multi-factor-authentication-for-devices-that-users-trust) |Hiermee kunt u apparaten onthouden voor een bepaald aantal dagen nadat een gebruiker heeft aangemeld met MFA. |
| [Selecteerbare verificatiemethoden](#selectable-verification-methods) |Kunt u de verificatiemethoden die beschikbaar zijn voor gebruikers om te gebruiken. |

## <a name="access-the-azure-mfa-management-portal"></a>Toegang tot de Azure MFA-beheerportal

De functies die in dit artikel worden geconfigureerd in de Azure multi-factor Authentication-beheerportal. Er zijn twee manieren toegang krijgen tot de MFA-beheerportal via de klassieke Azure portal. De eerste is door het beheer van een multi-factor Authentication-Provider. De tweede is via de MFA-service-instellingen. 

### <a name="use-an-authentication-provider"></a>Gebruik een verificatieprovider

Als u een multi-factor Authentication-Provider voor meervoudige verificatie op basis van verbruik gebruikt, moet u deze methode gebruiken voor toegang tot de beheerportal.

Voor toegang tot de MFA-beheerportal via een Azure multi-factor Authentication-Provider, meld u aan bij de klassieke Azure portal als beheerder en selecteer de optie Active Directory. Klik op de **multi-factor Auth-Providers** tabblad Selecteer vervolgens de map en klik op de **beheren** knop onderaan.

### <a name="use-the-mfa-service-settings-page"></a>Gebruik de pagina MFA-Service-instellingen 

Als u een van de volgende licenties hebt, kunt u de pagina MFA-Service-instellingen:
- Azure MFA
- Azure AD Premium 
- Enterprise Mobility + Security

Meld u aan bij de klassieke Azure portal als beheerder voor toegang tot de MFA-beheerportal via de pagina MFA-Service-instellingen, en selecteer de optie Active Directory. Klik op uw directory en klik vervolgens op het tabblad **Configureren**. Selecteer in de sectie Multi-Factor Authentication de optie **Service-instellingen beheren**. Klik onder aan de pagina met MFA Service-instellingen op de koppeling **Naar de portal**.


## <a name="fraud-alert"></a>Fraudewaarschuwing
Fraudewaarschuwing worden geconfigureerd en zo instellen dat uw gebruikers frauduleuze probeert te krijgen tot hun bronnen kunnen rapporteren.  Gebruikers kunnen rapporteren van fraude met de mobiele app of via hun telefoon.

### <a name="set-up-fraud-alert"></a>Fraudewaarschuwing instellen
1. Navigeer naar de MFA-beheerportal per de instructies boven aan deze pagina.
2. Klik in de Azure multi-factor Authentication-beheerportal op **instellingen** onder de sectie configureren.
3. Controleer onder de fraudewaarschuwing sectie van de pagina instellingen en de **dat gebruikers kunnen indienen Fraudewaarschuwingen** selectievakje.
4. Selecteer **opslaan** uw wijzigingen toe te passen. 

### <a name="configuration-options"></a>Configuratie-opties

- **Gebruiker blokkeren wanneer fraude wordt gemeld** - als een gebruiker rapporten fraude, hun account is geblokkeerd.
- **Code naar rapport fraude tijdens de eerste begroeting** -gebruikers normaal gesproken druk op # om te bevestigen dat verificatie in twee stappen. Als ze fraude rapporteren wilt, invoeren ze een code voordat u op #. Deze code is **0** standaard, maar u kunt deze aanpassen.

> [!NOTE]
> Microsoft standaard gesproken begroetingen vertelt gebruikers op 0# verzenden van een fraudewaarschuwing voor te drukken. Als u wilt met een code dan 0, moet u registreren en uw eigen aangepaste gesproken begroetingen met instructies voor het juiste uploaden.

![Waarschuwingsopties fraude - schermafbeelding](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Hoe gebruikers fraude rapporteren 
Fraudewaarschuwing kan op twee manieren worden gerapporteerd.  Hetzij via de mobiele app of via de telefoon.  

#### <a name="report-fraud-with-the-mobile-app"></a>Melden van fraude met de mobiele app
1. Wanneer een verificatie wordt verzonden naar je telefoon, selecteert u het openen van de Microsoft Authenticator-app.
2. Selecteer **weigeren** op de melding. 
3. Selecteer **melden van fraude**.
4. Sluit de app.

#### <a name="report-fraud-with-a-phone"></a>Melden van fraude met een telefoon
1. Als verificatie gebeld op uw telefoon binnenkomt, beantwoordt u deze.  
2. Als fraude te rapporteren, voert u de fraudecode (de standaardwaarde is 0) en klik vervolgens op de #-ondertekening. U vervolgens een melding dat er een fraudewaarschuwing voor is ingediend.
3. De oproep te beëindigen.

### <a name="view-fraud-reports"></a>Fraude-rapporten weergeven
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer links Active Directory.
3. Selecteer aan de bovenkant **multi-factor Auth-Providers** om weer te geven een lijst met uw multi-factor Auth-Providers.
4. Selecteer uw multi-factor Authentication-Provider en klik op **beheren** aan de onderkant van de pagina. Hiermee opent u de Azure multi-factor Authentication-beheerportal.
5. Klik op de Azure multi-factor Authentication-beheerportal, onder weergave een rapport **fraudewaarschuwing**.
6. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook gebruikersnamen, telefoonnummers en de status van de gebruiker opgeven.
7. Klik op **uitvoeren** om een rapport van Fraudewaarschuwingen weer te geven. Klik op **exporteren naar CSV** als u wilt exporteren van het rapport.

## <a name="one-time-bypass"></a>Eenmalig overslaan
Eenmalig overslaan kan een gebruiker één keer verifiëren zonder verificatie in twee stappen uitvoeren. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden. In situaties waar de mobiele app of de telefoon geen een melding of telefoongesprek ontvangt, kunt u een eenmalig overslaan inschakelen zodat de gebruiker toegang heeft tot de gewenste resource.

### <a name="create-a-one-time-bypass"></a>Maken van een eenmalig overslaan
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Navigeer naar de MFA-beheerportal per de instructies boven aan deze pagina.
3. Als u de naam van de Azure MFA-Provider aan de linkerkant met zien een  **+**  ernaast, klikt u op de  **+** . Replicatiegroepen voor verschillende MFA-Server en de groep Azure Default worden weergegeven. Selecteer de gewenste groep.
4. Selecteer onder beheer van de gebruiker **eenmalige**.
5. Klik op de pagina eenmalig overslaan **nieuwe eenmalige**.

  ![Cloud](./media/multi-factor-authentication-whats-next/create1.png)

6. Geef de gebruikersnaam, de duur van het overslaan en de reden voor het overslaan. Klik op **Bypass**.
7. De tijdslimiet wordt ingevoerd onmiddellijk, zodat de gebruiker moet zich aanmelden voordat het eenmalig overslaan verloopt. 

### <a name="view-the-one-time-bypass-report"></a>Het rapport eenmalig overslaan weergeven
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer links Active Directory.
3. Selecteer aan de bovenkant **multi-factor Auth-Providers** om weer te geven een lijst met uw multi-factor Auth-Providers.
4. Selecteer uw multi-factor Authentication-Provider en klik op **beheren** aan de onderkant van de pagina. Hiermee opent u de Azure multi-factor Authentication-beheerportal.
5. Klik op de Azure multi-factor Authentication-beheerportal, aan de linkerkant onder weergave een rapport **eenmalige**.
6. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook gebruikersnamen, telefoonnummers en de status van de gebruiker opgeven.
7. Klik op **uitvoeren** om een rapport van omleidingen weer te geven. Klik op **exporteren naar CSV** als u wilt exporteren van het rapport.

## <a name="custom-voice-messages"></a>Aangepaste spraakberichten
Aangepaste spraakberichten kunnen u uw eigen opnamen of begroetingen gebruiken voor verificatie in twee stappen. U kunt aangepaste spraakberichten naast of ter vervanging van de Microsoft records.

Voordat u begint, worden op de hoogte van de volgende dingen:

* Ondersteunde bestandsindelingen zijn .wav en .mp3.
* De maximale bestandsgrootte is 5 MB.
* Verificatieberichten moet korter zijn dan 20 seconden. Berichten die langer dan 20 seconden zijn ondertussen niet kan de gebruikers om te reageren voordat de verificatietijd wordt opraakt.

### <a name="set-up-a-custom-message"></a>Een aangepast bericht instellen

Er zijn twee onderdelen voor het maken van uw aangepaste bericht. Eerst uploaden van het bericht en vervolgens u inschakelen voor uw gebruikers.

Voor het uploaden van uw aangepaste bericht:

1. Maak een aangepaste gesproken-bericht met een van de ondersteunde bestandsindelingen.
2. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
3. Navigeer naar de MFA-beheerportal per de instructies boven aan deze pagina.
4. Klik in de Azure multi-factor Authentication-beheerportal op **spraakberichten** onder de sectie configureren.
5. Op de configureren: stem berichten pagina, klikt u op **Nieuw spraakbericht**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6. Op de configureren: nieuwe spraakberichten pagina, klikt u op **geluidsbestanden beheren**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7. Op de configureren: geluid pagina bestanden, klikt u op **geluidsbestand uploaden**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8. Op de configureren: geluidsbestand uploaden, klikt u op **Bladeren** en navigeer naar uw spraakbericht, klikt u op **Open**.
9. Voeg een beschrijving toe en klik op **uploaden**.
10. Zodra de spraakbericht is geüpload, wordt een bericht wordt bevestigd dat het bestand is geüpload.

Het bericht voor uw gebruikers inschakelen:

1. Klik aan de linkerkant op **spraakberichten**.
2. Klik onder de sectie spraakberichten op **Nieuw spraakbericht**.
3. Selecteer een taal in de vervolgkeuzelijst taal.
4. Als dit bericht voor een bepaalde toepassing is, kunt u dit in de toepassing opgeven.
5. Selecteer het berichttype worden overschreven met uw nieuwe aangepaste bericht in de vervolgkeuzelijst berichttype.
6. Selecteer in de vervolgkeuzelijst geluidsbestand het geluidsbestand die u hebt geüpload in het eerste deel.
7. Klik op **Create**. Een bericht wordt bevestigd dat u hebt een spraakbericht gemaakt.
    ![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Opslaan in cache in Azure multi-factor Authentication
In cache opslaan, kunt u een specifiek tijdstip periode zo instellen dat latere authenticatiepogingen binnen deze periode automatisch laten slagen. Opslaan in cache wordt gebruikt als on-premises systemen zoals VPN veel aanvragen voor verificatie verzendt terwijl de eerste aanvraag nog steeds uitgevoerd wordt. De volgende aanvragen voor het automatisch uitgevoerd nadat de gebruiker de eerste verificatie uitgevoerd slaagt toestaan. 

Opslaan in cache is niet bedoeld om te worden gebruikt voor aanmeldingen bij Azure AD.

### <a name="set-up-caching"></a>Opslaan in cache instellen 
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Navigeer naar de MFA-beheerportal per de instructies boven aan deze pagina.
3. Klik in de Azure multi-factor Authentication-beheerportal op **opslaan in cache** onder de sectie configureren.
4. Klik op **nieuwe Cache** op de pagina caching configureren.
5. Selecteer het type van de Cache en het aantal cache-seconden. Klik op **Create**.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Goedgekeurde IP-adressen
Goedgekeurde IP-adressen is een functie van Azure MFA die beheerders van een tenant beheerd of federatieve gebruiken kunnen voor verificatie in twee stappen overslaan. Die wordt gebruikt voor gebruikers die vanaf een lokaal intranet van het bedrijf aanmelden zich. Deze functie is beschikbaar met de volledige versie van Azure multi-factor Authentication, niet de gratis versie voor beheerders. Zie voor meer informatie over het verkrijgen van de volledige versie van Azure multi-factor Authentication [Azure multi-factor Authentication](multi-factor-authentication.md).

| Type van Azure AD-Tenant | Beschikbare goedgekeurde IP-opties |
|:--- |:--- |
| Managed |<li>Specifiek IP-adresbereiken: beheerders kunnen een bereik met IP-adressen die verificatie in twee stappen voor gebruikers die vanaf het bedrijfsintranet aanmelden zich omzeilen opgeven.</li> |
| Federatieve |<li>Alle federatieve gebruikers: alle federatieve gebruikers die vanaf binnen de organisatie aanmelden zich wordt verificatie in twee stappen met behulp van een claim uitgegeven door AD FS overslaan.</li><br><li>Specifiek IP-adresbereiken: beheerders kunnen een bereik met IP-adressen die verificatie in twee stappen voor gebruikers die vanaf het bedrijfsintranet aanmelden zich omzeilen opgeven. |

Dit werkt alleen in binnen het intranet van een bedrijf overslaan. 

**De eindgebruikerservaring voor binnen corpnet:**

Wanneer de goedgekeurde IP-adressen is uitgeschakeld, verificatie in twee stappen is vereist voor de browser stromen en app-wachtwoorden vereist zijn voor oudere interactieve apps. 

Wanneer de goedgekeurde IP-adressen is ingeschakeld, wordt verificatie in twee stappen is *niet* vereist voor de browser stromen. App-wachtwoorden *niet* vereist voor oudere rijke ClientApps, mits de gebruiker al een app-wachtwoord nog niet is gemaakt. Nadat een app-wachtwoord gebruikt wordt, blijft deze vereiste. 

**Eindgebruikerservaring buiten corpnet:**

Verificatie in twee stappen is vereist voor de browser stromen of goedgekeurde IP-adressen is ingeschakeld of niet, en app-wachtwoorden vereist zijn voor oudere interactieve apps. 

### <a name="to-enable-trusted-ips"></a>Goedgekeurde IP-adressen inschakelen
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Navigeer naar de pagina MFA-Service-instellingen per de instructies aan het begin van dit artikel.
3. Op de pagina Service-instellingen onder goedgekeurde IP-adressen, hebt u twee opties:
   
   * **Voor aanvragen van federatieve gebruikers die afkomstig zijn van mijn intranet** : Schakel het selectievakje in. Alle federatieve gebruikers die zich aanmelden vanaf het bedrijfsnetwerk overslaan in twee stappen verificatie met behulp van een claim uitgegeven door AD FS.
   * **Voor aanvragen van een specifiek bereik van openbare IP-adressen** – Geef de IP-adressen in het tekstvak opgegeven CIDR-notatie. Bijvoorbeeld: xxx.xxx.xxx.0/24 voor IP-adressen in het bereik xxx.xxx.xxx.1 – xxx.xxx.xxx.254 of xxx.xxx.xxx.xxx/32 voor een enkel IP-adres. U kunt maximaal 50 IP-adresbereiken. Gebruikers die zich via deze IP-adressen aanmelden overslaan verificatie in twee stappen.
4. Klik op **Opslaan**.
5. Nadat de updates zijn toegepast, klikt u op **sluiten**.

![Goedgekeurde IP-adressen](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>App-wachtwoorden
Bepaalde apps, zoals Office 2010 of ouder en Apple Mail ondersteunen geen verificatie in twee stappen. Ze zijn niet geconfigureerd voor het accepteren van een tweede verificatie. Voor het gebruik van deze apps, moet u 'app-wachtwoorden' gebruiken in plaats van uw traditionele wachtwoord. Het app-wachtwoord kunt de toepassing voor verificatie in twee stappen overslaan en doorgaan met werken.

> [!NOTE]
> Moderne verificatie voor de Office 2013-Clients
> 
> Office 2013-clients (inclusief Outlook) en nieuwere ondersteuning moderne verificatieprotocollen en kunnen worden ingeschakeld voor gebruik met verificatie in twee stappen. Eenmaal is ingeschakeld, zijn app-wachtwoorden niet vereist voor deze clients.  Zie voor meer informatie [Office 2013 modern authentication openbare preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Belangrijk om te weten over app-wachtwoorden
Hier volgt een lijst met belangrijke zaken die u over app-wachtwoorden weten moet.

* App-wachtwoorden moeten worden ingevoerd in het invoervak eenmaal per app. Gebruikers hoeven te volgen en deze elke keer invoeren.
* Het wachtwoord wordt automatisch gegenereerd en is niet geleverd door de gebruiker. De automatisch gegenereerde wachtwoorden moeilijker voor aanvallers om aan te raden is en is veiliger.
* Er is een limiet van 40 wachtwoorden per gebruiker. 
* Apps die wachtwoorden van de cache en worden gebruikt in scenario's voor lokale kunnen mislukken omdat het app-wachtwoord is niet bekend is buiten de organisatie-id. Een voorbeeld e-mailberichten voor het Exchange die zich on-premises maar gearchiveerde e-mail is in de cloud. Hetzelfde wachtwoord werkt niet.
* Wanneer multi-factor authentication-server wordt gestart, kunt u uw wachtwoord kunt gebruiken met een aantal niet-browserclients. U kunt app-wachtwoorden niet gebruiken voor beheertaken. Zorg ervoor dat u een serviceaccount maken met een sterk wachtwoord om uit te voeren PowerShell-scripts en dat account voor verificatie in twee stappen niet inschakelt.

> [!WARNING]
> App-wachtwoorden werken niet in hybride omgevingen waarin clients communiceren met zowel on-premises en in de cloud autodiscover-eindpunten. Domeinwachtwoorden zijn vereist voor de verificatie van lokale en app-wachtwoorden nodig zijn om te verifiëren met de cloud.

### <a name="naming-guidance-for-app-passwords"></a>Richtlijnen voor App-wachtwoorden voor de naamgeving
Namen van App-wachtwoord moeten overeenkomen met het apparaat waarop ze worden gebruikt. Als er een laptop met niet-browsertoepassingen, bijvoorbeeld één app-wachtwoord met de naam Laptop maken en dit app-wachtwoord gebruiken. Vervolgens maakt u een andere app-wachtwoord bureaublad met de naam voor de toepassingen op uw pc. 

Microsoft raadt u aan één appwachtwoord per apparaat, niet één appwachtwoord per toepassing maken.

### <a name="federated-sso-app-passwords"></a>App-wachtwoorden voor federatieve (SSO)
Azure AD biedt ondersteuning voor federatie (eenmalige aanmelding) met het lokale Windows Server Active Directory Domain Services (AD DS). Als uw organisatie is gefedereerd met Azure AD en u wilt gebruikmaken van Azure multi-factor Authentication, zijn de informatie over app-wachtwoorden is belangrijk voor u. Deze sectie is alleen van toepassing op klanten van federatieve (SSO).

* App-wachtwoorden worden geverifieerd door Azure AD en daarom federation overslaan. Federatie wordt alleen actief gebruikt bij het instellen van app-wachtwoorden.
* Voor federatieve gebruikers (SSO) gaat wordt nooit u naar de id-Provider (IdP) in tegenstelling tot de passieve stroom. De wachtwoorden worden opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, heeft dat gegevens naar de organisatie-id in de huidige tijd met behulp van DirSync stromen. Account uitschakelen/verwijderen kan duren tot drie uur te synchroniseren, vertragen uitschakelen/verwijderen van App-wachtwoord in Azure AD.
* On-premises instellingen voor toegangsbeheer van client worden niet herkend door het app-wachtwoord.
* Er is geen lokale verificatie mogelijkheid logboekregistratie/controle is beschikbaar voor App-wachtwoord.
* Bepaalde geavanceerde architectuur ontwerpen kunnen een combinatie van organisatie-gebruikersnaam, wachtwoord en het app-wachtwoorden vereist bij het gebruik van verificatie in twee stappen met clients. Hoewel dit afhankelijk van waar ze verifiëren. Voor clients die worden geverifieerd bij een on-premises infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij Azure AD, gebruikt u het app-wachtwoord.

  Stel bijvoorbeeld dat u hebt een architectuur die uit deze exemplaren bestaat:

  * U federeert uw on-premises exemplaar van Active Directory met Azure AD
  * U via Exchange online
  * U gebruikt Lync die specifiek on-premises
  * U gebruikt Azure multi-factor Authentication

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  In dergelijke gevallen moet u deze stappen volgen:

  * Bij het aanmelden bij Lync, gebruiken de gebruikersnaam en wachtwoord van uw organisatie.
  * Wanneer u probeert te krijgen tot het adresboek via een Outlook-client die is verbonden met Exchange online, gebruikt u een app-wachtwoord.

### <a name="allow-app-password-creation"></a>Maken van app-wachtwoord toestaan
Gebruikers kunnen geen app-wachtwoorden maken standaard, maar u kunt de functie zelf inschakelen. Als u wilt toestaan dat gebruikers de mogelijkheid te maken van app-wachtwoorden, gebruikt u de volgende procedure:

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Navigeer naar de pagina MFA-Service-instellingen per de instructies aan het begin van dit artikel.
3. Schakel het keuzerondje naast **gebruikers toestaan te maken van app-wachtwoorden voor aanmelding bij niet-browsertoepassingen**.

![App-wachtwoorden maken](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>App-wachtwoorden maken
Gebruikers kunnen app-wachtwoorden maken tijdens de initiële inschrijving. Een optie aan het einde van het registratieproces waarmee ze appwachtwoorden te maken, wordt verstrekt.

Gebruikers kunnen ook app-wachtwoorden maken na de registratie. Ze kunnen de app-wachtwoorden maken door hun instellingen in de Azure portal of de Office 365-portal te wijzigen. Zie voor meer informatie en gedetailleerde stappen voor uw gebruikers [wat zijn app-wachtwoorden in Azure multi-factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Houd er rekening mee multi-factor Authentication voor apparaten die gebruikers vertrouwen
Onthoud multi-factor Authentication voor apparaten en browsers dat gebruikers vertrouwensrelatie een gratis functie voor alle gebruikers van MFA is. Multi-factor authentication-server kan gebruikers omzeilen MFA voor een bepaald aantal dagen na het aanmelden. Deze functie verbetert de bruikbaarheid wordt verbeterd door het aantal keren dat een gebruiker verificatie in twee stappen op hetzelfde apparaat uitvoeren kan minimaliseren.

Echter, als een account of apparaat is geknoeid, onthoud MFA voor vertrouwde apparaten kan invloed hebben op beveiliging. Als een zakelijke account wordt aangetast of een vertrouwd apparaat is zoekgeraakt of gestolen, moet u [multi-factor Authentication herstellen op alle apparaten](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Deze actie worden de vertrouwde status van alle apparaten ingetrokken en de gebruiker is vereist om opnieuw verificatie in twee stappen uitvoeren. U kunt ook instrueert u uw gebruikers MFA om op te herstellen met de instructies in hun eigen apparaten [beheren van uw instellingen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Hoe werkt het?

Onthouden van multi-factor Authentication werkt door het instellen van een permanente cookie op de browser wanneer een gebruiker controleert de ' niet opnieuw vragen voor **X** dagen ' vak bij het aanmelden. De gebruiker niet gevraagd voor MFA opnieuw van die browser totdat de cookie verloopt. Als de gebruiker een andere browser op hetzelfde apparaat opent of hun cookies wist, wordt deze gevraagd om te controleren of opnieuw. 

De ' niet opnieuw vragen voor **X** dagen ' checkbox wordt niet weergegeven op niet-browsertoepassingen, al dan niet ondersteuning van moderne verificatie. Deze apps gebruik vernieuwen van tokens die nieuwe toegangstokens om het uur leveren. Wanneer een vernieuwingstoken wordt gevalideerd, controleert Azure AD dat de verificatie laatste tijd in twee stappen is uitgevoerd binnen het geconfigureerde aantal dagen. 

Tot slot beperkt MFA onthouden op vertrouwde apparaten het aantal authenticaties op web-apps (die normaal gesproken vraag elke keer). Maar verhoogt ook het aantal authenticaties voor moderne verificatie-clients (die normaal gesproken vraagt om de 90 dagen).

> [!NOTE]
>Deze functie is niet compatibel met de functie 'Aangemeld blijven' van AD FS wanneer gebruikers verificatie in twee stappen voor AD FS via de Azure MFA-Server of een MFA-oplossing van derden uitvoeren. Als uw gebruikers "Aangemeld blijven" Selecteer op de AD FS en ook hun apparaat als vertrouwd voor MFA markeren, is ze niet kunnen verifiëren nadat het 'MFA onthouden' aantal dagen is verlopen. Azure AD de verificatie van een nieuwe in twee stappen aanvraagt, maar de AD FS retourneert een token met het oorspronkelijke MFA claim en de datum in plaats van het uitvoeren van verificatie in twee stappen weer. Hiermee stelt u dit proces uit een lus verificatie tussen Azure AD en AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Onthoud dat multi-factor authentication inschakelen
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Navigeer naar de pagina MFA-Service-instellingen per de instructies aan het begin van dit artikel.
3. Op de pagina Service-instellingen beheren onder instellingen voor het apparaat van gebruikers, Controleer de **gebruikers toestaan om te onthouden meervoudige verificatie op vertrouwde apparaten** vak.
   ![Onthouden van apparaten](./media/multi-factor-authentication-whats-next/remember.png)
4. Stel het aantal dagen dat u wilt toestaan dat de vertrouwde apparaten verificatie in twee stappen overslaan. De standaardwaarde is 14 dagen.
5. Klik op **Opslaan**.
6. Klik op **Sluiten**.

### <a name="mark-a-device-as-trusted"></a>Een apparaat als vertrouwd is ingeschakeld

Zodra u deze functie inschakelt, gebruikers wilt markeren een apparaat als vertrouwde wanneer ze zich aanmelden door te controleren **niet opnieuw vragen**.

![Niet opnieuw vragen - schermafbeelding](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Selecteerbare verificatiemethoden
U kunt kiezen welke verificatiemethoden zijn beschikbaar voor uw gebruikers. De volgende tabel geeft een kort overzicht van elke methode.

Wanneer uw gebruikers hun account voor MFA registreren, kiest u de methode voorkeursoptie voor verificatie buiten de opties die u hebt ingeschakeld. De richtlijnen voor het registratieproces wordt beschreven in [Mijn account voor verificatie in twee stappen instellen](multi-factor-authentication-end-user-first-time.md)

| Methode | Beschrijving |
|:--- |:--- |
| Bellen naar telefoon |Een geautomatiseerd telefoongesprek plaatst. De gebruiker beantwoordt het gesprek en drukt # in op de toetsenblok van de telefoon om te verifiëren. Dit telefoonnummer is niet gesynchroniseerd met lokale Active Directory. |
| Sms-bericht naar telefoon |Verzendt een SMS-bericht met een verificatiecode. De gebruiker wordt gevraagd te beantwoorden aan de tekst, het bericht met de verificatiecode of de verificatiecode invoeren in de interface voor aanmelden. |
| Melding via de mobiele app |Een pushmelding verzendt naar uw telefoon of geregistreerd apparaat. De gebruiker de melding weergaven en **controleren** om verificatie te voltooien. <br>De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Verificatiecode via mobiele app |De Microsoft Authenticator-app genereert elke 30 seconden een nieuwe OATH-verificatiecode uit. De gebruiker voert deze bevestigingscode in de interface voor aanmelden.<br>De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Hoe verificatiemethoden in-of uitschakelen
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Navigeer naar de pagina MFA-Service-instellingen per de instructies aan het begin van dit artikel.
3. Op de pagina Service-instellingen onder verificatie-opties selecteren/uitschakelen de opties die u wilt gebruiken.
   ![Verificatie-opties](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Klik op **Opslaan**.
5. Klik op **Sluiten**.
