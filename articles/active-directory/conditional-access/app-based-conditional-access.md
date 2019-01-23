---
title: Goedgekeurde client-apps vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Informatie over het goedgekeurde client-apps vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang in Azure Active Directory.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 20beff7accbc987842141fa7ece9c17f11b0032e
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448802"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>Procedures: Goedgekeurde client-apps vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang 

Uw werknemers gebruiken mobiele apparaten voor zowel privé- als werktaken. Terwijl te zorgen dat uw werknemers productief zijn kan, wilt u ook om te voorkomen dat gegevens verloren gaan. Met voorwaardelijke toegang van Azure Active Directory (Azure AD), kunt u de toegang beperken tot uw cloud-apps op goedgekeurde client-apps die uw bedrijfsgegevens kunnen beveiligen.  

In dit onderwerp wordt uitgelegd hoe u toegangsbeleid voorwaarde waarvoor goedgekeurde client-apps configureren.

## <a name="overview"></a>Overzicht

Met [voorwaardelijke toegang voor Azure AD](overview.md), kunt u aanpassen hoe gemachtigde gebruikers toegang uw resources tot hebben. U kunt bijvoorbeeld de toegang beperken tot uw cloud-apps met vertrouwde apparaten.

U kunt [Intune beveiligingsbeleid voor apps](https://docs.microsoft.com/intune/app-protection-policy) om gegevens van uw bedrijf te beveiligen. Geen nodig oplossing voor mobile device management (MDM), waarmee u ter bescherming van uw bedrijfsgegevens met of zonder inschrijving van apparaten in een oplossing voor Apparaatbeheer voor Intune beveiligingsbeleid voor apps.

Azure Active Directory voor voorwaardelijke toegang kunt u de toegang tot uw cloud-apps op client-apps die ondersteuning bieden voor Intune beveiligingsbeleid voor apps beperken. U kunt bijvoorbeeld toegang tot Exchange Online beperken voor de Outlook-app.

In de terminologie voor voorwaardelijke toegang deze client-apps staat bekend als **goedgekeurde client-apps**.  


![Voorwaardelijke toegang](./media/app-based-conditional-access/05.png)


Zie voor een lijst van goedgekeurde client-apps, [goedgekeurde client-app vereiste](technical-reference.md#approved-client-app-requirement).


U kunt beleid voor voorwaardelijke toegang op basis van apps met een ander beleid zoals combineren [beleid voor voorwaardelijke toegang op basis van apparaat](require-managed-devices.md) biedt flexibiliteit bij het beveiligen van gegevens voor zowel persoonlijke als zakelijke apparaten.

 


## <a name="before-you-begin"></a>Voordat u begint

In dit onderwerp wordt ervan uitgegaan dat u bekend met bent:

- De [goedgekeurde client-app vereiste](technical-reference.md#approved-client-app-requirement) technische documentatie.


- De basisconcepten van [voorwaardelijke toegang in Azure Active Directory](overview.md).

- Hoe u [beleid voor voorwaardelijke toegang configureren](app-based-mfa.md).

- De [migratie van beleid voor voorwaardelijke toegang](best-practices.md#policy-migration).
 

## <a name="prerequisites"></a>Vereisten

Voor het maken van een beleid voor voorwaardelijke toegang op basis van apps, moet u een Enterprise Mobility + Security of een Azure Active Directory premium-abonnement hebben en de gebruikers moeten een licentie hebben voor EMS of Azure AD. 


## <a name="exchange-online-policy"></a>Beleid voor Exchange Online 

In dit scenario bestaat uit een beleid voor voorwaardelijke toegang op basis van Apps voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:

- Hiermee configureert u e-mailadres van een systeemeigen e-mailtoepassing op iOS of Android verbinding maken met Exchange

- Een e-mailbericht ontvangen die aangeeft dat de toegang is alleen beschikbaar via de Outlook-app

- De toepassing met de koppeling downloads

- De Outlook-toepassing wordt geopend en u zich aanmeldt met de Azure AD-referenties

- Wordt gevraagd voor het installeren van de Authenticator (iOS) of bedrijfsportal-App (Android) om door te gaan

- Installeert de toepassing en kan er gaat u terug naar de Outlook-app om door te gaan

- Wordt gevraagd om een apparaat te registreren

- Is geen toegang tot e-mail

Alle Intune beveiligingsbeleid voor apps worden geactiveerd op het moment van de toegang tot zakelijke gegevens en kan de gebruiker gevraagd om de toepassing opnieuw hebt gestart, gebruikt u een extra PINCODE enzovoort (indien geconfigureerd voor de toepassing en platform).

### <a name="configuration"></a>Configuratie 

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/01.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**:

    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaublad-apps** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/05.png)
 

**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met Active Sync (EAS) configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/06.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.


3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **Client-apps (preview)**. 

    a. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/92.png)

    b. Als **besturingselementen voor toegang**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/05.png)


**Stap 3: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.


## <a name="exchange-online-and-sharepoint-online-policy"></a>Beleid voor Exchange Online en SharePoint Online

In dit scenario bestaat uit een voorwaardelijke toegang met mobile Application management-beleid voor toegang tot Exchange Online en SharePoint Online met goedgekeurde apps.

### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:

- Probeert de SharePoint-app gebruiken om verbinding te maken en om hun zakelijke sites weer te geven

- Poging om aan te melden met dezelfde referenties als de referenties van de app Outlook

- Heeft geen opnieuw wilt registreren en toegang kunnen krijgen tot de resources


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online en SharePoint Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/71.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.


3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online** en **Office 365 SharePoint Online**. 

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/02.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**:

    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/05.png)




