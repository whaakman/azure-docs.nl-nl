---
title: Azure Active Directory Identity Protection playbook | Microsoft Docs
description: Meer informatie over hoe Azure AD Identity Protection kunt u de mogelijkheid van een aanvaller misbruik te maken van een apparaat of aangetaste identiteit en beveiliging van een identiteit of een apparaat dat eerder is verdacht of bekend is dat inbreuk worden gepleegd beperken.
services: active-directory
keywords: Azure active directory identity protection cloud discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 02d402b7de82631ce459c60cb42e5335c7e7cfe3
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003297"
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Playbook voor Azure Active Directory Identity Protection

Dit playbook helpt u bij:

* Gegevens in de omgeving Identity Protection te vullen door risicogebeurtenissen en beveiligingsproblemen te simuleren
* Een beleid voor voorwaardelijke toegang op basis van risico's in te stellen en de gevolgen van het beleid te testen


## <a name="simulating-risk-events"></a>Risicogebeurtenissen simuleren

Deze sectie bevat stappen voor het simuleren van de volgende typen risico-gebeurtenissen:

* Aanmeldingen vanaf anonieme IP-adressen (eenvoudig)
* Aanmeldingen vanaf onbekende locaties (gemiddeld)
* Onmogelijke reis naar ongewone locaties (moeilijk)

Andere risicogebeurtenissen kunnen niet op een veilige manier worden gesimuleerd.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen vanaf anonieme IP-adressen

