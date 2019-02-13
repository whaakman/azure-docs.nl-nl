---
title: Aan de slag met Azure AD in Visual Studio-WebApi-projecten
description: Aan de slag met Azure Active Directory in WebApi-projecten nadat u verbinding maken met of het maken van een Azure AD met behulp van Visual Studio services verbonden
services: active-directory
author: ghogen
manager: douge
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 662d958fc7ef6fe5c9d0e61a1d8e48983d10196f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198776"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Aan de slag met Azure Active Directory (WebApi-projecten)

> [!div class="op_single_selector"]
> - [Aan de slag](vs-active-directory-webapi-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-webapi-what-happened.md)

In dit artikel vindt u aanvullende richtlijnen nadat u Active Directory hebt toegevoegd aan een ASP.NET WebAPI-project via de **Project > Connected Services** opdracht van Visual Studio. Als u hebt de service nog niet is toegevoegd aan uw project, kunt u dit doen op elk gewenst moment.

Zie [wat is er gebeurd met mijn WebAPI-project?](vs-active-directory-webapi-what-happened.md) voor de wijzigingen aan uw project bij het toevoegen van de gekoppelde service.

## <a name="requiring-authentication-to-access-controllers"></a>Verificatie te vereisen voor toegang tot domeincontrollers

Alle domeincontrollers in uw project zijn adorned met de `[Authorize]` kenmerk. Dit kenmerk moet de gebruiker moet worden geverifieerd voordat u toegang tot de API's die door deze domeincontrollers gedefinieerd. Als u wilt toestaan dat de controller voor anonieme toegang, dit kenmerk van de netwerkcontroller te verwijderen. Als u de machtigingen op een meer gedetailleerd niveau instellen wilt, moet u het kenmerk toepassen op elke methode waarvoor autorisatie in plaats van toe te passen op de controllerklasse.

## <a name="next-steps"></a>Volgende stappen

- [Verificatiescenario's voor Azure Active Directory](authentication-scenarios.md)
- [Aanmelden bij Microsoft toevoegen aan een ASP.NET-web-app](quickstart-v1-aspnet-webapp.md)
