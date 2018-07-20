---
title: Azure Active Directory-verificatiemethoden
description: Welke verificatiemethoden zijn beschikbaar in Azure AD voor MFA en Self-service voor Wachtwoordherstel
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: 48f3a77d2aa81cda62f8206709268bae8e7c8737
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163916"
---
# <a name="what-are-authentication-methods"></a>Wat zijn verificatiemethoden?

Azure AD selfservice voor wachtwoordherstel (SSPR) en multi-factor Authentication (MFA) kunnen voor meer informatie, ook wel verificatiemethoden of -beveiligingsgegevens, om te bevestigen dat u die u zegt te zijn bij het gebruik van de bijbehorende functies vragen.

Beheerders kunnen definiëren in welke verificatiemethoden beschikbaar voor gebruikers van SSPR en MFA zijn-beleid. Sommige verificatiemethoden zijn mogelijk niet beschikbaar voor alle functies.

Microsoft raadt beheerders kunnen gebruikers meer dan de minimaal vereiste aantal van de ondersteunde verificatiemethoden selecteren in het geval ze geen toegang tot één.

|Verificatiemethode|Gebruik|
| --- | --- |
| Wachtwoord | MFA en Self-service voor Wachtwoordherstel |
| Beveiligingsvragen | Alleen SSPR |
| E-mailadres | Alleen SSPR |
| Microsoft Authenticator-app | Alleen MFA |
| Sms | MFA en Self-service voor Wachtwoordherstel |
| Spraakoproep | MFA en Self-service voor Wachtwoordherstel |
| App-wachtwoorden | MFA alleen in bepaalde gevallen |

## <a name="password"></a>Wachtwoord

Uw Azure AD-wachtwoord wordt beschouwd als een verificatiemethode. Dit is het een methode die **kan niet worden uitgeschakeld**.

## <a name="security-questions"></a>Beveiligingsvragen

Beveiligingsvragen zijn beschikbaar **alleen in Azure AD Self-service voor wachtwoord opnieuw instellen** naar niet-beheerdersaccounts.

Als u vragen over de beveiliging gebruikt, raden wij deze gebruikt in combinatie met een andere methode. Vragen over de beveiliging is minder veilig dan andere methoden, omdat sommige mensen u de antwoorden op vragen van een andere gebruiker kent mogelijk.

> [!NOTE]
> Beveiligingsvragen zijn privé en veilig opgeslagen op een gebruikersobject in de map en kunnen alleen door gebruikers worden beantwoord tijdens de registratie. Er is geen manier voor een beheerder om te lezen of wijzigen van de gebruiker vragen of antwoorden.
>

### <a name="predefined-questions"></a>Vooraf gedefinieerde vragen

* In welke stad hebt u uw eerste partner ontmoet?
* In welke stad hebben uw ouders elkaar leren kennen?
* In welke stad woont uw jongste of oudste broer of zus?
* In welke stad is uw vader geboren?
* In welke plaats hebt u uw eerste baan gehad?
* In welke stad is uw moeder geboren?
* In welke plaats was u op oudejaarsavond in 2000?
* Wat is de achternaam van uw favoriete leraar op de middelbare school?
* Voor welke universiteit bent u uitgeloot?
* Waar hebt u uw trouwreceptie gehouden?
* Wat is de tweede naam van uw vader?
* Wat is uw lievelingseten?
* Wat zijn de voor- en achternaam van uw oma van moederskant?
* Wat is de tweede naam van uw moeder?
* Wat is uw oudste Broer of zus geboren maand en jaar? (bijvoorbeeld November 1985.)
* Wat is de tweede naam van uw oudste broer of zus?
* Wat zijn de voor- en achternaam van uw opa van vaderskant?
* Wat is de tweede naam van uw jongste broer of zus?
* Op welke school zat u in de brugklas?
* Wat zijn de voor- en achternaam van uw beste jeugdvriend?
* Wat zijn de voor- en achternaam van uw eerste partner?
* Wat is de achternaam van uw favoriete leraar op de lagere school?
* Wat zijn het merk en model van uw eerste auto of motor?
* Wat is de naam van uw eerste school?
* In welk ziekenhuis bent u geboren?
* In welke straat staat of stond het huis waar u bent opgegroeid?
* Wat is de naam van uw jeugdidool?
* Wat is de naam van uw favoriete knuffeldier?
* Wat is de naam van uw eerste huisdier?
* Wat was uw bijnaam als kind?
* Wat was uw favoriete sport op de middelbare school?
* Wat was uw eerste baan?
* Wat zijn of waren de laatste vier cijfers van het telefoonnummer van uw ouderlijk huis?
* Wat wilde u worden toen u klein was?
* Wie is de grootste beroemdheid die u ooit hebt ontmoet?

