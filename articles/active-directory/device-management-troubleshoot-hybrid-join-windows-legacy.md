---
title: Het oplossen van hybride Azure Active Directory die lid zijn van downlevel-apparaten | Microsoft Docs
description: Het oplossen van hybride Azure Active Directory die lid zijn van downlevel-apparaten.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: ecf77a614922ef58cdfb2b2c8174f66e01ea9b46
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Het oplossen van hybride Azure Active Directory die lid zijn van downlevel-apparaten 

In dit onderwerp is alleen van toepassing op de volgende apparaten: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Zie voor Windows 10 of Windows Server 2016 [probleemoplossing hybride Azure Active Directory die lid zijn van apparaten met Windows 10 en Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

Dit onderwerp wordt ervan uitgegaan dat u hebt [geconfigureerde hybride Azure Active Directory die lid zijn van de apparaten](device-management-hybrid-azuread-joined-devices-setup.md) ter ondersteuning van de volgende scenario's:

- Voorwaardelijke toegang op basis van apparaten

- [Enterprise-instellingen voor roaming](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello voor Bedrijven](active-directory-azureadjoin-passport-deployment.md) 





Dit onderwerp vindt u richtlijnen over het oplossen van problemen op te lossen.  

**Wat u moet weten:** 

- Het maximum aantal apparaten per gebruiker is apparaatgerichte. Bijvoorbeeld, als *jdoe* en *jharnett* aanmelden bij een apparaat, de registratie van een afzonderlijke (DeviceID) wordt gemaakt voor elk van deze in de **gebruiker** tabblad info.  

- De initiële registratie / join van apparaten is geconfigureerd voor het uitvoeren van een poging gedaan bij het aanmelden of vergrendelen / ontgrendelen. Er zijn 5 minuten vertraging geactiveerd door een scheduler-taak. 

- Opnieuw installeren van het besturingssysteem of een handmatige unregister en opnieuw te registreren kunnen een nieuwe registratie maakt op Azure AD en resultaten in meerdere vermeldingen op het tabblad gebruikers gegevens in de Azure portal. 

## <a name="step-1-retrieve-the-registration-status"></a>Stap 1: De registratiestatus ophalen 

**De registratiestatus controleren:**  

1. Open de opdrachtprompt als beheerder 

2. type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Met deze opdracht geeft een dialoogvenster waarmee u meer informatie over de status van de join.

![Werkplekdeelname voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Stap 2: De hybride Azure AD join status evalueren 

Als de hybride Azure AD join niet geslaagd is, biedt in het dialoogvenster u informatie over het probleem dat is opgetreden.

**De meest voorkomende problemen zijn:**

- Een onjuist geconfigureerde AD FS of Azure AD

    ![Werkplekdeelname voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- U bent niet aangemeld op als een domeingebruiker

    ![Werkplekdeelname voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)
    
    Er zijn enkele verschillende redenen waarom deze fout kan optreden:
    
    1. De gebruiker aangemeld is niet als een domeingebruiker (bijvoorbeeld een lokale gebruiker). Hybride Azure AD join op downlevel-apparaten wordt alleen ondersteund voor gebruikers van een domein.
    
    2. Als Autoworkplace.exe om een of andere reden niet kan achtergrond verifiëren met Azure AD of AD FS. Een aantal mogelijke oorzaken kunt uitgaande gebonden problemen met de netwerkverbinding worden naar Azure AD-URL's (Raadpleeg de vereisten) of als MFA ingeschakeld/geconfigureerd voor de gebruiker is, maar WIAORMUTLIAUTHN niet is geconfigureerd op de federatieserver (configuratiestappen selectievakje). Een andere mogelijkheid is dat die thuis realm detectie (HRD) pagina wacht gebruikersinteractie zo wordt voorkomen dat Autoworkplace.exe achtergrond verkrijgen van een token.
    
    3. Als de organisatie van Azure AD gebruikmaakt zijn naadloze eenmalige aanmelding, de volgende URL's niet aanwezig op het intranet in Internet Explorer van het apparaat:
    
       - https://autologon.microsoftazuread-sso.com
       - https://aadg.windows.net.nsatc.net
    
       en de instelling 'Toestaan dat updates voor de statusbalk via scripts' moet zijn ingeschakeld voor de intranetzone.

- Een quotum is bereikt

    ![Werkplekdeelname voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- De service reageert niet 

    ![Werkplekdeelname voor Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

U kunt ook de statusinformatie vinden in het gebeurtenislogboek onder **toepassingen en Services Log\Microsoft-Workplace Join**.
  
**De meest voorkomende oorzaken van een mislukte hybride Azure AD join zijn:** 

- Uw computer zich niet in het interne netwerk van de organisatie of een VPN zonder verbinding met een on-premises AD-domeincontroller.

- U bent aangemeld met een lokaal computeraccount op de computer. 

- Problemen met de configuratie van service: 

  - De federation-server is geconfigureerd voor ondersteuning van **WIAORMULTIAUTHN**. 

  - Er is geen Service Connection Point-object dat verwijst naar de domeinnaam van uw geverifieerde in Azure AD in het AD-forest waar de computer deel uitmaakt.

  - Een gebruiker heeft de limiet van apparaten bereikt. 

## <a name="next-steps"></a>Volgende stappen

Zie voor vragen over de [Apparaatbeheer Veelgestelde vragen](device-management-faq.md)  
