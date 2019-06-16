---
title: Toegang en gebruik rapporten voor Azure MFA - Azure Active Directory
description: Dit wordt beschreven hoe u de functie Azure multi-factor Authentication - rapporten.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9cf0b848e551d3c0e7d7275af9eb2c8e970dbd9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113399"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in de Azure multi-factor Authentication

Azure multi-factor Authentication biedt verschillende rapporten die kunnen worden gebruikt door u en uw organisatie die toegankelijk zijn via Azure portal. De volgende tabel bevat de beschikbare rapporten:

| Rapport | Locatie | Description |
|:--- |:--- |:--- |
| Geschiedenis van geblokkeerde gebruikers | Azure AD > MFA-Server > gebruikers blokkeren/deblokkeren | Geeft de geschiedenis van aanvragen voor het blokkeren of deblokkeren van gebruikers. |
| Gebruik en fraude waarschuwingen | Azure AD > Sign-ins | Bevat informatie over algemene gebruik, overzicht van gebruikers en de details van de gebruiker; Als een geschiedenis van Fraudewaarschuwingen die zijn ingediend in het opgegeven datumbereik. |
| Gebruik voor on-premises onderdelen | Azure AD > MFA-Server > activiteitenrapport | Bevat informatie over algemene gebruik voor MFA via de NPS-extensie, ADFS, en de MFA-server. |
| Geschiedenis van overgeslagen gebruikers | Azure AD > MFA-Server > eenmalige bypass | Biedt een geschiedenis van aanvragen voor het overslaan van multi-factor Authentication voor een gebruiker. |
| Status van de server | Azure AD > MFA-Server > status van de Server | Hiermee wordt de status van multi-factor Authentication-Servers die zijn gekoppeld aan uw account. |

## <a name="view-mfa-reports"></a>MFA-rapporten weergeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **MFA-Server**.
3. Selecteer het rapport dat u wilt weergeven.

   ![Statusrapport van MFA-Server-server in Azure portal](./media/howto-mfa-reporting/report.png)

## <a name="azure-ad-sign-ins-report"></a>Rapport van Azure AD-aanmeldingen

