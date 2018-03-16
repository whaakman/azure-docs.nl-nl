---
title: Aan de slag met Azure AD in Visual Studio WebApi projecten | Microsoft Docs
description: Hoe u aan de slag met Azure Active Directory in WebApi projecten nadat verbinding maken met of maken van een Azure AD met Visual Studio services verbonden
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: a5953a8ae4ef3fc221c984cc7520422251ef2c82
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Aan de slag met Azure Active Directory (WebApi-projecten)

> [!div class="op_single_selector"]
> - [Aan de slag](vs-active-directory-webapi-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-webapi-what-happened.md)

Dit artikel vindt u aanvullende richtlijnen nadat u Active Directory hebt toegevoegd aan een ASP.NET WebAPI-project via de **Project > verbonden Services** opdracht van Visual Studio. Als u de service nog niet aan uw project hebt toegevoegd, kunt u dat doen op elk gewenst moment.

Zie [wat is er gebeurd met mijn WebAPI-project?](vs-active-directory-webapi-what-happened.md) de wijzigingen aan uw project in bij het toevoegen van de gekoppelde service.

## <a name="requiring-authentication-to-access-controllers"></a>Verificatie te vereisen voor toegang tot domeincontrollers

Alle domeincontrollers in uw project zijn adorned met de `[Authorize]` kenmerk. Dit kenmerk moet de gebruiker moet worden geverifieerd voordat toegang tot de API's die door deze domeincontrollers gedefinieerd. Verwijder dit kenmerk van de controller zodat de controller voor anonieme toegang. Als u de machtigingen instelt op een meer gedetailleerd niveau wilt, toepassen van het kenmerk voor elke methode waarvoor de autorisatie in plaats van aan de controllerklasse toepast.

## <a name="next-steps"></a>Volgende stappen

- [Verificatie-scenario's voor Azure Active Directory](active-directory-authentication-scenarios.md)
- [Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app](guidedsetups/active-directory-aspnetwebapp-v1.md)
