---
title: Azure Active Directory Identity Protection Glossary | Microsoft Docs
description: Azure Active Directory Identity Protection Glossary
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, beheer van toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid, verklarende woordenlijst
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 282daba47d106cd46c351084b9c2deb55c768a62
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55695284"
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory Identity Protection Glossary
### <a name="at-risk-user"></a>Risico (gebruiker)
Een gebruiker met een of meer actieve risicogebeurtenissen. 

### <a name="atypical-sign-in-location"></a>Ongewone aanmelding locatie
Een aanmelding vanaf een geografische locatie die is het niet gebruikelijk voor de specifieke gebruiker, vergelijkbaar gebruikers of de tenant.

### <a name="azure-ad-identity-protection"></a>Azure AD-identiteitsbeveiliging
Een beveiligingsmodule van Azure Active Directory, waarmee een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van een organisatie.

### <a name="conditional-access"></a>Voorwaardelijke toegang
Een beleid voor het beveiligen van toegang tot bronnen. Regels voor voorwaardelijke toegang worden opgeslagen in de Azure Active Directory en worden geëvalueerd door Azure AD voor het verlenen van toegang tot de resource.  Van de voorbeeldregels omvatten beperken van toegang op basis van locatie van de gebruiker, Apparaatstatus of methode voor gebruikersverificatie.

### <a name="credentials"></a>Referenties
Informatie, waaronder identificatie en proof-of-id die wordt gebruikt om toegang te krijgen van toegang tot lokale en netwerkbronnen. Voorbeelden van referenties zijn gebruikersnamen en wachtwoorden, smartcards en certificaten.

### <a name="event"></a>Gebeurtenis
Een record van een activiteit in Azure Active Directory.

### <a name="false-positive-risk-event"></a>Fout-positieve (risicogebeurtenis)
De status van een risico gebeurtenis is handmatig instellen door een gebruiker Identity Protection, waarmee wordt aangegeven dat de risicogebeurtenis is onderzocht en ten onrechte is gemarkeerd als een risicogebeurtenis.

### <a name="identity"></a>Identiteit
Een persoon of entiteit die moet worden gecontroleerd met behulp van verificatie op basis van criteria zoals wachtwoord of een certificaat.

### <a name="identity-risk-event"></a>Risicogebeurtenis identiteit
AAD-gebeurtenis die is gemarkeerd als afwijkend gemarkeerd door Identity Protection en kan duiden dat een identiteit is aangetast.

### <a name="ignored-risk-event"></a>Genegeerd (risicogebeurtenis)
De status van een risico gebeurtenis is handmatig instellen door een gebruiker Identity Protection, waarmee wordt aangegeven dat de risicogebeurtenis is gesloten zonder een herstelactie te ondernemen.

### <a name="impossible-travel-from-atypical-locations"></a>Onmogelijk traject van ongewone locaties
Een risicogebeurtenis geactiveerd wanneer er twee aanmeldingen voor dezelfde gebruiker worden gedetecteerd, waarbij ten minste één van deze vanaf een ongewone locatie aanmelden, en de tijd tussen de aanmeldingen is korter dan de minimale lang die het duren zou naar fysiek worden uitgewisseld tussen deze locaties.  

### <a name="investigation"></a>Onderzoek
Het proces voor het controleren van de activiteiten, logboeken en andere relevante informatie met betrekking tot een risicogebeurtenis om te bepalen of herstel of afname stappen nodig zijn als en begrijpen hoe de identiteit is geïnfecteerd en te begrijpen hoe de identiteit aangetast is gebruikt.

### <a name="leaked-credentials"></a>Gelekte referenties
Een risicogebeurtenis geactiveerd wanneer de huidige gebruikersreferenties (gebruikersnaam en wachtwoord) openbaar gepost in de web-donker door onze onderzoekers worden gevonden.

