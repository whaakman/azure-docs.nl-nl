---
title: App-beveiligingsbeleid vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Leer hoe u beveiligingsbeleid voor apps nodig voor toegang tot cloud-Apps met voorwaardelijke toegang in Azure Active Directory.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 4/4/2019
ms.author: joflore
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: f695d50e251d0104cf9f0d38fe4489a0e66dfe15
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288499"
---
# <a name="how-to-require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Procedure: App-beveiligingsbeleid vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang (Preview)

Uw werknemers gebruiken mobiele apparaten voor zowel privé- als werktaken. Terwijl te zorgen dat uw werknemers productief zijn kan, wilt u ook om te voorkomen dat gegevens verloren gaan. Met voorwaardelijke toegang van Azure Active Directory (Azure AD), kunt u uw bedrijfsgegevens beschermen door het beperken van toegang tot uw cloud-apps op client-apps die eerst een app-beveiligingsbeleid hebben.

In dit onderwerp wordt uitgelegd hoe u beleid voor voorwaardelijke toegang waarvoor app-beveiligingsbeleid voor toegang tot gegevens kunnen configureren.

## <a name="overview"></a>Overzicht

Met [voorwaardelijke toegang voor Azure AD](overview.md), kunt u aanpassen hoe gemachtigde gebruikers toegang uw resources tot hebben. U kunt bijvoorbeeld de toegang beperken tot uw cloud-apps met vertrouwde apparaten.

U kunt [Intune beveiligingsbeleid voor apps](https://docs.microsoft.com/intune/app-protection-policy) om gegevens van uw bedrijf te beveiligen. Geen nodig oplossing voor mobile device management (MDM), waarmee u ter bescherming van uw bedrijfsgegevens met of zonder inschrijving van apparaten in een oplossing voor Apparaatbeheer voor Intune beveiligingsbeleid voor apps.

Voorwaardelijke toegang van Azure Active Directory beperkt de toegang tot uw cloud-apps voor clienttoepassingen die Intune naar Azure AD als app-beveiligingsbeleid ontvangen heeft gerapporteerd. U kunt bijvoorbeeld toegang tot Exchange Online beperken voor de Outlook-app met Intune app-beveiligingsbeleid.

In de terminologie voor voorwaardelijke toegang deze client-apps bekend is dat het beleid dat is beveiligd met een **app-beveiligingsbeleid**.  

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/05.png)

