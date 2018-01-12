---
title: Azure Active Directory op basis van een app voorwaardelijke toegang | Microsoft Docs
description: Meer informatie over de werking van voorwaardelijke toegang voor Azure Active Directory op basis van een app.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/11/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 7814b53a69506f81bc6a75387c62766e36d528d8
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="azure-active-directory-app-based-conditional-access"></a>Azure Active Directory op basis van een app voorwaardelijke toegang  

Uw werknemers gebruiken mobiele apparaten voor zowel privé- als werktaken. Zorg dat uw werknemers productief kan zijn, wilt u ook om gegevensverlies te voorkomen. Met Azure Active Directory (Azure AD) op basis van een app voorwaardelijke toegang, kunt u toegang beperken tot uw cloud-apps voor ClientApps die uw bedrijfsgegevens kunnen beveiligen.  

In dit onderwerp wordt uitgelegd hoe voorwaardelijke toegang van Azure AD op basis van een app configureren.

## <a name="overview"></a>Overzicht

Met [voorwaardelijke toegang van Azure AD](active-directory-conditional-access-azure-portal.md), kunt u aanpassen hoe geautoriseerde gebruikers toegang uw resources tot hebben. U kunt bijvoorbeeld de toegang beperken tot uw cloud-apps op vertrouwde apparaten.

U kunt [beveiligingsbeleid voor Intune app](https://docs.microsoft.com/intune/app-protection-policy) om gegevens van uw bedrijf te beveiligen. Intune app beveiligingsbeleid nodig geen oplossing voor beheer van mobiele apparaten (MDM), waarmee u gegevens te beveiligen van uw bedrijf met of zonder inschrijving van apparaten in een oplossing voor Apparaatbeheer.

Azure Active Directory voorwaardelijke toegang op basis van een app kunt u de toegang tot uw cloud-apps op ClientApps die ondersteuning bieden voor beveiligingsbeleid voor Intune app beperken. U kunt bijvoorbeeld toegang tot Exchange Online beperken tot de Outlook-app.

In de terminologie voorwaardelijke toegang tot deze ClientApps worden aangeduid als **client-apps goedgekeurd**.  


![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/05.png)


Zie voor een lijst met goedgekeurde client-apps, [client app vereiste goedgekeurd](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


U kunt beleid voor voorwaardelijke toegang op basis van een app met ander beleid zoals combineren [beleidsregels voor voorwaardelijke toegang op basis van apparaten](active-directory-conditional-access-policy-connected-applications.md) biedt flexibiliteit bij het beveiligen van gegevens voor persoonlijke en zakelijke apparaten.

 


##<a name="before-you-begin"></a>Voordat u begint

In dit onderwerp wordt ervan uitgegaan dat u bekend met bent:

- De [client app vereiste goedgekeurd](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) technische documentatie.


- De basisconcepten van [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- Hoe [beleid voor voorwaardelijke toegang configureren](active-directory-conditional-access-azure-portal-get-started.md).

- De [migratie van beleidsregels voor voorwaardelijke toegang](active-directory-conditional-access-best-practices.md#policy-migration).
 

## <a name="prerequisites"></a>Vereisten

Voor het maken van een beleid voor voorwaardelijke toegang op basis van een app, moet u een Enterprise Mobility + Security of een Azure Active Directory premium-abonnement hebben en de gebruikers moeten een licentie hebben voor EMS of Azure AD. 


## <a name="exchange-online-policy"></a>Exchange Online-beleid 

Dit scenario bestaat uit een beleid voor voorwaardelijke toegang op basis van een app voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario playbook

Dit scenario wordt ervan uitgegaan dat een gebruiker:

- Hiermee regelt u e-mail met een systeemeigen e-mailtoepassing op iOS of Android verbonden met Exchange

- Ontvangt een e-mailbericht geeft aan dat de toegang is alleen beschikbaar via de Outlook-app

- De toepassing met de koppeling downloads

- De Outlook-toepassing wordt geopend en u zich aanmeldt met de Azure AD-referenties

- Wordt gevraagd te installeren verificator (iOS) of bedrijfsportal (Android) om door te gaan

- Installeert de toepassing en kan terugkeren naar de Outlook-app om door te gaan

- Wordt gevraagd een apparaat registreren

- Kan toegang tot e-mail

Alle beleidsregels voor Intune app beveiliging worden geactiveerd op het moment dat de toegang tot zakelijke gegevens en kan de gebruiker gevraagd om de toepassing opnieuw starten, gebruikt u een extra PINCODE enzovoort (indien geconfigureerd voor de toepassing en het platform).

### <a name="configuration"></a>Configuratie 

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/01.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/07.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **apparaatplatforms** en **Client-apps**:

    a. Als **apparaatplatforms**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/03.png)

    b. Als **Client-apps**, selecteer **mobiele apps en desktoptoepassingen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/04.png)

5. Als **toegangscontroles**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/05.png)
 

**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang configureert voor Exchange Online met Active Sync (EAS)**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/06.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.


3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/07.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **Client-apps**. 

    a. Als **Client-apps**, selecteer **Exchange Active Sync**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/08.png)

    b. Als **toegangscontroles**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/05.png)


