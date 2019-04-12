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
ms.openlocfilehash: 2250449c0ef342332945b80cb10cb9a02885b259
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496927"
---
# <a name="require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>App-beveiligingsbeleid vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang (preview)

Uw werknemers gebruiken mobiele apparaten voor zowel privé- als werktaken. Terwijl te zorgen dat uw werknemers productief zijn kan, wilt u ook om te voorkomen dat gegevens verloren gaan. Met voorwaardelijke toegang van Azure Active Directory (Azure AD), kunt u uw bedrijfsgegevens beschermen door het beperken van toegang tot uw cloud-apps. Gebruik eerst client-apps met app-beveiligingsbeleid.

In dit artikel wordt uitgelegd hoe u beleid voor voorwaardelijke toegang waarvoor een beveiligingsbeleid voor apps om toegang te tot gegevens krijgen kunnen configureren.

## <a name="overview"></a>Overzicht

Met [voorwaardelijke toegang voor Azure AD](overview.md), kunt u aanpassen hoe gemachtigde gebruikers toegang uw resources tot hebben. U kunt bijvoorbeeld de toegang beperken tot uw cloud-apps met vertrouwde apparaten.

U kunt [Intune beveiligingsbeleid voor apps](https://docs.microsoft.com/intune/app-protection-policy) om gegevens van uw bedrijf te beveiligen. Intune beveiligingsbeleid voor apps vereisen niet een beheeroplossing voor mobiele apparaten (MDM). U kunt uw bedrijfsgegevens met of zonder inschrijving van apparaten in een oplossing voor Apparaatbeheer kunt beveiligen.

Voorwaardelijke toegang van Azure Active Directory beperkt de toegang tot uw cloud-apps voor clienttoepassingen die Intune naar Azure AD als app-beveiligingsbeleid ontvangen heeft gerapporteerd. U kunt bijvoorbeeld toegang tot Exchange Online beperken voor de Outlook-app met Intune app-beveiligingsbeleid.

In de terminologie voor voorwaardelijke toegang deze client-apps bekend is dat het beleid dat is beveiligd met een *app-beveiligingsbeleid*.  

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/05.png)

