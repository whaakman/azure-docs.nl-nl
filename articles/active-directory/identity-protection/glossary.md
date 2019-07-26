---
title: Woorden lijst Azure Active Directory Identity Protection | Microsoft Docs
description: Azure Active Directory Identity Protection Glossary
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6751fe74bfd9b8a07aec2263582d9f1a4bc0b2ac
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333979"
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory Identity Protection Glossary

### <a name="at-risk-user"></a>Risico (gebruiker)
Een gebruiker met een of meer actieve risico gebeurtenissen. 

### <a name="atypical-sign-in-location"></a>Ongewoone aanmeldings locatie
Een aanmelding vanaf een geografische locatie die niet gebruikelijk is voor de specifieke gebruiker, soort gelijke gebruikers of de Tenant.

### <a name="azure-ad-identity-protection"></a>Azure AD-identiteitsbeveiliging
Een beveiligings module van Azure Active Directory die een geconsolideerde weer gave biedt voor risico gebeurtenissen en mogelijke beveiligings problemen die van invloed zijn op de identiteiten van een organisatie.

### <a name="conditional-access"></a>Voorwaardelijke toegang
Een beleid voor het beveiligen van de toegang tot bronnen. Regels voor voorwaardelijke toegang worden opgeslagen in de Azure Active Directory en worden geëvalueerd door Azure AD voordat ze toegang tot de resource verlenen.  Voor beelden van regels zijn het beperken van toegang op basis van de locatie van de gebruiker, de Apparaatstatus of de verificatie methode van de gebruiker.

### <a name="credentials"></a>Referenties
Informatie over identificatie en bewijs van identificatie die wordt gebruikt om toegang te krijgen tot lokale en netwerk bronnen. Voor beelden van referenties zijn gebruikers namen en wacht woorden, Smart Cards en certificaten.

### <a name="event"></a>Gebeurtenis
Een record van een activiteit in Azure Active Directory.

### <a name="false-positive-risk-event"></a>ONWAAR-positief (risico gebeurtenis)
Een status van een risico gebeurtenis is hand matig ingesteld door een gebruiker met identiteits beveiliging, waarmee wordt aangegeven dat de risico gebeurtenis is onderzocht en onjuist is gemarkeerd als een risico gebeurtenis.

### <a name="identity"></a>Identiteit
Een persoon of entiteit die moet worden geverifieerd door middel van verificatie, op basis van criteria zoals een wacht woord of een certificaat.

### <a name="identity-risk-event"></a>Identiteits risico gebeurtenis
AAD-gebeurtenis die is gemarkeerd als afwijkend door identiteits beveiliging, en kan erop wijzen dat er is geknoeid met een identiteit.

### <a name="ignored-risk-event"></a>Genegeerd (risico gebeurtenis)
Een status van een risico gebeurtenis is hand matig ingesteld door een gebruiker met identiteits beveiliging, wat aangeeft dat de risico gebeurtenis is gesloten zonder dat er een herstel actie wordt uitgevoerd.

### <a name="impossible-travel-from-atypical-locations"></a>Onmogelijk traject van ongewone locaties
Een risico gebeurtenis die wordt geactiveerd wanneer twee aanmeldingen voor dezelfde gebruiker worden gedetecteerd, waarbij ten minste een van de aanmeldingen afkomstig is van een ongewoone aanmeldings locatie en waar de tijd tussen de aanmeldings tijden korter is dan de minimale tijd die nodig is om fysiek te reizen tussen deze locaties.  

### <a name="investigation"></a>Onderzoek
Het proces van het controleren van de activiteiten, logboeken en andere relevante informatie met betrekking tot een risico gebeurtenis om te bepalen of de stappen voor herstel of beperking nood zakelijk zijn, te begrijpen of en hoe de identiteit is aangetast en hoe de aangetaste identiteit is gebruikt.

### <a name="leaked-credentials"></a>Gelekte referenties
Een risico gebeurtenis die wordt geactiveerd wanneer de huidige gebruikers referenties (gebruikers naam en wacht woord) door ons onderzoekers openbaar op het donkere web worden gevonden.

### <a name="mitigation"></a>Oplossing
Een actie om de mogelijkheid van een aanvaller te beperken of te elimineren om te voor komen dat de identiteit of het apparaat is aangetast zonder dat de identiteit of de status van een computer wordt hersteld. Bij een beperking worden eerdere risico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet opgelost.

