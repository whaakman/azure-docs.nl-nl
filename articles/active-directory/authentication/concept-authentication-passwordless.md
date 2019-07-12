---
title: Azure Active Directory zonder wachtwoord uitproberen aanmelden (preview)
description: De configuratie aanmelden bij Azure AD met behulp van beveiligingssleutels FIDO2 of de Microsoft Authenticator-app (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d80b359be0a6249327ba1ba1d51ffbc330bb073
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712083"
---
# <a name="what-is-passwordless"></a>Wat is het zonder wachtwoord uitproberen?

Multi-factor authentication (MFA) is een uitstekende manier voor het beveiligen van uw organisatie, maar gebruikers gefrustreerd zijn met de extra laag bovenop mailservers te hoeven onthouden hun wachtwoorden ontvangen. De configuratie verificatiemethoden zijn eenvoudiger omdat het wachtwoord is verwijderd en vervangen door iets hebt plus iets dat die u bent of iets dat die u weet.

|   | Iets dat die u hebt | Iets dat u weet of |
| --- | --- | --- |
| Zonder wachtwoord | Sleutel voor de telefoon of beveiliging | Biometrische of PINCODE |

Wordt herkend dat elke organisatie verschillende behoeften heeft als het gaat om verificatie. Microsoft biedt momenteel Windows Hello, onze premier zonder wachtwoord uitproberen-ervaring voor de Windows-PC. Er zijn nieuwe referenties toegevoegd aan de configuratie: Microsoft Authenticator-app en FIDO2 beveiligingssleutels.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator-App

Telefoon om te worden van een methode voor verificatie van de configuratie van uw werknemer toestaan. U al gebruikt de Microsoft Authenticator-App als een handige multi-factor authentication-optie naast een wachtwoord. Maar het is nu beschikbaar als een optie voor de configuratie.

Het verandert iOS- of Android-telefoon in een sterk, zonder wachtwoord uitproberen referentie doordat gebruikers zich aanmelden op elk platform of de browser door een melding naar de telefoon aan, die overeenkomt met een getal dat wordt weergegeven op het scherm op die hun telefoon en vervolgens met behulp van hun biometrische) touch of geconfronteerd) of PINCODE om te bevestigen.

## <a name="fido2-security-keys"></a>Beveiligingssleutels FIDO2

Beveiligingssleutels FIDO2 zijn een unphishable standaarden op basis van de configuratie verificatiemethode die kan worden geleverd in alle vormfactor. Snelle identiteit Online (FIDO) is een open standaard voor de configuratie-verificatie. Kunnen gebruikers en organisaties gebruikmaken van de standaard aan te melden met hun bronnen zonder een gebruikersnaam of wachtwoord met behulp van een externe beveiliging-sleutel of een platformsleutel die is ingebouwd in een apparaat.

Voor de openbare preview, werknemers externe sleutels gebruiken voor aanmelding bij naar de Azure Active Directory gekoppelde Windows 10-computers (met versie 1809 of hoger) en ophalen van eenmalige aanmelding hun cloudresources. Ook kunnen ze zich aanmelden voor ondersteunde browsers.

Er zijn veel sleutels die gecertificeerd door de FIDO-Alliance FIDO2 zijn, Microsoft vereist dat sommige optionele uitbreidingen van de specificatie FIDO2 CTAP moeten worden geïmplementeerd door de leverancier om ervoor te zorgen voor een maximale beveiliging en de beste ervaring.

Een beveiligingssleutel **moet** implementeren de volgende functies en uitbreidingen van het protocol FIDO2 CTAP Microsoft-compatibel zijn met:

| # | Functie / extensie vertrouwen | Waarom is dit vereist? |
| --- | --- | --- |
| 1 | Residente sleutel | Deze functie kunt de beveiligingssleutel zijn draagbaar, waar u uw referenties op de beveiligingssleutel is opgeslagen. |
| 2 | Pincode van de client | Deze functie kunt u uw referenties met een tweede factor beveiligen en is van toepassing op beveiligingssleutels waarvoor geen een gebruikersinterface. |
| 3 | hmac-secret | Deze extensie zorgt ervoor dat u kunt aanmelden bij uw apparaat wanneer het offline of in vliegtuigmodus. |
| 4 | Meerdere accounts per RP | Deze functie zorgt ervoor dat u kunt de dezelfde beveiligingssleutel over meerdere services, zoals Microsoft-Account en Azure Active Directory. |

De volgende providers bieden FIDO2 beveiligingssleutels van verschillende vormfactoren die bekend is dat deze compatibel is met de paswordless-ervaring. Microsoft adviseert klanten om te evalueren van de eigenschappen van de beveiliging van deze sleutels contact opnemen met de leverancier, evenals de FIDO-Alliance.

| Provider | Neem contact op met |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feitian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| VERBORGEN | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurity | [https://ensurity.com/contact-us.html](https://ensurity.com/contact-us.html) |
| eWBM | [https://www.ewbm.com/page/sub1_5](https://www.ewbm.com/page/sub1_5) |

Als u een leverancier en wilt u uw apparaat laten in deze lijst staat, neem dan contact op met [ Fido2Request@Microsoft.com ](mailto:Fido2Request@Microsoft.com).

Beveiligingssleutels FIDO2 zijn een goede optie voor bedrijven die zijn zeer gevoelige beveiliging of scenario's of werknemers die niet bereid of kunnen hun telefoon gebruiken als tweede factor.

## <a name="what-scenarios-work-with-the-preview"></a>Welke scenario's werken met de preview?

1. Beheerders kunnen de configuratie verificatiemethoden inschakelen voor hun tenant
1. Beheerders kunnen alle gebruikers of gebruikers/groepen binnen hun tenant voor elke methode selecteren
1. Eindgebruikers kunnen registreren en beheren van deze configuratie verificatiemethoden in de accountportal
1. Eindgebruikers kunnen aanmelden met deze configuratie verificatiemethoden
   1. Microsoft Authenticator-App: Werk in elk scenario waarin Azure AD-verificatie wordt gebruikt, wordt waaronder in alle browsers tijdens Windows 10 uit van de setup vak (OOBE), en met geïntegreerde mobiele apps in elk besturingssysteem.
   1. Sleutels: Werken op de website in de ondersteunde browsers, zoals Microsoft Edge en vergrendelingsscherm voor Windows 10, versie 1809 of hoger.

## <a name="next-steps"></a>Volgende stappen

[Opties voor de configuratie in uw organisatie inschakelen](howto-authentication-passwordless-enable.md)

### <a name="external-links"></a>Externe koppelingen

[FIDO-Alliance](https://fidoalliance.org/)

[FIDO2 CTAP-specificatie](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