Zie voor meer informatie over deze risicogebeurtenis [aanmeldingen vanaf anonieme IP-adressen](../reports-monitoring/concept-risk-events.md#sign-ins-from-anonymous-ip-addresses). 

De volgende procedure te voltooien, moet u gebruiken:

- De [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) om anonieme IP-adressen te simuleren. Mogelijk moet u een virtuele machine gebruiken als uw organisatie het gebruik van de Tor-browser beperkt.
- Een testaccount die nog niet is geregistreerd voor meervoudige verificatie.

**De volgende stappen uitvoeren om te simuleren een aanmelding vanaf een anoniem IP-adres,**:

1. Met behulp van de [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en), gaat u naar [ https://myapps.microsoft.com ](https://myapps.microsoft.com).   
2. Gebruik de referenties van het account dat moet worden weergegeven in het rapport **aanmeldingen vanaf anonieme IP-adressen**.

De aanmeldingspagina wordt binnen 10-15 minuten weergegeven op het Identity Protection-dashboard. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Aanmeldingen vanaf onbekende locaties

Zie voor meer informatie over deze risicogebeurtenis [aanmeldingen vanaf onbekende locaties](../reports-monitoring/concept-risk-events.md#sign-in-from-unfamiliar-locations). 

Om aanmeldingen vanaf onbekende locaties te simuleren, dient zich u aan te melden vanaf een locatie en een apparaat waar u zich nog niet eerder hebt aangemeld met uw testaccount.

De volgende procedure maakt gebruik van een nieuw gemaakte:

- VPN-verbinding voor het simuleren van de nieuwe locatie.

- De virtuele machine voor het simuleren van een nieuw apparaat.

Om de volgende procedure te voltooien, moet u een gebruikersaccount gebruiken dat voldoet aan de volgende voorwaarden:

- Ten minste een 30-daagse aanmelden geschiedenis.
- Multi-factor authentication is ingeschakeld.


**Voer de volgende stappen uit om een aanmelding vanaf een onbekende locatie te simuleren**:

1. Laat bij het aanmelden met uw testaccount de MFA-controle mislukken.
2. Met behulp van de nieuwe VPN-, gaat u naar [ https://myapps.microsoft.com ](https://myapps.microsoft.com) en voer de referenties van uw testaccount.
   

De aanmeldingspagina wordt binnen 10-15 minuten weergegeven op het Identity Protection-dashboard.

### <a name="impossible-travel-to-atypical-location"></a>Onmogelijke reis naar ongewone locatie

Zie voor meer informatie over deze risicogebeurtenis [onmogelijk reis naar ongewone locatie](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations). 

De voorwaarde onmogelijk traject simuleren is moeilijk, omdat het algoritme maakt gebruik van machine learning om te selecteren om fout-positieven zoals onmogelijk traject van vertrouwde apparaten of aanmeldingen van VPN's die worden gebruikt door andere gebruikers in de map. Bovendien vereist het algoritme een geschiedenis aanmelden van 14 dagen en 10 aanmeldingen van de gebruiker voordat deze wordt gestart met het genereren van risico's. Vanwege de complexe machine learning-modellen en hoger regels, is er een kans is dat de volgende stappen niet tot een risicogebeurtenis leiden zal. Het is raadzaam om te repliceren van deze stappen voor meerdere Azure AD-accounts voor het publiceren van deze risicogebeurtenis.


**De volgende stappen uitvoeren om te simuleren een onmogelijke reis naar ongewone locatie,**:

1. Gebruik uw standaardbrowser, Ga naar [ https://myapps.microsoft.com ](https://myapps.microsoft.com).  
2. Voer de referenties in van het account dat u wilt gebruiken om een risicogebeurtenis voor een onmogelijke reis te genereren.
3. Wijzig uw gebruikersagent. U kunt uw gebruikersagent in Internet Explorer wijzigen vanuit Ontwikkelhulpprogramma's of in Firefox of Chrome door een invoegtoepassing voor het wisselen van de gebruikersagent te gebruiken.
4. Uw IP-adres wijzigen. U kunt uw IP-adres wijzigen met behulp van een VPN, een Tor-invoegtoepassing, of instellen een nieuwe virtuele machine in Azure in een ander datacenter.
5. Aanmelden bij [ https://myapps.microsoft.com ](https://myapps.microsoft.com) met dezelfde referenties als voorheen en binnen een paar minuten na de vorige aanmelding.

De aanmelding wordt weergegeven in het dashboard Identity Protection binnen 2-4 uur.

## <a name="simulating-vulnerabilities"></a>Beveiligingsproblemen simuleren
Zwakke plekken zijn zwakke punten in een Azure AD-omgeving die door een kwaadwillend persoon kunnen worden misbruikt. Momenteel zijn er 3 typen zwakke plekken in Azure AD Identity Protection bekend die gebruikmaken van andere functies van Azure AD. Deze beveiligingslekken worden automatisch weergegeven op het Identity Protection-dashboard zodra deze functies zijn ingesteld.


* Azure AD [multi-factor Authentication](../authentication/multi-factor-authentication.md)
* Azure AD [Cloud Discovery](https://docs.microsoft.com/cloud-app-security/).
* Azure AD [Privileged Identity Management](../privileged-identity-management/pim-configure.md). 


## <a name="testing-security-policies"></a>Testen van beveiligingsbeleid

Deze sectie bevat stappen voor het testen van het gebruikersrisico en het beveiligingsbeleid van het aanmeldingsrisico.


### <a name="user-risk-security-policy"></a>Beleid voor gebruikersrisico's security

Zie voor meer informatie, [beleid voor gebruikersrisico's security](overview.md#user-risk-security-policy).

![Gebruikersrisico](./media/playbook/02.png "Playbook")


**De volgende stappen uitvoeren om te testen in een beleid voor gebruikersrisico's security,**:

1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.
2. Navigeer naar **Identity Protection**. 
3. Op de **Azure AD Identity Protection** pagina, klikt u op **beleid voor gebruikersrisico's**.
4. In de **toewijzingen** sectie, selecteer de gewenste gebruikers (en groepen) en risiconiveau van de gebruiker.

    ![Gebruikersrisico](./media/playbook/03.png "Playbook")

5. Selecteer de gewenste toegangsbeheer in de sectie besturingselementen (bijvoorbeeld wachtwoordwijziging vereisen).
5. Als **beleid afdwingen**, selecteer **uit**.
6. Verhoogde bevoegdheden het gebruikersrisico van een testaccount, bijvoorbeeld, een van de risicogebeurtenissen simuleren van een paar keer.
7. Wacht een paar minuten en controleer vervolgens of het gebruikersniveau van de voor de gebruiker normaal. Als dat niet het geval is, moet u meer risicogebeurtenissen voor de gebruiker simuleren.
8. Als **beleid afdwingen**, selecteer **op**.
9. U kunt nu voorwaardelijke toegang voor gebruikers op basis van risico testen als u zich aanmeldt bij het gebruik van een gebruiker met een niveau verhoogd risico.
    
    

### <a name="sign-in-risk-security-policy"></a>Beveiligingsbeleid voor aanmeldingsrisico

Zie voor meer informatie, [beleid voor gebruikersrisico's security](overview.md#user-risk-security-policy).

![Aanmeldingsrisico](./media/playbook/01.png "Playbook")


**Als u wilt testen een teken in het beleid voor gebruikersrisico's, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij [ https://portal.azure.com ](https://portal.azure.com) met referenties van de globale beheerder voor uw tenant.

2. Navigeer naar **Azure AD Identity Protection**.

3. Op de hoofdpagina **Azure AD Identity Protection** pagina, klikt u op **aanmelden beleid voor gebruikersrisico's**. 

4. In de **toewijzingen** sectie, selecteer de gewenste gebruikers (en groepen) en meld u risiconiveau.

    ![Aanmeldingsrisico](./media/playbook/04.png "Playbook")


5. In de **besturingselementen** sectie, selecteer de gewenste Access control (bijvoorbeeld **meervoudige verificatie vereisen**). 

6. Als **beleid afdwingen**, selecteer **op**.

7. Klik op **Opslaan**.

8. U kunt nu voorwaardelijke toegang op basis van risico's van de aanmelding testen als u zich aanmeldt bij het gebruik van een riskante sessie (bijvoorbeeld via de browser Tor). 

 




## <a name="see-also"></a>Zie ook

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

