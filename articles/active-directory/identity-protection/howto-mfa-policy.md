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
ms.openlocfilehash: 792a1fc2403e672c973577efd7a05c9c81d45ad4
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47054078"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Procedure: Het registratiebeleid voor meervoudige verificatie configureren

Azure AD Identity Protection kunt u het uitrollen van de multi-factor authentication (MFA)-inschrijving beheren door een beleid te configureren. Dit artikel wordt uitgelegd wat het beleid kan worden gebruikt voor een configureert.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>Wat is het registratiebeleid voor meervoudige verificatie?

Verificatie met meerdere factoren van Azure is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Het biedt een tweede beveiligingslaag gebruikersaanmeldingen en transacties.  

Het is raadzaam dat u Azure multi-factor authentication voor gebruikersaanmeldingen, vereisen omdat deze:

- Biedt een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties

- Speelt een belangrijke rol bij het voorbereiden van uw organisatie beveiligen en herstellen van account compromissen


Zie voor meer informatie, [wat is Azure multi-factor Authentication?](../authentication/multi-factor-authentication.md)


## <a name="how-do-i-access-the-mfa-registration-policy"></a>Hoe krijg ik toegang tot de MFA-registratiebeleid?
   
De MFA-registratiebeleid is in de **configureren** sectie op de [Azure AD Identity Protection-pagina](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![MFA-beleid](./media/howto-mfa-policy/1014.png)




## <a name="policy-settings"></a>Beleidsinstellingen

Wanneer u het beleid voor aanmeldingsrisico configureert, moet u instellen:

- De gebruikers en groepen die het beleid van toepassing op:

    ![Gebruikers en groepen](./media/howto-mfa-policy/11.png)

- Het type toegang dat u wilt worden afgedwongen:  

    ![Gebruikers en groepen](./media/howto-mfa-policy/12.png)

- De status van uw beleid:

    ![Afdwingen van beleid](./media/howto-mfa-policy/14.png)


Het dialoogvenster van de configuratie van beleid biedt u een optie voor het schatten van de impact van uw configuratie.

![Verwachte impact](./media/howto-mfa-policy/15.png)




## <a name="user-experience"></a>Gebruikerservaring


Zie voor een overzicht van de gebruikerservaring:

* [Multi-factor authentication-registratie stroom](flows.md#multi-factor-authentication-registration).  
* [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md).  



## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).