**Stap 3: de Intune app protection-beleid voor iOS en Android-clienttoepassingen configureren**


![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.


## <a name="exchange-online-and-sharepoint-online-policy"></a>Exchange Online en SharePoint Online-beleid

Dit scenario bestaat uit een voorwaardelijke toegang met mam-beleid voor toegang tot Exchange Online en SharePoint Online met goedgekeurde apps.

### <a name="scenario-playbook"></a>Scenario playbook

Dit scenario wordt ervan uitgegaan dat een gebruiker:

- Probeert de SharePoint-app gebruiken om verbinding te en hun zakelijke sites weergeven

- Poging om aan te melden met dezelfde referenties als de Outlook-app-referenties

- Is niet nodig om opnieuw te registreren en toegang kunnen krijgen tot de resources


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online en SharePoint Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/71.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.


3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online** en **Office 365 SharePoint Online**. 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/02.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **apparaatplatforms** en **Client-apps**:

    a. Als **apparaatplatforms**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/03.png)

    b. Als **Client-apps**, selecteer **mobiele apps en desktoptoepassingen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/04.png)

5. Als **toegangscontroles**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/05.png)




**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang configureert voor Exchange Online met Active Sync (EAS)**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/06.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online**. Online 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/07.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **Client-apps**:

    a. Als **Client-apps**, selecteer **Exchange Active Sync**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/08.png)

    b. Als **toegangscontroles**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/05.png)




**Stap 3: de Intune app protection-beleid voor iOS en Android-clienttoepassingen configureren**


![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Apparaatbeleid voor Exchange Online en SharePoint Online op basis van een App of een compatibel

Dit scenario bestaat uit een app of compatibel apparaat voorwaardelijke toegangsbeleid voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario playbook

Dit scenario wordt ervan uitgegaan dat:
 
- Sommige gebruikers zijn al geregistreerd (met of zonder zakelijke apparaten)

- Gebruikers die niet zijn ingeschreven en die zijn geregistreerd bij Azure AD dat gebruikmaakt van een app beveiligde toepassing moet u een apparaat toegang tot bronnen te registreren

- Geregistreerde gebruikers met de app beveiligde toepassing hebben geen opnieuw registreren van het apparaat


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online en SharePoint Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/62.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online** en **Office 365 SharePoint Online**. 

     ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/02.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **apparaatplatforms** en **Client-apps**. 
 
    a. Als **apparaatplatforms**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/03.png)

    b. Als **Client-apps**, selecteer **mobiele apps en desktoptoepassingen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/04.png)