### <a name="mitigation"></a>Oplossing
Een actie om te beperken of de mogelijkheid van een aanvaller om misbruik te maken van een apparaat of aangetaste identiteit zonder dat de identiteit of het apparaat terugzetten naar een veilige status te elimineren. Een vorige risicogebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet wordt opgelost.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Een verificatiemethode waarvoor twee of meer verificatiemethoden, waaronder iets dat de gebruiker heeft, zoals een certificaat; iets dat de gebruiker kent, zoals gebruikersnamen, wachtwoorden of wachtwoordzinnen; fysieke kenmerken, zoals een vingerafdruk; en persoonlijke kenmerken, zoals een persoonlijke handtekening.

### <a name="offline-detection"></a>Detectie van offline
De detectie van afwijkingen en evaluatie van het risico van een gebeurtenis, zoals aanmeldingspoging na de gebeurtenis, voor een gebeurtenis die al is gebeurd.

### <a name="policy-condition"></a>Beleidsvoorwaarde
Een deel van een beveiligingsbeleid, dat de entiteiten (groepen, gebruikers, apps, apparaatplatformen, apparaatstatussen, IP-bereiken, clienttypen) in het beleid is opgenomen of uitgesloten van deze definieert.

### <a name="policy-rule"></a>Beleidsregel
Het deel van een beveiligingsbeleid dat de omstandigheden die zouden resulteren in het beleid en de acties uitgevoerd beschrijft wanneer het beleid wordt geactiveerd.

### <a name="prevention"></a>Preventie
Een actie om te voorkomen dat schade aan de organisatie via misbruik van een identiteit of het apparaat verdacht of weten om te worden aangetast. Een actie preventie beveiligt niet het apparaat of de identiteit en vorige risicogebeurtenissen niet wordt opgelost.

### <a name="privileged-user"></a>Bevoegdheden (gebruiker)
Een gebruiker die op het moment van een risicogebeurtenis permanente als tijdelijke beheerdersmachtigingen op een of meer resources in Azure Active Directory, zoals een globale beheerder, had financieel medewerker, Service-beheerder, beheerder van de gebruiker en wachtwoordbeheerder. 

### <a name="real-time"></a>Realtime
Zie realtime gedetecteerd.

### <a name="real-time-detection"></a>Realtime-detectie
De detectie van afwijkingen en evaluatie van het risico van een gebeurtenis, zoals aanmeldingspoging voordat de gebeurtenis is toegestaan om door te gaan.

### <a name="remediated-risk-event"></a>Hersteld (risicogebeurtenis)
De status van een risico gebeurtenis is automatisch ingesteld door Identity Protection, waarmee wordt aangegeven dat de risicogebeurtenis is hersteld met behulp van de standaard herstelactie voor dit type risicogebeurtenis. Bijvoorbeeld, wanneer het wachtwoord van de gebruiker opnieuw wordt ingesteld, worden veel risicogebeurtenissen die aangeven dat het vorige wachtwoord is aangetast automatisch hersteld.

### <a name="remediation"></a>Herstel
Een actie voor het beveiligen van een identiteit of een apparaat die eerder zijn verdachte of bekend is. Een herstelactie wordt de identiteit of het apparaat hersteld naar een veilige status en wordt omgezet vorige risicogebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat.

### <a name="resolved-risk-event"></a>Opgelost (risicogebeurtenis)
De status van een risico gebeurtenis is handmatig instellen door een gebruiker Identity Protection, die aangeeft dat de gebruiker heeft een juiste herstelactie buiten Identity Protection, en dat de risicogebeurtenis beschouwd als moet worden gesloten.

### <a name="risk-event-status"></a>Gebeurtenisstatus van risico 's
Een eigenschap van een risicogebeurtenis, waarmee wordt aangegeven of de gebeurtenis is actief, en indien afgesloten en de reden voor het sluiten van het.

### <a name="risk-event-type"></a>Type risicogebeurtenis
Een categorie voor de risicogebeurtenis die wijzen op het type van die van de gebeurtenis te worden beschouwd als riskant heeft veroorzaakt.

### <a name="risk-level-risk-event"></a>Risiconiveau (risicogebeurtenis)
Een indicatie (hoog, Gemiddeld of laag) van de ernst van de risicogebeurtenis Identity Protection gebruikers prioriteit van de acties die ze uitvoeren vermindert het risico voor hun organisatie. 

