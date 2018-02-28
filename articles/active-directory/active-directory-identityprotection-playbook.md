---
title: 'Azure Active Directory: Identity Protection playbook | Microsoft Docs'
description: Meer informatie over hoe Azure AD Identity Protection kunt u de mogelijkheid van een aanvaller misbruik maakt van een verdachte identiteit of het apparaat en voor het beveiligen van een identiteit of een apparaat dat eerder is verdacht of bekend is dat inbreuk wordt gepleegd beperken.
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: f4240c9196796c2e83c408271fe81b20842ab722
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Playbook voor Azure Active Directory: Identity Protection

Deze playbook helpt u bij:

* Gegevens in de Identity Protection omgeving vullen door risicogebeurtenissen en beveiligingsproblemen te simuleren
* Beleid voor voorwaardelijke toegang op basis van risico's instellen en de gevolgen van het beleid testen


## <a name="simulating-risk-events"></a>Risico's te simuleren

Deze sectie bevat stappen voor het simuleren van de volgende type risico-gebeurtenissen:

* Aanmeldingen vanaf anonieme IP-adressen (eenvoudig)
* Aanmeldingen vanaf onbekende locaties (gemiddeld)
* Onmogelijke reis naar ongewone locaties (moeilijk)

Andere risicogebeurtenissen kunnen niet op een veilige manier worden gesimuleerd.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen vanaf anonieme IP-adressen