5. Als **toegangscontroles**, moet u het volgende zijn geselecteerd:

    - **Vereisen dat apparaat worden gemarkeerd als compatibel**

    - **Goedgekeurde client-app (preview) is vereist**

    - **Een van de geselecteerde besturingselementen vereisen**   
 
    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/11.png)



**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang configureert voor Exchange Online met Active Sync (EAS)**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/61.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/07.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **Client-apps**. 

    Als **Client-apps*, selecteer **Exchange Active Sync**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/08.png)

5. Als **toegangscontroles**, moet u beschikken over **goedgekeurde client-app (preview) vereisen** geselecteerde.
 
    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/11.png)




**Stap 3: de Intune app protection-beleid voor iOS en Android-clienttoepassingen configureren**


![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Beleid voor Exchange Online en SharePoint Online-App en compatibele apparaten

Dit scenario bestaat uit een app en compatibele apparaten voorwaardelijke toegangsbeleid voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario playbook

Dit scenario wordt ervan uitgegaan dat een gebruiker:
 
-   Hiermee regelt u e-mail met een systeemeigen e-mailtoepassing op iOS of Android verbonden met Exchange
-   Ontvangt een e-mailbericht geeft aan dat toegang is vereist voor uw apparaat moet zijn geregistreerd
-   Downloaden van de bedrijfsportal en zich aanmeldt bij de bedrijfsportal
-   E-mail wordt gecontroleerd en wordt gevraagd de Outlook-app gebruiken
-   De Outlook-app downloaden
-   Hiermee opent u de Outlook-app en voert de referenties in van de inschrijving
-   Gebruiker kan toegang tot e-mail

Een app protection-beleid van Intune worden geactiveerd op het moment van toegang tot zakelijke gegevens en kunnen de gebruiker gevraagd om de toepassing opnieuw starten, gebruikt u een extra PINCODE enzovoort (indien geconfigureerd voor de toepassing en het platform)


### <a name="configuration"></a>Configuratie

**Stap 1: een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online en SharePoint Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/62.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online** en **Office 365 SharePoint Online**. 

     ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/02.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **apparaatplatforms** en **Client-apps**. 
 
    a. Als **apparaatplatforms**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/03.png)

    b. Als **Client-apps**, selecteer **mobiele apps en desktoptoepassingen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/04.png)

5. Als **toegangscontroles**, moet u het volgende zijn geselecteerd:

    - **Vereisen dat apparaat worden gemarkeerd als compatibel**

    - **Goedgekeurde client-app (preview) is vereist**

    - **De geselecteerde besturingselementen vereisen**   
 
    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/13.png)



**Stap 2: een Azure AD-beleid voor voorwaardelijke toegang configureert voor Exchange Online met Active Sync (EAS)**

Voor het beleid voor voorwaardelijke toegang in deze stap moet u de volgende onderdelen configureren:

![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/61.png)

1. De **naam** van uw beleid voor voorwaardelijke toegang.

2. **Gebruikers en groepen**: elk beleid voor voorwaardelijke toegang moet hebben ten minste één gebruiker of groep geselecteerd.

3. **Cloud-apps:** als cloud-apps die u wilt selecteren **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/07.png)

4. **Voorwaarden:** als **voorwaarden**, moet u configureren **Client-apps**. 

    Als **Client-apps**, selecteer **Exchange Active Sync**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/08.png)

5. Als **toegangscontroles**, moet u het volgende zijn geselecteerd:

    - **Vereisen dat apparaat worden gemarkeerd als compatibel**

    - **Goedgekeurde client-app (preview) is vereist**

    - **De geselecteerde besturingselementen vereisen**   
 
    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/64.png)




**Stap 3: de Intune app protection-beleid voor iOS en Android-clienttoepassingen configureren**


![Voorwaardelijke toegang](./media/active-directory-conditional-access-mam/09.png)

Zie [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) voor meer informatie.






## <a name="next-steps"></a>Volgende stappen

Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