Zie voor een lijst van beleid voor beveiligde client-apps, [de vereiste van het beleid voor App-beveiliging](technical-reference.md#approved-client-app-requirement).

U kunt beleid voor voorwaardelijke toegang op basis van app-beveiliging met een ander beleid, zoals combineren [beleid voor voorwaardelijke toegang op basis van apparaat](require-managed-devices.md). Op deze manier kunt u flexibiliteit bij het beveiligen van gegevens voor zowel persoonlijke als zakelijke apparaten opgeven.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Voordelen van vereiste voor app-beveiliging op basis van voorwaardelijke toegang

Dit is vergelijkbaar met naleving die door Intune wordt gerapporteerd voor iOS en Android voor een beheerd apparaat, Intune nu rapporten aan Azure AD als app-beveiligingsbeleid wordt toegepast. Voorwaardelijke toegang kunt u dit beleid gebruiken als een toegangscontrole. Dit nieuwe beleid voor voorwaardelijke toegang, de app-beveiligingsbeleid, verhoogt de beveiliging. Ermee worden beveiligd tegen fouten van de beheerder, zoals:

- Gebruikers die niet beschikken over een Intune-licentie.
- Gebruikers die geen Intune app-beveiligingsbeleid ontvangen.
- Intune app protection-beleid apps die niet zijn geconfigureerd voor het ontvangen van een beleid.


## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u bekend met bent:

- De [de vereiste van het beleid voor app-beveiliging](technical-reference.md#app-protection-policy-requirement) technische documentatie.
- De [goedgekeurde client-app vereiste](technical-reference.md#approved-client-app-requirement) technische documentatie.
- De basisconcepten van [voorwaardelijke toegang in Azure Active Directory](overview.md).
- Hoe u [beleid voor voorwaardelijke toegang configureren](app-based-mfa.md).


## <a name="prerequisites"></a>Vereisten

Voor het maken van een app-beleid voor voorwaardelijke toegang op basis van beveiliging, moet u:

- Een Enterprise Mobility + Security of een Azure Active Directory premium-abonnement + Intune hebben.
- Zorg ervoor dat de gebruikers hebben een licentie voor Enterprise Mobility + Security of Azure AD en Intune.
- Zorg ervoor dat de client-app is geconfigureerd in Intune voor het ontvangen van app-beveiligingsbeleid.
- Zorg ervoor dat de gebruikers zijn geconfigureerd in Intune voor het ontvangen van een app-beveiligingsbeleid van Intune.

## <a name="app-protection-based-policy-for-exchange-online"></a>App-beveiliging op basis van beleid voor Exchange Online

In dit scenario bestaat uit een app-beveiliging op basis van voorwaardelijk toegangsbeleid voor toegang tot Exchange Online.

### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:

- Hiermee configureert u e-mailbericht met behulp van een systeemeigen e-mailtoepassing op iOS of Android verbinding maken met Exchange.
- Ontvangt een e-mailbericht dat aangeeft dat toegang alleen met behulp van de Outlook-app beschikbaar is.
- De toepassing met de koppeling downloads.
- Hiermee opent u de Outlook-toepassing en zich aanmeldt met Azure AD-referenties.
- Gevraagd voor het installeren van een Microsoft-Authenticator voor iOS gebruiken of Intune-bedrijfsportal voor Android gebruiken om door te gaan.
- De toepassing wordt geïnstalleerd en wordt de Outlook-app om door te gaan.
- Wordt gevraagd om een apparaat te registreren.
- Intune app-beveiligingsbeleid kan worden ontvangen.
- Toegang krijgen tot e-mail.

Een app-beveiligingsbeleid van Intune moeten zich in de aanvraag voor toegang tot bedrijfsgegevens. Het beleid kunnen de gebruiker gevraagd om de toepassing opnieuw hebt gestart of gebruik een andere PINCODE. Dit is het geval als de beleidsregels zijn geconfigureerd voor de toepassing en platform.

### <a name="configuration"></a>Configuratie

**Stap 1: Een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap configureert u de volgende onderdelen:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/01.png)

1. Voer de naam van uw beleid voor voorwaardelijke toegang.

2. Onder **toewijzingen**in **gebruikers en groepen**, selecteert u ten minste één gebruiker of groep voor elk beleid voor voorwaardelijke toegang.

3. In **Cloud-apps**, selecteer **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. In **voorwaarden**, Configureer **apparaatplatformen** en **Client-apps (preview)**:

    a. In **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. In **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Onder **besturingselementen voor toegang**, selecteer **vereisen app-beveiligingsbeleid (preview)**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/05.png)
 

**Stap 2: Configureren van een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met ActiveSync (EAS)**

Voor het beleid voor voorwaardelijke toegang in deze stap configureert u de volgende onderdelen:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/06.png)

1. Voer de naam van uw beleid voor voorwaardelijke toegang.

2. Onder **toewijzingen**in **gebruikers en groepen**, selecteert u ten minste één gebruiker of groep voor elk beleid voor voorwaardelijke toegang.


3. In **Cloud-apps**, selecteer **Office 365 Exchange Online**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. In **voorwaarden**, Configureer **Client-apps (preview)**. 

    a. In **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/92.png)

    b. Onder **besturingselementen voor toegang**, selecteer **vereisen app-beveiligingsbeleid (preview)**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/05.png)


**Stap 3: Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie voor meer informatie, [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).



## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>App-beveiliging op basis van of compatibel apparaatbeleid voor Exchange Online

In dit scenario bestaat uit een app-beleid op basis van beveiliging of compatibel apparaat voor voorwaardelijke toegang voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat:
 
- Een gebruiker is al geregistreerd met of zonder zakelijke apparaten.
- Gebruikers die niet zijn ingeschreven en geregistreerd bij Azure AD met behulp van een app beveiligde toepassing nodig om een apparaat toegang krijgen tot bronnen te registreren.
- Geregistreerde gebruikers die gebruikmaken van de app beschermd toepassing hoeft te opnieuw worden geregistreerd.
- De gebruiker een Intune-beveiligingsbeleid kunt ontvangen als ingeschreven of niet.
- De gebruiker kan toegang tot e-mail via Outlook en Intune app-beveiligingsbeleid als ingeschreven of niet.
- De gebruiker kan toegang krijgen tot e-mailbericht met Outlook als het apparaat is ingeschreven.