Alle van de vooraf gedefinieerde beveiligingsvragen zijn vertaald en gelokaliseerd in de volledige set met Office 365-talen op basis van landinstellingen van de browser van de gebruiker.

### <a name="custom-security-questions"></a>Aangepaste beveiligingsvragen

Aangepaste beveiligingsvragen zijn niet gelokaliseerd. Alle aangepaste vragen worden weergegeven in dezelfde taal als ze zijn ingevoerd in de interface van de gebruiker met beheerdersrechten, zelfs als de landinstellingen van de browser van de gebruiker is anders. Als u gelokaliseerde vragen, moet u de vooraf gedefinieerde vragen.

De maximale lengte van een aangepaste beveiligingsvraag is 200 tekens.

### <a name="security-question-requirements"></a>Vraag beveiligingsvereisten

* Het maximum aantal tekens minimumlengte voor antwoord is drie tekens.
* De maximale beantwoorden tekenlimiet is 40 tekens.
* De dezelfde vraag meer dan één keer kunnen niet worden beantwoord door gebruikers.
* Gebruikers kunnen niet hetzelfde antwoord op meer dan één vraag opgeven.
* Een tekenset kan worden gebruikt om de vragen en antwoorden, met inbegrip van Unicode-tekens te definiëren.
* Het aantal vragen dat is gedefinieerd moet groter zijn dan of gelijk zijn aan het aantal vragen die zijn vereist om u te registreren.

## <a name="email-address"></a>E-mailadres

E-mailadres is beschikbaar **alleen in Azure AD Self-service voor wachtwoord opnieuw instellen**.

Microsoft raadt het gebruik van een e-mailaccount dat geen van de gebruiker Azure AD-wachtwoord voor toegang tot nodig.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator-app

De Microsoft Authenticator-app biedt een extra beveiligingsniveau voor uw Azure AD-werk of school-account of uw Microsoft-account.

