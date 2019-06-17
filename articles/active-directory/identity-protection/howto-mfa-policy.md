---
title: Het registratiebeleid voor meervoudige verificatie configureren in Azure Active Directory Identity Protection | Microsoft Docs
description: Informatie over het configureren van het registratiebeleid voor multi-factor authentication van Azure AD Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.date: 05/01/2019
author: MicrosoftGuyJFlo
manager: daveba
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 434d07163713a139b42a5dbe1664f81dafc2a1ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108944"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Procedure: De Azure multi-factor Authentication-registratie-beleid configureren

Azure AD Identity Protection helpt die u bij het beheren van de uitrol van multi-factor authentication (MFA)-inschrijving door het configureren van beleid voor voorwaardelijke toegang om te vereisen dat MFA-registratie, ongeacht welke app u zich aanmeldt. In dit artikel wordt uitgelegd wat het beleid kan worden gebruikt en hoe dit moet worden geconfigureerd.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>Wat is de Azure multi-factor Authentication-registratiebeleid?

Azure multi-factor Authentication biedt een manier om te verifiëren wie u meer dan alleen een gebruikersnaam en wachtwoord. Het biedt een tweede beveiligingslaag aan gebruikersaanmeldingen. In de volgorde voor gebruikers om te kunnen reageren op prompts van MFA, moeten ze eerst registreren voor Azure multi-factor Authentication.

Het is raadzaam dat u Azure multi-factor Authentication voor gebruikersaanmeldingen, vereisen omdat deze:

- Biedt een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties
- Speelt een belangrijke rol bij het voorbereiden van uw organisatie beveiligen en herstellen van de risicogebeurtenissen in Identity Protection

Zie voor meer informatie over MFA [wat is Azure multi-factor Authentication?](../authentication/howto-mfa-getstarted.md)

## <a name="how-do-i-access-the-registration-policy"></a>Hoe krijg ik toegang tot het registratiebeleid?

De MFA-registratiebeleid is in de **configureren** sectie op de [Azure AD Identity Protection-pagina](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).

![MFA-beleid](./media/howto-mfa-policy/1014.png)

## <a name="policy-settings"></a>Beleidsinstellingen

Wanneer u de MFA-registratiebeleid configureert, moet u de volgende configuratiewijzigingen aanbrengen:

- De gebruikers en groepen die het beleid van toepassing op. Houd er rekening mee om uit te sluiten van uw organisatie [accounts voor toegang in noodgevallen](../users-groups-roles/directory-emergency-access.md).

    ![Gebruikers en groepen](./media/howto-mfa-policy/11.png)

- Het besturingselement dat u afdwingen wilt - **registratie bij Azure MFA vereisen**

    ![Access](./media/howto-mfa-policy/12.png)

- Afdwingen van beleid moet worden ingesteld op **op**.

    ![Afdwingen van beleid](./media/howto-mfa-policy/14.png)

- **Sla** uw beleid

## <a name="user-experience"></a>Gebruikerservaring

Zie voor een overzicht van de gebruikerservaring:

- [Multi-factor authentication-registratie stroom](flows.md#multi-factor-authentication-registration).  
- [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md).  

## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).