### <a name="configuration"></a>Configuratie

**Stap 1: Een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap configureert u de volgende onderdelen:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/62.png)

1. Voer de naam van uw beleid voor voorwaardelijke toegang.

2. Onder **toewijzingen**in **gebruikers en groepen**, selecteert u ten minste één gebruiker of groep voor elk beleid voor voorwaardelijke toegang.

3. In **Cloud-apps**, selecteer **Office 365 Exchange Online**. 

     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. In **voorwaarden**, Configureer **apparaatplatformen** en **Client-apps (preview)**. 
 
    a. In **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. In **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Onder **besturingselementen voor toegang**, selecteert u de volgende opties:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Een van de geselecteerde besturingselementen vereisen**   
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/11.png)



**Stap 2: Een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met ActiveSync configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap configureert u de volgende onderdelen:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/06.png)

1. Voer de naam van uw beleid voor voorwaardelijke toegang.

2. Onder **toewijzingen**in **gebruikers en groepen**, selecteert u ten minste één gebruiker of groep voor elk beleid voor voorwaardelijke toegang.

3. In **Cloud-apps**, selecteer **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. In **voorwaarden**, Configureer **Client-apps (preview)**. 

    In **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/92.png)

5. Onder **besturingselementen voor toegang**, selecteert u de volgende opties:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Een van de geselecteerde besturingselementen vereisen**

     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/11.png)



**Stap 3: Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie voor meer informatie, [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).





## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>App-beveiliging en compatibel apparaatbeleid voor Exchange Online

In dit scenario bestaat uit een voorwaardelijk toegangsbeleid op basis van app-beveiliging en compatibel apparaat voor toegang tot Exchange Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:
 
-   Hiermee configureert u e-mailbericht met behulp van een systeemeigen e-mailtoepassing op iOS of Android verbinding maken met Exchange.
-   Ontvangt een e-mailbericht dat aangeeft dat hun apparaat moeten worden geregistreerd voor toegang is vereist.
-   Intune-bedrijfsportal-App downloadt en zich aanmeldt bij de portal.
-   E-mail wordt gecontroleerd en wordt gevraagd de Outlook-app te gebruiken.
-   De Outlook-app downloadt.
-   Hiermee opent u de Outlook-app en voert de referenties die worden gebruikt in de registratie.
-   Intune app-beveiligingsbeleid kan worden ontvangen.
-   Toegang tot e-mail met Outlook en Intune app-beveiligingsbeleid.

Alle Intune beveiligingsbeleid voor apps worden geactiveerd om toegang te tot zakelijke gegevens krijgen. Het beleid kunnen de gebruiker gevraagd om de toepassing opnieuw hebt gestart of gebruik een andere PINCODE. Dit is het geval als de beleidsregels zijn geconfigureerd voor de toepassing en platform.


### <a name="configuration"></a>Configuratie

**Stap 1: Een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap configureert u de volgende onderdelen:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/01.png)

1. Voer de naam van uw beleid voor voorwaardelijke toegang.

2. Onder **toewijzingen**in **gebruikers en groepen**, selecteert u ten minste één gebruiker of groep voor elk beleid voor voorwaardelijke toegang.

3. In **Cloud-apps**, selecteer **Office 365 Exchange Online**. 

     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. In **voorwaarden**, Configureer **apparaatplatformen** en **Client-apps (preview)**. 
 
    a. In **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. In **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Onder **besturingselementen voor toegang**, selecteert u de volgende opties:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Alle geselecteerde besturingselementen vereisen**   
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/13.png)



**Stap 2: Een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online met ActiveSync configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap configureert u de volgende onderdelen:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/06.png)

1. Voer de naam van uw beleid voor voorwaardelijke toegang.

2. Onder **toewijzingen**in **gebruikers en groepen**, selecteert u ten minste één gebruiker of groep voor elk beleid voor voorwaardelijke toegang.

3. In **Cloud-apps**, selecteer **Office 365 Exchange Online**. 

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/07.png)

4. In **voorwaarden**, Configureer **Client-apps (preview)**. 

    In **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **Exchange ActiveSync-clients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/92.png)

5. Onder **besturingselementen voor toegang**, selecteert u de volgende opties:

   - **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Alle geselecteerde besturingselementen vereisen**   
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/13.png)