**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met Active Sync (EAS) configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/06.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. Online 

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **Client-apps**:

    a. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/92.png)

    b. Als **besturingselementen voor toegang**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/05.png)




**Stap 3: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Apparaatbeleid voor Exchange Online en SharePoint Online op basis van apps of compatibel

In dit scenario bestaat uit een beleid op basis van apps of voldoen aan het beleid voor voorwaardelijke toegang voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat:
 
- Een gebruiker al zijn ingeschreven (met of zonder zakelijke apparaten)

- Gebruikers die niet zijn ingeschreven en worden geregistreerd bij Azure AD met behulp van een app beveiligde toepassing nodig om een apparaat voor toegang tot bronnen te registreren

- Geregistreerde gebruikers met behulp van de toepassing van de app beschermd hoeven niet te opnieuw registreren het apparaat


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online en SharePoint Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/62.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online** en **Office 365 SharePoint Online**. 

     ![Voorwaardelijke toegang](./media/app-based-conditional-access/02.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**. 
 
    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

    - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

    - **Goedgekeurde client-app (preview) vereisen**

    - **Een van de geselecteerde besturingselementen vereisen**   
 
    ![Voorwaardelijke toegang](./media/app-based-conditional-access/11.png)



**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met Active Sync (EAS) configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/61.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **Client-apps**. 

    Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.
 
    ![Voorwaardelijke toegang](./media/app-based-conditional-access/11.png)




**Stap 3: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Beleid voor Exchange Online en SharePoint Online op basis van Apps en compatibel apparaat

In dit scenario bestaat uit een voorwaardelijk toegangsbeleid op basis van Apps en compatibel apparaat voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:
 
-   Hiermee configureert u e-mailadres van een systeemeigen e-mailtoepassing op iOS of Android verbinding maken met Exchange
-   Een e-mailbericht ontvangen die aangeeft dat uw apparaat moet zijn geregistreerd voor toegang is vereist
-   De bedrijfsportal-App downloadt en zich aanmeldt bij de bedrijfsportal-App
-   E-mail wordt gecontroleerd en wordt gevraagd de Outlook-app gebruiken
-   De Outlook-app downloaden
-   Hiermee opent u de Outlook-app en voert de referenties die worden gebruikt in de inschrijving
-   Gebruiker is geen toegang tot e-mail

Alle Intune beveiligingsbeleid voor apps worden geactiveerd op het moment van de toegang tot de zakelijke gegevens en kunnen de gebruiker gevraagd om de toepassing opnieuw hebt gestart, gebruikt u een extra PINCODE enzovoort (indien geconfigureerd voor de toepassing en platform)


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online en SharePoint Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/62.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online** en **Office 365 SharePoint Online**. 

     ![Voorwaardelijke toegang](./media/app-based-conditional-access/02.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **apparaatplatformen** en **Client-apps**. 
 
    a. Als **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/03.png)

    b. Als **Client-apps (preview)**, selecteer **mobiele apps en bureaublad-apps** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/91.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

    - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

    - **Goedgekeurde client-app (preview) vereisen**

    - **De geselecteerde besturingselementen vereisen**   
 
    ![Voorwaardelijke toegang](./media/app-based-conditional-access/13.png)



**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met Active Sync (EAS) configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/app-based-conditional-access/61.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: Elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** Als cloud-apps, moet u selecteren **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/07.png)

4. **Voorwaarden:** Als **voorwaarden**, moet u configureren **Client-apps (preview)**. 

    Als **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-based-conditional-access/92.png)

5. Als **besturingselementen voor toegang**, moet u het volgende zijn geselecteerd:

    - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

    - **Goedgekeurde client-app (preview) vereisen**

    - **De geselecteerde besturingselementen vereisen**   
 
    ![Voorwaardelijke toegang](./media/app-based-conditional-access/64.png)




**Stap 3: de Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-based-conditional-access/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.






## <a name="next-steps"></a>Volgende stappen

Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).

Zie [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md) als u klaar bent om beleid voor voorwaardelijke toegang in Azure Active Directory te configureren. 
