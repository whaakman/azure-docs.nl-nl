---
title: Eenmalige aanmelding voor apps met Azure AD-toepassingsproxy | Microsoft Docs
description: Schakel eenmalige aanmelding voor uw gepubliceerde on-premises toepassingen met Azure AD-toepassingsproxy in de Azure-portal.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 13dedf9d07b01befd4d5d6496a3e7427809e6dde
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464660"
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Wachtwoord voor eenmalige aanmelding met Application Proxy vaulting

Azure Active Directory-toepassingsproxy helpt u bij de productiviteit te verbeteren door het publiceren van on-premises toepassingen, zodat externe werknemers veilig toegang deze te tot. In de Azure-portal kunt u ook instellen van eenmalige aanmelding (SSO) tot deze apps. Er hoeft alleen uw gebruikers te verifiëren met Azure AD en ze toegang krijgen tot uw zakelijke toepassing zonder te hoeven aan te melden bij het opnieuw.

Application Proxy biedt ondersteuning voor diverse [eenmalige aanmelding modi](what-is-single-sign-on.md#choosing-a-single-sign-on-method). Wachtwoord gebaseerde aanmelding is bedoeld voor toepassingen die gebruikmaken van een combinatie van gebruikersnaam en wachtwoord voor verificatie. Wanneer u op basis van wachtwoorden aanmelding voor uw toepassing configureren, hebben uw gebruikers zich aanmeldt bij de on-premises toepassing één keer. Hierna Azure Active Directory de gegevens worden opgeslagen en deze automatisch biedt aan de toepassing als uw gebruikers toegang hebben tot het op afstand. 

U moet al hebt gepubliceerd en uw app met Application Proxy getest. Als dit niet het geval is, volg de stappen in [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-add-on-premises-application.md) vervolgens hier terugkeren. 

## <a name="set-up-password-vaulting-for-your-application"></a>Instellen van wachtwoord vaulting voor uw toepassing

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen**.
3. Selecteer de app die u wilt instellen met eenmalige aanmelding in de lijst.  
4. Selecteer **eenmalige aanmelding**.

   ![Schakel eenmalige aanmelding](./media/application-proxy-configure-single-sign-on-password-vaulting/select-sso.png)

5. Kies voor de SSO-modus, **op basis van wachtwoorden Sign-on**.
6. Voer de URL voor de aanmeldings-URL voor de pagina waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij uw app buiten het bedrijfsnetwerk invoeren. Dit kan zijn dat de externe URL die u hebt gemaakt toen u de app via de toepassingsproxy hebt gepubliceerd. 

   ![Kies op basis van wachtwoorden Sign-on en voer de URL van uw](./media/application-proxy-configure-single-sign-on-password-vaulting/password-sso.png)

7. Selecteer **Opslaan**.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Uw app testen

Ga naar de externe URL die u hebt geconfigureerd voor externe toegang tot uw toepassing. Meld u aan met uw referenties voor die app (of de referenties voor een testaccount dat u toegang hebt ingesteld). Zodra u zich is aanmeldt, zou het mogelijk de app verlaten en keert u terug zonder uw referenties opnieuw in te voeren. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over andere manieren om te implementeren [eenmalige aanmelding](what-is-single-sign-on.md)
- Meer informatie over [beveiligingsoverwegingen voor het openen van apps op afstand met Azure AD-toepassingsproxy](application-proxy-security.md)