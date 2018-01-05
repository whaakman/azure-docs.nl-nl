---
title: Azure multi-factor Authentication configureren | Microsoft Docs
description: In dit artikel wordt beschreven hoe instellingen van de Azure multi-factor Authentication voor rapporten, Fraudewaarschuwingen, eenmalige omleidingen, aangepaste spraakberichten caching, goedgekeurde IP-adressen en app-wachtwoorden configureren.
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
ms.date: 01/03/2018
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 4421b995e69e115fbb6c7379af79aaef537aed0d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure multi-factor Authentication-instellingen configureren

In dit artikel helpt u bij het beheren van Azure multi-factor Authentication nu dat u actief en werkend bent. Deze heeft verschillende onderwerpen die u helpen bij het Maak optimaal gebruik van Azure multi-factor Authentication. Niet alle functies zijn beschikbaar in elke [versie van Azure multi-factor Authentication](/multi-factor-authentication-get-started.md#what-features-do-i-need).

| Functie | Beschrijving | 
|:--- |:--- |
| [Blokkeren en blokkering van gebruikers](#block-and-unblock-users) |Gebruik de functie gebruikers blokkeren/blokkering opheffen om te voorkomen dat gebruikers ontvangen van verificatieaanvragen. |
| [Fraudewaarschuwing](#fraud-alert) |De waarschuwing fraude-functie configureren zodat uw gebruikers frauduleuze probeert te krijgen tot hun bronnen kunnen rapporteren. |
| [Eenmalig overslaan](#one-time-bypass) |De functie eenmalig overslaan gebruiken om gebruikers kunnen worden geverifieerd door één keer _omzeilen_ multi-factor Authentication. |
| [Aangepaste spraakberichten](#custom-voice-messages) |Gebruik de functie voor aangepaste gesproken berichten uw eigen opnamen of begroeting voor multi-factor Authentication. |
| [Opslaan in cache](#caching-in-azure-multi-factor-authentication) |De cache in functie gebruiken om een bepaalde periode zo instellen dat latere authenticatiepogingen automatisch laten slagen. |
| [Goedgekeurde IP-adressen](#trusted-ips) |Beheerders van een beheerd of federatieve tenant kunnen de functie goedgekeurde IP-adressen gebruiken voor het overslaan van de verificatie in twee stappen voor gebruikers die zich via het bedrijfsintranet aanmelden. |
| [App-wachtwoorden](#app-passwords) |De functie app-wachtwoord gebruiken om in te schakelen van een toepassing op de multi-factor Authentication overslaan en doorgaan met werken. |
| [Houd er rekening mee multi-factor Authentication voor vertrouwde apparaten en browsers](#remember-multi-factor-authentication-for-trusted-devices) |Gebruik deze functie te onthouden vertrouwde apparaten en browsers voor een bepaald aantal dagen nadat een gebruiker heeft is aangemeld met behulp van multi-factor Authentication. |
| [Selecteerbare verificatiemethoden](#selectable-verification-methods) |Gebruik deze functie om de lijst met verificatiemethoden die gebruikers kunnen gebruiken. |

## <a name="block-and-unblock-users"></a>Blokkeren en blokkering van gebruikers

Gebruik de _blokkeren en blokkering gebruikers_ functie om te voorkomen dat gebruikers ontvangen van verificatieaanvragen. Alle verificatiepogingen voor geblokkeerde gebruikers worden automatisch geweigerd. Gebruikers blijven geblokkeerd gedurende 90 dagen vanaf het moment dat ze worden geblokkeerd.

### <a name="block-a-user"></a>Een gebruiker blokkeren

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **gebruikers blokkeren/blokkering**.
3. Selecteer **toevoegen** om een gebruiker te blokkeren.
4. Selecteer de **replicatiegroep**. Geef de gebruikersnaam voor de geblokkeerde gebruiker als **gebruikersnaam<span></span>@domain.com**. Voer een opmerking in het **reden** veld.
5. Selecteer **toevoegen** voltooid blokkeren van de gebruiker.

### <a name="unblock-a-user"></a>Een gebruiker deblokkeren

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **gebruikers blokkeren/blokkering**.
3. Selecteer **blokkering** in de **actie** kolom naast de gebruiker toe te staan.
4. Voer een opmerking in het **reden voor het opheffen van de blokkering** veld.
5. Selecteer **blokkering** voltooid deblokkeren van de gebruiker.

## <a name="fraud-alert"></a>Fraudewaarschuwing

Configureer de _fraudewaarschuwing_ functie zodat uw gebruikers frauduleuze probeert te krijgen tot hun bronnen kunnen rapporteren. Gebruikers kunnen pogingen fraude rapporteren met behulp van de mobiele app of via hun telefoon.

### <a name="turn-on-fraud-alerts"></a>Fraudewaarschuwingen inschakelen

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **fraudewaarschuwing**.

   ![Fraudewaarschuwingen inschakelen](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Stel de **dat gebruikers kunnen indienen Fraudewaarschuwingen** instelt op **op**.
4. Selecteer **Opslaan**.

### <a name="configuration-options"></a>Configuratie-opties

- **Gebruiker blokkeren wanneer fraude wordt gemeld**: als een gebruiker fraude wordt gemeld, hun account gedurende 90 dagen of totdat een beheerder blokkering hun account opgeheven is geblokkeerd. Een beheerder kan aanmeldingen controleren met behulp van het rapport aanmelden en onderneem gepaste actie om toekomstige fraude te voorkomen. Vervolgens kan een beheerder [deblokkeren](#unblock-a-user) account van de gebruiker.
- **Code voor het melden van fraude tijdens de eerste begroeting**: wanneer gebruikers een telefonische oproep verificatie in twee stappen uitvoeren krijgen, druk ze normaal  **#**  om te bevestigen dat hun aanmelden. Rapport fraude, de gebruiker voert een code voordat zwaarwegende  **#** . Deze code is **0** standaard, maar u kunt deze aanpassen.

  >[!NOTE]
  >De standaard gesproken begroetingen van Microsoft gebruikers instrueren te druk op **0#** verzenden van een fraudewaarschuwing voor. Als u wilt gebruiken een code dan **0**, registreren en uw eigen aangepaste gesproken begroetingen met instructies voor uw gebruikers te uploaden.
  >

### <a name="view-fraud-reports"></a>Fraude-rapporten weergeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **aanmeldingen**. Het rapport fraude maakt nu deel uit van het standaard Azure AD aanmeldingen-rapport.

## <a name="one-time-bypass"></a>Eenmalige bypass

De _eenmalig overslaan_ functie kan een gebruiker één keer verifiëren zonder verificatie in twee stappen uitvoeren. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden. In situaties waar de mobiele app of de telefoon geen een melding of telefoongesprek ontvangt, kunt u een eenmalig overslaan toestaan, zodat de gebruiker toegang heeft tot de gewenste resource.

### <a name="create-a-one-time-bypass"></a>Maken van een eenmalig overslaan

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **eenmalig overslaan**.

   ![Eenmalige toegang maken](./media/multi-factor-authentication-whats-next/onetimebypass.png)

3. Selecteer **Toevoegen**.
4. Selecteer indien nodig, de replicatiegroep voor het overslaan.
5. Geef de gebruikersnaam als **gebruikersnaam<span></span>@domain.com**. Voer het aantal seconden dat de bypass moet duren. Geef de reden voor het overslaan. 
6. Selecteer **Toevoegen**. De tijdslimiet gaat onmiddellijk van kracht. De gebruiker moet zich aanmelden voordat het eenmalig overslaan verloopt. 

### <a name="view-the-one-time-bypass-report"></a>Het rapport eenmalig overslaan weergeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Active Directory** > **MFA-Server** > **eenmalig overslaan**.

## <a name="custom-voice-messages"></a>Aangepaste spraakberichten
U kunt uw eigen opnamen of begroetingen gebruiken voor verificatie in twee stappen met het _aangepaste spraakberichten_ functie. Deze berichten kunnen worden gebruikt in aanvulling op of de opnamen Microsoft vervangen.

Voordat u begint, worden op de hoogte van de volgende beperkingen:

* Ondersteunde bestandsindelingen zijn .wav en .mp3.
* De maximale bestandsgrootte is 5 MB.
* Verificatieberichten moet korter zijn dan 20 seconden. Berichten die langer dan 20 seconden zijn kunnen leiden tot de verificatie mislukt. De gebruiker kan niet reageren voordat het bericht is voltooid en de verificatie een optreedt time-out.

### <a name="set-up-a-custom-message"></a>Een aangepast bericht instellen

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **telefoongesprek instellingen**.

   ![Berichten van de aangepaste phone record](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Selecteer **wenskaart toevoegen**.
4. Kies het type begroeting. De taal kiezen.
5. Selecteer een MP3- of .wav geluidsbestand uploaden.
6. Selecteer **Toevoegen**.

## <a name="caching-in-azure-multi-factor-authentication"></a>Opslaan in cache in Azure multi-factor Authentication

U kunt instellen dat een bepaalde periode om toe te staan verificatiepogingen nadat een gebruiker is geverifieerd met behulp van de _caching_ functie. Latere authenticatiepogingen voor de gebruiker in de opgegeven mislukt periode automatisch. Opslaan in cache wordt voornamelijk gebruikt als on-premises systemen, zoals VPN, meerdere aanvragen voor verificatie verzenden bij de eerste aanvraag nog in voortgang is. Deze functie kunt de volgende aanvragen automatisch, slaagt alleen als de gebruiker de eerste verificatie uitgevoerd is geslaagd. 

>[!NOTE]
>De functie voor opslaan in cache is niet bedoeld voor aanmeldingen met Azure Active Directory (Azure AD) moet worden gebruikt.

### <a name="set-up-caching"></a>Opslaan in cache instellen 

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar **Azure Active Directory** > **MFA-Server** > **regels opslaan in cache**.

   ![Regels opslaan in cache instellen](./media/multi-factor-authentication-whats-next/cachingrules.png)

3. Selecteer **Toevoegen**.
4. Selecteer de **cachetype** uit de vervolgkeuzelijst. Geef het maximum aantal **seconden in de cache**. 
5. Indien nodig, een verificatietype te selecteren en een toepassing opgeven. 
6. Selecteer **Toevoegen**.


## <a name="trusted-ips"></a>Goedgekeurde IP-adressen

De _goedgekeurde IP-adressen_ functie van Azure multi-factor Authentication wordt gebruikt door beheerders van een beheerd of federatieve-tenant. De functie wordt overgeslagen voor verificatie in twee stappen voor gebruikers die zich via het bedrijfsintranet aanmelden. De functie is beschikbaar bij de volledige versie van Azure multi-factor Authentication en niet de gratis versie voor beheerders. Zie voor meer informatie over het verkrijgen van de volledige versie van Azure multi-factor Authentication [Azure multi-factor Authentication](multi-factor-authentication.md).

| Azure AD-tenant-type | Opties van de functie vertrouwde IP-adressen |
|:--- |:--- |
| Managed |**Specifiek IP-adressen**: beheerders opgeven in een bereik met IP-adressen die verificatie in twee stappen voor gebruikers die zich via het bedrijfsintranet aanmelden kunnen omzeilen.|
| Federatief |**Alle federatieve gebruikers**: alle federatieve gebruikers die u aan te binnen de organisatie melden verificatie in twee stappen kunnen omzeilen. De gebruikers omzeilen verificatie met behulp van een claim dat is uitgegeven door Active Directory Federation Services (AD FS).<br/>**Specifiek IP-adressen**: beheerders opgeven in een bereik met IP-adressen die verificatie in twee stappen voor gebruikers die zich via het bedrijfsintranet aanmelden kunnen omzeilen. |

De goedgekeurde IP-adressen overslaan werkt alleen in het bedrijfsintranet. Als u selecteert de **alle federatieve gebruikers** optie en een gebruiker zich aanmeldt via buiten het bedrijfsintranet, de gebruiker heeft om te verifiëren met behulp van verificatie in twee stappen. Het proces is hetzelfde, zelfs als de gebruiker een AD FS-claim aanbiedt. 

**Eindgebruikerservaring binnen corpnet**

Als de functie van de goedgekeurde IP-adressen is uitgeschakeld, is verificatie in twee stappen vereist voor browser-stromen. App-wachtwoorden zijn vereist voor oudere interactieve toepassingen. 

Als de functie van de goedgekeurde IP-adressen is ingeschakeld, wordt verificatie in twee stappen is *niet* vereist voor de browser stromen. App-wachtwoorden *niet* vereist zijn voor oudere interactieve toepassingen, mits de gebruiker een app-wachtwoord nog niet gemaakt. Nadat een app-wachtwoord gebruikt wordt, blijft het wachtwoord vereist. 

**Eindgebruikerservaring buiten corpnet**

Ongeacht of de functie goedgekeurde IP-adressen is ingeschakeld, is verificatie in twee stappen vereist voor browser-stromen. App-wachtwoorden zijn vereist voor oudere interactieve toepassingen. 

### <a name="enable-named-locations-by-using-conditional-access"></a>Benoemde locaties inschakelen met behulp van voorwaardelijke toegang

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **voorwaardelijke toegang** > **locaties met de naam**.
3. Selecteer **nieuwe locatie**.
4. Voer een naam voor de locatie.
5. Selecteer **markeren als vertrouwde locatie**.
6. Voer het IP-adresbereik in CIDR-notatie zoals **192.168.1.1/24**.
7. Selecteer **Maken**.

### <a name="enable-the-trusted-ips-feature-by-using-conditional-access"></a>De goedgekeurde IP-adressen inschakelen met behulp van voorwaardelijke toegang

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **voorwaardelijke toegang** > **locaties met de naam**.
3. Selecteer **MFA configureren goedgekeurde IP-adressen**.
4. Op de **Service-instellingen** pagina onder **goedgekeurde IP-adressen**, kiezen uit de volgende twee opties:
   
   * **Voor aanvragen van federatieve gebruikers die afkomstig zijn van mijn intranet**: Schakel het selectievakje in om deze optie te selecteren. Alle federatieve gebruikers die aanmelden met het bedrijfsnetwerk verificatie in twee stappen overslaan met behulp van een claim die door AD FS is uitgegeven. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```
     
   * **Voor aanvragen van een specifiek bereik van openbare IP-adressen**: voor deze optie kiest, voert u de IP-adressen in het tekstvak met behulp van de CIDR-notatie.
   
     * Gebruik voor IP-adressen die zich in het bereik xxx.xxx.xxx.1 via xxx.xxx.xxx.254, notatie zoals **xxx.xxx.xxx.0/24**.
     * Voor een enkel IP-adres gebruiken notatie zoals **xxx<span></span>.xxx.xxx.xxx/32**.
     
     Voer maximaal 50 IP-adresbereiken. Gebruikers die zich via deze IP-adressen aanmelden overslaan verificatie in twee stappen.

5. Selecteer **Opslaan**.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>De goedgekeurde IP-adressen inschakelen met behulp van de service-instellingen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **goedgekeurde IP-adressen**, kiezen uit de volgende twee opties:
   
   * **Voor aanvragen van federatieve gebruikers op mijn intranet**: Schakel het selectievakje in om deze optie te selecteren. Alle federatieve gebruikers die aanmelden met het bedrijfsnetwerk verificatie in twee stappen overslaan met behulp van een claim die door AD FS is uitgegeven. Zorg ervoor dat AD FS een regel voor het toevoegen van de intranet-claim op het juiste verkeer. Als de regel niet bestaat, maakt u de volgende regel in AD FS:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```

   * **Voor aanvragen van een opgegeven IP-adres subnetten**: voor deze optie kiest, voert u de IP-adressen in het tekstvak met behulp van de CIDR-notatie. 
     
     * Gebruik voor IP-adressen die zich in het bereik xxx.xxx.xxx.1 via xxx.xxx.xxx.254, notatie zoals **xxx.xxx.xxx.0/24**.
     * Voor een enkel IP-adres gebruiken notatie zoals **xxx<span></span>.xxx.xxx.xxx/32**.
     
     Voer maximaal 50 IP-adresbereiken. Gebruikers die zich via deze IP-adressen aanmelden overslaan verificatie in twee stappen.

6. Selecteer **Opslaan**.

![Goedgekeurde IP-adressen met de service-instellingen inschakelen](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>App-wachtwoorden

Sommige toepassingen, zoals Office 2010 of ouder en Apple Mail ondersteuning geen voor verificatie in twee stappen. De apps zijn niet geconfigureerd voor het accepteren van een tweede verificatie. Voor het gebruik van deze toepassingen profiteren van de _app-wachtwoorden_ functie. U kunt een app-wachtwoord gebruiken in plaats van uw traditionele wachtwoord toe zodat een app verificatie in twee stappen overslaan en doorgaan met werken.

>[!NOTE]
>Moderne verificatie voor de clients Microsoft Office 2013 en hoger
> 
>De clients met Office 2013 en hoger (inclusief Outlook) ondersteuning voor moderne verificatieprotocollen en kan worden ingeschakeld voor gebruik met verificatie in twee stappen. Nadat de client is ingeschakeld, app-wachtwoorden niet nodig zijn voor de client. Zie voor meer informatie de [Office 2013 modern authentication public preview aankondiging](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).
>

### <a name="considerations-about-app-passwords"></a>Overwegingen over app-wachtwoorden

Wanneer u app-wachtwoorden, houd rekening met de volgende belangrijke punten:

* App-wachtwoorden worden slechts één keer per toepassing ingevoerd. Gebruikers hebben geen bijhouden van de wachtwoorden of deze elke keer invoeren.
* Het wachtwoord wordt automatisch gegenereerd en is niet geleverd door de gebruiker. De automatisch gegenereerde wachtwoorden moeilijker voor aanvallers om aan te raden is en is veiliger.
* Er is een limiet van 40 wachtwoorden per gebruiker. 
* Toepassingen die wachtwoorden in de cache en deze gebruiken in scenario's voor lokale kunnen mislukt, omdat het app-wachtwoord is niet bekend is buiten het werk of school-account starten. Een voorbeeld van dit scenario is Exchange e-mails die zich on-premises, maar de gearchiveerde e-mail is in de cloud. In dit scenario wordt werkt hetzelfde wachtwoord niet.
* Nadat de multi-factor Authentication is ingeschakeld op een gebruikersaccount, kunnen app-wachtwoorden worden gebruikt met de meest niet-browserclients zoals Outlook en Microsoft Skype voor bedrijven. Beheeracties kunnen niet worden uitgevoerd met behulp van app-wachtwoorden via niet-browsertoepassingen, zoals Windows PowerShell. De acties kunnen niet worden uitgevoerd, zelfs wanneer de gebruiker een Administrator-account heeft. Een serviceaccount maken met een sterk wachtwoord voor het uitvoeren van PowerShell-scripts, en het account voor verificatie in twee stappen niet inschakelen.

>[!WARNING]
>App-wachtwoorden werken niet in hybride omgevingen waarin clients communiceren met zowel on-premises en in de cloud autodiscover-eindpunten. Domeinwachtwoorden zijn vereist voor lokale verificatie. App-wachtwoorden zijn vereist voor verificatie met de cloud.
>

### <a name="guidance-for-app-password-names"></a>Richtlijnen voor de namen van app-wachtwoord

Namen van App-wachtwoord moeten overeenkomen met het apparaat waarop ze allemaal hebt gebruikt. Als u een laptop met niet-browsertoepassingen zoals Outlook, Word en Excel hebt, maakt u één app-wachtwoord met de naam **Laptop** voor deze apps. Maken van een andere app-wachtwoord met de naam **bureaublad** voor de toepassingen die worden uitgevoerd op uw computer. 

>[!NOTE]
>U wordt aangeraden dat u een app-wachtwoord per apparaat in plaats van een app-wachtwoord per toepassing maakt.

### <a name="federated-or-single-sign-on-app-passwords"></a>Federatieve of eenmalige aanmelding-app-wachtwoorden

Azure AD biedt ondersteuning voor Federatie of eenmalige aanmelding (SSO) met het lokale Windows Server Active Directory Domain Services (AD DS). Als uw organisatie is gefedereerd met Azure AD en u Azure multi-factor Authentication, kunt u de volgende punten over app-wachtwoorden.

>[!NOTE]
>De volgende punten gelden alleen voor klanten met federatieve (SSO).

* App-wachtwoorden zijn geverifieerd door Azure AD en daarom federation omzeilen. Federatie wordt actief gebruikt alleen bij het instellen van app-wachtwoorden.
* De id-Provider (IdP) is geen contact worden gemaakt voor federatieve gebruikers (SSO), in tegenstelling tot de passieve stroom. De app-wachtwoorden worden opgeslagen in het werk of school-account. Als een gebruiker het bedrijf verlaat, gegevens van de gebruiker naar het werk of school-account loopt via **DirSync** in realtime. Het uitschakelen/verwijderen van het account kan tot drie uur te synchroniseren, die de uitschakelen/verwijderen van het app-wachtwoord in Azure AD kan uitstellen duren.
* Toegangsbeheer voor lokale clientinstellingen worden niet herkend door de functie app-wachtwoorden.
* Er is geen lokale verificatie mogelijkheid logboekregistratie/controle is beschikbaar voor gebruik met de functie app-wachtwoorden.
* Sommige geavanceerde architecturen vereisen een combinatie van referenties voor verificatie in twee stappen met clients. Deze referenties kunnen bestaan uit een werk of school gebruikersnaam en wachtwoorden en app-wachtwoorden. De vereisten, is afhankelijk van hoe de verificatie wordt uitgevoerd. Voor clients die worden geverifieerd bij een on-premises infrastructuur, account een werk of school-van gebruikersnaam en wachtwoord een vereiste. Een app-wachtwoord is vereist voor clients die worden geverifieerd bij Azure AD.

  Stel bijvoorbeeld dat u hebt de volgende architectuur:

  * Uw lokale exemplaar van Active Directory is gefedereerd met Azure AD.
  * U Exchange online gebruikt.
  * U gebruikt voor Skype voor bedrijven on-premises.
  * U Azure multi-factor Authentication.

  ![App-wachtwoorden gebruiken in een federatieve organisatie](./media/multi-factor-authentication-whats-next/federated.png)

  In dit scenario gebruikt u de volgende referenties:

  * Gebruiken om aan te melden bij Skype voor bedrijven, uw werk of school gebruikersnaam en het wachtwoord.
  * Voor toegang tot het adresboek van een Outlook-client die is verbonden met Exchange online, gebruikt u een app-wachtwoord.

### <a name="allow-users-to-create-app-passwords"></a>Toestaan dat gebruikers app-wachtwoorden maken

Gebruikers kunnen geen app-wachtwoorden maken standaard. De functie app-wachtwoorden moet zijn ingeschakeld. Als u gebruikers de mogelijkheid te maken van app-wachtwoorden geven, moet u de volgende procedure gebruiken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication **service-instellingen**.
5. Op de **Service-instellingen** pagina de **gebruikers toestaan te maken van app-wachtwoorden aan te melden bij niet-browsertoepassingen** optie.

   ![Toestaan dat gebruikers app-wachtwoorden maken](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>App-wachtwoorden maken

Gebruikers kunnen app-wachtwoorden maken tijdens de initiële inschrijving. De gebruiker heeft de optie voor het maken van app-wachtwoorden aan het einde van het registratieproces.

Gebruikers kunnen ook app-wachtwoorden maken na de registratie. De app-wachtwoorden kunnen worden gewijzigd via de instellingen in de Azure-portal of de Office 365-portal. Zie voor meer informatie en gedetailleerde stappen voor uw gebruikers [wat zijn app-wachtwoorden in Azure multi-factor Authentication?](./end-user/multi-factor-authentication-end-user-app-passwords.md)

<a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>
## <a name="remember-multi-factor-authentication-for-trusted-devices"></a>Houd er rekening mee multi-factor Authentication voor vertrouwde apparaten
De _onthouden multi-Factor Authentication_ onderdeel voor apparaten en browsers die worden vertrouwd door de gebruiker is een gratis onderdeel voor alle gebruikers van multi-factor Authentication. Gebruikers kunnen achteraf omzeilen voor een opgegeven aantal dagen, nadat ze hebt is aangemeld op een apparaat met behulp van multi-factor Authentication. De functie verbetert de bruikbaarheid door het aantal keren dat een gebruiker heeft voor het uitvoeren van verificatie in twee stappen op hetzelfde apparaat minimaliseren.

>[!IMPORTANT]
>Als een account of apparaat is geknoeid, kan Onthoud multi-factor Authentication voor vertrouwde apparaten beveiliging beïnvloeden. Als een zakelijke account wordt aangetast of een vertrouwd apparaat is zoekgeraakt of gestolen, moet u [multi-factor Authentication herstellen op alle apparaten](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user).
>
>De herstelbewerking trekt de vertrouwde status van alle apparaten en de gebruiker is vereist om opnieuw verificatie in twee stappen uitvoeren. U kunt ook instrueert u uw gebruikers multi-factor Authentication herstellen op hun eigen apparaten met de instructies in [beheren van uw instellingen voor verificatie in twee stappen](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted).
>

### <a name="how-the-feature-works"></a>De werking van de functie

De functie van de multi-factor Authentication onthouden stelt een permanente cookie op de browser wanneer een gebruiker selecteert de **niet opnieuw vragen voor X dagen** optie bij het aanmelden. De gebruiker wordt niet gevraagd opnieuw voor multi-factor Authentication van die dezelfde browser totdat de cookie verloopt. Als de gebruiker een andere browser op hetzelfde apparaat opent of hun cookies worden gewist, ze gevraagd opnieuw te controleren. 

De **niet opnieuw vragen voor X dagen** optie wordt niet weergegeven op niet-browsertoepassingen, ongeacht of de app moderne verificatie ondersteunt. Gebruik van deze apps _vernieuwingstokens_ die elk uur nieuwe toegangstokens bieden. Wanneer een vernieuwingstoken wordt gevalideerd, wordt Azure AD gecontroleerd dat de laatste verificatie in twee stappen is opgetreden binnen het opgegeven aantal dagen. 

De functie vermindert het aantal authenticaties op WebApps, die normaal gesproken telkens wanneer wordt gevraagd. De functie verhoogt het aantal authenticaties voor moderne verificatie-clients die normaal gesproken om de 90 dagen vraagt.

>[!IMPORTANT]
>De **onthouden multi-Factor Authentication** functie is niet compatibel met de **aangemeld blijven** functie van AD FS, wanneer gebruikers de verificatie in twee stappen voor AD FS via Azure multi-factor uitvoeren Authentication-Server of een oplossing van derden meervoudige verificatie.
>
>Als uw gebruikers selecteren **aangemeld blijven** voor AD FS en ook hun apparaat als vertrouwd voor multi-factor Authentication is ingeschakeld, wordt niet automatisch de gebruiker gecontroleerd nadat de **onthouden multi-factorauthenticatie**aantal dagen verloopt. Azure AD de verificatie van een nieuwe in twee stappen aanvraagt, maar de AD FS retourneert een token met het oorspronkelijke multi-Factor Authentication claim en datum, in plaats van presterende verificatie in twee stappen opnieuw. Hiermee stelt u deze reactie uit een lus verificatie tussen Azure AD en AD FS.
>

### <a name="enable-remember-multi-factor-authentication"></a>Schakel multi-factor Authentication onthouden

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **beheren onthouden multi-factorauthenticatie**, selecteer de **gebruikers toestaan om te onthouden multi-factor authentication-server op apparaten die ze vertrouwen**optie.

   ![Houd er rekening mee multi-factor Authentication voor vertrouwde apparaten](./media/multi-factor-authentication-whats-next/remember.png)

6. Stel het aantal dagen vertrouwde apparaten voor het overslaan van verificatie in twee stappen toe te staan. De standaardwaarde is 14 dagen.
7. Selecteer **Opslaan**.

### <a name="mark-a-device-as-trusted"></a>Een apparaat als vertrouwd is ingeschakeld

Nadat u de aanmeldingsgegevens onthouden multi-factor Authentication-functie hebt ingeschakeld, gebruikers wilt markeren een apparaat als vertrouwde wanneer ze zich aanmelden met **niet opnieuw vragen**.

![Selecteer "niet opnieuw vragen' voor vertrouwde apparaten](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Selecteerbare verificatiemethoden

U kunt de verificatiemethoden die beschikbaar voor uw gebruikers met behulp van zijn de _selecteerbare verificatiemethoden_ functie. De volgende tabel geeft een kort overzicht van de methoden.

Wanneer uw gebruikers hun account voor Azure multi-factor Authentication registreren, kies ze hun methode voorkeursoptie voor verificatie van de opties die u hebt ingeschakeld. Richtlijnen voor het registratieproces van de gebruiker is opgegeven [Mijn account voor verificatie in twee stappen instellen](multi-factor-authentication-end-user-first-time.md).

| Methode | Beschrijving |
|:--- |:--- |
| Bellen naar telefoon |Een geautomatiseerd telefoongesprek plaatst. De gebruiker beantwoordt het gesprek en drukt # in op de toetsenblok van de telefoon om te verifiëren. Het telefoonnummer is niet gesynchroniseerd met lokale Active Directory. |
| Sms-bericht naar telefoon |Verstuurt een tekstbericht met een verificatiecode. De gebruiker wordt gevraagd om de verificatiecode invoeren in de interface voor aanmelden. Dit proces heet SMS in één richting. SMS in twee richtingen betekent dat de gebruiker tekst weer een bepaalde code moet. SMS in twee richtingen is afgeschaft en wordt niet ondersteund na 14 November 2018. Gebruikers die zijn geconfigureerd voor SMS in twee richtingen automatisch worden overgeschakeld naar de _oproep naar telefoon_ verificatie op dat moment.|
| Melding via mobiele app |Een pushmelding verzendt naar uw telefoon of geregistreerd apparaat. De gebruiker de melding weergaven en **controleren** om verificatie te voltooien. De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Verificatiecode via mobiele app |De Microsoft Authenticator-app genereert elke 30 seconden een nieuwe OATH-verificatiecode uit. De gebruiker voert de verificatiecode in de interface voor aanmelden. De Microsoft Authenticator-app is beschikbaar voor [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), en [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="enable-and-disable-verification-methods"></a>Inschakelen en uitschakelen van verificatiemethoden

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers**.
3. Selecteer **multi-Factor Authentication**.
4. Selecteer onder multi-factor Authentication **service-instellingen**.
5. Op de **Service-instellingen** pagina onder **verificatie-opties**, selecteren/uitschakelen van de methoden om te bieden aan uw gebruikers.

   ![De verificatiemethoden selecteren](./media/multi-factor-authentication-whats-next/authmethods.png)

6. Klik op **Opslaan**.
