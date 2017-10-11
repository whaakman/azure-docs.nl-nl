---
title: Verklarende woordenlijst voor beveiliging van Azure Active Directory-identiteit | Microsoft Docs
description: Verklarende woordenlijst voor beveiliging van Azure Active Directory-identiteit
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid, verklarende woordenlijst
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 2cf64925cff9a78cf83532a1cfd231f7a1d98304
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Verklarende woordenlijst voor beveiliging van Azure Active Directory-identiteit
### <a name="at-risk-user"></a>Risico (gebruiker)
Een gebruiker met een of meer actieve risico's. 

### <a name="atypical-sign-in-location"></a>Ongewone aanmelding locatie
Een aanmelden vanaf een geografische locatie die niet standaard voor de specifieke gebruiker, vergelijkbare gebruikers of de tenant.

### <a name="azure-ad-identity-protection"></a>Azure AD-identiteitsbeveiliging
Een beveiligingsmodule van Azure Active Directory waarmee een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van een organisatie.

### <a name="conditional-access"></a>Voorwaardelijke toegang
Een beleid voor het beveiligen van toegang tot bronnen. Regels voor voorwaardelijke toegang in de Azure Active Directory worden opgeslagen en worden geëvalueerd door Azure AD voordat het verlenen van toegang tot de resource.  Voorbeeld van de regels omvatten beperken van toegang op basis van de locatie van de gebruiker, apparaat status of de gebruiker de verificatiemethode.

### <a name="credentials"></a>Referenties
Informatie met identificatie en bewijs van identificatie die wordt gebruikt om toegang tot lokale en netwerkbronnen te krijgen. Voorbeelden van referenties zijn gebruikersnamen en wachtwoorden, smartcards en certificaten.

### <a name="event"></a>Gebeurtenis
Een record van een activiteit in Azure Active Directory.

### <a name="false-positive-risk-event"></a>Fout-positieve (risicogebeurtenis)
Een risico gebeurtenisstatus handmatig instellen door een gebruiker Identity Protection, waarmee wordt aangegeven dat de risicogebeurtenis is onderzocht en is foutief gemarkeerd als een risicogebeurtenis.

### <a name="identity"></a>Identiteit
Een persoon of entiteit die moet worden gecontroleerd met behulp van verificatie op basis van criteria zoals wachtwoord of een certificaat.

### <a name="identity-risk-event"></a>Identiteit risicogebeurtenis
AAD-gebeurtenis die is gemarkeerd als afwijkend door Identity Protection, en kan wijzen op een identiteit is aangetast.

### <a name="ignored-risk-event"></a>Genegeerd (risicogebeurtenis)
Een risico gebeurtenisstatus handmatig instellen door een gebruiker Identity Protection, waarmee wordt aangegeven dat de risicogebeurtenis zonder een herstelactie is gesloten.

### <a name="impossible-travel-from-atypical-locations"></a>Onmogelijke reis van ongewone locaties
Een risicogebeurtenis geactiveerd wanneer er twee aanmeldingen voor dezelfde gebruiker gedetecteerd, waarbij ten minste één van deze vanaf een locatie voor ongebruikelijk aanmelden, en de tijd tussen de aanmeldingen korter is dan de minimale tijd die nodig hebben zou voor fysiek worden uitgewisseld tussen deze locaties.  

### <a name="investigation"></a>Onderzoek
Het proces voor het controleren van de activiteiten, logboeken en andere relevante informatie die betrekking hebben op een risicogebeurtenis om te bepalen of de updateherstel- of risicobeperking stappen vereist zijn, begrijpt als en hoe de identiteit is geknoeid en begrijpen hoe de identiteit van de verdachte is gebruikt.

### <a name="leaked-credentials"></a>Gelekte aanmeldingsreferenties
Een risicogebeurtenis geactiveerd wanneer de huidige gebruikersreferenties (gebruikersnaam en wachtwoord) worden gevonden door onze onderzoekers openbaar in het donkere web geplaatst.