Zie voor een overzicht van het beleid beveiligde client-apps, [de vereiste van het beleid voor app-beveiliging](technical-reference.md#approved-client-app-requirement).

U kunt beleid voor voorwaardelijke toegang op basis van app-beveiliging met een ander beleid zoals combineren [beleid voor voorwaardelijke toegang op basis van apparaat](require-managed-devices.md) biedt flexibiliteit bij het beveiligen van gegevens voor zowel persoonlijke als zakelijke apparaten.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Voordelen van vereiste voor app-beveiliging op basis van voorwaardelijke toegang

Vergelijkbaar met naleving worden gerapporteerd door Intune voor iOS en Android for beheerd apparaat, Intune nu rapporten aan Azure AD als app-beveiligingsbeleid is toegepast zodat voorwaardelijke toegang dit als een toegangscontrole gebruiken kunt. Dit nieuwe beleid voor voorwaardelijke toegang **App-beveiligingsbeleid** verhoogt de beveiliging door te beschermen tegen admin fouten, zoals:

- gebruikers die nog geen een Intune-licentie
- gebruikers die Intune app-beveiligingsbeleid kunnen niet ontvangen
- Intune app protection-beleid apps die niet zijn geconfigureerd voor het ontvangen van een beleid


## <a name="before-you-begin"></a>Voordat u begint

In dit onderwerp wordt ervan uitgegaan dat u bekend met bent:

- De [de vereiste van het beleid voor app-beveiliging](technical-reference.md#app-protection-policy-requirement) technische documentatie.

- De [goedgekeurde client-app vereiste](technical-reference.md#approved-client-app-requirement) technische documentatie.

- De basisconcepten van [voorwaardelijke toegang in Azure Active Directory](overview.md).

- Hoe u [beleid voor voorwaardelijke toegang configureren](app-based-mfa.md).


## <a name="prerequisites"></a>Vereisten

Voor het maken van een app-beleid voor voorwaardelijke toegang op basis van beveiliging, moet u
- Hebt u een Enterprise Mobility + Security of een Azure Active Directory premium-abonnement en Intune
- Zorg ervoor dat de gebruikers hebben een licentie voor EMS of Azure AD en Intune
- Zorg ervoor dat de client-app is geconfigureerd in Intune voor het ontvangen van een beveiligingsbeleid voor apps
- Zorg ervoor dat de gebruikers zijn geconfigureerd in Intune voor het ontvangen van een app-beveiligingsbeleid

## <a name="app-protection-based-policy-for-exchange-online"></a>App-beveiliging op basis van beleid voor Exchange Online

In dit scenario bestaat uit een app-beveiliging op basis van voorwaardelijk toegangsbeleid voor toegang tot Exchange Online.

### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:

- Hiermee configureert u e-mailadres van een systeemeigen e-mailtoepassing op iOS of Android verbinding maken met Exchange

- Een e-mailbericht ontvangen die aangeeft dat de toegang is alleen beschikbaar via de Outlook-app

- De toepassing met de koppeling downloads

- De Outlook-toepassing wordt geopend en u zich aanmeldt met de Azure AD-referenties

- Wordt gevraagd voor het installeren van de Authenticator (iOS) of bedrijfsportal-App (Android) om door te gaan

- Installeert de toepassing en kan er gaat u terug naar de Outlook-app om door te gaan

- Wordt gevraagd om een apparaat te registreren

- Kan een app-beveiligingsbeleid van Intune ontvangen

- Is geen toegang tot e-mail

Een app-beveiligingsbeleid van Intune moeten op de toepassing toegang geeft tot zakelijke gegevens en kunnen de gebruiker gevraagd om de toepassing opnieuw hebt gestart, gebruikt u een extra PINCODE enzovoort (indien geconfigureerd voor de toepassing en platform).

### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/01.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**:

    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaublad-apps** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u beschikken over **vereisen app-beveiligingsbeleid (preview)** geselecteerde.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/05.png)
 

**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met Active Sync (EAS) configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/06.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.


3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **Client-apps (preview)**. 

    a. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/92.png)

    b. Als **besturingselementen voor toegang**, moet u beschikken over **vereisen app-beveiligingsbeleid (preview)** geselecteerde.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/05.png)


**Stap 3: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.



## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>App-beveiliging op basis van of compatibel apparaatbeleid voor Exchange Online

In dit scenario bestaat uit een app-beleid op basis van beveiliging of compatibel apparaat voor voorwaardelijke toegang voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat:
 
- Een gebruiker is al geregistreerd (met of zonder zakelijke apparaten)

- Gebruikers die niet zijn ingeschreven en worden geregistreerd bij Azure AD met behulp van een app beveiligde toepassing nodig om een apparaat voor toegang tot bronnen te registreren

- Geregistreerde gebruikers met behulp van de toepassing van de app beschermd hoeven niet te opnieuw registreren het apparaat

- Gebruiker kan een app-beveiligingsbeleid ontvangen als dit niet geregistreerd

- Gebruiker kan toegang tot e-mail via Outlook en een app-beveiligingsbeleid van Intune als niet ingeschreven

- Gebruiker heeft toegang tot e-mail via Outlook als het apparaat is ingeschreven


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/62.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. 

     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**. 
 
    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Een van de geselecteerde besturingselementen vereisen**   
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/11.png)



**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met Active Sync (EAS) configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/06.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **Client-apps**. 

    Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/92.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Een van de geselecteerde besturingselementen vereisen**   
    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/11.png)



**Stap 3: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.





## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>App-beveiliging en compatibel apparaatbeleid voor Exchange Online

In dit scenario bestaat uit een voorwaardelijk toegangsbeleid op basis van app-beveiliging en compatibel apparaat voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:
 