De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) en [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

### <a name="notification-through-mobile-app"></a>Melding via mobiele app

De Microsoft Authenticator-app kunt u voorkomt ongeoorloofde toegang tot accounts en frauduleuze transacties stoppen door het pushen van een melding naar uw smartphone of tablet. Gebruikers weergeven van de melding en wanneer deze geldig is, is Selecteer verifiëren. Anders kunnen ze weigeren selecteren.

### <a name="verification-code-from-mobile-app"></a>Verificatiecode via mobiele app

De Microsoft Authenticator-app of andere apps van derden kunnen worden gebruikt als een Softwaretoken voor het genereren van een OAuth-verificatiecode. Na het invoeren van uw gebruikersnaam en wachtwoord, voert u de code die is geleverd door de app in het aanmeldingsscherm. De verificatiecode zorgt voor een tweede vorm van verificatie.

## <a name="mobile-phone"></a>Mobiele telefoon

Er zijn twee opties beschikbaar voor gebruikers met mobiele telefoons.

Als gebruikers niet dat het mobiele telefoonnummer wilt zichtbaar is in de map, maar ze nog steeds wilt gebruiken voor wachtwoord opnieuw instellen, moeten beheerders niet vullen het in de map. Gebruikers moeten invullen hun **telefoon voor authenticatie** kenmerk de [-registratieportal voor wachtwoordherstel](https://aka.ms/ssprsetup). Beheerders kunnen deze informatie in het profiel van de gebruiker zien, maar deze niet elders wordt gepubliceerd.

Voor een goede werking telefoonnummers moet zich in de indeling *+ CountryCode PhoneNumber*, bijvoorbeeld: + 1 4255551234.

> [!NOTE]
> Er moet een spatie tussen de landcode en het telefoonnummer.
>
> Wachtwoord opnieuw instellen biedt geen ondersteuning voor extensies van de telefoon. Zelfs in de indeling van de 4255551234 + 1 X-12345, worden de extensies worden verwijderd voordat de oproep wordt gedaan.

### <a name="text-message"></a>Sms-bericht

Een SMS-bericht is verzonden naar het mobiele nummer met een verificatiecode. Voer de verificatiecode die is opgegeven in de interface aanmelden om door te gaan.

### <a name="phone-call"></a>Telefoonoproep

Een geautomatiseerd telefoongesprek is naar het telefoonnummer dat u opgeeft gemaakt. Beantwoord de oproep en druk op # in het toetsenblok telefoon verifiëren

## <a name="office-phone"></a>Telefoon (werk)

Een geautomatiseerd telefoongesprek is naar het telefoonnummer dat u opgeeft gemaakt. Beantwoord het gesprek en drukt # in het toetsenblok van de telefoon om te verifiëren.

Voor een goede werking telefoonnummers moet zich in de indeling *+ CountryCode PhoneNumber*, bijvoorbeeld: + 1 4255551234.

Het kenmerk van de telefoon office wordt beheerd door uw beheerder.

> [!NOTE]
> Er moet een spatie tussen de landcode en het telefoonnummer.
>
> Wachtwoord opnieuw instellen biedt geen ondersteuning voor extensies van de telefoon. Zelfs in de indeling van de 4255551234 + 1 X-12345, worden de extensies worden verwijderd voordat de oproep wordt gedaan.

## <a name="app-passwords"></a>Appwachtwoorden

Bepaalde niet-browsertoepassingen geen ondersteuning voor meervoudige verificatie, als een gebruiker is ingeschakeld voor multi-factor authentication en proberen te gebruiken van niet-browsertoepassingen, kan niet worden geverifieerd zijn. Een app-wachtwoord kan gebruikers om door te gaan om te verifiëren

Als u multi-factor Authentication via beleid voor voorwaardelijke toegang en niet via MFA per gebruiker afdwingen, kunt u geen app-wachtwoorden maken. Toepassingen die gebruikmaken van beleid voor voorwaardelijke toegang om toegang te beheren, geen app-wachtwoorden nodig.

Als uw organisatie is gefedereerd voor eenmalige aanmelding met Azure AD en u wilt gebruikmaken van Azure MFA, klikt u rekening houden met de volgende gegevens:

* Het app-wachtwoord wordt gecontroleerd door Azure AD en daarom omzeilt federation. Federatie wordt alleen gebruikt bij het instellen van app-wachtwoorden. Voor federatieve gebruikers (SSO), worden wachtwoorden opgeslagen in de organisatie-ID. Als de gebruiker het bedrijf verlaat, heeft dat informatie naar de organisatie-ID met behulp van DirSync stromen. Account uitschakelen/verwijderen kan tot drie uur duren om te synchroniseren die uitschakelen/verwijderen van app-wachtwoorden in Azure AD uitstelt.
* On-premises instellingen voor toegangsbeheer van client worden niet herkend door het app-wachtwoord.
* Geen on-premises verificatie logboekregistratie/controle mogelijkheid is beschikbaar voor app-wachtwoorden.
* Bepaalde geavanceerde ontwerpen van de architectuur mogelijk met behulp van een combinatie van organisatie-gebruikersnaam en wachtwoorden en app-wachtwoorden als u de verificatie in twee stappen voor clients, afhankelijk van waar ze worden geverifieerd. Voor clients die verificatie op basis van een on-premises infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij Azure AD, gebruikt u het app-wachtwoord.
* Gebruikers kunnen geen app-wachtwoorden maken standaard. Als u toestaan dat gebruikers wilt kunnen maken van app-wachtwoorden, selecteer de **gebruikers toestaan te maken van app-wachtwoorden aan te melden bij niet-browsertoepassingen optie** onder service-instellingen.

## <a name="next-steps"></a>Volgende stappen

[Selfservice voor wachtwoord opnieuw instellen voor uw organisatie inschakelen](quickstart-sspr.md)

[Azure multi-factor Authentication inschakelen voor uw organisatie](howto-mfa-getstarted.md)