**Stap 3: Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie voor meer informatie, [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).


## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>App-beveiliging op basis van of op Apps gebaseerd beleid voor Exchange Online en SharePoint Online

In dit scenario bestaat uit een app op basis van beveiliging of goedgekeurde apps-beleid voor toegang tot Exchange Online en SharePoint Online.


### <a name="scenario-playbook"></a>Scenario-playbook

In dit scenario wordt ervan uitgegaan dat een gebruiker:

- Clienttoepassingen die zijn geconfigureerd in de lijst met apps die ondersteuning bieden voor de vereiste app protection-beleid of de vereiste voor goedgekeurde apps.  
- Maakt gebruik van clienttoepassingen die voldoen aan de vereiste app protection-beleid en Intune app-beveiligingsbeleid kunnen ontvangen.
- Maakt gebruik van clienttoepassingen die voldoen aan de vereisten voor het beleid van goedgekeurde apps die ondersteuning biedt voor app-beveiligingsbeleid.
- Hiermee opent de toepassing voor toegang tot e-mail of documenten.
- Hiermee opent u de Outlook-toepassing en zich aanmeldt met Azure AD-referenties.
- Gevraagd voor het installeren van een Microsoft-Authenticator voor iOS gebruiken of Intune-bedrijfsportal voor Android gebruiken als ze zijn nog niet is geïnstalleerd.
- Installeert de toepassing en kan terugkeren naar de Outlook-app om door te gaan.
- Wordt gevraagd om een apparaat te registreren.
- Intune app-beveiligingsbeleid kan worden ontvangen.
- Toegang tot e-mail met Outlook en Intune app-beveiligingsbeleid.
- Toegang tot sites en -documenten met een app niet op de vereiste van het beleid app-beveiliging, maar die worden vermeld in de vereiste voor goedgekeurde app.

Alle Intune beveiligingsbeleid voor apps zijn vereist om toegang te tot zakelijke gegevens krijgen. Het beleid kunnen de gebruiker gevraagd om de toepassing opnieuw hebt gestart of gebruik een andere PINCODE. Dit is het geval als de beleidsregels zijn geconfigureerd voor de toepassing en platform.

**Opmerkingen**

- U kunt dit scenario kunt gebruiken als u wilt ondersteunen beide beleidsregels voor de beveiliging op basis van en apps gebaseerde voorwaardelijke toegang.
- In deze *of* beleid, apps met een vereiste van het beleid app-beveiliging worden geëvalueerd om toegang te krijgen voordat de vereiste voor goedgekeurde client-apps.

### <a name="configuration"></a>Configuratie

**Stap 1: Een Azure AD-beleid voor voorwaardelijke toegang voor Exchange Online configureren**

Voor het beleid voor voorwaardelijke toegang in deze stap configureert u de volgende onderdelen:

![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/62.png)

1. Voer de naam van uw beleid voor voorwaardelijke toegang.

2. Onder **toewijzingen**in **gebruikers en groepen**, selecteert u ten minste één gebruiker of groep voor elk beleid voor voorwaardelijke toegang.

3. In **Cloud-apps**, selecteer **Office 365 Exchange Online**. 

     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/02.png)

4. In **voorwaarden**, Configureer **apparaatplatformen** en **Client-apps (preview)**. 
 
    a. In **apparaatplatformen**, selecteer **Android** en **iOS**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/03.png)

    b. In **Client-apps (preview)**, selecteer **mobiele apps en bureaubladclients** en **moderne verificatieclients**.

    ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/91.png)

5. Onder **besturingselementen voor toegang**, selecteert u de volgende opties:

   - **Goedgekeurde client-apps vereisen**

   - **Beleid voor app-beveiliging vereisen (preview)**

   - **Een van de geselecteerde besturingselementen vereisen**
 
     ![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/12.png)


**Stap 2: Intune app-beveiligingsbeleid voor iOS en Android-client-toepassingen configureren**


![Voorwaardelijke toegang](./media/app-protection-based-conditional-access/09.png)

Zie voor meer informatie, [apps en gegevens beschermen met Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).




## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).
- Als u klaar om te configureren van beleid voor voorwaardelijke toegang voor uw omgeving bent, Zie [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md). 