### <a name="multi-factor-authentication"></a>Meervoudige verificatie
Een verificatie methode waarvoor twee of meer authenticatie methoden zijn vereist, die iets kunnen bevatten dat de gebruiker heeft, een dergelijk certificaat; iets wat de gebruiker weet, zoals gebruikers namen, wacht woorden of door gegeven zinsdelen; fysieke kenmerken, zoals een vinger afdruk. en persoonlijke kenmerken, zoals een persoonlijke hand tekening.

### <a name="offline-detection"></a>Offline detectie
De detectie van afwijkingen en de evaluatie van het risico van een gebeurtenis, zoals een aanmeldings poging na het feit, voor een gebeurtenis die al heeft plaatsgevonden.

### <a name="policy-condition"></a>Beleids voorwaarde
Een deel van een beveiligings beleid, waarmee de entiteiten (groepen, gebruikers, apps, apparaat-platformen, Apparaatstatus, IP-bereiken, client typen) worden gedefinieerd die zijn opgenomen in het beleid of die daarvan zijn uitgesloten.

### <a name="policy-rule"></a>Beleidsregel
Het deel van een beveiligings beleid dat de omstandigheden beschrijft waarmee het beleid wordt geactiveerd en de acties die worden uitgevoerd wanneer het beleid wordt geactiveerd.

### <a name="prevention"></a>Preventie
Een actie om schade aan de organisatie te voor komen door misbruik van een identiteit of een apparaat waarvoor wordt vermoed dat ze zijn aangetast of waarvan is geknoeid. Met een preventieve actie wordt het apparaat of de identiteit niet beveiligd en worden eerdere risico gebeurtenissen niet opgelost.

### <a name="privileged-user"></a>Privileged (gebruiker)
Een gebruiker die op het moment van een risico gebeurtenis permanente of tijdelijke beheerders machtigingen heeft voor een of meer resources in Azure Active Directory, zoals een globale beheerder, facturerings beheerder, service beheerder, gebruikers beheerder en wachtwoord beheerder. 

### <a name="real-time"></a>Realtime
Zie real-time detectie.

### <a name="real-time-detection"></a>Real-time detectie
De detectie van afwijkingen en de evaluatie van het risico van een gebeurtenis, zoals een aanmeldings poging voordat de gebeurtenis kan door gaan.

### <a name="remediated-risk-event"></a>Hersteld (risico gebeurtenis)
De status van een risico gebeurtenis wordt automatisch ingesteld op identiteits beveiliging, wat aangeeft dat de risico gebeurtenis is hersteld met behulp van de standaard actie voor door voeren voor dit type risico gebeurtenis. Als het wacht woord van de gebruiker bijvoorbeeld opnieuw wordt ingesteld, worden veel risico gebeurtenissen die aangeven dat het vorige wacht woord is aangetast automatisch hersteld.

### <a name="remediation"></a>Herstel
Een actie voor het beveiligen van een identiteit of een apparaat dat eerder is vermoed of waarvan bekend is dat deze worden aangetast. Met een herstel actie wordt de identiteit of het apparaat teruggezet naar een veilige status en worden eerdere risico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat opgelost.

### <a name="resolved-risk-event"></a>Opgelost (risico gebeurtenis)
Een status van een risico gebeurtenis is hand matig ingesteld door een gebruiker voor identiteits beveiliging, waarmee wordt aangegeven dat de gebruiker een geschikte herstel actie heeft ondernomen voor externe identiteits beveiliging en dat de risico gebeurtenis moet worden beschouwd als gesloten.

### <a name="risk-event-status"></a>Status van risico gebeurtenis
Een eigenschap van een risico gebeurtenis die aangeeft of de gebeurtenis actief is en wanneer deze is gesloten, de reden voor het sluiten ervan.

### <a name="risk-event-type"></a>Type risicogebeurtenis
Een categorie voor de risico gebeurtenis, waarmee het type afwijkingen wordt aangegeven dat de gebeurtenis heeft veroorzaakt als riskant beschouwd.

### <a name="risk-level-risk-event"></a>Risico niveau (risico gebeurtenis)
Een indicatie (hoog, gemiddeld of laag) van de ernst van de risico gebeurtenis om gebruikers met een identiteits beveiliging de prioriteit te geven van de acties die nodig zijn om het risico voor hun organisatie te verminderen. 