### <a name="risk-level-sign-in"></a>Risiconiveau (aanmelden)
Een indicatie (hoog, Gemiddeld of laag) van de kans dat voor een specifieke aanmelden, iemand anders probeert te gebruiken van de identiteit van de gebruiker.

### <a name="risk-level-user-compromise"></a>Risiconiveau (gebruiker inbreuk)
Een indicatie (hoog, Gemiddeld of laag) van de kans dat een identiteit is aangetast.

### <a name="risk-level-vulnerability"></a>Risiconiveau (beveiligingslek)
Een indicatie (hoog, Gemiddeld of laag) van de ernst van het beveiligingsprobleem Identity Protection gebruikers prioriteit van de acties die ze uitvoeren vermindert het risico voor hun organisatie.

### <a name="secure-identity"></a>Beveiligen (identiteit)
Onderneem herstelactie, zoals een wachtwoord wijzigen of de machine installatiekopieën terug van de identiteit van een potentieel aangetast herstellen op een ongeschonden status.

### <a name="security-policy"></a>Beveiligingsbeleid
Een verzameling van regels en de voorwaarde. Een beleid kan worden toegepast op entiteiten, zoals gebruikers, groepen, apps, apparaten, apparaatplatformen, apparaatstatussen, IP-bereiken en clienttypen auth 2.0. Wanneer een beleid is ingeschakeld, wordt dit als een token voor een resource is uitgegeven door een entiteit die is opgenomen in het beleid geëvalueerd.

### <a name="sign-in-v"></a>Meld u aan (v)
Om te verifiëren bij een identiteit in Azure Active Directory.

### <a name="sign-in-n"></a>Aanmelden (n)
Het proces of een actie van een identiteit in Azure Active Directory en de gebeurtenis die wordt vastgelegd met deze bewerking te verifiëren.

### <a name="sign-in-from-anonymous-ip-address"></a>Meld u aan vanaf anonieme IP-adres
Een risicogebeurtenis geactiveerd na een geslaagde aanmelding vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres.

### <a name="sign-in-from-infected-device"></a>Meld u aan vanaf een geïnfecteerd apparaat
Een risicogebeurtenis geactiveerd wanneer een aanmelding is afkomstig uit een IP-adres waarvan bekend is door een of meer aangetaste apparaten die actief zijn probeert te communiceren met een bot-server moet worden gebruikt.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Zich aanmelden vanaf een IP-adres met verdachte activiteiten
Een risicogebeurtenis geactiveerd nadat een geslaagde aanmelding vanaf een IP-adres met een groot aantal mislukte aanmeldingspogingen bij verschillende gebruikersaccounts gedurende een korte periode.

### <a name="sign-in-from-unfamiliar-location"></a>Meld u aan vanaf een onbekende locatie
Een risicogebeurtenis geactiveerd wanneer een gebruiker zich heeft aangemeld vanaf een nieuwe locatie (IP, breedtegraad/lengtegraad en ASN).

### <a name="sign-in-risk"></a>Aanmeldingsrisico
Zie risico niveau (aanmelden)

### <a name="sign-in-risk-policy"></a>Beleid voor aanmeldingsrisico's
Dit is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als de risico's voor een specifieke aanmelding en oplossingen op basis van vooraf gedefinieerde voorwaarden en regels van toepassing is.

### <a name="user-compromise-risk"></a>Inbreuk op gebruikersrisico
Zie risico niveau (gebruiker inbreuk)

### <a name="user-risk"></a>Gebruikersrisico
Zie risico niveau (gebruiker inbreuk).

### <a name="user-risk-policy"></a>Beleid voor gebruikersrisico's
Dit is een beleid voor voorwaardelijke toegang die rekening gehouden met de aanmelding en oplossingen op basis van vooraf gedefinieerde voorwaarden en regels van toepassing is.

### <a name="users-flagged-for-risk"></a>Gebruikers voor wie wordt aangegeven dat ze risico lopen
Gebruikers die risico's, welke actieve of opgeloste hebben

### <a name="vulnerability"></a>Beveiligingsprobleem
Een configuratie of een voorwaarde in Azure Active Directory, waardoor de map vatbaar voor aanvallen en aanvallen.

## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

