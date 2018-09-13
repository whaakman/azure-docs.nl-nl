---
title: Het configureren van een nieuwe toepassing met meerdere tenants | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van eenmalige aanmelding voor een aangepaste toepassing ontwikkeling en registreren met Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 2aa3efcdec57a3e236d2b9739636656b35d16cd6
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724012"
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>Het configureren van een nieuwe toepassing met meerdere tenants

Federatieve eenmalige aanmelding (SSO) inschakelen in uw app wordt automatisch ingeschakeld wanneer de Federatie via Azure AD voor de OpenID Connect, SAML 2.0 of WS-Federation. Als uw gebruikers aan te melden bij ondanks al beschikken over een bestaande sessie met Azure AD, is het waarschijnlijk dat uw app is mogelijk onjuist geconfigureerd.

* Als u van ADAL/MSAL gebruikmaakt, zorg ervoor dat u hebt **PromptBehavior** ingesteld op **automatisch** in plaats van **altijd**.

* Als u een mobiele app bouwt nu, moet u mogelijk aanvullende configuraties brokered of niet-verwerkte eenmalige aanmelding inschakelen.

Zie voor Android, [SSO naar Cross-App in Android inschakelen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

Zie voor iOS-, [SSO naar Cross-App in iOS inschakelen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Volgende stappen

[Azure AD-eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Inschakelen van Cross-SSO-App in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[SSO naar Cross-App in iOS inschakelen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Integratie van Apps aan AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[De toestemming en rollen voor AzureAD v2.0 geconvergeerde Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
