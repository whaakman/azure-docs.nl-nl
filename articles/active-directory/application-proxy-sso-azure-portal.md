---
title: Eenmalige aanmelding voor apps met Azure AD-toepassingsproxy | Microsoft Docs
description: "Schakel één aanmelding voor uw gepubliceerde on-premises toepassingen met Azure AD-toepassingsproxy in de Azure portal."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9ddc0c1bd5f2cbb24f6761cfd041b820ee6464b8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Wachtwoord voor eenmalige aanmelding met toepassingsproxy vaulting

Azure Active Directory-toepassingsproxy helpt u bij de productiviteit door on-premises toepassingen publiceren zodat externe werknemers veilig toegang deze te tot verbeteren. In de Azure-portal kunt u ook instellen van eenmalige aanmelding (SSO) op deze apps. Uw gebruikers alleen vereist voor verificatie met Azure AD en toegang te krijgen tot uw zakelijke toepassing zonder opnieuw aanmelden.

Toepassingsproxy ondersteunt verschillende [eenmalige aanmelding modi](application-proxy-sso-overview.md). Op basis van wachtwoorden eenmalige aanmelding is bedoeld voor toepassingen die gebruikmaken van een combinatie van gebruikersnaam en wachtwoord voor verificatie. Wanneer u op basis van wachtwoorden eenmalige aanmelding voor uw toepassing configureert, wordt uw gebruikers aan te melden bij de on-premises toepassing eenmaal hebben. Hierna is Azure Active Directory de gegevens worden opgeslagen en automatisch aan de toepassing te biedt wanneer uw gebruikers toegang op afstand. 

U moet al hebt gepubliceerd en uw app met toepassingsproxy getest. Als dit niet het geval is, volg de stappen in [toepassingen publiceren met Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md) vervolgens kun je hier. 

## <a name="set-up-password-vaulting-for-your-application"></a>Wachtwoord voor uw toepassing vaulting instellen

1. Meld u als beheerder aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen**.
3. Selecteer de app die u wilt instellen met eenmalige aanmelding in de lijst.  
4. Selecteer **eenmalige aanmelding**.

   ![Schakel eenmalige aanmelding](./media/application-proxy-sso-azure-portal/select-sso.png)

5. Kies voor de modus SSO **op basis van wachtwoorden aanmelding**.
6. Voer de URL voor de URL eenmalige aanmelding voor de pagina waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij uw app buiten het bedrijfsnetwerk invoeren. Dit kan zijn dat de externe URL die u hebt gemaakt toen u de app via toepassingsproxy hebt gepubliceerd. 

   ![Kiezen op basis van wachtwoorden eenmalige aanmelding en voer de URL](./media/application-proxy-sso-azure-portal/password-sso.png)

7. Selecteer **Opslaan**.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Uw app testen

Ga naar de externe URL die u hebt geconfigureerd voor externe toegang tot uw toepassing. Aanmelden met uw referenties voor die app (of de referenties voor een testaccount dat u toegang hebt ingesteld). Zodra u zich is aanmeldt, kunt u moet de app laat en keert u terug zonder uw referenties opnieuw invoeren. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over andere manieren om te implementeren [eenmalige aanmelding met toepassingsproxy](application-proxy-sso-overview.md)
- Meer informatie over [beveiligingsoverwegingen voor toegang tot apps op afstand met Azure AD-toepassingsproxy](application-proxy-security-considerations.md)