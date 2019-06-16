---
title: Problemen met aanmelden bij een aangepaste toepassing | Microsoft Docs
description: Veelvoorkomende fouten die kunnen worden veroorzaakt door u pas weer aan te melden bij een toepassing die u hebt ontwikkeld met Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8ad2499aea8bf4e41ca00d6c78d76e112f0493e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65825241"
---
# <a name="problems-signing-in-to-a-custom-developed-application"></a>Problemen met aanmelden bij een aangepaste toepassing

Er zijn meerdere fouten die kunnen worden veroorzaakt door u pas weer aan te melden bij een app. De belangrijkste reden mensen ondervindt die dit probleem is onjuist geconfigureerd apps.

## <a name="errors-related-to--misconfigured-apps"></a>Fouten met betrekking tot onjuist geconfigureerde apps

* Controleer of zowel de configuraties in de portal overeenkomen met wat er in uw app. Vergelijk specifiek, Client/toepassings-ID, antwoord-URL's, Clientcodes geheimen en App-ID-URI.

* Vergelijk de resource die u bij het aanvragen van toegang tot in de code met de geconfigureerde machtigingen in de **vereiste Resources** tabblad om ervoor te zorgen dat u resources die u hebt geconfigureerd voor het alleen aanvragen.

* Zie [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory) voor vergelijkbare fouten of problemen.

## <a name="next-steps"></a>Volgende stappen

[Handleiding voor ontwikkelaars van Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Toestemming en het integreren van Apps aan Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[De toestemming en rollen voor Azure AD v2.0 geconvergeerde Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
