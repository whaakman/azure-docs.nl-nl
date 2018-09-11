---
title: Problemen met aanmelden bij een aangepaste toepassing | Microsoft Docs
description: Algemene rrors die kan worden veroorzaakt door u pas weer aan te melden bij een toepassing die u hebt ontwikkeld met Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 8ae8fa823b919ec4a67832e7c42088c994bd2d97
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356489"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Problemen met aanmelden bij een aangepaste toepassing

Er zijn meerdere fouten die kunnen worden veroorzaakt door u pas weer aan te melden bij een app. De belangrijkste reden mensen ondervindt die dit probleem is onjuist geconfigureerd apps.

## <a name="errors-related-to--misconfigured-apps"></a>Fouten met betrekking tot onjuist geconfigureerde apps

* Controleer of zowel de configuraties in de portal overeenkomen met wat er in uw app. Vergelijk specifiek, Client/toepassings-ID, antwoord-URL's, Clientcodes geheimen en App-ID-URI.

* Vergelijk de resource die u bij het aanvragen van toegang tot in de code met de geconfigureerde machtigingen in de **vereiste Resources** tabblad om ervoor te zorgen dat u resources die u hebt geconfigureerd voor het alleen aanvragen.

* Zie [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) voor vergelijkbare fouten of problemen.

## <a name="next-steps"></a>Volgende stappen

[Handleiding voor ontwikkelaars van Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Toestemming en het integreren van Apps aan Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[De toestemming en rollen voor Azure AD v2.0 geconvergeerde Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD-StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)
