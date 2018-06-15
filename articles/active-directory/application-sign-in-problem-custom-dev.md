---
title: Problemen met aanmelden bij een toepassing ontwikkelde aangepaste | Microsoft Docs
description: Algemene rrors die kan worden veroorzaakt door u niet kunt aanmelden bij een toepassing die u hebt ontwikkeld met Azure AD
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 57b620f45d1985351064020e122c088584bcdcf5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
ms.locfileid: "26614136"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Problemen met aanmelden bij een toepassing ontwikkelde aangepaste

Er zijn verschillende fouten die kunnen worden veroorzaakt door u niet kunt aanmelden bij een app. De belangrijkste reden mensen optreden die dit probleem is onjuist geconfigureerd apps.

## <a name="errors-related-to--misconfigured-apps"></a>Fouten met betrekking tot onjuist geconfigureerde apps

* Controleer of zowel de configuraties in de portal overeenkomen met wat u hebt in uw app. In het bijzonder vergelijken Client-toepassing-ID, antwoord-URL's, Clientcodes geheimen en App-ID-URI.

* Vergelijk de resource die u bij het aanvragen van toegang tot in de code met de geconfigureerde machtigingen in de **vereiste Resources** tabblad om ervoor te zorgen dat u resources die u hebt geconfigureerd voor het alleen aanvragen.

* Zie [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) voor vergelijkbare fouten of problemen.

## <a name="next-steps"></a>Volgende stappen

[Ontwikkelaarshandleiding voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Toestemming en het integreren van Apps naar Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Toestemming en rollen voor Azure AD v2.0 geconvergeerde Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD-StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)