Met de **activiteitenrapport voor aanmeldingen** in de [Azure-portal](https://portal.azure.com), krijgt u informatie die u nodig hebt om te bepalen hoe het gaat met uw omgeving.

Het aanmeldingenrapport kunt u voorzien van informatie over het gebruik van beheerde toepassingen en aanmelding bij activiteiten van gebruikers, waaronder informatie over het gebruik van multi-factor authentication (MFA). De MFA-gegevens geven u inzicht in hoe MFA werkt in uw organisatie. Hiermee kunt u vragen beantwoorden zoals:

- Is de aanmelding gelukt met MFA?
- Hoe heeft de gebruiker MFA voltooid?
- Waarom kan de gebruiker MFA niet voltooien?
- Hoeveel gebruikers gebruiken MFA?
- Hoeveel gebruikers kunnen MFA niet voltooien?
- Wat zijn de meest voorkomende MFA-problemen waarmee eindgebruikers te maken hebben?

Deze gegevens zijn beschikbaar via de [Azure-portal](https://portal.azure.com) en de [rapportage-API](../reports-monitoring/concept-reporting-api.md).

![Azure AD-aanmeldingen rapport in de Azure portal](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Structuur van de rapport-aanmeldingen

De rapporten voor aanmeldingsactiviteiten voor MFA bieden u toegang tot de volgende informatie:

**MFA vereist:** Of MFA is vereist voor de aanmelding of niet. MFA kan worden vereist vanwege MFA per gebruiker, voorwaardelijke toegang of andere redenen. Mogelijke waarden zijn **Ja** of **Nee**.

**MFA-resultaat:** Meer informatie over of MFA is voldaan of geweigerd:

- Als MFA is voldaan, bevat deze kolom meer informatie over hoe dit is gebeurd.
   - Azure Multi-Factor Authentication
      - voltooid in de cloud
      - verlopen als gevolg van beleid dat is geconfigureerd in de tenant
      - registratie gevraagd
      - voldaan door de claim in het token
      - voldaan door de claim geleverd door de externe provider
      - voldaan door geavanceerde verificatie
      - overgeslagen omdat de uitgevoerde stroom een Windows broker-aanmeldingsstroom is
      - overgeslagen vanwege een app-wachtwoord
      - overgeslagen vanwege de locatie
      - overgeslagen vanwege een geregistreerd apparaat
      - overgeslagen vanwege een geregistreerd apparaat
      - voltooid
   - Omgeleid naar externe provider voor Multi-Factor Authentication

- Als MFA is geweigerd, bevat deze kolom de reden van weigering.
   - Azure Multi-Factor Authentication is geweigerd
      - verificatie wordt uitgevoerd
      - dubbele verificatiepoging
      - onjuiste code te vaak ingevoerd
      - ongeldige verificatie
      - ongeldig verificatiecode via mobiele app
      - onjuiste configuratie
      - telefonische oproep verzonden naar voicemail
      - telefoonnummer heeft een ongeldige indeling
      - servicefout
      - kan telefoonnummer van gebruiker niet bereiken
      - kan de mobiele-app-melding niet verzenden naar het apparaat
      - kan de mobiele-app-melding niet verzenden
      - gebruiker heeft verificatie geweigerd
      - gebruiker heeft niet gereageerd op mobiele-app-melding
      - gebruiker heeft geen verificatiemethoden geregistreerd
      - gebruiker heeft onjuiste code ingevoerd
      - gebruiker heeft onjuiste pincode ingevoerd
      - gebruiker heeft opgehangen zonder te verifiëren
      - gebruiker is geblokkeerd
      - gebruiker heeft de verificatiecode niet ingevoerd
      - gebruiker is niet gevonden
      - verificatiecode is al gebruikt

**MFA-verificatiemethode:** De verificatiemethode de gebruiker gebruikt om MFA te voltooien. Mogelijke waarden:

- Sms-bericht
- Meldingen via mobiele app
- Telefonische oproep (verificatie via telefoonnummer)
- Verificatiecode via mobiele app
- Telefonische oproep (telefoon op het werk)
- Telefonische oproep (verificatie via telefoonnummer)

**Detail van MFA-verificatie:** Verwijderde versie van het telefoonnummer, bijvoorbeeld: + X XXXXXXXX64.

**Voorwaardelijke toegang** informatie vinden over beleid voor voorwaardelijke toegang die gevolgen van de aanmeldingspoging, waaronder ondervinden:

- Beleidsnaam
- Besturingselementen toekennen
- Sessiebesturingselementen
- Resultaat

## <a name="powershell-reporting-on-users-registered-for-mfa"></a>PowerShell rapportage over gebruikers die zijn geregistreerd voor MFA

Eerst voor zorgen dat u hebt de [MSOnline V1 PowerShell-module](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0) geïnstalleerd.

Identificeer gebruikers die zich hebben geregistreerd voor MFA met behulp van PowerShell die volgt.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Gebruikers identificeren die niet hebt geregistreerd voor MFA met behulp van PowerShell die volgt.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="possible-results-in-activity-reports"></a>Resultaten in activiteitenrapporten

De volgende tabel kan worden gebruikt om op te lossen met meervoudige verificatie met behulp van de gedownloade versie van het activiteitenrapport voor meervoudige verificatie. Ze weergegeven niet rechtstreeks in Azure portal.

| Gespreksresultaat | Description | Brede beschrijving |
| --- | --- | --- |
| SUCCESS_WITH_PIN | PINCODE ingevoerd | De gebruiker een PINCODE ingevoerd.  Als de verificatie is gelukt en ze de juiste PINCODE ingevoerd.  Als verificatie is geweigerd, klikt u vervolgens deze heeft een onjuiste PINCODE ingevoerd of de gebruiker is ingesteld op standaard-modus. |
| SUCCESS_NO_PIN | Alleen # ingevoerd | Als de gebruiker is ingesteld op modus PINCODE en de verificatie is geweigerd, betekent dit dat de gebruiker heeft niet de pincode en alleen ingevoerd #.  Als de gebruiker is ingesteld op standaard-modus en dit betekent dat er een verificatie geslaagde wordt de gebruiker alleen # dat het juiste wat te doen in de standaardmodus. |
| SUCCESS_WITH_PIN_BUT_TIMEOUT | # Niet ingedrukt na het invoeren | De gebruiker heeft DTMF-cijfers niet verzenden omdat # is niet opgegeven.  Andere cijfers die zijn ingevoerd, worden niet verzonden, tenzij # wordt ingevoerd die wijzen op de voltooiing van de vermelding. |
|SUCCESS_NO_PIN_BUT_TIMEOUT | Geen telefooninvoer - time-out | De oproep is beantwoord, maar er is geen antwoord.  Dit geeft doorgaans aan dat de aanroep is opgehaald door de voicemail. |
| SUCCESS_PIN_EXPIRED | PINCODE verlopen en niet gewijzigd | De PINCODE van de gebruiker is verlopen en ze wordt gevraagd om dit te wijzigen, maar het wijzigen van de PINCODE is niet voltooid. |
| SUCCESS_USED_CACHE | Gebruikte Cache | Verificatie is voltooid zonder een multi-factor Authentication-oproep sinds de vorige verificatie is geslaagd voor de gebruikersnaam die is opgetreden binnen een tijdsbestek van de geconfigureerde cachelocatie. |
| SUCCESS_BYPASSED_AUTH | Authenticatie overgeslagen | Verificatie is voltooid met behulp van een eenmalig overslaan voor de gebruiker geïnitieerd.  Zie het rapport gebruiker geschiedenis overgeslagen voor meer informatie over de mogelijkheid tot overslaan. |
| SUCCESS_USED_IP_BASED_CACHE | IP-gebaseerde Cache gebruikt | Verificatie is voltooid zonder een multi-factor Authentication-oproep sinds de vorige verificatie is geslaagd voor de dezelfde gebruikersnaam, het verificatietype, de toepassingsnaam en IP is opgetreden binnen een tijdsbestek van de geconfigureerde cachelocatie. |
| SUCCESS_USED_APP_BASED_CACHE | App-gebaseerde Cache gebruikt | Verificatie is voltooid zonder een multi-factor Authentication-oproep sinds de vorige verificatie is geslaagd voor de dezelfde gebruikersnaam, verificatietype en de naam van de toepassing binnen een tijdsbestek van de geconfigureerde cachelocatie. |
| SUCCESS_INVALID_INPUT | Ongeldige telefooninvoer | Het antwoord verzonden via de telefoon is niet geldig.  Dit kan zijn van een faxapparaat of modem of de gebruiker hebt ingevoerd * als onderdeel van hun PINCODE. |
| SUCCESS_USER_BLOCKED | Gebruiker is geblokkeerd | Telefoonnummer van de gebruiker is geblokkeerd.  Een geblokkeerde getal kan worden gestart door de gebruiker tijdens een verificatieoproep of door een beheerder met behulp van de Azure portal. <br> OPMERKING:  Een geblokkeerde getal is ook een bijkomend gevolg van een waarschuwing voor fraude. |
| SUCCESS_SMS_AUTHENTICATED | Tekstbericht geverifieerd | Voor beide richtingen testbericht, de gebruiker juist heeft gereageerd met de eenmalige wachtwoordcode (OTP) of OTP en PINCODE. |
| SUCCESS_SMS_SENT | SMS-bericht verzonden | De SMS-bericht met de eenmalige wachtwoordcode (OTP) is voor de SMS-bericht is verzonden.  De gebruiker wordt de OTP of OTP en PINCODE invoeren in de toepassing om de verificatie te voltooien. |
| SUCCESS_PHONE_APP_AUTHENTICATED | Mobiele App geverifieerd | De gebruiker is geverifieerd via de mobiele app. |
| SUCCESS_OATH_CODE_PENDING | OATH-Code in behandeling | De gebruiker is gevraagd om hun OATH-code, maar heeft niet gereageerd. |
| SUCCESS_OATH_CODE_VERIFIED | OATH-Code geverifieerd | De gebruiker heeft ingevoerd een geldige OATH-code wanneer hierom wordt gevraagd. |
| SUCCESS_FALLBACK_OATH_CODE_VERIFIED | OATH-Terugvalcode geverifieerd | De gebruiker is verificatie met behulp van hun primaire methode voor multi-factor Authentication wordt geweigerd en vervolgens wordt er een geldige OATH-code voor terugval. |
| SUCCESS_FALLBACK_SECURITY_QUESTIONS_ANSWERED | Alternatieve beveiligingsvragen beantwoord | De gebruiker verificatie met behulp van hun primaire methode voor multi-factor Authentication is geweigerd en vervolgens of haar beveiligingsvragen correct voor terugval beantwoord. |
| FAILED_PHONE_BUSY | Verificatie wordt al uitgevoerd | Multi-factor Authentication verwerkt een verificatie voor deze gebruiker al.  Dit wordt vaak veroorzaakt door RADIUS-clients die meerdere aanvragen voor authenticatie tijdens de dezelfde aanmelding verzenden. |
| CONFIG_ISSUE | Telefoon niet bereikbaar | Aanroep is uitgevoerd, maar een kan niet worden geplaatst of niet is beantwoord.  Dit omvat bezet was, snelle bezet was (de verbinding verbroken), drie-tonen (getal niet meer in de service), time-out tijdens het wanneer ze, enzovoort. |
| FAILED_INVALID_PHONENUMBER | Ongeldige telefoonnummerindeling | Het telefoonnummer heeft een ongeldige indeling.  Telefoonnummers moet numeriek zijn en moet uit 10 cijfers bestaan voor landnummer + 1 (Verenigde Staten en Canada). |
| FAILED_USER_HUNGUP_ON_US | Gebruiker heeft opgehangen | De gebruiker de telefoon, maar vervolgens opgehangen zonder op knoppen te drukken. |
| FAILED_INVALID_EXTENSION | Ongeldige extensie | De extensie bevat ongeldige tekens.  Alleen cijfers, komma's, *, en # zijn toegestaan.  Een voorvoegsel @ kan ook worden gebruikt. |
| FAILED_FRAUD_CODE_ENTERED | Fraudecode ingevoerd | De gebruiker gekozen voor het rapport van fraude tijdens de aanroep als resultaat in een afgewezen authenticatie en een geblokkeerde telefoonnummer.| 
| FAILED_SERVER_ERROR | Kan geen verbinding | De multi-factor Authentication-service kon het gesprek. |
| FAILED_SMS_NOT_SENT | SMS-bericht kan niet worden verzonden. | De SMS-bericht kan niet worden verzonden.  De verificatie is geweigerd. |
| FAILED_SMS_OTP_INCORRECT | Tekstbericht-OTP onjuist | De gebruiker ingevoerd een onjuiste eenmalige wachtwoordcode (OTP) van de SMS-bericht ontvangen.  De verificatie is geweigerd. |
| FAILED_SMS_OTP_PIN_INCORRECT | Tekst OTP + PINCODE onjuist | De gebruiker een onjuiste eenmalige wachtwoordcode (OTP) en/of een onjuiste PINCODE van de gebruiker heeft ingevoerd.  De verificatie is geweigerd. |
| FAILED_SMS_MAX_OTP_RETRY_REACHED | Maximale tekst OTP-pogingen is overschreden | De gebruiker heeft het maximale aantal pogingen van de eenmalige wachtwoordcode (OTP) overschreden. |
| FAILED_PHONE_APP_DENIED | Mobiele App geweigerd | De verificatie in de mobiele app voor de gebruiker geweigerd door op de knop weigeren. |
| FAILED_PHONE_APP_INVALID_PIN | Ongeldige PINCODE mobiele App | De gebruiker heeft een ongeldige PINCODE ingevoerd bij het verifiëren van in de mobiele app. |
| FAILED_PHONE_APP_PIN_NOT_CHANGED | Mobiele App-PINCODE niet gewijzigd | De gebruiker is een vereiste PINCODE wijzigen in de mobiele app niet is voltooid. |
| FAILED_FRAUD_REPORTED | Fraude gerapporteerd | De gebruiker heeft fraude in de mobiele app gerapporteerd. |
| FAILED_PHONE_APP_NO_RESPONSE | Mobiele App geen antwoord | De gebruiker heeft niet gereageerd op de verificatieaanvraag mobiele app. |
| FAILED_PHONE_APP_ALL_DEVICES_BLOCKED | Mobiele App voor alle apparaten geblokkeerd voor gebruik | De mobiele Apps op apparaten voor deze gebruiker niet meer reageren op meldingen en zijn geblokkeerd. |
| FAILED_PHONE_APP_NOTIFICATION_FAILED | Mobiele-Appmelding mislukt | Er is een fout opgetreden bij het verzenden van een melding naar de mobiele app op het apparaat van de gebruiker. |
| FAILED_PHONE_APP_INVALID_RESULT | Ongeldig resultaat mobiele App | De mobiele app heeft een ongeldig resultaat geretourneerd. |
| FAILED_OATH_CODE_INCORRECT | Onjuiste OATH-Code | De gebruiker heeft een onjuiste OATH-code ingevoerd.  De verificatie is geweigerd. |
| FAILED_OATH_CODE_PIN_INCORRECT | OATH-Code + PINCODE onjuist | De gebruiker een onjuiste OATH-code en/of een onjuiste PINCODE van de gebruiker heeft ingevoerd.  De verificatie is geweigerd. |
| FAILED_OATH_CODE_DUPLICATE | Dubbele OATH-Code | De gebruiker heeft ingevoerd een OATH-code die eerder is gebruikt.  De verificatie is geweigerd. |
| FAILED_OATH_CODE_OLD | OATH-Code verouderd | De gebruiker heeft ingevoerd een OATH-code die voorafgaat aan een OATH-code die eerder is gebruikt.  De verificatie is geweigerd. |
| FAILED_OATH_TOKEN_TIMEOUT | Time-out voor resultaat van OATH-Code | De gebruiker het duurt te lang is om in te voeren van de OATH-code en de multi-factor Authentication-poging was al een time-out opgetreden. |
| FAILED_SECURITY_QUESTIONS_TIMEOUT | Time-out voor resultaat van beveiliging vragen | De gebruiker het duurt te lang is om in te voeren antwoord op vragen over de beveiliging en de multi-factor Authentication-poging was al een time-out opgetreden. |
| FAILED_AUTH_RESULT_TIMEOUT | Time-out voor Authenticatieresultaat | De gebruiker het duurt te lang om te voltooien van de multi-factor Authentication-poging. |
| FAILED_AUTHENTICATION_THROTTLED | Verificatie beperkt | De multi-factor Authentication-poging wordt beperkt door de service. |

## <a name="next-steps"></a>Volgende stappen

* [Voor gebruikers](../user-help/multi-factor-authentication-end-user.md)
* [Waar u wilt implementeren](concept-mfa-whichversion.md)