### <a name="risk-level-sign-in"></a>Risico niveau (aanmelden)
Een indicatie (hoog, gemiddeld of laag) van de kans dat voor een bepaalde aanmelding een andere persoon de identiteit van de gebruiker probeert te gebruiken.

### <a name="risk-level-user-compromise"></a>Risico niveau (inbreuk op gebruiker)
Een indicatie (hoog, gemiddeld of laag) van de kans dat een identiteit is aangetast.

### <a name="risk-level-vulnerability"></a>Risico niveau (beveiligingslek)
Een indicatie (hoog, gemiddeld of laag) van de ernst van het beveiligings probleem om identiteits bescherming gebruikers te helpen bij het bepalen van de acties die nodig zijn om het risico voor hun organisatie te verminderen.

### <a name="secure-identity"></a>Beveiligen (identiteit)
Herstel actie uitvoeren, zoals het wijzigen van een wacht woord of het opnieuw instellen van een machine, om een mogelijk schadelijke identiteit te herstellen naar een onaangetaste status.

### <a name="security-policy"></a>Beveiligingsbeleid
Een verzameling beleids regels en-voor waarden. Een beleid kan worden toegepast op entiteiten zoals gebruikers, groepen, apps, apparaten, apparaat-platforms, Apparaatstatus, IP-bereiken en auth 2.0-client typen. Wanneer een beleid is ingeschakeld, wordt het geëvalueerd wanneer een entiteit die is opgenomen in het beleid, een token voor een resource heeft uitgegeven.

### <a name="sign-in-v"></a>Aanmelden (v)
Verifiëren met een identiteit in Azure Active Directory.

### <a name="sign-in-n"></a>Aanmelden (n)
Het proces of de actie voor het verifiëren van een identiteit in Azure Active Directory en de gebeurtenis die deze bewerking vastlegt.

### <a name="sign-in-from-anonymous-ip-address"></a>Aanmelden vanaf een anoniem IP-adres
Een risico gebeurtenis die wordt geactiveerd na een geslaagde aanmelding vanaf een IP-adres dat is geïdentificeerd als een IP-adres van een anonieme proxy.

### <a name="sign-in-from-infected-device"></a>Aanmelden vanaf een geïnfecteerd apparaat
Een risico gebeurtenis die wordt geactiveerd wanneer een aanmelding afkomstig is van een IP-adres, die bekend is om te worden gebruikt door een of meer apparaten die zijn aangetast en die actief probeert te communiceren met een bot-server.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Meld u aan bij een IP-adres met verdachte activiteiten
Een risico gebeurtenis die wordt geactiveerd na een geslaagde aanmelding vanaf een IP-adres met een groot aantal mislukte aanmeldings pogingen over meerdere gebruikers accounts gedurende een korte periode.

### <a name="sign-in-from-unfamiliar-location"></a>Aanmelden vanaf onbekende locatie
Een risico gebeurtenis die wordt geactiveerd wanneer een gebruiker zich heeft aangemeld vanaf een nieuwe locatie (IP, breedte graad/lengte graad en ASN).

### <a name="sign-in-risk"></a>Aanmeldingsrisico
Zie risico niveau (aanmelden)

### <a name="sign-in-risk-policy"></a>Beleid voor aanmeldingsrisico's
Een beleid voor voorwaardelijke toegang dat het risico op een specifieke aanmelding evalueert en oplossingen toepast op basis van vooraf gedefinieerde voor waarden en regels.

### <a name="user-compromise-risk"></a>Risico op inbreuk van gebruiker
Zie risico niveau (inbreuk op gebruiker)

### <a name="user-risk"></a>Gebruikersrisico
Zie risico niveau (inbreuk op de gebruiker).

### <a name="user-risk-policy"></a>Beleid voor gebruikersrisico's
Een beleid voor voorwaardelijke toegang dat de aanmelding beschouwt en oplossingen op basis van vooraf gedefinieerde voor waarden en regels toepast.

### <a name="users-flagged-for-risk"></a>Gebruikers voor wie wordt aangegeven dat ze risico lopen
Gebruikers met risico gebeurtenissen die actief of hersteld zijn

### <a name="vulnerability"></a>Beveiligingsprobleem
Een configuratie of voor waarde in Azure Active Directory, waardoor de Directory gevoelig is voor aanvallen of bedreigingen.

## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
