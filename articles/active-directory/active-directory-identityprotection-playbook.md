---
title: 'Azure Active Directory: Identity Protection playbook | Microsoft Docs'
description: Meer informatie over hoe Azure AD Identity Protection kunt u de mogelijkheid van een aanvaller misbruik maakt van een verdachte identiteit of het apparaat en voor het beveiligen van een identiteit of een apparaat dat eerder is verdacht of bekend is dat inbreuk wordt gepleegd beperken.
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 2ecd07faed785fa6aa179ac1cca35a70d965e1dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Playbook voor Azure Active Directory: Identity Protection
Deze playbook helpt u bij:

* Gegevens in de omgeving Identity Protection door simulating risicogebeurtenissen en beveiligingsproblemen vullen
* Beleid voor voorwaardelijke toegang op basis van risico's instellen en de gevolgen van het beleid testen

## <a name="simulating-risk-events"></a>Risico's te simuleren
Deze sectie bevat stappen voor het simuleren van de volgende typen van de risico-gebeurtenis:

* Aanmeldingen vanaf anonieme IP-adressen (eenvoudig)
* Aanmeldingen vanaf onbekende locaties (gemiddeld)
* Onmogelijke reis naar ongewone locaties (moeilijk)

Andere risicogebeurtenissen kunnen niet op een veilige manier worden gesimuleerd.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen vanaf anonieme IP-adressen
Dit type risico gebeurtenis identificeert gebruikers die zich heeft aangemeld vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres. Deze proxy's worden gebruikt door mensen die u wilt verbergen, IP-adres van het apparaat en kan worden gebruikt voor kwade bedoelingen.

**De volgende stappen uitvoeren om te simuleren een aanmeldingspagina van een anoniem IP,**:

1. Download de [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).
2. Met de Tor-Browser, Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3. Voer de referenties van het account moet worden weergegeven de **aanmeldingen vanaf anonieme IP-adressen** rapport.

De aanmeldingspagina wordt weergegeven op het dashboard Identity Protection binnen 5 minuten. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Aanmeldingen vanaf onbekende locaties
Het risico van onbekende locaties is een van realtime-in-evaluatiemechanisme dat uit het verleden aanmelden locaties overweegt (IP, breedtegraad / lengtegraad en ASN) om te bepalen van de nieuwe / onbekende locaties. Het systeem slaat vorige IP-adressen, de breedtegraad / lengtegraad en ASN's van een gebruiker en wordt deze aan het vertrouwde locaties worden beschouwd. Een locatie aanmelden wordt beschouwd als niet bekend als de locatie aanmelden komt niet overeen met een van de bestaande vertrouwde locaties.

Beveiliging van Azure Active Directory-identiteit:  

* heeft een initiële learning periode van 14 dagen aan waarover biedt het geen nieuwe locaties als onbekende locaties vlag.
* aanmeldingen vanaf bekende apparaten en de locaties die geografisch zich dicht bij een bestaande vertrouwde locatie negeert.

Om te simuleren onbekende locaties, die u moet zich aanmelden via een locatie en het apparaat dat het account is niet aangemeld uit voordat u. 

**De volgende stappen uitvoeren om te simuleren een aanmelden vanaf een onbekend locatie,**:

