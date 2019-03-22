---
title: Hulp krijgen bij de portal mijn Apps - Azure Active Directory | Microsoft Docs
description: Hulp bij het aanmelden bij en veelvoorkomende taken uitvoeren in de portal mijn Apps.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: lizross
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6e87ae0a8adf28176d9a97cbf1b78943179884a
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340107"
---
# <a name="troubleshoot-problems-with-the-my-apps-portal"></a>Oplossen van problemen met de portal mijn Apps
Als u problemen ondervindt met aanmelden bij of met behulp van de **mijn Apps** portal, probeert deze tips voor probleemoplossing voordat u contact opnemen met helpdesk of beheerder voor hulp.

## <a name="im-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Ik ondervind problemen bij het installeren van de extensie mijn Apps beveiligen aanmelden
Als u problemen bij het installeren van de extensie mijn Apps beveiligen aanmelden:

- Zorg ervoor dat u een ondersteunde browser, met inbegrip van:

    - **Microsoft Edge.** Wordt uitgevoerd op Windows 10 Anniversary Edition of hoger.
    - **Google Chrome.** Wordt uitgevoerd op Windows 7 of hoger, en in Mac OS X of hoger.
    - **Mozilla Firefox 26,0 of hoger.** Wordt uitgevoerd op Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger.
    - **Internet Explorer 11.** Uitgevoerd op Windows 7 of hoger (beperkte ondersteuning).

- Zorg ervoor dat de instellingen van uw browser-extensie worden ingeschakeld.

- Probeer het opnieuw starten van uw browser en aan te melden bij de **mijn Apps** portal opnieuw.

- Wist u cookies van uw browser, en start opnieuw op en meld u aan de **mijn Apps** portal opnieuw.

## <a name="i-cant-sign-in-to-the-my-apps-portal"></a>Ik kan niet aanmelden bij de **mijn Apps** portal
Als u problemen bij het aanmelden ondervindt bij de **mijn Apps** portal, kunt u het volgende proberen:

- Zorg ervoor dat u de juiste URL. Deze moet https://myapps.microsoft.com of een aangepaste pagina voor uw organisatie, zoals https://myapps.microsoft.com/contoso.com.

- Zorg ervoor dat uw wachtwoord juist is en nog niet is verlopen. Zie voor meer informatie, [uw werk- of schoolaccount wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

- Zorg ervoor dat de verificatie-informatie huidige en nauwkeurige. Zie voor meer informatie, [wat Azure multi-factor Authentication betekent voor mij?](multi-factor-authentication-end-user.md) of [wijzigen van de gegevens van uw informatie beveiligingsmethoden en](security-info-add-update-methods-overview.md).

- Toevoegen de **mijn App** portal URL naar de **Internet-eigenschappen > Beveiliging > vertrouwde websites** instelling.

- Wis de cache van uw browser en probeer het opnieuw aanmelden.

## <a name="my-password-isnt-working"></a>Mijn wachtwoord werkt niet
Als u uw wachtwoord bent vergeten, nooit van uw organisatie ontvangen, toegang tot uw account worden geblokkeerd of uw wachtwoord wilt wijzigen, raadpleegt u [Help, ik ben mijn wachtwoord voor Azure AD vergeten](active-directory-passwords-update-your-own-password.md).

## <a name="i-want-to-be-able-to-reset-my-own-password"></a>Ik wil mijn eigen wachtwoord opnieuw instellen
Als u uw eigen wachtwoord opnieuw instellen, de beheerder moet eerst de functie inschakelen voor uw organisatie, en vervolgens moet u bijwerken en controleer of de vereiste verificatiemethoden. Zie voor meer informatie over het bijwerken van uw verificatiemethoden [registreren voor self-service voor wachtwoord opnieuw instellen](active-directory-passwords-reset-register.md).

## <a name="im-getting-an-access-denied-message-when-i-start-an-app"></a>Ik krijg bericht toegang geweigerd wanneer ik een app te starten
Als u een **toegang geweigerd** bericht na het starten van een app uit de **mijn App** portal, kunt u het volgende proberen:

- Zorg ervoor dat u hebt geïnstalleerd het [mijn Apps beveiligde aanmelding extensie](my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension) en die u gebruikt een [ondersteunde browser](my-apps-portal-end-user-access.md#supported-browsers).

- Zorg ervoor dat u de juiste URL voor de app en de URL is op uw **Internet-eigenschappen > Beveiliging > vertrouwde websites** lijst.

- Zorg ervoor dat uw wachtwoord juist is en nog niet is verlopen. Zie voor meer informatie, [uw werk- of schoolaccount wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

- Zorg ervoor dat de verificatie-informatie huidige en nauwkeurige. Zie voor meer informatie, [wat Azure multi-factor Authentication betekent voor mij?](multi-factor-authentication-end-user.md) of [wijzigen van de gegevens van uw informatie beveiligingsmethoden en](security-info-add-update-methods-overview.md).

- Wis de cache van uw browser en probeer het opnieuw aanmelden.

Als nadat u hebt geprobeerd deze dingen die u nog steeds geen toegang uw app tot, moet u contact op met de helpdesk van uw organisatie voor hulp.

## <a name="next-steps"></a>Volgende stappen
Nadat u zich aanmeldt bij de **mijn Apps** portal, u kunt ook uw profiel bijwerken en accountgegevens, uw gegevens en de toegang controleert informatie (indien u bent gemachtigd).

- [Toegang tot en gebruik apps op de portal mijn Apps](my-apps-portal-end-user-access.md).

- [Wijzig uw profielgegevens](my-apps-portal-end-user-update-profile.md).

- [Weergeven en bijwerken van uw gegevens met betrekking tot groepen](my-apps-portal-end-user-groups.md).

- [Uitvoeren van uw eigen toegangsbeoordelingen](my-apps-portal-end-user-access-reviews.md).