Zie voor meer informatie over deze risicogebeurtenis [aanmeldingen vanaf anonieme IP-adressen](active-directory-reporting-risk-events.md#sign-ins-from-anonymous-ip-addresses). 

De volgende procedure te voltooien, moet u gebruiken:

- De [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) om te simuleren anonieme IP-adressen. Mogelijk moet u een virtuele machine gebruiken als uw organisatie het gebruik van de Tor-browser beperkt.
- Een testaccount dat nog niet is geregistreerd voor multi-factor authentication.

**De volgende stappen uitvoeren om te simuleren een aanmeldingspagina van een anoniem IP,**:

1. Met behulp van de [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en), gaat u naar [https://myapps.microsoft.com](https://myapps.microsoft.com).   
2. Gebruik de referenties van het account dat moet worden weergegeven in het **aanmeldingen vanaf anonieme IP-adressen** rapport.

De aanmeldingspagina wordt binnen 10-15 minuten weergegeven op het Identity Protection dashboard. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Aanmeldingen vanaf onbekende locaties

Zie voor meer informatie over deze risicogebeurtenis [aanmeldingen vanaf onbekende locaties](active-directory-reporting-risk-events.md#sign-in-from-unfamiliar-locations). 

Om aanmeldingen vanaf onbekende locaties te simuleren dient u aan te melden vanaf een locatie en een apparaat waar uw testaccount nog niet eerder is aangemeld.

De volgende procedure maakt gebruik van een nieuw gemaakte:

- VPN-verbinding, een nieuwe locatie te simuleren.

- Virtuele machine naar een nieuw apparaat te simuleren.

Om de volgende procedure te voltooien, moet u een gebruikersaccount gebruiken dat voldoet aan de volgende voorwaarden:

- Ten minste 30-daagse aanmelden in het verleden.
- Multi-factor authentication is ingeschakeld.


**De volgende stappen uitvoeren om een aanmelding vanaf een onbekende locatie te simuleren,**:

1. Laat bij het aanmelden met uw testaccount de MFA-controle mislukken.
2. Navigeer met uw nieuwe VPN naar [https://myapps.microsoft.com](https://myapps.microsoft.com) en voer de referenties van uw testaccount in.
   

De aanmeldingspagina wordt weergegeven op het dashboard Identity Protection binnen 10-15 minuten.

### <a name="impossible-travel-to-atypical-location"></a>Onmogelijke reis naar ongewone locatie

Zie voor meer informatie over deze risicogebeurtenis [Impossible op reis naar ongewone locatie](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations). 

Het is moeilijk om de onmogelijke reis te simuleren omdat het machine learning algoritme gebruikt wordt om de Fout-positieven zoals een onmogelijke reis vanaf vertrouwde apparaten of aanmeldingen vanaf VPN's die worden gebruikt door andere gebruikers er uit te filteren. Daarnaast vereist het algoritme een aanmeldgeschiedenis van 14 dagen en 10 aanmeldingen van de gebruiker voordat deze begint met het genereren van risico's. Vanwege de complexe machine learning-modellen en bovenstaande regels, is er een kans dat de volgende stappen niet tot een risicogebeurtenis leidt. Mogelijk wilt u deze stappen met meerdere Azure AD-accounts herhalen voordat u resultaat ziet op de aanmeldingspagina van het Identity Protection dashboard.


**De volgende stappen uitvoeren om te simuleren van een onmogelijke reis naar ongewone locatie,**:

1. Navigeer met uw standaardbrowser naar [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Voer de referenties van het account dat u wilt gebruiken voor het genereren van een onmogelijke reis risicogebeurtenis.
3. Wijzig uw gebruikersagent. U kunt uw gebruikersagent in Internet Explorer van hulpprogramma's voor ontwikkelaars wijzigen of uw gebruikersagent in Firefox of Chrome met de invoegtoepassing van een gebruikersagent schakelbaar wijzigen.
4. Wijzig het IP-adres. U kunt uw IP-adres wijzigen met behulp van een VPN, een invoegtoepassing Tor, of van een nieuwe machine in Azure in een ander datacenter draait.
5. Meldt u aan bij [https://myapps.microsoft.com](https://myapps.microsoft.com) met dezelfde referenties als in de eerdere stap. Zorg er ook voor dat het binnen een paar minuten na de vorige aanmelding is.

De aanmeldingspagina wordt weergegeven in het dashboard Identity Protection binnen 2-4 uur.

## <a name="simulating-vulnerabilities"></a>Beveiligingsproblemen simuleren
Zwakke plekken zijn zwakke punten in een Azure AD-omgeving die door een kwaadwillende kan worden misbruikt. Momenteel worden 3 typen zwakke plekken opgehaald in Azure AD Identity Protection die gebruikmaken van andere functies van Azure AD. Deze beveiligingslekken worden automatisch weergegeven op het Identity Protection dashboard zodra deze functies zijn ingesteld.

* Azure AD [multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 


## <a name="testing-security-policies"></a>Testen van beveiligingsbeleid

Deze sectie bevat stappen voor het testen van het risico van de gebruiker en het beveiligingsbeleid Aanmelden risico.


### <a name="user-risk-security-policy"></a>Gebruiker risico beveiligingsbeleid

Zie voor meer informatie [gebruiker risico beveiligingsbeleid](active-directory-identityprotection.md#user-risk-security-policy).

![Gebruiker risico](./media/active-directory-identityprotection-playbook/02.png "Playbook")


**De volgende stappen uitvoeren om te testen in een beveiligingsbeleid voor gebruiker risico,**:

1. Aanmelden bij [https://portal.azure.com](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.
2. Navigeer naar **Identity Protection**. 
3. Op de **Azure AD Identity Protection** pagina, klikt u op **risico gebruikersbeleid**.
4. In de **toewijzingen** sectie, selecteer de gewenste gebruikers (en groepen) en het risiconiveau van de gebruiker.

    ![Gebruiker risico](./media/active-directory-identityprotection-playbook/03.png "Playbook")

5. Selecteer de gewenste toegangsbeheer in de sectie besturingselementen (bijvoorbeeld wachtwoordwijziging vereist).
5. Als **beleid afdwingen**, selecteer **uit**.
6. De bevoegdheden het risico van de gebruiker van een testaccount, bijvoorbeeld een van de risicogebeurtenissen een paar keer simuleren.
7. Wacht een paar minuten en controleer vervolgens of gebruikersniveau voor uw gebruiker normaal. Als dat niet het geval is, moet u meer risico's voor de gebruiker te simuleren.
8. Als **beleid afdwingen**, selecteer **op**.
9. Nu kunt u voorwaardelijke toegang voor gebruikers op basis van risico testen door in het gebruik van een gebruiker met een verhoogde risiconiveau te ondertekenen.
    
    

### <a name="sign-in-risk-security-policy"></a>Beveiligingsbeleid Aanmelden risico

Zie voor meer informatie [gebruiker risico beveiligingsbeleid](active-directory-identityprotection.md#user-risk-security-policy).

![Aanmelden risico](./media/active-directory-identityprotection-playbook/01.png "Playbook")


**Als u wilt testen een teken in risico beleid, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij [https://portal.azure.com ](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.

2. Navigeer naar **Azure AD Identity Protection**.

3. Op de hoofdblade **Azure AD Identity Protection** pagina, klikt u op **aanmelden risico beleid**. 

4. In de **toewijzingen** sectie, selecteer de gewenste gebruikers (en groepen) en meld u risiconiveau.

    ![Aanmelden risico](./media/active-directory-identityprotection-playbook/04.png "Playbook")


5. In de **besturingselementen** sectie, selecteert u het gewenste toegangsbeheer (bijvoorbeeld **meervoudige authenticatie**). 

6. Als **beleid afdwingen**, selecteer **op**.

7. Klik op **Opslaan**.

8. U kunt nu voorwaardelijke toegang op basis van het risico van de aanmeldingspagina testen aanmelden via een riskante sessie (bijvoorbeeld met behulp van de Tor-browser). 

 




## <a name="see-also"></a>Zie ook

- [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