### <a name="mitigation"></a>Oplossing
Een actie te beperken of de mogelijkheid van een aanvaller misbruik maakt van een verdachte identiteit of het apparaat zonder dat de identiteit of het apparaat hersteld naar een veilige status elimineren. Een risicobeperking vorige risico's die zijn gekoppeld aan de identiteit of het apparaat niet is opgelost.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Een verificatiemethode waarbij twee of meer verificatiemethoden, waaronder iets de gebruiker heeft, zoals een certificaat; iets de gebruiker kent, zoals gebruikersnamen, wachtwoorden of wachtwoordzinnen; fysieke kenmerken, zoals een vingerafdruk; en persoonlijke kenmerken, zoals een persoonlijke handtekening.

### <a name="offline-detection"></a>Offline-detectie
De detectie van afwijkingen en evaluatie van het risico van een gebeurtenis, zoals aanmeldingspoging achteraf, voor een gebeurtenis die al is gebeurd.

### <a name="policy-condition"></a>Beleidsvoorwaarde
Een deel van een beveiligingsbeleid waarmee de entiteiten (groepen, gebruikers, apps, apparaatplatforms, de statussen van apparaten, IP-adresbereiken, clienttypen) in het beleid is opgenomen of uitgesloten van deze zijn gedefinieerd.

### <a name="policy-rule"></a>Beleidsregel
Het deel van een beveiligingsbeleid dat de omstandigheden die zou resulteren in het beleid en de acties uitgevoerd beschrijft wanneer het beleid wordt geactiveerd.

### <a name="prevention"></a>Preventie
Een actie om te voorkomen dat schade aan de organisatie via misbruik van een identiteit of het apparaat wordt vermoed of mogelijk onveilig weten. Een actie preventie van het apparaat of de identiteit niet wordt beveiligd en vorige risicogebeurtenissen kan niet worden omgezet.

### <a name="privileged-user"></a>Bevoegdheden (gebruiker)
Een gebruiker die op het moment van een risicogebeurtenis op permanente als tijdelijke beheerdersmachtigingen voor een of meer resources in Azure Active Directory, zoals een globale beheerder had financieel medewerker, servicebeheerder beheerderrol en wachtwoordbeheerder. 

### <a name="real-time"></a>Realtime
Zie realtime detectie.

### <a name="real-time-detection"></a>Realtime-detectie
De detectie van afwijkingen en evaluatie van het risico van een gebeurtenis, zoals aanmeldingspoging voordat de gebeurtenis is toegestaan om door te gaan.

### <a name="remediated-risk-event"></a>Hersteld (risicogebeurtenis)
De status van een risico gebeurtenis is automatisch ingesteld door Identity Protection, waarmee wordt aangegeven dat de risicogebeurtenis is hersteld met behulp van de standaard herstelactie voor dit type risicogebeurtenis. Bijvoorbeeld, wanneer de gebruikerswachtwoord opnieuw is ingesteld, worden veel risicogebeurtenissen die aangeven dat de vorige wachtwoord is aangetast automatisch hersteld.

### <a name="remediation"></a>Herstel
Een actie voor het beveiligen van een identiteit of een apparaat die eerder zijn verdachte of bekend is mogelijk onveilig. Een herstelactie herstelt u de identiteit of het apparaat naar een veilige status en wordt omgezet vorige risico's die zijn gekoppeld aan de identiteit of het apparaat.

### <a name="resolved-risk-event"></a>Opgelost (risicogebeurtenis)
Een risico gebeurtenisstatus handmatig instellen door een gebruiker Identity Protection, die aangeeft dat de gebruiker een juiste herstelactie buiten Identity Protection heeft en dat de gebeurtenis risico's nodig hebt gesloten.

### <a name="risk-event-status"></a>De gebeurtenisstatus risico
Een eigenschap van een risicogebeurtenis die aangeeft of de gebeurtenis actief is, en als gesloten, de reden waarom u deze sluit.

### <a name="risk-event-type"></a>Gebeurtenistype risico
Een categorie voor de risicogebeurtenis, die het type van de afwijkingsdetectie waardoor de gebeurtenis worden overwogen riskant aangeeft.

### <a name="risk-level-risk-event"></a>Risiconiveau (risicogebeurtenis)
Een indicatie (hoog, Gemiddeld of laag) van de ernst van de risicogebeurtenis voor Identity Protection gebruikers prioriteren van de acties die ze duren vermindert het risico voor hun organisatie. 