-   Hiermee configureert u e-mailadres van een systeemeigen e-mailtoepassing op iOS of Android verbinding maken met Exchange
-   Een e-mailbericht ontvangen die aangeeft dat uw apparaat moet zijn geregistreerd voor toegang is vereist
-   De bedrijfsportal-App downloadt en zich aanmeldt bij de bedrijfsportal-App
-   E-mail wordt gecontroleerd en wordt gevraagd de Outlook-app gebruiken
-   De Outlook-app downloaden
-   Hiermee opent u de Outlook-app en voert de referenties die worden gebruikt in de inschrijving
-   Kan ontvangen voor het ontvangen van een app-beveiligingsbeleid
-   Kan toegang tot e-mail via Outlook en Intune app-beveiligingsbeleid

Alle Intune beveiligingsbeleid voor apps worden geactiveerd voordat u toegang tot de zakelijke gegevens en kunnen de gebruiker gevraagd om de toepassing opnieuw hebt gestart, gebruikt u een extra PINCODE enzovoort (indien geconfigureerd voor de toepassing en platform)


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/01.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. 

     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**. 
 
    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaublad-apps** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Alle geselecteerde besturingselementen vereisen**   
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/13.png)



**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met Active Sync (EAS) configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/06.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **Client-apps (preview)**. 

    Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/92.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Goedgekeurde client-app (preview) vereisen**

   - **Alle geselecteerde besturingselementen vereisen**   
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/13.png)




**Stap 3: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.


## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>App-beveiliging op basis van of op Apps gebaseerd beleid voor Exchange Online en SharePoint Online

In dit scenario bestaat uit een app op basis van beveiliging of goedgekeurde apps-beleid voor toegang tot Exchange Online en SharePoint Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:

- Een van beide clienttoepassingen die zijn geconfigureerd in de lijst met apps die ondersteuning bieden voor de vereiste app protection-beleid of de vereiste voor goedgekeurde apps.  
- Gebruiker maakt gebruik van clienttoepassingen die voldoen aan de vereiste app protection-beleid kunnen ontvangen een Intune-beveiligingsbeleid
- Gebruiker maakt gebruik van clienttoepassingen die voldoen aan de vereisten voor het beleid van goedgekeurde apps die ondersteuning biedt voor app-beveiligingsbeleid
- Hiermee opent u de toepassing voor toegang tot e-mail of documenten
- De Outlook-toepassing wordt geopend en u zich aanmeldt met de Azure AD-referenties
- Wordt gevraagd of u wilt installeren Authenticator (iOS) of bedrijfsportal-App (Android) om door te gaan (indien niet geïnstalleerd)
- Installeert de toepassing en kan er gaat u terug naar de Outlook-app om door te gaan
- Wordt gevraagd om een apparaat te registreren
- Kan een app-beveiligingsbeleid van Intune ontvangen
- Kan toegang tot e-mail via Outlook en Intune app-beveiligingsbeleid
- Is toegang tot sites/documenten met een app niet op de vereiste van het beleid app-beveiliging, maar die in de vereiste voor goedgekeurde app genoemd.

Alle Intune beveiligingsbeleid voor apps zijn vereist voor toegang tot de zakelijke gegevens en kunnen de gebruiker gevraagd om de toepassing opnieuw hebt gestart, gebruikt u een extra PINCODE enzovoort (indien geconfigureerd voor de toepassing en platform)

**Opmerkingen**

- U kunt dit scenario kunt gebruiken als u wilt ondersteunen beide beleidsregels voor de beveiliging op basis van en apps gebaseerde voorwaardelijke toegang.

- In deze *of* beleid, apps met **app-beveiligingsbeleid** vereiste voor toegang worden geëvalueerd voordat **goedgekeurde client-apps** vereiste.

### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/62.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. 

     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/02.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**. 
 
    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaublad-apps** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

   - **Goedgekeurde client-apps vereisen**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Een van de geselecteerde besturingselementen vereisen**   
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/12.png)


**Stap 2: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.




## <a name="next-steps"></a>Volgende stappen

Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).

Zie [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md) als u klaar bent om beleid voor voorwaardelijke toegang in Azure Active Directory te configureren. 