---
title: Het oplossen van hybride Azure Active Directory gekoppelde downlevel-apparaten | Microsoft Docs
description: Het oplossen van hybride Azure Active Directory gekoppelde downlevel-apparaten.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3b99a82b59cbba22d30a4325e246c308a2042ad5
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136304"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Het oplossen van hybride Azure Active Directory gekoppelde downlevel-apparaten 

In dit artikel is alleen van toepassing op de volgende apparaten: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Zie voor Windows 10 of Windows Server 2016, [probleemoplossing hybride Azure Active Directory gekoppelde Windows 10 en Windows Server 2016-apparaten](device-management-troubleshoot-hybrid-join-windows-current.md).

In dit artikel wordt ervan uitgegaan dat u hebt [geconfigureerde hybride Azure Active Directory gekoppelde apparaten](device-management-hybrid-azuread-joined-devices-setup.md) ter ondersteuning van de volgende scenario's:

- Voorwaardelijke toegang op basis van apparaat

- [Zakelijke roaming van instellingen](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello voor Bedrijven](active-directory-azureadjoin-passport-deployment.md) 





In dit artikel biedt hulp bij probleemoplossing voor het oplossen van problemen.  

**Wat u moet weten:** 

- Het maximum aantal apparaten per gebruiker is apparaatgerichte. Bijvoorbeeld, als *jdoe* en *jharnett* aanmelden bij een apparaat, een afzonderlijke inschrijving (DeviceID) is gemaakt voor elk van deze in de **gebruiker** tabblad info.  

- De registratie / join van apparaten is geconfigureerd voor het uitvoeren van een poging bij het aanmelden of vergrendelen / ontgrendelen. Het is mogelijk dat er 5 minuten vertraging geactiveerd door een scheduler-taak. 

- Een nieuwe installatie van het besturingssysteem of de handmatige herregistratie kan een nieuwe registratie maakt in Azure AD, wat tot meerdere vermeldingen onder het tabblad van de gebruiker gegevens in Azure portal leidt. 

- Zorg ervoor dat [KB4284842](https://support.microsoft.com/en-us/help/4284842) is geïnstalleerd, in het geval van Windows 7 SP1 of Windows Server 2008 R2 SP1. Deze update voorkomt toekomstige authenticatiefouten vanwege verlies van de klant toegang tot beveiligde sleutels na het wijzigen van wachtwoord.

## <a name="step-1-retrieve-the-registration-status"></a>Stap 1: De registratiestatus ophalen 

**Om te controleren of de registratiestatus:**  

1. Aanmelden met het gebruikersaccount dat een hybride Azure AD join is uitgevoerd.

2. Open de opdrachtprompt als beheerder 

3. Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Met deze opdracht wordt het dialoogvenster waarmee u meer informatie over de join-status weergegeven.

![Workplace Join voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Stap 2: De hybride Azure AD join-status evalueren 

Als de hybride Azure AD join niet geslaagd is, biedt in het dialoogvenster u informatie over het probleem dat zich heeft voorgedaan.

**De meest voorkomende problemen zijn:**

- Een onjuist geconfigureerde AD FS of een Azure AD

    ![Workplace Join voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- U bent niet aangemeld op als een domeingebruiker

    ![Workplace Join voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)
    
    Er zijn enkele verschillende redenen waarom dit kan gebeuren:
    
    - De aangemelde gebruiker is niet een domeingebruiker (bijvoorbeeld een lokale gebruiker). Hybride Azure AD join op downlevel-apparaten wordt alleen ondersteund voor gebruikers van een domein.
    
    - Autoworkplace.exe is niet op de achtergrond verifiëren met Azure AD of AD FS. Dit kan zijn vanwege een uitgaande gebonden problemen met de netwerkverbinding met de Azure AD-URL's. Het kan ook zijn dat multi-factor authentication (MFA) ingeschakeld/geconfigureerd voor de gebruiker is en WIAORMUTLIAUTHN niet is geconfigureerd op de federation-server. Een andere mogelijkheid is dat thuisdomein detectie (HRD)-pagina wordt gewacht tot tussenkomst van de gebruiker, waardoor **autoworkplace.exe** uit op de achtergrond van het verkrijgen van een token.
    
    - Uw organisatie gebruikt Azure AD naadloze eenmalige aanmelding, `https://autologon.microsoftazuread-sso.com` of `https://aadg.windows.net.nsatc.net` bevinden zich niet op van het apparaat-instellingen voor het intranet van Internet Explorer, en **toestaan van updates op de statusbalk via script** is niet ingeschakeld voor de intranetzone.

- Een quotum is bereikt

    ![Workplace Join voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- De service reageert niet 

    ![Workplace Join voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

U kunt de statusinformatie ook vinden in het gebeurtenislogboek onder: **toepassingen en Services Log\Microsoft-Workplace Join**
  
**De meest voorkomende oorzaken voor een mislukte hybride Azure AD join zijn:** 

- Uw computer niet is verbonden met het interne netwerk van uw organisatie of met een VPN via een verbinding met uw on-premises AD-domeincontroller.

- U bent aangemeld met een lokaal computeraccount op de computer. 

- Problemen met de configuratie van service: 

  - De federation-server is geconfigureerd ter ondersteuning van **WIAORMULTIAUTHN**. 

  - De forest van uw computer heeft geen Service Connection Point-object dat naar de naam van uw geverifieerde domeinnaam in Azure AD verwijst 

  - Een gebruiker heeft het maximale aantal apparaten bereikt. 

## <a name="next-steps"></a>Volgende stappen

Voor vragen, Zie de [Apparaatbeheer Veelgestelde vragen](device-management-faq.md)  