### <a name="risk-level-sign-in"></a>Risiconiveau (sign-in)
Een aanduiding (hoog, Gemiddeld of laag) van de kans dat voor een specifieke aanmelden, iemand anders probeert te gebruiken de identiteit van de gebruiker.

### <a name="risk-level-user-compromise"></a>Risiconiveau (gebruiker inbreuk)
Een aanduiding (hoog, Gemiddeld of laag) van de kans dat een identiteit is aangetast.

### <a name="risk-level-vulnerability"></a>Risiconiveau (beveiligingsprobleem)
Een indicatie (hoog, Gemiddeld of laag) van de ernst van de kwetsbaarheid voor Identity Protection gebruikers prioriteren van de acties die ze duren vermindert het risico voor hun organisatie.

### <a name="secure-identity"></a>Secure (identiteit)
Nemen herstelactie zoals een wachtwoord wijzigen of de machine voor het herstellen van een eventueel verdachte identiteit op een ongeschonden status wordt teruggezet.

### <a name="security-policy"></a>Beveiligingsbeleid
Een verzameling van regels en de voorwaarde. Een beleid kan worden toegepast op entiteiten, zoals gebruikers, groepen, apps, apparaten, apparaatplatforms, de statussen van apparaten, IP-adresbereiken en Auth2.0 clienttypen. Wanneer een beleid is ingeschakeld, wordt dit geëvalueerd wanneer een token voor een bron is uitgegeven door een entiteit die is opgenomen in het beleid.

### <a name="sign-in-v"></a>Meld u aan (v)
Voor de verificatie een identiteit in Azure Active Directory.

### <a name="sign-in-n"></a>Aanmelden (n)
Het proces of de actie van een identiteit in Azure Active Directory en de gebeurtenis die deze bewerking legt vast te verifiëren.

### <a name="sign-in-from-anonymous-ip-address"></a>Aanmelden vanaf anonieme IP-adres
Een risicogebeurtenis geactiveerd na een geslaagde aanmelden van IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres.

### <a name="sign-in-from-infected-device"></a>Aanmelden van geïnfecteerde apparaten
Een risicogebeurtenis geactiveerd wanneer een aanmeldingspagina afkomstig van een IP-adres waarvan bekend is door een of meer aangetaste apparaten die actief zijn probeert is te communiceren met een bot-server moet worden gebruikt.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Aanmelden van IP-adres met verdachte activiteiten
Een risicogebeurtenis wordt geactiveerd nadat een geslaagde aanmelden van een-IP-adres met een groot aantal mislukte aanmeldingspogingen bij verschillende gebruikersaccounts gedurende een korte periode.

### <a name="sign-in-from-unfamiliar-location"></a>Aanmelden van onbekende locatie
Een risicogebeurtenis geactiveerd wanneer een gebruiker zich met succes aanmeldt vanaf een nieuwe locatie (IP-breedtegraad/lengtegraad en ASN).

### <a name="sign-in-risk"></a>Aanmelden risico
Zie risico niveau (sign-in)

### <a name="sign-in-risk-policy"></a>Beleid voor aanmelden risico
Een beleid voor voorwaardelijke toegang dat het risico op een specifieke aanmelden wordt geëvalueerd en oplossingen op basis van vooraf gedefinieerde voorwaarden en regels van toepassing is.

### <a name="user-compromise-risk"></a>Gebruiker risico op inbreuk
Zie risico niveau (gebruiker inbreuk)

### <a name="user-risk"></a>Gebruiker risico
Zie risico niveau (gebruiker inbreuk).

### <a name="user-risk-policy"></a>Gebruikersbeleid risico
Een beleid voor voorwaardelijke toegang die beschouwt de aanmeldingspagina en oplossingen op basis van vooraf gedefinieerde voorwaarden en bepalingen toepast.

### <a name="users-flagged-for-risk"></a>Gebruikers voor wie wordt aangegeven dat ze risico lopen
Gebruikers die risicogebeurtenissen zijn die actief of herstelde hebben

### <a name="vulnerability"></a>Beveiligingslek
Een configuratie of een voorwaarde in Azure Active Directory, waardoor de map vatbaar voor aanvallen of bedreigingen.

## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