1. Kies een account met ten minste een geschiedenis van de aanmeldingspagina 14 dagen. 
2. Doen:
   
   a. Tijdens het gebruik van een VPN, gaat u naar [https://myapps.microsoft.com](https://myapps.microsoft.com) en voer de referenties van het account dat u wilt de risicogebeurtenis voor simuleren.
   
   b. Vraag een koppelen in een andere locatie kunnen aanmelden met de accountreferenties (niet aanbevolen).

De aanmeldingspagina wordt weergegeven op het dashboard Identity Protection binnen 5 minuten.

### <a name="impossible-travel-to-atypical-location"></a>Onmogelijke reis naar ongewone locatie
Het is moeilijk om de voorwaarde onmogelijke reis simuleren omdat de algoritme machine learning gebruikt voor wieden uit ONWAAR-positieven zoals onmogelijke reis vanaf vertrouwde apparaten of aanmeldingen vanaf VPN's die worden gebruikt door andere gebruikers in de map. Daarnaast vereist het algoritme een geschiedenis aanmelden van 3 tot en met 14 dagen voor de gebruiker voordat u begint deze risico's te genereren.

**De volgende stappen uitvoeren om te simuleren van een onmogelijke reis naar ongewone locatie,**:

1. Navigeer met uw standaardbrowser naar [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Voer de referenties van het account dat u wilt genereren van een onmogelijke reis risicogebeurtenis voor.
3. Uw gebruikersagent wijzigen. U kunt gebruikersagent in Internet Explorer van hulpprogramma's voor ontwikkelaars wijzigen of uw gebruikersagent in Firefox of Chrome met de invoegtoepassing van een gebruikersagent schakelbaar wijzigen.
4. Wijzig het IP-adres. U kunt uw IP-adres wijzigen met behulp van een VPN, een invoegtoepassing Tor, of van een nieuwe machine in Azure in een ander datacenter draait.
5. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com) met dezelfde referenties als voordat en binnen een paar minuten na de vorige aanmelden.

De aanmeldingspagina wordt weergegeven in het dashboard Identity Protection binnen 2-4 uur.<br>
Vanwege de complexe machine learning-modellen die zijn betrokken, is er een kans dat deze wordt niet ophalen opgepikt.<br> Mogelijk wilt deze stappen over meerdere Azure AD-accounts worden gerepliceerd.

## <a name="simulating-vulnerabilities"></a>Beveiligingsproblemen simuleren
Zwakke plekken zijn zwakke punten in een Azure AD-omgeving die door een onjuiste acteur kan worden misbruikt. Momenteel worden 3 typen zwakke plekken die opgehaald in Azure AD Identity Protection die gebruikmaken van andere functies van Azure AD. Deze beveiligingslekken wordt weergegeven op het dashboard Identity Protection automatisch zodra deze functies zijn ingesteld.

* Azure AD [multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="user-compromise-risk"></a>Gebruiker risico op inbreuk
**De volgende stappen uitvoeren om het risico op inbreuk gebruiker testen,**:

1. Aanmelden bij [https://portal.azure.com](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.
2. Navigeer naar **Identity Protection**. 
3. Op de hoofdblade **Azure AD Identity Protection** blade, klikt u op **instellingen**. 
4. Op de **instellingen voor de Gebruikersportal** blade onder **beveiligingsregels**, klikt u op **gebruiker inbreuk risico**. 
5. Op de **aanmelden risico** blade inschakelen **inschakelen regel** uit en klik vervolgens op **opslaan** instellingen.
6. Voor een bepaald gebruikersaccount simuleren een onbekende locaties of anonieme IP-risicogebeurtenis. Hiermee wordt het risiconiveau van de gebruiker voor die gebruiker bevoegdheden **gemiddeld**.
7. Wacht een paar minuten en controleer vervolgens dat gebruikersniveau voor de gebruiker is **gemiddeld**.
8. Ga naar de **instellingen voor de Gebruikersportal** blade.
9. Op de **gebruiker inbreuk risico** blade onder **inschakelen regel**, selecteer **op** . 
10. Selecteer een van de volgende opties:
    
    a. Om te blokkeren, selecteer **gemiddeld** onder **blok aanmelden**.
    
    b. Afdwingen van beveiligd wachtwoord wijzigen **gemiddeld** onder **meervoudige authenticatie**.
11. Klik op **Opslaan**.
12. U kunt nu voorwaardelijke toegang op basis van risico's wanneer u zich aanmeldt bij het gebruik van een gebruiker met een verhoogde risiconiveau testen. Als het risico van de gebruiker normaal, afhankelijk van de configuratie van uw beleid voor het aanmelden is worden geblokkeerd of u gedwongen om uw wachtwoord te wijzigen. 
    <br><br>
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    <br>

## <a name="sign-in-risk"></a>Aanmelden risico
**Test een teken in de risico's door de volgende stappen uitvoeren:**

1. Aanmelden bij [https://portal.azure.com ](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.
2. Navigeer naar **Identity Protection**.
3. Op de hoofdblade **Azure AD Identity Protection** blade, klikt u op **instellingen**. 
4. Op de **instellingen voor de Gebruikersportal** blade onder **beveiligingsregels**, klikt u op **aanmelden risico**.
5. Op de ** aanmelden risico ** blade Selecteer **op** onder **inschakelen regel**. 
6. Selecteer een van de volgende opties:
   
   a. Als u wilt blokkeren, selecteer **gemiddeld** onder **blok aanmelden**
   
   b. Afdwingen van beveiligd wachtwoord wijzigen **gemiddeld** onder **meervoudige authenticatie**.
7. Selecteer om te blokkeren, gemiddeld onder blok aanmelden.
8. Afdwingen van multi-factor authentication-server **gemiddeld** onder **meervoudige authenticatie**.
9. Klik op **Opslaan**.
10. U kunt nu voorwaardelijke toegang op basis van risico's testen door simuleren onbekende locaties of anoniem IP gebeurtenissen risico omdat ze beide **gemiddeld** bestaat de kans dat gebeurtenissen.


![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")


## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

