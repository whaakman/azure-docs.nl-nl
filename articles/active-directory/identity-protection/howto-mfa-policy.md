---
title: Het registratiebeleid voor meervoudige verificatie configureren in Azure Active Directory Identity Protection | Microsoft Docs
description: Informatie over het configureren van het registratiebeleid voor multi-factor authentication van Azure AD Identity Protection.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: e626260dba3155ef56ee4a784aab2c6fd6897295
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581361"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Procedure: Het registratiebeleid voor meervoudige verificatie configureren

Azure AD Identity Protection kunt u het uitrollen van de multi-factor authentication (MFA)-inschrijving beheren door een beleid te configureren. Dit artikel wordt uitgelegd wat het beleid kan worden gebruikt voor een configureert.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>Wat is het registratiebeleid voor meervoudige verificatie?

Verificatie met meerdere factoren van Azure is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Het biedt een tweede beveiligingslaag gebruikersaanmeldingen en transacties.  
Het is raadzaam dat u Azure multi-factor authentication voor gebruikersaanmeldingen, vereisen omdat deze:

* Biedt een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties
* Speelt een belangrijke rol bij het voorbereiden van uw organisatie beveiligen en herstellen van account compromissen

![Beleid voor gebruikersrisico's](./media/howto-mfa-policy/1019.png "beleid voor gebruikersrisico's")

Zie voor meer informatie, [wat is Azure multi-factor Authentication?](../authentication/multi-factor-authentication.md)

## <a name="configuration"></a>Configuratie

**Opent het dialoogvenster gerelateerde configuratie**:

- Op de **Azure AD Identity Protection** blade in de **configureren** sectie, klikt u op **multi-factor authentication-registratie**.

    ![MFA-beleid](./media/howto-mfa-policy/1019.png "MFA-beleid")

### <a name="settings"></a>Instellingen

* Stel de gebruikers en groepen die het beleid van toepassing op:

    ![MFA-beleid](./media/howto-mfa-policy/1020.png "MFA-beleid")
* Stel de besturingselementen moeten worden afgedwongen wanneer het beleid wordt geactiveerd:  

    ![MFA-beleid](./media/howto-mfa-policy/1021.png "MFA-beleid")
* De status van uw beleid switch:

    ![MFA-beleid](./media/howto-mfa-policy/403.png "MFA-beleid")
* De huidige registratiestatus weergeven:

    ![MFA-beleid](./media/howto-mfa-policy/1022.png "MFA-beleid")

Zie voor een overzicht van de gebruikerservaring:

* [Multi-factor authentication-registratie stroom](flows.md#multi-factor-authentication-registration).  
* [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md).  



## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).
